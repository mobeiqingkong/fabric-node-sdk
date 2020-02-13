# AffiliationService

### AffiliationService

这是从属关系的实现，该服务使用 Fabric CA 客户端 [FabricCAClient](FabricCAClient.md)与 Fabric CA 服务器进行通信。

#### new AffiliationService()

### Methods

#### create(req, registrar)

创建一个新的从属关系。调用者必须具有 hf.AffiliationMgr 权限。

- 参数

| 名称      | 类型                                                  | 描述                                                       |
| --------- | ----------------------------------------------------- | ---------------------------------------------------------- |
| req       | [AffiliationRequest](../global.md#AffiliationRequest) | 必要. AffiliationRequest](../global.md#AffiliationRequest) |
| registrar | [User](../global.md#User)                             | 必要. 注册人员的身份（即执行注册的人）。                   |

- 返回结果:

  [ServiceResponse](https://hyperledger.github.io/fabric-sdk-node/release-1.4/global.html#ServiceResponse)

- 类型

  Promise

#### delete(req, registrar)

删除从属关系。调用者必须具有 hf.AffiliationMgr 权限。

- 参数

| 名称      | 类型                                                                        | 描述                                                       |
| --------- | --------------------------------------------------------------------------- | ---------------------------------------------------------- |
| req       | [AffiliationRequest](../global.md#AffiliationRequest)                       | 必要. AffiliationRequest](../global.md#AffiliationRequest) |
| registrar | [User](https://hyperledger.github.io/fabric-sdk-node/release-1.4/User.html) | 必要. 注册人员的身份（即执行注册的人）。                   |

- 返回结果:

  [ServiceResponse](https://hyperledger.github.io/fabric-sdk-node/release-1.4/global.html#ServiceResponse)

- 类型

  Promise

#### getAll(registrar)

列出所有等于或低于调用者从属关系的从属关系。调用者必须具有 hf.AffiliationMgr 权限。

- 参数

| 名称      | 类型                                                                        | 描述                                     |
| --------- | --------------------------------------------------------------------------- | ---------------------------------------- |
| registrar | [User](https://hyperledger.github.io/fabric-sdk-node/release-1.4/User.html) | 必要. 注册人员的身份（即执行注册的人）。 |

- 返回结果:

  [ServiceResponse](https://hyperledger.github.io/fabric-sdk-node/release-1.4/global.html#ServiceResponse)

- 类型

  Promise

#### getOne(affiliation, registrar)

列出调用者的联盟或以下的特定从属关系。调用者必须具有 hf.AffiliationMgr 权限。

- 参数

| 名称        | 类型                                                                        | 描述                                                       |
| ----------- | --------------------------------------------------------------------------- | ---------------------------------------------------------- |
| affiliation | string                                                                      | 必要. AffiliationRequest](../global.md#AffiliationRequest) |
| registrar   | [User](https://hyperledger.github.io/fabric-sdk-node/release-1.4/User.html) | 必要. 注册人员的身份（即执行注册的人）。                   |

- 返回结果:

  [ServiceResponse](https://hyperledger.github.io/fabric-sdk-node/release-1.4/global.html#ServiceResponse)

- 类型

  Promise

#### update(affiliation, req, registrar)

重命名从属关系。调用者必须具有 hf.AffiliationMgr 权限。

- 参数

| 名称        | 类型                                                                        | 描述                                                       |
| ----------- | --------------------------------------------------------------------------- | ---------------------------------------------------------- |
| affiliation | string                                                                      | 联盟路径要更新                                             |
| req         | [AffiliationRequest](../global.md#AffiliationRequest)                       | 必要. AffiliationRequest](../global.md#AffiliationRequest) |
| registrar   | [User](https://hyperledger.github.io/fabric-sdk-node/release-1.4/User.html) |                                                            |

- 返回结果:

  [ServiceResponse](https://hyperledger.github.io/fabric-sdk-node/release-1.4/global.html#ServiceResponse)

- 类型

  Promise

---
