# fabric-network.FileSystemWallet

## [fabric-network](https://hyperledger.github.io/fabric-sdk-node/release-1.4/module-fabric-network.html)~ FileSystemWallet

此类定义了持久保存在文件系统中的身份钱包的实现。

#### new FileSystemWallet(path [, mixin])

创建 FileSystemWallet 的实例。

- 属性

| 名称  | 类型        | 必要性 | 描述                                                                                                                                                      |
| ----- | ----------- | ------ | --------------------------------------------------------------------------------------------------------------------------------------------------------- |
| path  | string      |        | 该钱包在文件系统上的根路径                                                                                                                                |
| mixin | WalletMixin | 可选   | （可选）提供备用钱包混合。默认为[X509WalletMixin](https://hyperledger.github.io/fabric-sdk-node/release-1.4/module-fabric-network.X509WalletMixin.html)。 |

实现:

- [module:fabric-network.Wallet](https://hyperledger.github.io/fabric-sdk-node/release-1.4/module-fabric-network.Wallet.html)

---
