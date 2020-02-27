# BasicCommitHandler

## 说明

这是 [CommitHandler](https://hyperledger.github.io/fabric-sdk-node/release-1.4/module-api.CommitHandler.html) API 的实现。它将一次从提供的列表或当前分配给通道的列表中提交要提交给一个 orderer 的交易。

### new BasicCommitHandler(channel)

- Constructor
- 参数

| 名称    | 类型                                                                              | 描述               |
| ------- | --------------------------------------------------------------------------------- | ------------------ |
| channel | [Channel](https://hyperledger.github.io/fabric-sdk-node/release-1.4/Channel.html) | 此处理程序的通道。 |

### Extends

- [module:api.CommitHandler](https://hyperledger.github.io/fabric-sdk-node/release-1.4/module-api.CommitHandler.html)

### Methods

#### &lt;static&gt; create(channel)

创建提交程序处理程序实例的工厂方法。

- 参数

| 名称    | 类型                                                                              | 描述                               |
| ------- | --------------------------------------------------------------------------------- | ---------------------------------- |
| channel | [Channel](https://hyperledger.github.io/fabric-sdk-node/release-1.4/Channel.html) | 该提交处理程序将要服务的通道实例。 |

- 返回结果

  处理程序的实例

  - 类型

    [BasicCommitHandler](https://hyperledger.github.io/fabric-sdk-node/release-1.4/BasicCommitHandler.html)

#### commit(params)

此方法将处理参数以确定 orderer。处理程序将使用提供的 orderer 或使用分配给通道的 orderer。处理程序应执行故障转移，并使用所有可用的订购程序发送已背书的交易。

- 参数

| 名称    | 类型                    | 描述                              |
| ------- | ----------------------- | --------------------------------- |
| channel | CommitHandlerParameters | 一个 EndorsementHandlerParameters |

- 继承自 :

[module:api.CommitHandler#commit](https://hyperledger.github.io/fabric-sdk-node/release-1.4/module-api.CommitHandler.html#commit)

- 覆写 (Overrides):

[module:api.CommitHandler#commit](https://hyperledger.github.io/fabric-sdk-node/release-1.4/module-api.CommitHandler.html#commit)

- 返回结果

  [ProposalResponseObject](https://hyperledger.github.io/fabric-sdk-node/release-1.4/global.html#ProposalResponseObject)的 Promise，与直接调用 [Channel](https://hyperledger.github.io/fabric-sdk-node/release-1.4/Channel.html) 'sendTransactionProposal'方法的结果相同。

  - 类型

    Promise

#### initialize()

初始化通道后，通道将调用此方法。

- 继承自 :

  [module:api.CommitHandler#initialize](https://hyperledger.github.io/fabric-sdk-node/release-1.4/module-api.CommitHandler.html#initialize)

- 覆写 (Overrides):

  [module:api.CommitHandler#initialize](https://hyperledger.github.io/fabric-sdk-node/release-1.4/module-api.CommitHandler.html#initialize)

---
