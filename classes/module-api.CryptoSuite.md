# api.CryptoSuite

## 说明:[api](https://hyperledger.github.io/fabric-sdk-node/release-1.4/module-api.html). CryptoSuite

SDK 使用的一组加密算法的抽象类，用于执行数字签名，加密/解密和安全哈希。一套完整的套件包括对非对称密钥(例如 ECDSA 或 RSA)，对称密钥(例如 AES)和安全哈希(例如 SHA2 / 3)的支持。该 SDK 提供了基于 ECDSA + SHA2 / 3 的默认实现。可以使用 "crypto-suite-software"配置设置来指定替代实现，该设置指向指向模块包的完整 require() 路径。

### new CryptoSuite( )

### Methods

#### decrypt(key, cipherText, opts)

使用密钥解密密文。 opts 参数应适合所使用的算法。

- 参数

| 名称       | 类型                                                                                            | 描述             |
| ---------- | ----------------------------------------------------------------------------------------------- | ---------------- |
| key        | [module:api.Key](https://hyperledger.github.io/fabric-sdk-node/release-1.4/module-api.Key.html) | 解密密钥(私钥) |
| cipherText | Array.&lt;byte&gt;                                                                              | 密文解密         |
| opts       | Object                                                                                          | 解密选项         |

返回结果

- 解密后的纯文本。

  - 类型

    Array.&lt;byte&gt;

#### deriveKey(key, opts)

使用 opts 中传递的参数以便从源公共密钥派生新的私有密钥。导出与交易证书相对应的私钥需要此操作。

- 参数

| 名称 | 类型                                                                                            | 描述   |
| ---- | ----------------------------------------------------------------------------------------------- | ------ |
| key  | [module:api.Key](https://hyperledger.github.io/fabric-sdk-node/release-1.4/module-api.Key.html) | 源密钥 |
| opts | KeyOpts                                                                                         | 可选。 |

返回结果

- 派生密钥。

  - 类型

    [module:api.Key](https://hyperledger.github.io/fabric-sdk-node/release-1.4/module-api.Key.html)

#### encrypt(key, plainText, opts)

使用密钥加密明文。 opts 参数应适合所使用的算法。

- 参数

| 名称      | 类型                                                                                            | 描述             |
| --------- | ----------------------------------------------------------------------------------------------- | ---------------- |
| key       | [module:api.Key](https://hyperledger.github.io/fabric-sdk-node/release-1.4/module-api.Key.html) | 加密密钥(公钥) |
| plainText | Array.&lt;byte&gt;                                                                              | 纯文本加密       |
| opts      | Object                                                                                          | 加密选项         |

返回结果

- 派生密钥。

  - 类型

    Array.&lt;byte&gt;

#### generateEphemeralKey()

生成一个临时密钥。

抛出

- 如果没有实现会抛出错误

返回结果

- Key 类的实例。

  - 类型

    [module:api.Key](https://hyperledger.github.io/fabric-sdk-node/release-1.4/module-api.Key.html)

#### generateKey(opts)

使用 opts 中的选项生成密钥。如果 opts.ephemeral 参数为 false，则该方法除了返回导入的 Key 实例之外，还将生成的密钥作为 PEM 文件保存在密钥存储中，可以使用 getKey()方法进行检索

- 参数

| 名称 | 类型    | 描述 |
| ---- | ------- | ---- |
| opts | KeyOpts | 可选 |

抛出

- 如果没有实现会抛出错误

返回结果

- Key 类的 Promise。

  - 类型

    [module:api.Key](https://hyperledger.github.io/fabric-sdk-node/release-1.4/module-api.Key.html)

#### getKey(ski)

返回此实现关联到"主题密钥标识符"列表的密钥。

- 参数

| 名称 | 类型   | 描述                                                               |
| ---- | ------ | ------------------------------------------------------------------ |
| ski  | string | 特定用于 Crypto Suite 实现的主题密钥标识符，作为表示密钥的唯一索引 |

返回结果

- 对应于滑雪板的 Key 类实例的 Promise。

  - 类型

    [module:api.Key](https://hyperledger.github.io/fabric-sdk-node/release-1.4/module-api.Key.html)

#### hash(msg, opts)

使用选项 opts 产生消息 msg 的哈希

- 参数

| 名称 | 类型   | 描述                                         |
| ---- | ------ | -------------------------------------------- |
| msg  | string | 哈希的源消息                                 |
| opts | Object | algorithm:要使用的算法的标识符，例如" SHA3" |

返回结果

- 十六进制字符串编码中的哈希摘要。

  - 类型

    string

#### importKey(pem, opts)

使用 opts 从其原始表示形式导入密钥。如果 opts.ephemeral 参数为 false，则该方法除了返回导入的 Key 实例之外，还将导入的密钥保存为密钥库中的 PEM 文件，可以使用" getKey()"方法进行检索

- 参数

| 名称 | 类型    | 描述                  |
| ---- | ------- | --------------------- |
| pem  | string  | 导入密钥的 PEM 字符串 |
| opts | KeyOpts | 可选。                |

返回结果

- 如果" opts.ephemeral"为 true，则同步返回 Key 类。如果未设置" opts.ephemeral"或为 false，则返回 Key 类实例的 Promise。

  - 类型

    Key&#124;Promise

#### &lt;abstract&gt; setCryptoKeyStore(cryptoKeyStore)

设置 cryptoKeyStore。当应用程序需要使用默认存储以外的其他密钥存储时，应使用客户端 newCryptoKeyStore 创建一个实例，并使用此功能在 CryptoSuite 上设置该实例。

- 参数

| 名称           | 类型                                                                                            | 描述             |
| -------------- | ----------------------------------------------------------------------------------------------- | ---------------- |
| cryptoKeyStore | [CryptoKeyStore](https://hyperledger.github.io/fabric-sdk-node/release-1.4/CryptoKeyStore.html) | cryptoKeyStore。 |

#### sign(key, digest)

使用密钥对摘要进行签名。 opts 参数应适合所使用的算法。

- 参数

| 名称   | 类型                                                                                            | 描述                                                                                                               |
| ------ | ----------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------ |
| key    | [module:api.Key](https://hyperledger.github.io/fabric-sdk-node/release-1.4/module-api.Key.html) | 签名密钥(私钥)                                                                                                   |
| digest | Array.&lt;byte&gt;                                                                              | 要签名的消息摘要。请注意，当需要较大消息的签名时，调用方负责对较大消息进行哈希处理并将哈希(作为摘要)传递给签名。 |

返回结果

- 产生的签名。

  - 类型

    Array.&lt;byte&gt;

#### verify(key, signature, digest)

根据密钥和摘要验证签名

- 参数

| 名称      | 类型                                                                                            | 描述                 |
| --------- | ----------------------------------------------------------------------------------------------- | -------------------- |
| key       | [module:api.Key](https://hyperledger.github.io/fabric-sdk-node/release-1.4/module-api.Key.html) | 签名验证密钥(公钥) |
| signature | Array.&lt;byte&gt;                                                                              | 签名验证             |
| digest    | Array.&lt;byte&gt;                                                                              | 创建签名的摘要       |

返回结果

- 如果签名成功验证，则为 true。

  - 类型

    boolean

---
