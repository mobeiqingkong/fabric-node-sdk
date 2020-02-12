# api.KeyValueStore

## [api](https://hyperledger.github.io/fabric-sdk-node/release-1.4/module-api.html). KeyValueStore

键值存储的抽象类。 Channel类使用此存储来保存敏感信息，例如经过身份验证的用户的私钥，证书等。SDK提供了基于文件的默认实现。可以使用“键值存储”配置设置来指定替代实现，该设置指向指向模块包的完整require()路径。

#### new KeyValueStore()

### Methods

#### getValue(name)

获取与name关联的值。

- 参数

| 名称 | 类型   | 描述      |
| ---- | ------ | --------- |
| name | string | key的名称 |

返回结果

- 与密钥对应的value的Promise。如果存储中不存在该值，则在不拒绝promise的情况下返回null。

  - 类型

    Promise

#### setValue(name, value)

设置与name关联的value。

- 参数

| 名称  | 类型   | 描述        |
| ----- | ------ | ----------- |
| name  | string | key的名称   |
| value | string | 保存的value |

返回结果

- 成功写入操作后“value”对象的Promise。

  - 类型

    Promise