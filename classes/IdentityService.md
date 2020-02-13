# IdentityService

## IdentityService

这是身份服务的实现，该服务使用 fabric CA 客户端 [FabricCAClient](https://hyperledger.github.io/fabric-sdk-node/release-1.4/FabricCAClient.html)与 fabricCA 服务器进行通信。

#### new IdentityService()

### Methods

#### create(req, registrar)

使用 Fabric CA 服务器创建新的标识。返回一个注册 secret，然后可以将其与注册 ID 一起用于注册新的身份。调用者必须具有“ hf.Registrar”权限。

- 参数

| 名称      | 类型                                                                                                     | 描述                                                                                                     |
| --------- | -------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------- |
| req       | [IdentityRequest](https://hyperledger.github.io/fabric-sdk-node/release-1.4/global.html#IdentityRequest) | [IdentityRequest](https://hyperledger.github.io/fabric-sdk-node/release-1.4/global.html#IdentityRequest) |
| registrar | [User](https://hyperledger.github.io/fabric-sdk-node/release-1.4/User.html)                              | 注册商的身份（即执行注册的人）。                                                                         |

返回结果

- 返回这个新身份的 secret

  - 类型

    Promise

#### delete(enrollmentID, registrar, force)

删除现有身份。调用者必须具有“ hf.Registrar”权限。

- 参数

| 名称         | 类型                                                                        | 描述                                 |
| ------------ | --------------------------------------------------------------------------- | ------------------------------------ |
| enrollmentID | string                                                                      |                                      |
| registrar    | [User](https://hyperledger.github.io/fabric-sdk-node/release-1.4/User.html) |                                      |
| force        | boolean                                                                     | 可选的。使用某些身份可以强制删除自己 |

返回结果

- [ServiceResponse](https://hyperledger.github.io/fabric-sdk-node/release-1.4/global.html#ServiceResponse)

  - 类型

    Promise

#### getAll(registrar)

获取注册服务商有权查看的所有身份。

- 参数

| 名称      | 类型                                                                        | 描述                                   |
| --------- | --------------------------------------------------------------------------- | -------------------------------------- |
| registrar | [User](https://hyperledger.github.io/fabric-sdk-node/release-1.4/User.html) | 必要。注册商的身份（即执行注册的人）。 |

返回结果

- [ServiceResponse](https://hyperledger.github.io/fabric-sdk-node/release-1.4/global.html#ServiceResponse)

  - 类型

    Promise

#### getOne(enrollmentID, registrar)

获取身份。调用者必须具有“ hf.Registrar”权限。

- 参数

| 名称         | 类型                                                                        | 描述                                   |
| ------------ | --------------------------------------------------------------------------- | -------------------------------------- |
| enrollmentID | string                                                                      | 必要。唯一标识身份的注册 ID            |
| registrar    | [User](https://hyperledger.github.io/fabric-sdk-node/release-1.4/User.html) | 必要。注册商的身份（即执行注册的人）。 |

返回结果

- [ServiceResponse](https://hyperledger.github.io/fabric-sdk-node/release-1.4/global.html#ServiceResponse)

  - 类型

    Promise

#### update(enrollmentID, req, registrar)

更新现有身份。调用者必须具有“ hf.Registrar”权限。

- 参数

| 名称         | 类型                                                                                                     | 描述 |
| ------------ | -------------------------------------------------------------------------------------------------------- | ---- |
| enrollmentID | string                                                                                                   |      |
| req          | [IdentityRequest](https://hyperledger.github.io/fabric-sdk-node/release-1.4/global.html#IdentityRequest) |      |
| registrar    | [User](https://hyperledger.github.io/fabric-sdk-node/release-1.4/User.html)                              |      |

返回结果

- [ServiceResponse](https://hyperledger.github.io/fabric-sdk-node/release-1.4/global.html#ServiceResponse)

  - 类型

    Promise
