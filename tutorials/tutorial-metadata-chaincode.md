# fabric-client:如何在链码安装过程中添加 CouchDB 索引(How to add CouchDB indexes during chaincode installation)

## 说明

本教程说明了如何在链式代码安装中添加元数据。从 v1.1 开始，唯一的元数据就是可以添加到通道分类帐的 CouchDB 状态数据库中的索引。

了解更多信息:

- [Hyperledger Fabric 入门(getting started with Hyperledger Fabric)](http://hyperledger-fabric.readthedocs.io/en/latest/build_network.html)
- [设置一个 CouchDB 作为状态数据库(setting up a CouchDB as the state database)](http://hyperledger-fabric.readthedocs.io/en/latest/couchdb_as_state_database.html)

以下内容假定您对 Hyperledger Fabric 网络(orderer 和 peer)以及 Node 应用程序开发(包括 Java Promise 的使用)有所了解。

### 概述

Fabric 1.1 引入了在 CouchDB 状态数据库中定义索引的功能，以帮助提高您在链码中进行的查询的性能。 索引定义必须为 JSON 格式，并且文件扩展名为.json。 这些定义将包含在发送到 Fabric peer 的链码安装包中。

#### 修改后的 API，允许使用元数据

- client.installChaincode() - 安装请求中可能包含一个新属性('metadataPath')。 metadataPath 的值是一个字符串，表示包含 JSON 索引文件的目录结构的绝对路径。

### 安装链码

以下示例将安装链码" my_chaincode"并包括索引文件。

```javascript
let targets = buildTargets(); //build the list of peers that will require this chaincode // 建立将需要此链代码的Peer列表
let chaincode_path = path.resolve(
  __dirname,
  "../chaincode/src/node_cc/my_chaincode"
);
let metadata_path = path.resolve(__dirname, "../chaincode/my_indexes");

// send proposal to install
// 发送安装提议
var request = {
  targets: targets,
  chaincodePath: chaincode_path,
  metadataPath: metadata_path, // notice this is the new attribute of the request //注意，这是请求的新属性
  chaincodeId: "my_chaincode",
  chaincodeType: "node",
  chaincodeVersion: "v1"
};

client.installChaincode(request).then(
  results => {
    var proposalResponses = results[0];
    // check the results
    // 检查结果
  },
  err => {
    console.log(
      "Failed to send install proposal due to error: " + err.stack
        ? err.stack
        : err
    );
    throw new Error(
      "Failed to send install proposal due to error: " + err.stack
        ? err.stack
        : err
    );
  }
);
```

下面显示了用作上面的 metadataPath 的路径。 这是路径下所需的必需目录结构。 索引目录将保存带有索引定义的文件。 所需的目录结构和带有'json'扩展名的文件将包含在'META_INF'软件包目录下的 chaincode 安装软件包中。 " META-INF"不应包含在本地目录结构中。

```shell
 ..
  <> chaincode
  │
  └─── <> my_indexes // here is where the 'metadataPath' will point to // 这里的metadataPath将指向
       │
       └─── <> statedb //starting here are the required directories // 从这里开始是必需的目录
            │
            └─── <> couchdb
                 │
                 └─── <> indexes // this directory will contain the index files // 该目录将包含索引文件
                         index-owner.json   // these will be the index files and must // 这些将成为索引文件，并且必须
                         index-address.json // have the file extension of 'json' //具有'json'的文件扩展名
```

每个索引必须在其自己的文本文件中定义，扩展名为\* .json，并包含遵循 [CouchDB 索引 JSON 语法(CouchDB index JSON syntax)](http://docs.couchdb.org/en/2.1.1/api/database/find.html#db-index)的 JSON 格式的索引定义 。

```json
{
  "index": { "fields": ["docType", "owner"] },
  "ddoc": "indexOwnerDoc",
  "name": "indexOwner",
  "type": "json"
}
```

---
