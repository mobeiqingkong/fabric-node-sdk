# User

## 说明

User 类代表已注册的用户，并由注册证书(ECert)和签名密钥表示。注册用户(具有签名密钥和 ECert)可以使用 Channel 进行链码实例化，事务处理和查询。可以在安装和实例化应用程序之前预先从 CA 获得用户 ECert，也可以通过其注册过程从可选的 Fabric CA 服务获得用户 ECert。有时，用户身份与 peer 身份混淆。用户身份代表签名功能，因为它可以访问私钥，而在应用程序/ SDK 上下文中的对等身份仅具有用于验证签名的证书。应用程序无法使用 peer 身份进行签名，因为该应用无权访问 peer 身份的私钥。

### new User(cfg)

成员的构造函数。

- 参数

| 名称 | 类型   | 描述                                                                                                                                                                                                                                 |
| ---- | ------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| cfg  | string | 成员名称或具有以下属性的对象:-enrollmentID {string}:user name - name {string}: user name,如果还指定了"enrollmentID"，则将忽略"name"，\- roles {string[]}:可选。array of roles - affiliation {string}:可选。与团体或组织的隶属关系 |

### Methods

#### fromString(str, no_save)

从基于字符串的 JSON 对象设置此成员的当前状态

- 参数

| 名称    | 类型    | 描述                        |
| ------- | ------- | --------------------------- |
| str     | string  | 会员国序列化                |
| no_save | boolean | 表示 cryptoSuite 不应该保存 |

返回结果

- 由序列化字符串表示的未编组 Member 对象的 Promise。

  - 类型

    Promise

#### getAffiliation()

获取从属关系。

返回结果

- 从属关系。

  - 类型

    string

#### getCryptoSuite()

获取此 User 实例的 [module:api.CryptoSuite](https://hyperledger.github.io/fabric-sdk-node/release-1.4/module-api.CryptoSuite.html) cryptoSuite 对象。

返回结果

- 用于存储加密和密钥存储设置的 cryptoSuite。

  - 类型

    [module:api.CryptoSuite](https://hyperledger.github.io/fabric-sdk-node/release-1.4/module-api.CryptoSuite.html)

#### getIdentity()

获取此 User 实例的 [Identity](https://hyperledger.github.io/fabric-sdk-node/release-1.4/global.html#Identity) 对象，用于验证签名

返回结果

- 封装用户注册证书的标识对象。

  - 类型

    [Identity](https://hyperledger.github.io/fabric-sdk-node/release-1.4/global.html#Identity)

#### getName()

获取会员名称。

返回结果

- 成员名称。

  - 类型

    string

#### getRoles()

获取角色。

返回结果

- 角色。

  - 类型

    Array.&lt;string&gt;

#### getSigningIdentity()

获取此 User 实例的[SigningIdentity](https://hyperledger.github.io/fabric-sdk-node/release-1.4/SigningIdentity.html)对象，用于生成签名

返回结果

- 封装用于签名的用户专用密钥的标识对象。

  - 类型

    [SigningIdentity](https://hyperledger.github.io/fabric-sdk-node/release-1.4/SigningIdentity.html)

#### isEnrolled()

确定是否名称已注册。

返回结果

- 如果已注册，则为 true；否则为 false。

  - 类型

    boolean

#### setAffiliation(affiliation)

设置从属关系。

- 参数

| 名称        | 类型   | 描述     |
| ----------- | ------ | -------- |
| affiliation | string | 从属关系 |

#### setCryptoSuite(cryptoSuite)

设置 cryptoSuite。当应用程序需要使用默认设置以外的加密设置或密钥存储时，它需要设置使用所需的 CryptoSuite 设置和 CryptoKeyStore 选项创建的 cryptoSuite 实例。

- 参数

| 名称        | 类型                                                                                                            | 描述        |
| ----------- | --------------------------------------------------------------------------------------------------------------- | ----------- |
| cryptoSuite | [module:api.CryptoSuite](https://hyperledger.github.io/fabric-sdk-node/release-1.4/module-api.CryptoSuite.html) | cryptoSuite |

#### &lt;async&gt; setEnrollment(privateKey, certificate, mspId, skipPersistence)

设置此 User 实例的注册对象

- 参数

| 名称            | 类型                                                                                            | 描述                            |
| --------------- | ----------------------------------------------------------------------------------------------- | ------------------------------- |
| privateKey      | [module:api.Key](https://hyperledger.github.io/fabric-sdk-node/release-1.4/module-api.Key.html) | 私钥对象                        |
| certificate     | string                                                                                          | PEM 编码的证书字符串            |
| mspId           | string                                                                                          | 本地签名身份的会员服务提供商 ID |
| skipPersistence | boolean                                                                                         | 是否保留该用户                  |

返回结果

- 成功完成创建用户签名身份的 Promise。

  - 类型

    Promise

#### setRoles(roles)

设置角色。

- 参数

| 名称  | 类型                 | 描述 |
| ----- | -------------------- | ---- |
| roles | Array.&lt;string&gt; | 角色 |

#### toString()

将此成员的当前状态另存为字符串

返回结果

- 该成员的状态为字符串。

  - 类型

    string

---
