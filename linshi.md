#### RegistrationOpts

- 类型
  - Object

- 属性

| 名称       | 类型                   | 描述                                                         |
| ---------- | ---------------------- | ------------------------------------------------------------ |
| startBlock | integer                | 可选-事件检查的起始块号。如果包括该peer，则将要求peer的fabric服务从该块号开始发送块。这是恢复或重播已添加到分类账中的错过的区块的方法。默认值是分类账上的最新块。设置startBlock可能会使其他事件侦听器感到困惑，因此，当使用startBlock时，ChannelEventHub上将只允许一个侦听器。设置startBlock还需要此ChannelEventHub使用不同的选项连接到fabric服务。必须在调用connect()之前完成向startBlock的注册。 |
| endBlock   | integer&#124; 'newest' | 可选-事件检查的结束块号。值“最新”表示在注册时，对等方会将endBlock计算为分类账上的最新块。这允许应用程序重播到分类账上的最新块，然后侦听器将停止并通过“ onError”回调通知。如果包括该peer，则peer的fabric服务将在该块交付后被要求停止发送块。这是重播已添加到分类帐中的错过的区块的方法。如果不包含startBlock，则endBlock必须等于或大于当前通道块的高度。设置endBlock可能会混淆其他事件侦听器，因此，在使用endBlock时，ChannelEventHub上将只允许一个侦听器。设置endBlock还需要此ChannelEventHub使用不同的选项连接到结构服务。必须在调用connect()之前完成对endBlock的注册。 |
| unregister | boolean                | 可选-此选项设置指示看到事件时应删除注册（取消注册(unregister)）。当应用程序使用超时仅等待指定的时间量才能看到事务时，超时处理应包括对事务事件侦听器的手动“注销”，以避免意外调用事件回调。对于不同类型的事件侦听器，此设置的默认设置是不同的。对于块侦听器，默认值为true，但是仅当将end_block设置为选项并且该侦听器看到了end_block时，才假定事件侦听器已看到最终事件。对于事务侦听器，默认值为true，并且当此侦听器看到具有ID的事务时，该侦听器将被注销。对于链码侦听器，默认值将为false，因为匹配过滤器可能用于许多事务，而不是其他侦听器中的特定事务或块。如果未设置并且已设置endBlock，则侦听器将自动注销。 |
| disconnect | boolean                | 可选-此选项设置指示ChannelEventHub实例在看到事件后自动与对等方的Fabric服务断开连接。默认值为false。如果未设置并且已设置endBlock，则ChannelEventHub实例将自动断开自身连接。 |

#### RemoteCharacteristics

与对等实例对象有关的信息。

- 类型
  - Object
- 属性

| 名称    | 类型   | 描述                                                         |
| ------- | ------ | ------------------------------------------------------------ |
| url     | string | peer的url                                                    |
| name    | string | 此peer的名称，如果未使用name选项创建peer，则它将是主机和端口。 |
| options | Object | 该peer基于默认值以及创建时传递的内容构建的选项对象。典型的选项将包括GRPC连接设置。 |

#### REQUEST_TIMEOUT

错误消息字符串，指示由于远程节点问题，请求操作已超时。如果本地系统存在问题，将返回"SYSTEM_TIMEOUT"错误消息。对于两种类型的超时，该操作将仅使用一个计时器。随着操作开始，计时器将开始运行。如果计时器在本地实例能够发出出站请求之前到期，则将返回“ SYSTEM_TIMEOUT”错误。如果本地实例能够发出出站请求，并且计时器在远程节点响应之前到期，则返回“ REQUEST_TIMEOUT”。计时器由“请求超时”( 'request-timeout' )设置控制或在发出出站请求的呼叫中传递

- 类型
  - Error
- 示例

```javascript
'client.setConfigSetting('request-timeout', 3000)'
```

```javascript
'channel.sendTranaction(request, 3000)'
```

#### Response

表示批准提案是否成功的响应消息

- 类型
  - Object
- 属性

| 名称    | 类型         | 描述                                                         |
| ------- | ------------ | ------------------------------------------------------------ |
| status  | number       | 状态码。遵循：[HTTP status code definitions](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html) |
| message | string       | 与响应状态代码关联的消息                                     |
| payload | Array.<byte> | 可用于在此响应中包含元数据的有效负载                         |

#### Restriction

- 类型
  - Object
- 属性

| 名称          | 类型 | 描述                                                        |
| ------------- | ---- | ----------------------------------------------------------- |
| revokedBefore | Date | 在CRL中包括在此UTC时间戳之前被吊销的证书（采用RFC3339格式） |
| revokedAfter  | Date | 在CRL中包括在此UTC时间戳之后（RFC3339格式）被吊销的证书     |
| expireBefore  | Date | 在CRL中包含在此UTC时间戳之前（RFC3339格式）过期的已撤销证书 |
| expireAfter   | Date | 在CRL中包括在此UTC时间戳之后（RFC3339格式）到期的已撤销证书 |

#### Role

- 类型
  - Object
- 属性

| 名称  | 类型   | 描述                                |
| ----- | ------ | ----------------------------------- |
| name  | string | 角色名称。值可以是"member"或"admin" |
| mspId | string | 用来处理身份的成员服务提供商ID      |

#### ServiceResponse

- 类型
  - Object

- 属性

| 名称     | 类型                                                         | 描述                       |
| -------- | ------------------------------------------------------------ | -------------------------- |
| Success  | boolean                                                      | 布尔值，指示请求是否成功   |
| Result   | Object                                                       | 该请求的结果               |
| Errors   | Array.<[ServiceResponseMessage](https://hyperledger.github.io/fabric-sdk-node/release-1.4/global.html#ServiceResponseMessage)> | 错误消息数组（代码和消息） |
| Messages | Array.<[ServiceResponseMessage](https://hyperledger.github.io/fabric-sdk-node/release-1.4/global.html#ServiceResponseMessage)> | 信息消息数组（代码和消息） |

#### ServiceResponseMessage

- 类型
  - Object

- 属性

| 名称    | 类型   | 描述                   |
| ------- | ------ | ---------------------- |
| code    | number | 表示消息类型的整数代码 |
| message | string | 更具体的信息           |

#### SignatureHeader

Hyperledger Fabric中所有签名的一部分的对象。 “创建者”字段包含有关签名者身份的两个重要信息，签名者所属的组织（Mspid）和证书（IdBytes）。 “ nonce”字段是防止重放攻击的唯一值。

```javascript
creator
	Mspid -- {string}
	IdBytes -- {byte[]}
nonce -- {byte[]}
```

- 类型
  - Object

#### SignaturePolicy

SignaturePolicy是一种递归消息结构，它定义了一个轻量级DSL，用于描述比“恰好此签名”更复杂的策略。 NOutOf运算符足以表达AND和OR，当然还有以下M个策略中的N个。

SignedBy表示签名来自有效证书，该证书由字节中指定的受信机构签名。这将是自签名证书的证书本身，并将是更传统证书的CA

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

| 名称              | 类型                                                         | 描述                                                         |
| ----------------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| request           | [TransactionRequest](https://hyperledger.github.io/fabric-sdk-node/release-1.4/global.html#TransactionRequest) | 必要。提交请求                                               |
| signedTransaction | Buffer                                                       | 必要。签署的交易                                             |
| orderer           | [Orderer](https://hyperledger.github.io/fabric-sdk-node/release-1.4/Orderer.html) &#124; string | 可选的。要操作的orderer实例或orderer的字符串名称。请参阅Client.getTargetOrderer |

#### SignedEvent

- 类型
  - Object

- 属性

| 名称      | 类型   | 描述                           |
| --------- | ------ | ------------------------------ |
| signature | Buffer | 该有效负载上的签名             |
| payload   | Buffer | 要发送到peer的有效载荷字节数组 |

#### SignedProposal

- 类型
  - Object

- 属性

| 名称           | 类型                                                         | 描述             |
| -------------- | ------------------------------------------------------------ | ---------------- |
| targets        | Array.<[Peer](https://hyperledger.github.io/fabric-sdk-node/release-1.4/Peer.html)> | 必要。函数名称。 |
| signedProposal | Buffer                                                       | 必要。签署的提案 |

#### SYSTEM_TIMEOUT

错误消息字符串，指示由于系统问题，请求操作已超时。这将表明问题是本地的，而不是远程的。对于两种类型的超时，该操作将仅使用一个计时器。随着操作开始，计时器将开始运行。如果计时器在本地实例能够发出出站请求之前到期，则将返回“ SYSTEM_TIMEOUT”错误。如果本地实例能够发出出站请求，并且计时器在远程节点响应之前到期，则返回“ REQUEST_TIMEOUT”。计时器由“请求超时”设置控制或在发出出站请求的呼叫中传递

- 类型
  - Error

- 示例

```javascript
'client.setConfigSetting('request-timeout', 3000)'
```

```javascript
'channel.sendTranaction(request, 3000)'
```

#### TLSOptions

- 类型
  - Object

- 属性

| 名称         | 类型           | 描述                                              |
| ------------ | -------------- | ------------------------------------------------- |
| trustedRoots | Array.<string> | PEM编码的受信任根证书的数组                       |
| verify       | boolean        | 可选。默认为true。确定使用TLS时是否验证服务器证书 |

#### Transaction

交易（或“背书人交易”）是调用链码以收集背书的结果，在渠道的上下文中进行全局排序，并在最终正式“提交”到区块内部的分类帐之前，被提交者peer作为区块的一部分进行验证。每笔交易都包含代表一系列执行交易的不同步骤的“动作”，这些步骤将被原子地处理，这意味着如果任何一个步骤失败，则整个交易将被标记为已拒绝。

"actions" 数组的每个条目都包含一个链码提议和相应的提议响应，这些响应封装了背书的对等方关于该提议是否被视为有效的决定。请注意，即使背书的peer认为交易建议有效，提交人在交易验证期间仍可能拒绝交易建议。整个交易是否有效，并不反映在交易记录本身中，而是记录在区块元数据的单独字段中。

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

| 名称          | 类型   | 描述                                                         |
| ------------- | ------ | ------------------------------------------------------------ |
| commitTimeout | Number | 可选。默认为0.等待事务完成的秒数。零值表示处理程序应无限期等待。 |

#### TransactionRequest

- 类型
  - Object

- 属性

| 名称              | 类型                                                         | 描述                                                         |
| ----------------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| proposalResponses | Array.<[ProposalResponse](https://hyperledger.github.io/fabric-sdk-node/release-1.4/global.html#ProposalResponse)> | 包含[endorsement](https://hyperledger.github.io/fabric-sdk-node/release-1.4/Channel.html#sendTransactionProposal)调用响应的数组或单个[ProposalResponse](https://hyperledger.github.io/fabric-sdk-node/release-1.4/global.html#ProposalResponse)对象 |
| proposal          | Proposal                                                     | 包含原始认可请求的投标对象                                   |
| txID              | TransactionId                                                | 可选的。 -必须是提案认可中使用的交易ID对象。 transactionID将仅用于确定请求的签名是否应由管理员身份或分配给客户端实例的用户来完成。 |
| orderer           | [ Orderer](https://hyperledger.github.io/fabric-sdk-node/release-1.4/Orderer.html) &#124; string | 可选的。要操作的orderer实例或orderer的字符串名称。请参阅Client.getTargetOrderer |

#### UserNamePasswordObject

在'setUserContext'调用上使用的替代对象，以代替'User'对象。使用此对象时，假定当前的“客户端”实例已加载了公共连接配置文件。

- 类型
  - Object

- 属性

| 名称     | 类型   | 描述                                                         |
| -------- | ------ | ------------------------------------------------------------ |
| username | string | 需要。代表用户的用户名的字符串                               |
| password | string | 可选的。代表用户密码的字符串                                 |
| caName   | string | 可选的。代表认证中心名称的字符串。如果未指定，将使用列表中的第一个证书颁发机构。 |

#### UserOpts

- 类型
  - Object

- 属性

| 名称            | 类型                                                         | 描述                                            |
| --------------- | ------------------------------------------------------------ | ----------------------------------------------- |
| username        | string                                                       | 用于注册的用户名                                |
| mspid           | string                                                       | MSP ID                                          |
| cryptoContent   | [CryptoContent](https://hyperledger.github.io/fabric-sdk-node/release-1.4/global.html#CryptoContent) | 私钥和证书                                      |
| skipPersistence | boolean                                                      | 是否将此新用户对象保存到持久性(persistence)中。 |









































