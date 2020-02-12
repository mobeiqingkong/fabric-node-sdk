# fabric-network.Network

## [fabric-network](https://hyperledger.github.io/fabric-sdk-node/release-1.4/module-fabric-network.html)~ Query

描述要在一组指定的peer上评估的事务。查询处理程序实现用于评估所选peer实例上的事务。

#### new Query()

### Methods

&lt;async&gt; evaluate(peers)

指定peer上评估查询。

- 参数

| 名称  | 类型                                                         | 描述     |
| ----- | ------------------------------------------------------------ | -------- |
| peers | Array&lt;[ChannelPeer](https://hyperledger.github.io/fabric-sdk-node/release-1.4/ChannelPeer.html)&gt; | 查询peer |