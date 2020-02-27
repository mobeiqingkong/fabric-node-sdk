# api.Key

## 说明:[api](https://hyperledger.github.io/fabric-sdk-node/release-1.4/module-api.html). Key

密钥代表加密密钥。它可以是对称的或不对称的。在非对称密钥的情况下，密钥可以是公共的或私有的。在使用非对称私钥的情况下，getPublicKey()方法允许检索相应的公钥。可以通过主题密钥标识符(SKI)引用密钥，并且可以通过适当的 CryptoSuite 实现对其进行解析。

### new Key()

### Methods

#### getPublicKey()

如果此密钥是非对称私钥，则返回相应的公钥。如果此密钥已经公开，则返回此密钥本身。

返回结果

- 如果此密钥是非对称私钥，则为相应的公钥。如果此密钥已经公开，则返回此密钥本身。

  - 类型

    [module:api.Key](https://hyperledger.github.io/fabric-sdk-node/release-1.4/module-api.Key.html)

#### getSKI()

返回此键的主题键标识符。

返回结果

- 此密钥的主题密钥标识符，为十六进制编码的字符串。

  - 类型

    string

#### isPrivate()

如果此密钥是非对称私钥，则返回 true，否则返回 false。

返回结果

- 此密钥是否是非对称私钥。

  - 类型

    boolean

#### isSymmetric()

如果此密钥是对称密钥，则返回 true；如果此密钥是非对称密钥，则返回 false。

返回结果

- 此密钥是否是对称密钥。

  - 类型

    [module:api.Key](

#### toBytes()

如果允许此操作，则将该键转换为其 PEM 表示形式。

返回结果

- 密钥的 PEM 字符串表示形式。

  - 类型

    string

---
