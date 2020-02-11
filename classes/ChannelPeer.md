# ChannelPeer

ChannelPeer类表示此通道上目标区块链网络中的对等方。

#### new ChannelPeer(mspid, channel, peer, roles)

使用给定的Peer和opts构造ChannelPeer对象。通道peer对象包含基于通道的引用：该peer所属的组织的MSP ID。[Channel](https://hyperledger.github.io/fabric-sdk-node/release-1.4/Channel.html)对象，用于了解此peer正在交互的通道。[Peer](https://hyperledger.github.io/fabric-sdk-node/release-1.4/Peer.html)对象用于与Hyperledger fabric网络进行交互。 [ChannelEventHub](https://hyperledger.github.io/fabric-sdk-node/release-1.4/ChannelEventHub.html)对象，用于侦听通道上的块更改。[ChannelPeerRoles](https://hyperledger.github.io/fabric-sdk-node/release-1.4/global.html#ChannelPeerRoles) 列表，指示此peer在通道上扮演的角色。 Peer在此通道上扮演的角色由object指示。

- 参数

| 名称    | 类型                                                         | 描述                    |
| :------ | :----------------------------------------------------------- | ----------------------- |
| mspid   | string                                                       | 该peer所属组织的mspid。 |
| channel | [Channel](https://hyperledger.github.io/fabric-sdk-node/release-1.4/Channel.html) | Channel实例。           |
| peer    | [Peer](https://hyperledger.github.io/fabric-sdk-node/release-1.4/Peer.html) | peer实例。              |
| roles   | [ChannelPeerRoles](https://hyperledger.github.io/fabric-sdk-node/release-1.4/global.html#ChannelPeerRoles) | 此peer的角色。          |

### Methods

#### close()

关闭关联的peer服务连接。

参见 [Peer#close](https://hyperledger.github.io/fabric-sdk-node/release-1.4/Peer.html#close)。

参见 [ChannelEventHub#close](https://hyperledger.github.io/fabric-sdk-node/release-1.4/ChannelEventHub.html#close)

#### getChannelEventHub()

获取此通道peer的通道事件中心。使用[Channel](https://hyperledger.github.io/fabric-sdk-node/release-1.4/Channel.html)  newChannelEventHub()方法时，将分配ChannelEventHub实例。使用公共连接配置文件时，ChannelEventHub将在创建并添加到通道的通道peer上自动分配。

返回结果

- 与此[Peer](https://hyperledger.github.io/fabric-sdk-node/release-1.4/Peer.html)实例关联的ChannelEventHub实例。

  - 类型

    [ChannelEventHub](https://hyperledger.github.io/fabric-sdk-node/release-1.4/ChannelEventHub.html)

#### getMspid()

获取MSP ID。

返回结果

- mspId。

  - 类型

    string

#### getName()

获取名称。这是此对象的客户端唯一标识符。

返回结果

- 对象的名称。

  - 类型

    string

#### getPeer()

获取此ChannelPeer在通道上表示的Peer实例。

返回结果

- 关联的peer实例。

  - 类型

    [Peer](https://hyperledger.github.io/fabric-sdk-node/release-1.4/Peer.html)

#### getUrl()

获取此对象的URL。

返回结果

- 获取与peer对象关联的URL。

  - 类型

    string

#### isInOrg(mspid)

检查此peer是否在指定的组织中。未定义传入组织名称时，默认值为true。如果此peer未定义组织名称，则默认值为true。

- 参数

| 名称  | 类型   | 描述        |
| :---- | :----- | ----------- |
| mspid | string | 组织的mspid |

返回结果

- 此peer是否属于组织。

  - 类型

    boolean

#### isInRole()

检查此peer是否处于指定角色。未定义传入角色时，默认值为true。如果此对等方未定义角色，则默认值为true。

返回结果

- 此peer是否属于角色。

  - 类型

    boolean

#### sendDiscovery()

关联peer的包装方法，因此此对象可用作[Peer](https://hyperledger.github.io/fabric-sdk-node/release-1.4/Peer.html) [Peer#sendDiscovery](https://hyperledger.github.io/fabric-sdk-node/release-1.4/Peer.html#sendDiscovery)

#### sendProposal()

关联peer的包装方法，因此该对象可用作[Peer](https://hyperledger.github.io/fabric-sdk-node/release-1.4/Peer.html) [Peer#sendProposal](https://hyperledger.github.io/fabric-sdk-node/release-1.4/Peer.html#sendProposal)

#### setRole(role, isIn)

设置此peer的角色。

- 参数

| 名称 | 类型    | 描述                 |
| :--- | :------ | -------------------- |
| role | string  | 角色名称             |
| isIn | boolean | 该peer是否具有此角色 |