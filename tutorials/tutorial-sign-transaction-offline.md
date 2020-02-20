# 使用离线私钥(Working with an offline private key)

## 使用离线私钥(Working with an offline private key)

本教程说明了如何通过 Hyperledger Fabric Node.js SDK(fabric-client 和 fabric-ca-client)API 使用脱机私钥。

有关更多信息：

- Hyperledger Fabric 入门，请参阅[构建你的第一个网络(Building your first network)](http://hyperledger-fabric.readthedocs.io/en/latest/build_network.html)。
- 在标准资产交换过程中发生的交易机制。 [Fabric 中的交易流 (transacton flow in fabric)](https://hyperledger-fabric.readthedocs.io/en/latest/txflow.html)。
- PKI 系统中的证书签名[CSR](https://en.wikipedia.org/wiki/Certificate_signing_request)请求。(The Certificate Signing Request (CSR) in a PKI system. [CSR](https://en.wikipedia.org/wiki/Certificate_signing_request))

以下内容假定您对 Hyperledger Fabric 网络(Orderer 和 Peer)以及 Node 应用程序开发有所了解，包括对 Javascript promise 和 async await 的使用。

## 概述

在大多数使用情况下，Fabric 客户端将保留用户的凭据(包括私钥)并为用户签名交易。但是，某些业务场景可能需要更高级别的隐私。如果用户想要保留其私钥的秘密，并且不信任另一个系统或后端服务器来安全地存储和使用它，该怎么办？

Fabric 客户端具有使用脱机私钥签署交易的功能。与用用户身份(包含用户的私钥)调用 setUserContext()相比，另一种方法是将 tx 进程的 sign 签名从 Fabric 客户端中分离出来，然后让应用程序层选择存储私钥的位置，对交易进行签名并将已签名的交易发送回去。通过这种方法，结构客户端不再需要用户的私钥。

Fabric-ca 具备注册 PKCS#10 标准 CSR 的能力，这意味着用户可以使用现有的密钥对生成 CSR，并将此 CSR 发送到 Fabric-ca 以获取签名证书。 fabric-ca-client 还通过 API enroll()接受 CSR。

## 用于离线签署交易的交易流程

下面将显示脱机签名交易的步骤：

在 Fabric 客户端上设置用户身份(证书和私钥)：

1. 背书(Endorse )-> Channel.sendTransactionProposal()
2. 提交(Commit )-> Channel.sendTransaction()
3. ChannelEventHub-> ChannelEventHub.connect()(如果 channel-eventhub 尚未连接到 Peer)

在 Fabric 客户端上没有用户的私钥：

1. 背书：

   1. 使用身份的证书生成未签名的交易提议-> Channel.generateUnsignedProposal()
   2. 使用身份的私钥离线签署未签名的交易建议，以生成签名的交易建议
   3. 将签署的交易提议书发送给 Peer 并获得背书-> Channel.sendSignedProposal()

2. 提交：

   1. 生成带有签注的未签名交易-> Channel.generateUnsignedTransaction()
   2. 使用身份的私钥离线签署未签名的交易，从而生成签名的交易
   3. 将已签名的交易发送到 orderer-> Channel.sendSignedTransaction()

3. 注册通道事件侦听器：如果通道事件集线器尚未连接到 Peer，则通道事件集线器注册也需要私钥的签名。
   1. 为 ChannelEventHub-> ChannelEventHub.generateUnsignedRegistration()生成一个未签名的 eventhub 注册
4. 使用身份的私钥对未签名的 eventhub 注册进行签名，以离线生成已签名的 eventhub 注册
   3. 使用签名的 eventhub 注册进行 ChannelEventHub 的注册-> ChannelEventHub.connect(){signedEvent})

## 如何通过身份的私钥签署交易

可能有几种数字签名算法。如果我们在结构客户端设置用户身份，则默认情况下，Fabric 客户端将使用 ECDSA 和算法"EC"。 这是使用离线私钥的方式。

1. 首先，使用身份证明生成未签名的交易建议

```javascript
const certPem = "<PEM encoded certificate content>";
const mspId = "Org1MSP"; // the msp Id for this org // 该组织的msp ID

const transactionProposal = {
  fcn: "move",
  args: ["a", "b", "100"],
  chaincodeId: "mychaincodeId",
  channelId: "mychannel"
};
const { proposal, txId } = channel.generateUnsignedProposal(
  transactionProposal,
  mspId,
  certPem
);
// now we have the 'unsigned proposal' for this transaction
// 现在我们已经为此交易提供了 '未签名的提案'
```

2. 计算交易提议字节的哈希值。

   应该选择哈希算法并计算交易建议字节的哈希。

   存在多个哈希函数(例如 SHA2/3)。 默认情况下，Fabric 客户端将使用密钥大小为 256 的"SHA2"。

   用户可以使用替代实现

```javascript
// the proposal comes from step 1
// 提案来自步骤1
const proposalBytes = proposal.toBuffer();

// A hash function by the user's desire
// 用户期望的哈希函数
const hashFunction = xxxx;

// calculate the hash of the proposal bytes
// 计算提议字节的哈希值
const digest = hashFunction(proposalBytes);
```

3. 计算此交易提议的签名

   对于签名算法，我们可能有一系列选择。 包括非对称密钥(例如 ECDSA 或 RSA)，对称密钥(例如 AES)。

   默认情况下，Fabric 客户端将使用算法为"EC"的 ECDSA。

```javascript
// This is a sample code for signing the digest from step 2 with EC.
// 这是一个示例代码，用于使用EC从第2步签名摘要。
// Different signature algorithm may have different interfaces
// 不同的签名算法可能具有不同的接口
const elliptic = require("elliptic");
const { KEYUTIL } = require("jsrsasign");

const privateKeyPEM = "<The PEM encoded private key>";
const { prvKeyHex } = KEYUTIL.getKey(privateKeyPEM); // convert the pem encoded key to hex encoded private key // 将Pem编码的密钥转换为十六进制编码的私有密钥

const EC = elliptic.ec;
const ecdsaCurve = elliptic.curves["p256"];

const ecdsa = new EC(ecdsaCurve);
const signKey = ecdsa.keyFromPrivate(prvKeyHex, "hex");
const sig = ecdsa.sign(Buffer.from(digest, "hex"), signKey);

// now we have the signature, next we should send the signed transaction proposal to the peer
// 现在我们已经有了签名，接下来我们应该向Peer发送已签名的交易建议
const signature = Buffer.from(sig.toDER());
const signedProposal = {
  signature,
  proposal_bytes: proposalBytes
};
```

4. 将已签署的交易提议发送给 Peer

```javascript
const sendSignedProposalReq = { signedProposal, targets };
const proposalResponses = await channel.sendSignedProposal(
  sendSignedProposalReq
);
// check the proposal responses, if all good, commit the transaction
// 检查提案响应，如果一切都很好，请进行交易
```

5. 与步骤 1 相似，生成未签名的交易

```javascript
const commitReq = {
  proposalResponses,
  proposal
};

const commitProposal = await channel.generateUnsignedTransaction(commitReq);
```

6. 与步骤 3 相似，使用用户的私钥签署未签名的交易

```javascript
const signedCommitProposal = signProposal(commitProposal);
```

7. 提交已签署的交易

```javascript
const response = await channel.sendSignedTransaction({
  signedProposal: signedCommitProposal,
  request: commitReq
});
// response.status should be 'SUCCESS' if the commit succeed
//如果成功提交，则response.status应该为'SUCCESS'
```

8. 与步骤 1 相似，为 ChannelEventHub 生成未签名的 eventhub 注册。

```javascript
const unsignedEvent = eh.generateUnsignedRegistration({
  certificate: certPem,
  mspId
});
```

9. 与第 3 步类似，使用用户的私钥对未签名的 eventhub 注册进行签名

```javascript
const signedProposal = signProposal(unsignedEvent);
const signedEvent = {
  signature: signedProposal.signature,
  payload: signedProposal.proposal_bytes
};
```

10. 在 Peer 注册此 ChannelEventHub

```javascript
channelEventHub.connect({ signedEvent });
```

完整测试可以在 fabric-sdk-node/test/integration/signTransactionOffline.js 中找到

## 如何注册 CSR

fabric-ca-client 提供了 API enroll()，它接受可选参数"CSR"。 如果参数不包含 CSR，fabric-ca-client 将首先生成一个密钥对，然后使用用户的 enrollmentID 作为公用名来创建一个用新生成的私钥签名的 CSR。 如果注册参数中没有"CSR"，则响应将包含私钥对象。

要注册 CSR，首先我们应该调用 fabric-ca-client API register 在 Fabric-ca 上注册新的身份。 成功注册后，我们将获得 enrollmentID 和 enrollmentSecret。

然后，我们应该创建 CSR。 一种常见的方法是使用 openssl 命令。

```NONE
注意，在注册步骤中，CSR必须包含信息“common name(通用名称)”，并且“common name”必须与“enrollmentID”相同。
```

这是如何使用密钥算法 rsa 和密钥大小 2048 位创建 CSR 的示例

```shell
openssl req -nodes -newkey rsa:2048 -keyout test.key -out test.csr
```

上面命令中的 test.csr 表示为 Base64 编码的 PKCS#10。

这就是我们所谓的 CSR 注册

```javascript
const fs = require("fs");
const csr = fs.readFileSync("the path to test.csr", "utf8");
const req = {
  enrollmentID: enrollmentID,
  enrollmentSecret: enrollmentSecret,
  csr: csr
};

const enrollment = await caService.enroll(req);

// the enrollment.certificate contains the signed certificate from Fabric-ca
// 注册证书包含来自Fabric-ca的签名证书
```

---
