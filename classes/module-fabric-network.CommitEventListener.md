# fabric-network.CommitEventListener

## [fabric-network](https://hyperledger.github.io/fabric-sdk-node/release-1.4/module-fabric-network.html)~ CommitEventListener

提交事件侦听器处理事务提交事件

#### new CommitEventListener(network, transactionId, eventCallback, options)

- 参数

| 名称          | 类型                                                         | 描述                                                         |
| ------------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| network       | [module:fabric-network.Network](https://hyperledger.github.io/fabric-sdk-node/release-1.4/module-fabric-network.Network.html) | fabric 网络                                                  |
| transactionId | string                                                       | 正在监听的交易ID                                             |
| eventCallback | function                                                     | 提交事务时调用事件回调。它具有签名（err，transactionId，status，blockNumber） |
| options       | module:fabric-network.Network~ListenerOptions                |                                                              |

***