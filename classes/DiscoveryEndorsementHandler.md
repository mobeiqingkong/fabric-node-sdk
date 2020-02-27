# DiscoveryEndorsementHandler

## 说明

这是 [EndorsementHandler](https://hyperledger.github.io/fabric-sdk-node/release-1.4/module-api.EndorsementHandler.html) API 的实现。它将提交要背书的交易到根据服务发现结果生成的目标列表。

### new DiscoveryEndorsementHandler(channel)

constructor

- 参数

| 名称    | 类型                                                                              | 描述               |
| :------ | :-------------------------------------------------------------------------------- | ------------------ |
| channel | [Channel](https://hyperledger.github.io/fabric-sdk-node/release-1.4/Channel.html) | 此处理程序的通道。 |

### 扩展

- [module:api.EndorsementHandler](https://hyperledger.github.io/fabric-sdk-node/release-1.4/module-api.EndorsementHandler.html)

### Methods

#### &lt;static&gt; create(channel)

用于创建背书处理程序实例的工厂方法。

- 参数

| 名称    | 类型                                                                              | 描述                               |
| :------ | :-------------------------------------------------------------------------------- | ---------------------------------- |
| channel | [Channel](https://hyperledger.github.io/fabric-sdk-node/release-1.4/Channel.html) | 该背书处理程序将要服务的通道实例。 |

返回结果

- 处理程序的实例

  - 类型

    [DiscoveryEndorsementHandler](https://hyperledger.github.io/fabric-sdk-node/release-1.4/DiscoveryEndorsementHandler.html)

#### endorse(params)

此方法将处理请求对象以计算目标 peer。确定目标之后，将背书交易发送给所有目标的通道。将对结果进行分析，以查看是否已收到足够的完整背书。

- 参数

| 名称   | 类型                         | 描述                                                                                                                                                                                                                                                                                                                                        |
| :----- | :--------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| params | EndorsementHandlerParameters | EndorsementHandlerParameters 包含足够的信息来确定目标，并包含一个[ChaincodeInvokeRequest](https://hyperledger.github.io/fabric-sdk-node/release-1.4/global.html#ChaincodeInvokeRequest)，要使用包含的 [Channel](https://hyperledger.github.io/fabric-sdk-node/release-1.4/Channel.html) 通过 Channel'sendTransactionProposal'方法进行发送。 |

继承自(Inherited From):

- [module:api.EndorsementHandler#endorse](https://hyperledger.github.io/fabric-sdk-node/release-1.4/module-api.EndorsementHandler.html#endorse)

覆写(Overrides:):

- [module:api.EndorsementHandler#endorse](https://hyperledger.github.io/fabric-sdk-node/release-1.4/module-api.EndorsementHandler.html#endorse)

返回结果

- [ProposalResponseObject](https://hyperledger.github.io/fabric-sdk-node/release-1.4/global.html#ProposalResponseObject)的 Promise，与直接调用 [Channel](https://hyperledger.github.io/fabric-sdk-node/release-1.4/Channel.html) 'sendTransactionProposal'方法的结果相同。

  - 类型

    Promise

#### initialize()

初始化通道后，通道将调用此方法。

继承自(Inherited From):

- [module:api.EndorsementHandler#initialize](https://hyperledger.github.io/fabric-sdk-node/release-1.4/module-api.EndorsementHandler.html#initialize)

覆写(Overrides:):

- [module:api.EndorsementHandler#initialize](https://hyperledger.github.io/fabric-sdk-node/release-1.4/module-api.EndorsementHandler.html#initialize)

---
