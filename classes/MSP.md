# MSP

## MSP

MSP 是由不同算法（ECDSA，RSA 等）和 PKI（基于软件管理或基于 HSM）生成的私钥和证书 用于管理身份（在签名和签名验证方面）的最小成员资格服务提供商接口。

#### new MSP(config)

根据配置信息设置 MSP 实例

- 参数

| 名称   | 类型   | 描述                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
| ------ | ------ | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| config | Object | 特定于实现的配置对象。对于此实现，它使用以下字段：<br>rootCerts：表示信任锚的[Identity](https://hyperledger.github.io/fabric-sdk-node/release-1.4/global.html#Identity)数组，用于验证签名证书。验证签名中使用的 MSP 必需<br>intermediateCerts：[Identity](https://hyperledger.github.io/fabric-sdk-node/release-1.4/global.html#Identity)的数组，表示用于验证签名证书的信任锚。用于验证签名的 MSP 可选<br>admins：代表管理员权限的[Identity](https://hyperledger.github.io/fabric-sdk-node/release-1.4/global.html#Identity)数组<br>signer：[SigningIdentity](https://hyperledger.github.io/fabric-sdk-node/release-1.4/SigningIdentity.html)签名身份。用于签名的 MSP 必需<br>id：此实例的标识符的{string}值<br>orgs：组织单位标识符的{string}数组<br>cryptoSuite：用于加密基元操作的基础 [module:api.CryptoSuite](https://hyperledger.github.io/fabric-sdk-node/release-1.4/module-api.CryptoSuite.html) |

### Methods

#### deserializeIdentity(serializedIdentity, storeKey)

- 参数

| 名称               | 类型               | 描述                                                                           |
| ------------------ | ------------------ | ------------------------------------------------------------------------------ |
| serializedIdentity | Array.&lt;byte&gt; | 具有两个字段的对象的基于 Protobuf 的序列化：mspid 和 idBytes 用于证书 PEM 字节 |
| storeKey           | boolean            | 用户是否应该存储在密钥库中。只有当错误时，诺言才会被退回                       |

返回结果

- [Identity](https://hyperledger.github.io/fabric-sdk-node/release-1.4/global.html#Identity) 实例的 Promise 或 Identity 对象本身（如果“ storeKey”参数为 false）

- 类型

  Promise

#### getDefaultSigningIdentity()

返回默认的签名身份

返回结果

- 类型

  [SigningIdentity](https://hyperledger.github.io/fabric-sdk-node/release-1.4/SigningIdentity.html)

#### getId()

获取提供者标识符

返回结果

- 类型

  string

#### getOrganizationUnits()

获取组织单位标识符

返回结果

- 类型

  Array.&lt;string&gt;

#### getPolicy()

获得控制变更的政策

返回结果

- 类型

  Object

#### getSigningIdentity(identifier)

返回与提供的标识符相对应的签名身份

- 参数

| 名称       | 类型   | 描述                   |
| ---------- | ------ | ---------------------- |
| identifier | string | 请求的身份对象的标识符 |

返回结果

- 类型

  [SigningIdentity](https://hyperledger.github.io/fabric-sdk-node/release-1.4/SigningIdentity.html)

#### toProtobuf()

返回此 MSP Config 的 Protobuf 表示形式

#### validate(id)

检查提供的身份是否有效

- 参数

| 名称 | 类型                                                                                       | 描述 |
| ---- | ------------------------------------------------------------------------------------------ | ---- |
| id   | [Identity](https://hyperledger.github.io/fabric-sdk-node/release-1.4/global.html#Identity) |      |

返回结果

- 类型

  boolean

---
