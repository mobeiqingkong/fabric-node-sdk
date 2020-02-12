# fabric-network.EventHubManager

## [fabric-network](https://hyperledger.github.io/fabric-sdk-node/release-1.4/module-fabric-network.html)~ EventHubManager

事件中心管理器负责创建和分发事件中心。它使用事件中心工厂重用现有的事件中心，并维护其自己的用于事件重播的新事件中心列表。

#### new EventHubManager(network)

Constructor

- 参数

| 名称    | 类型                                                         | 描述 |
| ------- | ------------------------------------------------------------ | ---- |
| network | [module:fabric-network.Network](https://hyperledger.github.io/fabric-sdk-node/release-1.4/module-fabric-network.Network.html) | 网络 |

### Methods

#### dispose()

断开连接并删除所有事件中心

#### getEventHub(peer, filtered)

获取事件中心。如果给定一个peer，它将获取该peer事件中心，否则将获取由EventHubSelectionStrategy定义的下一个peer

- 参数

| 名称     | 类型                                                         | 描述                         |
| -------- | ------------------------------------------------------------ | ---------------------------- |
| peer     | [Peer](https://hyperledger.github.io/fabric-sdk-node/release-1.4/Peer.html) | peer实例                     |
| filtered | boolean                                                      | 标记来区分已过滤和未过滤事件 |

返回结果

- 事件中心
  - 类型

    [ChannelEventHub](https://hyperledger.github.io/fabric-sdk-node/release-1.4/ChannelEventHub.html)

#### getEventHubs(peers)

从EventHubFactory获取事件中心的列表以获取peer的列表

- 参数

| 名称 | 类型                                                         | 描述     |
| ---- | ------------------------------------------------------------ | -------- |
| peer | Array.&lt;[Peer](https://hyperledger.github.io/fabric-sdk-node/release-1.4/Peer.html)&gt; | peer实例 |

#### getFixedEventHub(peer)

将获得一个新的事件中心实例，而无需选择新的peer实例

- 参数

| 名称 | 类型                                                         | 描述     |
| ---- | ------------------------------------------------------------ | -------- |
| peer | [Peer](https://hyperledger.github.io/fabric-sdk-node/release-1.4/Peer.html) | peer实例 |

返回结果

- 事件中心

  - 类型

    [ChannelEventHub](https://hyperledger.github.io/fabric-sdk-node/release-1.4/ChannelEventHub.html)

#### getReplayEventHub(peer)

获取给定peer的新事件中心实例，并更新已创建的新事件中心的列表

- 参数

| 名称 | 类型                                                         | 描述     |
| ---- | ------------------------------------------------------------ | -------- |
| peer | [Peer](https://hyperledger.github.io/fabric-sdk-node/release-1.4/Peer.html) | peer实例 |

返回结果

- 事件中心

  - 类型

    [ChannelEventHub](https://hyperledger.github.io/fabric-sdk-node/release-1.4/ChannelEventHub.html)

#### updateEventHubAvailability(deadPeer)

与peer一起调用时，它将使用peer的新状态更新EventHubSelectionStrategy以允许采用智能策略

- 参数

| 名称 | 类型                                                         | 描述     |
| ---- | ------------------------------------------------------------ | -------- |
| peer | [Peer](https://hyperledger.github.io/fabric-sdk-node/release-1.4/Peer.html) | peer实例 |