# CouchDBKeyValueStore

## CouchDBKeyValueStore

这是[KeyValueStore](https://hyperledger.github.io/fabric-sdk-node/release-1.4/module-api.KeyValueStore.html) API 的示例数据库实现。它使用本地或远程 CouchDB 数据库实例存储密钥。

#### new CouchDBKeyValueStore(options)

constructor

- 参数

| 名称    | 类型                                                                                             | 描述                          |
| :------ | :----------------------------------------------------------------------------------------------- | ----------------------------- |
| options | [CouchDBOpts](https://hyperledger.github.io/fabric-sdk-node/release-1.4/global.html#CouchDBOpts) | 用于连接到 CouchDB 实例的设置 |

### 扩展

- [module:api.KeyValueStore](https://hyperledger.github.io/fabric-sdk-node/release-1.4/module-api.KeyValueStore.html)

### Methods

#### getValue(name)

获取与 name 关联的值。

- 参数

| 名称 | 类型   | 描述     |
| :--- | :----- | -------- |
| name | string | key 名称 |

继承自(Inherited From):

- [module:api.KeyValueStore#getValue](https://hyperledger.github.io/fabric-sdk-node/release-1.4/module-api.KeyValueStore.html#getValue)

覆写(Overrides:):

- [module:api.KeyValueStore#getValue](

返回结果

- key 对应的值的 Promise。如果存储中不存在该值，则在不拒绝 promise 的情况下返回 null。

  - 类型

    Promise

#### setValue(name, value)

设置与 name 关联的值。

- 参数

| 名称  | 类型   | 描述              |
| :---- | :----- | ----------------- |
| name  | string | 要保存的 key 名称 |
| value | string | 保存的 value      |

继承自(Inherited From):

- [module:api.KeyValueStore#setValue](https://hyperledger.github.io/fabric-sdk-node/release-1.4/module-api.KeyValueStore.html#setValue)

覆写(Overrides:):

- [module:api.KeyValueStore#setValue](https://hyperledger.github.io/fabric-sdk-node/release-1.4/module-api.KeyValueStore.html#setValue)

返回结果

- 成功写入操作后"value"对象的 Promise。

  - 类型

    Promise

---
