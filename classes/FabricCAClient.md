# FabricCAClient

## (未完成)FabricCAClient

与 Fabric CA API 通信的客户端

#### new FabricCAClient(connect_opts)

constructor

- 参数

| 名称         | 类型   | 描述                                            |
| :----------- | :----- | ----------------------------------------------- |
| connect_opts | object | 与 Fabric CA 服务器通信的连接选项，属性表见下表 |

- 属性表

| 名称       | 类型                                                                                           | 描述                                                                                                                     |
| :--------- | :--------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------ |
| protocol   | string                                                                                         | 使用的协议(HTTP 或 HTTPS)                                                                                              |
| hostname   | string                                                                                         | Fabric CA 服务器终结点的主机名                                                                                           |
| port       | number                                                                                         | Fabric CA 服务器端点的端口                                                                                               |
| tlsOptions | [TLSOptions](https://hyperledger.github.io/fabric-sdk-node/release-1.4/global.html#TLSOptions) | Fabric CA 端点使用“ https”时要使用的 TLS 设置                                                                            |
| caname     | string                                                                                         | CA 的可选名称。 Fabric-ca 服务器从单个服务器支持多个证书颁发机构。如果省略，为 null 或为空字符串，则默认 CA 为请求的目标 |

##### 抛出

如果连接选项丢失或无效，将引发错误

### Methods

#### enroll(enrollmentID, enrollmentSecret, csr, profile, attr_reqs)

注册注册用户以接收签名的 X509 证书

- 参数

| 名称             | 类型                                                                                                                     | 描述                                                                                                             |
| :--------------- | :----------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------- |
| enrollmentID     | string                                                                                                                   | 用于注册的注册 ID                                                                                                |
| enrollmentSecret | string                                                                                                                   | 与注册 ID 关联的 secret                                                                                          |
| csr              | string                                                                                                                   | PEM 编码的 PKCS#10 证书签名请求                                                                                  |
| profile          | string                                                                                                                   | 配置文件名称。为 TLS 证书指定“ tls”配置文件；否则，将颁发注册证书。                                              |
| attr_reqs        | Array.&lt;[AttributeRequest](https://hyperledger.github.io/fabric-sdk-node/release-1.4/global.html#AttributeRequest)&gt; | [AttributeRequest](https://hyperledger.github.io/fabric-sdk-node/release-1.4/global.html#AttributeRequest)的数组 |

抛出

- 如果未提供所有参数，将引发错误
- 如果由于任何原因调用 enroll API 失败将引发错误

返回结果

- [EnrollmentResponse](https://hyperledger.github.io/fabric-sdk-node/release-1.4/global.html#EnrollmentResponse)

  - 类型

    Promise

#### newAffiliationService()

创建一个新的[AffiliationService](https://hyperledger.github.io/fabric-sdk-node/release-1.4/AffiliationService.html)实例

返回结果

- 实例

  - 类型

    [AffiliationService](https://hyperledger.github.io/fabric-sdk-node/release-1.4/AffiliationService.html)

#### newCertificateService()

创建一个新的 CertificateService 实例

返回结果

- 实例

  - 类型

    CertificateService

#### newIdentityService()

创建一个新的 [IdentityService](https://hyperledger.github.io/fabric-sdk-node/release-1.4/IdentityService.html) 实例

返回结果

- 实例

  - 类型

    [IdentityService](https://hyperledger.github.io/fabric-sdk-node/release-1.4/IdentityService.html)

#### reenroll(csr, signingIdentity, attr_reqs)

重新注册现有用户。

- 参数

| 名称            | 类型                                                                                                                     | 描述                                                                                                             |
| :-------------- | :----------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------- |
| csr             | string                                                                                                                   | PEM 编码的 PKCS#10 证书签名请求                                                                                  |
| signingIdentity | [SigningIdentity](https://hyperledger.github.io/fabric-sdk-node/release-1.4/SigningIdentity.html)                        | 封装签名证书，哈希算法和签名算法的 SigningIdentity 实例                                                          |
| attr_reqs       | Array.&lt;[AttributeRequest](https://hyperledger.github.io/fabric-sdk-node/release-1.4/global.html#AttributeRequest)&gt; | [AttributeRequest](https://hyperledger.github.io/fabric-sdk-node/release-1.4/global.html#AttributeRequest)的数组 |

返回结果

- [EnrollmentResponse](https://hyperledger.github.io/fabric-sdk-node/release-1.4/global.html#EnrollmentResponse)

  - 类型

    Promise

#### register(enrollmentID, enrollmentSecret, role, affiliation, maxEnrollments, attrs, signingIdentity)

注册新用户并返回注册 secret

- 参数

| 名称             | 类型                                                                                                                       | 描述                                                                                          |
| :--------------- | :------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------- |
| enrollmentID     | string                                                                                                                     | 将用于注册的 ID                                                                               |
| enrollmentSecret | string                                                                                                                     | 为注册用户设置的可选注册密码。如果未提供，则服务器将生成一个。不包括时，请为此参数使用 null。 |
| role             | string                                                                                                                     | 此用户的可选角色类型。不包括时，请为此参数使用 null。                                         |
| affiliation      | string                                                                                                                     | 与该用户关联的关联关系                                                                        |
| maxEnrollments   | number                                                                                                                     | 允许用户注册的最大次数                                                                        |
| attrs            | Array.&lt;[KeyValueAttribute](https://hyperledger.github.io/fabric-sdk-node/release-1.4/global.html#KeyValueAttribute)&gt; | 要分配给用户的键/值属性数组                                                                   |
| signingIdentity  | [SigningIdentity](https://hyperledger.github.io/fabric-sdk-node/release-1.4/SigningIdentity.html)                          | 封装签名证书，哈希算法和签名算法的 SigningIdentity 实例                                       |

返回结果

- 该用户注册时使用的注册 secret

  - 类型

    Promise

#### revoke(enrollmentID, aki, serial, reason, signingIdentity)

吊销现有证书(注册证书或交易证书)，或吊销颁发给注册 ID 的所有证书。如果吊销特定证书，则同时需要授权密钥标识符和序列号。如果按注册 ID 撤销，则以后所有注册此 ID 的请求都将被拒绝。

- 参数

| 名称            | 类型                                                                                              | 描述                                                                     |
| :-------------- | :------------------------------------------------------------------------------------------------ | ------------------------------------------------------------------------ |
| enrollmentID    | string                                                                                            | 要撤消的 ID                                                              |
| aki             | string                                                                                            | 授权密钥标识符字符串，十六进制编码，用于吊销特定证书                     |
| serial          | string                                                                                            | 十六进制编码的序列号字符串，用于撤销特定的证书                           |
| reason          | string                                                                                            | 撤销的原因。有关有效值，请参见<https://godoc.org/golang.org/x/crypto/ocsp> |
| signingIdentity | [SigningIdentity](https://hyperledger.github.io/fabric-sdk-node/release-1.4/SigningIdentity.html) | 封装签名证书，哈希算法和签名算法的 SigningIdentity 实例                  |

返回结果

- 吊销结果

  - 类型

    Promise

---
