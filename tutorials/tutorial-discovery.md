# fabric-client:如何使用发现服务(How to use the discovery service)

## 说明

本教程从 1.2 版本开始说明 Hyperledger Fabric Node.js 客户端对服务发现的使用。

有关更多信息:

- Hyperledger Fabric 入门，请参[阅构建第一个网络(Building your first network)](http://hyperledger-fabric.readthedocs.io/en/latest/build_network.html)。
- Hyperledger Fabric 中通道的配置以及创建和更新的内部过程，请参见[Hyperledger Fabric 通道配置(Hyperledger Fabric channel configuration)](http://hyperledger-fabric.readthedocs.io/en/latest/configtx.html)
- [服务发现(Service Discovery)](https://hyperledger-fabric.readthedocs.io/en/latest/discovery-overview.html)

以下内容假定您对 Hyperledger Fabric 网络(Orderer 和 Peer)以及 Node 应用程序开发有所了解，包括对 Javascript promise 和 async await 的使用。

### 概述

Hyperledger Fabric 提供的服务发现可帮助应用程序了解网络的当前视图。 服务发现还洞察了链码的背书策略，并能够提供网络上当前活跃的 peer 的各种列表，这些 peer 可用于背书提案。 要使用该服务，应用程序将只能与一个 peer 连接。

#### 修改后的 API 将使用服务发现

- channel.initialize() - 通过添加使用新服务发现初始化通道对象的 peer 的查询选项，此方法得到了增强。 可以随时调用此方法以重新初始化通道。 当使用发现时，这可以用于分配提供发现服务的新目标 peer。 实例化处理程序也需要 initialize()方法，默认情况下，fabric-client 附带的处理程序被设计为使用发现结果。
- channel.sendTransactionProposal() - 此方法已得到增强，可以使用发现的 peer 发送背书。
- channel.sendTransaction()-此方法已得到增强，可以使用发现的 orderer 发送已签名的背书。

#### 使用服务发现的新 API

- channel.refresh() - 将使用新的服务发现结果刷新该通道，添加新的 peer，orderer 和 MSP。该调用将使用 channel.initialize()调用中提供的服务发现设置。如果需要新的 peer 来刷新发现结果，则使用新的目标 peer 调用 channel.initialize()，而不是调用 refresh()。
- channel.getDiscoveryResults() - 通道将缓存对服务发现进行的最后一次查询的结果并使结果可用。该调用将使用 Discovery-cache-life 设置来确定是否应刷新结果。如果需要刷新结果，则将在内部调用 channel.refresh()以获取新的服务发现结果。 DiscoveryEndorsementHandler 使用该调用开始确定目标 peer。
- client.queryPeers() - 客户端对象将能够使用发现服务查询目标 peer，以提供查询时网络上活动的 Peer 端点和关联组织的列表。请参阅 [Client#queryPeers](https://hyperledger.github.io/fabric-sdk-node/release-1.4/Client.html#queryPeers)

#### 新的配置设置(New configuration settings)

- initialize-with-discovery - 布尔值(boolean)-当应用程序要求初始化通道时，将使用服务发现。(默认为 false)
- Discovery-cache-life - 整数(integer)(以毫秒为单位的时间) - 服务发现结果被视为有效的时间。(默认 300000-5 分钟)
- override-discovery-protocol-字符串(string)-覆盖在为发现的端点构建 URL 时使用的协议。发现服务仅提供 host:port。默认情况下，如果您不使用 TLS(grpc://)连接到发现服务，则所有发现的端点将不使用 TLS 连接到。如果使用 TLS(grpcs://)连接到 Discovery Service，则所有发现的端点都将使用 TLS 连接。您可以使用此配置设置为所有发现的端点强制使用 grpc 或 grpcs，无论您如何连接到发现服务。请注意，强烈建议不要连接到发现服务或没有 TLS(grpc://)的任何发现的端点，因为所有信息都将以未加密的纯文本形式发送。
- endorsement-handler-字符串(string)-背书处理程序的路径。允许使用自定义处理程序。sendTransactionProposal 方法中使用此处理程序来确定目标 peer 以及如何发送建议。(默认为'fabric-client/lib/impl/DiscoveryEndorsementHandler.js')
- commit-handler-字符串-提交处理程序的路径。允许使用自定义处理程序。在 sendTransaction 方法中使用此处理程序来确定排序器以及如何发送要提交的事务。(默认为'fabric-client/lib/impl/BasicCommitHandler.js')

#### DiscoveryEndorsementHandler 是如何运作的

sendTransactionProposal 将使用"targets"中包含的 Peer 来批准提案。 如果没有"targets"参数，则背书请求将由背书处理程序处理。 Fabric 客户端随附的默认处理程序旨在使用 Fabric 发现服务的结果。 在通道初始化期间分配为目标的 Peer 将是发送发现服务请求的 Peer。 发现服务结果将基于背书的链码或背书请求中包含的背书提示(endorsementHint)。 该提示可以包括一个或多个链码，并且每个链码可以包括一个或多个关联的集合名称。 结果将使用 Discovery-cache-life 系统设置进行刷新。 默认情况下，缓存寿命为 5 分钟。 使用以下方法可以轻松更改。

```javascript
Client.setConfigSetting('discover-cache-life', <milliseconds>);
```

如果没有服务发现结果，则处理程序会将背书请求发送给已分配给具有 endorsingPeer 角色的通道的 Peer(未分配角色的 Peer 将默认具有该角色，这意味着角色必须明确关闭)。

当处理程序处理发现服务结果时，它假定所引用的所有 Peer 均具有创建并分配给通道实例对象的 peer 实例对象。通道实例将在将结果传递给处理程序之前处理发现服务结果时，将构建所需的对等实例以支持背书。如果已通过应用程序或先前的发现服务请求将同级分配给该通道，则该通道将不会构建新的同级实例。

默认的"DiscoveryEndorsementHandler"具有可选参数，这些可选参数允许应用程序指定将被首选，忽略或要求的同级或组织。发现服务结果将包括 peer 组和布局，这些布局和布局指定从每个组中需要多少 peer 来满足提案链码的背书策略或背书提示的背书策略。每个组将使用背书调用的参数进行修改。处理程序将首先删除不需要或应忽略的 peer。然后，组列表将按分类帐高度排序或随机分组。最后，首选 Peer 将移至组列表的顶部。处理程序将随机选择一个布局以进行背书。处理程序查看布局中的每个组，然后选择布局中该组指定的 peer 数。peer 数量是满足背书策略所需的背书数量。将在修改后的群组列表的顶部开始选择 Peer，以向其发送背书请求。如果任何请求失败，则处理程序将从已修改的组列表中选择下一个可用 Peer。如果成功背书的数量达到布局中每个组呼出的 Peer 的数量，则处理程序将成功返回背书。如果没有足够的成功背书，处理程序将选择另一个随机布局，然后重试或返回错误，表明它无法成功完成。该错误将包括所有 Peer 的响应。

注意:默认处理程序不会记住上一次调用的结果。可能失败的 Peer 将再次尝试。随着发现服务结果的随机化和刷新，peer 的选择顺序可能会随每个请求而改变。

注意:如果上述行为不能满足您组织的需求，则可以使用自定义处理程序。

#### BasicCommitHandler 是如何运作的

Fabric 客户端随附的默认处理程序将一次发送给一个 orderer，直到成功接收到事务提交。 将交易(一组背书)发送给 orderer 并不意味着该交易将被提交，这意味着该请求已正确构建并且发送者有权发送该请求。 orderer 的响应将指示 orderer 已接受该请求。 sendTransaction 有一个可选的参数 orderer，指示 orderer 发送交易。 处理程序将使用 orderer 参数指定的排序程序，而不发送给其他任何排序程序。 如果未指定任何 orderer，则处理程序将获取分配给该通道的 orderer 列表。 这些 orderer 可能已通过 channel.addOrderer()调用手动分配给了通道，或者在使用服务发现时自动分配了。

### 初始化

默认情况下，Fabric 客户端将不使用服务发现。 要启用该服务，请将 config 设置设置为 true 或在 initialize()调用上使用 discover 参数。

注意:必须运行[Channel#initialize](https://hyperledger.github.io/fabric-sdk-node/release-1.4/Channel.html#initialize)才能启用发现并启动处理程序。

```javascript
Client.setConfigSetting("initialize-with-discovery", true);
--or--;
Client.addConfigFile("/path/to/config.json");
// the json file contains the following line
//"initialize-with-discovery": true

//--or--

await channel.initialize({ discover: true });
```

要在 initialize()上使用服务发现，一个通道必须至少具有一个分配了发现角色的 Peer，或者必须在呼叫中提供目标。 可以通过加载连接配置文件来自动分配 Peer，或者可以使用 channel.addPeer()手动添加 Peer。

```javascript
await channel.initialize({
  discover: true,
  target: peer
});

//--or--

await channel.initialize({
  discover: true,
  target: "peer2.org2.example.com" //peer defined in the connection profile
});

//--or--

// no target specified, using the first peer with the discover role
// peer was added to the channel either by the 'addPeer' or when using
// a connection profile
await channel.initialize({
  discover: true
});
```

通过服务发现进行初始化的返回结果将是 MSP 配置，peer，orderer 以及通道上所有 JSON 格式的链码的背书计划。 结果在内部存储和缓存，调用者不必对结果做任何事情，它们仅供参考。

初始化调用还允许通过指定自定义背书处理程序的路径来更改背书处理程序。 可以独立于使用服务发现来更改处理程序。 但是，默认的背书处理程序确实使用发现服务结果来确定背书的 peer。

```javascript
await channel.initialize({
  endorsementHandler: "/path/to/my/handler.js"
});
```

当 Fabric 网络在 docker-compose 中运行并且 node.js 应用程序在 docker 容器之外运行时，有必要修改从服务发现返回的地址。 服务发现将 peer 和 orderer 的地址视为主机名和端口，但是在 docker 外部运行的 node.js 应用程序仅将端点称为 localhost 和 port。 在 docker-compose 文件中，请注意 Docker 如何映射端口地址，使用服务发现时，这些地址必须相同。 使用 \- 7061:7051 将无法使用，因为 Fabric 客户端无法看到 docker-compose 文件。

请注意以下来自 docker-compose 文件的 peer 的定义。 端口号是相同的，并且已与 peer 和 gossip 设置的主机名 peer0.org1.example.com:7051 一起定义。 在 docker 容器外部运行的 node.js Fabric 客户端应用程序将使用 localhost:7051。

```yaml
peer0.org1.example.com:
  container_name: peer0.org1.example.com
  image: hyperledger/fabric-peer
  environment:
    - CORE_VM_ENDPOINT=unix:///host/var/run/docker.sock
    - CORE_PEER_ID=peer0.org1.example.com
    - CORE_PEER_ADDRESS=peer0.org1.example.com:7051
    - CORE_PEER_LISTENADDRESS=peer0.org1.example.com:7051
    - CORE_PEER_GOSSIP_ENDPOINT=peer0.org1.example.com:7051
    - CORE_PEER_GOSSIP_EXTERNALENDPOINT=peer0.org1.example.com:7051
    - FABRIC_LOGGING_SPEC=debug
    ## the following setting redirects chaincode container logs to the peer container logs
    - CORE_VM_DOCKER_ATTACHSTDOUT=true
    - CORE_PEER_LOCALMSPID=Org1MSP
    - CORE_PEER_MSPCONFIGPATH=/etc/hyperledger/msp/peer
    ##
    - CORE_PEER_TLS_ENABLED=true
    - CORE_PEER_TLS_CLIENTAUTHREQUIRED=true
    - CORE_PEER_TLS_KEY_FILE=/etc/hyperledger/msp/peer/tls/key.pem
    - CORE_PEER_TLS_CERT_FILE=/etc/hyperledger/msp/peer/tls/cert.pem
    - CORE_PEER_TLS_ROOTCERT_FILE=/etc/hyperledger/msp/peer/cacerts/org1.example.com-cert.pem
    - CORE_PEER_TLS_CLIENTROOTCAS_FILES=/etc/hyperledger/msp/peer/cacerts/org1.example.com-cert.pem
    # # the following setting starts chaincode containers on the same
    # # bridge network as the peers
    # # https://docs.docker.com/compose/networking/
    - CORE_VM_DOCKER_HOSTCONFIG_NETWORKMODE=fixtures_default
  working_dir: /opt/gopath/src/github.com/hyperledger/fabric
  command: peer node start
  ports:
    - 7051:7051
  volumes:
    - /var/run/:/host/var/run/
    - ./channel/crypto-config/peerOrganizations/org1.example.com/peers/peer0.org1.example.com/:/etc/hyperledger/msp/peer
  depends_on:
    - orderer.example.com
```

channel.initialize()作为新参数调用，以指示应将主机名映射到 localhost。将 asLocalhost 与 true 或 false 一起使用，默认值为 false。

```javascript
await channel.initialize({ discover: true, asLocalhost: true });
```

#### 使用手动添加到通道的 peer

当应用程序以编程方式添加 peer 和 orderer 以构建通道实例时，现在仅需要将一个 peer 添加到通道。 peer 必须具有发现角色 (role discover)。 与所有角色一样，如果未定义角色并将其设置为 false，则默认情况下，peer 将在通道上具有该角色。

```javascript
const channel = client.newChannel('mychannel');
const peer = client.newPeer(....);
channel.addPeer(peer);
await channel.initialize({discover:true});
```

使用服务发现对通道进行初始化并将 peer 和 orderer 添加到该通道后，具有用于服务发现的地址的 peer 将很可能位于已发现 peer 的列表中。 具有用于服务发现的地址的 peer 将不会再次添加到该通道，因为已经将该地址的 peer 分配给该通道。

创建 peer 时，可以使用 name 设置来设置 peer 将要知道的名称。

```javascript
const peer = client.newPeer(url, {name: 'peer0', ...});
```

如果未提供 name 参数或服务发现添加的 peer，则 peer 的默认名称将是主机名和端口。

```yaml
peer0.org1.example.com:7051
```

#### 使用未添加到通道的 peer

要使用服务发现，需要一个 Peer。 应用程序可以定义一个 peer，并将其传递给初始化调用。 不需要将 peer 添加到通道实例。

```javascript
const channel = client.newChannel('mychannel');
const peer = client.newPeer(....);
await channel.initialize({discover:true, target:peer});
```

使用服务发现对通道进行初始化并将 peer 和 orderer 添加到该通道后，具有用于服务发现的地址的 peer 将很可能位于已发现 peer 的列表中。 具有用于服务发现的地址的 peer 实例将被添加到与用于服务发现的 peer 实例相同的地址的通道中，因为在初始化调用中使用的 peer 实例没有被添加到通道中，而是仅用于 初始化调用。

如果应用程序选择不再使用，请在 initialize 调用或自动分配的 peer 上使用 peer，再次调用 channel.initialize()并提供 peer 实例或名称。 以后将使用此新 peer 进行服务发现调用。

#### 使用连接配置文件

使用连接配置文件时，将不再需要提供网络上的所有 peer 和 orderer。 仅需要一个同级并分配给该通道。 peer 必须具有发现角色。 与所有角色一样，如果未定义角色并将其设置为 false，则默认情况下，peer 将在通道上具有该角色。

以下示例显示了将主要用于服务发现的 peer。

```yaml
channels:
  mychannel:
    peers:
      peer1.org2.example.com:
        endorsingPeer: false
        chaincodeQuery: false
        ledgerQuery: true
        eventSource: false
        discover: true
    peer2.org2.example.com:
      endorsingPeer: true
      chaincodeQuery: true
      ledgerQuery: true
      eventSource: false
      discover: false

peers:
  peer1.org2.example.com:
    url: grpcs://localhost:8051
    grpcOptions:
      ssl-target-name-override: peer1.org2.example.com
    tlsCACerts:
      path: test/fixtures/channel/c...
  peer2.org2.example.com:
    url: grpcs://localhost:8052
    grpcOptions:
      ssl-target-name-override: peer2.org2.example.com
    tlsCACerts:
      path: test/fixtures/channel/c...
```

在客户端实例加载连接配置文件后使用 client.getChannel()创建通道时，Fabric 客户端将创建 peer 并将其分配给该通道。 peer 和 orderer 将固有地分配给客户端实例的连接选项。 请参见 Client#addConnectionOptions。在调用 channel.initialize()且未将任何 peer 作为目标传递时，将使用具有 Discover 角色的 peer。

```javascript
const client = Client.loadFromConfig(...);
const channel = client.getChannel('mychannel');
await channel.initialize({discover:true, asLocalhost:true};
```

如果初始化由于具有 discover 角色的 peer 不在线而失败，则应用程序可能会选择另一个 peer。

```javascript
await channel.initialize({ discover: true, target: "peer2.org2.example.com" });
```

当应用程序将 peer 名称或 peer 实例传递给初始化调用时，将使用该 peer，并且不会检查 discover 角色。 peer 将被知道的名称是 yaml 文件中使用的名称。

```yaml
peer1.org2.example.com:port
```

peer 的名称将是服务发现添加的 peer 的主机名和端口。

```yaml
peer0.org1.example.com:7051
```

### 背书

如上所述，channel.sendTransactionProposal()现在将使用可插入处理程序。 Fabric 客户端将附带使用服务发现的处理程序。 默认情况下，背书处理程序配置设置将指向 DiscoveryEndorsementHandler。 如果通道已使用服务发现进行了初始化，并且在 sendTransactionProposal 调用上未定义任何目标，则处理程序将根据提议请求的链码使用服务发现结果来确定要执行背书的目标 peer。

```javascript
const tx_id = client.newTransactionID();
const request = {
  chaincodeId: "example",
  fcn: "move",
  args: ["a", "b", "100"],
  txId: tx_id
};
await channel.sendTransactionProposal(request);
```

如果背书将需要一个或多个链式代码来进行链式调用和/或超过一个或两个集合，则背书提议请求必须包含参数"endorsement_hint"。 这将帮助发现服务根据所涉及的链码和集合的所有背书策略以及网络上的活动 peer 制定背书计划。 以下示例显示了一个链代码以对集合进行链代码调用。 请注意，开始签注的链码也必须仍然包含在签注请求的"chaincodeId"中。

```javascript
const hint = {
  chaincodes: [
    {
      name: "my_chaincode1",
      collection_names: ["my_collection1", "my_collection2"]
    },
    {
      name: "my_chaincode2",
      collection_names: ["my_collection1", "my_collection2"]
    }
  ]
};

const tx_id = client.newTransactionID();
const request = {
  chaincodeId: "my_chaincode1",
  fcn: "move",
  args: ["a", "b", "100"],
  txId: tx_id,
  endorsement_hint: hint
};
await channel.sendTransactionProposal(request);
```

该应用程序可以使特定的 peer 或指定组织中的 peer 先于其他 peer 被选中，或者根本不被选中进行背书。

- required : 代表 peer 名称的字符串数组。在此列表和背书计划中命名的 peer 将是唯一收到背书请求的 peer。背书计划中发现的其他 peer 将不会使用。

- preferred : 一个字符串数组，表示如果分类帐的分类账高度是最新的，则应由背书处理程序给予优先级的同级的名称。

- ignored : 一个字符串数组，表示背书处理程序应忽略的 peer 的名称。

- requiredOrgs : 代表组织名称的字符串数组。这些组织中背书计划中发现的 peer 将是唯一收到背书请求的 peer。背书计划中发现的其他 peer 将不会使用。

- preferredOrgs : 代表组织的 MSP ID 的字符串数组，如果分类帐的高度是最新的，则应由背书处理程序给予优先级。

- ignoredOrgs : 字符串数组，表示背书处理程序应忽略的组织的 MSP ID。

- preferredHeightGap : 一个整数，表示 peer 的分类帐高度和一组 peer 中找到的最高分类帐高度的最大差。如果 peer 的分类帐高度在此范围内，则将对其给予优先级。没有默认值，如果未提供此值，则在将 peer 添加到首选列表时将不考虑 peer 的分类帐高度。

- sort : 一个字符串值，指示应如何选择组中的 peer。有两种可用的:

  - "ledgerHeight" : 按通道分类账上的块数降序排列 peer。

  - "random" : 从列表中 peer 进行随机排序，preferred peer 将首先被随机添加，然后再随机添加其它 peer。

    默认值是按分类帐高度排序。

例如，当处理程序收到以下请求并具有背书计划，"peer3"分类帐高度为 2000，"peer1"分类帐高度为 1990。请注意，该间隙为 10，则该间隙太大，"peer1"将不会为给予优先权。

```javascript
const request = {
  chaincodeId: "example",
  fcn: "move",
  args: ["a", "b", "100"],
  txId: tx_id,
  preferred: ["peer0", "peer1.org1.example.com:8051"],
  preferredHeightGap: 5,
  ignored: ["peer1", "peer2.org2.example.com:8054"]
};
```

例如，当处理程序收到以下请求并具有背书计划时，"Org3MSP"分类帐高度为 2000 的"peer3"，而"Org1MSP"分类帐高度为 1990 的"peer1"。请注意，两者之差为 10，这个距离也是如此大，"peer1"将不会被赋予优先级。

```javascript
const request = {
  chaincodeId: "example",
  fcn: "move",
  args: ["a", "b", "100"],
  txId: tx_id,
  requiredOrgs: ["Org2MSP", "Org3MSP"]
};
```

### To Commit

如上所述，channel.sendTransaction()现在将使用可插入处理程序。 Fabric 客户端将附带一个处理程序，该处理程序将使用添加到通道的所有 orderer 程序。 默认情况下，提交处理程序配置设置将指向 BasicCommitHandler。 此处理程序将一次向每个已分配给该通道的 orderer 发送一次事务，直到获得成功响应或列表用完为止。 由于服务发现初始化或两者的结合，可以手动添加 orderer。 如果在呼叫中指定了 orderer，则仅使用该 orderer。

---
