# Global

## global

### Members

#### &lt;constant&gt; CLIENT

CLIENT 表示此身份正在充当客户端

#### &lt;constant&gt; HFAFFILIATIONMGR

HFAFFILIATIONMGR 是一个布尔属性，它允许身份管理从属关系

#### &lt;constant&gt; HFGENCRL

HFGENCRL 是允许身份生成 CRL 的属性

#### &lt;constant&gt; HFINTERMEDIATECA

HFINTERMEDIATECA 是一个布尔属性，它允许身份注册为中间 CA

#### &lt;constant&gt; HFREGISTRARATTRIBUTES

HFREGISTRARATTRIBUTES 是一个属性，该属性具有允许注册服务商为身份注册的属性列表

#### &lt;constant&gt; HFREGISTRARDELEGATEROLES

HFREGISTRARDELEGATEROLES 是允许注册服务商为其"hf.Registrar.Roles''属性将指定的角色赋予 registree 的属性

#### &lt;constant&gt; HFREGISTRARROLES

HFREGISTRARROLES 是允许注册服务商管理指定角色的身份的属性

#### &lt;constant&gt; HFREVOKER

HFREVOKER 是一个布尔属性，它允许身份重新调用用户和/或证书

#### &lt;constant&gt; jsSHA3

实现哈希原函数。

#### &lt;constant&gt; ORDERER

ORDERER 指示身份正在充当orderer

#### &lt;constant&gt; PEER

PEER 表示身份正在充当peer

#### &lt;constant&gt; USER

USER 表示身份正在充当用户

### Methods

#### bitsToBytes(arr)

从 bitArray 转换为字节(请参阅 SJCL 的编解码器

- 参数

| 名称 | 类型                 | 描述                       |
| ---- | -------------------- | -------------------------- |
| arr  | Array.&lt;number&gt; | a bitArray to convert from |

- 返回结果

  从 bitArray 转换的字节(bytes )

#### bytesToBits(bytes)

从字节转换为 bitArray(请参阅 SJCL 的编解码器)

- 参数

| 名称  | 类型                 | 描述         |
| ----- | -------------------- | ------------ |
| bytes | Array.&lt;number&gt; | 要转换的字节 |

- 返回结果

  从字节转换为 bitArray

#### loadConfigGroup(config_items, versions, group, name, org, top)

加载配置组的实用方法

- 参数

| 名称         | 类型   | 描述                     |
| ------------ | ------ | ------------------------ |
| config_items | Object | 配置中找到的值的持有者   |
| versions     | Object |                          |
| group        | Object | 用于递归调用             |
| name         | string | 用于帮助递归调用         |
| org          | string | 机构(Organizational)名称 |
| top          | string | 处理小组结构上的差异     |

- 参见:
  - /protos/common/configtx.proto

#### loadConfigValue()

加载配置值的实用方法

- 参见
  - /protos/common/configtx.proto
  - /protos/msp/mspconfig.proto
  - /protos/orderer/configuration.proto
  - /protos/peer/configuration.proto

#### package(chaincodePath, chaincodeType, devmode, metadataPath)

打包链码的实用程序功能。内容将作为字节数组返回。

- 参数

| 名称          | 类型    | 描述                                                           |
| ------------- | ------- | -------------------------------------------------------------- |
| chaincodePath | string  | 必要。链码源代码位置的路径字符串                               |
| chaincodeType | string  | 链码类型['golang','node','car','java']的字符串(默认为'golang') |
| devmode       | boolean | 设置为 true 以使用 Chaincode 开发模式                          |
| metadataPath  | string  | 可选。包含元数据描述符的顶级目录的路径                         |

- 返回结果

  将数据作为字节数组的 Promise

  - 类型

    Promise

#### toEnvelope(signature, proposal_bytes)

将 proposal.proto:SignedProposal 转换为 common.proto:Envelope

- 参数

| 名称           | 类型 | 描述 |
| -------------- | ---- | ---- |
| signature      |      |      |
| proposal_bytes |      |      |

### Type Definitions

#### AffiliationRequest

- 类型

  - Object

- 属性

| 名称   | 类型    | 描述                                                                                                                                                                                                                                                                                                                                                                                                    |
| ------ | ------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| name   | string  | 必要。建立联系的途径                                                                                                                                                                                                                                                                                                                                                                                    |
| caname | string  | 可选。将请求发送到 fabric CA 服务器内的 CA 的名称                                                                                                                                                                                                                                                                                                                                                       |
| force  | boolean | 可选。<br>- 对于创建从属关系请求，如果不存在任何父从属关系并且'force'为 true，则还要创建所有父从属关系。<br>- 对于删除隶属关系请求，如果 force 为 true 且存在任何子隶属关系或任何与该隶属关系或子隶属关系相关的标识，这些身份和子从属关系将被删除；否则，将返回错误。<br>- 对于更新隶属关系请求，如果任何身份与此隶属关系相关联，则“ force”为 true 会导致这些身份的隶属关系被重命名；否则，将返回错误。 |

#### AttributeRequest

- 类型

  - Object

- 属性

| 名称     | 类型    | 描述                           |
| -------- | ------- | ------------------------------ |
| name     | string  | 要包含在证书中的属性名称       |
| optional | boolean | 如果身份不具有属性，则抛出错误 |

#### Block

完全解码的 protobuf 消息“块”的对象。

块可以包含通道的配置或通道上的事务。

Block 对象将具有以下对象结构。

```javascript
header
	number -- {int}
	previous_hash -- {byte[]}
	data_hash -- {byte[]}
data
	data -- {array}
		signature -- {byte[]}
		payload
			header -- {Header}
			data -- {ConfigEnvelope | Transaction}
metadata
	metadata -- {array} #each array item has it's own layout
		[0] #SIGNATURES
			signatures -- {MetadataSignature[]}
		[1] #LAST_CONFIG
			value
				index -- {number}
				signatures -- {MetadataSignature[]}
		[2] #TRANSACTIONS_FILTER
				{int[]} #see TxValidationCode in proto/peer/transaction.proto
```

- 类型
  - Object

##### 示例

获取块号:

```javascript
var block_num = block.header.number;
```

获取交易数量，包括无效交易:

```javascript
var block_num = block.data.data.legnth;
```

获取代码块中第一个事务的 ID:

```javascript
var tx_id = block.data.data[0].payload.header.channel_header.tx_id;
```

#### BlockchainInfo

- 类型

  - Object

- 属性

| 名称              | 类型               | 描述                                                                                                                                |
| ----------------- | ------------------ | ----------------------------------------------------------------------------------------------------------------------------------- |
| height            | number             | 通道分类帐中有多少个区块                                                                                                            |
| currentBlockHash  | Array.&lt;byte&gt; | 块哈希是通过对以下串联的 ASN.1 编码字节进行哈希计算得出的:块编号，先前的块哈希和当前的块数据哈希。区块哈希的链保证了分类账的不变性 |
| previousBlockHash | Array.&lt;byte&gt; | 前一个块的块哈希。                                                                                                                  |

#### BroadcastResponse

- 类型

  - Object

- 属性

| 名称   | 类型   | 描述                             |
| ------ | ------ | -------------------------------- |
| status | string | 值为“ SUCCESS”或描述性错误字符串 |
| info   | string | 可选。有关状态的其他信息         |

#### ChaincodeEvent

- 类型

  - Object

- 属性

| 名称         | 类型               | 描述                                                                                      |
| ------------ | ------------------ | ----------------------------------------------------------------------------------------- |
| chaincode_id | string             | 引发此事件的链码的名称。                                                                  |
| tx_id        | string             | 此事件的交易 ID。                                                                         |
| event_name   | string             | 背书期间链码所设置的作为此事件的 event_name 的字符串。 stub.SetEvent(event_name, payload) |
| payload      | Array.&lt;byte&gt; | 链代码调用 stub.SetEvent(event_name,payload)时设置的应用程序特定的字节数组                |

#### ChaincodeInfo

- 类型

  - Object

- 属性

| 名称    | 类型   | 描述                                                                              |
| ------- | ------ | --------------------------------------------------------------------------------- |
| name    | string |                                                                                   |
| version | string |                                                                                   |
| path    | string | 安装/实例化事务指定的路径                                                         |
| input   | string | 实例化 chaincode 函数及其参数。如果查询返回有关已安装链码的信息，则该字段将为空。 |
| escc    | string | 此链码的 ESCC 名称。如果查询返回有关已安装链码的信息，则该字段将为空。            |
| vscc    | string | 此链码的 VSCC 的名称。如果查询返回有关已安装链码的信息，则该字段将为空。          |

#### ChaincodeInstallRequest

- 类型

  - Object

- 属性

| 名称             | 类型                                                                                                                | 描述                                                                                                                                                                                                                                                                                            |
| ---------------- | ------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| targets          | Array.&lt;[Peer](./Peer.md&gt;&#124;Array.&lt;string&gt; | 可选。将安装链码的[Peer](./Peer.md对象或 peer 名称的数组。如果排除在外，则将使用在公共连接配置文件中定义的分配给该客户组织的同级。如果包括“ channelNames”属性，则目标 peer 将基于通道中定义的 peer。                                 |
| chaincodePath    | string                                                                                                              | 必要。链码源代码位置的路径。如果 chaincode 类型为 golang，则此路径为标准软件包名称，例如'mycompany.com/myproject/mypackage/mychaincode'                                                                                                                                                         |
| metadataPath     | string                                                                                                              | 可选。包含元数据描述符的顶级目录的路径。                                                                                                                                                                                                                                                        |
| chaincodeId      | string                                                                                                              | 必要。链码名称                                                                                                                                                                                                                                                                                  |
| chaincodeVersion | string                                                                                                              | 必要。链码的版本字符串，例如“ v1”                                                                                                                                                                                                                                                               |
| chaincodePackage | Array.&lt;byte&gt;                                                                                                  | 选的。链码源的归档内容的字节数组。归档文件必须具有一个“ src”文件夹，其中包含与“ chaincodePath”字段相对应的子文件夹。例如，如果 chaincodePath 为“mycompany.com/myproject/mypackage/mychaincode”，则归档文件必须包含链目录源代码所在的文件夹“src/mycompany.com/myproject/mypackage/mychaincode”。 |
| chaincodeType    | string                                                                                                              | 可选。链码的类型。 “ golang”，“ car”，“ node”或“ java”之一。默认值为“ golang”。                                                                                                                                                                                                                 |
| channelNames     | Array.&lt;string&gt;&#124;string                                                                                    | 可选。没有提供目标时，将在加载的公共连接配置文件中搜索合适的目标 peer。将选择在此属性命名的通道中以及在此客户的组织中定义且在命名通道上具有背书或链码查询角色的同级。                                                                                                                           |
| txId             | [TransactionID](./TransactionID.md                       | 可选。此请求的 TransactionID 对象。                                                                                                                                                                                                                                                             |

#### ChaincodeInstantiateUpgradeRequest

- 类型
  - Object
- 属性

| 名称               | 类型                                                                                                                                                                                     | 描述                                                                                                                                                                                                                                                                                                                                                                                                                                                                            |
| ------------------ | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| targets            | Array.[[Peer](./Peer.md](<[Peer](./Peer.md>)&#124;Array.&lt;string&gt; | 可选。将安装链码的[Peer](./Peer.md对象或 peer 名称的数组。如果排除在外，则将使用在公共连接配置文件中定义的分配给该客户组织的同级。如果包括“ channelNames”属性，则目标 peer 将基于通道中定义的 Peer。                                                                                                                                                                                                                 |
| chaincodeType      | string                                                                                                                                                                                   | 可选。链码的类型。 “ golang”，“ car”，“ node”或“ java”之一。默认值为“ golang”。                                                                                                                                                                                                                                                                                                                                                                                                 |
| chaincodeId        | string                                                                                                                                                                                   | 必要。链码名称                                                                                                                                                                                                                                                                                                                                                                                                                                                                  |
| chaincodeVersion   | string                                                                                                                                                                                   | 必要。链码的版本字符串，例如“ v1”                                                                                                                                                                                                                                                                                                                                                                                                                                               |
| txId               | [TransactionID](./TransactionID.md                                                                                            | 可选。此请求的 TransactionID 对象。                                                                                                                                                                                                                                                                                                                                                                                                                                             |
| collections-config | string                                                                                                                                                                                   | 可选。集合配置的路径。可以在本[教程(tutorial)](./tutorial-private-data.md中找到更多详细信息                                                                                                                                                                                                                                                                                                                          |
| transientMap       | object                                                                                                                                                                                   | 可选。带有 String 属性名称和 Buffer 属性值的对象，可以由链码使用，但不能保存在分类帐中。可以使用此技术将诸如用于加密的密码信息的数据传递到链码。应将要保留在私有数据存储区(集合)中的数据传递给 transientMap 中的链码。                                                                                                                                                                                                                                                        |
| fcn                | string                                                                                                                                                                                   | 可选。在目标链码中调用 stub.GetFunctionAndParameters()时要返回的函数名称。默认为'init';如果要不传入任何函数名，请显式传入 fcn，其值为 null 或”(空字符串)                                                                                                                                                                                                                                                                                                                      |
| args               | Array.&lt;string&gt;                                                                                                                                                                     | 可选。传递给 fcn 值所标识的函数的字符串参数数组。                                                                                                                                                                                                                                                                                                                                                                                                                               |
| endorsement-policy | Object                                                                                                                                                                                   | 可选。此链码的 EndorsementPolicy 对象(请参见下面的示例)。如果未指定，则使用默认策略“由来自与成员服务提供者阵列相对应的任何组织的任何成员的签名”("a signature by any member from any of the organizations corresponding to the array of member service providers")。警告:不建议将默认策略用于生产，因为这将允许应用程序绕过投标背书，并直接向orderer发送手动构造的事务(在写入集中具有任意输出)。分配给创建签名的客户端实例的用户上下文将允许成功验证交易并将其提交到分类账。 |

##### 示例

背书策略:“由任何一个组织的成员签署”

```javascript
{
  identities: [
    { role: { name: "member", mspId: "org1" }},
    { role: { name: "member", mspId: "org2" }}
  ],
  policy: {
    "1-of": [{ "signed-by": 0 }, { "signed-by": 1 }]
  }
}
```

背书政策:“由 ordererOrg 的管理员和一个 peer 组织的任何成员签署”

```javascript
{
  identities: [
    { role: { name: "member", mspId: "peerOrg1" }},
    { role: { name: "member", mspId: "peerOrg2" }},
    { role: { name: "admin", mspId: "ordererOrg" }}
  ],
  policy: {
    "2-of": [
      { "signed-by": 2},
      { "1-of": [{ "signed-by": 0 }, { "signed-by": 1 }]}
    ]
  }
}
```

#### ChaincodeInvocationSpec

背书提议，包括要调用的链码的名称和要传递给链码的参数。

“ ChaincodeInvocationSpec”具有以下对象结构。

```javascript
chaincode_spec
	type -- {int}
	chaincode_id
		path -- {string}
		name -- {string}
		version -- {string}
	input
		args -- {byte[][]}
		decorations -- {map of string to byte[]}
	timeout -- {int}
```

- 类型
  - Object

#### ChaincodeInvokeRequest

该对象包含发现服务将使用的许多属性。

- 类型

  - Object

- 属性

| 名称               | 类型                                                                                                                                                                                     | 描述                                                                                                                                                                                                                                                                                                                                                                                                                                             |
| ------------------ | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| targets            | Array.[[Peer](./Peer.md](<[Peer](./Peer.md>)&#124;Array.&lt;string&gt; | 可选。背书 peer 对象或 peer 名称作为请求目标的数组。如果省略此参数，则目标列表将包括分配给该通道实例的 peer，它们具有背书角色。                                                                                                                                                                                                                                                                                                                  |
| chaincodeId        | string                                                                                                                                                                                   | 必要。用于处理交易建议的链码的 ID                                                                                                                                                                                                                                                                                                                                                                                                                |
| endorsement_hint   | DiscoveryChaincodeIntereset                                                                                                                                                              | 可选。 [DiscoveryChaincodeInterest](#DiscoveryChaincodeInterest)对象的一个，发现服务将使用该对象来计算适当的背书计划。仅当背书将由可调用其他链码的链码执行背书，或者背书应仅由一个或多个集合中的 peer 进行背书时，才需要该参数。                                                                                                                                            |
| txId               | [TransactionID](./TransactionID.md                                                                                            | 可选。具有交易 ID 和随机数的 TransactionID 对象。 [sendTransactionProposal](./Channel.html#sendTransactionProposal)需要 txId，[generateUnsignedProposal](./Channel.html#generateUnsignedProposal)是可选的                                                                                                                        |
| transientMap       | object                                                                                                                                                                                   | 可选。带有 String 属性名称和 Buffer 属性值的对象，可以由链码使用，但不能保存在分类帐中。可以使用此技术将诸如用于加密的密码信息的数据传递到链码。应将要保留在私有数据存储区(集合)中的数据传递给 transientMap 中的链码。                                                                                                                                                                                                                         |
| fcn                | string                                                                                                                                                                                   | 可选。在目标链码中调用 stub.GetFunctionAndParameters()时要返回的函数名称。默认为'invoke'。                                                                                                                                                                                                                                                                                                                                                       |
| args               | Array.&lt;string&gt;                                                                                                                                                                     | 可选。传递给 fcn 值所标识的函数的字符串参数数组。                                                                                                                                                                                                                                                                                                                                                                                                |
| required           | Array.&lt;string&gt;                                                                                                                                                                     | 可选。字符串数组，表示背书所需的 peer 的名称。这些将是发送提案的唯一 peer。该列表仅适用于使用发现服务的背书。此属性由[DiscoveryEndorsementHandler](./DiscoveryEndorsementHandler.md使用。                                                                                                                                                                                                                                                                                         |
| ignore             | Array.&lt;string&gt;                                                                                                                                                                     | 可选。字符串数组，表示背书应忽略的 peer 的名称。该列表仅适用于使用发现服务的背书。此属性由[DiscoveryEndorsementHandler](./DiscoveryEndorsementHandler.md使用。                                                                                                                                                                                                                        |
| preferred          | Array.&lt;string&gt;                                                                                                                                                                     | 可选。字符串数组，代表应该由背书赋予优先级的 peer 的名称。优先级意味着当背书计划在组中有更多 peer，然后需要满足背书策略时，将首先选择这些 peer 进行背书。该列表仅适用于使用发现服务的背书。此属性由[DiscoveryEndorsementHandler](./DiscoveryEndorsementHandler.md使用。                                                                                                               |
| requiredOrgs       | Array.&lt;string&gt;                                                                                                                                                                     | 可选。字符串数组，表示背书所需的组织的 MSP ID。仅向这些组织中的 peer 发送提议。该列表仅适用于使用发现服务的背书。此属性由[DiscoveryEndorsementHandler](./DiscoveryEndorsementHandler.md使用。                                                                                                                                                                                         |
| ignoreOrgs         | Array.&lt;string&gt;                                                                                                                                                                     | 可选。字符串数组，表示背书的组织应忽略的 MSP ID 的名称。在将组织中的 peer 添加到优先级列表之前，可以使用可选属性 preferredHeightGap 考虑其分类帐高度。该列表仅适用于使用发现服务的背书。此属性由[DiscoveryEndorsementHandler](./DiscoveryEndorsementHandler.md使用。                                                                                                                  |
| preferredOrgs      | Array.&lt;string&gt;                                                                                                                                                                     | 可选。一组字符串，代表应由背书赋予优先级的组织的 MSP ID 的名称。在将组织中的 peer 添加到优先级列表之前，可以使用可选属性 preferredHeightGap 考虑其分类帐高度。该列表仅适用于使用发现服务的背书。此属性由[DiscoveryEndorsementHandler](./DiscoveryEndorsementHandler.md使用。                                                                                                          |
| preferredHeightGap | Number                                                                                                                                                                                   | 可选。一个整数，表示一个 peer 的区块高度的最大差值和在背书计划内的一组 peer 中找到的最高区块高度，该整数允许成为首选 peer。如果 peer 的块高度比最高块高度小一个值大于此值，则不会给它一个优先级。没有默认值，如果未提供此值，则在添加到首选列表时将不考虑 peer 的块高。该列表仅适用于使用发现服务的背书。此属性由[DiscoveryEndorsementHandler](./DiscoveryEndorsementHandler.md使用。 |
| sort               | string                                                                                                                                                                                   | 可选。一个字符串值，指示应如何选择组内的 peer。 “ ledgerHeight”，将 peer 按通道分类账上的块数降序排列。“随机”，从列表中随机拉 peer，preferred 将首先拉。默认设置为按分类帐高度排序。                                                                                                                                                                                                                                                             |

#### ChaincodeQueryRequest

- 类型

  - Object

- 属性

| 名称            | 类型                                                                                                                                                                                     | 描述                                                                                                                                                                                                                     |
| --------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| targets         | Array.[[Peer](./Peer.md](<[Peer](./Peer.md>)&#124;Array.&lt;string&gt; | 可选。当未提供时，将使用将接收此请求的 peer，添加到此通道对象的 peer 列表                                                                                                                                                |
| chaincodeId     | string                                                                                                                                                                                   | 必要。用于处理交易建议的链码的 ID                                                                                                                                                                                        |
| transientMap    | object                                                                                                                                                                                   | 可选。带有 String 属性名称和 Buffer 属性值的对象，可以由链码使用，但不能保存在分类帐中。可以使用此技术将诸如用于加密的密码信息的数据传递到链码。应将要保留在私有数据存储区(集合)中的数据传递给 transientMap 中的链码。 |
| fcn             | string                                                                                                                                                                                   | 可选。在目标链码中调用 stub.GetFunctionAndParameters()时要返回的函数名称。默认为“invoke”                                                                                                                                 |
| args            | Array.&lt;string&gt;                                                                                                                                                                     | 特定于链码的“ Invoke”方法的字符串参数数组                                                                                                                                                                                |
| request_timeout | integer                                                                                                                                                                                  | 用于此请求的超时值                                                                                                                                                                                                       |
| txId            | [TransactionID](./TransactionID.md                                                                                           | 可选。用于查询的交易 ID。                                                                                                                                                                                                |

#### ChaincodeQueryResponse

- 类型

  - Object

- 属性

| 名称       | 类型                                                                                                               | 描述 |
| ---------- | ------------------------------------------------------------------------------------------------------------------ | ---- |
| chaincodes | Array.&lt;[ChaincodeInfo](#ChaincodeInfo)&gt; |      |

#### ChannelConfigGroup

通道本身的块中包含控制结构应如何维护通道的配置设置。当一个块包含通道配置时，通道配置记录是块数据数组中的唯一项。每个块(包括配置块本身)都有一个指向最新配置块的指针，从而可以轻松查询最新的通道配置设置。

通道配置记录将具有以下对象结构。

```go
version -- {int}
mod_policy -- {string}
groups
 Orderer
		version -- {int}
		groups
			&lt;orderer_org_name&gt; -- {OrganizationConfigGroup}
		values
			ConsensusType
				version -- {int}
				mod_policy -- {string}
				value
					type -- {string}
			BatchSize
				version -- {int}
				mod_policy -- {string}
				value
					max_message_count -- {int}
					absolute_max_bytes -- {int}
					preferred_max_bytes -- {int}
			BatchTimeout
				version -- {int}
				mod_policy -- {string}
				value
					timeout -- {duration}
			ChannelRestrictions
				version -- {int}
				mod_policy -- {string}
				value
					max_count -- {int}
		policies
			Admins
				version -- {int}
				mod_policy -- {string}
				policy -- {ImplicitMetaPolicy}
			Writers
				version -- {int}
				mod_policy -- {string}
				policy -- {ImplicitMetaPolicy}
			Readers
				version -- {int}
				mod_policy -- {string}
				policy -- {ImplicitMetaPolicy}
			BlockValidation
				version -- {int}
				mod_policy -- {string}
				policy -- {SignaturePolicy}
	Application
		version -- {int}
		groups
			&lt;peer_org_name&gt; -- {OrganizationConfigGroup}
		values
		policies
			Admins
				version -- {int}
				mod_policy -- {string}
				policy -- {ImplicitMetaPolicy}
			Writers
				version -- {int}
				mod_policy -- {string}
				policy -- {ImplicitMetaPolicy}
			Readers
				version -- {int}
				mod_policy -- {string}
				policy -- {ImplicitMetaPolicy}
values
	OrdererAddresses
		version -- {int}
		mod_policy -- {string}
		value
			addresses -- {array}
				{string - host:port}
	HashingAlgorithm
		version -- {int}
		mod_policy -- {string}
		value
			name -- {string}
	BlockDataHashingStructure
		version -- {int}
		mod_policy -- {string}
		value
			width -- {int}
	Consortium
		version -- {int}
		mod_policy -- {string}
		value
			name -- {string}
```

- 类型

  - Object

- 属性

| 名称                                            | 类型                                                                                                                     | 描述                                                         |
| ----------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------ | ------------------------------------------------------------ |
| groups.Orderer.groups.&lt;orderer_org_name&gt;  | [OrganizationConfigGroup](#OrganizationConfigGroup) | 这些是网络上定义的 Orderer 组织名称                          |
| groups.Application.groups.&lt;peer_org_name&gt; | [OrganizationConfigGroup](#OrganizationConfigGroup) | 这些是网络上定义的 peer 组织名称                             |
| policy                                          | [ImplicitMetaPolicy](#ImplicitMetaPolicy)          | 这些策略指向其他策略，并指定"ANY"，"MAJORITY"或"ALL"中的阈值 |

#### ChannelInfo

- 类型

  - Object

- 属性

| 名称       | 类型   | 描述 |
| ---------- | ------ | ---- |
| channel_id | string |      |

#### ChannelPeerRoles

- 类型

  - Object

- 属性

| 名称           | 类型    | 描述                                                                                                                                      |
| -------------- | ------- | ----------------------------------------------------------------------------------------------------------------------------------------- |
| endorsingPeer  | boolean | 可选。可以向该 peer 发送背书交易提议。peer 必须安装链码。该应用程序还可以使用此属性来确定哪些 peer 发送链码安装请求。默认值:true         |
| chaincodeQuery | boolean | 可选。可以向该 peer 发送仅作为查询的交易建议。peer 必须安装链码。该应用程序还可以使用此属性来确定哪些 peer 发送链码安装请求。默认值:true |
| ledgerQuery    | boolean | 可选。可以向此 peer 发送不需要链码的查询提议，例如 queryBlock()，queryTransaction()等。默认值:true                                       |
| eventSource    | boolean | 可选。此 peer 可能是事件侦听器注册的目标？所有 peer 都可以产生事件，但是该应用通常仅需要连接到一个事件。默认值:true                      |
| discover       | boolean | 可选。该 peer 可能是服务发现的目标。默认值:true                                                                                          |

#### ChannelQueryResponse

- 类型

  - Object

- 属性

| 名称     | 类型                                                                                                           | 描述 |
| -------- | -------------------------------------------------------------------------------------------------------------- | ---- |
| channels | Array.&lt;[ChannelInfo](#ChannelInfo)&gt; |      |

#### ChannelRequest

- 类型

  - Object

- 属性

| 名称       | 类型                                                                                                                                               | 描述                                                                                                                                                                                                                                                                               |
| ---------- | -------------------------------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| name       | string                                                                                                                                             | 必要。新通道的名称                                                                                                                                                                                                                                                                 |
| orderer    | [Orderer](./Orderer.md &#124; string                                                    | 必要。代表要发送通道创建请求的 orderer 节点的 orderer 对象或 orderer 名称                                                                                                                                                                                                          |
| envelope   | Array.&lt;byte&gt;                                                                                                                                 | 可选。信封对象的字节，其中包含初始化此通道所需的所有设置和签名。该信封将由命令行工具[configtxgen](http://hyperledger-fabric.readthedocs.io/en/latest/configtxgen.md 或 [configtxlator](https://github.com/hyperledger/fabric/blob/master/examples/configtxupdate/README.md)创建 |
| config     | Array.&lt;byte&gt;                                                                                                                                 | 可选。从由 configtxgen 工具创建的 ConfigEnvelope 中提取的 Protobuf ConfigUpdate 对象。请参见[extractChannelConfig()](./Client.html#extractChannelConfig)。 ConfigUpdate 对象也可以由 configtxlator 工具创建。              |
| signatures | Array.&lt;[ConfigSignature](#ConfigSignature)&gt; &#124; Array.&lt;string&gt; | 必要。使用“config"参数时，通道创建或更新策略所需的签名列表。                                                                                                                                                                                                                       |
| txId       | [TransactionID](./TransactionID.md                                                      | 必要。具有交易 ID 和随机数的 TransactionID 对象                                                                                                                                                                                                                                    |

#### collectionConfig

- 类型

  - Object

- 属性

| 名称              | 类型                                      | 描述                                       |
| ----------------- | ----------------------------------------- | ------------------------------------------ |
| name              | string                                    |                                            |
| policy            |                                           |                                            |
| maxPeerCount      | number                                    | 整数                                       |
| requiredPeerCount | number                                    | 整数                                       |
| blockToLive       | Long&#124;number&#124;string&#124; Object | 参数将被转换为 unsigned int64 as Long      |
| memberOnlyRead    | boolean                                   | 表示是否只有集合成员客户端可以读取私有数据 |

#### CollectionQueryOptions

- 类型

  - Object

- 属性

| 名称        | 类型                                                                                    | 描述                                                                    |
| ----------- | --------------------------------------------------------------------------------------- | ----------------------------------------------------------------------- |
| chaincodeId | string                                                                                  | 必要。链码名称                                                          |
| target      | string&#124;[Peer](./Peer.md | 可选。当未提供此通道对象中的第一个 peer 时，将使用将接收此请求的 peer。 |

#### CollectionQueryResponse

- 类型

  - Object

- 属性

| 名称               | 类型                                                                                   | 描述                                                                                                                                                                |
| ------------------ | -------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| type               | string                                                                                 | Collection 类型                                                                                                                                                     |
| name               | string                                                                                 | 表示的链码内的集合名称                                                                                                                                              |
| required_peer_coun | number                                                                                 | 背书时将发送最少数量的 peer 私有数据。如果至少没有传播到这个数目的 peer，则背书将失败。                                                                             |
| maximum_peer_count | number                                                                                 | 背书后将发送私人数据的最大 peer 数。此数字必须大于 required_peer_count。                                                                                            |
| block_to_live      | number                                                                                 | 收集数据到期之后的块数。例如，如果将该值设置为 10，则最后由块号 100 修改的密钥将在块号 111 处清除。零值与 MaxUint64 相同，不会清除数据。                            |
| member_read_only   | boolean                                                                                | 成员仅读取访问权限表示是仅集合成员客户端可以读取私有数据(如果设置为 true)，还是非成员可以读取数据(如果设置为 false，例如，如果您想在链码中实现更精细的访问逻辑) |
| policy             | [Policy](#Policy) | "member_orgs_policy"政策                                                                                                                                            |

#### ConfigEnvelope

ConfigEnvelope 包含通道配置数据，并且是配置块的主要内容。另一种类型的块是包含背书交易的块，其中主要内容是[Transaction](#Transaction)数组。

"ConfigEnvelope"将具有以下对象结构。

```javascript
config
	sequence -- {int}
	channel_group -- {ConfigGroup}
	type -- {int}
last_update
	signature -- {byte[]}
	payload
		header -- {Header}
		data -- {ConfigUpdateEnvelope}
```

- 类型
  - Object

#### ConfigSignature

- 类型

  - Object

- 属性

| 名称             | 类型               | 描述                                                                                                               |
| ---------------- | ------------------ | ------------------------------------------------------------------------------------------------------------------ |
| signature_header | Array.&lt;byte&gt; | [SignatureHeader](#SignatureHeader)的编码字节 |
| signature        | Array.&lt;byte&gt; | 签名的串联后签名的编码字节                                                                                         |

#### ConfigUpdateEnvelope

原始消息"ConfigUpdateEnvelope"的对象。

"ConfigUpdateEnvelope"将具有以下对象结构。

```javascript
config_update
	channel_id -- {string}
	read_set -- {ChannelConfigGroup}
	write_set -- {ChannelConfigGroup}
	type -- {int}
signatures -- {array}
	signature_header -- {SignatureHeader}
	signature -- {byte[]}
```

- 类型

  - Object

- 属性

| 名称                    | 类型                                                                                                           | 描述                                                                                 |
| ----------------------- | -------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------ |
| config_update.read_set  | [ChannelConfigGroup](#ChannelConfigGroup) | 一组所有正在更新的配置项的当前版本号                                                 |
| config_update.write_set | [ChannelConfigGroup](#ChannelConfigGroup) | 一组所有正在更新的配置项。版本号必须比 read_set 中相同项目的版本号大一，并带有新值。 |

#### ConnectionOpts

- 类型

  - Object

- 属性

| 名称                     | 类型   | 描述                                                                                                                                                                                       |
| ------------------------ | ------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| name                     | string | 可选。为这个远程端点命名。如果未提供名称，则端点将通过其 URL 知道。                                                                                                                        |
| request-timeout          | string | 一个整数值(以毫秒为单位)，用作等待请求响应的最长时间。                                                                                                                                   |
| pem                      | string | PEM 格式的证书文件，用于 gRPC 协议(即 TransportCredentials)。使用 grpcs 协议时必需。                                                                                                     |
| ssl-target-name-override | string | 仅在测试环境中使用，当服务器证书的主机名(在“ CN”字段中)与服务器进程在其上运行的实际主机端点不匹配时，通过将此属性设置为服务器证书的主机名的值，应用程序可以解决客户端 TLS 验证失败的问题 |
| &lt;any&gt;              | any    | 任何其他标准 grpc 调用选项将直接传递到 grpc 服务调用                                                                                                                                       |

#### ConnectOptions

- 类型

  - Object

- 属性

| 名称        | 类型                                                                                             | 描述                                                                                                                                                                                                                                                                                                                                                                               |
| ----------- | ------------------------------------------------------------------------------------------------ | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| full_block  | boolean                                                                                          | 可选。指示与 peer 的连接将向此 ChannelEventHub 发送完整的块或已过滤的块。默认设置是使用过滤后的块建立连接。过滤的块具有提供交易状态和链码事件所需的信息(无有效载荷)。使用未过滤的块(完整块)时，将要求用户有权建立连接以接收完整块。在已过滤的块连接上注册块侦听器可能无法提供足够的信息。                                                                                      |
| startBlock  | number&#124;string                                                                               | 可选。这将具有连接设置，以使用该编号开始将块发送回事件中心。如果与 startBlock 连接，则事件侦听器可能未注册到 startBlock 或 endBlock。如果事件中心应该以它看到的最后一个块开头，则使用字符串“ last_seen”。如果事件中心应以分类账上最旧的块开头，则使用字符串“ oldest”。如果事件中心应以分类帐上的最新块开头，请使用字符串“ latest”或使用 startBlock。默认是从分类账上的最新块开始。 |
| endBlock    | number&#124;string                                                                               | 可选。这将具有连接设置，以结束将块发送回具有该编号的块处的事件中心。如果与 endBlock 连接，则事件侦听器可能未注册到 startBlock 或 endBlock。如果事件中心应该以它看到的最后一个块结尾，则使用字符串'last_seen'。如果事件中心应以分类账上的当前块结尾，则使用字符串“最新”。默认为不停止发送。                                                                                         |
| signedEvent | [SignedEvent](#SignedEvent) | 可选。已签名的事件将发送给 peer。当 fabric 客户端应用程序没有用户的 privateKey 并且无法签署对结构网络的请求时，此选项很有用。                                                                                                                                                                                                                                                      |
| target      | [Peer](./Peer.md&#124;string          | 可选。提供结构事件服务的 peer。使用字符串时，必须为 Channel 分配一个具有该名称的 peer。此 peer 将替换此[Channel](./Channel.md事件中心的当前 peer 端点。                                                                                                                                                                 |
| as_array    | boolean                                                                                          | 可选。仅与链码代码事件一起使用，以指示在块中找到的所有链码事件应作为数组发送到回调，而不是一次发送给默认事件。                                                                                                                                                                                                                                                                     |

#### CouchDBOpts

- 类型

  - Object

- 属性

| 名称 | 类型   | 描述                                                |
| ---- | ------ | --------------------------------------------------- |
| url  | string | CouchDB 实例 URL，形式为 http(s)://:@host:port      |
| name | string | 可选。标识要使用的数据库的名称。默认值:member_db。 |

#### CryptoContent

- 类型

  - Object

- 属性

| 名称          | 类型                                                                                            | 描述                                                                  |
| ------------- | ----------------------------------------------------------------------------------------------- | --------------------------------------------------------------------- |
| privateKey    | string                                                                                          | 私钥的 PEM 文件路径                                                   |
| privateKeyPEM | string                                                                                          | 私钥的 PEM 字符串(如果设置了 privateKey 或 privateKeyObj，则不需要) |
| privateKeyObj | [module:api.Key](./module-api.Key.md | 私钥对象(如果设置了 privateKey 或 privateKeyPEM，则不需要)          |
| signedCert    | string                                                                                          | 证书的 PEM 文件路径                                                   |
| signedCertPEM | string                                                                                          | 证书的 PEM 字符串(如果设置了 signedCert，则不需要)                  |

#### CryptoSetting

- 类型

  - Object

- 属性

| 名称      | 类型    | 描述                                                                                                                                 |
| --------- | ------- | ------------------------------------------------------------------------------------------------------------------------------------ |
| software  | boolean | 加载基于软件的实施(true)或 HSM 实施(false)默认为 true(对于基于软件的实施)，在"crypto-suite-software"设置中指定了特定的实施模块 |
| keysize   | number  | 用于加密套件实例的密钥大小。默认值为设置"crypto-keysize"的值                                                                         |
| algorithm | string  | 数字签名算法，目前仅支持 ECDSA，其值为"EC"                                                                                           |
| hash      | string  | "SHA2"或"SHA3"                                                                                                                       |

#### DiscoveryChaincodeCall

- 类型

  - Object

- 属性

| 名称             | 类型                 | 描述           |
| ---------------- | -------------------- | -------------- |
| name             | string               | 链码的名称     |
| collection_names | Array.&lt;string&gt; | 相关集合的名称 |

- 示例

“单个 chaincode”

```javascript
{
  name: "mychaincode";
}
```

“从链码到链码”

```javascript
[{ name: "mychaincode" }, { name: "myotherchaincode" }];
```

“带有集合的单个 chaincode”

```javascript
{ name: "mychaincode", collection_names: ["mycollection"] }
```

“将链码转换为带有集合的链码”

```javascript
[
    { name: "mychaincode", collection_names: ["mycollection"] },
    { name: "myotherchaincode", collection_names: ["mycollection"] }}
  ]
```

“将链码转换为带有集合的链码”

```javascript
[
    { name: "mychaincode", collection_names: ["mycollection", "myothercollection"] },
    { name: "myotherchaincode", collection_names: ["mycollection", "myothercollection"] }}
  ]
```

#### DiscoveryChaincodeInterest

- 类型

  - Object

- 属性

| 名称       | 类型                                                                                                                                 | 描述                                           |
| ---------- | ------------------------------------------------------------------------------------------------------------------------------------ | ---------------------------------------------- |
| chaincodes | Array.&lt;[DiscoveryChaincodeCall](#DiscoveryChaincodeCall)&gt; | 链码名称和集合将发送到发现服务以计算背书计划。 |

#### DiscoveryChaincodeQuery

请求 DiscoveryResults 给定的列表调用。每个 interest 都是对一个或多个链码的单独调用，其中可能包括对集合的调用。对于每个给定的 interest ，将独立评估背书策略。

- 类型

  - Object

- 属性

| 名称      | 类型                                                                                                                                         | 描述                       |
| --------- | -------------------------------------------------------------------------------------------------------------------------------------------- | -------------------------- |
| interests | Array.&lt;[DiscoveryChaincodeInterest](#DiscoveryChaincodeInterest)&gt; | 在调用链码时定义 interests |

- 示例

“链码，没有集合”

```javascript
{
  interests: [{ chaincodes: [{ name: "mychaincode" }] }];
}
```

“带有集合的链码”

```javascript
{
  interests: [
    {
      chaincodes: [{ name: "mychaincode", collection_names: ["mycollection"] }]
    }
  ];
}
```

“将链码转换为带有集合的链码”

```javascript
{
  interests: [
     { chaincodes: [
         { name: "mychaincode", collection_names: ["mycollection"] }},
         { name: "myotherchaincode", collection_names: ["mycollection"] }}
       ]
     }
  ]
}
```

“查询多个调用”

```javascript
{
  interests: [
     { chaincodes: [
         { name: "mychaincode", collection_names: ["mycollection"] }},
         { name: "myotherchaincode", collection_names: ["mycollection"] }}
       ]
     },
    { chaincodes: [{ name: "mychaincode", collection_names: ["mycollection"] }]},
    { chaincodes: [{ name: "mychaincode"}]}
  ]
}
```

#### DiscoveryResultChaincode

- 类型

  - Object

- 属性

| 名称    | 类型   | 描述 |
| ------- | ------ | ---- |
| name    | string |      |
| version | string |      |

#### DiscoveryResultEndorsementGroup

- 类型

  - Object

- 属性

| 名称  | 类型                                                                                                                           | 描述          |
| ----- | ------------------------------------------------------------------------------------------------------------------------------ | ------------- |
| peers | Array.&lt;[DiscoveryResultPeer](#DiscoveryResultPeer)&gt; | 该组中的 peer |

#### DiscoveryResultEndorsementLayout

列出组名，以及每个组所需的签名数量。

- 类型
  - Object.&lt;string, number&gt;

#### DiscoveryResultEndorsementPlan

- 类型

  - Object

- 属性

| 名称      | 类型                                                                                                                                                            | 描述                                                                                                                                                                                                                                          |
| --------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| chaincode | string                                                                                                                                                          | 链码名称，是 interest 的用于计算此计划的第一个链码。                                                                                                                                                                                          |
| plan_id   | string                                                                                                                                                          | JSON 对象的字符串，它表示用于为此结果构建查询的提示。提示是[DiscoveryChaincodeInterest](#DiscoveryChaincodeInterest)，其中包含发现服务用来计算返回计划的链码名称和集合。 |
| groups    | Object.&lt;string, [DiscoveryResultEndorsementGroup](#DiscoveryResultEndorsementGroup)&gt; | 指定背书者，分为组。                                                                                                                                                                                                                          |
| layouts   | Array.&lt;[DiscoveryResultEndorsementLayout](#DiscoveryResultEndorsementLayout)&gt;        | 指定实现背书策略的选项                                                                                                                                                                                                                        |

#### DiscoveryResultEndpoint

- 类型

  - Object

- 属性

| 名称 | 类型   | 描述               |
| ---- | ------ | ------------------ |
| host | string |                    |
| port | number |                    |
| name | string | 可选。该端点的名称 |

#### DiscoveryResultEndpoints

- 类型

  - Object

- 属性

| 名称      | 类型                                                                                                                                   | 描述 |
| --------- | -------------------------------------------------------------------------------------------------------------------------------------- | ---- |
| endpoints | Array.&lt;[DiscoveryResultEndpoint](#DiscoveryResultEndpoint)&gt; |      |

#### DiscoveryResultMSPConfig

- 类型

  - Object

- 属性

| 名称                   | 类型                 | 描述                                                                                                                                                                                                                      |
| ---------------------- | -------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| rootCerts              | string               | 此 MSP 信任的根证书列表。它们在证书验证时使用。                                                                                                                                                                           |
| intermediateCerts      | string               | 此 MSP 信任的中间证书列表。它们在证书验证时使用，如下所示:验证尝试从要验证的证书(位于路径的一端)和 RootCerts 字段中的证书之一(位于路径的另一端)构建路径。如果路径长于 2，则在 IntermediateCerts 池中搜索中间的证书。 |
| admins                 | string               | 表示此 MSP 管理员的身份                                                                                                                                                                                                   |
| id                     | string               | MSP 的标识符                                                                                                                                                                                                              |
| orgs                   | Array.&lt;string&gt; | 属于此 MSP 配置的 fabric 组织单位标识符                                                                                                                                                                                   |
| tls_root_certs         | string               | 此 MSP 信任的 TLS 根证书                                                                                                                                                                                                  |
| tls_intermediate_certs | string               | 此 MSP 信任的 TLS 中间证书                                                                                                                                                                                                |

#### DiscoveryResultPeer

- 类型

  - Object

- 属性

| 名称          | 类型                                                                                                                                     | 描述              |
| ------------- | ---------------------------------------------------------------------------------------------------------------------------------------- | ----------------- |
| mspid         | string                                                                                                                                   |                   |
| endpoint      | string                                                                                                                                   | peer 的 host:port |
| ledger_height | Long                                                                                                                                     |                   |
| name          | string                                                                                                                                   |                   |
| chaincodes    | Array.&lt;[DiscoveryResultChaincode](#DiscoveryResultChaincode)&gt; |                   |

#### DiscoveryResultPeers

- 类型

  - Object

- 属性

| 名称  | 类型                                                                                                                           | 描述 |
| ----- | ------------------------------------------------------------------------------------------------------------------------------ | ---- |
| peers | Array.&lt;[DiscoveryResultPeer](#DiscoveryResultPeer)&gt; |      |

#### DiscoveryResults

- 类型

  - Object

- 属性

| 名称              | 类型                                                                                                                                                 | 描述                    |
| ----------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------- | ----------------------- |
| msps              | Object.&lt;string, [DiscoveryResultMSPConfig](#DiscoveryResultMSPConfig)&gt;    | 可选。找到了 msp 配置。 |
| orderers          | Object.&lt;string, [DiscoveryResultEndpoints](#DiscoveryResultEndpoints)&gt;    | 可选。找到orderer。      |
| peers_by_org      | Object.&lt;string, [DiscoveryResultPeers](#DiscoveryResultPeers)&gt;            | 可选。组织的peer发现。  |
| endorsement_plans | Array.&lt;[DiscoveryResultEndorsementPlan](#DiscoveryResultEndorsementPlan)&gt; | 可选。                  |
| timestamp         | number                                                                                                                                               | 发现结果更新的时间戳。  |

#### Endorsement

背书是背书人对提案回复的签名。通过产生背书消息，背书者会隐式“批准”提案响应及其中包含的操作。收集到足够的背书后，就可以从一组提案响应中生成交易。

背书消息具有以下结构:

```javascript
endorser
	Mspid -- {string]
	IdBytes -- {byte[]}
signature -- {byte[]}
```

- 类型
  - Object

#### Enrollment

- 类型

  - Object

- 属性

| 名称            | 类型   | 描述                                           |
| --------------- | ------ | ---------------------------------------------- |
| key             | Object | 私钥                                           |
| certificate     | string | 以 64 位编码的 PEM 格式注册的证书              |
| rootCertificate | string | CA 签名证书的基于 64 位编码的 PEM 编码的证书链 |

#### EnrollmentRequest

- 类型

  - Object

- 属性

| 名称             | 类型                                                                                                                     | 描述                                                                                                           |
| ---------------- | ------------------------------------------------------------------------------------------------------------------------ | -------------------------------------------------------------------------------------------------------------- |
| enrollmentID     | string                                                                                                                   | 用于注册的注册 ID                                                                                              |
| enrollmentSecret | string                                                                                                                   | 与注册 ID 关联的 secret                                                                                        |
| profile          | string                                                                                                                   | 配置文件名称。为 TLS 证书指定“ tls”配置文件；否则，将颁发入学证书。                                            |
| csr              | string                                                                                                                   | 可选。 PEM 编码的 PKCS#10 证书签名请求。从客户端发送到 Fabric-ca 以获得数字身份证书的消息。                   |
| attr_reqs        | Array.&lt;[AttributeRequest](#AttributeRequest)&gt; | [AttributeRequest](#AttributeRequest)数组 |

#### EnrollmentResponse

- 类型

  - Object

- 属性

| 名称           | 类型   | 描述                                          |
| -------------- | ------ | --------------------------------------------- |
| enrollmentCert | string | PEM 编码的 X509 注册证书                      |
| caCertChain    | string | 用于颁发证书颁发机构的 PEM 编码的 X509 证书链 |

#### EventCount

- 类型

  - Object

- 属性

| 名称     | 类型   | 描述                                     |
| -------- | ------ | ---------------------------------------- |
| success  | Number | 收到的成功事件数。                       |
| fail     | Number | 收到的错误数。                           |
| expected | Number | 预期响应事件(或错误)的事件中心的数量。 |

#### EventHubRegistrationRequest

- 类型

  - Object

- 属性

| 名称        | 类型                                                                                           | 描述                            |
| ----------- | ---------------------------------------------------------------------------------------------- | ------------------------------- |
| identity    | [Identity](#Identity)     | 进行此注册的身份                |
| txId        | [TransactionID](./TransactionID.md | 此注册的交易 ID                 |
| certificate | string                                                                                         | 证书文件，PEM 格式              |
| mspId       | string                                                                                         | 用来处理身份的成员服务提供商 ID |

#### Header

Headers 描述有关交易记录的基本信息，例如其类型(配置更新或背书交易等)，其所属通道的 ID，交易 ID 等。标头消息还包含一个公共字段 SignatureHeader，该字段描述有关如何验证签名的关键信息。

“Headers ”将具有以下对象结构。

```javascript
channel_header
	type -- {string}
	version -- {int}
	timestamp -- {time}
	channel_id -- {string}
	tx_id -- {string}
	epoch -- {int}
signature_header -- {SignatureHeader}
```

- 类型
  - Object

#### HTTPEndpoint

- 类型

  - Object

- 属性

| 名称     | 类型   | 描述 |
| -------- | ------ | ---- |
| hostname | string |      |
| port     | number |      |
| protocol | string |      |

#### Identity

- 类型

  - Object

- 属性

| 名称             | 类型                                                                               | 描述                                 |
| ---------------- | ---------------------------------------------------------------------------------- | ------------------------------------ |
| role             | [Role](#Role) | 特定角色的任何身份                   |
| OrganizationUnit |                                                                                    | 每个信任证书链所属组织单位的任何身份 |
| Identity         |                                                                                    | 特定身份                             |

#### IdentityRequest

- 类型

  - Object

- 属性

| 名称             | 类型                                                                                                                       | 描述                                                                                                                                                                                                                                                 |
| ---------------- | -------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| enrollmentID     | string                                                                                                                     | 必要。唯一标识身份的注册 ID                                                                                                                                                                                                                          |
| affiliation      | string                                                                                                                     | 必要。新身份的关联路径                                                                                                                                                                                                                               |
| attrs            | Array.&lt;[KeyValueAttribute](#KeyValueAttribute)&gt; | 要分配给用户的[KeyValueAttribute](#KeyValueAttribute)属性数组                                                                                                                   |
| type             | string                                                                                                                     | 可选。身份类型(例如 _user_, _app_, _peer_, _orderer_)                                                                                                                                                                                                |
| enrollmentSecret | string                                                                                                                     | 可选。注册 secret。如果未提供，则会生成随机 secret。                                                                                                                                                                                                 |
| maxEnrollments   | number                                                                                                                     | 可选。秘密可用于注册的最大次数。如果为 0，则使用 fabric-ca-server 的已配置 max_enrollments；否则为 0。如果大于 0 并且小于等于配置了 fabric-ca-server 的最大注册人数，请使用 max_enrollments;如果大于配置了 fabric-ca-server 的最大注册数量，则错误。 |
| caname           | string                                                                                                                     | 可选。将请求发送到 fabricCA 服务器内的 CA 的名称                                                                                                                                                                                                     |

#### ImplicitMetaPolicy

ImplicitMetaPolicy 是一种策略类型，取决于配置的层次结构性质。它是隐式的，因为规则是基于子策略的数量隐式生成的。它是元数据，因为它仅取决于其他策略的结果

评估后，此策略将遍历所有直接子子组，检索名称为 sub_policy 的策略，评估集合并应用规则。

ImplicitMetaPolicy 具有 4 个子组，策略名称为"Readers"，它检索每个子组，为每个子组检索策略"Readers"，对其进行评估，并且满足 ANY 1 就足够了，那么 ALL 将需要 4 个签名，而 MAJORITY 将需要 3 个签名。

"ImplicitMetaPolicy"将具有以下对象结构。

```javascript
type -- IMPLICIT_META
policy
	sub_policy -- {string}
	rule -- ANY | ALL | MAJORITY
```

- 类型
  - Object

#### InitializeRequest

- 类型

  - Object

- 属性

| 名称               | 类型                                                                                                                                                                                              | 描述                                                                                                                                                              |
| ------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| target             | string&#124;[Peer](./Peer.md&#124;[ChannelPeer](./ChannelPeer.md            | 可选。用于对配置信息进行初始化请求的目标 peer。与"targets"参数一起使用时，此处引用的 peer 将被添加到"targets"数组中。默认是使用分配给该通道的第一个 ChannelPeer。 |
| targets            | Array.&lt;([Peer](./Peer.md &#124; [Channe&#124;Peer](./ChannelPeer.md)&gt; | 可选。用于对配置信息进行初始化请求的目标 peer。当与"target"参数一起使用时，被引用的 peer 将被添加到"targets"数组中。默认是使用分配给该通道的第一个 ChannelPeer。  |
| discover           | boolean                                                                                                                                                                                           | 可选。在目标 peer 上使用发现服务来加载配置和网络信息。默认为 false。如果为 false，则目标 peer 将使用 peer 查询来仅加载配置信息。                                  |
| endorsementHandler | string                                                                                                                                                                                            | 可选。实现 EndorsementHandler 的自定义背书处理程序的路径。                                                                                                        |
| commitHandler      | string                                                                                                                                                                                            | 可选。实现 CommitHandler 的自定义提交处理程序的路径。                                                                                                             |
| asLocalhost        | boolean                                                                                                                                                                                           | 可选。将发现的主机地址转换为“ localhost”。在本地系统上运行由 docker 组成的 fabric 网络时需要，否则应禁用。默认为 true。                                           |
| configUpdate       | Array.&lt;byte&gt;                                                                                                                                                                                | 可选。使用序列化的 ConfigUpdate protobuf 对象初始化此通道。                                                                                                       |

#### JoinChannelRequest

- 类型

  - Object

- 属性

| 名称    | 类型                                                                                                                                                                                      | 描述                                                                                                                                                                                                                           |
| ------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| targets | Array.&lt;[Peer](./Peer.md&gt;./Peer.md &#124; Array.&lt;string&gt; | 可选。要求加入此通道的 peer 对象或 peer 名称数组。使用 peer 名称或将其留空(使用默认目标)时，必须已加载网络配置。参见[loadFromConfig()](./Client.html#loadFromConfig) |
| block   | Array.&lt;byte&gt;                                                                                                                                                                        | 通道的创世块的编码字节。请参见[getGenesisBlock()](./Channel.html#getGenesisBlock)方法                                                                                  |
| txId    | [TransactionID](./TransactionID.md                                                                                             | 必要。具有交易 ID 和随机数的 TransactionID 对象                                                                                                                                                                                |

#### KeyValueAttribute

- 类型

  - Object

- 属性

| 名称  | 类型    | 描述                                                     |
| ----- | ------- | -------------------------------------------------------- |
| name  | string  | key 被用来关联的属性                                     |
| value | string  | 属性的值                                                 |
| ecert | boolean | 可选，值为 true 表示默认情况下应将此属性包括在注册证书中 |

#### MetadataSignature

在块的元数据上签名，以确保描述块的元数据的真实性。

```javascript
signature_header {SignatureHeader}
signature -- {byte[]}
```

- 类型
  - Object

#### OrdererRequest

- 类型

  - Object

- 属性

| 名称    | 类型                                                                                          | 描述                                                         |
| ------- | --------------------------------------------------------------------------------------------- | ------------------------------------------------------------ |
| txId    | [TransactionID](./TransactionID.md | 可选。具有交易 ID 和随机数的对象                             |
| orderer | [Orderer](./Orderer.md             | 可选。要从中检索生成块的 orderer 实例或 orderer 的字符串名称 |

#### OrganizationConfigGroup

通道的每个参与组织将在配置块的一个部分中表示，如下所述。这些部分中包含有关组织的重要信息，例如其成员资格服务提供商(MSP)内容及其预定义的策略，这些信息构成了通道的访问控制策略(Admins, Writers and Readers) 的基础。

组织配置将具有以下对象结构。

```javascript
version -- {int}
mod_policy -- {string}
values
	MSP
		version -- {int}
		mod_policy -- {string}
		value
			type -- {int}
			config
				name -- {string}
				root_certs -- {string[]}
				intermediate_certs -- {string[]}
				admins -- {string[]}
				revocation_list -- {string[]}
				signing_identity -- {byte[]}
				organizational_unit_identifiers -- {string[]}
policies
	 Admins
			version -- {int}
			mod_policy -- {string}
			policy -- {SignaturePolicy}
	 Writers
			version -- {int}
			mod_policy -- {string}
			policy -- {SignaturePolicy}
	 Readers
			version -- {int}
			mod_policy -- {string}
			policy -- {SignaturePolicy}
```

- 类型
  - Object

#### OrganizationIdentifier

- 类型

  - Object

- 属性

| 名称 | 类型   | 描述          |
| ---- | ------ | ------------- |
| id   | string | 组织的 MSP ID |

#### PeerQueryRequest

- 类型

  - Object

- 属性

| 名称     | 类型                                                                                      | 描述                                                                                                                       |
| -------- | ----------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------- |
| target   | [Peer](./Peer.md &#124; string | 用于服务发现请求的[Peer](./Peer.md对象或 peer 名称              |
| useAdmin | boolean                                                                                   | 可选。指示在向 Peer 发出此呼叫时应使用管理员凭据。必须通过连接配置文件或使用“ setAdminSigningIdentity”方法来加载管理身份。 |

#### PeerQueryResponse

-
- 类型

  - Object

- 属性

| 名称         | 类型   | 描述 |
| ------------ | ------ | ---- |
| peers_by_org | Object |      |

- 示例

```javascript
{
    "peers_by_org": {
        "Org1MSP": {
            "peers":[
                {"mspid":"Org1MSP", "endpoint":"peer0.org1.example.com:7051"}
            ]
        },
        "Org2MSP": {
        "peers":[
                {"mspid":"Org2MSP","endpoint":"peer0.org2.example.com:8051"}
            ]
        }
    }
}
```

#### Policy

制定背书政策

- 类型

  - Object

- 属性

| 名称       | 类型                                                                                                         | 描述                                                                              |
| ---------- | ------------------------------------------------------------------------------------------------------------ | --------------------------------------------------------------------------------- |
| identities | Array.&lt;[Identity](#Identity)&gt;     | "policy"部分中要引用的身份列表                                                    |
| policy     | Array.&lt;[PolicySpec](#PolicySpec)&gt; | 使用“签署人”和“ n-of”("signed-by" and "n-of")结构的组合来指定策略。该设计允许递归 |

#### PolicySpec

- 类型

  - Object

- 属性

| 名称 | 类型   | 描述                                                                                                                                                                                                                                                                                                                                              |
| ---- | ------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| type | Object | 策略的类型可以是单个身份签名的"签署人"("signed-by")，也可以是"n-of"，其中"n"是数字值。如果 type 属性是''签署者''，则该值是策略中指定的标识数组的数字索引。如果 type 属性为“ n-of”，则该值为[PolicySpec](#PolicySpec)对象的数组。如您所见，此结构允许对复杂策略进行递归定义。 |

#### ProcessedTransaction

- 类型

  - Object

- 属性

| 名称                | 类型   | 描述                                                                                                                         |
| ------------------- | ------ | ---------------------------------------------------------------------------------------------------------------------------- |
| validationCode      | number | 有关所有定义的验证码，请参见[这个列表](https://github.com/hyperledger/fabric/blob/v1.0.0/protos/peer/transaction.proto#L125) |
| transactionEnvelope | Object | 封装事务和签名。它具有以下结构:<br>signature -- {byte[]}<br>payload -- {}<br>header -- {Header}<br>data -- {Transaction}    |

#### ProposalRequest

- 类型

  - Object

- 属性

| 名称         | 类型                    | 描述                                                                                                                                                                                                                     |
| ------------ | ----------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| fcn          | string                  | 必要。函数名称。                                                                                                                                                                                                         |
| args         | Array.&lt;string&gt;    | 必要。要发送到链码的参数。                                                                                                                                                                                               |
| chaincodeId  | string                  | 必要。 ChaincodeId。                                                                                                                                                                                                     |
| argbytes     | Buffer                  | 可选。当参数必须作为字节包含时，包含。                                                                                                                                                                                   |
| transientMap | object &lt;optional&gt; | 可选。带有 String 属性名称和 Buffer 属性值的对象，可以由链码使用，但不能保存在分类帐中。可以使用此技术将诸如用于加密的密码信息的数据传递到链码。应将要保留在私有数据存储区(集合)中的数据传递给 transientMap 中的链码。 |

#### ProposalResponse

Protobuf 消息，由提案请求的 peer 背书而返回。peer 节点运行提案指定的目标链码，并决定是否背书提案，并将背书结果以及提案响应消息中的 [读写集(read and write sets)](http://hyperledger-fabric.readthedocs.io/en/latest/arch-deep-dive.html?highlight=readset#the-endorsing-peer-simulates-a-transaction-and-produces-an-endorsement-signature) 发送回去。

- 类型

  - Object

- 属性

| 名称        | 类型                                                                                                                 | 描述                                                                                                                                                                                                                   |
| ----------- | -------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| version     | number                                                                                                               |                                                                                                                                                                                                                        |
| timestamp   | Timestamp                                                                                                            | 提交者创建提案的时间                                                                                                                                                                                                   |
| response    | [Response](#Response)                           |                                                                                                                                                                                                                        |
| payload     | Array.&lt;byte&gt;                                                                                                   | 响应的有效负载。它是“ ProposalResponsePayload” protobuf 消息的编码字节                                                                                                                                                 |
| endorsement | [Endorsement](#Endorsement)                     | 提案的背书，基本上是背书人在有效载荷上的签名                                                                                                                                                                           |
| peer        | [RemoteCharacteristics](#RemoteCharacteristics) | 创建此 ProposalResponse 的 peer 的特征。项目包括 url，名称和连接选项。此信息不是从 peer 返回的，也不是 peer 返回的序列化 protobuf 数据的一部分。客户端实例 peer 对象添加了此信息，以帮助标识此 ProposalResponse 对象。 |

#### ProposalResponseObject

所有对背书的 peer 进行提案背书的调用都返回此标准对象数组。

- 类型

  - Object

- 属性

| 名称    | 类型                                                                                                                                   | 描述                                                                                                                                      |
| ------- | -------------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------- |
| index:0 | Array.&lt;([ProposalResponse](#ProposalResponse)&#124; Error)&gt; | 数组，其中每个元素要么是 ProposalResponse 对象(对于来自背书 peer 的成功响应)，要么是 Error 对象(对于不成功的 peer 响应或运行时错误)。 |
| index:1 | Object                                                                                                                                 | 将交易请求发送给 orderer 时所需的原始投标对象                                                                                             |

#### RegisterRequest

- 类型

  - Object

- 属性

| 名称             | 类型                                                                                                                       | 描述                                                                                                                               |
| ---------------- | -------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------- |
| enrollmentID     | string                                                                                                                     | 将用于注册的 ID                                                                                                                    |
| enrollmentSecret | string                                                                                                                     | 为注册用户设置的可选注册密码。如果未提供，则服务器将生成一个。                                                                     |
| role             | string                                                                                                                     | 表示用户角色值的可选任意字符串                                                                                                     |
| affiliation      | string                                                                                                                     | 该用户将与之关联的关联公司，例如公司或组织                                                                                         |
| maxEnrollments   | number                                                                                                                     | 允许该用户注册的最大次数                                                                                                           |
| attrs            | Array.&lt;[KeyValueAttribute](#KeyValueAttribute)&gt; | 要分配给用户的[KeyValueAttribute](#KeyValueAttribute)属性数组 |

#### RegistrationOpts

- 类型

  - Object

- 属性

| 名称       | 类型                   | 描述                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   |
| ---------- | ---------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| startBlock | integer                | 可选-事件检查的起始块号。如果包括该 peer，则将要求 peer 的 fabric 服务从该块号开始发送块。这是恢复或重播已添加到分类账中的错过的区块的方法。默认值是分类账上的最新块。设置 startBlock 可能会使其他事件侦听器感到困惑，因此，当使用 startBlock 时，ChannelEventHub 上将只允许一个侦听器。设置 startBlock 还需要此 ChannelEventHub 使用不同的选项连接到 fabric 服务。必须在调用 connect()之前完成向 startBlock 的注册。                                                                                                                                                                                                  |
| endBlock   | integer&#124; 'newest' | 可选-事件检查的结束块号。值“最新”表示在注册时，peer 会将 endBlock 计算为分类账上的最新块。这允许应用程序重播到分类账上的最新块，然后侦听器将停止并通过“ onError”回调通知。如果包括该 peer，则 peer 的 fabric 服务将在该块交付后被要求停止发送块。这是重播已添加到分类帐中的错过的区块的方法。如果不包含 startBlock，则 endBlock 必须等于或大于当前通道块的高度。设置 endBlock 可能会混淆其他事件侦听器，因此，在使用 endBlock 时，ChannelEventHub 上将只允许一个侦听器。设置 endBlock 还需要此 ChannelEventHub 使用不同的选项连接到结构服务。必须在调用 connect()之前完成对 endBlock 的注册。                          |
| unregister | boolean                | 可选-此选项设置指示看到事件时应删除注册(取消注册(unregister))。当应用程序使用超时仅等待指定的时间量才能看到事务时，超时处理应包括对事务事件侦听器的手动“注销”，以避免意外调用事件回调。对于不同类型的事件侦听器，此设置的默认设置是不同的。对于块侦听器，默认值为 true，但是仅当将 end_block 设置为选项并且该侦听器看到了 end_block 时，才假定事件侦听器已看到最终事件。对于事务侦听器，默认值为 true，并且当此侦听器看到具有 ID 的事务时，该侦听器将被注销。对于链码侦听器，默认值将为 false，因为匹配过滤器可能用于许多事务，而不是其他侦听器中的特定事务或块。如果未设置并且已设置 endBlock，则侦听器将自动注销。 |
| disconnect | boolean                | 可选-此选项设置指示 ChannelEventHub 实例在看到事件后自动与 peer 的 Fabric 服务断开连接。默认值为 false。如果未设置并且已设置 endBlock，则 ChannelEventHub 实例将自动断开自身连接。                                                                                                                                                                                                                                                                                                                                                                                                                                     |

#### RemoteCharacteristics

与 peer 实例对象有关的信息。

- 类型
  - Object
- 属性

| 名称    | 类型   | 描述                                                                                   |
| ------- | ------ | -------------------------------------------------------------------------------------- |
| url     | string | peer 的 url                                                                            |
| name    | string | 此 peer 的名称，如果未使用 name 选项创建 peer，则它将是主机和端口。                    |
| options | Object | 该 peer 基于默认值以及创建时传递的内容构建的选项对象。典型的选项将包括 GRPC 连接设置。 |

#### REQUEST_TIMEOUT

错误消息字符串，指示由于远程节点问题，请求操作已超时。如果本地系统存在问题，将返回"SYSTEM_TIMEOUT"错误消息。对于两种类型的超时，该操作将仅使用一个计时器。随着操作开始，计时器将开始运行。如果计时器在本地实例能够发出出站请求之前到期，则将返回“ SYSTEM_TIMEOUT”错误。如果本地实例能够发出出站请求，并且计时器在远程节点响应之前到期，则返回“ REQUEST_TIMEOUT”。计时器由“请求超时”( 'request-timeout' )设置控制或在发出出站请求的呼叫中传递

- 类型
  - Error
- 示例

```javascript
'client.setConfigSetting('request-timeout', 3000)'
```

```javascript
"channel.sendTranaction(request, 3000)";
```

#### Response

表示批准提案是否成功的响应消息

- 类型
  - Object
- 属性

| 名称    | 类型               | 描述                                                                                                  |
| ------- | ------------------ | ----------------------------------------------------------------------------------------------------- |
| status  | number             | 状态码。遵循:[HTTP status code definitions](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.md |
| message | string             | 与响应状态代码关联的消息                                                                              |
| payload | Array.&lt;byte&gt; | 可用于在此响应中包含元数据的有效负载                                                                  |

#### Restriction

- 类型
  - Object
- 属性

| 名称          | 类型 | 描述                                                              |
| ------------- | ---- | ----------------------------------------------------------------- |
| revokedBefore | Date | 在 CRL 中包括在此 UTC 时间戳之前被吊销的证书(采用 RFC3339 格式) |
| revokedAfter  | Date | 在 CRL 中包括在此 UTC 时间戳之后(RFC3339 格式)被吊销的证书      |
| expireBefore  | Date | 在 CRL 中包含在此 UTC 时间戳之前(RFC3339 格式)过期的已撤销证书  |
| expireAfter   | Date | 在 CRL 中包括在此 UTC 时间戳之后(RFC3339 格式)到期的已撤销证书  |

#### Role

- 类型
  - Object
- 属性

| 名称  | 类型   | 描述                                |
| ----- | ------ | ----------------------------------- |
| name  | string | 角色名称。值可以是"member"或"admin" |
| mspId | string | 用来处理身份的成员服务提供商 ID     |

#### ServiceResponse

- 类型

  - Object

- 属性

| 名称     | 类型                                                                                                                                 | 描述                       |
| -------- | ------------------------------------------------------------------------------------------------------------------------------------ | -------------------------- |
| Success  | boolean                                                                                                                              | 布尔值，指示请求是否成功   |
| Result   | Object                                                                                                                               | 该请求的结果               |
| Errors   | Array.&lt;[ServiceResponseMessage](#ServiceResponseMessage)&gt; | 错误消息数组(代码和消息) |
| Messages | Array.&lt;[ServiceResponseMessage](#ServiceResponseMessage)&gt; | 信息消息数组(代码和消息) |

#### ServiceResponseMessage

- 类型

  - Object

- 属性

| 名称    | 类型   | 描述                   |
| ------- | ------ | ---------------------- |
| code    | number | 表示消息类型的整数代码 |
| message | string | 更具体的信息           |

#### SignatureHeader

Hyperledger Fabric 中所有签名的一部分的对象。 “创建者”字段包含有关签名者身份的两个重要信息，签名者所属的组织(Mspid)和证书(IdBytes)。 “ nonce”字段是防止重放攻击的唯一值。

```javascript
creator
	Mspid -- {string}
	IdBytes -- {byte[]}
nonce -- {byte[]}
```

- 类型
  - Object

#### SignaturePolicy

SignaturePolicy 是一种递归消息结构，它定义了一个轻量级 DSL，用于描述比“恰好此签名”更复杂的策略。 NOutOf 运算符足以表达 AND 和 OR，当然还有以下 M 个策略中的 N 个。

SignedBy 表示签名来自有效证书，该证书由字节中指定的受信机构签名。这将是自签名证书的证书本身，并将是更传统证书的 CA

"SignaturePolicy"将具有以下对象结构。

```javascript
type -- SIGNATURE
rule
	Type -- n_out_of
	n_out_of
		N -- {int}
		rules -- {array}
			Type -- signed_by
			signed_by -- {int}
	identities -- {array}
		principal_classification -- {int}
		msp_identifier -- {string}
		Role -- MEMBER | ADMIN
```

- 类型
  - Object

#### SignedCommitProposal

- 类型

  - Object

- 属性

| 名称              | 类型                                                                                                           | 描述                                                                               |
| ----------------- | -------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------- |
| request           | [TransactionRequest](#TransactionRequest) | 必要。提交请求                                                                     |
| signedTransaction | Buffer                                                                                                         | 必要。签署的交易                                                                   |
| orderer           | [Orderer](./Orderer.md &#124; string                | 可选。要操作的 orderer 实例或 orderer 的字符串名称。请参阅 Client.getTargetOrderer |

#### SignedEvent

- 类型

  - Object

- 属性

| 名称      | 类型   | 描述                             |
| --------- | ------ | -------------------------------- |
| signature | Buffer | 该有效负载上的签名               |
| payload   | Buffer | 要发送到 peer 的有效载荷字节数组 |

#### SignedProposal

- 类型

  - Object

- 属性

| 名称           | 类型                                                                                      | 描述             |
| -------------- | ----------------------------------------------------------------------------------------- | ---------------- |
| targets        | Array.&lt;[Peer](./Peer.md&gt; | 必要。函数名称。 |
| signedProposal | Buffer                                                                                    | 必要。签署的提案 |

#### SYSTEM_TIMEOUT

错误消息字符串，指示由于系统问题，请求操作已超时。这将表明问题是本地的，而不是远程的。对于两种类型的超时，该操作将仅使用一个计时器。随着操作开始，计时器将开始运行。如果计时器在本地实例能够发出出站请求之前到期，则将返回“ SYSTEM_TIMEOUT”错误。如果本地实例能够发出出站请求，并且计时器在远程节点响应之前到期，则返回“ REQUEST_TIMEOUT”。计时器由“请求超时”设置控制或在发出出站请求的呼叫中传递

- 类型

  - Error

- 示例

```javascript
'client.setConfigSetting('request-timeout', 3000)'
```

```javascript
"channel.sendTranaction(request, 3000)";
```

#### TLSOptions

- 类型

  - Object

- 属性

| 名称         | 类型                 | 描述                                                 |
| ------------ | -------------------- | ---------------------------------------------------- |
| trustedRoots | Array.&lt;string&gt; | PEM 编码的受信任根证书的数组                         |
| verify       | boolean              | 可选。默认为 true。确定使用 TLS 时是否验证服务器证书 |

#### Transaction

交易(或“背书人交易”)是调用链码以收集背书的结果，在通道的上下文中进行全局排序，并在最终正式“提交”到区块内部的分类帐之前，被提交者 peer 作为区块的一部分进行验证。每笔交易都包含代表一系列执行交易的不同步骤的“动作”，这些步骤将被原子地处理，这意味着如果任何一个步骤失败，则整个交易将被标记为已拒绝。

"actions" 数组的每个条目都包含一个链码提议和相应的提议响应，这些响应封装了背书的 peer 关于该提议是否被视为有效的决定。请注意，即使背书的 peer 认为交易建议有效，提交人在交易验证期间仍可能拒绝交易建议。整个交易是否有效，并不反映在交易记录本身中，而是记录在区块元数据的单独字段中。

"Transaction" 将具有以下对象结构。

```javascript
actions {array}
	header -- {SignatureHeader}
	payload
		chaincode_proposal_payload
			input -- {ChaincodeInvocationSpec} for a endorser transaction
		action
			proposal_response_payload
				proposal_hash -- {byte[]}
				extension
					results
						data_model -- {int}
						ns_rwset -- {array}
							namespace -- {string}
							rwset
								reads -- {array}
									key -- {string}
									version
										block_num -- {number}
										tx_num -- {number}
								range_queries_info -- {array}
								writes -- {array}
									key -- {string}
									is_delete -- {boolean}
									value -- {string}
								metadata_writes -- {array}
									key -- {string}
									entries -- {array}
										name -- {string}
										value -- {byte[]}
						collection_hashed_rwset -- {array}
							collection_name -- {string}
							hashed_rwset
								hashed_reads -- {array}
									key_hash -- {byte[]}
									version
										block_num -- {number}
										tx_num -- {number}
								hashed_writes -- {array}
									key_hash -- {byte[]}
									is_delete -- {boolean}
									value_hash -- {byte[]}
								metadata_writes -- {array}
									key_hash -- {byte[]}
									entries -- {array}
										name -- {string}
										value -- {byte[]}
							pvt_rwset_hash -- {byte[]}
					events
						chaincode_id --  {string}
						tx_id -- {string}
						event_name -- {string}
						payload -- {byte[]}
					response
						status -- {int}
						message -- {string}
						payload -- {byte[]}
			endorsements -- {Endorsement[]}
```

- 类型
  - Object

#### TransactionEventHandlerOptions

- 类型

  - Object

- 属性

| 名称          | 类型   | 描述                                                              |
| ------------- | ------ | ----------------------------------------------------------------- |
| commitTimeout | Number | 可选。默认为 0.等待事务完成的秒数。零值表示处理程序应无限期等待。 |

#### TransactionRequest

- 类型

  - Object

- 属性

| 名称              | 类型                                                                                                                     | 描述                                                                                                                                                                                                                                                |
| ----------------- | ------------------------------------------------------------------------------------------------------------------------ | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| proposalResponses | Array.&lt;[ProposalResponse](#ProposalResponse)&gt; | 包含[endorsement](./Channel.html#sendTransactionProposal)调用响应的数组或单个[ProposalResponse](#ProposalResponse)对象 |
| proposal          | Proposal                                                                                                                 | 包含原始背书请求的投标对象                                                                                                                                                                                                                          |
| txID              | TransactionId                                                                                                            | 可选。 -必须是提案背书中使用的交易 ID 对象。 transactionID 将仅用于确定请求的签名是否应由管理员身份或分配给客户端实例的用户来完成。                                                                                                                 |
| orderer           | [Orderer](./Orderer.md &#124; string                         | 可选。要操作的 orderer 实例或 orderer 的字符串名称。请参阅 Client.getTargetOrderer                                                                                                                                                                  |

#### UserNamePasswordObject

在'setUserContext'调用上使用的替代对象，以代替'User'对象。使用此对象时，假定当前的“客户端”实例已加载了公共连接配置文件。

- 类型

  - Object

- 属性

| 名称     | 类型   | 描述                                                                           |
| -------- | ------ | ------------------------------------------------------------------------------ |
| username | string | 必要。代表用户的用户名的字符串                                                 |
| password | string | 可选。代表用户密码的字符串                                                     |
| caName   | string | 可选。代表认证中心名称的字符串。如果未指定，将使用列表中的第一个证书颁发机构。 |

#### UserOpts

- 类型

  - Object

- 属性

| 名称            | 类型                                                                                                 | 描述                                            |
| --------------- | ---------------------------------------------------------------------------------------------------- | ----------------------------------------------- |
| username        | string                                                                                               | 用于注册的用户名                                |
| mspid           | string                                                                                               | MSP ID                                          |
| cryptoContent   | [CryptoContent](#CryptoContent) | 私钥和证书                                      |
| skipPersistence | boolean                                                                                              | 是否将此新用户对象保存到持久性(persistence)中。 |

---
