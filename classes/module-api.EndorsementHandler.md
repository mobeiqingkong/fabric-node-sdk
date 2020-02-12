# api.EndorsementHandler

## [api](https://hyperledger.github.io/fabric-sdk-node/release-1.4/module-api.html). EndorsementHandler

背书的基类

#### new EndorsementHandler()

### Methods

#### &lt;static&gt; create()

通道将调用此静态方法来创建此处理程序的实例。该处理程序正在使用的channel对象将被传递。

#### endorse(params)

此方法将处理请求对象以计算目标peer。确定目标之后，将背书交易发送给所有目标的通道。将对结果进行分析，以查看是否已收到足够的完整背书。

- 参数

| 名称   | 类型                         | 描述                                                         |
| ------ | ---------------------------- | ------------------------------------------------------------ |
| params | EndorsementHandlerParameters | EndorsementHandlerParameters包含足够的信息来确定目标，并包含一个[ChaincodeInvokeRequest](https://hyperledger.github.io/fabric-sdk-node/release-1.4/global.html#ChaincodeInvokeRequest)，要使用包含的通道通过[Channel](https://hyperledger.github.io/fabric-sdk-node/release-1.4/Channel.html) 的 sendTransactionProposal'方法进行发送。 |

返回结果

- [ProposalResponseObject](https://hyperledger.github.io/fabric-sdk-node/release-1.4/global.html#ProposalResponseObject)的Promise，与直接调用[Channel](https://hyperledger.github.io/fabric-sdk-node/release-1.4/Channel.html)'sendTransactionProposal'方法的结果相同。

  - 类型

    boolean

#### initialize()

初始化通道后，通道将调用此方法。