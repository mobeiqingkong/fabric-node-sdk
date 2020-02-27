# CryptoKeyStore

## 说明

### new CryptoKeyStore(KVSImplClass, opts)

CryptoKeyStore 使用[module:api.KeyValueStore](https://hyperledger.github.io/fabric-sdk-node/release-1.4/module-api.KeyValueStore.html)实现的基础实例来保留加密密钥。

- 参数

| 名称         | 类型     | 描述                                                                                                                                     |
| :----------- | :------- | ---------------------------------------------------------------------------------------------------------------------------------------- |
| KVSImplClass | function | 可选。内置密钥存储区可保存私钥。密钥库可以由不同的 KeyValueStore 实现来支持。如果指定，则参数的值必须指向实现 KeyValueStore 接口的模块。 |
| opts         | Object   | 构造函数中使用的特定于实现的选项对象                                                                                                     |

---
