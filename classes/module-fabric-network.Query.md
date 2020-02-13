# fabric-network.Query

## [fabric-network](https://hyperledger.github.io/fabric-sdk-node/release-1.4/module-fabric-network.html)~ Query

描述要在一组指定的对等方上评估的事务。查询处理程序实现用于评估所选对等实体上的事务。

#### new Query()

### Methods

#### &lt;async&gt; evaluate(peers)

在指定对等体上评估查询。

- 参数

| 名称  | 类型                                                                                                   | 描述      |
| ----- | ------------------------------------------------------------------------------------------------------ | --------- |
| peers | Array&lt;[ChannelPeer](https://hyperledger.github.io/fabric-sdk-node/release-1.4/ChannelPeer.html)&gt; | 查询 peer |

返回结果

- 具有同级名称键和关联值的对象，它们是 Buffer 或 Error 对象。

  - 类型

    Object.&lt;String, (Buffer&#124;Error)&gt;

---
