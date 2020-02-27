# CryptoSuite_PKCS11

## 说明

兼容 PKCS#11 的实现，以支持硬件安全模块。

### new CryptoSuite_PKCS11(keySize, hash, opts)

- 参数

| 名称    | 类型   | 描述                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
| :------ | :----- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| keySize | number | 密钥的长度(以字节为单位)，又称"安全级别"<br>Length of key (in bytes), a.k.a "security level"                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              |
| hash    | string | 可选的。哈希算法，支持的值为" SHA2"和" SHA3"                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              |
| opts    | Object | 选项的形式<br>{ lib: string, // the library package to support this implementation slot: number, // the hardware slot number pin: string, // the user's PIN usertype: number, // the user type readwrite: boolean // true if the session is read/write or false if read-only }<br>如果未指定'lib'或为 null，则其值将从 CRYPTO_PKCS11_LIB env var 获取，如果未设置 env var，则其值将从配置文件中的 crypto-pkcs11-lib 密钥获取。<br>如果未指定'slot'或为 null，则其值将从 CRYPTO_PKCS11_SLOT env var 获取，如果未设置 env var，则其值将从配置文件中的 crypto-pkcs11-slot 密钥获取。<br>如果未指定'pin'或为 null，则其值将从 CRYPTO_PKCS11_PIN env var 获取，如果未设置 env var，则其值将从配置文件中的 crypto-pkcs11-pin 密钥获取。<br>如果未指定'usertype'或为 null，则其值将从 CRYPTO_PKCS11_USERTYPE env var 获取，如果未设置 env var，则其值将从配置文件中的 crypto-pkcs11-usertype 密钥获取，如果未设置 config 值，则其值将默认为 1。假定 C_Login 可以验证，则该值将不被验证。 来自[这里](http://docs.oasis-open.org/pkcs11/pkcs11-base/v2.40/os/pkcs11-base-v2.40-os.html)<br>0 CKU_SO 0UL 1 CKU_USER 1UL 2 CKU_CONTEXT_SPECIFIC 2UL 4294967295 max allowed 0xFFFFFFFFUL<br>如果未指定'readwrite'或为 null，则其值将从 CRYPTO_PKCS11_READWRITE env var 获取，如果未设置 env var，则其值将从配置文件中的 crypto-pkcs11-readwrite 密钥获取，如果未设置 config 值，则其值将默认为 true。 |

### 扩展

- [module:api.CryptoSuite](https://hyperledger.github.io/fabric-sdk-node/release-1.4/module-api.CryptoSuite.html)

### Methods

#### decrypt()

这是 [module:api.CryptoSuite#decrypt](https://hyperledger.github.io/fabric-sdk-node/release-1.4/module-api.CryptoSuite.html#decrypt) 使用密钥解密 cipherText 的实现。尚不支持 opts 参数。

覆写(Overrides:):

- [module:api.CryptoSuite#decrypt](https://hyperledger.github.io/fabric-sdk-node/release-1.4/module-api.CryptoSuite.html#decrypt)

#### deriveKey()

这是 [module:api.CryptoSuite#deriveKey](https://hyperledger.github.io/fabric-sdk-node/release-1.4/module-api.CryptoSuite.html#deriveKey) 的实现。

覆写(Overrides:):

- [module:api.CryptoSuite#deriveKey](https://hyperledger.github.io/fabric-sdk-node/release-1.4/module-api.CryptoSuite.html#deriveKey)

#### encrypt()

这是 [module:api.CryptoSuite#encrypt](https://hyperledger.github.io/fabric-sdk-node/release-1.4/module-api.CryptoSuite.html#encrypt) 使用密钥加密 plainText 的实现。尚不支持 opts 参数。

覆写(Overrides:):

- [module:api.CryptoSuite#encrypt](https://hyperledger.github.io/fabric-sdk-node/release-1.4/module-api.CryptoSuite.html#encrypt)

#### generateEphemeralKey()

生成一个临时密钥。

继承自(Inherited From):

- [module:api.CryptoSuite#generateEphemeralKey](https://hyperledger.github.io/fabric-sdk-node/release-1.4/module-api.CryptoSuite.html#generateEphemeralKey)

覆写(Overrides:):

- [module:api.CryptoSuite#generateEphemeralKey](https://hyperledger.github.io/fabric-sdk-node/release-1.4/module-api.CryptoSuite.html#generateEphemeralKey)

抛出

- 如果未实现，将抛出错误

返回结果

- Key 类的实例

  - 类型

    [module:api.Key](https://hyperledger.github.io/fabric-sdk-node/release-1.4/module-api.Key.html)

#### generateKey()

这是 [module:api.CryptoSuite#generateKey](https://hyperledger.github.io/fabric-sdk-node/release-1.4/module-api.CryptoSuite.html#generateKey) 返回代表私钥的 module.api.Key 的实例的实现。它也封装了公钥。默认情况下，除非 opts.ephemeral 设置为 false，否则生成的密钥(keypar)是临时的，在这种情况下，HSM 将在 PKCS11 会话中保存密钥(密钥对)

覆写(Overrides:):

- [module:api.CryptoSuite#generateKey](https://hyperledger.github.io/fabric-sdk-node/release-1.4/module-api.CryptoSuite.html#generateKey)

返回结果

- 包含私有密钥和公共密钥的 module:PKCS11_ECDSA_KEY 实例的 Promise

  - 类型

    [module:api.Key](https://hyperledger.github.io/fabric-sdk-node/release-1.4/module-api.Key.html)

#### getKey()

这是 [module:api.CryptoSuite#getKey](https://hyperledger.github.io/fabric-sdk-node/release-1.4/module-api.CryptoSuite.html#getKey) 返回此 CSP 关联到"主题密钥标识符"列表密钥的实现。

覆写(Overrides:):

- [module:api.CryptoSuite#getKey](https://hyperledger.github.io/fabric-sdk-node/release-1.4/module-api.CryptoSuite.html#getKey)

#### hash()

这是 [module:api.CryptoSuite#hash](https://hyperledger.github.io/fabric-sdk-node/release-1.4/module-api.CryptoSuite.html#hash) 的实现。尚不支持 opts 参数。

覆写(Overrides:):

- [module:api.CryptoSuite#hash](https://hyperledger.github.io/fabric-sdk-node/release-1.4/module-api.CryptoSuite.html#hash)

#### importKey()

这是 [module:api.CryptoSuite#importKey](https://hyperledger.github.io/fabric-sdk-node/release-1.4/module-api.CryptoSuite.html#importKey) 的实现。

覆写(Overrides:):

- [module:api.CryptoSuite#importKey](https://hyperledger.github.io/fabric-sdk-node/release-1.4/module-api.CryptoSuite.html#importKey)

#### &lt;abstract&gt; setCryptoKeyStore(cryptoKeyStore)

设置 cryptoKeyStore。当应用程序需要使用默认存储以外的其他密钥存储时，应使用客户端 [Client](https://hyperledger.github.io/fabric-sdk-node/release-1.4/Client.html) newCryptoKeyStore 创建实例，并使用此功能在 CryptoSuite 上设置实例。

覆写(Overrides:):

- [module:api.CryptoSuite#decrypt](https://hyperledger.github.io/fabric-sdk-node/release-1.4/module-api.CryptoSuite.html#decrypt)

#### decrypt( )

这是 [module:api.CryptoSuite#decrypt](https://hyperledger.github.io/fabric-sdk-node/release-1.4/module-api.CryptoSuite.html#decrypt) 使用密钥解密 cipherText 的实现。尚不支持 opts 参数。

覆写(Overrides:):

- [module:api.CryptoSuite#decrypt](https://hyperledger.github.io/fabric-sdk-node/release-1.4/module-api.CryptoSuite.html#decrypt)

- 参数

| 名称           | 类型                                                                                            | 描述           |
| :------------- | :---------------------------------------------------------------------------------------------- | -------------- |
| cryptoKeyStore | [CryptoKeyStore](https://hyperledger.github.io/fabric-sdk-node/release-1.4/CryptoKeyStore.html) | cryptoKeyStore |

继承自(Inherited From):

- [module:api.CryptoSuite#setCryptoKeyStore](https://hyperledger.github.io/fabric-sdk-node/release-1.4/module-api.CryptoSuite.html#setCryptoKeyStore)

覆写(Overrides:):

- [module:api.CryptoSuite#setCryptoKeyStore](https://hyperledger.github.io/fabric-sdk-node/release-1.4/module-api.CryptoSuite.html#setCryptoKeyStore)

#### sign()

这是 [module:api.CryptoSuite#sign](https://hyperledger.github.io/fabric-sdk-node/release-1.4/module-api.CryptoSuite.html#sign) 使用密钥 k 标记摘要的实现。

覆写(Overrides:):

- [module:api.CryptoSuite#sign](https://hyperledger.github.io/fabric-sdk-node/release-1.4/module-api.CryptoSuite.html#sign)

#### verify()

这是 [module:api.CryptoSuite#verify](https://hyperledger.github.io/fabric-sdk-node/release-1.4/module-api.CryptoSuite.html#verify) 根据密钥 k 和摘要验证签名的实现。

覆写(Overrides:):

- [module:api.CryptoSuite#verify](https://hyperledger.github.io/fabric-sdk-node/release-1.4/module-api.CryptoSuite.html#verify)

---
