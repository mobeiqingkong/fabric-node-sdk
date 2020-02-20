# fabric-client：如何使用公共连接配置文件(How to use a common connection profile)

## fabric-client：如何使用公共连接配置文件(How to use a common connection profile)

本教程说明了通用连接配置文件的用法。 从 1.1 开始，连接配置文件是 Hyperledger Fabric Node.js 客户端的一项新功能。 连接配置文件将向 Hyperledger Fabric Node.js 客户端(Fabric 客户端)描述 Hyperledger Fabric 网络。

有关更多信息：

- Hyperledger Fabric 入门，请参阅[构建第一个网络(Building your first network)](http://hyperledger-fabric.readthedocs.io/en/latest/build_network.html)。
- Hyperledger Fabric 中通道的配置以及创建和更新的内部过程，请参见[Hyperledger Fabric 通道(Hyperledger Fabric channel configuration)](http://hyperledger-fabric.readthedocs.io/en/latest/configtx.html)配置
- 密码生成请参阅 [cryptogen](http://hyperledger-fabric.readthedocs.io/en/latest/build_network.html#crypto-generator)
- 配置事务生成器，请参见[configtxgen](http://hyperledger-fabric.readthedocs.io/en/latest/build_network.html#configuration-transaction-generator)
- 配置转换工具，请参阅[configtxlator](https://github.com/hyperledger/fabric/tree/master/examples/configtxupdate)

以下内容假定您对 Hyperledger Fabric 网络(orderer 和 peer)以及 Node 应用程序开发(包括 Java Promise 的使用)有所了解。

### 概述

连接配置文件包含描述 Hyperledger Fabric 网络的条目，包括描述将访问网络的 Fabric 客户端的条目。 该应用程序将加载配置文件，然后 Fabric 客户端将使用它来简化设置和使用网络所需的步骤。 连接配置文件具有网络项目的特定地址和设置。 实例化的 javascript 类之类的资源存储在 Fabric 客户端的配置系统中。 使用装载有连接配置文件配置的 Fabric 客户端将更加容易，因为它可以减少调用操作之前的设置。 诸如目标之类的项目的参数可以通过名称指定，并且在调用操作之前不必创建和维护对象。 在许多呼叫中，如果未指定目标 peer，则 Fabric 客户端将查看该操作所需的角色中是否存在 peer。

#### API 加载连接配置文件

- Client.loadFromConfig() - 一种静态实用程序方法，用于获取通过连接配置文件配置加载的 Fabric 客户端实例。
- client.loadFromConfig() - 一种 Fabric 客户端实例方法，用于加载连接配置文件配置，覆盖通过上述调用创建此客户端对象时可能已经设置的所有现有连接配置文件配置设置。

#### 使用已加载的连接配置文件的新 API

- client.initCredentialStores() - 一种 Fabric 客户端实例方法，用于基于加载的连接配置文件配置中的当前设置创建状态存储并将其分配给 Fabric 客户端实例。它还将创建加密套件并将其分配给 Fabric 客户端实例。如果需要，将创建一个加密存储并将其分配给加密套件。(基于 HSM 的加密套件不需要加密存储)。
- client.setTlsClientCertAndKey(clientCert, clientKey) - 一种 Fabric 客户端实例方法，该方法将在客户端实例上设置证书和相应的私钥。相互的 TLS 客户端设置未存储在连接配置文件中。当从连接配置文件中定义的端点为用户创建 peer 实例或 orderer 实例时，这些设置将用作客户端相互 TLS 设置。使用双向 TLS 和连接配置文件时，必须在要求端点之前调用此方法。仅在使用双向 TLS 和连接配置文件时才需要调用此方法。
- channel.newChannelEventHub() - 一种 Fabric 通道实例方法，用于基于命名 Peer 的已加载连接配置文件配置中的当前设置来创建基于通道的事件中心。
- channel.getChannelEventHubsForOrg() - 一种 Fabric 通道实例方法，用于返回与组织相关联的基于通道的事件中心的列表。将 eventEvent 设置为 true 的组织中的 Peer 返回。
- client.getPeersForOrg() - 一种 Fabric 客户端实例方法，用于返回与组织关联的 Peer 对象的列表。

#### 修改后的 API(如果已加载)将使用连接配置文件配置

- client.getChannel() - Fabric 客户端实例方法，它将基于当前加载的连接配置文件配置中定义的通道设置创建通道实例对象。
- client.newTransactionID(&lt;boolean&gt;)-对该方法进行了修改，以允许使用布尔值来指示生成的事务 ID 应该基于管理身份(如果已分配)，而不是基于分配给 Fabric 客户端的用户上下文。
- client.setUserContext() - 现在允许用户名和密码作为参数或用户对象。使用用户名和密码时，Fabric 客户端将使用用户名和密码向证书颁发机构执行注册。
- client.installChaincode() - 如果从请求参数列表中排除了 targets 参数，则将使用在客户端的当前组织中定义的 peer 项。
- client.queryXXXX() - 查询 API 现在将 Peer 名称(在连接配置文件配置中定义)或 peer 对象实例作为目标。
- channel.instantiateChaincode() - 如果将目标参数从请求参数列表中排除，则将使用也在该通道上也在客户端的当前组织中定义的 Peer。
- channel.sendTransactionProposal()请求对象参数可以使用名称作为目标，或者让结构客户端根据连接配置文件配置中的定义，使用 peer 作为目标。
- channel.sendTransaction()请求对象参数可以使用 orderer 名称，也可以让结构客户端根据连接配置文件配置中的定义，找到要使用的 orderer。
- channel.queryXXXX() - 所有查询 API 现在都将 peer 名称作为目标，而不是 peer 实例对象。

### 加载连接配置文件配置

应用程序代码可以指向包含配置信息的 yaml 或 json 文件，也可以将 Javascript 对象目录传递给 API 以加载配置。 为了方便起见，在 Fabric-client 上有一个静态实用程序方法来创建新的 fabric 客户端对象并同时加载连接配置文件配置。 Fabric 客户端实例上还有一种方法可用于在现有连接配置文件配置之上加载连接配置文件配置。

以下示例将创建 Fabric 客户端的新实例并加载连接配置文件配置。 但是，在这种情况下，连接配置文件配置不包含有关 Fabric 网络客户端的任何信息，仅包含 Fabric 网络元素。

```javascript
var client = Client.loadFromConfig("test/fixtures/network.yaml");
```

这是加载的连接配置文件定义

```yaml
name: "Network"
version: "1.0"

channels:
  mychannel:
    orderers:
      - orderer.example.com
    peers:
      peer0.org1.example.com:
        endorsingPeer: true
        chaincodeQuery: true
        ledgerQuery: true
        eventSource: true
      peer0.org2.example.com:
        endorsingPeer: true
        chaincodeQuery: false
        ledgerQuery: true
        eventSource: false

organizations:
  Org1:
    mspid: Org1MSP
    peers:
      - peer0.org1.example.com
    certificateAuthorities:
      - ca-org1
    adminPrivateKey:
      path: test/fixtures/channel/crypto-config/peerOrganizations/org1.example.com/users/Admin@org1.example.com/keystore/9022d671ceedbb24af3ea69b5a8136cc64203df6b9920e26f48123fcfcb1d2e9_sk
    signedCert:
      path: test/fixtures/channel/crypto-config/peerOrganizations/org1.example.com/users/Admin@org1.example.com/signcerts/Admin@org1.example.com-cert.pem

  Org2:
    mspid: Org2MSP
    peers:
      - peer0.org2.example.com
    certificateAuthorities:
      - ca-org2
    adminPrivateKey:
      path: test/fixtures/channel/crypto-config/peerOrganizations/org2.example.com/users/Admin@org2.example.com/keystore/5a983ddcbefe52a7f9b8ee5b85a590c3e3a43c4ccd70c7795bec504e7f74848d_sk
    signedCert:
      path: test/fixtures/channel/crypto-config/peerOrganizations/org2.example.com/users/Admin@org2.example.com/signcerts/Admin@org2.example.com-cert.pem

orderers:
  orderer.example.com:
    url: grpcs://localhost:7050
    grpcOptions:
      ssl-target-name-override: orderer.example.com
      grpc-max-send-message-length: 4194304
    tlsCACerts:
      path: test/fixtures/channel/crypto-config/ordererOrganizations/example.com/orderers/orderer.example.com/tlscacerts/example.com-cert.pem

peers:
  peer0.org1.example.com:
    url: grpcs://localhost:7051
    grpcOptions:
      ssl-target-name-override: peer0.org1.example.com
      grpc.keepalive_time_ms: 600000
    tlsCACerts:
      path: test/fixtures/channel/crypto-config/peerOrganizations/org1.example.com/peers/peer0.org1.example.com/tlscacerts/org1.example.com-cert.pem

  peer0.org2.example.com:
    url: grpcs://localhost:8051
    grpcOptions:
      ssl-target-name-override: peer0.org2.example.com
    tlsCACerts:
      path: test/fixtures/channel/crypto-config/peerOrganizations/org2.example.com/peers/peer0.org2.example.com/tlscacerts/org2.example.com-cert.pem

certificateAuthorities:
  ca-org1:
    url: https://localhost:7054
    httpOptions:
      verify: false
    tlsCACerts:
      path: test/fixtures/channel/crypto-config/peerOrganizations/org1.example.com/ca/org1.example.com-cert.pem
    registrar:
      - enrollId: admin
        enrollSecret: adminpw
    caName: caorg1

  ca-org2:
    url: https://localhost:8054
    httpOptions:
      verify: false
    tlsCACerts:
      path: test/fixtures/channel/crypto-config/peerOrganizations/org2.example.com/ca/org2.example.com-cert.pem
    registrar:
      - enrollId: admin
        enrollSecret: adminpw
    caName: caorg2
```

以下示例将使现有的 Fabric 客户端加载连接配置文件配置。该定义将仅包含客户端定义，而不包含 Fabric 网络定义。客户端可以随时加载新的配置文件，它将覆盖它包含的先前加载的顶级部分。在这种情况下，正在加载的文件仅包含一个客户部分，因此，已加载的定义现在将具有以前加载的通道，组织，peer，orderer 和 certificateAuthorities 部分定义以及新加载的客户部分定义。这使现有的 Fabric 客户端能够在不同的组织内工作。注意，此客户端定义包含一个连接部分，该连接部分的 client 部分具有 options 属性。此处定义的设置将应用于客户端创建的新 peer 实例和 orderer 实例。这将包括使用发现服务时将自动创建的 peer 和 orderer。peer 和 orderer 可以在其 grpcOptions 设置中覆盖这些连接设置。

注意：Fabric 客户端 grpc-max-send-message-length 和 grpc-max-receive-message-length 的默认值为-1（无限制）。

```javascript
client.loadFromConfig("test/fixtures/org1.yaml");
```

这是上面加载的客户端定义

```yaml
name: "Org1 Client"
version: "1.0"

client:
  organization: Org1
  credentialStore:
    path: "/tmp/hfc-kvs/org1"
    cryptoStore:
      path: "/tmp/hfc-cvs/org1"
  connection:
    options
      grpc.keepalive_time_ms: 120000
```

### 设置商店

下一步是使用状态和加密存储来设置客户端对象。 如果连接配置文件配置的客户机部分已定义了这些内容，则只需运行以下内容即可。 这个 API 是基于 Promise 的，请注意，在返回的 Promise 上我们需要一个.then 使其真正执行。 该 API 不会返回任何内容，但是它创建了状态存储并将其分配给客户端，它创建了 CryptoSuite 并将其分配给客户端，并创建了加密存储并将其分配给加密套件。 请注意，在连接配置文件配置的 "client" 部分中，上面的 credentialStore 和 cryptoStore 定义。 在这种情况下，我们使用的是两个不同的位置，初次启动时将它们放在相同的位置可能会更容易。

下面将创建两个存储和一个 CryptoSuite，然后根据加载的配置将它们分配给 Fabric 客户端。

```javascript
client.initCredentialStores()
.then((nothing) => {
```

### 使用用户上下文

当组织上存在证书颁发机构信息时，可以使用 Fabric 客户端简化注册和用户上下文创建。该应用程序仍将必须向证书颁发机构注册新用户，但是，在加载了连接配置文件配置后，可以使用一种更简单的方法来获取证书颁发 Fabric 客户端。

因此，首先让我们注册一个管理员用户，以便我们拥有与证书颁发机构和结构网络进行交互所需的凭据(加密材料)。以下便捷方法将首先查看状态存储区(如上定义)，以查看用户是否存在。如果找不到用户，并且已加载连接配置文件配置，则 Fabric 客户端将使用在当前加载的连接配置文件配置中定义的地址来构建 Fabric 客户端配置中定义的证书颁发 Fabric 客户端对象。Fabric 客户端使用证书颁发机构客户端来注册具有证书颁发机构的 admin 用户，这要求在客户端上生成一组新的密钥。然后，Fabric 客户端将使用证书颁发机构从注册中返回的签名证书来创建用户上下文。然后将上下文分配给 Fabric 客户端，并将其存储在状态存储中以及将密钥存储在加密存储中。此时，结构客户端已准备好与 Fabric 网络进行交互，并且应用程序可以使用返回的用户对象与证书颁发机构进行交互。

在以下示例中，我们可以注册用户，因为证书颁发机构知道该用户。新用户必须先注册。

```javascript
client.setUserContext({username:'admin', password:'adminpw'})
.then((admin) => {
```

以下示例将通过首先查找客户端部分中定义的组织，然后查找与该组织关联的证书颁发机构，使结构客户端基于当前加载的连接配置文件配置来构建证书颁发机构客户端。

```javascript
var fabric_ca_client = client.getCertificateAuthority();
```

然后，一旦我们有了 fabric-ca-client，我们就可以注册新用户。 我们还可以使用 fabric-ca-client 来注册用户，并向 Fabric 客户端进行一些调用以创建用户对象，然后将该用户对象分配给 Fabric 客户端，但是使用便捷方法会容易得多。 Fabric 客户端实例的实例。 注意，我们必须如何使用从 client.setUserContext()返回的'admin'用户对象进行注册。 admin 用户对象具有注册所需的凭据。 然后注意，我们调用了与上述管理员相同的 setUserContext 方法，这将为 Fabric 客户端对象分配“ user1”用户上下文，从而提供与 Fabric 网络交互的凭据。 请注意，setUserContext 还存储了用户上下文，其中包含来自证书颁发机构的签名证书以及现在注册用户的新创建的公钥和私钥。

```javascript
fabric_ca_client.register({enrollmentID: 'user1', affiliation: 'org1'}, admin)
.then((secret) => {
    return client.setUserContext({username:'user1', password:secret});
}).then((user)=> {
```

### 使用双向 TLS

当您的网络使用双向 TLS 时，在自动构建端点之前，客户端实例必须使用客户端证书和私钥。 客户实例将能够将所需的资料传递给建立连接所需的端点实例。 所示示例还将检索材料。 必须在 Fabric 网络上执行任何操作之前执行这些步骤。

```javascript
// get the CA associated with this client's organization
let fabric_ca_client = client.getCertificateAuthority();

let request = {
    enrollmentID: 'user1',
    enrollmentSecret: secret,
    profile: 'tls'
};

// make the request to build the keys and get the certificate
fabric_ca_client.enroll(request)
.then((enrollment) => {
   // Successfully called the Certificate Authority to get the TLS material
   let key = enrollment.key.toBytes();
   let cert = enrollment.certificate;

   // set the material on the client to be used when building endpoints for the user
   client.setTlsClientCertAndKey(cert, key);
   ...
```

### 需要管理员时

请注意，在连接配置文件配置的"organizations "部分中，一个组织可能具有与该组织相关联的签名证书设置和管理员私钥设置。这为您的组织带来了便利，以便需要 Fabric 网络管理员的操作将很容易获得。加载配置后，这些凭据将分配给 Fabric 客户端。如果尚未分配，则假定分配给 Fabric 客户端的当前用户上下文是管理员。客户端对象上还有一种便捷的方法，该方法会将凭证分配给客户端，以用于需要管理员的操作。

```javascript
client.setAdminSigningIdentity("admin privateKey", "admin cert");
```

假设已经加载了一个公共连接配置文件，则使用管理员设置了一个组织，并指示客户端位于该组织中。然后，当调用获取事务 ID 对象时，Fabric 客户端将检查是否已将管理员分配给 Fabric 客户端，并使用该管理员生成事务 ID。返回的交易 ID 将被标记为它是使用分配的管理标识生成的。请注意，构建的请求对象是如何仅使用 orderer 的名称而不是 orderer 对象的。Fabric 客户端将在已加载的连接配置文件配置中查找此名称。进行 createChannel 调用时，Fabric 客户端将知道此操作应由管理身份签名，因为事务 ID 被标记为基于 admin 的事务。请注意，如果登录用户是管理用户并已分配给 Fabric 客户端，则不需要管理签名身份。

假设已经加载了一个公共连接配置文件，则使用管理员设置了一个组织，并指示客户端位于该组织中。然后，当调用获取事务 ID 对象时，Fabric 客户端将检查是否已将管理员分配给 Fabric 客户端，并使用该管理员生成事务 ID。返回的交易 ID 将被标记为它是使用分配的管理标识生成的。请注意，构建的请求对象是如何仅使用 orderer 的名称而不是 orderer 对象的。Fabric 客户端将在已加载的连接配置文件配置中查找此名称。进行 createChannel 调用时，Fabric 客户端将知道此操作应由管理身份签名，因为事务 ID 被标记为基于 admin 的事务。请注意，如果登录用户是管理用户并已分配给 Fabric 客户端，则不需要管理签名身份。

```javascript
let tx_id = client.newTransactionID(true);

let request = {
    config: config,
    signatures : signatures,
    name : channel_name,
    orderer : 'orderer.example.com',
    txId  : tx_id
};

return client.createChannel(request);
}).then((result) => {
```

### 需要 Peer 时

注意，如何将 peer 添加到组织中，它不仅仅是对实际 peer 定义的引用，还定义了 peer 以在该组织内具有角色。

```yaml
peer0.org2.example.com:
  endorsingPeer: true
  chaincodeQuery: false
  ledgerQuery: true
  eventSource: false
```

该 Peer 可用于背书交易，但不用于运行链码查询。 可通过进行基于分类帐的查询(例如 queryBlock)来使用此 Peer 来审核通道，但不能将其用作事件源。 当然，这种角色组合在现实生活中没有多大意义。

因此，让我们看一下 chaincode 调用背书

```javascript
let tx_id = client.newTransactionID();

var request = {
    chaincodeId : 'example',
    fcn: 'move',
    args: ['a', 'b','100'],
    txId: tx_id
};

channel.sendTransactionProposal(request)
.then((results) => {
```

注意，我们省略了请求对象的 targets 参数。 这将使 Fabric 客户端在连接配置文件配置中的此通道上查找 Peer。 Fabric 客户端将寻找以 endorsingPeer 角色定义的 Peer。 然后，Fabric 客户端将提案发送给所定位的 Peer，并在结果对象中返回所有背书。

可能只需要特定组织中的 Peer。 使用组织的 mspid。

```javascript
var peers = getPeersForOrg("Org1MSP");
```

或者可能是针对连接配置文件的 client 部分中定义的组织。

```javascript
var peers = getPeersForOrg();
```

### 需要 orderer 时

在收到来自 Peer 的关于交易提议的背书后，需要将它们与提议一起发送给 orderer，以将交易提交给分类帐。

```javascript
var request = {
    proposalResponses: proposalResponses,
    proposal: proposal
};

channel.sendTransaction(request)
.then((results) => {
```

请注意，将事务发送到的 orderer 未包括在请求对象中。将使用在连接配置文件配置中定义的 orderer。

### 进行查询时

当加载了连接配置文件配置且查询调用未传递给目标 Peer 使用时，Fabric 客户端将在连接配置文件配置中查找要使用的 Peer。

- 这些是基于 Fabric 客户端的查询，并且要求用户具有管理员角色或指示应使用管理员身份。这些查询不使用连接配置文件配置查找来查找要使用的 Peer，而必须将其传递给目标 Peer。
  - queryChannels
  - queryInstalledChaincodes
- 这些查询是基于通道的查询，需要具有 ledgerQuery 角色的 peer。

  - queryInstantiatedChaincodes (用户必须是管理员或表明应使用分配的管理员身份)
  - queryInfo
  - queryBlockByHash
  - queryBlock
  - queryTransaction

- 这是基于通道的查询，需要具有 chaincodeQuery 角色的 peer。
  - queryByChaincode

### 监视事件时

加载连接配置文件配置后，使用基于通道的事件中心的工作将不会更改。 已将新方法添加到 Fabric 客户端，以简化 ChannelEventHub 对象的设置。 使用以下代码获取一个 ChannelEventHub 对象，该对象将被设置为与指定 Peer 的基于通道的事件中心一起使用。

```javascript
var channel_event_hub = channel.newChannelEventHub("peer0.org1.example.com");
```

请注意，调用的参数如何是 Peer 的名称。用于创建基于通道的事件中心的所有设置均由连接配置文件配置在同级下的同名名称定义。

```yaml
peer0.org1.example.com:
  url: grpcs://localhost:7051
  grpcOptions:
    ssl-target-name-override: peer0.org1.example.com
    grpc.keepalive_time_ms: 600000
  tlsCACerts:
    path: test/fixtures/channel/crypto-config/peerOrganizations/org1.example.com/peers/peer0.org1.example.com/tlscacerts/org1.example.com-cert.pem
```

以下将是"Org1"组织内的事件中心的列表。组织将"eventSource"设置为 true 的所有 Peer。使用组织的 mspid。

```javascript
var channel_event_hubs = channel.getChannelEventHubsForOrg("Org1MSP");
```

以下是连接配置文件的 client 部分中定义的组织内的基于通道的事件中心的列表。

```javascript
var channel_event_hubs = channel.getChannelEventHubsForOrg();
```

## Fabric 客户端在通用连接配置文件中查找什么

Fabric 客户端将寻找以下密钥名称和这些密钥的参数：

```yaml
#
# Schema version of the content. Used by the SDK to apply the parsing rules.
#
version: "1.0" # only supported version as of fabric-client v1.3.0

#
# The client section is SDK-specific. These are the settings that the
# NodeSDK will use to automatically set up a Client instance.
#
client:
  # Which organization does this application instance belong to? The value must be the name of an org
  # defined under "organizations" ... see below
  organization: Org1

  # Some SDKs support pluggable KV stores, the properties under "credentialStore"
  # are implementation specific
  credentialStore:
    # Specific to FileKeyValueStore.js or similar implementations in other SDKs. Can be others
    # if using an alternative impl. For instance, CouchDBKeyValueStore.js would require an object
    # here for properties like url, db name, etc.
    path: "/tmp/hfc-kvs"
      or
    <implementation specific properties>

    # Specific to the CryptoSuite implementation. Software-based implementations like
    # CryptoSuite_ECDSA_AES.js in node SDK requires a key store. PKCS#11 based implementations does
    # not.
    cryptoStore:
      # Specific to the underlying KeyValueStore that backs the crypto key store.
      path: "/tmp/hfc-cvs"
       or
    <implementation specific properties>

  # Sets the connection timeouts for new peer and orderer objects when the client creates
  # peer and orderer objects during the client.getPeer() and client.getOrderer() calls
  # or when the peer and orderer objects are created automatically when a channel
  # is created by the client.getChannel() call.
  connection:
    timeout:
       peer:
         # the timeout in seconds to be used on requests to a peer,
         # for example 'sendTransactionProposal'
         endorser: 120
         # the timeout in seconds to be used by applications when waiting for an
         # event to occur. This time should be used in a javascript timer object
         # that will cancel the event registration with the channel event hub instance.
         eventHub: 60
         # the timeout in seconds to be used when setting up the connection
         # with a peer event hub. If the peer does not acknowledge the
         # connection within the time, the application will be notified over the
         # error callback if provided.
         eventReg: 3
       # the timeout in seconds to be used on request to the orderer,
       # for example
       orderer: 30

#
# How a channel is defined and the peers and orderers on that channel. When the
# client.getChannel() call is used the client will pre-populate the channel with
# orderers and peers as defined in this section.
#
channels:
  # name of the channel
  mychannel2:
    # List of orderers designated by the application to use for transactions on this channel.
    # The values must be orderer names defined under "orderers" section
    orderers:
      - orderer.example.com

    # List of peers from participating organizations
    peers:
      # The values must be peer names defined under "peers" section
      peer0.org1.example.com:
        # Will this peer be sent transaction proposals for endorsement? The peer must
        # have the chaincode installed. The app can also use this property to decide which peers
        # to send the chaincode install request. Default: true
        endorsingPeer: true

        # Will this peer be sent query proposals? The peer must have the chaincode
        # installed. The app can also use this property to decide which peers to send the
        # chaincode install request. Default: true
        chaincodeQuery: true

        # Will this peer be sent query proposals that do not require chaincodes, like
        # queryBlock(), queryTransaction(), etc. Default: true
        ledgerQuery: true

        # Will this peer be the target of a SDK listener registration? All peers can
        # produce events but the app typically only needs to connect to one to listen to events.
        # Default: true
        eventSource: true

        # Will this peer be the target of Discovery requests.
        # Default: true
        discover: true

#
# list of participating organizations in this network
#
organizations:
  Org1:
    mspid: Org1MSP

    # The peers that are known to be in this organization
    peers:
      - peer0.org1.example.com

    # Certificate Authorities issue certificates for identification purposes in a Fabric based
    # network. Typically certificates provisioning is done in a separate process outside of the
    # runtime network. Fabric-CA is a special certificate authority that provides a REST APIs for
    # dynamic certificate management (enroll, revoke, re-enroll). The following section is only for
    # Fabric-CA servers.
    certificateAuthorities:
      - ca-org1

    # If the application is going to make requests that are reserved to organization
    # administrators, including creating/updating channels, installing/instantiating chaincodes, it
    # must have access to the admin identity represented by the private key and signing certificate.
    # Both properties can be the PEM string or local path to the PEM file.
    #   path: <the path to a file containing the byte string>
    #    or
    #   pem: <the byte string>
    # Note that this is mainly for convenience in development mode, production systems
    # should not expose sensitive information this way.
    # The SDK should allow applications to set the org admin identity via APIs, and only use
    # this route as an alternative when it exists.
    adminPrivateKey:
      path: <path to file>
       or
      pem: <byte string>
    signedCert:
      path: <path to file>
       or
      pem: <byte string>

  # the profile will contain public information about organizations other than the one it belongs to.
  # These are necessary information to make transaction lifecycles work, including MSP IDs and
  # peers with a public URL to send transaction proposals. The file will not contain private
  # information reserved for members of the organization, such as admin key and certificate,
  # fabric-ca registrar enroll ID and secret, etc.
  Org2:
    mspid: Org2MSP
    peers:
      - peer0.org2.example.com
    certificateAuthorities:
      - ca-org2
    adminPrivateKey:
      path: <path to file>
       or
      pem: <byte string>
    signedCert:
      path: <path to file>
       or
      pem: <byte string>

#
# List of orderers to send transaction and channel create/update requests.
#
orderers:
  orderer.example.com:
    url: grpcs://localhost:7050

    # these are standard properties defined by the gRPC library
    # they will be passed in as-is to gRPC client constructor
    grpcOptions:
      ssl-target-name-override: orderer.example.com

    tlsCACerts:
      path: <path to file>
       or
      pem: <byte string>

#
# List of peers to send various requests to, including endorsement, query
# and event listener registration.
#
peers:
  peer0.org1.example.com:
    # this URL is used to send endorsement and query requests
    url: grpcs://localhost:7051

    grpcOptions:
      ssl-target-name-override: peer0.org1.example.com
      request-timeout: 120001

    tlsCACerts:
      path: <path to file>
       or
      pem: <byte string>

  peer0.org2.example.com:
    url: grpcs://localhost:8051
    grpcOptions:
      ssl-target-name-override: peer0.org2.example.com
    tlsCACerts:
      path: <path to file>
       or
      pem: <byte string>

#
# Fabric-CA is a special kind of Certificate Authority provided by Hyperledger Fabric which allows
# certificate management to be done via REST APIs. Application may choose to use a standard
# Certificate Authority instead of Fabric-CA, in which case this section would not be specified.
#
certificateAuthorities:
  ca-org1:
    url: https://localhost:7054
    # the properties specified under this object are passed to the 'http' client verbatim when
    # making the request to the Fabric-CA server
    httpOptions:
      verify: false
    tlsCACerts:
      path: <path to file>
        or
      pem: <byte string>

    # Fabric-CA supports dynamic user enrollment via REST APIs. A "root" user, a.k.a registrar, is
    # needed to enroll and invoke new users.
    registrar:
      - enrollId: admin
        enrollSecret: adminpw
    # The optional name of the CA.
    caName: ca-org1
```

---
