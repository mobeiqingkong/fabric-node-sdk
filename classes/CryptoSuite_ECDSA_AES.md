# CryptoSuite_ECDSA_AES

## CryptoSuite_ECDSA_AES

[module:api.CryptoSuite](https://hyperledger.github.io/fabric-sdk-node/release-1.4/module-api.CryptoSuite.html)用于ECDSA的实现和使用软件密钥生成的AES算法。此类实现基于软件的密钥生成（与基于硬件安全模块的密钥管理相对）

#### new CryptoSuite_ECDSA_AES(keySize, hash)

constructor

- 参数

| 名称    | 类型   | 描述                                       |
| :------ | :----- | ------------------------------------------ |
| keySize | number | ECDSA算法的密钥大小，只能为256或384        |
| hash    | string | 可选的。哈希算法，支持的值为"SHA2"和"SHA3" |

### 扩展

- [module:api.CryptoSuite](https://hyperledger.github.io/fabric-sdk-node/release-1.4/module-api.CryptoSuite.html)

### Methods

#### decrypt()

这是 [module:api.CryptoSuite#decrypt](https://hyperledger.github.io/fabric-sdk-node/release-1.4/module-api.CryptoSuite.html#decrypt) 的实现。

覆写(Overrides:):

- [module:api.CryptoSuite#decrypt](https://hyperledger.github.io/fabric-sdk-node/release-1.4/module-api.CryptoSuite.html#decrypt)

#### deriveKey()

这是 [module:api.CryptoSuite#deriveKey](https://hyperledger.github.io/fabric-sdk-node/release-1.4/module-api.CryptoSuite.html#deriveKey) 的实现

覆写(Overrides:):

- [module:api.CryptoSuite#deriveKey](https://hyperledger.github.io/fabric-sdk-node/release-1.4/module-api.CryptoSuite.html#deriveKey)

#### encrypt()

这是 [module:api.CryptoSuite#encrypt](https://hyperledger.github.io/fabric-sdk-node/release-1.4/module-api.CryptoSuite.html#encrypt) 的实现

覆写(Overrides:):

- [module:api.CryptoSuite#encrypt](https://hyperledger.github.io/fabric-sdk-node/release-1.4/module-api.CryptoSuite.html#encrypt)

#### generateEphemeralKey()

生成一个临时密钥。

继承自(Inherited From)：

- [module:api.CryptoSuite#generateEphemeralKey](https://hyperledger.github.io/fabric-sdk-node/release-1.4/module-api.CryptoSuite.html#generateEphemeralKey)

覆写(Overrides:):

- [module:api.CryptoSuite#generateEphemeralKey](https://hyperledger.github.io/fabric-sdk-node/release-1.4/module-api.CryptoSuite.html#generateEphemeralKey)

抛出

- 如果未实现，将抛出错误

返回结果

- Key类的实例

  - 类型

    [module:api.Key](https://hyperledger.github.io/fabric-sdk-node/release-1.4/module-api.Key.html)

#### generateKey(opts)

使用opts中的选项生成密钥。如果opts.ephemeral参数为false，则该方法除了返回导入的Key实例外，还将生成的密钥作为PEM文件保存在密钥存储区中，可以使用getKey()方法进行检索。

- 参数

| 名称 | 类型    | 描述   |
| :--- | :------ | ------ |
| opts | KeyOpts | 可选。 |

继承自(Inherited From)：

- [module:api.CryptoSuite#generateKey](https://hyperledger.github.io/fabric-sdk-node/release-1.4/module-api.CryptoSuite.html#generateKey)

覆写(Overrides:):

- [module:api.CryptoSuite#generateKey](https://hyperledger.github.io/fabric-sdk-node/release-1.4/module-api.CryptoSuite.html#generateKey)

抛出

- 如果未实现，将抛出错误

返回结果

- Key类的实例

  - 类型

    [module:api.Key](https://hyperledger.github.io/fabric-sdk-node/release-1.4/module-api.Key.html)

#### getKey(ski)

返回实现关联到“主题密钥标识符”(Subject Key Identifier)列表的密钥。

- 参数

| 名称 | 类型   | 描述                                                         |
| :--- | :----- | ------------------------------------------------------------ |
| ski  | string | 特定用于Crypto Suite实现的主题密钥标识符，作为表示密钥的唯一索引 |

继承自(Inherited From)：

- [module:api.CryptoSuite#getKey](https://hyperledger.github.io/fabric-sdk-node/release-1.4/module-api.CryptoSuite.html#getKey)

覆写(Overrides:):

- [module:api.CryptoSuite#getKey](https://hyperledger.github.io/fabric-sdk-node/release-1.4/module-api.CryptoSuite.html#getKey)

抛出

- 如果未实现，将抛出错误

返回结果

- Key类对应于ski实例的Promise

  - 类型

    [module:api.Key](https://hyperledger.github.io/fabric-sdk-node/release-1.4/module-api.Key.html)

#### hash()

这是 [module:api.CryptoSuite#hash](https://hyperledger.github.io/fabric-sdk-node/release-1.4/module-api.CryptoSuite.html#hash) 的实现

覆写(Overrides:):

- [module:api.CryptoSuite#hash](https://hyperledger.github.io/fabric-sdk-node/release-1.4/module-api.CryptoSuite.html#hash)

#### importKey()

这是 [module:api.CryptoSuite#importKey](https://hyperledger.github.io/fabric-sdk-node/release-1.4/module-api.CryptoSuite.html#importKey) 的实现

覆写(Overrides:):

- [module:api.CryptoSuite#importKey](https://hyperledger.github.io/fabric-sdk-node/release-1.4/module-api.CryptoSuite.html#importKey)

#### setCryptoKeyStore(cryptoKeyStore)

设置cryptoKeyStore。当应用程序需要使用默认存储以外的其他密钥存储时，应使用客户端[Client](https://hyperledger.github.io/fabric-sdk-node/release-1.4/Client.html) newCryptoKeyStore创建实例，并使用此功能在CryptoSuite上设置实例。

- 参数

| 名称           | 类型                                                         | 描述           |
| :------------- | :----------------------------------------------------------- | -------------- |
| cryptoKeyStore | [CryptoKeyStore](https://hyperledger.github.io/fabric-sdk-node/release-1.4/CryptoKeyStore.html) | cryptoKeyStore |

覆写(Overrides:):

- [module:api.CryptoSuite#setCryptoKeyStore](https://hyperledger.github.io/fabric-sdk-node/release-1.4/module-api.CryptoSuite.html#setCryptoKeyStore)

#### sign()

这是 [module:api.CryptoSuite#sign](https://hyperledger.github.io/fabric-sdk-node/release-1.4/module-api.CryptoSuite.html#sign) 使用密钥k进行摘要签名的实现

覆写(Overrides:):

- [module:api.CryptoSuite#sign](https://hyperledger.github.io/fabric-sdk-node/release-1.4/module-api.CryptoSuite.html#sign)

#### verify(key, signature, digest)

根据密钥和摘要验证签名

- 参数

| 名称      | 类型                                                         | 描述                 |
| :-------- | :----------------------------------------------------------- | -------------------- |
| key       | [module:api.Key](https://hyperledger.github.io/fabric-sdk-node/release-1.4/module-api.Key.html) | 签名验证密钥（公钥） |
| signature | Array.&lt;byte&gt;                                           | 签名验证             |
| digest    | Array.&lt;byte&gt;                                           | 创建签名的摘要       |

继承自(Inherited From)：

- [module:api.CryptoSuite#verify](https://hyperledger.github.io/fabric-sdk-node/release-1.4/module-api.CryptoSuite.html#verify)

覆写(Overrides:):

- [module:api.CryptoSuite#verify](https://hyperledger.github.io/fabric-sdk-node/release-1.4/module-api.CryptoSuite.html#verify)

返回结果

- 如果签名成功验证，则为true

  - 类型

    boolean