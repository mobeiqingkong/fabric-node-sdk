# Orderer

## Orderer

Orderer 类封装了与目标区块链网络中的 Orderer 节点进行交互的客户端功能。orderer节点公开了两个 API: broadcast()和 deliver()。两者都是流 API，因此客户端和orderer之间存在持久的 grpc 流连接，在这两个方向上都可以交换消息。broadcast() API 用于将交易发送到orderer进行处理。 delivery() API 用于向orderer询问诸如通道配置之类的信息。

#### new Orderer(url, opts)

使用给定的 url 和 opts 构造一个 Orderer 对象。一个 Orderer 对象封装了 Orderer 节点的属性以及通过 grpc 流 API 与其交互的属性。 [Client](https://hyperledger.github.io/fabric-sdk-node/release-1.4/Client.html) 对象使用 Orderer 对象来广播创建和更新通道的请求。[Channel](https://hyperledger.github.io/fabric-sdk-node/release-1.4/Channel.html)对象还使用它们来广播订购交易的请求。

- 参数

| 名称 | 类型                                                                                                    | 描述                            |
| ---- | ------------------------------------------------------------------------------------------------------- | ------------------------------- |
| url  | string                                                                                                  | "grpc(s)://host:port"格式的 URL |
| opts | [ConnectionOpts](https://hyperledger.github.io/fabric-sdk-node/release-1.4/global.html#ConnectionOpts) | 连接到 orderer 的选项。         |

返回结果

- Orderer 实例。

  - 类型

    [Orderer](https://hyperledger.github.io/fabric-sdk-node/release-1.4/Orderer.html)

### 扩展

- [Remote](https://hyperledger.github.io/fabric-sdk-node/release-1.4/Remote.html)

### Methods

#### close()

关闭服务连接。

#### getCharacteristics()

获取此远程端点的特征它的名称，URL 和连接选项是使该实例唯一的项。这些项目在调试问题或验证响应时可能很有用。

- 继承自 :

[Remote#getCharacteristics](https://hyperledger.github.io/fabric-sdk-node/release-1.4/Remote.html#getCharacteristics)

- 覆写 (Overrides):

[Remote#getCharacteristics](https://hyperledger.github.io/fabric-sdk-node/release-1.4/Remote.html#getCharacteristics)

#### getClientCertHash()

获取客户端证书哈希。

- 继承自 :

[Remote#getClientCertHash](https://hyperledger.github.io/fabric-sdk-node/release-1.4/Remote.html#getClientCertHash)

- 覆写 (Overrides):

[Remote#getClientCertHash](https://hyperledger.github.io/fabric-sdk-node/release-1.4/Remote.html#getClientCertHash)

返回结果

- 客户端证书的哈希。

  - 类型

    Array.&lt;byte&gt;

#### getName()

获取名称。这是此对象的客户端唯一标识符。

- 继承自 :

[Remote#getName](https://hyperledger.github.io/fabric-sdk-node/release-1.4/Remote.html#getName)

- 覆写 (Overrides):

[Remote#getName](https://hyperledger.github.io/fabric-sdk-node/release-1.4/Remote.html#getName)

返回结果

- 对象的名称。

  - 类型

    string

#### getUrl()

获取此对象的 URL。

- 继承自 :

[Remote#getUrl](https://hyperledger.github.io/fabric-sdk-node/release-1.4/Remote.html#getUrl)

- 覆写 (Overrides):

[Remote#getUrl](https://hyperledger.github.io/fabric-sdk-node/release-1.4/Remote.html#getUrl)

返回结果

- 获取与对象关联的 URL。

  - 类型

    string

#### isTLS()

确定此远程端点是否使用 TLS。

- 继承自 :

[Remote#isTLS](https://hyperledger.github.io/fabric-sdk-node/release-1.4/Remote.html#isTLS)

- 覆写 (Overrides):

[Remote#isTLS](https://hyperledger.github.io/fabric-sdk-node/release-1.4/Remote.html#isTLS)

返回结果

- 如果此端点使用 TLS，则为 true，否则为 false。

  - 类型

    boolean

#### &lt;async&gt; sendBroadcast(envelope, timeout)

将广播消息发送到 orderer 服务。

- 参数

| 名称     | 类型               | 描述                                                                                                                                                                                                                                                                                                                                                                                                                     |
| -------- | ------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| envelope | Array.&lt;byte&gt; | 广播中要包含的字节数据。这必须是[common.Envelope](https://github.com/hyperledger/fabric/blob/v1.0.0/protos/common/common.proto#L132)的经 protobuf 编码的字节数组，该信封在信封的 payload.data 属性中包含[ConfigUpdateEnvelope](https://github.com/hyperledger/fabric/blob/v1.0.0/protos/common/configtx.proto#L70)或[Transaction](https://github.com/hyperledger/fabric/blob/v1.0.0/protos/peer/transaction.proto#L70)。 |
| timeout  | Number             | 一个数字，表示等待响应之前等待超时(以毫秒为单位)的毫秒数。这将覆盖 Peer 实例的默认超时和配置设置中的全局超时。                                                                                                                                                                                                                                                                                                         |

抛出

- [SYSTEM_TIMEOUT](https://hyperledger.github.io/fabric-sdk-node/release-1.4/global.html#SYSTEM_TIMEOUT) | [REQUEST_TIMEOUT](https://hyperledger.github.io/fabric-sdk-node/release-1.4/global.html#REQUEST_TIMEOUT)

返回结果

- [BroadcastResponse](https://hyperledger.github.io/fabric-sdk-node/release-1.4/global.html#BroadcastResponse) 对象的 Promise。

- 类型

  Promise

#### &lt;async&gt; sendDeliver(envelope)

发送传递消息到orderer服务。

- 参数

| 名称     | 类型               | 描述                                                                                                                                                                                                                                                                                                                                                                                                                                                        |
| -------- | ------------------ | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| envelope | Array.&lt;byte&gt; | 广播中要包含的字节数据。这必须是[common.Envelope](https://github.com/hyperledger/fabric/blob/v1.0.0/protos/common/common.proto#L132)的经 protobuf 编码的字节数组，该信封在信封的有效负载中包含一个[SeekInfo](https://github.com/hyperledger/fabric/blob/v1.0.0/protos/orderer/ab.proto#L54)。 header.channelHeader.type 必须设置为[common.HeaderType.DELIVER_SEEK_INFO](https://github.com/hyperledger/fabric/blob/v1.0.0/protos/common/common.proto#L44)。 |

抛出

- [SYSTEM_TIMEOUT](https://hyperledger.github.io/fabric-sdk-node/release-1.4/global.html#SYSTEM_TIMEOUT) | [REQUEST_TIMEOUT](https://hyperledger.github.io/fabric-sdk-node/release-1.4/global.html#REQUEST_TIMEOUT)

返回结果

- 一个对 common.Block 类型的 protobuf 对象的 Promise。请注意，这与由 BlockDecoder.decode()方法和各种其他方法返回的 [Block](https://hyperledger.github.io/fabric-sdk-node/release-1.4/global.html#Block) 的对象类型不同。[Block](https://hyperledger.github.io/fabric-sdk-node/release-1.4/global.html#Block) 是纯 JavaScript 对象，而此方法返回的对象是包含访问器方法的 protobuf 对象，每个属性的 getter 和 setter 以及 toBuffer()都将用于进一步处理该对象并在字节数组之间来回转换。

  - 类型

    Promise

#### setName(name)

将名称设置为该对象的客户端唯一标识符。

- 参数

| 名称 | 类型   | 描述 |
| ---- | ------ | ---- |
| name | string |      |

- 继承自 :

[Remote#setName](https://hyperledger.github.io/fabric-sdk-node/release-1.4/Remote.html#setName)

- 覆写 (Overrides):

[Remote#setName](https://hyperledger.github.io/fabric-sdk-node/release-1.4/Remote.html#setName)

#### toString()

返回此对象的可打印表示形式

- 覆写 (Overrides):

[Remote#toString](https://hyperledger.github.io/fabric-sdk-node/release-1.4/Remote.html#toString)

---
