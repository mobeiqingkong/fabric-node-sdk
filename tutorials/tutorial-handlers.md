# fabric-common:如何使用背书，查询和提交处理程序(How to use the endorsement and commit handlers)

## fabric-common:如何使用背书，查询和提交处理程序(How to use the endorsement and commit handlers)

本教程从 1.3 版本开始说明 Hyperledger Fabric Node.js 客户端对处理程序的使用。

有关更多信息:

- Hyperledger Fabric 入门，请参阅[构建第一个网络(Building your first network)](http://hyperledger-fabric.readthedocs.io/en/latest/build_network.html)。
- Hyperledger Fabric 中通道的配置以及创建和更新的内部过程，请参见[Hyperledger Fabric 通道配置(Hyperledger Fabric channel configuration)](http://hyperledger-fabric.readthedocs.io/en/latest/configtx.html)
- [服务发现(Service Discovery)](https://hyperledger-fabric.readthedocs.io/en/latest/discovery-overview.html)

以下内容假定您对 Hyperledger Fabric 网络(Orderer 和 Peer)以及 Node 应用程序开发有所了解，包括对 Javascript promise 和 async await 的使用。

### 概述

Fabric 客户端提供了用于定制代码的功能，该代码将处理背书过程以及向 orderer 提交背书。 定义了两个插入点，一个在 [Channel#sendTransactionProposal](https://hyperledger.github.io/fabric-sdk-node/release-1.4/Channel.html#sendTransactionProposal) 上，一个在[Channel#sendTransaction](https://hyperledger.github.io/fabric-sdk-node/release-1.4/Channel.html#sendTransaction)上。 Fabric 客户端将控制权交给处理程序以完成处理。 定制代码可以决定重试，尝试另一个端点或使用发现来完成任务。

请参见 [fabric-client: How to use the discovery service](https://hyperledger.github.io/fabric-sdk-node/release-1.4/tutorial-discovery.html) (如何使用发现服务)以及如何将默认处理程序与发现一起使用。

#### 修改后的 API 将使用处理程序(Modified API's that will use handlers)

- channel.initialize() - 增强了此方法，以实例化供通道使用的处理程序实例。该方法将从系统配置设置中获取路径以创建和初始化它们。
- channel.sendTransactionProposal() - 如果已实例化和初始化此方法，则该方法已得到增强，可以使用背书处理程序。
- channel.sendTransaction() - 如果已实例化和初始化了此方法，则此方法已增强为使用提交处理程序。

#### 新的配置设置(New configuration settings)

- endorsement-handler - 字符串-背书处理程序的路径。允许使用自定义处理程序。 sendTransactionProposal()方法中使用此处理程序来确定目标 peer 以及如何发送建议。(默认为'fabric-client/lib/impl/DiscoveryEndorsementHandler.js')
- commit-handler - 字符串-提交处理程序的路径。允许使用自定义处理程序。在 sendTransaction()方法中使用此处理程序来确定排序器以及如何发送要提交的事务。(默认为'fabric-client/lib/impl/BasicCommitHandler.js')

### 新的背书处理程序(new Endorsement Handler)

可以使用自定义代码发送要批准的提案。 默认情况下，Fabric 客户端将使用名为 DiscoveryEndorsementHandler 的文件。 通过使用 setConfigSetting()更改配置设置"endorsement-handler"或在应用程序已应用到 fabric 客户端配置的配置 JSON 文件中放置新行，可以使用其他背书处理程序。 这将为调用后初始化的所有通道实例化一个位于属性提供路径上的处理程序。 缺省处理程序旨在与发现一起使用，以提供 Peer 的自动选择和故障转移。 如果在没有发现的情况下使用处理程序，则处理程序将仅发送至 targets 参数中定义的 Peer，而不会进行故障转移。 还可以使用 channel.initialize()请求调用参数上的 endorsementHandler 属性来更改处理程序。 这将实例化一个处理程序，该处理程序位于属性中为此通道提供的路径上。

```javascript
// set value in memory
// 在内存中设置值
Client.setConfigSetting('endorsement-handler', '/path/to/the/handler.js');
--or--
// the path to an additional config file
// 附加配置文件的路径
Client.addConfigFile('/path/to/config.json');
// the json file contains the following line
// json文件包含以下行
// "endorsement-handler": "/path/to/the/handler.js"
--or--
const request = {
    ...
    endorsementHandler: "/path/to/the/handler.js",
    ...
}
// initialize must be run to use handlers.
// 必须运行初始化以使用处理程序。
channel.initialize(request);
```

背书处理程序必须实现 api.EndorsementHandler。初始化通道后，通道将读取路径设置并创建处理程序的实例，以供新的通道实例使用。

### 新的提交处理程序(new CommitHandler)

可以使用自定义代码发送要提交的背书。 默认情况下，fabric 客户端将使用名为 BasicCommitHandler 的文件。 可以通过执行 setConfigSetting()更改配置设置"commit-handler"或在应用程序已应用到 fabric 客户端配置的配置 JSON 文件中放置新行来更改提交处理程序。 默认处理程序旨在与发现一起使用，以提供对 Orderer 程序的自动选择并进行故障转移。 当在没有发现的情况下使用时，处理程序仍将故障转移到分配给该通道的所有 Orderer，发送给每个 Orderer，直到 Orderer 对交易提交成功做出响应为止。 还可以使用 channel.initialize()请求调用参数上的 commitHandler 属性来更改处理程序。 这将实例化一个处理程序，该处理程序位于属性中为此通道提供的路径上。

```javascript
// set the config value in memory
// 在内存中设置配置值
Client.setConfigSetting('commit-handler', '/path/to/the/handler.js');
--or--
// path of an additional config file
// 附加配置文件的路径
Client.addConfigFile('/path/to/config.json');
// the json file contains the following line
// json文件包含以下行
// "commit-handler": "/path/to/the/handler.js"
--or--
const request = {
    ...
    commitHandler: "/path/to/the/handler.js",
    ...
}
// initialize must be run to use handlers.
// 必须运行初始化以使用处理程序。
channel.initialize(request);
```

提交处理程序必须实现 api.CommitHandler。初始化通道后，通道将读取路径设置并创建处理程序的实例，以供新的通道实例使用。

---
