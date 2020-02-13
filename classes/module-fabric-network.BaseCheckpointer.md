# fabric-network.BaseCheckpointer

## [fabric-network](https://hyperledger.github.io/fabric-sdk-node/release-1.4/module-fabric-network.html)~ BaseCheckpointer

基本检查点为检查点提供接口

#### new BaseCheckpointer(options)

The constructor

- 参数

| 名称    | 类型   | 描述             |
| ------- | ------ | ---------------- |
| options | Object | 配置检查点的选项 |

### Methods

#### &lt;async&gt; load()

加载最新的检查点

返回结果

- 对象参数具有键 blockNumber：字符串和值[module:fabric-network.BaseCheckpointer~Checkpoint](https://hyperledger.github.io/fabric-sdk-node/release-1.4/module-fabric-network.BaseCheckpointer.html#~Checkpoint)

  - 类型

    [module:fabric-network.BaseCheckpointer~Checkpoint](https://hyperledger.github.io/fabric-sdk-node/release-1.4/module-fabric-network.BaseCheckpointer.html#~Checkpoint) &#124;Object

#### &lt;async&gt; loadLatestCheckpoint()

加载最早的不完整检查点，以决定从哪个块重播

返回结果

- 类型

  [module:fabric-network.BaseCheckpointer~Checkpoint](https://hyperledger.github.io/fabric-sdk-node/release-1.4/module-fabric-network.BaseCheckpointer.html#~Checkpoint)

#### &lt;async&gt; save(transactionId, blockNumber, expectedTotal)

更新存储机制

- 参数

| 名称          | 类型   | 描述               |
| ------------- | ------ | ------------------ |
| transactionId | String | 交易 ID            |
| blockNumber   | Number | 块号               |
| expectedTotal | Number | 此块中预期的事件数 |

#### setChaincodeId(chaincodeId)

设置链码 ID 以将侦听器分组在一起

- 参数

| 名称          | 类型   | 描述        |
| ------------- | ------ | ----------- |
| transactionId | String | chaincodeId |

### 类型定义

#### Checkpoint

类型

- 参数

| 名称           | 类型                 | 必要性 | 描述               |
| -------------- | -------------------- | ------ | ------------------ |
| blockNumber    | number               |        |                    |
| transactionIds | Array.&lt;string&gt; |        |                    |
| expectedTotal  | number               | 可选   | 此块中预期的事件数 |
