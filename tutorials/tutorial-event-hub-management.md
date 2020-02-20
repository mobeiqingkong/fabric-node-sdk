# fabric-network:如何自动选择并重新连接到事件中心(How to automatically select and reconnect to event hubs)

## fabric-network:如何自动选择并重新连接到事件中心(How to automatically select and reconnect to event hubs)

本教程描述了如何定义事件中心断开连接或需要新的事件中心时使用的事件中心选择策略的行为。

ChannelEventHub 是一个 fabric-client 类，它从 Peer 内的事件中心接收合约，提交和块事件。 Fabric 网络将事件中心抽象化，而使用事件中心选择策略来创建新的事件中心实例或重用现有实例。

下面是示例事件中心选择策略:

```javascript
class ExampleEventHubSelectionStrategy extends AbstractEventHubSelectionStrategy {
  constructor(peers) {
    this.peers = peers;
    this.disconnectedPeers = [];

    this.cleanupInterval = null;
  }
  _disconnectedPeerCleanup() {
    this.cleanupInterval = setInterval(() => {
      // Reset the list of disconnected peers every 10 seconds
      for (const peerRecord of disconnectedPeers) {
        // If 10 seconds has passed since the disconnect
        if (Date.now() - peerRecord.time > 10000) {
          this.disconnectedPeers = this.disconnectedPeers.filter(
            p => p !== peerRecord.peer
          );
        }
      }

      if (this.disconnectedPeers.length === 0) {
        clearInterval(this.cleanupInterval);
        this.cleanupInterval = null;
      }
    }, 10000);
  }
  /**
   * Returns the next peer in the list per the strategy implementation
   * @returns {ChannelPeer}
   */
  getNextPeer() {
    // Only select those peers that have not been disconnected recently
    let availablePeers = this.peers.filter(
      peer => this.disconnectedPeers.indexOf(peer) === -1
    );
    if (availablePeers.length === 0) {
      availablePeers = this.peers;
    }
    const randomPeerIdx = Math.floor(Math.random() * availablePeers.length);
    return availablePeers[randomPeerIdx];
  }

  /**
   * Updates the status of a peers event hub
   * @param {ChannelPeer} deadPeer The peer that needs its status updating
   */
  updateEventHubAvailability(deadPeer) {
    if (!this.cleanupInterval) {
      this._disconnectedPeerCleanup();
    }
    this.disconnectedPeers.push({ peer: deadPeer, time: Date.now() });
  }
}
```

事件中心策略存在于网关级别，并且以工厂功能的形式包含在 GatewayOptions 中。工厂为事件中心选择策略实例提供了可以从中选择事件中心的 Peer 列表。

```javascript
function EXAMPLE_EVENT_HUB_SELECTION_FACTORY(network) {
    const orgPeers = getOrganizationPeers(network);
    const eventEmittingPeers = filterEventEmittingPeers(orgPeers);
    return new ExampleEventHubSelectionStrategy(eventEmittingPeers);
}

const gateway = new Gateway();
await gateway.connect(connectionProfile, {
    ...
    eventHubSelectionOptions: {
        strategy: EXAMPLE_EVENT_HUB_SELECTION_FACTORY
    }
})
```

### 静态事件中心(Static event hub)

调用 [module:fabric-network.AbstractEventListener#setEventHub](https://hyperledger.github.io/fabric-sdk-node/release-1.4/module-fabric-network.AbstractEventListener.html#setEventHub) 允许您设置一个不会更改的事件中心。 在意外断开连接时，SDK 将尝试重新连接到该事件中心，而不是使用事件中心选择策略选择下一个 Peer。

---
