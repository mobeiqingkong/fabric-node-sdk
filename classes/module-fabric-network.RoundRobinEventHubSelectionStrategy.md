# fabric-network.RoundRobinEventHubSelectionStrategy

## [fabric-network](https://hyperledger.github.io/fabric-sdk-node/release-1.4/module-fabric-network.html)~ RoundRobinEventHubSelectionStrategy

循环事件中心策略是一种基本的事件中心选择策略，用于将负载分散到不同的事件中心并跟踪事件中心的可用性

#### new RoundRobinEventHubSelectionStrategy(peers)

Constructor.

- 参数

| 名称  | 类型                                                         | 描述                     |
| ----- | ------------------------------------------------------------ | ------------------------ |
| peers | Array&lt;[Peer](https://hyperledger.github.io/fabric-sdk-node/release-1.4/Peer.html)&gt; | 该策略可以选择的peer列表 |

实现:

- [module:fabric-network.AbstractEventHubSelectionStrategy](https://hyperledger.github.io/fabric-sdk-node/release-1.4/module-fabric-network.AbstractEventHubSelectionStrategy.html)

### Methods

#### getNextPeer()

获取peer列表中的下一个peer

实现:

- [module:fabric-network.AbstractEventHubSelectionStrategy#getNextPeer](https://hyperledger.github.io/fabric-sdk-node/release-1.4/module-fabric-network.AbstractEventHubSelectionStrategy.html#getNextPeer)

返回结果

- 类型

  [Peer](https://hyperledger.github.io/fabric-sdk-node/release-1.4/Peer.html)

***

