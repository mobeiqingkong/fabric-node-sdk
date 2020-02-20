# fabric-common：如何设置 gRPC 设置(How to set gRPC settings)

## fabric-common：如何设置 gRPC 设置(How to set gRPC settings)

本教程说明了从 1.4 版本开始，通过 Hyperledger Fabric Node.js 客户端设置用于连接 Hyperledger Fabric 网络的 gRPC 设置的不同方法。

有关更多信息：

- Hyperledger Fabric 入门，请参阅[构建第一个网络(Building your first network)](http://hyperledger-fabric.readthedocs.io/en/latest/build_network.html)。

以下内容假定您对 Hyperledger Fabric 网络（Orderer 和 Peer）以及 Node 应用程序开发有所了解。

### 概述

Hyperledger Fabric Node.js javascript SDK(fabric-client)使用 gRPC 与 Hyperledger Fabric 网络通信。 gRPC 技术框架可可靠地处理 fabric 网络与 fabric 客户端应用程序之间的数据移动。 fabric-client 允许应用程序提供控制环境所需的设置。

fabric-client 具有包括默认 gRPC 设置的默认连接选项。 应用程序可以通过多种方式来覆盖默认的连接选项。

### 默认连接选项(Default connection options)

fabric-client 默认具有以下 gRPC 连接选项。这些位于 fabric-client NPM 软件包随附的 default.json 系统配置文件中。

```json
    "connection-options": {
        "grpc.max_receive_message_length": -1,
        "grpc.max_send_message_length": -1,
        "grpc.keepalive_time_ms": 120000,
        "grpc.http2.min_time_between_pings_ms": 120000,
        "grpc.keepalive_timeout_ms": 20000,
        "grpc.http2.max_pings_without_data": 0,
        "grpc.keepalive_permit_without_calls": 1
    }
```

- grpc.max_receive_message_length - 通道可以接收的最大消息长度。整数值，以字节为单位。 -1 表示无限。
- grpc.max_send_message_length - 通道可以发送的最大消息长度。整数值，以字节为单位。 -1 表示无限。
- grpc.keepalive_time_ms - 在这段时间之后，客户端/服务器对它的对等端执行 ping 操作，以查看传输是否仍然有效。整数(以毫秒为单位)。
- grpc.keepalive_timeout_ms - 等待此时间后，如果 keepalive ping 发件人未收到 ping ack，它将关闭传输。整数(以毫秒为单位)。
- grpc.keepalive_permit_without_calls - 是否允许在没有任何未完成流的情况下发送 keepalive ping？整数值，0(false)/1(true)。
- grpc.http2.min_time_between_pings_ms - 发送连续的 ping 帧而不接收任何数据帧之间的最短时间。整数(以毫秒为单位)。
- grpc.http2.max_pings_without_data - 服务器接收连续的 ping 帧而不发送任何数据帧之间的最小允许时间。整数(以毫秒为单位)。

### 更改默认连接选项(Change default connection options)

该应用程序可能需要更改或添加新的 gRPC 设置。 通过使用系统配置，应用程序可以更改用于所有已建立的新连接的默认连接选项。

当[Client](https://hyperledger.github.io/fabric-sdk-node/release-1.4/Client.html) 实例构建新的[Peer](https://hyperledger.github.io/fabric-sdk-node/release-1.4/Peer.html)s 或新的[Orderer](https://hyperledger.github.io/fabric-sdk-node/release-1.4/Orderer.html)s 时，将默认的连接选项作为一组选项进行检索。 要在运行时之前修改默认连接选项，请更新 default.json 文件或将您自己的配置文件添加到系统配置中。 最后加载的文件将覆盖所有先前的文件，包括 fabric-client 随附的 default.json 文件。 请参见 [BaseClient.addConfigFile](https://hyperledger.github.io/fabric-sdk-node/release-1.4/BaseClient.html#.addConfigFile)。

```javascript
const Client = require('fabric-client');
Client.addConfigFile(<path to the config file>);
```

要在运行时修改默认连接选项，请从系统配置中获取它们，进行修改，然后将其重新设置为系统配置。

```javascript
const default_options = client.getConfigSetting("connection-options");
const new_option = {
  "grpc.keepalive_timeout_ms": 10000
};

// use the assign call to keep all other options and only update
//使用分配调用保留所有其他选项，仅更新
// the one setting or add a setting.
//一个设置或添加一个设置。
const new_defaults = Object.assign(default_options, new_option);
client.setConfigSetting("connection-options", new_defaults);

// peer will have default options
// peer 将具有默认选项
const peer = client.newPeer(url, options);
```

注意：更改系统配置将使所有新连接使用新的默认连接选项。 这包括使用发现服务时由新 Peer 和新 Orderer 创建的连接。 分配新值时要小心，不要删除其他值。 所有默认连接选项都包含在一个系统配置设置连接选项中。

### 将连接选项添加到客户端(Add connection options to the client)

该应用程序可能需要更改或添加新的 gRPC 设置。 应用程序可以向客户端实例添加连接选项，这些连接选项可以是新选项，也可以覆盖存储在系统配置中的现有默认连接选项。 请参阅上面有关如何在系统配置中更改选项的讨论。

```javascript
const new_options = {
  "grpc.keepalive_timeout_ms": 10000
};
client.addConnectionOptions(new_options);

// peer will have options from default and from client
// Peer将具有来自默认和客户端的选项
const peer = client.newPeer(url, options);

// discovered peers will have options from default and from client
// 发现的Peer将具有默认值和客户端的选项
channel.initialize({ discover: true, target: peer });
```

注意：此客户端创建的所有新连接，包括使用发现服务时自动创建的新连接，都将使用客户端的连接选项来覆盖默认连接选项。

### 在创建时添加连接选项(Add connection options on create)

该应用程序可能需要单个 Peer 或 Orderer 的唯一连接选项。 唯一设置可以在 option 参数中的 [Client#newPeer](https://hyperledger.github.io/fabric-sdk-node/release-1.4/Client.html#newPeer) 或 [Client#newOrderer](https://hyperledger.github.io/fabric-sdk-node/release-1.4/Client.html#newOrderer) 调用上传递。 调用中传递的选项将覆盖基于客户端的选项和基于系统配置的选项。

```javascript
const options = {
  pem: "<pem string>",
  "ssl-target-name-override": "myhost.org1.com",
  "grpc.keepalive_timeout_ms": 10000
};

// peer will have options from default, client, and parameter
// Peer将具有来自默认，客户端和参数的选项
const peer = client.newPeer(url, options);
```

注意：在 newPeer()和 newOrderer()调用中传递的连接选项将仅用于该 Peer 或 Orderer。

### 将连接选项添加到通用连接配置文件(Add connection options to a common connection profile)

使用公共连接配置文件时，可以在客户端级别设置连接选项，也可以在 Peer 和 Orderer 上分别设置连接选项。在以下配置文件中，客户端部分和一个 Peer 均具有 gRPC 设置。

```yaml
client:
  # Which organization does this application instance belong to? The value is the name of an org
  # 该应用程序实例属于哪个组织？ 该值是组织的名称
  # defined under "organizations"
  # "organizations"下的定义
  organization: Org1

  # set connection timeouts for the peer and orderer for the client
  # 设置客户端的Peer和Orderer的连接超时
  connection:
    timeout:
      peer:
        # the timeout in seconds to be used on requests to a peer,
        # 在Peer的请求中使用的超时（以秒为单位），
        # for example 'sendTransactionProposal'
        # 例如'sendTransactionProposal'
        endorser: 120
        # the timeout in seconds to be used by applications when waiting for an
        # event to occur. This time should be used in a javascript timer object
        # that will cancel the event registration with the channel event hub instance.
        # 等待事件发生时应用程序将使用的超时（以秒为单位）。
        # 此时间应在javascript计时器对象中使用，该对象将取消与通道事件中心实例的事件注册。
        eventHub: 60
        # the timeout in seconds to be used when setting up the connection
        # with the peer event hub. If the peer does not acknowledge the
        # connection within the time, the application will be notified over the
        # error callback if provided.
        # 与Peer事件中心建立连接时要使用的超时（以秒为单位）。
        # 如果Peer在此时间内未确认连接，则将通过错误回调（如果提供）通知应用程序。
        eventReg: 3
      # the timeout in seconds to be used on request to the orderer,
      # 要求Orderer使用的超时（以秒为单位），
      # for example
      # 示例
      orderer: 30
      # connection options, typically these will be common GRPC settings,
      # 连接选项，通常是通用的GRPC设置，
      # overriding what has been set in the system config file "default.json"
      # 覆盖已在系统配置文件“ default.json”中设置的内容
    options:
      grpc.keepalive_timeout_ms: 10000
peers:
  peer1.org2.example.com:
    url: grpcs://localhost:8051
    grpcOptions:
      ssl-target-name-override: peer1.org2.example.com
      grpc.keepalive_timeout_ms: 20000
    tlsCACerts:
      path: test/fixtures/channel/c...
  peer2.org2.example.com:
    url: grpcs://localhost:8052
    grpcOptions:
      ssl-target-name-override: peer2.org2.example.com
    tlsCACerts:
      path: test/fixtures/channel/c...
```

注意：此客户端创建的所有新连接，包括使用发现服务时自动创建的新连接，都将使用客户端的连接选项来覆盖默认连接选项。 peer1 将使用其自己的唯一值覆盖一个设置。 peer2 不会覆盖任何客户端默认值或系统默认值。 应用程序可以调用 client.addConnectionOptions()以添加其他设置或替代设置。 在添加调用之后，通过调用 [Client#getPeer](https://hyperledger.github.io/fabric-sdk-node/release-1.4/Client.html#getPeer) 或通过调用 [Client#getOrderer](https://hyperledger.github.io/fabric-sdk-node/release-1.4/Client.html#getOrderer) 或通过调用[Client#getChannel](https://hyperledger.github.io/fabric-sdk-node/release-1.4/Client.html#getChannel) 创建的 Peer 将使用新的值集。

---
