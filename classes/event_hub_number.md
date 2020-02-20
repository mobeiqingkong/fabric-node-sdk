# event_hub_number

## event_hub_number

#### new event_hub_number()

Fabric v1.1 中的事务处理是一项跨越多个组件(应用程序，背书peer，orderer，提交对等端)的漫长操作，并且需要相对较长的时间段(以秒为单位，而不是毫秒)来完成。结果，应用程序必须以异步方式设计其对事务生命周期的处理。在成功许可([endorsed](https://hyperledger.github.io/fabric-sdk-node/release-1.4/Channel.html#sendTransactionProposal))交易建议之后，并且在成功将交易消息发送([sent](https://hyperledger.github.io/fabric-sdk-node/release-1.4/Channel.html#sendTransaction))给 orderer 之前，应用程序应该注册一个侦听器，以便在交易完成时获得通知，即交易的区块被添加到peer的分类账/区块链中。

Fabric 提交 Peer 提供块传递服务，以将块或已过滤的块发布到连接的 Fabric 客户端。请参阅连接([connect](https://hyperledger.github.io/fabric-sdk-node/release-1.4/ChannelEventHub.html#connect) )连接选项以及此 ChannelEventHub 如何连接到 Fabric 服务。有关该服务的更多信息，请参见交付( [deliver](https://hyperledger-fabric.readthedocs.io/en/release-1.2/peer_event_services.html))。只要提交 Peer 将经过验证的块添加到分类帐中，就会发布一个块。当 Fabric 客户端收到一个块时，它将调查该块并将有关的内容通知感兴趣的侦听器(例如 transactionId，状态)。 Fabric-client 接收到来自 Fabric 传递服务的已发布块后，会收到三种类型的侦听器通知。

- 每个接收到的块都会调用一个“块侦听器”。除非与 Fabric 服务的连接使用已过滤的块，否则将向侦听器传递一个完全解码的 [Block](https://hyperledger.github.io/fabric-sdk-node/release-1.4/global.html#Block) 对象。参见[registerBlockEvent](https://hyperledger.github.io/fabric-sdk-node/release-1.4/ChannelEventHub.html#registerBlockEvent)
- 提交特定事务(在已发布的块内发现)时，将调用“事务侦听器”。侦听器也可以注册为侦听“所有”事务。侦听器将被传递交易 ID，交易状态和区块编号。参见[registerTxEvent](https://hyperledger.github.io/fabric-sdk-node/release-1.4/ChannelEventHub.html#registerTxEvent)
- 当在块中发现特定的链码事件时，将调用“链码事件侦听器”。将向侦听器传递块号，事务 ID 和事务状态。 [ChaincodeEvent](https://hyperledger.github.io/fabric-sdk-node/release-1.4/global.html#ChaincodeEvent) 也将被传递，但是，如果与Fabric服务的连接正在发布已过滤的块，则不会传递事件的有效负载。参见[registerChaincodeEvent](https://hyperledger.github.io/fabric-sdk-node/release-1.4/ChannelEventHub.html#registerChaincodeEvent)

当 Fabric 客户端连接到 Peer 时，它告诉 Peer 从哪个块开始交付。如果未提供起始块，则客户端将仅接收最近提交的块开始的事件。为避免客户端崩溃/脱机时发布的块中丢失事件，客户端应记录最近处理的块，并在启动时从该块号恢复事件传递。这样，就没有丢失事件的自定义恢复路径，而可以执行常规处理代码。如果您希望在收到一系列事件后停止收听，还可以包含一个 endBlock 编号。

##### 示例

```javascript
const eh = channel.newChannelEventHub(peer);

// register the listener before calling "connect()" so there
// is an error callback ready to process an error in case the
// connect() call fails
eh.registerTxEvent(
  "all", // this listener will be notified of all transactions
  (tx, status, block_num) => {
    record(tx, status, block_num);
    console.log(util.format("Transaction %s has completed", tx));
  },
  err => {
    eh.unregisterTxEvent("all");
    reportError(err);
    console.log(
      util.format(
        "Error %s! Transaction listener has been " + "deregistered for %s",
        err,
        eh.getPeerAddr()
      )
    );
  }
);

eh.connect();
```

---
