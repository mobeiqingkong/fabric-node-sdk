# 设置应用程序开发人员的环境(Setting up the Application Developer's Environment)

## 说明

本教程描述了如何准备开发环境，以构建业务应用程序以使用基于 Hyperledger Fabric 的区块链网络。 从高层次上讲，运行在 Hyperledger Fabric 网络上的业务应用程序由两部分组成:在服务器([背书(endorser)](http://hyperledger-fabric.readthedocs.io/en/latest/arch-deep-dive.html#peer) 节点)中运行的链码和在 Node.js 应用程序中运行的客户端代码。

对于链代码开发，请访问 Hyperledger Fabric [链码教程(chaincode tutorials)](http://hyperledger-fabric.readthedocs.io/en/latest/chaincode.html)教程。

有关启动 Hyperledger Fabric 网络的完整信息，请参阅[构建第一个网络教程 (Build your first network tutorial)](http://hyperledger-fabric.readthedocs.io/en/latest/build_network.html)。

以下教程假定已经开发了链码，并且重点是开发客户端应用程序。

## 什么构成了 Hyperledger Fabric 应用程序开发环境？(What makes up a Hyperledger Fabric application development environment?)

您将在下面找到 Hyperledger Fabric 设计的高级摘要，旨在以入门级的了解为基础，以便您可以自行设置开发环境。有关概念和体系结构的全面说明，请访问官方[Hyperledger Fabric 文档 (Hyperledger Fabric documentation)](http://hyperledger-fabric.readthedocs.io/en/latest)。

首先，您将需要一个订购者(orderer)。但是 orderer 不是负责达成共识吗？为什么先从这里开始？的确，Hyperledger Fabric 区块链网络订购服务的主要职责是在分类账维护者(也就是提交者节点(committer nodes))之间就交易达成共识。但是，订购服务还维护有关整个网络的关键数据:哪些组织(organizations )正在参与，已创建了哪些通道(channels)，哪些组织是给定通道的一部分，以及最后但同样重要的，对网络进行任何形式的变更都制定了哪些策略。本质上，订购服务将网络保持在一起。

好的，我们必须有一个 orderer 节点，以便我们可以向其中添加参与组织并启动网络。接下来，您需要每个参与组织的 Peer，才能参与对交易的背书和维护分类帐。

Peer 节点扮演两个角色:背书者和提交者。可以根据引导程序配置启用或禁用 Peer 的背书者角色。请注意，所有 Peer 总是提交者。为了获得高可用性，在实际部署中，每个组织都需要多个 Peer。对于开发环境，在大多数情况下，每个组织一个 Peer 就足够了。该 Peer 将既是背书人又是提交人。它将向交易提案发送以进行背书和查询，以从分类帐中发现信息。

Peer 节点扮演的另一个重要角色是将事件广播给感兴趣的各方。每当将块添加到分类账时，Peer 就会通过专用流端口发送事件。组织内的任何应用程序都可以注册自己以监听该端口以获得通知。

最后一部分难题是身份。 Hyperledger Fabric 网络中的每个操作都必须经过数字签名，以用于访问控制或出处/审核(谁做了什么)，或两者兼而有之。身份基于公钥基础结构(PKI)标准。每个 orderer 节点，每个 Peer 节点以及每个用户/交易者都必须具有一个密钥对，该密钥对的公钥包含在由证书颁发机构(CA)签名的 x.509 证书中。由于 x.509 是开放标准，因此 Hyperledger Fabric 可以与任何现有的证书颁发机构一起使用。这通常是一个痛苦的过程，需要大量潜在的繁文 tape 节来获取真实证书，因此出于开发目的，使用本地生成的自签名证书是一种流行的做法。正如您将在后面的部分中看到的那样，Hyperledger Fabric 提供了减轻这种麻烦的工具。

同样与身份有关，您应该确定 Fabric-CA 是否应包含在解决方案中。这是具有 REST API 的服务器，该服务器支持动态身份管理，包括注册，注册(获取证书(getting certificates))，吊销和重新注册。因此，在动态提供用户身份时非常有用。请注意，以这种方式配置的用户身份仅具有 MEMBER 角色，这意味着它将无法执行为 ADMIN 角色保留的某些操作:

- 创建/更新通道
- 安装/实例化链码
- 查询已安装/实例化的链码

对于这些特权操作，客户端必须使用 ADMIN 用户来提交请求。

如果选择不使用 Fabric-CA，则所有内容仍然可以使用，但是应用程序负责管理用户证书。

## 先决条件(Prerequisites)

您将需要以下软件:

- [Docker and Docker Compose](http://hyperledger-fabric.readthedocs.io/en/latest/prereqs.html#docker-and-docker-compose) - 参见 Hyperledger Fabric 的详细说明
- [Nodejs](https://nodejs.org/en/download/) v8.9.0 或更高的版本, 最高 9.0 ( **Node v9.0+ 不支持** )

## 准备加密材料(Prepare crypto materials)

如上所述，使用 x.509 证书建立身份。如果您考虑使用，我们将需要一大堆证书，因为其中涉及许多身份:

- Peer 需要身份来签署背书
- orderer 需要身份来签署提议的区块，以便提交者进行验证并附加到分类账中
- 应用程序需要身份来签署交易请求
- Fabric CA 本身也需要身份，因此可以验证其在证书中的签名

幸运的是有一个工具可以做到这一点。遵循[本指南(this guide)](http://hyperledger-fabric.readthedocs.io/en/latest/build_network.html#crypto-generator)以使用 cryptogen 工具一次生成所有必需的密钥和证书。

请注意，cryptogen 工具将为每个 orderer 和 Peer 组织自动为 Fabric CA 节点生成身份，这些身份可用于启动 Fabric-CA 服务器(如果您选择将其用作上述解决方案的一部分)。此外，它还会生成一个具有 ADMIN 角色的管理员用户，该用户具有执行上面列出的管理员级别操作的特权。最后，它还会生成常规用户(MEMBER 角色)来提交事务。

这将为我们提供启动所需的所有加密材料。

## 让事情真正发生-创世区块(Getting things rolling for real - the genesis block)

如上所述，orderer 应该是引导(启动)网络的第一步。 orderer 将需要包装在创世区块的初始配置。 请按照[此处的说明(instructions here)](http://hyperledger-fabric.readthedocs.io/en/latest/build_network.html#configuration-transaction-generator) 使用 configtxgen 工具生成 genesis.block。 输出，即 orderer 的创始块文件，将在下一步中用于启动 orderer 节点。

```none
请注意，某些功能不向后兼容。 要启用特定fabric版本的功能，应在生成genesis.block之前更新configtx.yaml中的"Capabilities"部分。
```

## 开启网络(Start the network)

现在我们准备将所有内容放在一起。 启动开发环境的最简单方法是使用 docker-compose。 请按照 [此处的说明(instructions here)](http://hyperledger-fabric.readthedocs.io/en/latest/build_network.html#start-the-network) 启动网络。 为了最大程度地减少出错的可能性，您可能希望在不使用 TLS 的情况下运行网络。

以上步骤为您提供了一个开发环境。 现在，在您可以要求它处理任何交易之前，必须首先创建一个通道。

---
