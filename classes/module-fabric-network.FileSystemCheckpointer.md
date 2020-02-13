# fabric-network.FileSystemCheckpointer

## [fabric-network](https://hyperledger.github.io/fabric-sdk-node/release-1.4/module-fabric-network.html)~ FileSystemCheckpointer

在每个事件侦听器的文件中创建检查点

#### new FileSystemCheckpointer(channelName, listenerName [, options])

- 参数

| 名称         | 类型                                                         | 必要性 | 描述                                                         |
| ------------ | ------------------------------------------------------------ | ------ | ------------------------------------------------------------ |
| channelName  | string                                                       |        | 网络                                                         |
| listenerName | string                                                       |        | 侦听器的名称是检查点 The name of the listener being checkpointer |
| options      | [module:fabric-network.FileSystemCheckpointer~FileSystemCheckpointerOptions](https://hyperledger.github.io/fabric-sdk-node/release-1.4/module-fabric-network.FileSystemCheckpointer.html#~FileSystemCheckpointerOptions) | 可选   |                                                              |

### 扩展

- module:fabric-network~BaseCheckpointer

### Methods

#### &lt;async&gt; load()

#### &lt;async&gt; loadLatestCheckpoint()

#### &lt;async&gt; save()

### 类型定义

#### FileSystemCheckpointerOptions

- 属性

| 名称      | 类型   | 必要性 | 描述                         |
| --------- | ------ | ------ | ---------------------------- |
| basePath  | string | 可选   | 将存储检查点的目录           |
| maxLength | number | 可选   | 检查指针中可以包含的最大块数 |

***