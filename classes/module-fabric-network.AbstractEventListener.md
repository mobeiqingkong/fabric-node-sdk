# fabric-network.AbstractEventListener

## [fabric-network](https://hyperledger.github.io/fabric-sdk-node/release-1.4/module-fabric-network.html)~ AbstractEventListener

事件侦听器基类处理跨合约，事务和块事件侦听器的初始化通用属性。事件侦听器的实例是有状态的，只能用于一个侦听器

### new AbstractEventListener(network, listenerName, eventCallback, options)

Constructor

- 参数

| 名称          | 类型                                                                                                                          | 描述                                                                      |
| ------------- | ----------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------- |
| network       | [module:fabric-network.Network](https://hyperledger.github.io/fabric-sdk-node/release-1.4/module-fabric-network.Network.html) | 网络                                                                      |
| listenerName  | string                                                                                                                        | 标识侦听器的唯一名称。                                                    |
| eventCallback | function                                                                                                                      | 触发事件时调用的函数。它具有签名(err，... args)，其中 args 取决于事件类型 |
| options       | module:fabric-network.Network~ListenerOptions                                                                                 | 事件处理程序选项                                                          |

### Methods

#### getCheckpointer()

返回由检查点工厂创建的检查点实例

返回结果

- 特定于此侦听器的 Checkpointer 实例。

  - 类型

    BaseCheckpointer

#### getEventHubManager()

从网络返回事件中心管理器

返回结果

- 事件中心管理。

  - 类型

    EventHubManager

#### isregistered()

返回结果

- 监听注册状态。

  - 类型

    boolean

#### &lt;async&gt; register()

由父类注册函数调用。保存开始侦听所需的信息，如果类型不正确，则断开事件中心的连接

#### setEventHub(eventHub, isFixed)

- 参数

| 名称     | 类型                                                                                              | 描述                                  |
| -------- | ------------------------------------------------------------------------------------------------- | ------------------------------------- |
| eventHub | [ChannelEventHub](https://hyperledger.github.io/fabric-sdk-node/release-1.4/ChannelEventHub.html) | 事件中心。                            |
| isFixed  | boolean                                                                                           | 如果为 true，则仅使用此 peer 事件中心 |

#### unregister()

由超级类取消注册功能调用。

---
