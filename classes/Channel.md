# Channel

## Channel

Channel 为一组参与的组织提供了数据隔离。

Channel 对象捕获在通道上下文中与 fabric 后端交互所需的设置。这些设置包括参与的组织列表，成员资格服务提供商(MSP)实例，背书 peer 列表和一个 orderer。

客户端应用程序可以使用 Channel 对象与 orderer 创建新通道，更新现有的通道，向 peer 节点发送各种 Channel 感知请求，例如调用链码来处理交易或查询。

Channel 对象还负责验证交易提议响应中的背书签名。在使用 peer 和 orderer 列表配置 Channel 对象之后，必须对其进行初始化。初始化将获取配置块请求发送到主 orderer ，以检索该通道的配置设置。

#### new Channel(name, clientContext)

返回该类的新实例。仅客户端调用。要在 fabric 中创建新通道，请调用[createChannel()](https://hyperledger.github.io/fabric-sdk-node/release-1.4/Client.html#createChannel)。

- 参数:

| 名称          | 类型                                                                            | 描述                                                                                                                                                                                                                                      |
| ------------- | ------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| name          | string                                                                          | 用于标识通道的名称。这个值在 fabric 里做感知请求时作为通道标识使用，例如调用链码以背书交易。通道的命名由订购服务强制执行，并且在 fabric 后端中必须唯一。 Fabric 网络中的通道名称受配置设置 channel-name-regx-checker 中显示的模式的约束。 |
| clientContext | [Client](https://hyperledger.github.io/fabric-sdk-node/release-1.4/Client.html) | 客户端实例，提供操作环境，例如签名身份                                                                                                                                                                                                    |

### Methods

#### &lt;async, static&gt; sendSignedProposal(request, timeout)

发送签名的交易提议给 peer

- 参数

| 名称    | 类型                                                                                                   | 描述                                                |
| :------ | :----------------------------------------------------------------------------------------------------- | :-------------------------------------------------- |
| request | [SignedProposal](https://hyperledger.github.io/fabric-sdk-node/release-1.4/global.html#SignedProposal) | 签署的背书交易提案，该签署的提议将直接发送给 peer。 |
| timeout | number                                                                                                 | 在 sendSignedProposal 上传递的超时设置              |

#### &lt;async&gt; \_discover(request)

向已知 peer 节点发送请求以发现有关 fabric 网络的信息。

- 参数

| 名称    | 类型             | 描述 |
| :------ | :--------------- | :--- |
| request | DiscoveryRequest |      |

返回结果

- 发现服务的结果

  - 类型

    DiscoveryResponse

#### addOrderer(orderer, replace)

将 orderer 对象添加到 channel 对象，仅客户端操作。一个应用程序可以向 channel 对象添加多个 orderer 对象，但是 SDK 仅使用列表中的第一个 orderer 将广播消息发送到 orderer 后端。

- 参数

| 名称    | 类型                                                                              | 描述                                       |
| :------ | :-------------------------------------------------------------------------------- | :----------------------------------------- |
| orderer | [Orderer](https://hyperledger.github.io/fabric-sdk-node/release-1.4/Orderer.html) | Orderer 类的实例。                         |
| replace | boolean                                                                           | 如果存在同名 orderer，请替换为该 orderer。 |

#### addPeer(peer, mspid, roles, replace)

将 peer 对象添加到 channel 对象。可以选择使用 peer 对象列表配置 channel 对象，该对象将在调用某些方法(例如 [sendInstantiateProposal()](https://hyperledger.github.io/fabric-sdk-node/release-1.4/Channel.html#sendInstantiateProposal), [sendUpgradeProposal()](https://hyperledger.github.io/fabric-sdk-node/release-1.4/Channel.html#sendUpgradeProposal), [sendTransactionProposal](https://hyperledger.github.io/fabric-sdk-node/release-1.4/Channel.html#sendTransactionProposal))时使用。

- 参数

| 名称    | 类型                                                                                                       | 描述                                                                         |
| :------ | :--------------------------------------------------------------------------------------------------------- | :--------------------------------------------------------------------------- |
| peer    | [Peer](https://hyperledger.github.io/fabric-sdk-node/release-1.4/Peer.html)                                | 已使用 URL 和其他 gRPC 选项(如 TLS 凭据和请求超时)初始化的 Peer 类的实例。 |
| mspid   | string                                                                                                     | 该 peer 所属组织的 mpsid。                                                   |
| roles   | [ChannelPeerRoles](https://hyperledger.github.io/fabric-sdk-node/release-1.4/global.html#ChannelPeerRoles) | 可选的。此 peer 在此通道上扮演的角色。未定义的角色将默认为 true              |
| replace | boolean                                                                                                    | 如果 peer 存在同名，请替换为该 peer。                                        |

#### close()

关闭所有已分配的 peer 和 orderer 的服务连接

#### compareProposalResponseResults(The)

检查一组提议的实用程序方法，以检查它们是否包含相同的背书结果写集。这将验证背书的 peer 都同意链码执行的结果。

- 参数

| 名称 | 类型                                                                                                                     | 描述                     |
| :--- | :----------------------------------------------------------------------------------------------------------------------- | :----------------------- |
| The  | Array.&lt;[ProposalResponse](https://hyperledger.github.io/fabric-sdk-node/release-1.4/global.html#ProposalResponse)&gt; | 所有背书 peer 的提案回复 |

返回结果

- 当所有提议比较相等时为 true，否则为 false。

  - 类型

    boolean

#### generateUnsignedProposal(request, mspId, certificate, admin)

生成事务的背书提议字节。当前[sendTransactionProposal](https://hyperledger.github.io/fabric-sdk-node/release-1.4/Channel.html#sendTransactionProposal)使用 SDK 上下文中的用户身份(包含用户的私钥)对事务进行签名。此方法旨在在 SDK 端构建提议字节，并且用户可以使用其私钥对该提议进行签名，并通过[sendSignedProposal]将已签名的提议发送给 peer，因此在 SDK 端将不需要用户的私钥。

- 参数

| 名称        | 类型                                                                                                         | 描述                   |
| :---------- | :----------------------------------------------------------------------------------------------------------- | :--------------------- |
| request     | [ProposalRequest](https://hyperledger.github.io/fabric-sdk-node/release-1.4/global.html#ProposalRequest)&gt; | 链码调用请求           |
| mspId       | string                                                                                                       | 此身份的 mspId         |
| certificate | string                                                                                                       | PEM 编码证书           |
| admin       | boolean                                                                                                      | 此事务是否由管理员调用 |

返回结果

- 类型

  Proposal

#### generateUnsignedTransaction(request)

生成交易的提交提议

- 参数

| 名称    | 类型                                                                                                           | 描述 |
| :------ | :------------------------------------------------------------------------------------------------------------- | :--- |
| request | [TransactionRequest](https://hyperledger.github.io/fabric-sdk-node/release-1.4/global.html#TransactionRequest) |      |

#### &lt;async&gt;getChannelConfig(target, timeout)

向 peer 询问此通道的当前(最新)配置块。

- 参数

| 名称    | 类型                                                                                    | 描述                                                                                                                                                                                        |
| :------ | :-------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| target  | string&#124;[Peer](https://hyperledger.github.io/fabric-sdk-node/release-1.4/Peer.html) | 可选的。用于发出请求的 peer。                                                                                                                                                               |
| timeout | Number                                                                                  | 可选的。一个数字，表示等待响应之前等待超时(以毫秒为单位)的毫秒数。这将覆盖[Peer](https://hyperledger.github.io/fabric-sdk-node/release-1.4/Peer.html)实例的默认超时和配置设置中的全局超时 |

返回结果

- 包含配置项的[ConfigEnvelope](https://hyperledger.github.io/fabric-sdk-node/release-1.4/global.html#ConfigEnvelope)对象的 Promise。

  - 类型

    Promise

#### &lt;async&gt;getChannelConfigFromOrderer()

向 orderer 询问该通道的当前(最新)配置块。该方法类似于 [getGenesisBlock()](https://hyperledger.github.io/fabric-sdk-node/release-1.4/Channel.html#getGenesisBlock), 不同之处在于它不是获取块编号 0，而是获取包含通道配置的最新块，并且仅返回解码后的[ConfigEnvelope](https://hyperledger.github.io/fabric-sdk-node/release-1.4/global.html#ConfigEnvelope)。

返回结果

- 包含配置项的[ConfigEnvelope](https://hyperledger.github.io/fabric-sdk-node/release-1.4/global.html#ConfigEnvelope)对象的 Promise。

  - 类型

    Promise

#### getChannelEventHub(name)

返回[ChannelEventHub](https://hyperledger.github.io/fabric-sdk-node/release-1.4/ChannelEventHub.html)对象。ChannelEventHub 对象封装了 peer 节点上事件流的属性，peer 节点通过 ChannelEventHub 发布通道分类帐中提交的 block 的通知。如果不存在，则此方法将创建一个新的 ChannelEventHub。

- 参数

| 名称 | 类型   | 描述                                                                                                                                                                        |
| :--- | :----- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| name | string | 与此通道事件中心关联的 peer 名称。使用[Peer#getName](https://hyperledger.github.io/fabric-sdk-node/release-1.4/Peer.html#getName)方法获取已添加到此通道的 peer 实例的名称。 |

返回结果

- 与 peer 关联的 ChannelEventHub。

  - 类型

    [ChannelEventHub](https://hyperledger.github.io/fabric-sdk-node/release-1.4/ChannelEventHub.html)

#### getChannelEventHubsForOrg(mspid)

根据组织中此通道中定义的 peer 返回 ChannelEventHub 的列表。

- 参数

| 名称  | 类型   | 描述               |
| :---- | :----- | ------------------ |
| mspid | string | 可选。组织的 mspid |

返回结果

- ChannelEventHub 实例的数组。

  - 类型

    Array.&lt;[ChannelEventHub](https://hyperledger.github.io/fabric-sdk-node/release-1.4/ChannelEventHub.html)&gt;

#### getChannelPeer(name)

见[getPeer](https://hyperledger.github.io/fabric-sdk-node/release-1.4/Channel.html#getPeer)

- 参数

| 名称 | 类型   | 描述        |
| :--- | :----- | ----------- |
| name | string | peer 的名称 |

返回结果

- ChannelPeer 实例。

  - 类型

    [ChannelPeer](https://hyperledger.github.io/fabric-sdk-node/release-1.4/ChannelPeer.html)

#### getChannelPeers()

见[getPeers](https://hyperledger.github.io/fabric-sdk-node/release-1.4/Channel.html#getPeers)

返回结果

- 通道上通道 peer 数组。

  - 类型

    Array.&lt;[ChannelPeer](https://hyperledger.github.io/fabric-sdk-node/release-1.4/ChannelPeer.html)&gt;

#### &lt;async&gt; getDiscoveryResults(endorsement_hints)

返回发现结果。仅当此通道已初始化时，发现结果才可用。如果结果太旧，则会刷新。

- 参数

| 名称              | 类型                                                                                                    | 描述                       |
| :---------------- | :------------------------------------------------------------------------------------------------------ | -------------------------- |
| endorsement_hints | Array.&lt;[ChannelPeer](https://hyperledger.github.io/fabric-sdk-node/release-1.4/ChannelPeer.html)&gt; | 指示发现如何计算背书计划。 |

返回结果

- 类型

  Promise.&lt;[DiscoveryResults](https://hyperledger.github.io/fabric-sdk-node/release-1.4/global.html#DiscoveryResults)&gt;

#### &lt;async&gt; getEndorsementPlan(endorsement_hint)

根据 [DiscoveryChaincodeInterest](https://hyperledger.github.io/fabric-sdk-node/release-1.4/global.html#DiscoveryChaincodeInterest)返回单个背书计划。

- 参数

| 名称             | 类型                                                                                                                           | 描述                                   |
| :--------------- | :----------------------------------------------------------------------------------------------------------------------------- | -------------------------------------- |
| endorsement_hint | [DiscoveryChaincodeInterest](https://hyperledger.github.io/fabric-sdk-node/release-1.4/global.html#DiscoveryChaincodeInterest) | 发现服务如何计算背书计划的链码和集合。 |

返回结果

- 根据提供的提示进行的背书计划。

  - 类型

    [DiscoveryResultEndorsementPlan](https://hyperledger.github.io/fabric-sdk-node/release-1.4/global.html#DiscoveryResultEndorsementPlan)

#### getGenesisBlock(request)

通道的第一个块称为“创世块”。该块捕获了初始通道配置。为了使 peer 点加入通道，必须提供创世块。必须在调用[joinChannel()](https://hyperledger.github.io/fabric-sdk-node/release-1.4/Channel.html#joinChannel)之前调用此方法。

- 参数

| 名称    | 类型                                                                                                   | 描述               |
| :------ | :----------------------------------------------------------------------------------------------------- | ------------------ |
| request | [OrdererRequest](https://hyperledger.github.io/fabric-sdk-node/release-1.4/global.html#OrdererRequest) | 可选。交易 ID 对象 |

返回结果

- 对编码的 protobuf“块”的 Promise

  - 类型

    Promise

#### getMSPManager()

获取此通道的 MSP 管理器

返回结果

- 类型

  [MSPManager](https://hyperledger.github.io/fabric-sdk-node/release-1.4/MSPManager.html)

#### getName()

获取通道名称

返回结果

- 通道的名称

  - 类型

    string

#### getOrderer(name)

如果分配给此通道，则此方法将返回[Orderer](https://hyperledger.github.io/fabric-sdk-node/release-1.4/Orderer.html)实例。如果在创建过程中选项中未提供名称，则可以通过 url 引用由[Client#newOrderer](https://hyperledger.github.io/fabric-sdk-node/release-1.4/Client.html#newOrderer)方法创建并添加到此通道的 peer。

- 参数

| 名称 | 类型   | 描述                 |
| :--- | :----- | -------------------- |
| name | string | orderer 的名称或网址 |

返回结果

- orderer 实例

  - 类型

    [Orderer](https://hyperledger.github.io/fabric-sdk-node/release-1.4/Orderer.html)

#### getOrderers()

返回此通道对象的 orderer。

返回结果

- channel 对象中的orderer列表

  - 类型

    Array.&lt;[Orderer](https://hyperledger.github.io/fabric-sdk-node/release-1.4/Orderer.html)&gt;

#### getOrganizations()

从此通道的 MSP 获取组织标识符

返回结果

- 代表通道参与组织的 OrganizationIdentifier 对象数组

  - 类型

    Array.&lt;[OrganizationIdentifier](https://hyperledger.github.io/fabric-sdk-node/release-1.4/global.html#OrganizationIdentifier)&gt;

#### getPeer(name)

如果分配给此通道，则此方法将返回[ChannelPeer](https://hyperledger.github.io/fabric-sdk-node/release-1.4/ChannelPeer.html)实例。如果在创建过程中未在选项中提供名称，则由[Client#newPeer](https://hyperledger.github.io/fabric-sdk-node/release-1.4/Client.html#newPeer) 方法创建并随后添加到此通道的 peer 可以由 url 引用。[ChannelPeer](https://hyperledger.github.io/fabric-sdk-node/release-1.4/ChannelPeer.html)提供对 [Peer](https://hyperledger.github.io/fabric-sdk-node/release-1.4/Peer.html)，[channel event hub objects](https://hyperledger.github.io/fabric-sdk-node/release-1.4/ChannelEventHub.html)对象的引用，以及在此通道上如何使用此 Peer 的属性。

- 参数

| 名称 | 类型   | 描述        |
| :--- | :----- | ----------- |
| name | string | peer 的名称 |

返回结果

- ChannelPeer 实例

  - 类型

    [ChannelPeer](https://hyperledger.github.io/fabric-sdk-node/release-1.4/ChannelPeer.html)

#### getPeers()

返回分配给此通道实例的[ChannelPeer](https://hyperledger.github.io/fabric-sdk-node/release-1.4/ChannelPeer.html) 的列表。 [ChannelPeer](https://hyperledger.github.io/fabric-sdk-node/release-1.4/ChannelPeer.html) 提供对 peer 和通道事件中心的引用，以及在此通道上如何使用此 peer。

返回结果

- 通道上通道 peer 列表

  - 类型

    Array.&lt;[ChannelPeer](https://hyperledger.github.io/fabric-sdk-node/release-1.4/ChannelPeer.html)&gt;

#### getPeersForOrg(mspid)

返回在此通道中在命名组织中定义的 peer 的列表。

- 参数

| 名称  | 类型   | 描述           |
| :---- | :----- | -------------- |
| mspid | string | 可选。组织名称 |

返回结果

- Peer 实例数组

  - 类型

    Array.&lt;[Peer](https://hyperledger.github.io/fabric-sdk-node/release-1.4/Peer.html)&gt;

#### &lt;async&gt; initialize(request)

用成员资格服务提供程序(MSP)初始化 channel 对象。通道的 MSP 对于为应用程序提供验证证书和验证从 fabric 网后端接收到的消息中的签名的能力至关重要。例如，在调用[sendTransactionProposal()](https://hyperledger.github.io/fabric-sdk-node/release-1.4/Channel.html#sendTransactionProposal)之后，应用程序可以验证投标响应的背书中的签名，以确保它们未被篡改。

如果未传入“ config”参数，则此方法从 orderer 那里检索配置。可以选择传入一个配置以初始化此通道，而无需调用orderer。

使用发现时，此方法还将自动加载代表 fabric 网络的 orderer 和 peer 实体。该应用程序必须提供一个运行 fabric 发现服务的 peer。

- 参数

| 名称    | 类型                                                                                                         | 描述                                                                                                                   |
| :------ | :----------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------- |
| request | [InitializeRequest](https://hyperledger.github.io/fabric-sdk-node/release-1.4/global.html#InitializeRequest) | 可选。一个[InitializeRequest](https://hyperledger.github.io/fabric-sdk-node/release-1.4/global.html#InitializeRequest) |

返回结果

- 操作完成后将解决的 Promise

  - 类型

    Promise

#### joinChannel(request, timeout)

为了使 peer 节点成为通道的一部分，必须将其发送给创世块，详细说明见[这里](https://hyperledger.github.io/fabric-sdk-node/release-1.4/Channel.html#getGenesisBlock)。此方法将加入通道提议发送给一个或多个背书可 peer。

- 参数

| 名称    | 类型                                                                                                           | 描述                                                                                                                                                                                  |
| :------ | :------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| request | [JoinChannelRequest](https://hyperledger.github.io/fabric-sdk-node/release-1.4/global.html#JoinChannelRequest) |                                                                                                                                                                                       |
| timeout | Number                                                                                                         | 一个数字，表示等待响应之前等待超时(以毫秒为单位)的毫秒数。这将覆盖[Peer](https://hyperledger.github.io/fabric-sdk-node/release-1.4/Peer.html)实例的默认超时和配置设置中的全局超时。 |

返回结果

- 来自目标 Peer 的一系列[ProposalResponse](https://hyperledger.github.io/fabric-sdk-node/release-1.4/global.html#ProposalResponse)的 Promise

  - 类型

    Promise

#### newChannelEventHub(peer)

返回一个 [ChannelEventHub](https://hyperledger.github.io/fabric-sdk-node/release-1.4/ChannelEventHub.html) 对象。事件中心对象封装了 peer 节点上事件流的属性，peer节点通过该事件发布对通道分类帐中提交的块的通知。此方法将创建一个新的 ChannelEventHub 而不保存引用。使用{getChannelEventHub}重用 ChannelEventHub。

- 参数

| 名称 | 类型                                                                                    | 描述                                                                                                    |
| :--- | :-------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------- |
| peer | [Peer](https://hyperledger.github.io/fabric-sdk-node/release-1.4/Peer.html)&#124;string | 可选。peer 实例或者已分配给该通道的 peer 名称。如果未提供，则 ChannelEventHub 必须与“target”peer 连接。 |

返回结果

- ChannelEventHub 实例

  - 类型

    [ChannelEventHub](https://hyperledger.github.io/fabric-sdk-node/release-1.4/ChannelEventHub.html)

#### &lt;async&gt; queryBlock(blockNumber, target, useAdmin, skipDecode)

通过块号查询目标 peer 的分类帐。

- 参数

| 名称        | 类型                                                                        | 描述                                                                                        |
| :---------- | :-------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------- |
| blockNumber | number                                                                      | 有问题的块的编号。                                                                          |
| target      | [Peer](https://hyperledger.github.io/fabric-sdk-node/release-1.4/Peer.html) | 可选。向其发送查询的 peer。如果未传递目标，则查询将发送到添加到 channel 对象的第一个 peer。 |
| useAdmin    | boolean                                                                     | 可选。指示在向 peer 发出此呼叫时应使用管理员凭据。                                          |
| skipDecode  | boolean                                                                     | 可选。如果为 true，则此函数返回一个编码块。                                                 |

返回结果

- 账本中的 blockNumber 插槽处的[Block](https://hyperledger.github.io/fabric-sdk-node/release-1.4/global.html#Block)Promise，已完全解码为一个对象。

  - 类型

    Promise

#### &lt;async&gt; queryBlockByHash(blockHash, target, useAdmin, skipDecode)

查询目标 peer 的分类账，以逐块进行哈希处理。

- 参数

| 名称       | 类型                                                                        | 描述                                                                                        |
| :--------- | :-------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------- |
| blockHash  | Array.&lt;byte&gt;                                                          | 有关的区块。                                                                                |
| target     | [Peer](https://hyperledger.github.io/fabric-sdk-node/release-1.4/Peer.html) | 可选。向其发送查询的 peer。如果未传递目标，则查询将发送到添加到 channel 对象的第一个 peer。 |
| useAdmin   | boolean                                                                     | 可选。指示在向 peer 发出此呼叫时应使用管理员凭据。                                          |
| skipDecode | boolean                                                                     | 可选。如果为 true，则此函数返回一个编码块。                                                 |

返回结果

- 与哈希匹配的[Block](https://hyperledger.github.io/fabric-sdk-node/release-1.4/global.html#Block) Promise，完全解码为一个对象。

  - 类型

    Promise

#### &lt;async&gt; queryBlockByTxID(tx_id, target, useAdmin, skipDecode)

在目标 peer 的分类账中查询 Block TransactionID。

- 参数

| 名称       | 类型                                                                        | 描述                                                                                        |
| :--------- | :-------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------- |
| tx_id      | string                                                                      | 相关区块的 TransactionID。                                                                  |
| target     | [Peer](https://hyperledger.github.io/fabric-sdk-node/release-1.4/Peer.html) | 可选。向其发送查询的 peer。如果未传递目标，则查询将发送到添加到 channel 对象的第一个 peer。 |
| useAdmin   | boolean                                                                     | 可选。指示在向 peer 发出此呼叫时应使用管理员凭据。                                          |
| skipDecode | boolean                                                                     | 可选。如果为 true，则此函数返回一个编码块。                                                 |

返回结果

- 与 tx_id 匹配的[Block](https://hyperledger.github.io/fabric-sdk-node/release-1.4/global.html#Block) Promise，完全解码为一个对象。

  - 类型

    Promise

#### &lt;async&gt; queryByChaincode(request, useAdmin)

将提议发送给将由链码处理的一个或多个背书 peer。背书 peer 调用链码处理事务与调用链码进行查询没有什么不同。所有请求都将呈现给目标链码的“Invoke”方法，必须执行该方法才能从参数中了解这是一个查询请求。链码还必须以字节数组格式返回结果，并且调用方将必须能够解码这些结果。如果请求包含 txId 属性，则将使用该事务 ID，并将应用其管理特权。在这种情况下，此函数的 useAdmin 参数将被忽略。

- 参数

| 名称     | 类型                                                                                                                 | 描述                                               |
| :------- | :------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------- |
| request  | [ChaincodeQueryRequest](https://hyperledger.github.io/fabric-sdk-node/release-1.4/global.html#ChaincodeQueryRequest) |                                                    |
| useAdmin | boolean                                                                                                              | 可选。指示在向 peer 发出此呼叫时应使用管理员凭据。 |

返回结果

- 在所有背书 peer 上从链码返回的字节数组结果的 Promise

  - 类型

    Promise

- 示例

  获取链码返回的查询结果列表

```javascript
const responsePayloads = await channel.queryByChaincode(request);
for (let i = 0; i < responsePayloads.length; i++) {
  console.log(
    util.format(
      "Query result from peer [%s]: %s",
      i,
      responsePayloads[i].toString("utf8")
    )
  );
}
```

#### &lt;async&gt; queryCollectionsConfig(options, useAdmin)

查询与链码关联的集合定义。

- 参数

| 名称     | 类型                                                                                                                   | 描述                                               |
| :------- | :--------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------- |
| options  | [CollectionQueryOptions](https://hyperledger.github.io/fabric-sdk-node/release-1.4/global.html#CollectionQueryOptions) |                                                    |
| useAdmin | boolean                                                                                                                | 可选。指示在向 peer 发出此呼叫时应使用管理员凭据。 |

返回结果

- 返回一个 [CollectionQueryResponse](https://hyperledger.github.io/fabric-sdk-node/release-1.4/global.html#CollectionQueryResponse)对象数组的 promise。

  - 类型

    Promise.&lt;Array.&lt;[CollectionQueryResponse](https://hyperledger.github.io/fabric-sdk-node/release-1.4/global.html#CollectionQueryResponse)&gt;&gt;

#### &lt;async&gt; queryInfo(target, useAdmin)

查询有关通道状态(高度，已知 peer)的各种有用信息。

- 参数

| 名称     | 类型                                                                        | 描述                                                                                             |
| :------- | :-------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------ |
| target   | [Peer](https://hyperledger.github.io/fabric-sdk-node/release-1.4/Peer.html) | 可选。作为此查询目标的 peer。如果未传递目标，则查询将使用添加到 channel 对象的第一个 peer 对象。 |
| useAdmin | boolean                                                                     | 可选。指示在向 peer 发出此呼叫时应使用管理员凭据。                                               |

返回结果

- 具有区块链高度，当前区块哈希和先前区块哈希的 [BlockchainInfo](https://hyperledger.github.io/fabric-sdk-node/release-1.4/global.html#BlockchainInfo)对象的 promise。

  - 类型

    Promise

#### &lt;async&gt; queryInstantiatedChaincodes(target, useAdmin)

在目标 peer 的分类帐中查询此通道上的实例化链码。

- 参数

| 名称     | 类型                                                                        | 描述                                                                                                                           |
| :------- | :-------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------ |
| target   | [Peer](https://hyperledger.github.io/fabric-sdk-node/release-1.4/Peer.html) | 可选。作为此查询目标的 peer。如果未传递目标，则查询将使用添加到 channel 对象的第一个 peer 对象。                               |
| useAdmin | boolean                                                                     | 可选。指示在向 peer 发出此呼叫时应使用管理员凭据。必须通过通用连接配置文件或使用“ setAdminSigningIdentity”方法来加载管理标识。 |

返回结果

- 完全解码的 [ChaincodeQueryResponse](https://hyperledger.github.io/fabric-sdk-node/release-1.4/global.html#ChaincodeQueryResponse) 对象的 Promise。

  - 类型

    Promise

#### &lt;async&gt; queryTransaction(tx_id, target, useAdmin, skipDecode)

根据 ID 查询目标 peer 上的分类帐。

- 参数

| 名称       | 类型                                                                        | 描述                                                                                             |
| :--------- | :-------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------ |
| tx_id      | string                                                                      | 交易编号                                                                                         |
| target     | [Peer](https://hyperledger.github.io/fabric-sdk-node/release-1.4/Peer.html) | 可选。作为此查询目标的 peer。如果未传递目标，则查询将使用添加到 channel 对象的第一个 peer 对象。 |
| useAdmin   | boolean                                                                     | 可选。指示在向 peer 发出此呼叫时应使用管理员凭据。                                               |
| skipDecode | boolean                                                                     | 可选。如果为 true，则此函数返回编码的事务。                                                      |

返回结果

- 完全解码的 [ProcessedTransaction](https://hyperledger.github.io/fabric-sdk-node/release-1.4/global.html#ProcessedTransaction) 对象的 Promise。

  - 类型

    Promise

#### &lt;async&gt; refresh()

刷新通道的配置。将使用发现服务从 peer 查询 MSP 配置，peer，orderer 和背书计划。如果未提供“ target”参数，将查询以前用于发现的 peer。

返回结果

- 刷新的结果。

  - 类型

    [DiscoveryResults](https://hyperledger.github.io/fabric-sdk-node/release-1.4/global.html#DiscoveryResults)

#### removeOrderer(orderer)

删除通道对象的 orderer 列表中第一个 orderer 对象，该 orderer 的端点 url 属性与传入的 orderer 的 URL 相匹配。

- 参数

| 名称    | 类型                                                                              | 描述               |
| :------ | :-------------------------------------------------------------------------------- | ------------------ |
| orderer | [Orderer](https://hyperledger.github.io/fabric-sdk-node/release-1.4/Orderer.html) | Orderer 类的实例。 |

#### removePeer(peer)

在 channel 对象的端点 url 属性与传入的 peer 的 url 或名称匹配的 peer 列表中，删除 peer 对象。

- 参数

| 名称 | 类型                                                                        | 描述            |
| :--- | :-------------------------------------------------------------------------- | --------------- |
| peer | [Peer](https://hyperledger.github.io/fabric-sdk-node/release-1.4/Peer.html) | Peer 类的实例。 |

#### sendInstantiateProposal(request, timeout)

将链码实例化提议发送给一个或多个背书 peer。在使用链码之前，必须逐个通道地对其进行实例化。必须先通过调用[client.installChaincode()](https://hyperledger.github.io/fabric-sdk-node/release-1.4/Client.html#installChaincode)将链码安装在预期运行此链码的背书 peer 上。

实例化一个链码是一个完整的交易操作，这意味着必须首先将其作为提议背书，然后将背书发送到 orderer 进行处理以进行订购和验证。当交易最终提交到 peer 的 channel 分类帐时，链码将被视为已激活，并且 peer 准备接受请求以处理交易。

- 参数

| 名称    | 类型                                                                                                                                           | 描述                                                                                                             |
| :------ | :--------------------------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------- |
| request | [ChaincodeInstantiateUpgradeRequest](https://hyperledger.github.io/fabric-sdk-node/release-1.4/global.html#ChaincodeInstantiateUpgradeRequest) |                                                                                                                  |
| timeout | Number                                                                                                                                         | 一个数字，表示等待响应之前等待超时(以毫秒为单位)的毫秒数。这将覆盖 Peer 实例的默认超时和配置设置中的全局超时。 |

返回结果

- [ProposalResponseObject](https://hyperledger.github.io/fabric-sdk-node/release-1.4/global.html#ProposalResponseObject)的 Promise。

  - 类型

    Promise

#### &lt;async&gt; sendSignedProposal(request, timeout)

发送签名的交易提议给 peer

- 参数

| 名称    | 类型                                                                                                   | 描述                                                |
| :------ | :----------------------------------------------------------------------------------------------------- | --------------------------------------------------- |
| request | [SignedProposal](https://hyperledger.github.io/fabric-sdk-node/release-1.4/global.html#SignedProposal) | 签署的背书交易提案，该签署的提案将直接发送给 peer。 |
| timeout | number                                                                                                 | 在 sendSignedProposal 上传递的超时设置              |

#### &lt;async&gt; sendSignedTransaction(request, timeout)

发送已签署的提交交易提案

- 参数

| 名称    | 类型                                                                                                   | 描述                                   |
| :------ | :----------------------------------------------------------------------------------------------------- | -------------------------------------- |
| request | [SignedProposal](https://hyperledger.github.io/fabric-sdk-node/release-1.4/global.html#SignedProposal) | 签署的提交提案                         |
| timeout | number                                                                                                 | 在 sendSignedProposal 上传递的超时设置 |

#### &lt;async&gt; sendTransaction(request, timeout)

将包含交易提议背书的提议响应发送给 orderer 以进行进一步处理。这是 fabric 中事务生命周期的第二阶段。orderer 将在此通道的上下文中对交易进行全局订购，并将生成的块交付给提交的 peer，以根据链码的背书策略进行验证。当提交 peer 成功验证事务时，它将在该块内将事务标记为有效。在验证了一个区块中的所有交易并将其标记为有效或无效(带有[reason code](https://github.com/hyperledger/fabric/blob/v1.0.0/protos/peer/transaction.proto#L125))之后，该区块将被附加(提交(committed))到 peer 的通道分类帐中。

此方法的调用者必须使用背书者返回的提案响应以及发送给背书者的原始提案。这两个对象都包含在 [ProposalResponseObject](https://hyperledger.github.io/fabric-sdk-node/release-1.4/global.html#ProposalResponseObject)中，该对象通过调用以下任何方法返回:

- [sendInstantiateProposal()](https://hyperledger.github.io/fabric-sdk-node/release-1.4/Channel.html#sendInstantiateProposal)
- [sendUpgradeProposal()](https://hyperledger.github.io/fabric-sdk-node/release-1.4/Channel.html#sendUpgradeProposal)
- [sendTransactionProposal()](https://hyperledger.github.io/fabric-sdk-node/release-1.4/Channel.html#sendTransactionProposal)

- 参数

| 名称    | 类型                                                                                                           | 描述                                                                                                                |
| :------ | :------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------- |
| request | [TransactionRequest](https://hyperledger.github.io/fabric-sdk-node/release-1.4/global.html#TransactionRequest) | [TransactionRequest](https://hyperledger.github.io/fabric-sdk-node/release-1.4/global.html#TransactionRequest)      |
| timeout | Number                                                                                                         | 一个数字，表示等待响应之前等待超时(以毫秒为单位)的毫秒数。这将覆盖 Orderer 实例的默认超时和配置设置中的全局超时。 |

返回结果

- orderer 返回的"BroadcastResponse"消息的Promise，其中包含一个用于标准[HTTP response code](https://github.com/hyperledger/fabric/blob/v1.0.0/protos/common/common.proto#L27)的“状态”字段。这将是成功提交交易的 orderer 的确认。

  - 类型

    Promise

#### sendTransactionProposal(request, timeout)

将交易建议发送给一个或多个背书 peer。 [installed](https://hyperledger.github.io/fabric-sdk-node/release-1.4/Client.html#installChaincode)并实例化 Chaincode 后，就可以准备背书建议并参与交易处理了。链码事务处理始于将提案发送到背书 peer，提案将执行目标链码，并决定背书(如果成功执行)或不背书(如果链码返回错误)。

- 参数

| 名称    | 类型                                                                                                                   | 描述                                                                                                                |
| :------ | :--------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------- |
| request | [ChaincodeInvokeRequest](https://hyperledger.github.io/fabric-sdk-node/release-1.4/global.html#ChaincodeInvokeRequest) |                                                                                                                     |
| timeout | Number                                                                                                                 | 一个数字，表示等待响应之前等待超时(以毫秒为单位)的毫秒数。这将覆盖 Orderer 实例的默认超时和配置设置中的全局超时。 |

返回结果

- [ProposalResponseObject](https://hyperledger.github.io/fabric-sdk-node/release-1.4/global.html#ProposalResponseObject) 的 promise。

  - 类型

    Promise

#### sendUpgradeProposal(request, timeout)

向一个或多个背书 peer 发送链码升级提议。升级链码涉及类似于实例化链码的步骤。新的链码必须首先安装在预计将运行该链码的背书 peer 上。

类似于实例化链码，升级链码也是完整的事务操作。

- 参数

| 名称    | 类型                                                                                                                                           | 描述                                                                                                                |
| :------ | :--------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------- |
| request | [ChaincodeInstantiateUpgradeRequest](https://hyperledger.github.io/fabric-sdk-node/release-1.4/global.html#ChaincodeInstantiateUpgradeRequest) |                                                                                                                     |
| timeout | Number                                                                                                                                         | 一个数字，表示等待响应之前等待超时(以毫秒为单位)的毫秒数。这将覆盖 Orderer 实例的默认超时和配置设置中的全局超时。 |

返回结果

- [ProposalResponseObject](https://hyperledger.github.io/fabric-sdk-node/release-1.4/global.html#ProposalResponseObject) 的 promise。

  - 类型

    Promise

#### setMSPManager(msp_manager)

设置此通道的 MSP 管理器。通常不使用此实用程序方法，因为 [initialize()](https://hyperledger.github.io/fabric-sdk-node/release-1.4/Channel.html#initialize) 方法将读取该通道的当前配置，并使用在通道配置中找到的 MSP 重置 MSPManager。

- 参数

| 名称        | 类型                                                                                    | 描述                |
| :---------- | :-------------------------------------------------------------------------------------- | ------------------- |
| msp_manager | [MSPManager](https://hyperledger.github.io/fabric-sdk-node/release-1.4/MSPManager.html) | 此通道的 MSP 管理器 |

#### toString()

返回此 channel 对象的可打印表示形式

#### verifyProposalResponse(proposal_response)

验证单个投标响应的实用方法。它检查以下方面:

- 背书人的身份属于该通道的合法 MSP，可以成功反序列化
- 背书签名可以通过背书人的身份证书成功验证

此方法要求已调用此通道对象的 initialize 方法来加载此通道的 MSP。 MSP 将具有此通道的受信任的根证书。

- 参数

| 名称              | 类型                                                                                                       | 描述                                                                 |
| :---------------- | :--------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------- |
| proposal_response | [ProposalResponse](https://hyperledger.github.io/fabric-sdk-node/release-1.4/global.html#ProposalResponse) | 来自 peer 的背书响应，包括背书人证书和提案签名+背书结果+背书人证书。 |

返回结果

- 布尔值，当标识和签名均有效时为 true，否则为 false。

  - 类型

    boolean

---
