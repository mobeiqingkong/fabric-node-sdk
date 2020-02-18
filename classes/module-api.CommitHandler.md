# api.CommitHandler

## [api](https://hyperledger.github.io/fabric-sdk-node/release-1.4/module-api.html). CommitHandler

提交处理的基类

#### new CommitHandler()

### Methods

#### &lt;static&gt; create()

通道将调用此静态方法来创建此处理程序的实例。该处理程序正在使用的 channel 对象将被传递。

#### commit(params)

此方法将处理参数以确定 orderers。处理程序将使用提供的 orderer 或使用分配给通道的 orderer。处理程序应执行故障转移，并使用所有可用的 orderer 程序发送已认可的交易。

- 参数

| 名称   | 类型                    | 描述                              |
| ------ | ----------------------- | --------------------------------- |
| params | CommitHandlerParameters | 一个 EndorsementHandlerParameters |

返回结果

- [ProposalResponseObject](https://hyperledger.github.io/fabric-sdk-node/release-1.4/global.html#ProposalResponseObject)的 Promise，与直接调用[Channel](https://hyperledger.github.io/fabric-sdk-node/release-1.4/Channel.html)'sendTransactionProposal'方法的结果相同。

  - 类型

    Promise

#### initialize()

初始化通道后，通道将调用此方法。

---
