# EndorsementPolicy

## EndorsementPolicy

控制背书策略的构造，以将其传递到实例化链码的调用中

#### new EndorsementPolicy()

### Methods

&lt;static&gt; buildPolicy(msps, policy)

构造背书政策信封。如果不存在可选的"policy"对象，则返回默认策略"a signature by any member from any of the organizations corresponding to the array of member service providers"(由与成员服务提供者阵列相对应的任何组织的任何成员签名)。

- 参数

| 名称   | 类型                                                         | 描述                                                         |
| :----- | :----------------------------------------------------------- | ------------------------------------------------------------ |
| msps   | Array.&lt;[MSP](https://hyperledger.github.io/fabric-sdk-node/release-1.4/MSP.html)&gt; | 成员服务提供者对象数组，代表要构建的背书策略的参与者         |
| policy | [Policy](https://hyperledger.github.io/fabric-sdk-node/release-1.4/global.html#Policy) | 策略规范。它具有两个高级属性：身份(identities)和策略(policy)。有关详细信息，请参见[Policy](https://hyperledger.github.io/fabric-sdk-node/release-1.4/global.html#Policy)的类型定义。 |