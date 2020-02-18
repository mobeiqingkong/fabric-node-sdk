# TransactionID

## TransactionID

代表交易标识符的类。提供在创建此对象的实例时自动创建“ nonce”值的功能。

#### new TransactionID(signer_or_userContext, admin)

根据用户的证书建立一个新的交易 ID，并自动生成一个现时值。

- 参数

| 名称                  | 类型                                                                                       | 描述                                                                                                                                                  |
| --------------------- | ------------------------------------------------------------------------------------------ | ----------------------------------------------------------------------------------------------------------------------------------------------------- |
| signer_or_userContext | [Identity](https://hyperledger.github.io/fabric-sdk-node/release-1.4/global.html#Identity) | [Identity](https://hyperledger.github.io/fabric-sdk-node/release-1.4/global.html#Identity)实例，为该事务 ID 提供唯一的基础。这也可能是{@User}的实例。 |
| admin                 | boolean                                                                                    | 指示此实例将用于管理事务。                                                                                                                            |

### Methods

#### getNonce()

随机数值

#### getTransactionID()

交易编号

#### isAdmin()

指示是否为管理员生成此 transactionID

---
