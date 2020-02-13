# CryptoSuite_PKCS11

## CryptoSuite_PKCS11

兼容PKCS#11的实现，以支持硬件安全模块。

#### new CryptoSuite_PKCS11(keySize, hash, opts)

- 参数

| 名称    | 类型   | 描述                                                         |
| :------ | :----- | ------------------------------------------------------------ |
| keySize | number | 密钥的长度（以字节为单位），又称“安全级别”<br>Length of key (in bytes), a.k.a "security level" |
| hash    | string | 可选的。哈希算法，支持的值为“ SHA2”和“ SHA3”                 |
| opts    | Object | 选项的形式<br>```   {     lib: string,       // the library package to support this implementation     slot: number,      // the hardware slot number     pin: string,       // the user's PIN     usertype: number,  // the user type     readwrite: boolean // true if the session is read/write or false if read-only   }<br>如果未指定'lib'或为null，则其值将从CRYPTO_PKCS11_LIB env var获取，如果未设置env var，则其值将从配置文件中的crypto-pkcs11-lib密钥获取。<br>如果未指定'slot'或为null，则其值将从CRYPTO_PKCS11_SLOT env var获取，如果未设置env var，则其值将从配置文件中的crypto-pkcs11-slot密钥获取。<br>如果未指定'pin'或为null，则其值将从CRYPTO_PKCS11_PIN env var获取，如果未设置env var，则其值将从配置文件中的crypto-pkcs11-pin密钥获取。<br>如果未指定'usertype'或为null，则其值将从CRYPTO_PKCS11_USERTYPE env var获取，如果未设置env var，则其值将从配置文件中的crypto-pkcs11-usertype密钥获取，如果未设置config值，则其值将默认为1。假定C_Login可以验证，则该值将不被验证。 ---来自http://docs.oasis-open.org/pkcs11/pkcs11-base/v2.40/os/pkcs11-base-v2.40-os.html<br>0          CKU_SO                          0UL 1          CKU_USER                        1UL 2          CKU_CONTEXT_SPECIFIC            2UL 4294967295 max allowed            0xFFFFFFFFUL<br>如果未指定'readwrite'或为null，则其值将从CRYPTO_PKCS11_READWRITE env var获取，如果未设置env var，则其值将从配置文件中的crypto-pkcs11-readwrite密钥获取，如果未设置config值，则其值将默认为true。 |

### 扩展

- [module:api.CryptoSuite](https://hyperledger.github.io/fabric-sdk-node/release-1.4/module-api.CryptoSuite.html)

### Methods

#### decrypt()

这是 [module:api.CryptoSuite#decrypt](https://hyperledger.github.io/fabric-sdk-node/release-1.4/module-api.CryptoSuite.html#decrypt) 使用密钥解密cipherText的实现。尚不支持opts参数。

覆写(Overrides:):

- [module:api.CryptoSuite#decrypt](https://hyperledger.github.io/fabric-sdk-node/release-1.4/module-api.CryptoSuite.html#decrypt)

#### deriveKey()

这是 [module:api.CryptoSuite#deriveKey](https://hyperledger.github.io/fabric-sdk-node/release-1.4/module-api.CryptoSuite.html#deriveKey) 的实现。

覆写(Overrides:):

- [module:api.CryptoSuite#deriveKey](https://hyperledger.github.io/fabric-sdk-node/release-1.4/module-api.CryptoSuite.html#deriveKey)

#### encrypt()

这是 [module:api.CryptoSuite#encrypt](https://hyperledger.github.io/fabric-sdk-node/release-1.4/module-api.CryptoSuite.html#encrypt) 使用密钥加密plainText的实现。尚不支持opts参数。

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

#### generateKey()

这是 [module:api.CryptoSuite#generateKey](https://hyperledger.github.io/fabric-sdk-node/release-1.4/module-api.CryptoSuite.html#generateKey) 返回代表私钥的module.api.Key的实例的实现。它也封装了公钥。默认情况下，除非opts.ephemeral设置为false，否则生成的密钥（keypar）是临时的，在这种情况下，HSM将在PKCS11会话中保存密钥（密钥对）

覆写(Overrides:):

- [module:api.CryptoSuite#generateKey](https://hyperledger.github.io/fabric-sdk-node/release-1.4/module-api.CryptoSuite.html#generateKey)

返回结果

- 包含私有密钥和公共密钥的module:PKCS11_ECDSA_KEY实例的Promise

  - 类型

    [module:api.Key](https://hyperledger.github.io/fabric-sdk-node/release-1.4/module-api.Key.html)

#### getKey()

这是 [module:api.CryptoSuite#getKey](https://hyperledger.github.io/fabric-sdk-node/release-1.4/module-api.CryptoSuite.html#getKey) 返回此CSP关联到“主题密钥标识符”列表密钥的实现。

覆写(Overrides:):

- [module:api.CryptoSuite#getKey](https://hyperledger.github.io/fabric-sdk-node/release-1.4/module-api.CryptoSuite.html#getKey)

#### hash()

这是 [module:api.CryptoSuite#hash](https://hyperledger.github.io/fabric-sdk-node/release-1.4/module-api.CryptoSuite.html#hash) 的实现。尚不支持opts参数。

覆写(Overrides:):

-  [module:api.CryptoSuite#hash](https://hyperledger.github.io/fabric-sdk-node/release-1.4/module-api.CryptoSuite.html#hash) 

#### importKey()

这是 [module:api.CryptoSuite#importKey](https://hyperledger.github.io/fabric-sdk-node/release-1.4/module-api.CryptoSuite.html#importKey) 的实现。

覆写(Overrides:):

- [module:api.CryptoSuite#importKey](https://hyperledger.github.io/fabric-sdk-node/release-1.4/module-api.CryptoSuite.html#importKey)

#### &lt;abstract&gt; setCryptoKeyStore(cryptoKeyStore)

设置cryptoKeyStore。当应用程序需要使用默认存储以外的其他密钥存储时，应使用客户端  [Client](https://hyperledger.github.io/fabric-sdk-node/release-1.4/Client.html) newCryptoKeyStore创建实例，并使用此功能在CryptoSuite上设置实例。

覆写(Overrides:):

- [module:api.CryptoSuite#decrypt](https://hyperledger.github.io/fabric-sdk-node/release-1.4/module-api.CryptoSuite.html#decrypt)

#### decrypt()

这是 [module:api.CryptoSuite#decrypt](https://hyperledger.github.io/fabric-sdk-node/release-1.4/module-api.CryptoSuite.html#decrypt) 使用密钥解密cipherText的实现。尚不支持opts参数。

覆写(Overrides:):

- [module:api.CryptoSuite#decrypt](https://hyperledger.github.io/fabric-sdk-node/release-1.4/module-api.CryptoSuite.html#decrypt)

- 参数

| 名称           | 类型                                                         | 描述           |
| :------------- | :----------------------------------------------------------- | -------------- |
| cryptoKeyStore | [CryptoKeyStore](https://hyperledger.github.io/fabric-sdk-node/release-1.4/CryptoKeyStore.html) | cryptoKeyStore |

继承自(Inherited From)：

- [module:api.CryptoSuite#setCryptoKeyStore](https://hyperledger.github.io/fabric-sdk-node/release-1.4/module-api.CryptoSuite.html#setCryptoKeyStore)

覆写(Overrides:):

- [module:api.CryptoSuite#setCryptoKeyStore](https://hyperledger.github.io/fabric-sdk-node/release-1.4/module-api.CryptoSuite.html#setCryptoKeyStore)

#### sign()

这是 [module:api.CryptoSuite#sign](https://hyperledger.github.io/fabric-sdk-node/release-1.4/module-api.CryptoSuite.html#sign) 使用密钥k标记摘要的实现。

覆写(Overrides:):

- [module:api.CryptoSuite#sign](https://hyperledger.github.io/fabric-sdk-node/release-1.4/module-api.CryptoSuite.html#sign)

#### verify()

这是 [module:api.CryptoSuite#verify](https://hyperledger.github.io/fabric-sdk-node/release-1.4/module-api.CryptoSuite.html#verify) 根据密钥k和摘要验证签名的实现。

覆写(Overrides:):

- [module:api.CryptoSuite#verify](https://hyperledger.github.io/fabric-sdk-node/release-1.4/module-api.CryptoSuite.html#verify)

***