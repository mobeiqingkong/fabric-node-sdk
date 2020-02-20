# Client

## Client

客户端实例提供了主要的 API 表面，可以与peer和orderer的网络进行交互。使用 SDK 的应用程序可能需要与多个网络交互，每个网络都通过客户端的单独实例进行。

当前 Client 类设计的一个重要方面是它是有状态的。必须先使用 userContext 配置实例，然后才能使用它与Fabric后端进行对话。userContext 是 User 类的实例，它封装了对请求进行签名的功能。如果在多用户环境中使用 SDK，则建议使用两种推荐的技术来管理经过身份验证的用户和客户端实例。

- 对每个经过身份验证的用户使用专用的客户端实例。为每个通过身份验证的用户创建一个新实例。您可以分别注册每个经过身份验证的用户，以便每个用户获得自己的签名身份。
- 在经过身份验证的用户之间使用共享的客户端实例和公共的签名身份。

重要的是要理解，将具有相同客户端实例的 userContexts 切换为反模式。这是有状态设计的直接结果。JIRA 工作项已打开，以讨论添加对 SDK 的无状态使用的支持:[FAB-4563](https://jira.hyperledger.org/browse/FAB-4563)

客户端还通过 stateStore 支持持久性。状态存储区是一个简单的存储插件，可实现以下[module:api.KeyValueStore](https://hyperledger.github.io/fabric-sdk-node/release-1.4/module-api.KeyValueStore.html) 接口，这有助于 SDK 保存在服务器重新启动/崩溃时使用的关键信息。开箱即用，SDK 将签名身份(User 类的实例)保存在状态存储中。

#### new Client()

### 扩展

- [BaseClient](https://hyperledger.github.io/fabric-sdk-node/release-1.4/BaseClient.html)

### Methods

#### &lt;static&gt; loadFromConfig(loadConfig)

加载公共连接配置文件对象或 JSON 文件并返回 Client 对象。

- 参数

| 名称       | 类型                 | 描述                             |
| :--------- | :------------------- | -------------------------------- |
| loadConfig | object &#124; string | 这可能是配置对象或配置文件的路径 |

返回结果

- 用网络端点初始化的此类的实例。

  - 类型

    [Client](https://hyperledger.github.io/fabric-sdk-node/release-1.4/Client.html)

#### &lt;async&gt; \_setUserFromConfig(opts)

实用程序方法根据传入的用户名和密码以及通用连接配置文件设置的“客户端”部分中的组织来设置用户上下文。

- 参数

| 名称 | 类型   | 描述                                                                                                                              |
| :--- | :----- | --------------------------------------------------------------------------------------------------------------------------------- |
| opts | Object | 包含<br>- username [required] - user 的 username <br>- password [optional] - user 的密码 <br>- caName [optional] - 认证中心的名称 |

#### addConnectionOptions(options)

向此客户端添加一组连接选项。 创建新的 Peer 和 Orderer 时，或者当通道使用发现功能自动在通道上创建peer和orderer时，这些将可合并到应用程序的选项中。 这将是一个方便的地方，可以存储影响该客户端的所有连接的通用 GRPC 设置。 当调用[Client#newPeer](https://hyperledger.github.io/fabric-sdk-node/release-1.4/Client.html#newPeer), [Client#getPeer](https://hyperledger.github.io/fabric-sdk-node/release-1.4/Client.html#getPeer), [Client#newOrderer](https://hyperledger.github.io/fabric-sdk-node/release-1.4/Client.html#newOrderer) 或 [Client#getOrderer](https://hyperledger.github.io/fabric-sdk-node/release-1.4/Client.html#getOrderer) 方法时，此客户端对象构建新的[Peer](https://hyperledger.github.io/fabric-sdk-node/release-1.4/Peer.html) 或[Orderer](https://hyperledger.github.io/fabric-sdk-node/release-1.4/Orderer.html)实例时将使用这些设置。 加载公共连接配置文件时，选项将自动添加，并且客户端部分的“连接”部分带有“选项”属性。 默认连接选项将首先从系统配置的“连接选项”设置中加载。

- 参数

| 名称    | 类型   | 描述                             |
| :------ | :----- | -------------------------------- |
| options | object | 将添加到此客户端实例的连接选项。 |

#### addTlsClientCertAndKey(opts)

将相互的 tls 客户资料添加到一组选项的实用程序方法。

- 参数

| 名称 | 类型   | 描述                                                                |
| :--- | :----- | ------------------------------------------------------------------- |
| opts | object | 选项对象包含将使用相互 TLS clientCert 和 clientKey 更新的连接设置。 |

抛出

- 如果生成 tls 客户端材料失败，将引发错误

#### buildConnectionOptions(options)

将连接选项合并到一组选项中并返回一个新的选项对象的实用程序方法。 客户端的选项和默认连接选项不会覆盖任何具有相同名称的设置，它们只会作为新设置添加到所传入的应用程序选项中。有关此客户端如何合并连接选项的信息，请参阅 [Client#addConnectionOptions](https://hyperledger.github.io/fabric-sdk-node/release-1.4/Client.html#addConnectionOptions) 。

- 参数

| 名称    | 类型   | 描述                                                       |
| :------ | :----- | ---------------------------------------------------------- |
| options | object | 包含应用程序选项的对象，该对象将在此客户端的选项之上合并。 |

返回结果

- 同时包含应用程序选项和此客户端选项的对象。

  - 类型

    object

#### createChannel(request)

呼叫 Orderer 开始建立新通道。 一个通道通常有多个参与组织。 要创建新通道，参与组织之一应调用此方法以将创建请求提交给 orderer 服务。

一旦 orderer 成功创建了通道，下一步就是通过将通道配置发送到每个 Peer 节点，使每个组织的 Peer 节点加入该通道。 该步骤通过调用 joinChannel()方法完成。

- 参数

| 名称    | 类型                                                                                                   | 描述       |
| :------ | :----------------------------------------------------------------------------------------------------- | ---------- |
| request | [ChannelRequest](https://hyperledger.github.io/fabric-sdk-node/release-1.4/global.html#ChannelRequest) | 请求对象。 |

返回结果

- 保证结果对象具有 orderer 接受创建请求的状态。 请注意，这并不是成功创建通道的确认。 客户端应用程序必须轮询 orderer 以发现通道是否已完全创建。

  - 类型

    Promise

#### &lt;async&gt; createUser(opts)

根据私钥和相应的 x509 证书返回具有签名标识的 User 对象。 这允许应用程序使用预先存在的加密材料(私钥和证书)来构造具有签名功能的用户对象，以替代通过 fabric-ca 动态注册用户

请注意，成功创建新用户对象后，会将其设置为客户端实例作为当前 userContext。

- 参数

| 名称 | 类型                                                                                       | 描述               |
| :--- | :----------------------------------------------------------------------------------------- | ------------------ |
| opts | [UserOpts](https://hyperledger.github.io/fabric-sdk-node/release-1.4/global.html#UserOpts) | 有关用户的基本信息 |

返回结果

- 对用户对象的 Promise。

  - 类型

    Promise

#### extractChannelConfig(config_envelope)

362/5000

从[configtxgen tool](http://hyperledger-fabric.readthedocs.io/en/latest/configtxgen.html)生成的'ConfigEnvelope' 对象中提取 protobuf 'ConfigUpdate' 对象。 然后可以使用此类的 signChannelConfig()方法对返回的对象进行签名。 一旦收集了所有签名，就可以在[createChannel()](https://hyperledger.github.io/fabric-sdk-node/release-1.4/Client.html#createChannel) 或[updateChannel()](https://hyperledger.github.io/fabric-sdk-node/release-1.4/Client.html#updateChannel)调用中使用"ConfigUpdate"对象和签名。

- 参数

| 名称            | 类型               | 描述                          |
| :-------------- | :----------------- | ----------------------------- |
| config_envelope | Array.&lt;byte&gt; | ConfigEnvelope 协议的编码字节 |

返回结果

- ConfigUpdate protobuf 的编码字节，准备进行签名。

  - 类型

    Array.&lt;byte&gt;

#### getCertificateAuthority(name)

返回由当前加载的公共连接配置文件和客户端配置中的设置定义的 CertificateAuthority 实现。 必须加载一个公共连接配置文件，此 get 方法才能返回证书颁发机构。 必须将加密套件分配给此客户端实例。 运行'initCredentialStores'方法将建立存储并创建通用连接配置文件中定义的加密套件。

- 参数

| 名称 | 类型   | 描述                                                    |
| :--- | :----- | ------------------------------------------------------- |
| name | string | 可选-在已加载的连接配置文件中定义的证书颁发机构的名称。 |

返回结果

- 类型

  [CertificateAuthority](https://hyperledger.github.io/fabric-sdk-node/release-1.4/CertificateAuthority.html)

#### getChannel(name, throwError)

从客户端实例获取[Channel](https://hyperledger.github.io/fabric-sdk-node/release-1.4/Channel.html)实例。 这是仅内存的查找。 如果加载的公共连接配置文件具有按“名称”命名的通道，则将创建一个新的通道实例，并使用在公共连接配置文件中定义的[Orderer](https://hyperledger.github.io/fabric-sdk-node/release-1.4/Orderer.html) 对象和[Peer](https://hyperledger.github.io/fabric-sdk-node/release-1.4/Peer.html)对象进行填充。

- 参数

| 名称       | 类型    | 描述                                                                       |
| :--------- | :------ | -------------------------------------------------------------------------- |
| name       | string  | 可选的。通道名称。省略时，将返回已加载的公共连接配置文件中定义的第一个通道 |
| throwError | boolean | 默认为 false。指示如果找不到通道，此方法是否将引发错误。默认为 true。      |

返回结果

- channel 实例

  - 类型

    Channel

#### getClientCertHash(create)

获取客户端证书哈希

- 参数

| 名称   | 类型    | 描述                                                               |
| :----- | :------ | ------------------------------------------------------------------ |
| create | boolean | 可选的。如果尚未将客户端证书分配给该客户端，则基于当前用户创建哈希 |

返回结果

- 客户端证书的哈希

  - 类型

    Array.&lt;byte&gt;

#### getClientConfig()

返回公共连接配置文件的"client"部分。

返回结果

- 配置中的客户端部分

  - 类型

    Object

#### getCryptoSuite()

返回此客户端实例使用的 CryptoSuite 对象

继承自(Inherited From):

- [BaseClient#getCryptoSuite](https://hyperledger.github.io/fabric-sdk-node/release-1.4/BaseClient.html#getCryptoSuite)

覆写(Overrides:):

- [BaseClient#getCryptoSuite](https://hyperledger.github.io/fabric-sdk-node/release-1.4/BaseClient.html#getCryptoSuite)

返回结果

- 类型

  [module:api.CryptoSuite](https://hyperledger.github.io/fabric-sdk-node/release-1.4/module-api.CryptoSuite.html)

#### getMspid()

返回客户端的 mspid。 mspid 也用作组织的参考。

返回结果

- 在已加载的公共连接配置文件的客户机部分中定义的组织的 mspid

  - 类型

    string

#### getOrderer(name)

如果 orderer 不存在，则此方法将创建[Orderer](https://hyperledger.github.io/fabric-sdk-node/release-1.4/Orderer.html) ，并按名称保存对实例对象的引用。 仅当存在包含名称为 orderer 的连接概要文件时，此方法才能创建 orderer 的实例。 如果在选项中未提供名称，则可以通过 url 引用由 newOrderer 方法创建的 orderer。

- 参数

| 名称 | 类型   | 描述                 |
| :--- | :----- | -------------------- |
| name | string | orderer 的名称或网址 |

返回结果

- Orderer 实例

  - 类型

    [Orderer](https://hyperledger.github.io/fabric-sdk-node/release-1.4/Orderer.html)

#### getPeer(name)

此方法将创建一个[Peer](https://hyperledger.github.io/fabric-sdk-node/release-1.4/Peer.html)实例。仅当存在包含名称为 Peer 的已加载连接配置文件时，此方法才能创建 Peer 的实例。

- 参数

| 名称 | 类型   | 描述        |
| :--- | :----- | ----------- |
| name | string | Peer 的名称 |

返回结果

- Peer 实例

  - 类型

    [Peer](https://hyperledger.github.io/fabric-sdk-node/release-1.4/Peer.html)

#### getPeersForOrg(mspid)

返回在当前加载的公共连接配置文件中定义的组织的 mspid 的 [Peer](https://hyperledger.github.io/fabric-sdk-node/release-1.4/Peer.html)列表。如果未提供 ID，则将使用当前活动网络配置的“客户端”部分中命名的组织。

- 参数

| 名称  | 类型   | 描述              |
| :---- | :----- | ----------------- |
| mspid | string | 可选-组织的 mspid |

返回结果

- 为此组织定义的 Peer 实例数组

  - 类型

    Array.&lt;[Peer](https://hyperledger.github.io/fabric-sdk-node/release-1.4/Peer.html)&gt;

#### getStateStore()

一种获取此客户端正在使用的状态存储对象的便捷方法。

返回结果

- 在此 Client 上设置的 KeyValueStore 实现对象；如果尚未设置，则为 null。

  - 类型

    [module:api.KeyValueStore](https://hyperledger.github.io/fabric-sdk-node/release-1.4/module-api.KeyValueStore.html)

#### &lt;async&gt; getUserContext(name, checkPersistence)

通过给定名称返回用户。 这可以是同步调用，也可以是异步调用，具体取决于“ checkPersistent”是否正确。 如果为真，则该方法为异步方法并返回 Promise，否则为同步方法。 如上所述，客户端实例可以具有可选的状态存储。 SDK 将注册的用户保存在存储中，该存储可以由应用程序的授权用户访问(身份验证由 SDK 外部的应用程序完成)。 此函数尝试按名称从本地存储中加载用户(通过 KeyValueStore 接口)。 加载的用户对象必须代表具有由受信任的 CA(例如 CA 服务器)签名的有效注册证书的注册用户。

- 参数

| 名称             | 类型    | 描述                                                                                                                                                      |
| :--------------- | :------ | --------------------------------------------------------------------------------------------------------------------------------------------------------- |
| name             | string  | 可选的。如果未指定，则仅返回当前的内存用户上下文对象；如果未设置，则返回 null。如果指定了"name"，则如果在内存中搜索失败，还将尝试从状态存储中加载它。     |
| checkPersistence | boolean | 可选的。如果指定且为真，则该方法返回 Promise 并将尝试通过"name"为请求的用户检查状态存储。如果未指定或为 false，则该方法是同步的，并从内存中返回请求的用户 |

返回结果

- 该名称对应的用户对象的 Promise；如果该用户不存在或未设置状态存储，则为 null。

  - 类型

    Promise.&lt;[User](https://hyperledger.github.io/fabric-sdk-node/release-1.4/User.html)&gt;

#### &lt;async&gt; initCredentialStores()

设置状态和加密套件供此客户端使用。 这要求已加载公共连接配置文件。 将使用公共连接配置文件中的设置以及系统配置来构建存储实例，并在需要时将其分配给此客户端和加密套件。

返回结果

- 建立密钥值存储和加密存储的 Promise。

  - 类型

    Promise

#### &lt;async&gt; installChaincode(request, timeout)

必须将 Chaincode 安装到 Peer 并在通道上实例化，然后才能调用它来处理事务。

链码安装只是将链码源和依赖项上载到同级。 该操作是“与通道无关的”，并且是在 Peer 基础上执行的。 仅允许对等组织的 ADMIN 身份执行此操作。

- 参数

| 名称    | 类型                                                                                                                     | 描述                                                                                                             |
| :------ | :----------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------- |
| request | [ChaincodeInstallRequest](https://hyperledger.github.io/fabric-sdk-node/release-1.4/global.html#ChaincodeInstallRequest) | 请求对象                                                                                                         |
| timeout | Number                                                                                                                   | 一个数字，表示等待响应之前等待超时(以毫秒为单位)的毫秒数。这将覆盖 Peer 实例的默认超时和配置设置中的全局超时。 |

返回结果

- [ProposalResponseObject](https://hyperledger.github.io/fabric-sdk-node/release-1.4/global.html#ProposalResponseObject)的 Promise。

  - 类型

    Promise

#### isDevMode()

确定 Fabric 后端是否以开发模式启动。 在开发模式下，背书的 Peer 将不会尝试启动 docker 实例来运行交易建议所请求的目标链码，而是将调用请求重定向到已向背书的 Peer 注册了自己的链码流程。 这使得在开发链代码期间测试链代码的更改更加容易。

可以将客户端实例设置为 dev 模式，以反映后端的开发模式。 这将导致 SDK 对某些行为进行调整，例如在安装链码期间不将链码包发送给peer。

#### loadFromConfig(config)

加载公共连接配置文件对象或 JSON 文件，并使用配置中的任何值更新此客户端。

- 参数

| 名称   | 类型                 | 描述                             |
| :----- | :------------------- | -------------------------------- |
| config | object &#124; string | 这可能是配置对象或配置文件的路径 |

#### &lt;async&gt; loadUserFromStateStore(name)

通过键值存储中的给定名称恢复 [User](https://hyperledger.github.io/fabric-sdk-node/release-1.4/User.html) 状态(如果找到)。如果找不到，则返回 null。

- 参数

| 名称 | 类型   | 描述        |
| :--- | :----- | ----------- |
| name | string | user 的名称 |

返回结果

- 成功还原后对{User}对象的 Promise，或者如果状态存储中不存在该用户的名称，则在不拒绝 promise 的情况下返回 null。

  - 类型

    Promise

#### newChannel(name)

返回具有给定名称的 [Channel](https://hyperledger.github.io/fabric-sdk-node/release-1.4/Channel.html) 实例。这代表一个通道及其相关的分类帐。

- 参数

| 名称 | 类型   | 描述                                   |
| :--- | :----- | -------------------------------------- |
| name | string | 通道名称。建议使用名称空间以避免冲突。 |

返回结果

- 未初始化的通道实例。

  - 类型

    [Channel](https://hyperledger.github.io/fabric-sdk-node/release-1.4/Channel.html)

#### newOrderer(url, opts)

返回具有给定 url 和 opts 的 Orderer 对象。 一个 Orderer 对象封装了 Orderer 节点的属性以及通过 grpc 流 API 与其交互的属性。 客户端对象使用orderer对象来广播创建和更新通道的请求。 Channel 对象使用它们来广播请求交易的请求。 此方法将创建 Orderer。

- 参数

| 名称 | 类型                                                                                                   | 描述                                |
| :--- | :----------------------------------------------------------------------------------------------------- | ----------------------------------- |
| url  | string                                                                                                 | 格式为 "grpc(s)://host:port" 的 URL |
| opts | [ConnectionOpts](https://hyperledger.github.io/fabric-sdk-node/release-1.4/global.html#ConnectionOpts) | 连接到 Orderer 的选项。             |

返回结果

- orderer 实例。

  - 类型

    [Orderer](https://hyperledger.github.io/fabric-sdk-node/release-1.4/Orderer.html)

#### newPeer(url, opts)

返回具有给定 url 和 opts 的[Peer](https://hyperledger.github.io/fabric-sdk-node/release-1.4/Peer.html)对象。 对等对象封装了背书对等的属性以及通过 grpc 服务 API 与其进行的交互。 对等对象由[Client](https://hyperledger.github.io/fabric-sdk-node/release-1.4/Client.html)对象用于发送与通道无关的请求(例如，安装链码，查询 Peer 对象以获取已安装的链代码等)。它们也被[Channel](https://hyperledger.github.io/fabric-sdk-node/release-1.4/Channel.html)对象用于发送通道感知的请求(如实例化链代码和调用事务)。 此方法将返回一个新的[Peer](https://hyperledger.github.io/fabric-sdk-node/release-1.4/Peer.html)对象。

- 参数

| 名称 | 类型                                                                                                   | 描述                                |
| :--- | :----------------------------------------------------------------------------------------------------- | ----------------------------------- |
| url  | string                                                                                                 | 格式为 "grpc(s)://host:port" 的 URL |
| opts | [ConnectionOpts](https://hyperledger.github.io/fabric-sdk-node/release-1.4/global.html#ConnectionOpts) | 与 Peer 连接的选项。                |

返回结果

- Peer 实例。

  - 类型

    [Peer](https://hyperledger.github.io/fabric-sdk-node/release-1.4/Peer.html)

#### newTransactionID(admin)

返回一个新的 [TransactionID](https://hyperledger.github.io/fabric-sdk-node/release-1.4/TransactionID.html) 对象。 Fabric TransactionID 被构造为随机数的哈希值，该哈希值与签名身份的序列化字节串联在一起。 TransactionID 对象将随机数和结果 ID 字符串捆绑在一起，成为一个连贯的对。

此方法需要为客户端实例分配一个 userContext。

- 参数

| 名称  | 类型    | 描述                                                                                                     |
| :---- | :------ | -------------------------------------------------------------------------------------------------------- |
| admin | boolean | 如果为 true，则应基于管理员凭据构建此 transactionID。默认值为基于 userContext 的非管理员 TransactionID。 |

返回结果

- 一个对象，该对象包含基于客户端的 userContext 的事务 ID 和随机生成的随机数值。

  - 类型

    [TransactionID](https://hyperledger.github.io/fabric-sdk-node/release-1.4/TransactionID.html)

#### &lt;async&gt; queryChannels(peer, useAdmin)

查询目标 Peer 以获取 Peer 已加入的所有通道的名称。

- 参数

| 名称     | 类型                                                                        | 描述                                                                                                                             |
| :------- | :-------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------- |
| peer     | [Peer](https://hyperledger.github.io/fabric-sdk-node/release-1.4/Peer.html) | 目标 Peer 发送查询                                                                                                               |
| useAdmin | boolean                                                                     | 可选的。指示在向 Peer 发出此呼叫时应使用管理员凭据。必须通过通用连接配置文件或使用“ setAdminSigningIdentity”方法来加载管理标识。 |

返回结果

- 返回一个[ChannelQueryResponse](https://hyperledger.github.io/fabric-sdk-node/release-1.4/global.html#ChannelQueryResponse)的 Promise。

  - 类型

    Promise

#### &lt;async&gt; queryInstalledChaincodes(peer, useAdmin)

查询 Peer 上已安装的链码。

- 参数

| 名称     | 类型                                                                        | 描述                                                                                                                             |
| :------- | :-------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------- |
| peer     | [Peer](https://hyperledger.github.io/fabric-sdk-node/release-1.4/Peer.html) | 目标 Peer                                                                                                                        |
| useAdmin | boolean                                                                     | 可选的。指示在向 Peer 发出此呼叫时应使用管理员凭据。必须通过通用连接配置文件或使用“ setAdminSigningIdentity”方法来加载管理标识。 |

返回结果

- [ChaincodeQueryResponse](https://hyperledger.github.io/fabric-sdk-node/release-1.4/global.html#ChaincodeQueryResponse) 对象的 Promise。

  - 类型

    Promise

#### &lt;async&gt; queryPeers(request)

查询目标 Peer 以获取目标 Peer 已知的所有 Peer 的 [Peer](https://hyperledger.github.io/fabric-sdk-node/release-1.4/Peer.html) 对象列表。

- 参数

| 名称    | 类型                                                                                                       | 描述       |
| :------ | :--------------------------------------------------------------------------------------------------------- | ---------- |
| request | [PeerQueryRequest](https://hyperledger.github.io/fabric-sdk-node/release-1.4/global.html#PeerQueryRequest) | 请求参数。 |

返回结果

- Peer 信息列表。

  - 类型

    [PeerQueryResponse](https://hyperledger.github.io/fabric-sdk-node/release-1.4/global.html#PeerQueryResponse)

#### &lt;async&gt; saveUserToStateStore()

将当前的 userContext 保留到键值存储中。

返回结果

- 成功持久化(persistence)后对 userContext 对象的 Promise。

  - 类型

    Promise

#### setAdminSigningIdentity(private_key, certificate, mspid)

设置管理员签名身份对象。此方法将仅分配供此客户端实例使用的签名身份，而不会保留该身份。

- 参数

| 名称        | 类型   | 描述                            |
| :---------- | :----- | ------------------------------- |
| private_key | string | 私钥 PEM 字符串                 |
| certificate | string | PEM 编码的证书字符串            |
| mspid       | string | 本地签名身份的会员服务提供商 ID |

#### setCryptoSuite(cryptoSuite)

设置客户端实例以使用 CryptoSuite 对象进行签名和哈希创建和设置 CryptoSuite 是可选的，因为客户端将基于默认配置设置构造实例:

- crypto-hsm:使用硬件安全模块(如果设置为 true)或基于软件的密钥管理(如果设置为 false)的实现
- crypto-keysize:与数字签名公钥算法一起使用的安全级别或密钥大小。 当前支持 ECDSA，有效密钥大小为 256 和 384
- crypto-hash-algo:哈希算法
- 密钥值存储:某些 CryptoSuite 实现需要密钥存储来保留私钥。 为此提供了一个 CryptoKeyStore，它可以在 KeyValueStore 接口的任何实现之上使用，例如基于文件的存储或基于数据库的存储。 具体的实现方式取决于此配置设置的值。

- 参数

| 名称        | 类型                                                                                                            | 描述             |
| :---------- | :-------------------------------------------------------------------------------------------------------------- | ---------------- |
| cryptoSuite | [module:api.CryptoSuite](https://hyperledger.github.io/fabric-sdk-node/release-1.4/module-api.CryptoSuite.html) | CryptoSuite 对象 |

继承自(Inherited From):

- [BaseClient#setCryptoSuite](https://hyperledger.github.io/fabric-sdk-node/release-1.4/BaseClient.html#setCryptoSuite)

覆写(Overrides:):

- [BaseClient#setCryptoSuite](https://hyperledger.github.io/fabric-sdk-node/release-1.4/BaseClient.html#setCryptoSuite)

#### setDevMode(devMode)

将 dev mode 设置为 true 或 false 可以反映 Fabric 后端的模式。有关详细信息，请参见 [Client#isDevMode](https://hyperledger.github.io/fabric-sdk-node/release-1.4/Client.html#isDevMode) 。

- 参数

| 名称    | 类型    | 描述 |
| :------ | :------ | ---- |
| devMode | boolean |      |

#### setStateStore(keyValueStore)

设置一个可选的状态存储以保留应用程序状态。 状态存储区必须实现 [module:api.KeyValueStore](https://hyperledger.github.io/fabric-sdk-node/release-1.4/module-api.KeyValueStore.html)接口。

SDK 支持持久存储 [User](https://hyperledger.github.io/fabric-sdk-node/release-1.4/User.html)对象，这样就不必重复传递诸如证书和私钥之类的重量级对象。 SDK 开箱即用地提供了基于文件的实现和基于 CouchDB 的实现，该实现也支持 Cloudant。 应用程序可以提供其他实现。

- 参数

| 名称          | 类型                                                                                                                | 描述                     |
| :------------ | :------------------------------------------------------------------------------------------------------------------ | ------------------------ |
| keyValueStore | [module:api.KeyValueStore](https://hyperledger.github.io/fabric-sdk-node/release-1.4/module-api.KeyValueStore.html) | KeyValueStore 实现的实例 |

#### setTlsClientCertAndKey(clientCert, clientKey)

设置使用公用连接配置文件(连接配置文件)时建立网络端点所必需的双向 TLS 客户端证书和密钥。 必须在需要 Peer，Orderer 或通道 eventhub 之前调用此方法。 如果尚未为客户提供 tls 客户资料，则在用户已分配给该客户的情况下将生成该资料。 请注意，它将始终使用默认软件 cryptosuite，而不是分配给客户端的默认软件。

- 参数

| 名称       | 类型               | 描述                   |
| :--------- | :----------------- | ---------------------- |
| clientCert | string             | Pem 编码的客户端证书。 |
| clientKey  | Array.&lt;byte&gt; | 客户端密钥。           |

#### &lt;async&gt; setUserContext(user, skipPersistence)

设置 User 类的实例作为此客户端实例的安全上下文。 该用户的签名身份(私钥及其相应的证书)将用于通过 Fabric 后端对所有请求进行签名。

设置用户上下文后，如果已在客户端实例上设置了“状态存储”，则 SDK 会将对象保存在持久性缓存中。 如果未设置状态存储，则不会建立此缓存，并且如果应用程序崩溃并恢复，则应用程序负责再次设置用户上下文。

- 参数

| 名称            | 类型                                                                                                                                                                                                      | 描述                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |
| :-------------- | :-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| user            | [User](https://hyperledger.github.io/fabric-sdk-node/release-1.4/User.html) &#124; [UserNamePasswordObject](https://hyperledger.github.io/fabric-sdk-node/release-1.4/global.html#UserNamePasswordObject) | User 类的一个实例，其中封装了经过身份验证的用户的签名材料(私钥和注册证书)。 该参数也可以是[UserNamePasswordObject](https://hyperledger.github.io/fabric-sdk-node/release-1.4/global.html#UserNamePasswordObject)，其中包含用户名以及可选的密码和 caName。 必须已加载公共连接配置文件才能使用[UserNamePasswordObject](https://hyperledger.github.io/fabric-sdk-node/release-1.4/global.html#UserNamePasswordObject)，这还将创建用户上下文并在此客户端实例上进行设置。 创建的用户上下文将基于当前的网络配置(即当前组织的 CA，当前持久性存储)。 |
| skipPersistence | boolean                                                                                                                                                                                                   | 是否跳过将用户对象保存到持久性中。 默认值为 false，该方法将尝试将用户对象保存到状态存储中。 当使用公共连接配置文件和[UserNamePasswordObject](https://hyperledger.github.io/fabric-sdk-node/release-1.4/global.html#UserNamePasswordObject)时，用户对象将始终存储到持久性存储中。                                                                                                                                                                                                                                                                 |

返回结果

- 成功将用户持久存储到状态存储后，'user'对象的 Promise。

  - 类型

    Promise

#### signChannelConfig(loadConfig)

通道配置更新可以发送给 orderer 进行处理。 orderer 强制执行通道创建或更新策略，以便仅在请求中发现参与组织的足够签名时才进行更新。 通常，通道创建或更新请求必须由参与组织的 ADMIN 负责人签名，尽管可以在定义联盟时自定义此策略。

此方法使用客户端实例的当前签名身份来签名传入的配置字节，并返回准备好包含在配置更新协议消息中的签名，以发送给 orderer。

- 参数

| 名称       | 类型               | 描述                     |
| :--------- | :----------------- | ------------------------ |
| loadConfig | Array.&lt;byte&gt; | 字节(byte)形式的配置更新 |

返回结果

- 配置字节上当前用户的签名。

  - 类型

    [ConfigSignature](https://hyperledger.github.io/fabric-sdk-node/release-1.4/global.html#ConfigSignature)

#### updateChannel(request)

呼叫 orderer 以更新现有通道。在 orderer 成功处理了通道更新之后，orderer 将剪切包含新通道配置的新块，并将其交付给通道中的所有参与 Peer

- 参数

| 名称    | 类型                                                                                                   | 描述     |
| :------ | :----------------------------------------------------------------------------------------------------- | -------- |
| request | [ChannelRequest](https://hyperledger.github.io/fabric-sdk-node/release-1.4/global.html#ChannelRequest) | 请求对象 |

返回结果

- 保证结果对象具有 orderer 接受更新请求的状态。 当 orderer 创建的新通道配置块已提交给通道的 Peer 时，通道更新最终完成。 要通知成功更新通道，应用程序应使用 ChannelEventHub 连接到 Peer 并注册一个块侦听器。

  - 类型

    Promise

---
