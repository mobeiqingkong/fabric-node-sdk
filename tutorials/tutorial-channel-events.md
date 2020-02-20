# fabric-client:如何使用基于通道的事件服务(How to use the channel-based event service)

## fabric-client:如何使用基于通道的事件服务(How to use the channel-based event service)

本教程说明了基于通道的事件的用法。 从 v1.1 开始，基于通道的事件是 Hyperledger Fabric Node.js 客户端的一项新功能。 它从 v1.0 取代了事件中心，并提供了一个更有用和更可靠的界面，供应用程序接收事件。

有关 Fabric 入门的更多信息，请查看 [构建您的第一个网络(Building your first network)](http://hyperledger-fabric.readthedocs.io/en/latest/build_network.html)。

以下内容假定您对 Fabric 网络(orderer 和 peer)以及 Node 应用程序开发(包括 Java Promise 的使用)有所了解。

### 总述

客户端应用程序可以使用 Fabric Node.js 客户端注册"侦听器(listener)"，以接收将其添加到通道分类帐中的块。我们称这些为"基于通道的事件(channel-based events)"，它们允许客户端开始从特定的块编号接收块，从而使事件处理可以在可能已丢失的块上正常运行。 Fabric Node.js 客户端还可以通过处理传入的块并查找特定的事务或链码事件来协助客户端应用程序。这允许向客户端应用程序通知事务完成或任意链码事件，而不必在接收到块时执行多次查询或搜索整个块。

应用程序可以使用块或链码事件将通道数据提供给其他应用程序。例如，应用程序可以侦听区块事件并将事务数据写入数据存储，以针对通道数据执行查询或其他分析。对于接收到的每个块，块侦听器应用程序可以遍历块事务，并使用来自每个有效事务的'rwset'的键/值写入来构建数据存储(有关这些数据结构的详细信息，请参见 [Block](https://hyperledger.github.io/fabric-sdk-node/release-1.4/global.html#Block) 和[Transaction](https://hyperledger.github.io/fabric-sdk-node/release-1.4/global.html#Transaction) 类型定义)。

事件服务还允许应用程序接收"filtered(过滤的)"block(块)事件(允许接收事务验证状态而无需提供其他敏感信息)。可以在 Fabric 中独立配置对"filtered(已过滤)"和"unfiltered(未过滤)"事件的访问。默认行为是连接以接收过滤后的块事件。要连接以接收未过滤的块事件，请调用 connect(true)(请参见下文)。

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

这是一个可选参数。此参数将包含以下可选属性:

- {integer &#124; 'newest' &#124; 'oldest' &#124; 'last_seen'} startBlock(可选)用于事件检查的起始块号。如果包含该 Peer，则将要求 Peer 基于通道的事件服务从该块号开始发送块。这是恢复已添加到分类帐中的丢失块的恢复聆听或重播方法。此选项更改了与 Fabric Peer 基于通道的事件服务的连接方式，因此必须在通道事件中心建立连接之前进行注册。重播事件可能会使其他事件侦听器迷惑；因此，在侦听器注册上使用 startBlock 和/或 endBlock 时，ChannelEventHub 上将只允许一个侦听器。

  - Number - 可以将数字值指定为块编号。
  - 'newest' - 字符串'newest'。在账本上最新块的连接时，将具有由 Peer 基于通道的事件服务确定的块号。
  - 'oldest' - 字符串'oldest'。这将具有由 Peer 基于通道的事件服务在分类帐上最旧的块的连接时确定的块号，除非修剪了您的分类帐，否则它将是块 0。
  - 'last_seen' - 'last_seen'的字符串。这将使通道事件中心实例在注册时确定块编号。该数字将基于此通道事件中心从 Peer 基于通道的事件服务接收到的最后一个块。在事件侦听器上使用此选项确实需要该通道事件中心之前已运行。

- {boolean} unregister - (可选)此设置指示在看到事件后应注销 (unregister) 。当应用程序使用超时仅等待指定的时间量才能看到事务时，超时处理应包括对事务事件侦听器的手动“注销”，以避免意外调用事件回调。对于不同类型的事件侦听器，此设置的默认设置是不同的。对于块侦听器，当将 end_block 设置为选项时，默认值为 true，侦听器将处于活动状态并接收块，直到接收到结束块，然后侦听器将自动注销。对于事务侦听器，默认值为 true，并且一旦发生事务事件，侦听器将自动注销。如果事务侦听器使用了 endBlock，则默认值为即使未看到事务也将自动注销该侦听器。对于 chaincode 事件侦听器，默认值将为 false，因为匹配筛选器可能用于许多事务，但是，如果 chaincode 事件侦听器设置了 endBlock，则在看到 endBlock 后将自动取消注册。

- {boolean} disconnect - (可选)此设置指示 ChannelEventHub 实例在看到事件后自动与 Peer 基于通道的事件服务断开连接。默认值为 false。如果未设置并且设置了 endBlock，则 ChannelEventHub 实例将自动断开自身连接。

- {boolean} as_array - (可选)此设置指示 ChannelEventHub 实例将所有链码事件作为数组而不是一次发送给回调。此设置仅适用于链码事件。

### 如何使用通道事件中心

ChannelEventHub 类非常灵活。它允许许多使用模块。

- 等待我的交易完成
- 查看链码事件
- 审核所有新块的通道
- 重播事件

#### 交易事务

大多数用户将需要知道何时将事务提交到分类帐。 所有交易都有可以监控的唯一标识符。 用户可以注册事件侦听器，以指示特定交易已写入分类账。 这将称为交易事件。

通知交易事件的步骤:

- 获取一个通道事件中心实例，这可以针对每个事务完成，也可以一次执行并重用。
- 将通道事件中心实例与 Peer 的事件服务连接。 对于许多事务重用 ChannelEventHub 实例时，您可能希望在注册之前先进行连接。
- 创建交易并获得背书。
- 使用带有通道事件中心实例的事务的事务 ID 字符串注册回调。
- 如果尚未连接，请连接通道事件中心。
- 提交要订购的背书交易。
- 等待通知已提交到分类帐的事务，如果发生问题则超时。
- 看到事务时注销事件监听器，默认情况下将自动完成。
- 侦听完成后，断开通道事件中心，如果已配置，则可以自动完成。

#### 链码事件

在 Fabric 网络上运行的 Chaincode 程序能够在事务中添加名称和值，这称为 Chaincode 事件。"name"很可能不是唯一的，并且一个以上的事务可能包含链码事件名称，因此侦听器回调可能被调用多次。 可以将侦听器设置为在查找名称匹配时使用正则表达式，以便可以用许多不同的名称通知单个侦听器。

注意:必须先提交 Chaincode 事件并将其写入分类帐，然后才能通知侦听器。 ChannelEventHub 实例将看不到事务中的链码事件，直到事务提交并写入 ChannelEventHub 已连接到事件服务的 Peer 上的 Peer 分类帐中。

发生链码事件时要通知的步骤:

- 获取一个通道事件中心实例，应将其完成一次并重新使用。
- 将通道事件中心实例与 peer 的事件服务连接。 对于许多事务重用 ChannelEventHub 实例时，您可能希望在注册之前先进行连接。
- 用 chaincode 事件的名称注册回调，您可以使用正则表达式来匹配多个名称。
- 如果尚未连接，请连接通道事件中心。
- 在网络上的某个地方，背书并提交了包含链码事件的事务。
- 处理进来的 chaincode 事件。
- 完成后注销事件监听器。
- 收听完毕后，断开通道事件中心。

#### 区块事件

一旦 ChannelEventHub 连接到 Peer 的事件服务，它将开始接收添加到分类账中的块，除非指定了"startBlock"，否则它将开始从指定的块中接收块。 当 ChannelEventHub 实例从 Peer 的事件服务接收到一个块时，这称为块事件，

发生阻塞事件时要通知的步骤:

- 获取通道事件中心实例。
- 注册接收块。
- 将通道事件中心实例与 Peer 的事件服务连接。
- 在网络上的某个地方，交易被背书并提交。
  处理进来的块。
- 收听完毕后，断开通道事件中心。

#### 重播事件

如果希望查看已经发生的事件，请使用“ startBlock”选项来重播事件。 使用起始块将连接到 Peer 的事件服务，并使其开始发送以指定的块号而不是最新的块开头的现有块。 块将继续发送，直到看到"endBlock"为止。 如果未指定结束块，则将块添加到分类帐时将继续发送它们。 当您的应用程序离线时，重播可用于再次查找您的事务或链码事件。 当未指定结束块时，通道事件中心可能会继续用于监视新事件，因为新事件在赶上现有事件之后会在通道上发生。

发生重播事件时要通知的步骤:

- 获取通道事件中心实例。
- 注册以接收您的事件。
- 使用“ startBlock”将通道事件中心实例与 Peer 的事件服务连接
- 处理事件，使其进入。
- 收听完毕后，断开通道事件中心。

### 获取通道活动中心

使用 Fabric 客户端[Channel](https://hyperledger.github.io/fabric-sdk-node/release-1.4/Channel.html) [newChannelEventHub](https://hyperledger.github.io/fabric-sdk-node/release-1.4/Channel.html#newChannelEventHub)对象创建[ChannelEventHub](https://hyperledger.github.io/fabric-sdk-node/release-1.4/ChannelEventHub.html)对象的新实例。 使用以下命令获取 ChannelEventHub 实例，该实例将被设置为与 Peer 的基于通道的事件服务一起使用。 ChannelEventHub 实例将使用与对等实例相同的所有端点配置设置，例如 tls certs 以及主机和端口地址。

使用连接配置文件时([see](https://hyperledger.github.io/fabric-sdk-node/release-1.4/tutorial-network-config.html))，则可以使用 Peer 的名称来获取新的通道事件中心。

```javascript
// peer is a instance
const channel_event_hub = channel.newChannelEventHub(peer);

// using the peer name
const channel_event_hub = channel.newChannelEventHub("peer0.org1.example.com");
```

使用连接配置文件时(请参阅 [如何使用公用的公共连接配置文件(How to use a common common connection profile file)](https://hyperledger.github.io/fabric-sdk-node/release-1.4/tutorial-network-config.html))，则可以使用 Peer 的名称来获取通道事件中心。 每次调用"getChannelEventHub"时，它将返回相同的 ChannelEventHub 实例。

```javascript
// must use peer name
// 必须使用Peer名称
const channel_event_hub = channel.getChannelEventHub("peer0.org1.example.com");
```

这是使用连接配置文件时如何获取通道事件中心列表的示例。 以下将基于连接配置文件的当前活动 client 部分中定义的当前组织获取列表。 组织中定义的将 eventSource 设置为 true 的 Peer 将添加到列表中。

```javascript
const channel_event_hubs = channel.getChannelEventHubsForOrg();
```

创建 Peer 实例时，可以使用 Peer 实例获取 ChannelEventHub 实例。

```javascript
const data = fs.readFileSync(
  path.join(__dirname, "somepath/tlscacerts/org1.example.com-cert.pem")
);
const peer = client.newPeer("grpcs://localhost:7051", {
  pem: Buffer.from(data).toString(),
  "ssl-target-name-override": "peer0.org1.example.com"
});
const channel_event_hub = channel.newChannelEventHub(peer);
```

### 连接通道事件中心

一旦拥有 ChannelEventHub 实例，您将需要连接到 Peer 的事件服务。"connect"调用建立与 Peer 事件服务的连接。与 peer 的事件服务的连接必须指示要接收的块。默认情况下，ChannelEventHub 将指定最新的块作为起点。这通常是分类账上需要监视的点。用户可以指定起点和终点。当应用程序需要查看现有事务，链码事件或块时，指定"startBlock"很有用。连接调用可以在注册之前或之后进行，但是在进行连接调用后，开始块和结束块可能不会更改。与 Peer 的事件服务的连接还必须指示完整的块或已过滤的块。默认情况下，连接将被设置为接收已过滤的块，因为它包含事务状态并且不包含敏感数据。

最佳实践是在注册事务事件之前进行连接并提供回调。

```javascript
const channel_event_hub = ...

channel_event_hub.connect({full_block: false}, (err, status) => {
    if (err) {
        // process the error
    } else {
        // connect was good
    }
});

channel_event_hub.register...

```

最佳实践是在注册链码事件或块事件后进行连接。 之后的连接允许轻松地将连接修改为包括"startBlock"(用于重播)，而不更改流。 由于已过滤的块包含的信息很少，因此除非收到完整的块，否则链码事件和块事件可能没有用。 执行连接的用户必须具有访问权限才能查看完整的块。

```javascript
const channel_event_hub = ...

channel_event_hub.register...

channel_event_hub.connect({full_block: true}, (err, status) => {
    if (err) {
        // process the error
    } else {
        // connect was good
    }
});
```

通过重放，请注意，用户从先前的 ChannelEventHub 获取了起始块。

```javascript
const channel_event_hub = ...

const my_start = old_channel_event_hub.lastBlockNumber();

channel_event_hub.register...

channel_event_hub.connect({full_block: true, startBlock: my_start}, (err, status) => {
    if (err) {
        // process the error
    } else {
        // connect was good
    }
});
```

### 区块监听

当需要监视要添加到分类帐中的新块时，请使用块事件侦听器。当将新的块提交到 Peer 的分类帐时，将通知 Fabric 客户端 Node.js。然后，客户端 Node.js 将调用应用程序的注册回调。回调将传递新添加的块的 JSON 表示形式。请注意，当未使用 true 值调用 connect()时，回调将收到一个已过滤的块。注册接收完整块的用户的访问权限将由 Peer 基于通道的事件服务检查。当需要查看以前添加的块时，回调的注册可以包含一个起始块号。回调将从该编号开始接收块，并在将新块添加到分类账时继续接收新块。如果应用程序处于脱机状态，这是应用程序恢复和重播可能丢失的事件的一种方式。应用程序应记住它已处理的最后一个块，以避免重播整个分类帐。

以下示例将注册一个块侦听器，以开始接收新的块(将它们添加到分类帐中)。

```javascript
// keep the block_reg to unregister with later if needed
// 如有需要，请保留block_reg的注销权限
block_reg = channel_event_hub.registerBlockEvent((block) => {
    console.log('Successfully received the block event');
    <do something with the block>
}, (error)=> {
    console.log('Failed to receive the block event ::'+error);
    <do something with the error>
});
```

下面的示例将注册一个起始块号，因为此应用程序需要在特定的块处恢复并重播丢失的块。 应用程序回调将以与当前事件相同的方式处理重播的块。 块侦听器将在块被提交到 Peer 的分类账时继续接收它们。

```javascript
// keep the block_reg to unregister with later if needed
// 如有需要，请保留block_reg的注销权限
block_reg = channel_event_hub.registerBlockEvent((block) => {
    console.log('Successfully received the block event');
    <do something with the block>
}, (error)=> {
    console.log('Failed to receive the block event ::'+error);
    <do something with the error>
},
    {startBlock:23}
);
```

以下示例将注册一个起始块号和一个结束块。 应用程序需要重播错过的块。 应用程序回调将以与当前事件相同的方式处理重播的块。 当侦听器看到结束块事件时，该侦听器将自动取消注册，并且 ChannelEventHub 将关闭。 该应用程序将不必处理此内务处理。

```javascript
block_reg = channel_event_hub.registerBlockEvent((block) => {
    console.log('Successfully received a block event');
    <do something with the block>
    const event_block = Long.fromValue(block.header.number);
    if(event_block.equals(current_block)) {
        console.log('Successfully got the last block number');
        <application is now up to date>
    }
}, (error)=> {
    console.log('Failed to receive the block event ::'+error);
    <do something with the error>
},
    // for block listeners, the defaults for unregister and disconnect are true,
    // 对于块侦听器，取消注册和断开连接的默认设置为true，
    // so they are not required to be set in the following example
    // 因此以下示例中不需要设置它们
    {startBlock:23, endBlock:30, unregister: true, disconnect: true}
);
channel_event_hub.connect({full_block: true}); //get full blocks and no connect callback // 得到完整的块并且没有连接回调
```

以下示例将注册一个起始块号和一个设置为'newest'的终止块。错误回调将被调用，以通知应用程序已交付了最后一个块，并且侦听器已关闭。

```javascript
block_reg = channel_event_hub.registerBlockEvent((block) => {
    console.log('Successfully received the block event');
    <do something with the block>
}, (error)=> {
    if(error.toString().indexOf('Newest block received')) {
        console.log('Received latest block');
        <application is now up to date>
    } else {
        console.log('Failed to receive the block event ::'+error);
        <do something with the error>
    }

},
    {startBlock:23, endBlock:'newest'}
);
```

### 交易监听

当需要监视组织 peer 上的事务完成时，请使用事务侦听器。当新的块提交到 Peer 的分类帐时，将通知客户端 Node.js。然后，客户将在该块中检查已注册的交易标识符。如果找到事务，则将通过事务 ID，事务状态和块号通知回调。筛选的块包含事务状态，因此无需连接到 Peer 基于通道的事件服务即可接收完整的块。由于大多数非管理员用户将看不到完整的块，因此当这些用户仅需要侦听要提交的事务时，连接接收已过滤的块将避免访问问题。

以下示例将显示在 javascript Prommise 中注册交易 ID 并构建另一个将交易发送给 orderer 的 Promise。这两个 Promise 将一起执行，以便一起接收两个行动的结果。对于事务侦听器，注销的默背书选设置为 true。因此，在下面的示例中，已注册的侦听器将在看到事务后自动注销。

```javascript
let tx_object = client.newTransactionID();

// get the transaction ID string for later use
// 获取最后用户使用的交易ID字符串
let tx_id = tx_object.getTransactionID();

let request = {
    targets : targets,
    chaincodeId: 'my_chaincode',
    fcn: 'invoke',
    args: ['doSomething', 'with this data'],
    txId: tx_object
};

return channel.sendTransactionProposal(request);
}).then((results) => {
// a real application would check the proposal results
// 真实的应用程序将检查建议的结果
console.log('Successfully endorsed proposal to invoke chaincode');

// start block may be null if there is no need to resume or replay
// 如果不需要继续或重播，则开始块可能为空
let start_block = getBlockFromSomewhere();

let event_monitor = new Promise((resolve, reject) => {
    let handle = setTimeout(() => {
        // do the housekeeping when there is a problem
       //  有问题时进行housekeeping channel_event_hub.unregisterTxEvent(tx_id);
        console.log('Timeout - Failed to receive the transaction event');
        reject(new Error('Timed out waiting for block event'));
    }, 20000);

    channel_event_hub.registerTxEvent((event_tx_id, status, block_num) => {
        clearTimeout(handle);
        //channel_event_hub.unregisterTxEvent(event_tx_id); let the default do this
 // channel_event_hub.unregisterTxEvent(event_tx_id);让我们默认执行此操作
        console.log('Successfully received the transaction event');
        storeBlockNumForLater(block_num);
        resolve(status);
    }, (error)=> {
        clearTimeout(handle);
        console.log('Failed to receive the transaction event ::'+error);
        reject(error);
    },
        // when this `startBlock` is null (the normal case) transaction
       //当此'startBlock'为空(正常情况)时
       // checking will start with the latest block
       //检查将从最新的块开始
        {startBlock:start_block}
        // notice that `unregister` is not specified, so it will default to true
        // 注意未指定取消注册，因此默认为true
        // `disconnect` is also not specified and will default to false
        // 'disconnect'也未指定，并且默认为false
    );
    channel_event_hub.connect();
});
let send_trans = channel.sendTransaction({proposalResponses: results[0], proposal: results[1]});

return Promise.all([event_monitor, send_trans]);
}).then((results) => {
```

### Chaincode 事件监听器

当需要监视将在链码中发布的事件时，请使用链码事件侦听器。当新的块提交到分类账时，将通知客户端 Node.js。然后，客户端将在 chaincode 事件的名称字段中检查注册的 chaincode 模式。侦听器的注册包括一个正则表达式，该正则表达式将用于检查 chaincode 事件名称。如果发现链码事件名称与侦听器的正则表达式匹配，则将使用链码事件，块号，事务 ID 和事务状态来通知侦听器的回调。过滤后的块将没有链码事件有效负载信息；它只有链码事件名称。如果需要有效负载信息，则用户必须有权访问完整块，并且通道事件中心必须为 connect(true)才能从 Peer 基于通道的事件服务中接收完整块事件。

以下示例演示了如何在 javascript Promise 中注册 Chaincode 事件侦听器，并构建另一个将 Promise 发送到 Orderer 的 Promise。这两个 Peer 将一起执行，以便一起接收两个行动的结果。如果需要使用链码事件监听器进行长期监视，请遵循上面的块监听器示例。

```javascript
let tx_object = client.newTransactionID();
let request = {
    targets : targets,
    chaincodeId: 'my_chaincode',
    fcn: 'invoke',
    args: ['doSomething', 'with this data'],
    txId: tx_object
};

return channel.sendTransactionProposal(request);
}).then((results) => {
// a real application would check the proposal results
console.log('Successfully endorsed proposal to invoke chaincode');

// Build the promise to register a event listener with the NodeSDK.
// The NodeSDK will then send a request to the peer's channel-based event
// service to start sending blocks. The blocks will be inspected to see if
// there is a match with a chaincode event listener.
let event_monitor = new Promise((resolve, reject) => {
    let regid = null;
    let handle = setTimeout(() => {
        if (regid) {
            // might need to do the clean up this listener
            channel_event_hub.unregisterChaincodeEvent(regid);
            console.log('Timeout - Failed to receive the chaincode event');
        }
        reject(new Error('Timed out waiting for chaincode event'));
    }, 20000);

    regid = channel_event_hub.registerChaincodeEvent(chaincode_id.toString(), '^evtsender*',
        (event, block_num, txnid, status) => {
        // This callback will be called when there is a chaincode event name
        // within a block that will match on the second parameter in the registration
        // from the chaincode with the ID of the first parameter.
        console.log('Successfully got a chaincode event with transid:'+ txnid + ' with status:'+status);

        // might be good to store the block number to be able to resume if offline
        storeBlockNumForLater(block_num);

        // to see the event payload, the channel_event_hub must be connected(true)
        let event_payload = event.payload.toString('utf8');
        if(event_payload.indexOf('CHAINCODE') > -1) {
            clearTimeout(handle);
            // Chaincode event listeners are meant to run continuously
            // Therefore the default to automatically unregister is false
            // So in this case we want to shutdown the event listener once
            // we see the event with the correct payload
            channel_event_hub.unregisterChaincodeEvent(regid);
            console.log('Successfully received the chaincode event on block number '+ block_num);
            resolve('RECEIVED');
        } else {
            console.log('Successfully got chaincode event ... just not the one we are looking for on block number '+ block_num);
        }
    }, (error)=> {
        clearTimeout(handle);
        console.log('Failed to receive the chaincode event ::'+error);
        reject(error);
    }
        // no options specified
        // unregister will default to false
        // disconnect will default to false
    );
});

// build the promise to send the proposals to the orderer
let send_trans = channel.sendTransaction({proposalResponses: results[0], proposal: results[1]});

// now that we have two promises all set to go... execute them
return Promise.all([event_monitor, send_trans]);
}).then((results) => {
```

默认设置是一次接收一个链码事件，但是很难知道一个链码事件丢失了，并且很难在块中保持顺序。 使用新的选项 as_array，回调将接收在一个块中找到的所有链码事件作为数组。 下面的示例将使用一个将链代码作为数组处理的回调注册一个链代码侦听器，请注意，第五个参数是具有'as_array'true 设置的 options 对象。

```javascript
channel_event_hub.registerChaincodeEvent(
  "mychaincode",
  "myeventname",
  (...events) => {
    for (const { chaincode_event, block_num, tx_id, tx_status } of events) {
      /* process each event */
    }
  },
  err => {
    /* process err */
  },
  { as_array: true }
);
```

### 使用双向 tls 时

所有 Peer 和 Orderer 对象都需要为相互 TLS 连接使用相同的客户端凭据。 在将凭据用于创建 ChannelEventHub 创建中使用的 Peer 之前，必须将其分配给"client"对象实例。

```javascript
const client = new Client();
client.setTlsClientCertAndKey(tlsInfo.certificate, tlsInfo.key);

const channel = client.newChannel("mychannel");
const peer = client.newPeer("grpcs://localhost:7051", {
  pem: "<pem string here>",
  "ssl-target-name-override": "peer0.org1.example.com"
});
channel.addPeer(peer);
const channelEventHub = channel.newChannelEventHub(peer);
```

### 连接重播时

您的应用程序可能正在记录进入的块号，或者它可能使用另一个通道事件中心的最后一个块。 您的应用程序已脱机，现在希望赶上错过的块，然后继续处理新的块。 以下内容将在您选择的点将通道事件中心连接到 Peer 基于通道的事件服务，并且由于未指定 endBlock，因此它将继续接收添加到分类帐中的块。

注意:使用 ChannelEventHubs.lastBlockNumber()获取从先前运行的 ChannelEventHub 实例接收到的最后一个块的编号。

```javascript
const channel_event_hub = channel.newChannelEventHub(mypeer);

// be sure to register your listeners before calling `connect` or you may
// miss an event
channel_event_hub.registerBlockEvent(eventCallBack, errorCallBack, options);

const my_starting_point = this._calculate_starting_point(old_event_hub);

channel_event_hub.connect(
  { startBlock: my_starting_point },
  my_connect_call_back
);
```

### 重新连接时

您的应用程序具有运行时间较长的块侦听器或 chaincode 事件侦听器，并且您希望重新启动事件侦听。 下面的操作会将通道事件中心重新连接到 Peer 基于通道的事件服务，而不会打扰现有的事件侦听器。 连接将被设置为从通道事件中心看到的最后一个块开始发送块。 可以通过已经看到的块或事件来通知侦听器，这可以用于验证通知是否再次运行。

```javascript
channel_event_hub.reconnect({ startBlock: "last_seen" }, my_connect_call_back);
```

---
