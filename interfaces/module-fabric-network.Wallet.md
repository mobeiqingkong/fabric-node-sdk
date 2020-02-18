# fabric-network.Wallet

## [fabric-network](https://hyperledger.github.io/fabric-sdk-node/release-1.4/module-fabric-network.html). Wallet

电子钱包定义了用于在 Fabric 网络中存储和管理用户身份的接口。这是一个抽象基类，必须进行扩展。

### Methods

#### &lt;async&gt; delete(label)

从钱包(wallet)中删除身份(identity)

- 参数

| 名称  | 类型   | 描述 |
| ----- | ------ | ---- |
| label | string |      |

#### &lt;async&gt; exists(label)

查询钱包中是否存在身份

- 参数

| 名称  | 类型   | 描述 |
| ----- | ------ | ---- |
| label | string |      |

返回结果

- 类型

  boolean

#### &lt;async&gt; export(label)

从钱包中提取身份

- 参数:

| Name  | Type   | Description |
| :---- | :----- | :---------- |
| label | string |             |

返回结果

- 类型

  [module:fabric-network.Wallet~Identity](https://hyperledger.github.io/fabric-sdk-node/release-1.4/module-fabric-network.Wallet.html#~Identity)

#### &lt;async&gt; import(label, identity)

将身份导入钱包

- 参数:

| Name     | Type                                                                                                                                            | Description |
| :------- | :---------------------------------------------------------------------------------------------------------------------------------------------- | ----------- |
| label    | string                                                                                                                                          |             |
| identity | [module:fabric-network.Wallet~Identity](https://hyperledger.github.io/fabric-sdk-node/release-1.4/module-fabric-network.Wallet.html#~Identity) |             |

#### &lt;async&gt; list()

列出钱包的内容

返回结果

- 类型

  Array.&lt;[module:fabric-network.Wallet~IdentityInfo](https://hyperledger.github.io/fabric-sdk-node/release-1.4/module-fabric-network.Wallet.html#~IdentityInfo)&gt;

### Type Definitions

#### Identity

- 类型

  Object

- 参数

- 参数:

| Name        | Type   | Description                 |
| :---------- | :----- | --------------------------- |
| type        | string | 凭据的类型，例如 X509。     |
| mspId       | string | 此身份所属的组织单位。      |
| certificate | string | 包含 PEM 格式的公钥的证书。 |
| privateKey  | string | PEM 格式的私钥。            |

#### IdentityInfo

- 类型

  Object

- 参数:

| Name       | Type   | Description                    |
| :--------- | :----- | ------------------------------ |
| label      | string | 用于在钱包中引用此身份的标签。 |
| mspId      | string | 此身份所属的组织单位。         |
| identifier | string |                                |

---
