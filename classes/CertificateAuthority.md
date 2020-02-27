# CertificateAuthority

## 说明

CertificateAuthority 类表示在连接配置文件中定义的证书颁发机构配置 。 当从 [Client#getCertificateAuthority](https://hyperledger.github.io/fabric-sdk-node/release-1.4/Client.html#getCertificateAuthority) 方法返回此类时，此类将作为 FabricCAServices 实例包装 FabricCAClientImpl fabric-ca-client 实现。此类具有与 [FabricCAServices](https://hyperledger.github.io/fabric-sdk-node/release-1.4/FabricCAServices.html) 相同的所有方法，因此可以直接使用此类，也可以使用此类的 [CertificateAuthority#getFabricCAServices](https://hyperledger.github.io/fabric-sdk-node/release-1.4/CertificateAuthority.html#getFabricCAServices) 方法来获取实际的 FabricCAServices 实例。

### new CertificateAuthority(name)

构造一个 CertificateAuthority 对象

- 参数

| 名称 | 类型   | 描述                 |
| ---- | ------ | -------------------- |
| name | string | 该证书颁发机构的名称 |

- 返回结果

  - 此 CertificateAuthority 的网址

  - 类型

    string

  - 此 CertificateAuthority 的网址

    - 类型

      [CertificateAuthority](https://hyperledger.github.io/fabric-sdk-node/release-1.4/CertificateAuthority.html)

### Methods

#### enroll()

见 [FabricCAServices#enroll](https://hyperledger.github.io/fabric-sdk-node/release-1.4/FabricCAServices.html#enroll)

#### generateCRL()

见 [FabricCAServices#generateCRL](https://hyperledger.github.io/fabric-sdk-node/release-1.4/FabricCAServices.html#generateCRL)

#### getCaName()

获取要在请求中使用的此 CertificateAuthority 的名称

- 返回结果

  - 此 CertificateAuthority 的 ca 名称

  - 类型

    string

#### getConnectionOptions()

获取此 CertificateAuthority 的连接选项

返回结果

- 此 CertificateAuthority 的连接选项

- 类型

  object

#### getFabricCAServices()

获取 FabricCAServices 的实现 ( implementation )

返回结果

- [FabricCAServices](https://hyperledger.github.io/fabric-sdk-node/release-1.4/FabricCAServices.html)

#### getName()

获取此 CertificateAuthority 的名称

返回结果

- 此 CertificateAuthority 的名称

- 类型

  string

#### getRegistrar()

获取此 CertificateAuthority 的注册商

返回结果

- 此 CertificateAuthority 的注册商

  - 类型

    object

#### getTlsCACerts()

获取此 CertificateAuthority 的 TLS CA 证书

返回结果

- 此 CertificateAuthority 的 TLS CA Cert PEM 字符串

- 类型

  string

#### getUrl()

获取此 CertificateAuthority 的 URL

返回结果

- 此 CertificateAuthority 的 URL

  - 类型

    string

#### newAffiliationService()

见 [FabricCAServices#newAffiliationService](https://hyperledger.github.io/fabric-sdk-node/release-1.4/FabricCAServices.html#newAffiliationService)

#### newCertificateService()

见 [FabricCAServices#newCertificateService](https://hyperledger.github.io/fabric-sdk-node/release-1.4/FabricCAServices.html#newCertificateService)

#### newIdentityService()

见 [FabricCAServices#newIdentityService](https://hyperledger.github.io/fabric-sdk-node/release-1.4/FabricCAServices.html#newIdentityService)

#### reenroll()

见 [FabricCAServices#reenroll](https://hyperledger.github.io/fabric-sdk-node/release-1.4/FabricCAServices.html#reenroll)

#### register()

见 [FabricCAServices#register](https://hyperledger.github.io/fabric-sdk-node/release-1.4/FabricCAServices.html#register)

#### revoke()

见 [FabricCAServices#revoke](https://hyperledger.github.io/fabric-sdk-node/release-1.4/FabricCAServices.html#revoke)

#### setFabricCAServices(ca_services)

设置 FabricCAServices 实现

- 参数

| 名称        | 类型                                                                                                | 描述                                                                                                |
| ----------- | --------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------- |
| ca_services | [FabricCAServices](https://hyperledger.github.io/fabric-sdk-node/release-1.4/FabricCAServices.html) | [FabricCAServices](https://hyperledger.github.io/fabric-sdk-node/release-1.4/FabricCAServices.html) |

#### toString()

返回此对象的可打印表示形式

---
