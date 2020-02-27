# SigningIdentity

## 说明

SigningIdentity 是 Identity 的扩展，涵盖了签名功能。例如，如果客户希望签署提案答复和交易，则应要求签署身份

### new SigningIdentity(certificate, publicKey, mspId, cryptoSuite, signer)

- 参数

| 名称        | 类型                                                                                                            | 描述                                                       |
| ----------- | --------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------- |
| certificate | string                                                                                                          | PEM 编码证书的十六进制字符串                               |
| publicKey   | [module:api.Key](https://hyperledger.github.io/fabric-sdk-node/release-1.4/module-api.Key.html)                 | 证书代表的公钥                                             |
| mspId       | string                                                                                                          | 管理此身份的关联的 MSP 的 ID                               |
| cryptoSuite | [module:api.CryptoSuite](https://hyperledger.github.io/fabric-sdk-node/release-1.4/module-api.CryptoSuite.html) | 数字签名算法的底层 CryptoSuite 实现                        |
| signer      | [Signer](https://hyperledger.github.io/fabric-sdk-node/release-1.4/Signer.html)                                 | 封装不透明私钥的签名者对象和用于签名操作的相应数字签名算法 |

### Methods

#### sign(msg, opts)

使用签名者内部包含的私钥对摘要进行签名。

- 参数

| 名称 | 类型               | 描述                                                                                                                                         |
| ---- | ------------------ | -------------------------------------------------------------------------------------------------------------------------------------------- |
| msg  | Array.&lt;byte&gt; | 要签名的消息                                                                                                                                 |
| opts | Object             | 用于签名的 Options 对象包含一个字段" hashFunction"，该字段允许使用不同的哈希算法。如果不存在，则默认为为身份自己的加密套件对象配置的哈希函数 |

---
