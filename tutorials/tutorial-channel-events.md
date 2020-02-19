# fabric-client：如何使用基于通道的事件服务(How to use the channel-based event service)

## fabric-client：如何使用基于通道的事件服务(How to use the channel-based event service)

本教程说明了基于通道的事件的用法。 从 v1.1 开始，基于通道的事件是 Hyperledger Fabric Node.js 客户端的一项新功能。 它从 v1.0 取代了事件中心，并提供了一个更有用和更可靠的界面，供应用程序接收事件。

有关 Fabric 入门的更多信息，请查看 [构建您的第一个网络(Building your first network)](http://hyperledger-fabric.readthedocs.io/en/latest/build_network.html)。

以下内容假定您对 Fabric 网络（orderer 和 peer）以及 Node 应用程序开发（包括 Java Promise 的使用）有所了解。

### 总述

客户端应用程序可以使用 Fabric Node.js 客户端注册"侦听器(listener)"，以接收将其添加到通道分类帐中的块。我们称这些为"基于通道的事件(channel-based events)"，它们允许客户端开始从特定的块编号接收块，从而使事件处理可以在可能已丢失的块上正常运行。 Fabric Node.js 客户端还可以通过处理传入的块并查找特定的事务或链码事件来协助客户端应用程序。这允许向客户端应用程序通知事务完成或任意链码事件，而不必在接收到块时执行多次查询或搜索整个块。

应用程序可以使用块或链码事件将通道数据提供给其他应用程序。例如，应用程序可以侦听区块事件并将事务数据写入数据存储，以针对通道数据执行查询或其他分析。对于接收到的每个块，块侦听器应用程序可以遍历块事务，并使用来自每个有效事务的'rwset'的键/值写入来构建数据存储（有关这些数据结构的详细信息，请参见 [Block](https://hyperledger.github.io/fabric-sdk-node/release-1.4/global.html#Block) 和[Transaction](https://hyperledger.github.io/fabric-sdk-node/release-1.4/global.html#Transaction) 类型定义）。

事件服务还允许应用程序接收"filtered(过滤的)"block(块)事件（允许接收事务验证状态而无需提供其他敏感信息）。可以在 Fabric 中独立配置对"filtered(已过滤)"和"unfiltered(未过滤)"事件的访问。默认行为是连接以接收过滤后的块事件。要连接以接收未过滤的块事件，请调用 connect(true)（请参见下文）。

请注意，如果您注册块事件然后提交交易，则不应假设哪个块包含您的交易。特别是，您不应假定您的事务位于与注册到 Peer 基于通道的事件服务之后收到的第一个块事件相关联的块中。相反，您可以简单地注册一个交易事件。

### 通道 APIs(Channel APIs)

- newChannelEventHub(peer)-获取 ChannelEventHub 的新实例的 Channel 实例方法。
- getChannelEventHubsForOrg-根据组织获取 ChannelEventHub 的列表。 如果省略组织名称，则使用当前用户的当前组织。

### 通道事务中心 APIs(ChannelEventHub APIs)

- registerBlockEvent(eventCallBack,errorCallBack,options)-注册块事件。
- unregisterBlockEvent(reg_num)-删除块注册
- registerTxEvent(tx_id,eventCallBack,errorCallBack,options)-注册特定的交易事件。
- unregisterTxEvent(tx_id)-删除特定的交易注册。
- registerChaincodeEvent(ccid,eventCallBack,errorCallBack,options)-注册链码事件。
- unregisterChaincodeEvent(cc_handle)-删除链码事件注册。
- connect({full_block: true})-使客户端通道事件中心与基于结构通道的事件服务连接。必须在您的 ChannelEventHub 实例接收事件之前进行此调用。当基于通道的事件中心与服务连接时，它将请求接收块或已过滤的块。如果省略了 full_block 参数，它将默认为 false，并且将请求过滤的块。一旦调用 connect()，就不能更改接收块或过滤块。在重播块时(通过设置 startBlock 和 endBlock)，必须在注册侦听器后调用 connect()，因为必须设置与 Peer 的连接以请求现有块。
- disconnect()-让客户端通道事件中心关闭与基于 fabric 网络通道的事件服务的连接，并使用已注册的 errorCallBacks 将关闭通知给所有当前通道事件注册。

#### peer 参数

获取 ChannelEventHub 的新实例时，必须包含此参数。使用连接配置文件时，该值可以是 Peer 实例或 Peer 的名称，请参阅 [如何使用公用的公共连接配置文件(How to use a common common connection profile file)](https://hyperledger.github.io/fabric-sdk-node/release-1.4/tutorial-network-config.html)。

#### eventCallback 参数

必须包含此参数。当侦听特定事务或链码事件时，此通道将在收到新块时通知该回调函数。

#### errorCallback 参数

这是一个可选参数。 当此通道事件中心关闭时，将通知此回调函数。 关机可能是由 Fabric 网络错误，网络连接问题或对 disconnect() 方法的调用引起的。 如果在将 endBlock 设置为"newest"的情况下进行回放，也会由于接收到最后一个块而导致通道事件中心关闭时调用此回调。

#### options 参数

这是一个可选参数。此参数将包含以下可选属性：

- {integer &#124; 'newest' &#124; 'oldest' &#124; 'last_seen'} startBlock（可选）用于事件检查的起始块号。如果包含该 Peer，则将要求 Peer 基于通道的事件服务从该块号开始发送块。这是恢复已添加到分类帐中的丢失块的恢复聆听或重播方法。此选项更改了与 Fabric Peer 基于通道的事件服务的连接方式，因此必须在通道事件中心建立连接之前进行注册。重播事件可能会使其他事件侦听器迷惑；因此，在侦听器注册上使用 startBlock 和/或 endBlock 时，ChannelEventHub 上将只允许一个侦听器。

  - Number - 可以将数字值指定为块编号。
  - 'newest' - 字符串'newest'。在账本上最新块的连接时，将具有由 Peer 基于通道的事件服务确定的块号。
  - 'oldest' - 字符串'oldest'。这将具有由 Peer 基于通道的事件服务在分类帐上最旧的块的连接时确定的块号，除非修剪了您的分类帐，否则它将是块 0。
  - 'last_seen' - 'last_seen'的字符串。这将使通道事件中心实例在注册时确定块编号。该数字将基于此通道事件中心从 Peer 基于通道的事件服务接收到的最后一个块。在事件侦听器上使用此选项确实需要该通道事件中心之前已运行。

- {boolean} unregister - (可选)此设置指示在看到事件后应注销 (unregister) 。当应用程序使用超时仅等待指定的时间量才能看到事务时，超时处理应包括对事务事件侦听器的手动“注销”，以避免意外调用事件回调。对于不同类型的事件侦听器，此设置的默认设置是不同的。对于块侦听器，当将 end_block 设置为选项时，默认值为 true，侦听器将处于活动状态并接收块，直到接收到结束块，然后侦听器将自动注销。对于事务侦听器，默认值为 true，并且一旦发生事务事件，侦听器将自动注销。如果事务侦听器使用了 endBlock，则默认值为即使未看到事务也将自动注销该侦听器。对于 chaincode 事件侦听器，默认值将为 false，因为匹配筛选器可能用于许多事务，但是，如果 chaincode 事件侦听器设置了 endBlock，则在看到 endBlock 后将自动取消注册。

- {boolean} disconnect - (可选)此设置指示 ChannelEventHub 实例在看到事件后自动与 Peer 基于通道的事件服务断开连接。默认值为 false。如果未设置并且设置了 endBlock，则 ChannelEventHub 实例将自动断开自身连接。

- {boolean} as_array - (可选)此设置指示 ChannelEventHub 实例将所有链码事件作为数组而不是一次发送给回调。此设置仅适用于链码事件。

#### peer 参数

#### peer 参数
