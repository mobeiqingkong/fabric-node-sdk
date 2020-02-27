# BlockDecoder

## 说明

实用程序类，用于将 Hyperledger Fabric 块消息的 protobuf 编码的字节数组转换为纯 Javascript 对象

### new BlockDecoder()

### Methods

#### &lt;static&gt; decode(block_bytes)

构造一个 JSON 对象，其中包含来自 protobuf 编码的"Block"字节的所有已解码值。

- 参数

| 名称        | 类型               | 描述                     |
| ----------- | ------------------ | ------------------------ |
| block_bytes | Array.&lt;byte&gt; | Block 协议消息的编码字节 |

- 返回结果

  一个 Block 对象

  - 类型

    [Block](https://hyperledger.github.io/fabric-sdk-node/release-1.4/global.html#Block)

#### &lt;static&gt;decodeBlock(block_data)

构造一个对象，该对象包含来自 protobuf 编码的"Block"对象的所有解码值

- 参数

| 名称        | 类型   | 描述                                |
| ----------- | ------ | ----------------------------------- |
| block_bytes | Object | 表示 protobuf common.Block 的对象。 |

- 返回结果

  完全解码的 protobuf common.Block 对象。

  - 类型

    [Block](https://hyperledger.github.io/fabric-sdk-node/release-1.4/global.html#Block)

#### &lt;static&gt;decodeTransaction(processed_transaction_bytes)

构造一个对象，该对象包含来自 protobuf 编码的" ProcessedTransaction"字节的所有解码值

- 参数

| 名称                        | 类型               | 描述                                           |
| --------------------------- | ------------------ | ---------------------------------------------- |
| processed_transaction_bytes | Array.&lt;byte&gt; | Protobuf 消息" ProcessedTransaction"的编码字节 |

- 返回结果

  完全解码的 ProcessedTransaction 对象

  - 类型

    [ProcessedTransaction](https://hyperledger.github.io/fabric-sdk-node/release-1.4/global.html#ProcessedTransaction)

---
