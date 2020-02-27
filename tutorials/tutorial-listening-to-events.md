# fabric-network:如何监听事件(How to listen to events)

## 说明

## 使用 Fabric Network 收听事件

本教程描述了使用 Fabric-Network 模块侦听网络发出的事件的不同方法。

## 概述

可以订阅三种事件类型:

1. 合约事件 - 链码开发人员在交易中明确发出的事件
2. 事务(Commit)事件 - 调用后提交事务时自动发出的事件
3. 块事件 - 提交块后自动发出的事件

监听这些事件可以使应用程序做出反应而无需直接调用事务。 这在用例(例如监视网络分析)中是理想的。

## 用法

每种侦听器类型都至少使用一个参数，即事件回调。 这是接收事件时调用的函数。

预期给定的回调函数是一个 Promise，这意味着回调可以执行异步任务而不会冒丢失事件的风险。

## 选项

[module:fabric-network.Network~EventListenerOptions](https://hyperledger.github.io/fabric-sdk-node/release-1.4/module-fabric-network.Network.html#~EventListenerOptions).

注意:侦听器将连接到事件中心，并默认要求接收未过滤的事件。要接收过滤的事件，请设置 EventListenerOptions.filtered:true。

## 命名

所有事件侦听器(包括使用事务 ID 的 CommitEventListeners)在 Network 级别必须具有唯一的名称

## 合约事件

```javascript
const gateway = new Gateway();
await gateway.connect(connectionProfile, gatewayOptions);
const network = await gateway.getNetwork("mychannel");
const contract = network.getContract("my-contract");

/**
 * @param {String} listenerName the name of the event listener
 * @param {String} eventName the name of the event being listened to
 * @param {Function} callback the callback function with signature (error, event, blockNumber, transactionId, status)
 * @param {module:fabric-network.Network~EventListenerOptions} options
 **/
const listener = await contract.addContractListener(
  "my-contract-listener",
  "sale",
  (err, event, blockNumber, transactionId, status) => {
    if (err) {
      console.error(err);
      return;
    }
    console.log(
      `Block Number: ${blockNumber} Transaction ID: ${transactionId} Status: ${status}`
    );
  }
);
```

请注意，无需指定事件中心，因为 EventHubSelectionStrategy 将自动选择它。

### 区块事件

```javascript
const gateway = new Gateway();
await gateway.connect(connectionProfile, gatewayOptions);
const network = await gateway.getNetwork("mychannel");

/**
 * @param {String} listenerName the name of the event listener
 * @param {Function} callback the callback function with signature (error, blockNumber, transactionId, status)
 * @param {module:fabric-network.Network~EventListenerOptions} options
 **/
const listener = await network.addBlockListener(
  "my-block-listener",
  (error, block) => {
    if (err) {
      console.error(err);
      return;
    }
    console.log(`Block: ${block}`);
  }
);
```

侦听块事件时，指定要过滤事件还是不过滤事件很重要，因为这将确定哪个事件中心与请求兼容。

### Option 1

```javascript
const gateway = new Gateway();
await gateway.connect(connectionProfile, gatewayOptions);
const network = await gateway.getNetwork("mychannel");
const contract = network.getContract("my-contract");

const transaction = contract.newTransaction("sell");
/**
 * @param {String} transactionId the transaction ID
 * @param {Function} callback the callback function with signature (error, transactionId, status, blockNumber)
 * @param {Object} options
 **/
const listener = await network.addCommitListener(
  transaction.getTransactionID().getTransactionID(),
  (err, transactionId, status, blockNumber) => {
    if (err) {
      console.error(err);
      return;
    }
    console.log(
      `Transaction ID: ${transactionId} Status: ${status} Block number: ${blockNumber}`
    );
  }
);
```

### Option 2

```javascript
const gateway = new Gateway();
await gateway.connect(connectionProfile, gatewayOptions);
const network = await gateway.getNetwork("mychannel");
const contract = network.getContract("my-contract");

const transaction = contract.newTransaction("sell");
/**
 * @param {String} transactionId the transaction ID
 * @param {Function} callback the callback function with signature (error, transactionId, status, blockNumber)
 * @param {Object} options
 **/
const listener = await transaction.addCommitListener(
  (err, transactionId, status, blockNumber) => {
    if (err) {
      console.error(err);
      return;
    }
    console.log(
      `Transaction ID: ${transactionId} Status: ${status} Block number: ${blockNumber}`
    );
  }
);
```

Network.addCommitListener 和 Contract.addCommitListener 都具有可选的 eventHub 参数。 设置后，侦听器将仅侦听该事件中心，并且在无法预料的断开连接的情况下，它将尝试重新连接而无需使用 EventHubSelectionStrategy。

## 检查点(Checkpointing)

[fabric-network: How to replay missed events](https://hyperledger.github.io/fabric-sdk-node/release-1.4/tutorial-event-checkpointer.html)

## 起始块和结束块

在 module:fabric-network\~EventListenerOptions 中，可以指定一个 startBlock 和一个 endBlock。 其行为与本教程中所示的 ChannelEventHub 上相同选项的行为相同:[fabric-client:如何使用基于通道的事件服务(fabric-client: How to use the channel-based event service)](https://hyperledger.github.io/fabric-sdk-node/release-1.4/tutorial-channel-events.html).。 使用 startBlock 和 endBlock 可以使用检查点对该侦听器接收到的事件禁用事件重播。

## 注销监听器

addContractListener，addBlockListener 和 addCommitListener 分别返回 ContractEventListener，BlockEventListener 和 CommitEventListener。 每个函数都有一个 unregister()函数，该函数将从事件中心中删除侦听器，这意味着在再次调用 register()之前，不会再从该侦听器接收任何事件。

---
