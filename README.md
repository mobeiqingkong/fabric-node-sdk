# 前言

Hyperledger Fabric SDK for Node.js 提供了功能强大的 API 用于与 Hyperledger Fabric 区块链进行交互。该 SDK 旨在运行 Node.js JavaScript 时使用。

## 概述

Hyperledger Fabric 是企业级许可的区块链网络的操作系统。有关该结构的高级概述，请访问<http://hyperledger-fabric.readthedocs.io/en/latest/。>

- 创建 [通道(channels)](channel.md)
- 请求 [对等节点(peer nodes)](peer.md) 加入通道
- 在 peers 里安装 [链码(chaincodes)](chaincode.md)
- 实例化通道中的链码
- 通过链码来调用交易
- 查询 [分类账本(ledger)](ledger-features.md) 中的交易或区块

### 不同的 Fabric 网络组件的不同之处

[事务流(Transaction Flow)](Transaction Flow.md) 文档很好地描述了应用程序/ SDK, peers, 和 orderers 共同工作来处理事务和产生区块。

Fabric 上的安全性通过数字签名来实施。 用户对 fabric 的所有请求都必须由用户使用适当的注册证书签名。 为了使用户的注册证书在 fabric 上被视为有效，必须由受信任的证书颁发机构（CA）对其进行签名。 Fabric 支持任何标准 CA。 此外，Fabric 提供了一个 CA 服务器。 请参阅此[概述(overview)](overview.md)。

### 适用于 Node.js 的 SDK 的功能

The Hyperledger Fabric SDK for Node.js is designed in an Object-Oriented programming style. Its modular construction enables application developers to plug in alternative implementations of key functions such as crypto suites, the state persistence store, and logging utility.

Hyperledger Fabric SDK for Node.js 以面向对象的编程风格设计。 它的模块化结构使应用程序开发人员可以插入关键功能的替代实现，例如加密套件，状态持久性存储和日志记录。

SDK 的功能列表包括：

- [**fabric-network**](module-fabric-network.md) (推荐的 API):
  - [提交交易(Submitting transactions)](module-fabric-network.Transaction.md) 到智能合约。
  - 向智能合约[查询(Querying)](evaluate.md)最新的应用程序状态。
- **fabric-client**:
  - [创建新的通道(create a new channel)](createChannel.md)
  - [发送通道信息给 peer 加入 (send channel information to a peer to join)](joinChannel.md)
  - [在 peer 上安装链码 (install chaincode on a peer)](installChaincode.md)
  - 实例化通道中的链码，包括两个步骤： [提议(propose)](sendInstantiateProposal.md)和[事物(transact)](sendTransaction.md)
  - 提交交易，涉及两个步骤：[提议(propose)](sendTransactionProposal.md) and [交易(transact)](sendTransaction.md)
  - [查询链码以获取最新的应用程序状态 (query a chaincode for the latest application state)](queryByChaincode.md)
  - 各种查询功能：
    - [通道高度 (channel height)](https://hyperledger.github.io/fabric-sdk-node/release-1.4/Channel.html#queryInfo)
    - [通过 number 查询区块(block-by-number)](https://hyperledger.github.io/fabric-sdk-node/release-1.4/Channel.html#queryBlock), [通过哈希查询区块(block-by-hash)](https://hyperledger.github.io/fabric-sdk-node/release-1.4/Channel.html#queryBlockByHash)
    - [peer 所属的所有通道(all channels that a peer is part of)](https://hyperledger.github.io/fabric-sdk-node/release-1.4/Client.html#queryChannels)
    - [peer 中所有已安装的链码 (all installed chaincodes in a peer)](https://hyperledger.github.io/fabric-sdk-node/release-1.4/Client.html#queryInstalledChaincodes)
    - [通道中所有实例化的链码 (all instantiated chaincodes in a channel)](https://hyperledger.github.io/fabric-sdk-node/release-1.4/Channel.html#queryInstantiatedChaincodes)
    - [通过 id 进行的交易 (transaction-by-id)](https://hyperledger.github.io/fabric-sdk-node/release-1.4/Channel.html#queryTransaction)
    - [通道配置数据 (channel configuration data)](https://hyperledger.github.io/fabric-sdk-node/release-1.4/Channel.html#getChannelConfig)
  - 监视事件:
    - [连接到 peer 的事件流 (connect to a peer's event stream)](https://hyperledger.github.io/fabric-sdk-node/release-1.4/ChannelEventHub.html#connect)
    - 监听 [区块事件(block events)](https://hyperledger.github.io/fabric-sdk-node/release-1.4/ChannelEventHub.html#registerBlockEvent)
    - 监听 [交易事件(transactions events)](https://hyperledger.github.io/fabric-sdk-node/release-1.4/ChannelEventHub.html#registerTxEvent) 并确定交易是否已成功提交到分类账或标记为无效
    - 监听链码产生的[自定义事件(custom events)](https://hyperledger.github.io/fabric-sdk-node/release-1.4/ChannelEventHub.html#registerChaincodeEvent)
  - 具有签名功能的可序列化[用户(User)](https://hyperledger.github.io/fabric-sdk-node/release-1.4/User.html)对象
  - 具有多层覆盖的分层配置设置：文件，环境变量，程序参数，内存设置
  - 带有内置记录器（winston）的记录实用程序，可以用许多流行的记录器（包括 log4js 和 bunyan）覆盖
  - 可插拔的 CryptoSuite 接口描述了与 Fabric 进行成功交互所需的加密操作。 提供了两种实现方式：
    - [基于软件的 ECDSA(Software-based ECDSA)](https://hyperledger.github.io/fabric-sdk-node/release-1.4/CryptoSuite_ECDSA_AES.html)
    - [PKCS#11-compliant ECDSA](https://hyperledger.github.io/fabric-sdk-node/release-1.4/CryptoSuite_PKCS11.html)
  - 可插拔的状态存储接口，用于持久存储状态缓存（例如用户）
    - [基于文件的存储(File-based store)](https://hyperledger.github.io/fabric-sdk-node/release-1.4/FileKeyValueStore.html)
    - [基于 CouchDB 的商店(CouchDB-base store)](https://hyperledger.github.io/fabric-sdk-node/release-1.4/CouchDBKeyValueStore.html) 可与 CouchDB 数据库和 IBM Cloudant 一起使用
  - 可自定义的 [加密密钥库(Crypto Key Store)](https://hyperledger.github.io/fabric-sdk-node/release-1.4/CryptoKeyStore.html) 用于任何基于软件的加密套件实现
  - 支持 peers 与 orderers 的 TLS（grpcs：//）或非 TLS（grpc：//）连接，请参阅[Remote](https://hyperledger.github.io/fabric-sdk-node/release-1.4/Remote.html)（[peers](https://hyperledger.github.io/fabric-sdk-node/release-1.4/Peer.html) 和 [orderers](https://hyperledger.github.io/fabric-sdk-node/release-1.4/Orderer.html)的超类）
- **fabric-ca-client**:
  - [注册(register)](https://hyperledger.github.io/fabric-sdk-node/release-1.4/FabricCAServices.html#register) 一个新用户
  - [登记(enroll)](https://hyperledger.github.io/fabric-sdk-node/release-1.4/FabricCAServices.html#enroll) 用户获取由 Fabric CA 签名的注册证书
  - 通过注册 ID [撤消(revoke)](https://hyperledger.github.io/fabric-sdk-node/release-1.4/FabricCAServices.html#revoke) 现有用户或撤消特定证书
  - [可自定义的持久性存储(customizable persistence store)](https://hyperledger.github.io/fabric-sdk-node/release-1.4/FabricCAServices.html)

### API 参考

该 SDK 由 4 个顶级模块组成，可以通过导航菜单**模块**进行访问：

- [**fabric-network**](https://hyperledger.github.io/fabric-sdk-node/release-1.4/module-fabric-network.html): 为客户端应用程序提供高级 API 用于提交事务并评估智能合约（链码）的查询。

- **api**: 可插拔的 API，供应用程序开发人员提供 SDK 使用的关键接口的替代实现。 对于每个接口，都有内置的默认实现。

- **fabric-client**: 提供与基于 Hypreledger Fabric 的区块链网络的核心组件（即对等方(peers)，订购者(orderers)和事件流(event streams)）进行交互的 API。

- **fabric-ca-client**: 提供与可选组件 fabric-ca 交互的 API，该组件包含用于成员资格管理的服务。
  This work is licensed under a [Creative Commons Attribution 4.0 International License](http://creativecommons.org/licenses/by/4.0/).

  该项目已根据[知识共享署名 4.0 国际许可协议(Creative Commons Attribution 4.0 International License)](http://creativecommons.org/licenses/by/4.0/)]获得许可。

### 兼容性

下表显示了经过结构测试的 Fabric，Node 和其他依赖项的版本，这些依赖关系可与 1.4 版 Fabric SDK for Node 一起使用。

|              | Tested       | Supported    |
| :----------- | :----------- | :----------- |
| **Fabric**   | 1.4          | 1.4.x, 2.0.x |
| **Node**     | 10           | 8.9+, 10.13+ |
| **Platform** | Ubuntu 18.04 |              |
---
