# Signer

## Signer

签名者是不透明私钥的接口，可用于签名操作

#### new Signer(cryptoSuite, key)

- 参数

| 名称        | 类型                                                                                                            | 描述                                |
| ----------- | --------------------------------------------------------------------------------------------------------------- | ----------------------------------- |
| cryptoSuite | [module:api.CryptoSuite](https://hyperledger.github.io/fabric-sdk-node/release-1.4/module-api.CryptoSuite.html) | 数字签名算法的底层 CryptoSuite 实现 |
| key         | [module:api.Key](https://hyperledger.github.io/fabric-sdk-node/release-1.4/module-api.Key.html)                 | 私钥                                |

### Methods

#### getPublicKey()

返回与不透明私钥相对应的公钥

返回结果

- 对应于私钥的公钥。

  - 类型

    [module:api.Key](https://hyperledger.github.io/fabric-sdk-node/release-1.4/module-api.Key.html)

#### sign(digest)

用私钥签名摘要。哈希实现 SignerOpts 接口，并且在大多数情况下，可以简单地传入用作 opt 的哈希函数。Sign 也可以尝试将 assert opts 键入其他类型，以获得特定于算法的值。请注意，当需要较大消息的哈希签名时，调用方负责哈希较大的消息，并将哈希(作为摘要)传递给 Sign。

- 参数

| 名称   | 类型               | 描述         |
| ------ | ------------------ | ------------ |
| digest | Array.&lt;byte&gt; | 要签名的消息 |

---
