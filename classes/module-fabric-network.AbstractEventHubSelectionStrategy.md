# fabric-network.AbstractEventHubSelectionStrategy

## [fabric-network](https://hyperledger.github.io/fabric-sdk-node/release-1.4/module-fabric-network.html)~ AbstractEventHubSelectionStrategy

为其他选择策略提供接口的抽象选择策略

#### new AbstractEventHubSelectionStrategy(peers)

- 参数

| 名称  | 类型                                                         | 描述                     |
| ----- | ------------------------------------------------------------ | ------------------------ |
| peers | Array.&lt;[Peer](https://hyperledger.github.io/fabric-sdk-node/release-1.4/Peer.html)&gt; | 使用其事件中心的peer列表 |

### Methods

#### getNextPeer()

获取下一个peer

返回结果

- 类型

  [Peer](https://hyperledger.github.io/fabric-sdk-node/release-1.4/Peer.html)

#### updateEventHubAvailability(deadPeer)

更新peer可用性

- 参数

| 名称     | 类型                                                         | 描述           |
| -------- | ------------------------------------------------------------ | -------------- |
| deadPeer | [Peer](https://hyperledger.github.io/fabric-sdk-node/release-1.4/Peer.html) | 与peer断开连接 |