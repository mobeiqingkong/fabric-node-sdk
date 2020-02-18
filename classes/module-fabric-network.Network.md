# fabric-network.Network

## [fabric-network](https://hyperledger.github.io/fabric-sdk-node/release-1.4/module-fabric-network.html)~ Network

网络代表 Fabric 网络中的一组对等点。应用程序应该使用网关的[getNetwork](https://hyperledger.github.io/fabric-sdk-node/release-1.4/module-fabric-network.Gateway.html#getNetwork) 方法获取一个 Network 实例。

#### new Network()

### Methods

#### &lt;async&gt; addBlockListener(listenerName, callback [, options])

创建一个阻止事件监听器

- 参数

| 名称         | 类型                                                                                                                                                                     | 描述                                       |
| ------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ------------------------------------------ |
| listenerName | String                                                                                                                                                                   | 标识收听者的唯一名称                       |
| callback     | function                                                                                                                                                                 | 当事件被签名（错误，阻止）触发时调用的回调 |
| options      | [module:fabric-network.Network~EventListenerOptions](https://hyperledger.github.io/fabric-sdk-node/release-1.4/module-fabric-network.Network.html#~EventListenerOptions) | 可选。事件监听器选项                       |

返回结果

- 类型

  module:fabric-network~BlockEventListener

#### &lt;async&gt; addCommitListener(transactionId, callback [, options][, eventhub])

为此事务创建一个提交事件侦听器。

- 参数

| 名称          | 类型                                                                                                                                                                     | 描述                                                                              |
| ------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | --------------------------------------------------------------------------------- |
| transactionId | String                                                                                                                                                                   | 正在监视 transactionId                                                            |
| callback      | function                                                                                                                                                                 | 发出事务提交事件时将触发此回调。它包含错误，transactionId，交易状态和块编号的参数 |
| options       | [module:fabric-network.Network~EventListenerOptions](https://hyperledger.github.io/fabric-sdk-node/release-1.4/module-fabric-network.Network.html#~EventListenerOptions) | 可选。事件侦听器选项注册允许开始和结束块号。                                      |
| eventHub      | [ChannelEventHub](https://hyperledger.github.io/fabric-sdk-node/release-1.4/ChannelEventHub.html)                                                                        | 可选。用于覆盖事件中心选择                                                        |

返回结果

- 类型

  module:fabric-network~CommitEventListener

#### getChannel()

获取此网络的基础通道对象表示形式。

返回结果

- 通道

  - 类型

    [Channel](https://hyperledger.github.io/fabric-sdk-node/release-1.4/Channel.html)

#### getContract(chaincodeId [, name])

获取当前网络上合约（链码）的实例。

- 参数

| 名称        | 类型   | 描述             |
| ----------- | ------ | ---------------- |
| chaincodeId | string | 链码标识符。     |
| name        | string | 可选。合约名称。 |

返回结果

- 合约

  - 类型

    [module:fabric-network.Contract](https://hyperledger.github.io/fabric-sdk-node/release-1.4/module-fabric-network.Contract.html)

### Type Definitions

#### CheckpointerFactory(channelName, listenerName [, options])

- 参数

| 名称         | 类型   | 描述                                                           |
| ------------ | ------ | -------------------------------------------------------------- |
| channelName  | String | 检查点所在的通道的名称                                         |
| listenerName | String | 被检查点的侦听器的名称                                         |
| options      | Object | 可选。用于配置自定义检查点行为的选项，即提供数据库连接详细信息 |

返回结果

- 类型

  BaseCheckpointer

#### EventListenerOptions

- 类型

  BaseCheckpointer

- 属性

| 名称                   | 类型    | 默认值 | 描述                         |
| ---------------------- | ------- | ------ | ---------------------------- |
| checkpointer           | Object  |        | 检查点工厂和选项             |
| replay                 | boolean | false  | 事件重播和侦听器上的检查点   |
| filtered               | boolean | false  | 是否使用接收过滤块事件       |
| unregister             | boolean | false  | 收到单个事件后立即注销侦听器 |
| startBlock             | number  |        | 播放事件的第一个块           |
| endBlock               | number  |        | 播放事件的最后一块           |
| asArray                | boolean |        | 会将所有事件分块发送给回调   |
| eventHubConnectWait    | number  | 1000   | 寻找新的事件中心之前的毫秒数 |
| eventHubConnectTimeout | number  | 30000  | 超时事件中心连接之前的毫秒数 |

- checkpointer 属性

| 名称    | Type                                                                                                                                                                   | 默认值 | 描述                 |
| ------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------ | -------------------- |
| factory | [module:fabric-network.Network~CheckpointerFactory](https://hyperledger.github.io/fabric-sdk-node/release-1.4/module-fabric-network.Network.html#~CheckpointerFactory) |        | 检查站工厂           |
| options | Object                                                                                                                                                                 | false  | 可选。检查点配置选项 |

---
