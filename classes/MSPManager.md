# MSPManager

## MSPManager

MSPManager 是定义一个或多个 MSP 的管理器的接口。这实质上充当 MSP 调用的中介，并将 MSP 相关的调用路由到适当的 MSP。该对象是不可变的，它只初始化一次就不会更改。

#### new MSPManager()

### Methods

#### addMSP(config)

根据配置信息创建 MSP 实例并将其添加到此管理器

- 参数

| 名称   | 类型   | 描述                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
| ------ | ------ | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| config | Object | 特定于实现的配置对象。对于此实现，它使用以下字段:<br>rootCerts:表示信任锚的[Identity](https://hyperledger.github.io/fabric-sdk-node/release-1.4/global.html#Identity)数组，用于验证签名证书。验证签名中使用的 MSP 必需<br>intermediateCerts:[Identity](https://hyperledger.github.io/fabric-sdk-node/release-1.4/global.html#Identity)的数组，表示用于验证签名证书的信任锚。用于验证签名的 MSP 可选<br>admins:代表管理员权限的[Identity](https://hyperledger.github.io/fabric-sdk-node/release-1.4/global.html#Identity)数组<br>signer:[SigningIdentity](https://hyperledger.github.io/fabric-sdk-node/release-1.4/SigningIdentity.html)签名身份。用于签名的 MSP 必需<br>id:此实例的标识符的{string}值<br>orgs:组织单位标识符的{string}数组<br>cryptoSuite:用于加密基元操作的基础 [module:api.CryptoSuite](https://hyperledger.github.io/fabric-sdk-node/release-1.4/module-api.CryptoSuite.html) |

返回结果

- 新创建的 MSP 实例

  - 类型

    [MSP](https://hyperledger.github.io/fabric-sdk-node/release-1.4/MSP.html)

#### deserializeIdentity(serializedIdentity)

DeserializeIdentity 反序列化身份

- 参数

| 名称               | 类型               | 描述                                                                           |
| ------------------ | ------------------ | ------------------------------------------------------------------------------ |
| serializedIdentity | Array.&lt;byte&gt; | 具有两个字段的对象的基于 Protobuf 的序列化:mspid 和 idBytes 用于证书 PEM 字节 |

返回结果

- [Identity](https://hyperledger.github.io/fabric-sdk-node/release-1.4/global.html#Identity) 实例的 Promise

- 类型

  Promise

#### getMSP()

返回一个验证的 MSP

#### getMSPs()

返回验证的 MSP。请注意，这不会返回本地 MSP

#### loadMSPs(mspConfigs)

实例化 MSP 以验证身份(例如 ProposalResponse 中的背书人)。通过此方法加载的 MSP 需要代表签名的身份证明的证书颁发机构的 CA 证书。它们还可以选择包含 CA 证书代表的组织管理员的证书。

- 参数

| 名称       | 类型                       | 描述                                                             |
| ---------- | -------------------------- | ---------------------------------------------------------------- |
| mspConfigs | protos/msp/mspconfig.proto | 由 protobuf protos/msp/mspconfig.proto 定义的 MSPConfig 对象数组 |

---
