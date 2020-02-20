# fabric-network.CouchDBWallet

## [fabric-network](https://hyperledger.github.io/fabric-sdk-node/release-1.4/module-fabric-network.html)~ CouchDBWallet

此类定义了持久存储在 Couch DB 数据库中的 Identity 钱包的实现

#### new CouchDBWallet(options [, mixin])

创建 CouchDBWallet 的实例

- 参数

| 名称    | 类型                                                                                                                                                                                 | 必要性 | 描述                                                                                                                                                      |
| ------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ------ | --------------------------------------------------------------------------------------------------------------------------------------------------------- |
| options | [module:fabric-network.CouchDBWallet~CouchDBWalletOptions](https://hyperledger.github.io/fabric-sdk-node/release-1.4/module-fabric-network.CouchDBWallet.html#~CouchDBWalletOptions) |        | 包含必需的属性 url 和其他 Nano 选项                                                                                                                       |
| mixin   | WalletMixin                                                                                                                                                                          | 可选   | (可选)提供备用钱包混合。默认为[X509WalletMixin](https://hyperledger.github.io/fabric-sdk-node/release-1.4/module-fabric-network.X509WalletMixin.html)。 |

实现

- [module:fabric-network.Wallet](https://hyperledger.github.io/fabric-sdk-node/release-1.4/module-fabric-network.Wallet.html)

### 类型定义

#### CouchDBWalletOptions

类型

- Object

- 属性

| 名称 | 类型   | 描述           |
| ---- | ------ | -------------- |
| url  | string | CouchDB 的 URL |

---
