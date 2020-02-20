# fabric-network:如何等待将事务提交到分类帐(How to wait for transactions to be committed to the ledger)

## fabric-network:如何等待将事务提交到分类帐(How to wait for transactions to be committed to the ledger)

本教程描述了 Fabric 网络模块的用户可以选择的方法，以确保已提交的事务在 Peer 上提交。

### 概述

提交事务涉及几个步骤:

1. 将提案发送给背书 Peer。
2. 将背书的交易发送给 Orderer。
3. 最终，交易将提交给网络中的所有 Peer。

在某些情况下，客户应用程序可能会乐于在交易成功发送到 Orderer 后立即进行。在其他情况下，客户端应用程序可能需要确保在继续进行之前，已在要与之交互的某些 Peer 上落实了事务。

重要的是要注意，从特定 Peer 可见的区块链状态将保持不变，直到在该 Peer 上提交交易为止。如果客户端应用程序在成功将已批准的事务发送给 Orderer 之后但在对该 Peer 提交事务之前向同级查询状态，则返回的状态仍将是该事务之前的状态。例如，在从该银行帐户中扣除资金的交易提交给 Orderer 后查询银行余额将返回旧余额，直到该交易最终提交给正在查询的 Peer 为止。

### 事件处理策略

SDK 提供了几种可选的策略，说明在事务调用后如何等待提交事件。 可用策略在 DefaultEventHandlerStrategies 中定义。 (可选)将所需的策略指定为 Gateway 上 connect()的参数，并将该策略用于从该网关实例获取的所有合约交易调用。

如果未指定事件处理策略，则默认使用 MSPID_SCOPE_ALLFORTX。

```javascript
const { Gateway, DefaultEventHandlerStrategies } = require("fabric-network");

const connectOptions = {
  eventHandlerOptions: {
    strategy: DefaultEventHandlerStrategies.MSPID_SCOPE_ALLFORTX
  }
};

const gateway = new Gateway();
await gateway.connect(connectionProfile, connectOptions);
```

将 null 指定为事件处理策略将在成功将背书的事务发送给 Orderer 后立即导致事务调用返回。 它不会等待从 Peer 收到任何提交事件。 有关事件处理选项的更多详细信息，请参见 [DefaultEventHandlerOptions](https://hyperledger.github.io/fabric-sdk-node/release-1.4/module-fabric-network.Gateway.html#~DefaultEventHandlerOptions__anchor)。

### 插件事件处理程序

如果需要默认事件处理策略未提供的行为，则可以实现自己的事件处理。 这是通过指定自己的工厂功能作为事件处理策略来实现的。 工厂函数应返回一个事务事件处理程序对象，并采用两个参数:

1. Transaction: Transaction
2. Blockchain network: Network

该网络提供对应该接收事件的 Peer 和事件中心的访问。

```javascript
function createTransactionEventHandler(transactionId, network) {
  /* Your implementation here */
  return new MyTransactionEventHandler(transactionId, eventHubs);
}

const connectOptions = {
  eventHandlerOptions: {
    strategy: createTransactionEventhandler
  }
};

const gateway = new Gateway();
await gateway.connect(connectionProfile, connectOptions);
```

有关事件处理选项的更多详细信息，请参见[DefaultEventHandlerOptions](https://hyperledger.github.io/fabric-sdk-node/release-1.4/module-fabric-network.Gateway.html#~DefaultEventHandlerOptions__anchor)。 返回的事务事件处理程序对象必须实现以下生命周期功能。

```javascript
class MyTransactionEventHandler {
  /**
   * Called to initiate listening for transaction events.
   * @async
   * @throws {Error} if not in a state where the handling strategy can be satified and the transaction should
   * be aborted. For example, if insufficient event hubs are available.
   */
  async startListening() {
    /* Your implementation here */
  }

  /**
   * Wait until enough events have been received from the event hubs to satisfy the event handling strategy.
   * @async
   * @throws {Error} if the transaction commit is not successfully confirmed.
   */
  async waitForEvents() {
    /* Your implementation here */
  }

  /**
   * Cancel listening for events.
   */
  cancelListening() {
    /* Your imeplementation here */
  }
}
```

有关完整的示例插件事件处理程序的实现，请参见[sample-transaction-event-handler.js](https://github.com/hyperledger/fabric-sdk-node/blob/master/test/integration/network-e2e/sample-transaction-event-handler.js)。

---
