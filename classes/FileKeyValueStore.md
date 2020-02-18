# FileKeyValueStore

## FileKeyValueStore

这是[KeyValueStore](https://hyperledger.github.io/fabric-sdk-node/release-1.4/module-api.KeyValueStore.html) API 的默认实现。它使用文件来存储键值。

#### new FileKeyValueStore(options)

constructor

- 参数

| 名称    | 类型   | 描述                                          |
| ------- | ------ | --------------------------------------------- |
| options | Object | 包含单个属性路径，该路径指向 store 的顶级目录 |

### Methods

#### getValue(name)

获取与 name 关联的值。

- 参数

| 名称 | 类型   | 描述       |
| ---- | ------ | ---------- |
| name | string | key 的名称 |

继承自(Inherited From)：

- [module:api.KeyValueStore#getValue](https://hyperledger.github.io/fabric-sdk-node/release-1.4/module-api.KeyValueStore.html#getValue)

覆写(Overrides:):

- [module:api.KeyValueStore#getValue](https://hyperledger.github.io/fabric-sdk-node/release-1.4/module-api.KeyValueStore.html#getValue)

返回结果

- 与密钥对应的 value 的 Promise。如果存储中不存在该值，则在不拒绝 promise 的情况下返回 null。

  - 类型

    Promise

#### setValue(name, value)

设置与 name 关联的 value。

- 参数

| 名称  | 类型   | 描述         |
| ----- | ------ | ------------ |
| name  | string | key 的名称   |
| value | string | 保存的 value |

继承自(Inherited From)：

- [module:api.KeyValueStore#setValue](https://hyperledger.github.io/fabric-sdk-node/release-1.4/module-api.KeyValueStore.html#setValue)

覆写(Overrides:):

- [module:api.KeyValueStore#setValue](https://hyperledger.github.io/fabric-sdk-node/release-1.4/module-api.KeyValueStore.html#setValue)

返回结果

- 成功写入操作后“value”对象的 Promise。

  - 类型

    Promise

---
