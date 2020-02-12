# fabric-network.InMemoryWallet

## [fabric-network](https://hyperledger.github.io/fabric-sdk-node/release-1.4/module-fabric-network.html)~ InMemoryWallet

内存钱包实现。请注意，在给定的Node.js进程中，此类的所有实例之间共享内存状态。

#### new InMemoryWallet( [walletmixin])

创建一个内存中钱包的实例。

- 属性

| 名称        | 类型        | 必要性 | 描述                                                         |
| ----------- | ----------- | ------ | ------------------------------------------------------------ |
| walletmixin | WalletMixin | 可选   | （可选）提供备用钱包混合。默认为[X509WalletMixin](https://hyperledger.github.io/fabric-sdk-node/release-1.4/module-fabric-network.X509WalletMixin.html)。 |

实现:

- [module:fabric-network.Wallet](https://hyperledger.github.io/fabric-sdk-node/release-1.4/module-fabric-network.Wallet.html)