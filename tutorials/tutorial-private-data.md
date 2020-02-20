# 如何使用私人数据(How to use private data)

## 如何使用私人数据(How to use private data)

本教程说明了如何使用 Node.js SDK API 在 Hyperledger Fabric 网络中存储和检索私有数据。

从 v1.2 开始，Fabric 提供了创建私有数据集合的功能，该功能允许通道上的组织子集支持，提交或查询私有数据，而无需创建单独的通道。 有关更多信息，请参阅:

- [私有数据概念(Private Data Concept)](http://hyperledger-fabric.readthedocs.io/en/latest/private-data/private-data.html)
- [私有数据架构(Private Data Architecture)](http://hyperledger-fabric.readthedocs.io/en/latest/private-data-arch.html)
- [在 Fabric 中使用私有数据(Using Private Data in Fabric)](http://hyperledger-fabric.readthedocs.io/en/latest/private_data_tutorial.html)

### 概述

以下是将私有数据与 Node.js SDK(Fabric 客户端)一起使用的步骤。请查看以下各节，以获取有关这些步骤的详细信息。

1. 创建一个集合定义 json 文件(Create a collection definition json file)
2. 实施链码以存储和查询私有数据(Implement chaincode to store and query private data)
3. 安装并使用集合定义实例化 chaincode(Install and instantiate chaincode with a collection definition)
4. 调用链码以存储和查询私有数据(Invoke chaincode to store and query private data)
5. 清除私人数据(Purge private data)
6. 查询集合定义(Query for a collection definition)

### 创建一个集合定义 json 文件

集合定义描述了谁可以保留数据，将数据分发给多少 Peer，需要多少 Peer 来传播私有数据以及私有数据在私有数据库中保留了多长时间。 Chaincode API 将按集合名称将集合映射到私有数据。

集合定义由以下五个属性组成。

- name:集合的名称。
- policy:定义允许持久保存收集数据的组织 Peer。
- requiredPeerCount:传播私有数据所需的 peer 数目，以作为对链码的背书
- maxPeerCount:出于数据冗余的目的，当前背书 Peer 将尝试向其分发数据的其他 Peer 的数量。如果背书 Peer 发生故障，则如果有请求拉私有数据的请求，则这些其他 Peer 在提交时可用。
- blockToLive:对于价格或个人信息等非常敏感的信息，此值表示数据应在数据块上驻留在专用数据库上的时间。在指定数量的专用数据库块之后，将清除数据。要无限期保留私有数据，即从不清除私有数据，请将 blockToLive 属性设置为 0。

这是一个样本集合定义 JSON 文件，其中包含两个集合定义的数组:

```json
[
  {
    "name": "collectionMarbles",
    "policy": {
      "identities": [
        {
          "role": {
            "name": "member",
            "mspId": "Org1MSP"
          }
        },
        {
          "role": {
            "name": "member",
            "mspId": "Org2MSP"
          }
        }
      ],
      "policy": {
        "1-of": [
          {
            "signed-by": 0
          },
          {
            "signed-by": 1
          }
        ]
      }
    },
    "requiredPeerCount": 1,
    "maxPeerCount": 2,
    "blockToLive": 100
  },
  {
    "name": "collectionMarblePrivateDetails",
    "policy": {
      "identities": [
        {
          "role": {
            "name": "member",
            "mspId": "Org1MSP"
          }
        }
      ],
      "policy": {
        "1-of": [
          {
            "signed-by": 0
          }
        ]
      }
    },
    "requiredPeerCount": 1,
    "maxPeerCount": 1,
    "blockToLive": 100
  }
]
```

此示例包含两个私有数据集合:collectionMarbles 和 collectionMarblePrivateDetails。 collectionMarbles 定义中的 policy 属性允许通道的所有成员(Org1 和 Org2)在私有数据库中拥有私有数据。 collectionMarblesPrivateDetails 集合仅允许 Org1 的成员在其私有数据库中拥有私有数据。

对于 Node.js SDK，必须以与上面所示相同的格式定义策略。

### 实施链码以存储和查询私有数据

Fabric 提供 chaincode API 来存储和查询私人数据。例如，请查看[marbles private data example](https://github.com/hyperledger/fabric-samples/blob/master/chaincode/marbles02_private/go/marbles_chaincode_private.go)，以了解如何使用链码 API 读取和写入私有数据。本示例实现以下功能来管理私有数据。

- readMarble:使用集合 collectionMarbles 查询 name，color，size 和 owner 属性的值。
- readMarblePrivateDetails:使用集合 collectionMarblePrivateDetails 查询 price 属性的值。
- initMarble:存储集合的私有数据。
- delete:从私有数据库中删除 marble 。

### 使用集合定义安装和实例化 chaincode

客户端应用程序通过链码与区块链分类账进行交互。 因此，我们需要在将执行的背书交易的每个 Peer 节点上安装并实例化链码。 当实例化通道上的链码时，集合将与该链码相关联。

- 安装 chaincode。 不需要支持私有数据的特定参数。
- 实例化 chaincode。 为了支持私有数据，请求必须包含 collections-config 属性。

```javascript
const collectionsConfigPath = path.resolve(
  __dirname,
  collection_definition_json_filepath
);
const request = {
  targets: peers,
  chaincodeId: chaincodeId,
  chaincodeType: chaincodeType,
  chaincodeVersion: chaincodeVersion,
  fcn: functionName,
  args: args,
  txId: tx_id,
  "collections-config": collectionsConfigPath
};
const endorsementResults = await channel.sendInstantiateProposal(
  request,
  time_out
);
// additional code needed to validate endorsementResults and send transaction to commit
// 需要额外的代码来验证背书结果并发送交易以提交
```

### 调用链码以存储和查询私有数据

您必须被授权根据集合定义中定义的策略与私有数据进行交易。 回想一下，上面的集合定义允许 Org1 和 Org2 的所有成员访问其私有数据库中的 collectionMarbles(name, color, size, and owner)，但是只有 Org1 中的 Peer 可以访问其私有数据库中的 collectionMarblePrivateDetails(Price)。

作为 Org1 的成员，您可以对 marble 私有数据示例执行以下操作，

- 调用 initMarble 创建一个包含私有数据的 marble 。
- 调用 readMarble 从私有数据库中读取 name, color, size, and owner 以供 collectionMarbles 使用。
- 调用 readMarblePrivateDetails 以从私有数据库中读取 Price 以获取 collectionMarblePrivateDetails。

### 清除私人数据

Hyperledger Fabric 允许客户端应用程序通过设置 blockToLive 属性来清除集合中的私有数据。 当私人数据包括个人或机密信息并且交易双方希望数据的使用寿命有限时，可能需要此选项。

当集合定义文件中的 blockToLive 设置为非零值时，在提交指定数量的块后，Fabric 将自动清除相关的私有数据。 客户端应用程序不需要调用任何 API。

### 查询集合定义

Hyperledger Fabric 允许客户端应用程序向同级查询集合定义。 Node.js SDK(Fabric 客户端)具有一个 API，该 API 将向 Hyperledger Fabric Peer 查询与在指定通道上运行的链码相关联的集合定义。 有关详细信息，请参见[Channel#queryCollectionsConfig](https://hyperledger.github.io/fabric-sdk-node/release-1.4/Channel.html#queryCollectionsConfig)。

```javascript
const request = {
  chaincodeId: chaincodeId,
  target: peer
};

try {
  const response = await channel.queryCollectionsConfig(request);
  // response contains an array of collection definitions
  return response;
} catch (error) {
  throw error;
}
```

### 在事务调用中包含私有数据

如果应用程序希望保留私有数据，则客户端应用程序必须将所有私有数据放入提案请求的临时数据中。 背书结果中不返回瞬态数据，Peer 创建的背书中仅返回瞬态数据的哈希。 在背书期间执行的链码将负责从提案请求的临时区域中提取私有数据，然后与 Peer 的私有数据存储一起使用。

#### 使用 Fabric-Network API 的示例

```javascript
// Private data sent as transient data: { [key: string]: Buffer }
// 发送为临时数据的私有数据:{ [key: string]: Buffer }
const transientData = {
  marblename: Buffer.from("marble1"),
  color: Buffer.from("red"),
  owner: Buffer.from("John"),
  size: Buffer.from("85"),
  price: Buffer.from("99")
};
const result = await contract
  .createTransaction("initMarble")
  .setTransient(transientData)
  .submit();
```

#### 使用 Fabric-Client API 的示例

```javascript
// private data
const transient_data = {
  marblename: Buffer.from("marble1"), // string <-> byte[]
  color: Buffer.from("red"), // string <-> byte[]
  owner: Buffer.from("John"), // string <-> byte[]
  size: Buffer.from("85"), // string <-> byte[]
  price: Buffer.from("99") // string <-> byte[]
};
const tx_id = client.newTransactionID();
const request = {
  chaincodeId: chaincodeId,
  txId: tx_id,
  fcn: "initMarble",
  args: [], // all data is transient data
  transientMap: transient_data // private data
};

// results will not contain the private data
const endorsementResults = await channel.sendTransactionProposal(request);
```

---
