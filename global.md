# Global

### Members

#### <constant> CLIENT

CLIENT表示此身份正在充当客户端

#### <constant> HFAFFILIATIONMGR

HFAFFILIATIONMGR是一个布尔属性，它允许身份管理从属关系

#### <constant> HFGENCRL

HFGENCRL是允许身份生成CRL的属性

#### <constant> HFINTERMEDIATECA

HFINTERMEDIATECA是一个布尔属性，它允许身份注册为中间CA

#### <constant> HFREGISTRARATTRIBUTES

HFREGISTRARATTRIBUTES是一个属性，该属性具有允许注册服务商为身份注册的属性列表

#### <constant> HFREGISTRARDELEGATEROLES

HFREGISTRARDELEGATEROLES是允许注册服务商为其"hf.Registrar.Roles''属性将指定的角色赋予registree的属性

#### <constant> HFREGISTRARROLES

HFREGISTRARROLES是允许注册服务商管理指定角色的身份的属性

#### <constant> HFREVOKER

HFREVOKER是一个布尔属性，它允许身份重新调用用户和/或证书

#### <constant> jsSHA3

实现哈希原函数。

#### <constant> ORDERER

ORDERER指示身份正在充当订购者

#### <constant> PEER

PEER表示身份正在充当对等方

#### <constant> USER

USER表示身份正在充当用户

### Methods

#### bitsToBytes(arr)

从bitArray转换为字节（请参阅SJCL的编解码器

- 参数

| 名称 | 类型           | 描述                       |
| ---- | -------------- | -------------------------- |
| arr  | Array.<number> | a bitArray to convert from |

- 返回结果

  从bitArray转换的字节(bytes )

#### bytesToBits(bytes)

从字节转换为bitArray（请参阅SJCL的编解码器）

- 参数

| 名称  | 类型           | 描述         |
| ----- | -------------- | ------------ |
| bytes | Array.<number> | 要转换的字节 |

- 返回结果

  从字节转换为bitArray

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

- 参见：
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

| 名称          | 类型    | 描述                                                         |
| ------------- | ------- | ------------------------------------------------------------ |
| chaincodePath | string  | 必要。链码源代码位置的路径字符串                             |
| chaincodeType | string  | 链码类型['golang','node','car','java']的字符串(默认为'golang') |
| devmode       | boolean | 设置为true以使用Chaincode开发模式                            |
| metadataPath  | string  | 可选。包含元数据描述符的顶级目录的路径                       |

- 返回结果

  将数据作为字节数组的Promise

  - 类型

    Promise

#### toEnvelope(signature, proposal_bytes)

将proposal.proto：SignedProposal转换为common.proto：Envelope

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

| 名称   | 类型    | 描述                                                         |
| ------ | ------- | ------------------------------------------------------------ |
| name   | string  | 必要。建立联系的途径                                         |
| caname | string  | 可选。将请求发送到fabric CA服务器内的CA的名称                |
| force  | boolean | 可选。<br>- 对于创建从属关系请求，如果不存在任何父从属关系并且'force'为true，则还要创建所有父从属关系。<br>- 对于删除隶属关系请求，如果force为true且存在任何子隶属关系或任何与该隶属关系或子隶属关系相关的标识，这些身份和子从属关系将被删除；否则，将返回错误。<br>- 对于更新隶属关系请求，如果任何身份与此隶属关系相关联，则“ force”为true会导致这些身份的隶属关系被重命名；否则，将返回错误。 |

#### AttributeRequest

- 类型
  - Object

- 属性

| 名称     | 类型    | 描述                           |
| -------- | ------- | ------------------------------ |
| name     | string  | 要包含在证书中的属性名称       |
| optional | boolean | 如果身份不具有属性，则抛出错误 |

#### Block

完全解码的protobuf消息“块”的对象。

块可以包含通道的配置或通道上的事务。

Block对象将具有以下对象结构。

```
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

获取块号：

```
var block_num = block.header.number;
```

获取交易数量，包括无效交易：

```
var block_num = block.data.data.legnth;
```

获取代码块中第一个事务的ID：

```
var tx_id = block.data.data[0].payload.header.channel_header.tx_id;
```

#### BlockchainInfo

- 类型
  - Object

- 属性

| 名称              | 类型         | 描述                                                         |
| ----------------- | ------------ | ------------------------------------------------------------ |
| height            | number       | 通道分类帐中有多少个区块                                     |
| currentBlockHash  | Array.<byte> | 块哈希是通过对以下串联的ASN.1编码字节进行哈希计算得出的：块编号，先前的块哈希和当前的块数据哈希。区块哈希的链保证了分类账的不变性 |
| previousBlockHash | Array.<byte> | 前一个块的块哈希。                                           |

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

| 名称         | 类型         | 描述                                                         |
| ------------ | ------------ | ------------------------------------------------------------ |
| chaincode_id | string       | 引发此事件的链码的名称。                                     |
| tx_id        | string       | 此事件的交易ID。                                             |
| event_name   | string       | 背书期间链码所设置的作为此事件的event_name的字符串。 stub.SetEvent(event_name, payload) |
| payload      | Array.<byte> | 链代码调用stub.SetEvent(event_name,payload)时设置的应用程序特定的字节数组 |

#### ChaincodeInfo

- 类型
  - Object

- 属性

| 名称    | 类型   | 描述                                                         |
| ------- | ------ | ------------------------------------------------------------ |
| name    | string |                                                              |
| version | string |                                                              |
| path    | string | 安装/实例化事务指定的路径                                    |
| input   | string | 实例化chaincode函数及其参数。如果查询返回有关已安装链码的信息，则该字段将为空。 |
| escc    | string | 此链码的ESCC名称。如果查询返回有关已安装链码的信息，则该字段将为空。 |
| vscc    | string | 此链码的VSCC的名称。如果查询返回有关已安装链码的信息，则该字段将为空。 |

#### ChaincodeInstallRequest

- 类型
  - Object

- 属性

| 名称             | 类型                                                         | 描述                                                         |
| ---------------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| targets          | Array.<[Peer](https://hyperledger.github.io/fabric-sdk-node/release-1.4/Peer.html)>&#124;Array.<string> | 可选的。将安装链码的[Peer](https://hyperledger.github.io/fabric-sdk-node/release-1.4/Peer.html)对象或peer名称的数组。如果排除在外，则将使用在公共连接配置文件中定义的分配给该客户组织的同级。如果包括“ channelNames”属性，则目标peer将基于通道中定义的peer。 |
| chaincodePath    | string                                                       | 需要。链码源代码位置的路径。如果chaincode类型为golang，则此路径为标准软件包名称，例如'mycompany.com/myproject/mypackage/mychaincode' |
| metadataPath     | string                                                       | 可选的。包含元数据描述符的顶级目录的路径。                   |
| chaincodeId      | string                                                       | 需要。链码名称                                               |
| chaincodeVersion | string                                                       | 需要。链码的版本字符串，例如“ v1”                            |
| chaincodePackage | Array.<byte>                                                 | 选的。链码源的归档内容的字节数组。归档文件必须具有一个“ src”文件夹，其中包含与“ chaincodePath”字段相对应的子文件夹。例如，如果chaincodePath为“mycompany.com/myproject/mypackage/mychaincode”，则归档文件必须包含链目录源代码所在的文件夹“src/mycompany.com/myproject/mypackage/mychaincode”。 |
| chaincodeType    | string                                                       | 可选的。链码的类型。 “ golang”，“ car”，“ node”或“ java”之一。默认值为“ golang”。 |
| channelNames     | Array.<string>&#124;string                                   | 可选的。没有提供目标时，将在加载的公共连接配置文件中搜索合适的目标peer。将选择在此属性命名的通道中以及在此客户的组织中定义且在命名通道上具有背书或链码查询角色的同级。 |
| txId             | [TransactionID](https://hyperledger.github.io/fabric-sdk-node/release-1.4/TransactionID.html) | 可选。此请求的TransactionID对象。                            |

#### ChaincodeInstantiateUpgradeRequest

- 类型
  - Object
- 属性

| 名称               | 类型                                                         | 描述                                                         |
| ------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ |
| targets            | Array.[[Peer](https://hyperledger.github.io/fabric-sdk-node/release-1.4/Peer.html)]([Peer](https://hyperledger.github.io/fabric-sdk-node/release-1.4/Peer.html))\|Array.<string> | 可选的。将安装链码的[Peer](https://hyperledger.github.io/fabric-sdk-node/release-1.4/Peer.html)对象或peer名称的数组。如果排除在外，则将使用在公共连接配置文件中定义的分配给该客户组织的同级。如果包括“ channelNames”属性，则目标peer将基于通道中定义的Peer。 |
| chaincodeType      | string                                                       | 可选的。链码的类型。 “ golang”，“ car”，“ node”或“ java”之一。默认值为“ golang”。 |
| chaincodeId        | string                                                       | 需要。链码名称                                               |
| chaincodeVersion   | string                                                       | 需要。链码的版本字符串，例如“ v1”                            |
| txId               | [TransactionID](https://hyperledger.github.io/fabric-sdk-node/release-1.4/TransactionID.html) | 可选。此请求的TransactionID对象。                            |
| collections-config | string                                                       | 可选的。集合配置的路径。可以在本[教程(tutorial)](https://hyperledger.github.io/fabric-sdk-node/release-1.4/tutorial-private-data.html)中找到更多详细信息 |
| transientMap       | object                                                       | 可选的。带有String属性名称和Buffer属性值的对象，可以由链码使用，但不能保存在分类帐中。可以使用此技术将诸如用于加密的密码信息的数据传递到链码。应将要保留在私有数据存储区（集合）中的数据传递给transientMap中的链码。 |
| fcn                | string                                                       | 可选的。在目标链码中调用stub.GetFunctionAndParameters()时要返回的函数名称。默认为'init';如果要不传入任何函数名，请显式传入fcn，其值为null或”（空字符串） |
| args               | Array.<string>                                               | 可选的。传递给fcn值所标识的函数的字符串参数数组。            |
| endorsement-policy | Object                                                       | 可选的。此链码的EndorsementPolicy对象（请参见下面的示例）。如果未指定，则使用默认策略“由来自与成员服务提供者阵列相对应的任何组织的任何成员的签名”("a signature by any member from any of the organizations corresponding to the array of member service providers")。警告：不建议将默认策略用于生产，因为这将允许应用程序绕过投标认可，并直接向订购者发送手动构造的事务（在写入集中具有任意输出）。分配给创建签名的客户端实例的用户上下文将允许成功验证交易并将其提交到分类账。 |

##### 示例

背书策略：“由任何一个组织的成员签署”

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

背书政策：“由ordererOrg的管理员和一个peer组织的任何成员签署”

```
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

```
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

| 名称               | 类型                                                         | 描述                                                         |
| ------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ |
| targets            | Array.[[Peer](https://hyperledger.github.io/fabric-sdk-node/release-1.4/Peer.html)]([Peer](https://hyperledger.github.io/fabric-sdk-node/release-1.4/Peer.html))\|Array.<string> | 可选的。背书peer对象或peer名称作为请求目标的数组。如果省略此参数，则目标列表将包括分配给该通道实例的peer，它们具有背书角色。 |
| chaincodeId        | string                                                       | 需要。用于处理交易建议的链码的ID                             |
| endorsement_hint   | DiscoveryChaincodeIntereset                                  | 可选的。 [DiscoveryChaincodeInterest](https://hyperledger.github.io/fabric-sdk-node/release-1.4/global.html#DiscoveryChaincodeInterest)对象的一个，发现服务将使用该对象来计算适当的背书计划。仅当背书将由可调用其他链码的链码执行背书，或者背书应仅由一个或多个集合中的对等方进行背书时，才需要该参数。 |
| txId               | [TransactionID](https://hyperledger.github.io/fabric-sdk-node/release-1.4/TransactionID.html) | 可选的。具有交易ID和随机数的TransactionID对象。 [sendTransactionProposal](https://hyperledger.github.io/fabric-sdk-node/release-1.4/Channel.html#sendTransactionProposal)需要txId，[generateUnsignedProposal](https://hyperledger.github.io/fabric-sdk-node/release-1.4/Channel.html#generateUnsignedProposal)是可选的 |
| transientMap       | object                                                       | 可选的。带有String属性名称和Buffer属性值的对象，可以由链码使用，但不能保存在分类帐中。可以使用此技术将诸如用于加密的密码信息的数据传递到链码。应将要保留在私有数据存储区（集合）中的数据传递给transientMap中的链码。 |
| fcn                | string                                                       | 可选的。在目标链码中调用stub.GetFunctionAndParameters()时要返回的函数名称。默认为'invoke'。 |
| args               | Array.<string>                                               | 可选的。传递给fcn值所标识的函数的字符串参数数组。            |
| required           | Array.<string>                                               | 可选的。字符串数组，表示背书所需的peer的名称。这些将是发送提案的唯一peer。该列表仅适用于使用发现服务的背书。此属性由DiscoveryEndorsementHandler使用。 |
| ignore             | Array.<string>                                               | 可选的。字符串数组，表示背书应忽略的peer的名称。该列表仅适用于使用发现服务的背书。此属性由[DiscoveryEndorsementHandler](https://hyperledger.github.io/fabric-sdk-node/release-1.4/DiscoveryEndorsementHandler.html)使用。 |
| preferred          | Array.<string>                                               | 可选的。字符串数组，代表应该由背书赋予优先级的peer的名称。优先级意味着当背书计划在组中有更多peer，然后需要满足背书策略时，将首先选择这些peer进行背书。该列表仅适用于使用发现服务的认可。此属性由[DiscoveryEndorsementHandler](https://hyperledger.github.io/fabric-sdk-node/release-1.4/DiscoveryEndorsementHandler.html)使用。 |
| requiredOrgs       | Array.<string>                                               | 可选的。字符串数组，表示背书所需的组织的MSP ID。仅向这些组织中的peer发送提议。该列表仅适用于使用发现服务的背书。此属性由[DiscoveryEndorsementHandler](https://hyperledger.github.io/fabric-sdk-node/release-1.4/DiscoveryEndorsementHandler.html)使用。 |
| ignoreOrgs         | Array.<string>                                               | 可选的。字符串数组，表示背书的组织应忽略的MSP ID的名称。在将组织中的peer添加到优先级列表之前，可以使用可选属性preferredHeightGap考虑其分类帐高度。该列表仅适用于使用发现服务的背书。此属性由[DiscoveryEndorsementHandler](https://hyperledger.github.io/fabric-sdk-node/release-1.4/DiscoveryEndorsementHandler.html)使用。 |
| preferredOrgs      | Array.<string>                                               | 可选的。一组字符串，代表应由背书赋予优先级的组织的MSP ID的名称。在将组织中的peer添加到优先级列表之前，可以使用可选属性preferredHeightGap考虑其分类帐高度。该列表仅适用于使用发现服务的背书。此属性由[DiscoveryEndorsementHandler](https://hyperledger.github.io/fabric-sdk-node/release-1.4/DiscoveryEndorsementHandler.html)使用。 |
| preferredHeightGap | Number                                                       | 可选的。一个整数，表示一个peer的区块高度的最大差值和在背书计划内的一组peer中找到的最高区块高度，该整数允许成为首选peer。如果peer的块高度比最高块高度小一个值大于此值，则不会给它一个优先级。没有默认值，如果未提供此值，则在添加到首选列表时将不考虑对等方的块高。该列表仅适用于使用发现服务的背书。此属性由[DiscoveryEndorsementHandler](https://hyperledger.github.io/fabric-sdk-node/release-1.4/DiscoveryEndorsementHandler.html)使用。 |
| sort               | string                                                       | 可选的。一个字符串值，指示应如何选择组内的peer。 “ ledgerHeight”，将peer按通道分类账上的块数降序排列。“随机”，从列表中随机拉peer，preferred将首先拉。默认设置为按分类帐高度排序。 |

#### ChaincodeQueryRequest

- 类型
  - Object

- 属性

| 名称            | 类型                                                         | 描述                                                         |
| --------------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| targets         | Array.[[Peer](https://hyperledger.github.io/fabric-sdk-node/release-1.4/Peer.html)]([Peer](https://hyperledger.github.io/fabric-sdk-node/release-1.4/Peer.html))\|Array.<string> | 可选的。当未提供时，将使用将接收此请求的peer，添加到此通道对象的peer列表 |
| chaincodeId     | string                                                       | 需要。用于处理交易建议的链码的ID                             |
| transientMap    | object                                                       | 可选的。带有String属性名称和Buffer属性值的对象，可以由链码使用，但不能保存在分类帐中。可以使用此技术将诸如用于加密的密码信息的数据传递到链码。应将要保留在私有数据存储区（集合）中的数据传递给transientMap中的链码。 |
| fcn             | string                                                       | 可选的。在目标链码中调用stub.GetFunctionAndParameters()时要返回的函数名称。默认为“invoke” |
| args            | Array.<string>                                               | 特定于链码的“ Invoke”方法的字符串参数数组                    |
| request_timeout | integer                                                      | 用于此请求的超时值                                           |
| txId            | [ TransactionID](https://hyperledger.github.io/fabric-sdk-node/release-1.4/TransactionID.html) | 可选的。用于查询的交易ID。                                   |

#### ChaincodeQueryResponse

- 类型
  - Object

- 属性

| 名称       | 类型                                                         | 描述 |
| ---------- | ------------------------------------------------------------ | ---- |
| chaincodes | Array.<[ChaincodeInfo](https://hyperledger.github.io/fabric-sdk-node/release-1.4/global.html#ChaincodeInfo)> |      |

#### ChannelConfigGroup

通道本身的块中包含控制结构应如何维护通道的配置设置。当一个块包含通道配置时，通道配置记录是块数据数组中的唯一项。每个块（包括配置块本身）都有一个指向最新配置块的指针，从而可以轻松查询最新的通道配置设置。

通道配置记录将具有以下对象结构。

```
version -- {int}
mod_policy -- {string}
groups
	Orderer
		version -- {int}
		groups
			<orderer_org_name> -- {OrganizationConfigGroup}
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
			<peer_org_name> -- {OrganizationConfigGroup}
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

| 名称                                      | 类型                                                         | 描述                                                         |
| ----------------------------------------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| groups.Orderer.groups.<orderer_org_name>  | [OrganizationConfigGroup](https://hyperledger.github.io/fabric-sdk-node/release-1.4/global.html#OrganizationConfigGroup) | 这些是网络上定义的Orderer组织名称                            |
| groups.Application.groups.<peer_org_name> | [OrganizationConfigGroup](https://hyperledger.github.io/fabric-sdk-node/release-1.4/global.html#OrganizationConfigGroup) | 这些是网络上定义的peer组织名称                               |
| policy                                    | [ ImplicitMetaPolicy](https://hyperledger.github.io/fabric-sdk-node/release-1.4/global.html#ImplicitMetaPolicy) | 这些策略指向其他策略，并指定"ANY"，"MAJORITY"或"ALL"中的阈值 |

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

| 名称           | 类型    | 描述                                                         |
| -------------- | ------- | ------------------------------------------------------------ |
| endorsingPeer  | boolean | 可选的。可以向该peer发送背书交易提议。peer必须安装链码。该应用程序还可以使用此属性来确定哪些peer发送链码安装请求。默认值：true |
| chaincodeQuery | boolean | 可选的。可以向该peer发送仅作为查询的交易建议。peer必须安装链码。该应用程序还可以使用此属性来确定哪些peer发送链码安装请求。默认值：true |
| ledgerQuery    | boolean | 可选的。可以向此peer发送不需要链码的查询提议，例如queryBlock()，queryTransaction()等。默认值：true |
| eventSource    | boolean | 可选的。此peer可能是事件侦听器注册的目标？所有peer都可以产生事件，但是该应用通常仅需要连接到一个事件。默认值：true |
| discover       | boolean | 可选的。该peer可能是服务发现的目标。默认值：true             |

#### ChannelQueryResponse

- 类型
  - Object

- 属性

| 名称     | 类型                                                         | 描述 |
| -------- | ------------------------------------------------------------ | ---- |
| channels | Array.<[ChannelInfo](https://hyperledger.github.io/fabric-sdk-node/release-1.4/global.html#ChannelInfo)> |      |

#### ChannelRequest

- 类型
  - Object

- 属性

| 名称       | 类型                                                         | 描述                                                         |
| ---------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| name       | string                                                       | 必要。新通道的名称                                           |
| orderer    | [Orderer](https://hyperledger.github.io/fabric-sdk-node/release-1.4/Orderer.html) &#124; string | 必要。代表要发送通道创建请求的orderer节点的orderer对象或orderer名称 |
| envelope   | Array.<byte>                                                 | 可选的。信封对象的字节，其中包含初始化此通道所需的所有设置和签名。该信封将由命令行工具[configtxgen](http://hyperledger-fabric.readthedocs.io/en/latest/configtxgen.html) 或 [configtxlator](https://github.com/hyperledger/fabric/blob/master/examples/configtxupdate/README.md)创建 |
| config     | Array.<byte>                                                 | 可选的。从由configtxgen工具创建的ConfigEnvelope中提取的Protobuf ConfigUpdate对象。请参见[extractChannelConfig()](https://hyperledger.github.io/fabric-sdk-node/release-1.4/Client.html#extractChannelConfig)。 ConfigUpdate对象也可以由configtxlator工具创建。 |
| signatures | Array.<[ConfigSignature](https://hyperledger.github.io/fabric-sdk-node/release-1.4/global.html#ConfigSignature)> &#124; Array.<string> | 必要。使用“config"参数时，通道创建或更新策略所需的签名列表。 |
| txId       | [TransactionID](https://hyperledger.github.io/fabric-sdk-node/release-1.4/TransactionID.html) | 必要。具有交易ID和随机数的TransactionID对象                  |

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
| blockToLive       | Long&#124;number&#124;string&#124; Object | 参数将被转换为unsigned int64 as Long       |
| memberOnlyRead    | boolean                                   | 表示是否只有集合成员客户端可以读取私有数据 |

#### CollectionQueryOptions

- 类型
  - Object

- 属性

| 名称        | 类型                                                         | 描述                                                         |
| ----------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| chaincodeId | string                                                       | 必要。链码名称                                               |
| target      | string&#124;[Peer](https://hyperledger.github.io/fabric-sdk-node/release-1.4/Peer.html) | 可选的。当未提供此通道对象中的第一个peer时，将使用将接收此请求的peer。 |

#### CollectionQueryResponse

- 类型
  - Object

- 属性

| 名称               | 类型                                                         | 描述                                                         |
| ------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ |
| type               | string                                                       | Collection类型                                               |
| name               | string                                                       | 表示的链码内的集合名称                                       |
| required_peer_coun | number                                                       | 背书时将发送最少数量的peer私有数据。如果至少没有传播到这个数目的peer，则背书将失败。 |
| maximum_peer_count | number                                                       | 背书后将发送私人数据的最大peer数。此数字必须大于required_peer_count。 |
| block_to_live      | number                                                       | 收集数据到期之后的块数。例如，如果将该值设置为10，则最后由块号100修改的密钥将在块号111处清除。零值与MaxUint64相同，不会清除数据。 |
| member_read_only   | boolean                                                      | 成员仅读取访问权限表示是仅集合成员客户端可以读取私有数据（如果设置为true），还是非成员可以读取数据（如果设置为false，例如，如果您想在链码中实现更精细的访问逻辑） |
| policy             | [Policy](https://hyperledger.github.io/fabric-sdk-node/release-1.4/global.html#Policy) | "member_orgs_policy"政策                                     |

#### ConfigEnvelope

ConfigEnvelope包含通道配置数据，并且是配置块的主要内容。另一种类型的块是包含背书交易的块，其中主要内容是[Transaction](https://hyperledger.github.io/fabric-sdk-node/release-1.4/global.html#Transaction)数组。

"ConfigEnvelope"将具有以下对象结构。

```
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

| 名称             | 类型         | 描述                                                         |
| ---------------- | ------------ | ------------------------------------------------------------ |
| signature_header | Array.<byte> | [SignatureHeader](https://hyperledger.github.io/fabric-sdk-node/release-1.4/global.html#SignatureHeader)的编码字节 |
| signature        | Array.<byte> | 签名的串联后签名的编码字节                                   |

#### ConfigUpdateEnvelope

原始消息"ConfigUpdateEnvelope"的对象。

"ConfigUpdateEnvelope"将具有以下对象结构。

```
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

| 名称                    | 类型                                                         | 描述                                                         |
| ----------------------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| config_update.read_set  | [ChannelConfigGroup](https://hyperledger.github.io/fabric-sdk-node/release-1.4/global.html#ChannelConfigGroup) | 一组所有正在更新的配置项的当前版本号                         |
| config_update.write_set | [ChannelConfigGroup](https://hyperledger.github.io/fabric-sdk-node/release-1.4/global.html#ChannelConfigGroup) | 一组所有正在更新的配置项。版本号必须比read_set中相同项目的版本号大一，并带有新值。 |

#### ConnectionOpts

- 类型
  - Object

- 属性

| 名称                     | 类型   | 描述                                                         |
| ------------------------ | ------ | ------------------------------------------------------------ |
| name                     | string | 可选的。为这个远程端点命名。如果未提供名称，则端点将通过其URL知道。 |
| request-timeout          | string | 一个整数值（以毫秒为单位），用作等待请求响应的最长时间。     |
| pem                      | string | PEM格式的证书文件，用于gRPC协议（即TransportCredentials）。使用grpcs协议时必需。 |
| ssl-target-name-override | string | 仅在测试环境中使用，当服务器证书的主机名（在“ CN”字段中）与服务器进程在其上运行的实际主机端点不匹配时，通过将此属性设置为服务器证书的主机名的值，应用程序可以解决客户端TLS验证失败的问题 |
| <any>                    | any    | 任何其他标准grpc调用选项将直接传递到grpc服务调用             |

#### ConnectOptions

- 类型
  - Object

- 属性

| 名称        | 类型                                                         | 描述                                                         |
| ----------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| full_block  | boolean                                                      | 可选的。指示与对等方的连接将向此ChannelEventHub发送完整的块或已过滤的块。默认设置是使用过滤后的块建立连接。过滤的块具有提供交易状态和链码事件所需的信息（无有效载荷）。使用未过滤的块（完整块）时，将要求用户有权建立连接以接收完整块。在已过滤的块连接上注册块侦听器可能无法提供足够的信息。 |
| startBlock  | number&#124;string                                           | 可选的。这将具有连接设置，以使用该编号开始将块发送回事件中心。如果与startBlock连接，则事件侦听器可能未注册到startBlock或endBlock。如果事件中心应该以它看到的最后一个块开头，则使用字符串“ last_seen”。如果事件中心应以分类账上最旧的块开头，则使用字符串“ oldest”。如果事件中心应以分类帐上的最新块开头，请使用字符串“ latest”或使用startBlock。默认是从分类账上的最新块开始。 |
| endBlock    | number&#124;string                                           | 可选的。这将具有连接设置，以结束将块发送回具有该编号的块处的事件中心。如果与endBlock连接，则事件侦听器可能未注册到startBlock或endBlock。如果事件中心应该以它看到的最后一个块结尾，则使用字符串'last_seen'。如果事件中心应以分类账上的当前块结尾，则使用字符串“最新”。默认为不停止发送。 |
| signedEvent | [SignedEvent](https://hyperledger.github.io/fabric-sdk-node/release-1.4/global.html#SignedEvent) | 可选的。已签名的事件将发送给peer。当fabric客户端应用程序没有用户的privateKey并且无法签署对结构网络的请求时，此选项很有用。 |
| target      | [Peer](https://hyperledger.github.io/fabric-sdk-node/release-1.4/Peer.html)&#124;string | 可选的。提供结构事件服务的peer。使用字符串时，必须为Channel分配一个具有该名称的peer。此peer将替换此[Channel](https://hyperledger.github.io/fabric-sdk-node/release-1.4/Channel.html)事件中心的当前peer端点。 |
| as_array    | boolean                                                      | 可选的。仅与链码代码事件一起使用，以指示在块中找到的所有链码事件应作为数组发送到回调，而不是一次发送给默认事件。 |

#### CouchDBOpts

- 类型
  - Object

- 属性

| 名称 | 类型   | 描述                                                  |
| ---- | ------ | ----------------------------------------------------- |
| url  | string | CouchDB实例URL，形式为 http(s)://:@host:port          |
| name | string | 可选的。标识要使用的数据库的名称。默认值：member_db。 |

#### CryptoContent

- 类型
  - Object

- 属性

| 名称          | 类型                                                         | 描述                                                         |
| ------------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| privateKey    | string                                                       | 私钥的PEM文件路径                                            |
| privateKeyPEM | string                                                       | 私钥的PEM字符串（如果设置了privateKey或privateKeyObj，则不需要） |
| privateKeyObj | [module:api.Key](https://hyperledger.github.io/fabric-sdk-node/release-1.4/module-api.Key.html) | 私钥对象（如果设置了privateKey或privateKeyPEM，则不需要）    |
| signedCert    | string                                                       | 证书的PEM文件路径                                            |
| signedCertPEM | string                                                       | 证书的PEM字符串（如果设置了signedCert，则不需要）            |

#### CryptoSetting

- 类型
  - Object

- 属性

| 名称      | 类型    | 描述                                                         |
| --------- | ------- | ------------------------------------------------------------ |
| software  | boolean | 加载基于软件的实施（true）或HSM实施（false）默认为true（对于基于软件的实施），在"crypto-suite-software"设置中指定了特定的实施模块 |
| keysize   | number  | 用于加密套件实例的密钥大小。默认值为设置"crypto-keysize"的值 |
| algorithm | string  | 数字签名算法，目前仅支持ECDSA，其值为"EC"                    |
| hash      | string  | "SHA2"或"SHA3"                                               |

#### DiscoveryChaincodeCall

- 类型
  - Object

- 属性

| 名称             | 类型           | 描述           |
| ---------------- | -------------- | -------------- |
| name             | string         | 链码的名称     |
| collection_names | Array.<string> | 相关集合的名称 |

- 示例

“单个chaincode”

```
{ name: "mychaincode"}
```

“从链码到链码”

```
[ { name: "mychaincode"}, { name: "myotherchaincode"} ]
```

“带有集合的单个chaincode”

```
{ name: "mychaincode", collection_names: ["mycollection"] }
```

“将链码转换为带有集合的链码”

```
[
    { name: "mychaincode", collection_names: ["mycollection"] },
    { name: "myotherchaincode", collection_names: ["mycollection"] }}
  ]
```

“将链码转换为带有集合的链码”

```
[
    { name: "mychaincode", collection_names: ["mycollection", "myothercollection"] },
    { name: "myotherchaincode", collection_names: ["mycollection", "myothercollection"] }}
  ]
```

#### DiscoveryChaincodeInterest

- 类型
  - Object

- 属性

| 名称       | 类型                                                         | 描述                                           |
| ---------- | ------------------------------------------------------------ | ---------------------------------------------- |
| chaincodes | Array.<[DiscoveryChaincodeCall](https://hyperledger.github.io/fabric-sdk-node/release-1.4/global.html#DiscoveryChaincodeCall)> | 链码名称和集合将发送到发现服务以计算认可计划。 |

#### DiscoveryChaincodeQuery

请求DiscoveryResults给定的列表调用。每个interest 都是对一个或多个链码的单独调用，其中可能包括对集合的调用。对于每个给定的interest ，将独立评估背书策略。

- 类型
  - Object

- 属性

| 名称      | 类型                                                         | 描述                      |
| --------- | ------------------------------------------------------------ | ------------------------- |
| interests | Array.<[DiscoveryChaincodeInterest](https://hyperledger.github.io/fabric-sdk-node/release-1.4/global.html#DiscoveryChaincodeInterest)> | 在调用链码时定义interests |

- 示例

“链码，没有集合”

```
{
  interests: [
    { chaincodes: [{ name: "mychaincode"}]}
  ]
}
```

“带有集合的链码”

```
{
  interests: [
    { chaincodes: [{ name: "mychaincode", collection_names: ["mycollection"] }]}
  ]
}
```

“将链码转换为带有集合的链码”

```
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

```
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

| 名称  | 类型                                                         | 描述         |
| ----- | ------------------------------------------------------------ | ------------ |
| peers | Array.<[DiscoveryResultPeer](https://hyperledger.github.io/fabric-sdk-node/release-1.4/global.html#DiscoveryResultPeer)> | 该组中的peer |

#### DiscoveryResultEndorsementLayout

列出组名，以及每个组所需的签名数量。

- 类型
  - Object.<string, number>

#### DiscoveryResultEndorsementPlan

- 类型
  - Object

- 属性

| 名称      | 类型                                                         | 描述                                                         |
| --------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| chaincode | string                                                       | 链码名称，是interest的用于计算此计划的第一个链码。           |
| plan_id   | string                                                       | JSON对象的字符串，它表示用于为此结果构建查询的提示。提示是[DiscoveryChaincodeInterest](https://hyperledger.github.io/fabric-sdk-node/release-1.4/global.html#DiscoveryChaincodeInterest)，其中包含发现服务用来计算返回计划的链码名称和集合。 |
| groups    | Object.<string, [DiscoveryResultEndorsementGroup](https://hyperledger.github.io/fabric-sdk-node/release-1.4/global.html#DiscoveryResultEndorsementGroup)> | 指定背书者，分为组。                                         |
| layouts   | Array.<[DiscoveryResultEndorsementLayout](https://hyperledger.github.io/fabric-sdk-node/release-1.4/global.html#DiscoveryResultEndorsementLayout)> | 指定实现背书策略的选项                                       |

#### DiscoveryResultEndpoint

- 类型
  - Object

- 属性

| 名称 | 类型   | 描述                 |
| ---- | ------ | -------------------- |
| host | string |                      |
| port | number |                      |
| name | string | 可选的。该端点的名称 |

#### DiscoveryResultEndpoints

- 类型
  - Object

- 属性

| 名称      | 类型                                                         | 描述 |
| --------- | ------------------------------------------------------------ | ---- |
| endpoints | Array.<[DiscoveryResultEndpoint](https://hyperledger.github.io/fabric-sdk-node/release-1.4/global.html#DiscoveryResultEndpoint)> |      |

#### DiscoveryResultMSPConfig

- 类型
  - Object

- 属性

| 名称                   | 类型           | 描述                                                         |
| ---------------------- | -------------- | ------------------------------------------------------------ |
| rootCerts              | string         | 此MSP信任的根证书列表。它们在证书验证时使用。                |
| intermediateCerts      | string         | 此MSP信任的中间证书列表。它们在证书验证时使用，如下所示：验证尝试从要验证的证书（位于路径的一端）和RootCerts字段中的证书之一（位于路径的另一端）构建路径。如果路径长于2，则在IntermediateCerts池中搜索中间的证书。 |
| admins                 | string         | 表示此MSP管理员的身份                                        |
| id                     | string         | MSP的标识符                                                  |
| orgs                   | Array.<string> | 属于此MSP配置的fabric组织单位标识符                          |
| tls_root_certs         | string         | 此MSP信任的TLS根证书                                         |
| tls_intermediate_certs | string         | 此MSP信任的TLS中间证书                                       |

#### DiscoveryResultPeer

- 类型
  - Object

- 属性

| 名称          | 类型                                                         | 描述            |
| ------------- | ------------------------------------------------------------ | --------------- |
| mspid         | string                                                       |                 |
| endpoint      | string                                                       | peer的host:port |
| ledger_height | Long                                                         |                 |
| name          | string                                                       |                 |
| chaincodes    | Array.<[DiscoveryResultChaincode](https://hyperledger.github.io/fabric-sdk-node/release-1.4/global.html#DiscoveryResultChaincode)> |                 |

#### DiscoveryResultPeers

- 类型
  - Object

- 属性

| 名称  | 类型                                                         | 描述 |
| ----- | ------------------------------------------------------------ | ---- |
| peers | Array.<[DiscoveryResultPeer](https://hyperledger.github.io/fabric-sdk-node/release-1.4/global.html#DiscoveryResultPeer)> |      |

#### DiscoveryResults

- 类型
  - Object

- 属性

| 名称              | 类型                                                         | 描述                     |
| ----------------- | ------------------------------------------------------------ | ------------------------ |
| msps              | Object.<string, [DiscoveryResultMSPConfig](https://hyperledger.github.io/fabric-sdk-node/release-1.4/global.html#DiscoveryResultMSPConfig)> | 可选的。找到了msp配置。  |
| orderers          | Object.<string, [DiscoveryResultEndpoints](https://hyperledger.github.io/fabric-sdk-node/release-1.4/global.html#DiscoveryResultEndpoints)> | 可选的。找到订购者。     |
| peers_by_org      | Object.<string, [DiscoveryResultPeers](https://hyperledger.github.io/fabric-sdk-node/release-1.4/global.html#DiscoveryResultPeers)> | 可选的。组织的同行发现。 |
| endorsement_plans | Array.<[DiscoveryResultEndorsementPlan](https://hyperledger.github.io/fabric-sdk-node/release-1.4/global.html#DiscoveryResultEndorsementPlan)> | 可选的。                 |
| timestamp         | number                                                       | 发现结果更新的时间戳。   |

#### Endorsement

背书是背书人对提案回复的签名。通过产生认可消息，背书者会隐式“批准”提案响应及其中包含的操作。收集到足够的背书后，就可以从一组提案响应中生成交易。

背书消息具有以下结构：

```
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

| 名称            | 类型   | 描述                                      |
| --------------- | ------ | ----------------------------------------- |
| key             | Object | 私钥                                      |
| certificate     | string | 以64位编码的PEM格式注册的证书             |
| rootCertificate | string | CA签名证书的基于64位编码的PEM编码的证书链 |

#### EnrollmentRequest

- 类型
  - Object

- 属性

| 名称             | 类型                                                         | 描述                                                         |
| ---------------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| enrollmentID     | string                                                       | 用于注册的注册ID                                             |
| enrollmentSecret | string                                                       | 与注册ID关联的secret                                         |
| profile          | string                                                       | 配置文件名称。为TLS证书指定“ tls”配置文件；否则，将颁发入学证书。 |
| csr              | string                                                       | 可选的。 PEM编码的PKCS＃10证书签名请求。从客户端发送到Fabric-ca以获得数字身份证书的消息。 |
| attr_reqs        | Array.<[AttributeRequest](https://hyperledger.github.io/fabric-sdk-node/release-1.4/global.html#AttributeRequest)> | [AttributeRequest](https://hyperledger.github.io/fabric-sdk-node/release-1.4/global.html#AttributeRequest)数组 |

#### EnrollmentResponse

- 类型
  - Object

- 属性

| 名称           | 类型   | 描述                                      |
| -------------- | ------ | ----------------------------------------- |
| enrollmentCert | string | PEM编码的X509注册证书                     |
| caCertChain    | string | 用于颁发证书颁发机构的PEM编码的X509证书链 |

#### EventCount

- 类型
  - Object

- 属性

| 名称     | 类型   | 描述                                     |
| -------- | ------ | ---------------------------------------- |
| success  | Number | 收到的成功事件数。                       |
| fail     | Number | 收到的错误数。                           |
| expected | Number | 预期响应事件（或错误）的事件中心的数量。 |

#### EventHubRegistrationRequest

- 类型
  - Object

- 属性

| 名称        | 类型                                                         | 描述                           |
| ----------- | ------------------------------------------------------------ | ------------------------------ |
| identity    | [Identity](https://hyperledger.github.io/fabric-sdk-node/release-1.4/global.html#Identity) | 进行此注册的身份               |
| txId        | [ TransactionID](https://hyperledger.github.io/fabric-sdk-node/release-1.4/TransactionID.html) | 此注册的交易ID                 |
| certificate | string                                                       | 证书文件，PEM格式              |
| mspId       | string                                                       | 用来处理身份的成员服务提供商ID |

#### Header

Headers 描述有关交易记录的基本信息，例如其类型（配置更新或背书交易等），其所属通道的ID，交易ID等。标头消息还包含一个公共字段SignatureHeader，该字段描述有关如何验证签名的关键信息。

“Headers ”将具有以下对象结构。

```
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

| 名称             | 类型                                                         | 描述                                 |
| ---------------- | ------------------------------------------------------------ | ------------------------------------ |
| role             | [Role](https://hyperledger.github.io/fabric-sdk-node/release-1.4/global.html#Role) | 特定角色的任何身份                   |
| OrganizationUnit |                                                              | 每个信任证书链所属组织单位的任何身份 |
| Identity         |                                                              | 特定身份                             |

#### IdentityRequest

- 类型
  - Object

- 属性

| 名称             | 类型                                                         | 描述                                                         |
| ---------------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| enrollmentID     | string                                                       | 需要。唯一标识身份的注册ID                                   |
| affiliation      | string                                                       | 需要。新身份的关联路径                                       |
| attrs            | Array.<[KeyValueAttribute](https://hyperledger.github.io/fabric-sdk-node/release-1.4/global.html#KeyValueAttribute)> | 要分配给用户的[KeyValueAttribute](https://hyperledger.github.io/fabric-sdk-node/release-1.4/global.html#KeyValueAttribute)属性数组 |
| type             | string                                                       | 可选。身份类型(例如 *user*, *app*, *peer*, *orderer*)        |
| enrollmentSecret | string                                                       | 可选的。注册secret。如果未提供，则会生成随机secret。         |
| maxEnrollments   | number                                                       | 可选的。秘密可用于注册的最大次数。如果为0，则使用fabric-ca-server的已配置max_enrollments；否则为0。如果大于0并且小于等于配置了fabric-ca-server的最大注册人数，请使用max_enrollments;如果大于配置了fabric-ca-server的最大注册数量，则错误。 |
| caname           | string                                                       | 可选的。将请求发送到fabricCA服务器内的CA的名称               |

#### ImplicitMetaPolicy

ImplicitMetaPolicy是一种策略类型，取决于配置的层次结构性质。它是隐式的，因为规则是基于子策略的数量隐式生成的。它是元数据，因为它仅取决于其他策略的结果

评估后，此策略将遍历所有直接子子组，检索名称为sub_policy的策略，评估集合并应用规则。

ImplicitMetaPolicy具有4个子组，策略名称为"Readers"，它检索每个子组，为每个子组检索策略"Readers"，对其进行评估，并且满足ANY 1就足够了，那么ALL将需要4个签名，而MAJORITY将需要3个签名。

"ImplicitMetaPolicy"将具有以下对象结构。

```
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

| 名称               | 类型                                                         | 描述                                                         |
| ------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ |
| target             | string&#124;[Peer](https://hyperledger.github.io/fabric-sdk-node/release-1.4/Peer.html)&#124;[ChannelPeer](https://hyperledger.github.io/fabric-sdk-node/release-1.4/ChannelPeer.html) | 可选的。用于对配置信息进行初始化请求的目标peer。与"targets"参数一起使用时，此处引用的peer将被添加到"targets"数组中。默认是使用分配给该通道的第一个ChannelPeer。 |
| targets            | Array.<([Peer](https://hyperledger.github.io/fabric-sdk-node/release-1.4/Peer.html) &#124; [Channe&#124;Peer](https://hyperledger.github.io/fabric-sdk-node/release-1.4/ChannelPeer.html))> | 可选的。用于对配置信息进行初始化请求的目标peer。当与"target"参数一起使用时，被引用的peer将被添加到"targets"数组中。默认是使用分配给该通道的第一个ChannelPeer。 |
| discover           | boolean                                                      | 可选的。在目标peer上使用发现服务来加载配置和网络信息。默认为false。如果为false，则目标peer将使用peer查询来仅加载配置信息。 |
| endorsementHandler | string                                                       | 可选的。实现EndorsementHandler的自定义背书处理程序的路径。   |
| commitHandler      | string                                                       | 可选的。将发现的主机地址转换为“ localhost”。在本地系统上运行由docker组成的fabric网络时将需要否则应禁用。默认为true。 |
| asLocalhost        | boolean                                                      |                                                              |
| configUpdate       | Array.<byte>                                                 | 可选的。使用序列化的ConfigUpdate protobuf对象初始化此通道。  |



- 类型
  - Object

- 属性

| 名称         | 类型   | 描述                       |
| ------------ | ------ | -------------------------- |
| enrollmentID | string | 需要。唯一标识身份的注册ID |





- 类型
  - Object

- 属性

| 名称         | 类型   | 描述                       |
| ------------ | ------ | -------------------------- |
| enrollmentID | string | 需要。唯一标识身份的注册ID |





- 类型
  - Object

- 属性

| 名称         | 类型   | 描述                       |
| ------------ | ------ | -------------------------- |
| enrollmentID | string | 需要。唯一标识身份的注册ID |





- 类型
  - Object

- 属性

| 名称         | 类型   | 描述                       |
| ------------ | ------ | -------------------------- |
| enrollmentID | string | 需要。唯一标识身份的注册ID |



- 类型
  - Object

- 属性

| 名称         | 类型   | 描述                       |
| ------------ | ------ | -------------------------- |
| enrollmentID | string | 需要。唯一标识身份的注册ID |





- 类型
  - Object

- 属性

| 名称         | 类型   | 描述                       |
| ------------ | ------ | -------------------------- |
| enrollmentID | string | 需要。唯一标识身份的注册ID |



- 类型
  - Object

- 属性

| 名称         | 类型   | 描述                       |
| ------------ | ------ | -------------------------- |
| enrollmentID | string | 需要。唯一标识身份的注册ID |



- 类型
  - Object

- 属性

| 名称         | 类型   | 描述                       |
| ------------ | ------ | -------------------------- |
| enrollmentID | string | 需要。唯一标识身份的注册ID |



- 类型
  - Object

- 属性

| 名称         | 类型   | 描述                       |
| ------------ | ------ | -------------------------- |
| enrollmentID | string | 需要。唯一标识身份的注册ID |









# bitsToBytes



# bytesToBits

# CLIENT

# HFAFFILIATIONMGR

# HFGENCRL

# HFINTERMEDIATECA

# HFREGISTRARATTRIBUTES

# HFREGISTRARDELEGATEROLES

# HFREGISTRARROLES

# HFREVOKER

# jsSHA3

# loadConfigGroup

# loadConfigValue

# ORDERER

# package

# PEER

# toEnvelope

# [USER](#CLIENT)

