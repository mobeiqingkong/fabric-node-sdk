# fabric-network.Gateway

## [fabric-network](https://hyperledger.github.io/fabric-sdk-node/release-1.4/module-fabric-network.html)~ Gateway

网关peer为应用程序访问 fabric 网络提供连接点。它使用默认构造函数实例化。然后，可以通过传递 CCP 定义或现有的 [Client](https://hyperledger.github.io/fabric-sdk-node/release-1.4/Client.html) 对象，使用 connect 方法将其连接到 fabric 网络。连接后，它便可以使用 getNetwork 方法访问各个网络实例(通道)，该方法又可以访问安装在网络上的智能合约并将事务提交到分类账。

#### new Gateway()

### Methods

#### &lt;async&gt; connect(config, options)

使用连接配置文件或预构建的客户端实例连接到网关。

- 参数

| 名称    | 类型                                                                                                                                                         | 描述                                                                                                                      |
| ------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------ | ------------------------------------------------------------------------------------------------------------------------- |
| config  | string&#124; object &#124; [Client](https://hyperledger.github.io/fabric-sdk-node/release-1.4/Client.html)                                                   | 该网关的配置可以是:<br>完全限定的通用连接配置文件路径(String)<br>通用连接配置文件 JSON(对象)<br>预先配置的客户端实例 |
| options | [module:fabric-network.Gateway~GatewayOptions](https://hyperledger.github.io/fabric-sdk-node/release-1.4/module-fabric-network.Gateway.html#~GatewayOptions) | 创建此网关实例的特定选项                                                                                                  |

##### 示例

```javascript
const gateway = new Gateway();
const wallet = new FileSystemWallet("./WALLETS/wallet");
const ccpFile = fs.readFileSync("./network.json");
const ccp = JSON.parse(ccpFile.toString());
await gateway.connect(ccp, {
  identity: "admin",
  wallet: wallet
});
```

#### disconnect()

清理并断开此网关连接，以准备将其丢弃并回收垃圾

#### getClient()

获取基础的 Client 对象实例

返回结果

- 基础客户端实例。

  - 类型

    [Client](https://hyperledger.github.io/fabric-sdk-node/release-1.4/Client.html)

#### getCurrentIdentity()

获取当前身份

返回结果

- 网关使用的当前身份。

  - 类型

    [User](https://hyperledger.github.io/fabric-sdk-node/release-1.4/User.html)

#### &lt;async&gt; getNetwork(networkName)

返回代表网络的对象

- 参数

| 名称        | 类型   | 描述                 |
| ----------- | ------ | -------------------- |
| networkName | string | 网络名称(通道名称) |

返回结果

- 网关使用的当前身份。

  - 类型

    [module:fabric-network.Network](https://hyperledger.github.io/fabric-sdk-node/release-1.4/module-fabric-network.Network.html)

#### getOptions()

返回与网关连接关联的选项集

返回结果

- 网关连接选项。

  - 类型

    [module:fabric-network.Gateway~GatewayOptions](https://hyperledger.github.io/fabric-sdk-node/release-1.4/module-fabric-network.Gateway.html#~GatewayOptions)

### Type Definitions

#### AbstractEventHubSelectionStrategy

- 类型

  Object

- 属性

| 名称                       | 类型     | 描述                                     |
| -------------------------- | -------- | ---------------------------------------- |
| getNextPeer                | function | 返回可用 Peer 列表中的下一个 Peer 的函数 |
| updateEventHubAvailability | function | 更新事件中心可用性的功能                 |

#### DefaultEventHandlerOptions

- 类型

  Object

- 属性

| 名称          | 类型                                                                                                                                                                       | 默认值               | 描述                                                                                                                                                                                                              |
| ------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | -------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| commitTimeout | number                                                                                                                                                                     | 300                  | 等待提交通知完成的超时时间(以秒为单位)。                                                                                                                                                                        |
| strategy      | [module:fabric-network.Gateway~TxEventHandlerFactory](https://hyperledger.github.io/fabric-sdk-node/release-1.4/module-fabric-network.Gateway.html#~TxEventHandlerFactory) | MSPID_SCOPE_ALLFORTX | 事件处理策略，用于识别成功的事务提交。空值表示不需要事件处理。默认是 [MSPID_SCOPE_ALLFORTX](https://hyperledger.github.io/fabric-sdk-node/release-1.4/module-fabric-network.html#.DefaultEventHandlerStrategies). |

#### DefaultEventHubSelectionFactory

- 类型

  Object

#### DefaultEventHubSelectionOptions

- 类型

  Object

- 属性

| 名称     | 类型                                                                                                                                                                       | 默认值                  | 描述                                                                   |
| -------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ----------------------- | ---------------------------------------------------------------------- |
| strategy | [module:fabric-network.Gateway~TxEventHandlerFactory](https://hyperledger.github.io/fabric-sdk-node/release-1.4/module-fabric-network.Gateway.html#~TxEventHandlerFactory) | MSPID_SCOPE_ROUND_ROBIN | 可选。可为空。在创建新的侦听器或事件中心断开连接时选择下一个事件中心。 |

#### DefaultQueryHandlerOptions

- 类型

  Object

- 属性

| 名称     | 类型                                                                                                                                                                    | 默认值             | 描述                                                                                                                                                                           |
| -------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| strategy | [ module:fabric-network.Gateway~QueryHandlerFactory](https://hyperledger.github.io/fabric-sdk-node/release-1.4/module-fabric-network.Gateway.html#~QueryHandlerFactory) | MSPID_SCOPE_SINGLE | 用于评估查询的查询处理策略。默认是 [MSPID_SCOPE_SINGLE](https://hyperledger.github.io/fabric-sdk-node/release-1.4/module-fabric-network.html#.DefaultQueryHandlerStrategies)。 |

#### DiscoveryOptions

- 类型

  Object

- 属性

| 名称        | 类型    | 默认值 | 描述                                                                                                       |
| ----------- | ------- | ------ | ---------------------------------------------------------------------------------------------------------- |
| enabled     | boolean | true   | 可选。如果 discovery 应该被使用，则为 true；否则为 true。否则为假。                                        |
| asLocalhost | boolean | true   | 可选。将发现的主机地址转换为“ localhost”。在本地系统上运行由 docker 组成的 fabric 网络时将需要否则应禁用。 |

#### GatewayOptions

- 类型

  Object

- 属性

| 名称                     | 类型                                                                                                                                                                                           | 描述                                     |
| ------------------------ | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------- |
| wallet                   | [module:fabric-network.Wallet](https://hyperledger.github.io/fabric-sdk-node/release-1.4/module-fabric-network.Wallet.html)                                                                    | 与此网关实例一起使用的身份钱包实现。     |
| identity                 | string                                                                                                                                                                                         | 钱包中此网关实例上所有交互的身份。       |
| clientTlsIdentity        | string                                                                                                                                                                                         | 可选。钱包中的身份用作客户端 TLS 身份。  |
| eventHandlerOptions      | [ module:fabric-network.Gateway~DefaultEventHandlerOptions](https://hyperledger.github.io/fabric-sdk-node/release-1.4/module-fabric-network.Gateway.html#~DefaultEventHandlerOptions)          | 可选。内置的默认事件处理程序功能的选项。 |
| queryHandlerOptions      | [ module:fabric-network.Gateway~DefaultQueryHandlerOptions](https://hyperledger.github.io/fabric-sdk-node/release-1.4/module-fabric-network.Gateway.html#~DefaultQueryHandlerOptions)          | 可选。内置默认查询处理程序功能的选项。   |
| discovery                | [module:fabric-network.Gateway~DiscoveryOptions](https://hyperledger.github.io/fabric-sdk-node/release-1.4/module-fabric-network.Gateway.html#~DiscoveryOptions)                               | 可选。发现选项。                         |
| eventHubSelectionOptions | [module:fabric-network.Gateway~DefaultEventHubSelectionOptions](https://hyperledger.github.io/fabric-sdk-node/release-1.4/module-fabric-network.Gateway.html#~DefaultEventHubSelectionOptions) | 可选。事件中心选择选项。                 |
| checkpointer             | [module:fabric-network.Network~CheckpointerFactory](https://hyperledger.github.io/fabric-sdk-node/release-1.4/module-fabric-network.Network.html#~CheckpointerFactory)                         | 可选。事件中心选择选项。                 |

#### QueryHandler

- 类型

  Object

- 属性

| 名称     | 类型     | 描述                                                                                                                                              |
| -------- | -------- | ------------------------------------------------------------------------------------------------------------------------------------------------- |
| evaluate | function | 异步函数，它接受查询并根据[Query](https://hyperledger.github.io/fabric-sdk-node/release-1.4/module-fabric-network.Query.html)评估的结果进行解析。 |

#### QueryHandlerFactory(network)

- 参数

| 名称    | 类型                                                                                                                          | 描述                 |
| ------- | ----------------------------------------------------------------------------------------------------------------------------- | -------------------- |
| network | [module:fabric-network.Network](https://hyperledger.github.io/fabric-sdk-node/release-1.4/module-fabric-network.Network.html) | 正在评估查询的网络。 |

返回结果

- 查询处理程序。

  - 类型

    [module:fabric-network.Gateway~QueryHandler](https://hyperledger.github.io/fabric-sdk-node/release-1.4/module-fabric-network.Gateway.html#~QueryHandler)

#### TxEventHandler

- 类型

  Object

- 属性

| 名称            | 类型     | 描述                                                                                                       |
| --------------- | -------- | ---------------------------------------------------------------------------------------------------------- |
| startListening  | function | 异步函数，用于解析处理程序何时开始侦听事务提交事件。在交易建议被接受之后并且在将交易提交给orderer之前调用。 |
| waitForEvents   | function | 当接收到适当的事务提交事件时解析(或拒绝)的异步功能。在将交易提交给orderer后调用。                         |
| cancelListening | function | 取消收听。如果无法将交易提交给orderer，则调用。                                                             |

#### TxEventHandlerFactory(transaction, network)

- 参数

| 名称        | 类型                                                                                                                           | 描述                           |
| ----------- | ------------------------------------------------------------------------------------------------------------------------------ | ------------------------------ |
| transaction | [Transaction](https://hyperledger.github.io/fabric-sdk-node/release-1.4/global.html#Transaction)                               | 处理程序应侦听提交事件的事务。 |
| network     | [ module:fabric-network.Network](https://hyperledger.github.io/fabric-sdk-node/release-1.4/module-fabric-network.Network.html) | 在其上提交此事务的网络。       |

返回结果

- 事务事件处理程序。

  - 类型

    [module:fabric-network.Gateway~TxEventHandler](https://hyperledger.github.io/fabric-sdk-node/release-1.4/module-fabric-network.Gateway.html#~TxEventHandler)

---
