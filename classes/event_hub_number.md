# event_hub_number

## event_hub_number

#### new event_hub_number()

Transaction processing in fabric v1.1 is a long operation spanning multiple components (application, endorsing peer, orderer, committing peer) and takes a relatively lengthy period of time (think seconds instead of milliseconds) to complete. As a result the applications must design their handling of the transaction lifecycle in an asynchronous fashion. After the transaction proposal has been successfully [endorsed](https://hyperledger.github.io/fabric-sdk-node/release-1.4/Channel.html#sendTransactionProposal), and before the transaction message has been successfully [sent](https://hyperledger.github.io/fabric-sdk-node/release-1.4/Channel.html#sendTransaction) to the orderer, the application should register a listener to be notified when the transaction achieves finality, which is when the block containing the transaction gets added to the peer's ledger/blockchain.

Fabric committing peers provide a block delivery service to publish blocks or filtered blocks to connected fabric-clients. See [connect](https://hyperledger.github.io/fabric-sdk-node/release-1.4/ChannelEventHub.html#connect) on connection options and how this ChannelEventHub may connect to the fabric service. For more information on the service see [deliver](https://hyperledger-fabric.readthedocs.io/en/release-1.2/peer_event_services.html). A block gets published whenever the committing peer adds a validated block to the ledger. When a fabric-client receives a block it will investigate the block and notify interested listeners with the related contents of the block (e.g. transactionId, status). There are three types of listeners that will get notified by the fabric-client after it receives a published block from the fabric deliver service.

A "block listener" gets called for every block received. The listener will be passed a fully decoded [Block](https://hyperledger.github.io/fabric-sdk-node/release-1.4/global.html#Block) object unless the connection to the fabric service is using filtered blocks. See [registerBlockEvent](https://hyperledger.github.io/fabric-sdk-node/release-1.4/ChannelEventHub.html#registerBlockEvent)

A "transaction listener" gets called when the specific transaction is committed (discovered inside a published block). The listener may also be registered to listen to "all" transactions. The listener will be passed the transaction id, transaction status and block number. See [registerTxEvent](https://hyperledger.github.io/fabric-sdk-node/release-1.4/ChannelEventHub.html#registerTxEvent)

A "chaincode event listener" gets called when a specific chaincode event is discovered within a block. The listener will be passed the block number, transaction id, and transaction status. The [ChaincodeEvent](https://hyperledger.github.io/fabric-sdk-node/release-1.4/global.html#ChaincodeEvent) will be also be passed, however the payload of the event will not be passed if the connection to the fabric service is publishing filtered blocks. See [registerChaincodeEvent](https://hyperledger.github.io/fabric-sdk-node/release-1.4/ChannelEventHub.html#registerChaincodeEvent)

When the fabric-client connects to the peer, it tells the peer which block to begin delivering from. If no start block is provided, then the client will only receive events for the most recently committed block onwards. To avoid missing events in blocks that are published while the client is crashed/offline, the client should record the most recently processed block, and resume event delivery from this block number on startup. In this way, there is no custom recovery path for missed events, and the normal processing code may execute instead. You may also include an endBlock number if you wish to stop listening after receiving a range of events.

- 示例

```javascript
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
