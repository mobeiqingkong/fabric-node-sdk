# api

 此模块定义了node.js SDK的可插入组件API。 

## 类

- [CommitHandler](../classes/module-api.CommitHandler.md)
- [CryptoSuite](../classes/module-api.CryptoSuite.md)
- [EndorsementHandler](../classes/module-api.EndorsementHandler.md)
- [Hash](../classes/module-api.Hash.md)
- [Key](../classes/module-api.Key.md)
- [KeyValueStore](../classes/module-api.KeyValueStore.md)

## 类型定义

#### CommitHandlerParameters

- 类型：Object

- 属性：


| 名称            | 类型   | 描述                                                         |
| --------------- | ------ | ------------------------------------------------------------ |
| request         | Object | [TransactionRequest](../global.md#TransactionRequest)        |
| signed_envelope | Object | 一个发送到订购者(orderer)的包含编码许可提议和发送者签名的对象。 |
| timeout         | Number | 由sendTransaction方法传递超时设置。                          |

#### EndorsementHandlerParameters

- 类型：Object
- 属性：

| 名称            | 类型   | 描述                                                         |
| --------------- | ------ | ------------------------------------------------------------ |
| request         | Object | [ChaincodeInvokeRequest](../global.md#ChaincodeInvokeRequest) |
| signed_proposal | Object | 编码协议原型“ SignedProposal”在调用处理程序之前由sendTransactionProposal方法创建。将成为目标peer节点认可的对象 |
| timeout         | Number | 由sendTransaction方法传递超时设置。                          |