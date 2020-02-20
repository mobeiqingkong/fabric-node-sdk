# fabric-network:如何选择 Peer 以评估交易(查询)(How to select peers for evaluating transactions (queries))

## fabric-network:如何选择 Peer 以评估交易(查询)(How to select peers for evaluating transactions (queries))

本教程描述了如何选择 Peer 来评估不会随后被写入分类帐的事务，也可以将其视为查询。

### 查询处理策略

SDK 如何评估网络中 pwwe 节点上的事务提供了几种可选策略。 可用策略在 DefaultQueryHandlerStrategies 中定义。 (可选)将所需策略指定为网关上 connect()的参数，并将该策略用于从该网关实例获取的合约的所有交易评估。

如果未指定查询处理策略，则默认使用 MSPID_SCOPE_SINGLE。 这将评估可以从其获得响应的第一个 Peer 上的所有事务，并且仅在该 Peer 失败时才切换到另一个 Peer。

```javascript
const { Gateway, DefaultQueryHandlerStrategies } = require("fabric-network");

const connectOptions = {
  queryHandlerOptions: {
    strategy: DefaultQueryHandlerStrategies.MSPID_SCOPE_SINGLE
  }
};

const gateway = new Gateway();
await gateway.connect(connectionProfile, connectOptions);
```

### 插件查询处理程序

如果需要默认查询处理策略未提供的行为，则可以实现自己的查询处理。 通过指定您自己的工厂功能作为查询处理策略来实现。 工厂函数应返回查询处理程序对象并采用一个参数:

1. Blockchain network: Network

网络提供了对应该评估交易的 Peer 的访问权限。

```javascript
function createQueryHandler(network) {
  /* Your implementation here */
  /* 你的实现在这儿 */
  return new MyQueryHandler(peers);
}

const connectOptions = {
  queryHandlerOptions: {
    strategy: createQueryHandler
  }
};

const gateway = new Gateway();
await gateway.connect(connectionProfile, connectOptions);
```

返回的查询处理程序对象必须实现以下功能。

```javascript
class MyQueryHandler {
  /**
   * Evaluate the supplied query on appropriate peers.
   * @param {Query} query A query object that provides an evaluate()
   * function to invoke itself on specified peers.
   * @returns {Buffer} Query result.
   */
  async evaluate(query) {
    /* Your implementation here */
  }
}
```

有关完整的示例插件查询处理程序实现，请参阅 [sample-query-handler.ts](https://github.com/hyperledger/fabric-sdk-node/blob/master/test/typescript/integration/network-e2e/sample-query-handler.ts)

---
