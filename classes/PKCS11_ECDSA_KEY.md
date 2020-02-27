# PKCS11_ECDSA_KEY

## 说明

### new PKCS11_ECDSA_KEY()

该模块实现[module:api.Key](https://hyperledger.github.io/fabric-sdk-node/release-1.4/module-api.Key.html) 接口，用于通过 PKCS#11 接口通过硬件安全模块支持进行 ECDSA 密钥管理。

### 扩展

- [module:api.Key](https://hyperledger.github.io/fabric-sdk-node/release-1.4/module-api.Key.html)

### Methods

#### getPublicKey()

如果此密钥是非对称私钥，则返回相应的公钥。如果此密钥已经公开，则返回此密钥本身。

- 继承自 :

[module:api.Key#getPublicKey](https://hyperledger.github.io/fabric-sdk-node/release-1.4/module-api.Key.html#getPublicKey)

- 覆写 (Overrides):

[module:api.Key#getPublicKey](https://hyperledger.github.io/fabric-sdk-node/release-1.4/module-api.Key.html#getPublicKey)

返回结果

- 如果此密钥是非对称私钥，则为相应的公钥。如果此密钥已经公开，则返回此密钥本身。

  - 类型

    [module:api.Key](https://hyperledger.github.io/fabric-sdk-node/release-1.4/module-api.Key.html)

#### getSKI()

返回此键的主题键标识符

- 继承自 :

[module:api.Key#getSKI](https://hyperledger.github.io/fabric-sdk-node/release-1.4/module-api.Key.html#getSKI)

- 覆写 (Overrides):

[module:api.Key#getSKI](https://hyperledger.github.io/fabric-sdk-node/release-1.4/module-api.Key.html#getSKI)

返回结果

- 此密钥的主题密钥标识符，为十六进制编码的字符串。

  - 类型

    string

#### isPrivate()

如果此密钥是非对称私钥，则返回 true，否则返回 false。

- 继承自 :

[module:api.Key#isPrivate](https://hyperledger.github.io/fabric-sdk-node/release-1.4/module-api.Key.html#isPrivate)

- 覆写 (Overrides):

[module:api.Key#isPrivate](https://hyperledger.github.io/fabric-sdk-node/release-1.4/module-api.Key.html#isPrivate)

返回结果

- 此密钥是否是非对称私钥。

  - 类型

    boolean

#### isSymmetric()

如果此密钥是对称密钥，则返回 true；如果此密钥是非对称密钥，则返回 false

- 继承自 :

[module:api.Key#isSymmetric](https://hyperledger.github.io/fabric-sdk-node/release-1.4/module-api.Key.html#isSymmetric)

- 覆写 (Overrides):

[module:api.Key#isSymmetric](https://hyperledger.github.io/fabric-sdk-node/release-1.4/module-api.Key.html#isSymmetric)

返回结果

- 此密钥是否是对称密钥。

  - 类型

    boolean

#### toBytes()

如果允许此操作，则将该键转换为其 PEM 表示形式。

- 继承自 :

[module:api.Key#toBytes](https://hyperledger.github.io/fabric-sdk-node/release-1.4/module-api.Key.html#toBytes)

- 覆写 (Overrides):

[module:api.Key#toBytes](https://hyperledger.github.io/fabric-sdk-node/release-1.4/module-api.Key.html#toBytes)

返回结果

- 密钥的 PEM 字符串表示形式。

  - 类型

    string

---
