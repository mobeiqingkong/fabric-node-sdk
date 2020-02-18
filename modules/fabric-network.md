# fabric-network

## 概述

该模块提供了用于与智能合约进行交互的更高级别的 API，并且是客户端应用程序与部署到 Hyperledger Fabric 区块链网络的智能合约进行交互的推荐 API。

请注意，此 API 当前未提供管理功能，例如安装和启动智能合约。对于这些任务或其他特定的高级用法，应使用较低级别的*Fabric-Client* API。通过 fabric-network API 对象提供相关 fabric-client 对象的访问。

[TypeScript](http://www.typescriptlang.org/) 定义包含在此模块中。

## 入门

与 Hyperledger Fabric 区块链网络进行交互的入口点是 [Gateway](https://hyperledger.github.io/fabric-sdk-node/release-1.4/module-fabric-network.Gateway.html)类。该类提供了与区块链网络中 peer 节点的连接，并允许访问以该 peer 节点为成员的所有区块链[网络](https://hyperledger.github.io/fabric-sdk-node/release-1.4/module-fabric-network.Network.html)（通道）。继而提供运行在该区块链网络中的智能[合约](https://hyperledger.github.io/fabric-sdk-node/release-1.4/module-fabric-network.Contract.html)（链码）的访问，并且可以向其 [提交](https://hyperledger.github.io/fabric-sdk-node/release-1.4/module-fabric-network.Transaction.html#submit)[交易](https://hyperledger.github.io/fabric-sdk-node/release-1.4/module-fabric-network.Transaction.html)或可以[评估](https://hyperledger.github.io/fabric-sdk-node/release-1.4/module-fabric-network.Transaction.html#evaluate)查询 。

可以将私有数据作为[临时](https://hyperledger.github.io/fabric-sdk-node/release-1.4/module-fabric-network.Transaction.html#setTransient) 数据提交到事务处理，以防止将其记录在分类帐中。

## 示例

```javascript
// 获取我们应用程序想要交互的智能合约
const wallet = new FileSystemWallet(walletDirectoryPath);
const gatewayOptions: GatewayOptions = {
  identity: "user@example.org", // 先前导入的身份
  wallet
};
const gateway = new Gateway();
await gateway.connect(commonConnectionProfile, gatewayOptions);
const network = await gateway.getNetwork(channelName);
const contract = network.getContract(chaincodeId);

// 向智能合约提交交易或者评估查询
const result = await contract
  .createTransaction(transactionName)
  .setTransient(privateData)
  .submit(arg1, arg2);
```

## 类

- [AbstractEventHubSelectionStrategy](../classes/module-fabric-network.AbstractEventHubSelectionStrategy.md)

- [AbstractEventListener](../classes/module-fabric-network.AbstractEventListener.md)

- [BaseCheckpointer](../classes/module-fabric-network.BaseCheckpointer.md)

- [BaseWallet](../classes/module-fabric-network.BaseWallet.md)

- [CommitEventListener](../classes/module-fabric-network.CommitEventListener.md)

- [Contract](../classes/module-fabric-network.Contract.md)

- [ContractEventListener](../classes/module-fabric-network.ContractEventListener.md)

- [CouchDBWallet](../classes/module-fabric-network.CouchDBWallet.md)

- [EventHubDisconnectError](../classes/module-fabric-network.EventHubDisconnectError.md)

- [EventHubManager](../classes/module-fabric-network.EventHubManager.md)

- [FabricError](../classes/module-fabric-network.FabricError.md)

- [FileSystemCheckpointer](../classes/module-fabric-network.FileSystemCheckpointer.md)

- [FileSystemWallet](../classes/module-fabric-network.FileSystemWallet.md)

- [Gateway](../classes/module-fabric-network.Gateway.md)

- [HSMWalletMixin](../classes/module-fabric-network.HSMWalletMixin.md)

- [InMemoryWallet](../classes/module-fabric-network.InMemoryWallet.md)

- [Network](../classes/module-fabric-network.Network.md)

- [Query](../classes/module-fabric-network.Query.md)

- [RoundRobinEventHubSelectionStrategy](../classes/module-fabric-network.RoundRobinEventHubSelectionStrategy.md)

- [TimeoutError](../classes/module-fabric-network.TimeoutError.md)

- [Transaction](../classes/module-fabric-network.Transaction.md)

- [X509WalletMixin](../classes/module-fabric-network.X509WalletMixin.md)

### 类型定义

#### DefaultEventHandlerStrategies

- 属性：

| 名称                   | 类型 | 描述                                                                                                                                                 |
| ---------------------- | ---- | ---------------------------------------------------------------------------------------------------------------------------------------------------- |
| MSPID_SCOPE_ALLFORTX   |      | 监听来自客户端身份组织中所有 peer 节点的事务提交事件。 SubmitTransaction 函数将等待，直到从当前连接的所有 peer 节点接收到成功事件为止（至少 1 个）。 |
| MSPID_SCOPE_ANYFORTX   |      | 侦听来自客户端身份组织中所有 peer 节点的事务提交事件。 SubmitTransaction 函数将等待，直到从任何 peer 节点接收到成功事件为止。                        |
| NETWORK_SCOPE_ALLFORTX |      | 侦听网络中所有对等方的事务提交事件。 SubmitTransaction 函数将等待，直到从当前连接的所有 peer 节点接收到成功事件为止（至少 1 个）。                   |
| NETWORK_SCOPE_ANYFORTX |      | 侦听网络中所有对等方的事务提交事件。 SubmitTransaction 函数将等待，直到从任何 peer 节点接收到成功事件为止。                                          |

#### DefaultQueryHandlerStrategies

- 属性：

| 名称                    | 类型     | 描述                                                                               |
| ----------------------- | -------- | ---------------------------------------------------------------------------------- |
| MSPID_SCOPE_SINGLE      | function | 向已连接的组织查询任意一个事件中心。如果查询成功，则对所有查询使用相同的事件中心。 |
| MSPID_SCOPE_ROUND_ROBIN | function | 向已连接的组织查询任意一个事件中心。每次成功查询后使用下一个可用的 peer 节点。     |

#### CheckpointFactories

- 属性：

| 名称                     | 类型     | 描述                 |
| ------------------------ | -------- | -------------------- |
| FILE_SYSTEM_CHECKPOINTER | function | 使用文件系统的检查点 |

#### DefaultEventHubSelectionStrategies

- 属性

| 名称                    | 类型     | 描述                                                                               |
| ----------------------- | -------- | ---------------------------------------------------------------------------------- |
| MSPID_SCOPE_ROUND_ROBIN | function | 当出现连接断开时，会重新连接到组织中的任意一个事件发出端。以“循环”方式选择事件中心 |
---
