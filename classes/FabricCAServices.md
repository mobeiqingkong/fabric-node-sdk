# FabricCAServices

## FabricCAServices

这是与 Fabric CA 服务器通信的成员服务客户端的实现。

#### new FabricCAServices(url, tlsOptions, caName, cryptoSuite)

constructor

- 参数

| 名称        | 类型                                                                                           | 描述                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         |
| ----------- | ---------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| url         | string &#124; object                                                                           | Fabric CA 服务的端点 URL，格式为:"<http://host:port"或"https://host:port"。如果此参数是一个对象，则它必须包括列为键值对的参数。>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |
| tlsOptions  | [TLSOptions](https://hyperledger.github.io/fabric-sdk-node/release-1.4/global.html#TLSOptions) | Fabric CA 服务端点使用“ https”时要使用的 TLS 设置                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            |
| caName      | string                                                                                         | CA 的可选名称。 Fabric-ca 服务器从单个服务器支持多个证书颁发机构。如果省略，为 null 或为空字符串，则默认 CA 为请求的目标                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     |
| cryptoSuite | CryptoSuite                                                                                    | 如果需要除默认值以外的其他选项，则使用可选的 cryptoSuite 实例。如果未指定，则将基于当前配置设置构造 CryptoSuite 实例：<br>-crypto-hsm：使用硬件安全模块（如果设置为 true）或基于软件的密钥管理（如果设置为 false）的实现<br>-crypto-keysize：与数字签名公钥算法一起使用的安全级别或密钥大小。当前支持 ECDSA，有效密钥大小为 256 和 384<br> -crypto-hash-algo：哈希算法<br>-密钥值存储：某些 CryptoSuite 实现需要密钥存储来保留私钥。为此提供了一个[CryptoKeyStore](https://hyperledger.github.io/fabric-sdk-node/release-1.4/CryptoKeyStore.html)，它可以在 KeyValueStore 接口的任何实现之上使用，例如基于文件的存储或基于数据库的存储。具体的实现方式取决于此配置设置的值。 |

### 扩展

- [BaseClient](https://hyperledger.github.io/fabric-sdk-node/release-1.4/BaseClient.html)

### Methods

#### &lt;async&gt; enroll(req)

注册成员并返回一个不透明的成员对象。

- 参数

| 名称 | 类型 | 描述                                                                                                                                                                                                                             |
| ---- | ---- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| req  |      | [EnrollmentRequest](https://hyperledger.github.io/fabric-sdk-node/release-1.4/global.html#EnrollmentRequest)，如果请求包含字段“ csr”，则此 csr 将用于从 Fabric-CA 获取证书。否则，将生成一个新的私钥，并在以后用于生成一个 csr。 |

返回结果

- 如果请求不包含字段“ csr”，则返回的 Promise 将为新生成的私钥解析带有“ key”的[Enrollment](https://hyperledger.github.io/fabric-sdk-node/release-1.4/global.html#Enrollment) 对象。如果请求包含字段“ csr”，则解析的[Enrollment](https://hyperledger.github.io/fabric-sdk-node/release-1.4/global.html#Enrollment) 对象不包含属性“ key”。

  - 类型

    Promise.&lt;[Enrollment](https://hyperledger.github.io/fabric-sdk-node/release-1.4/global.html#Enrollment)&gt;

#### generateCRL(request, registrar)

- 参数

| 名称      | 类型                                                                        | 描述                               |
| --------- | --------------------------------------------------------------------------- | ---------------------------------- |
| request   | [Restriction                                                                |                                    |
| registrar | [User](https://hyperledger.github.io/fabric-sdk-node/release-1.4/User.html) | 注册服务商的身份（即执行撤销的人） |

返回结果

- 证书吊销列表（CRL）。

  - 类型

    Promise

#### getCaName()

返回证书颁发机构的名称。

返回结果

- caName。

  - 类型

    string

#### getCryptoSuite()

返回此客户端实例使用的 CryptoSuite 对象

继承自(Inherited From)：

- [BaseClient#getCryptoSuite](https://hyperledger.github.io/fabric-sdk-node/release-1.4/BaseClient.html#getCryptoSuite)

覆写(Overrides:):

- [BaseClient#getCryptoSuite](https://hyperledger.github.io/fabric-sdk-node/release-1.4/BaseClient.html#getCryptoSuite)

返回结果

- 类型

  [module:api.CryptoSuite](https://hyperledger.github.io/fabric-sdk-node/release-1.4/module-api.CryptoSuite.html)

#### newAffiliationService()

创建一个新的 [AffiliationService](https://hyperledger.github.io/fabric-sdk-node/release-1.4/AffiliationService.html) 对象

返回结果

- Object

  - 类型

    [AffiliationService](https://hyperledger.github.io/fabric-sdk-node/release-1.4/AffiliationService.html)

#### newCertificateService()

创建一个新的 CertificateService 实例

返回结果

- Object

  - 类型

    CertificateService

#### newIdentityService()

创建一个新的[IdentityService](https://hyperledger.github.io/fabric-sdk-node/release-1.4/IdentityService.html)对象

返回结果

- Object

  - 类型

    [IdentityService](https://hyperledger.github.io/fabric-sdk-node/release-1.4/IdentityService.html)

#### reenroll(currentUser, Optional)

在现有注册证书即将过期或已被泄露的情况下，重新注册成员

- 参数

| 名称        | 类型                                                                                                                     | 描述                                                                                                                                       |
| ----------- | ------------------------------------------------------------------------------------------------------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------ |
| currentUser | [User](https://hyperledger.github.io/fabric-sdk-node/release-1.4/User.html)                                              | 拥有现有注册证书的当前用户的身份                                                                                                           |
| Optional    | Array.&lt;[AttributeRequest](https://hyperledger.github.io/fabric-sdk-node/release-1.4/global.html#AttributeRequest)&gt; | [AttributeRequest](https://hyperledger.github.io/fabric-sdk-node/release-1.4/global.html#AttributeRequest)的数组，指示要包含在证书中的属性 |

返回结果

- 一个对象的 Promise，其中“ key”代表私钥，“ certificate”代表签名证书

#### register(req, registrar)

注册会员并返回注册 secret。

- 参数

| 名称      | 类型                                                                                                     | 描述                                                                                                     |
| --------- | -------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------- |
| req       | [RegisterRequest](https://hyperledger.github.io/fabric-sdk-node/release-1.4/global.html#RegisterRequest) | [RegisterRequest](https://hyperledger.github.io/fabric-sdk-node/release-1.4/global.html#RegisterRequest) |
| registrar | [User](https://hyperledger.github.io/fabric-sdk-node/release-1.4/User.html)                              | 注册商的身份（即执行注册的人）                                                                           |

返回结果

- 该用户注册时使用的注册 secret

  - 类型

    Promise

#### revoke(request, registrar)

吊销现有证书（注册证书或交易证书），或吊销颁发给注册 ID 的所有证书。如果撤销特定证书，然后必须输入授权密钥标识符和序列号。如果按注册 ID 撤销，则以后所有注册此 ID 的请求都将被拒绝。

- 参数

| 名称      | 类型                                                                        | 描述                                                                                                                                                                                                                                                                                                                                     |
| --------- | --------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| request   | Object                                                                      | 请求对象具有以下字段： <br>-enrollmentID {字符串}。要撤消的 ID <br>-aki {string}。授权密钥标识符字符串，十六进制编码，用于吊销特定证书 <br>-serial{string}。十六进制编码的序列号字符串，用于撤销特定的证书<br>-reason{string}。撤销的原因。有关有效值，请参见<https://godoc.org/golang.org/x/crypto/ocsp。默认值为0(ocsp.Unspecified)。> |
| registrar | [User](https://hyperledger.github.io/fabric-sdk-node/release-1.4/User.html) | 注册服务商的身份（即执行撤销的人）                                                                                                                                                                                                                                                                                                       |

返回结果

- 吊销结果

  - 类型

    Promise

#### setCryptoSuite(cryptoSuite)

设置客户端实例以使用 CryptoSuite 对象进行签名和哈希创建和设置 CryptoSuite 是可选的，因为客户端将基于默认配置设置构造实例：

- crypto-hsm：使用硬件安全模块（如果设置为 true）或基于软件的密钥管理（如果设置为 false）的实现
- crypto-keysize：与数字签名公钥算法一起使用的安全级别或密钥大小。当前支持 ECDSA，有效密钥大小为 256 和 384
- crypto-hash-algo：哈希算法
- 密钥值存储：某些 CryptoSuite 实现需要密钥存储来保留私钥。为此提供了一个 [CryptoKeyStore](https://hyperledger.github.io/fabric-sdk-node/release-1.4/CryptoKeyStore.html) ，它可以在 KeyValueStore 接口的任何实现之上使用，例如基于文件的存储或基于数据库的存储。具体的实现方式取决于此配置设置的值。

- 参数

| 名称        | 类型                                                                                                            | 描述             |
| ----------- | --------------------------------------------------------------------------------------------------------------- | ---------------- |
| cryptoSuite | [module:api.CryptoSuite](https://hyperledger.github.io/fabric-sdk-node/release-1.4/module-api.CryptoSuite.html) | CryptoSuite 对象 |

继承自(Inherited From)：

- [BaseClient#setCryptoSuite](https://hyperledger.github.io/fabric-sdk-node/release-1.4/BaseClient.html#setCryptoSuite)

覆写(Overrides:):

- [BaseClient#setCryptoSuite](https://hyperledger.github.io/fabric-sdk-node/release-1.4/BaseClient.html#setCryptoSuite)

#### toString()

返回此对象的可打印表示形式

---
