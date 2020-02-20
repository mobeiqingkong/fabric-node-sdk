# fabric-network.Contract

## [fabric-network](https://hyperledger.github.io/fabric-sdk-node/release-1.4/module-fabric-network.html)~ CommitEventListener

表示网络中的智能合约(链码)实例。应用程序应该使用网络的 [getContract](https://hyperledger.github.io/fabric-sdk-node/release-1.4/module-fabric-network.Network.html#getContract) 方法获取一个Contract实例。

#### new Contract()

### Methods

#### &lt;async&gt; addContractListener(listenerName, callback [, options] [, eventHub])

为此事务创建一个提交事件侦听器。

- 参数

| 名称         | 类型                                                         | 描述                                                         |
| ------------ | ------------------------------------------------------------ | ------------------------------------------------------------ |
| listenerName | string                                                       | 侦听器的名称                                                 |
| callback     | function                                                     | 发出事务提交事件时将触发此回调。它包含错误，事件有效负载，块编号，事务ID和状态的参数 |
| options      | module:fabric-network.Network~ListenerOptions                | 可选。注册选项允许起始和终止编号。                           |
| eventHub     | [ ChannelEventHub](https://hyperledger.github.io/fabric-sdk-node/release-1.4/ChannelEventHub.html) | 可选。用于覆盖事件中心选择                                   |

返回结果

- 类型
  - module:fabric-network~CommitEventListener

#### createTransaction(name)

创建一个对象，该对象表示对此合约实现的事务功能的特定调用，并提供对事务调用的更多控制。必须为每个事务调用创建一个新的事务对象。

- 参数

| 名称 | 类型   | 描述           |
| ---- | ------ | -------------- |
| name | String | 交易功能名称。 |

返回结果

- 交易对象。
  - 类型

    [module:fabric-network.Transaction](https://hyperledger.github.io/fabric-sdk-node/release-1.4/module-fabric-network.Transaction.html)

#### &lt;async&gt; evaluateTransaction(name [, args])

评估交易功能并返回其结果。交易功能名称将在背书的Peer上进行评估，但响应不会发送到订购服务，因此不会提交到分类帐。这用于查询世界状态。此函数等效于调用createTransaction(name).evaluate()。

- 参数

| 名称 | 类型   | 描述                         |
| ---- | ------ | ---------------------------- |
| name | string | 交易功能名称。               |
| args | string | 可选。可重复。交易函数参数。 |

返回结果

- 来自交易功能的有效负载响应。

  - 类型

    Buffer

#### &lt;async&gt; submitTransaction(name [, args])

将事务提交到分类帐。交易功能名称将在背书Peer上进行评估，然后提交给订购服务以提交到分类帐。此函数等效于调用createTransaction(name).submit()。

- 参数

| 名称 | 类型   | 描述                         |
| ---- | ------ | ---------------------------- |
| name | string | 交易功能名称。               |
| args | string | 可选。可重复。交易函数参数。 |

抛出

- 如果交易已成功提交给orderer，但在从Peer接收到提交事件之前超时。

  - 类型

    [module:fabric-network.TimeoutError](https://hyperledger.github.io/fabric-sdk-node/release-1.4/module-fabric-network.TimeoutError.html)

返回结果

- 来自交易功能的有效负载响应。

  - 类型

    Buffer

---

