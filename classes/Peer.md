# Peer

## Peer

Peer 类代表目标区块链网络中的一个对等体。该应用程序可以发送背书提议，并通过此类来查询请求。

#### new Peer(url, opts)

使用给定的 url 和 opts 构造一个 Peer 对象。Peer 对象封装了背书 peer 的属性以及通过 grpc 服务 API 与其进行的交互。Peer 对象由[Client](https://hyperledger.github.io/fabric-sdk-node/release-1.4/Client.html) 对象用于发送与通道无关的请求，例如安装链码，查询 peer 以获取已安装的链码等。[Channel](https://hyperledger.github.io/fabric-sdk-node/release-1.4/Channel.html) 对象还使用它们发送通道感知的请求，例如实例化链码和调用事务。

- 参数

| 名称 | 类型                                                                                                   | 描述                              |
| ---- | ------------------------------------------------------------------------------------------------------ | --------------------------------- |
| url  | string                                                                                                 | 格式为"grpc(s)://host:port"的 url |
| opts | [ConnectionOpts](https://hyperledger.github.io/fabric-sdk-node/release-1.4/global.html#ConnectionOpts) | 与 Peer 连接的选项。              |

返回结果

- Peer 实例。

  - 类型

    [Peer](https://hyperledger.github.io/fabric-sdk-node/release-1.4/Peer.html)

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

获取客户端证书哈希

- 继承自 :

[Remote#getClientCertHash](https://hyperledger.github.io/fabric-sdk-node/release-1.4/Remote.html#getClientCertHash)

- 覆写 (Overrides):

[Remote#getClientCertHash](https://hyperledger.github.io/fabric-sdk-node/release-1.4/Remote.html#getClientCertHash)

返回结果

- 客户端证书的哈希。

  - 类型

    Array.&lt;byte&gt;

#### getName()

得到名字。这是此对象的客户端唯一标识符。

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

    string

#### &lt;async&gt; sendDiscovery(request, timeout)

向该 Peer 发送发现请求。

- 参数

| 名称    | 类型          | 描述                                                                                                                             |
| ------- | ------------- | -------------------------------------------------------------------------------------------------------------------------------- |
| request | SignedRequest | [Proposal](https://github.com/hyperledger/fabric/blob/release-1.2/protos/discovery/protocol.proto)类型的 protobuf 编码的字节数组 |
| timeout | Number        | 一个数字，表示等待响应之前等待超时（以毫秒为单位）的毫秒数。这将覆盖 Peer 实例的默认超时和配置设置中的全局超时。                 |

返回结果

- DiscoveryResponse 的 Promise。

  - 类型

    Promise

#### &lt;async&gt; sendProposal(proposal, timeout)

发送背书给背书人。这用于调用背书对等体以执行链码来处理交易建议或运行查询。

- 参数

| 名称     | 类型     | 描述                                                                                                                        |
| -------- | -------- | --------------------------------------------------------------------------------------------------------------------------- |
| proposal | Proposal | [Proposal](https://github.com/hyperledger/fabric/blob/release-1.2/protos/peer/proposal.proto)类型的 protobuf 编码的字节数组 |
| timeout  | Number   | 一个数字，表示等待响应之前等待超时（以毫秒为单位）的毫秒数。这将覆盖 Peer 实例的默认超时和配置设置中的全局超时。            |

返回结果

- [ProposalResponse](https://hyperledger.github.io/fabric-sdk-node/release-1.4/global.html#ProposalResponse)的 Promise。

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
