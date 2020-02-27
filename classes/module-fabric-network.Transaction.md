# fabric-network.Transaction

## [fabric-network](https://hyperledger.github.io/fabric-sdk-node/release-1.4/module-fabric-network.html)~ Transaction

表示事务功能的特定调用，并提供有关如何调用该事务的可简化性。应用程序应通过调用 [Contract#createTransaction()](https://hyperledger.github.io/fabric-sdk-node/release-1.4/module-fabric-network.Contract.html#createTransaction) 获得此类的实例。

此类的实例是有状态的。必须为每个事务调用创建一个新实例。

### new Transaction()

### Methods

#### &lt;async&gt; addCommitListener(callback [, options][, eventhub])

为此事务创建一个提交事件侦听器。

- 参数

| 名称     | 类型                                                                                               | 描述                                                                              |
| -------- | -------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------- |
| callback | function                                                                                           | 发出事务提交事件时将触发此回调。它包含错误，transactionId，交易状态和块编号的参数 |
| options  | module:fabric-network.Network~ListenerOptions                                                      | 可选。注册选项允许起始和终止编号。                                                |
| eventHub | [ChannelEventHub](https://hyperledger.github.io/fabric-sdk-node/release-1.4/ChannelEventHub.html) | 可选。PEM 格式的私钥。用于覆盖事件中心选择                                        |

返回结果

- 类型

  module:fabric-network~CommitEventListener

#### &lt;async&gt; evaluate( [args])

评估交易功能并返回其结果。交易功能将在背书的 Peer 上进行评估，但响应不会发送到订购服务，因此不会提交到分类账。这用于查询世界状态。

- 参数

| 名称 | 类型   | 描述                         |
| ---- | ------ | ---------------------------- |
| args | String | 可选。可重复。交易函数参数。 |

返回结果

- 来自交易功能的有效负载响应。

  - 类型

    Buffer

#### getName()

获取交易功能的全限定名称。

返回结果

- 交易名称。

  - 类型

    String

#### getNetwork()

从合约返回网络

返回结果

- 类型

  [module:fabric-network.Network](https://hyperledger.github.io/fabric-sdk-node/release-1.4/module-fabric-network.Network.html)

#### getTransactionID()

获取将用于此事务调用的 ID。

返回结果

- Transaction ID。

  - 类型

    module:fabric-client.TransactionID

#### setEndorsingPeers(peers)

设置此事务提交到分类帐时应用于背书的 Peer。

- 参数

| 名称  | 类型                                                                                                    | 描述      |
| ----- | ------------------------------------------------------------------------------------------------------- | --------- |
| peers | Array.&lt;[ChannelPeer](https://hyperledger.github.io/fabric-sdk-node/release-1.4/ChannelPeer.html)&gt; | 背书 Peer |

返回结果

- 这个对象，允许函数链接。

  - 类型

    [module:fabric-network.Transaction](https://hyperledger.github.io/fabric-sdk-node/release-1.4/module-fabric-network.Transaction.html)

#### &lt;async&gt; submit( [args])

将事务提交到分类帐。交易功能名称将在背书 Peer 上进行评估，然后提交给订购服务以提交到分类帐。

- 参数

| 名称 | 类型   | 描述                         |
| ---- | ------ | ---------------------------- |
| args | String | 可选。可重复。交易函数参数。 |

抛出

- 如果交易已成功提交给orderer，但在从peer接收到提交事件之前超时。

  - 类型

    [module:fabric-network.TimeoutError](https://hyperledger.github.io/fabric-sdk-node/release-1.4/module-fabric-network.TimeoutError.html)

- 如果发生基础基础结构故障。错误的"responses"属性包含来自背书peer的提议响应对象数组。

  - 类型

    Error

返回结果

- 来自交易功能的有效负载响应。

  - 类型

    Buffer

---
