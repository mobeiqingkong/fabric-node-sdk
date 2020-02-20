# fabric-client:如何创建 Hyperledger Fabric 通道(How to create a Hyperledger Fabric channel)

## fabric-client:如何创建 Hyperledger Fabric 通道(How to create a Hyperledger Fabric channel)

本教程说明了如何使用 Node.js fabric-client SDK 创建 Hyperledger Fabric 通道。 它显示了如何使用初始(默认)通道定义以及如何从该定义开始构建自定义的定义。 创建网络和通道的过程还涉及创建和使用加密材料(cryptographic material)，这里将不再讨论。

有关更多信息:

- Hyperledger Fabric 入门，请参阅[构建你的第一个网络(Building your first network)](http://hyperledger-fabric.readthedocs.io/en/latest/build_network.html)。
- Hyperledger Fabric 中通道的配置以及创建和更新的内部过程，请参见[Hyperledger Fabric 通道配置(Hyperledger Fabric channel configuration)](http://hyperledger-fabric.readthedocs.io/en/latest/configtx.html)
- 密码生成请参阅 [cryptogen](http://hyperledger-fabric.readthedocs.io/en/latest/build_network.html#crypto-generator)
- 配置事务生成器，请参见 [configtxgen](http://hyperledger-fabric.readthedocs.io/en/latest/build_network.html#configuration-transaction-generator)
- 配置转换工具，请参阅 [configtxlator](https://github.com/hyperledger/fabric/tree/master/examples/configtxupdate)

以下内容假定您对 Hyperledger Fabric 网络(orderer 和 peer)，[protobuf](https://developers.google.com/protocol-buffers/)以及 Node 应用程序开发(包括使用 Javascript Promise)有所了解。

下面显示的示例基于余额转移示例应用程序。 参见[Hyperledger Fabric 示例(Hyperledger Fabric Samples)](https://github.com/hyperledger/fabric-samples/tree/master/balance-transfer)

### 通道创建的步骤(steps of a channel create)

- 运行 configtxgen 工具生成创世区块
- 运行 configtxgen 工具生成初始二进制配置定义
- 通过以下两种方法之一获得可签名的通道定义

  1. 使用初始二进制通道配置定义

     - 使用 fabric-client SDK 从初始二进制通道配置定义中提取可签名通道定义

  2. 建立自定义的定义

     - 使用 configtxlator 将初始二进制通道配置定义转换为可读文本

     - 编辑可读文本[更多信息(more info)](http://hyperledger-fabric.readthedocs.io/en/latest/configtx.html)

     - 使用 configtxlator 将编辑的文本转换为可签名的通道定义

- 使用 Fabric-Client SDK 签名可签名通道定义
- 使用 Fabric-client SDK 将签名和可签名的通道定义发送给 orderer
- 使用 Fabric-client SDK 使 Peer 加入通道
- 然后可以使用新通道

## 使用初始定义来构建可签名的通道定义(Use the initial definition to build a sign-able channel definition)

[configtxgen 工具(configtxgen tool)](http://hyperledger-fabric.readthedocs.io/en/latest/configtx.html) 生成的初始二进制通道配置定义是一个二进制文件，其中包含 Hyperledger Fabric 配置 protobuf common.Envelope 元素。 该元素内部是 common.ConfigUpdate protobuf 元素。 此配置元素是必须签名的元素。 configtx.yaml 中的配置文件元素是由 configtxgen 工具创建的初始二进制通道配置定义的源。

```shell
../../../bin/configtxgen -channelID mychannel -outputCreateChannelTx mychannel.tx -profile TwoOrgsChannel
```

让 Fabric-Client SDK 从 mychannel.tx 文件中提取配置更新元素

```javascript
// first read in the file, this gives us a binary config envelope
// 首先读取文件，这为我们提供了一个二进制配置信封
let envelope_bytes = fs.readFileSync(
  path.join(
    __dirname,
    "fabric-samples/balance-transfer/artifacts/channel/mychannel.tx"
  )
);
// have the nodeSDK extract out the config update
// 让nodeSDK提取配置更新
var config_update = client.extractChannelConfig(envelope_bytes);
```

二进制 config_update 现在可以在签名过程中使用，并发送给 orderer 以创建通道。

您可能会问为什么将 common.ConfigUpdate 用于创建。 这使得创建和更新的过程相同。 新通道的创建是系统通道中定义的增量，更新是通道中定义的增量。 提交的 common.ConfigUpdate 对象将仅包含创建和更新的更改。

## 创建自定义的可签名通道定义(Creating a custom sign-able channel definition)

转换一个已经存在或可以用来创建新通道的现有二进制文件，以用来读取 JSON。该配置有许多要素，一开始很难做到。使用用于生成 Hyperledger Fabric 网络的相同 configtx.yaml 文件，使用 configtxgen 工具为新通道创建初始二进制配置定义。然后，通过将该二进制文件发送到 configtxlator 将其转换为 JSON，您将能够看到布局(layout )并有一个起点。该 JSON 也可以用作在网络上创建其他新通道的模板。对于新通道配置中未定义的设置，新通道将从系统通道继承设置。必须在系统通道上的联盟中定义将在新通道上使用的组织。因此，在创建新通道时，对网络的系统通道具有可读的定义将很有帮助。将用于启动 Hyperledger Fabric 网络的 genesis.block 发送到 configtxlator，以获取用作参考的 JSON 文件。

使用 configtxgen 工具生成二进制配置文件。从样本目录 fabric-samples/balance-transfer/artifacts/channel

```shell
export FABRIC_CFG_PATH=$PWD
../../../bin/configtxgen -outputBlock genesis.block -profile TwoOrgsOrdererGenesis
../../../bin/configtxgen -channelID mychannel -outputCreateChannelTx mychannel.tx -profile TwoOrgsChannel
```

该路径还必须包含二进制对象的类型，在第一种情况下，它是 common.Block。 可以对 fabric-client\lib\protos 目录 protobuf 文件中找到的任何 protobuf message 对象类型进行“解码(decode)”或“编码(encode)”。 首先从 fabric-samples/bin 目录启动 configtxlator 服务

```shell
./configtxlator start
```

然后

```shell
curl -X POST --data-binary @genesis.block http://127.0.0.1:7059/protolator/decode/common.Block > genesis.json
curl -X POST --data-binary @mychannel.tx http://127.0.0.1:7059/protolator/decode/common.Envelope > mychannel.json
```

解码文件 mychannel.tx 的结果是 configtxgen 工具生成的 common.Envelope 包含 common.ConfigUpdate 对象。 该对象在"payload.data" JSON 对象中的名称为"config_update"。 这是用作创建新通道的模板源所需要的对象。 common.ConfigUpdate 是将由所有组织签名并提交给 orderer 以创建新通道的对象。

以下是从上面生成的"TwoOrgsChannel"通道创建二进制文件的解码中提取的 JSON "config_update"(common.ConfigUpdate)对象。

```json
{
  "channel_id": "mychannel",
  "read_set": {
    "groups": {
      "Application": {
        "groups": {
          "Org1MSP": {}
        }
      }
    },
    "values": {
      "Consortium": {
        "value": {
          "name": "SampleConsortium"
        }
      }
    }
  },
  "write_set": {
    "groups": {
      "Application": {
        "groups": {
          "Org1MSP": {}
        },
        "mod_policy": "Admins",
        "policies": {
          "Admins": {
            "policy": {
              "type": 3,
              "value": {
                "rule": "MAJORITY",
                "sub_policy": "Admins"
              }
            }
          },
          "Readers": {
            "policy": {
              "type": 3,
              "value": {
                "sub_policy": "Readers"
              }
            }
          },
          "Writers": {
            "policy": {
              "type": 3,
              "value": {
                "sub_policy": "Writers"
              }
            }
          }
        },
        "version": "1"
      }
    },
    "values": {
      "Consortium": {
        "value": {
          "name": "SampleConsortium"
        }
      }
    }
  }
}
```

请注意，所使用的联盟(Consortium)名称必须存在于系统通道上。您要添加到新通道的所有组织都必须在"Consortium"部分下的系统通道上用该名称定义。使用已解码的创世区块来验证所有值，例如，通过查看上面生成的 genesis.json 文件。要将组织添加到通道，必须将它们放置在"Applications"部分下的"groups"部分下，如上所示。看到 Org1MSP 是 Applications.groups 部分的属性。在此示例中，组织 Org1MSP 的所有设置都将从系统通道继承(请注意该组织属性的空对象"{}")。要查看此组织的当前设置，请在系统通道的"Consortium"部分下的"SampleConsortium"部分中查看(系统通道的创世区块)。

一旦具有代表您的通道的 JSON 配置，请将其发送给 configtxlator，以将其编码为配置二进制文件。以下示例将 REST 请求发送到 configtxlator 的示例使用 Node.js 包超级代理，因为它易于使用 HTTP 请求。

```javascript
var response = superagent
  .post(
    "http://127.0.0.1:7059/protolator/encode/common.ConfigUpdate",
    config_json.toString()
  )
  .buffer()
  .end((err, res) => {
    if (err) {
      logger.error(err);
      return;
    }
    config_proto = res.body;
  });
```

## 签署并提交通道更新(Signing and submitting the channel update)

二进制配置必须由所有组织签名。 该应用程序将必须存储二进制配置，并使其可用于签名，并在收集签名时存储所有签名。 然后，签名完成后，应用程序将使用 fabric-client SDK API createChannel()将二进制配置和所有签名发送给 orderer。

首先进行签名，假设客户端 fabric-client SDK 对象在所需组织中具有有效用户

```javascript
var signature = client.signChannelConfig(config_proto);
signatures.push(signature);
```

现在是时候创建通道了，假设签名对象是由 client.signChannelConfig()方法返回的 common.ConfigSignature 数组。

注意:orderer 必须从与初始二进制通道配置定义相同的配置文件生成的 genesis.block 开始

```javascript
// create an orderer object to represent the orderer of the network
// 创建一个orderer对象来代表网络的orderder
var orderer = client.newOrderer(url, opts);

// have the SDK generate a transaction id
// 让SDK生成交易ID
let tx_id = client.newTransactionID();

request = {
  config: config_proto, //the binary config
  signatures: signatures, // the collected signatures
  name: "mychannel", // the channel name
  orderer: orderer, //the orderer from above
  txId: tx_id //the generated transaction id
};

// this call will return a Promise
// 做个调用将返回一个Promise
client.createChannel(request);
```

createChannel API 返回一个 Promise，以返回提交状态。通道创建将由 orderer 异步进行。

几秒钟的小延迟后，orderer 将创建该通道，Peer 现在可以将其加入。 向通道上所需的 Peer 发出以下命令。 这是一个两步过程，首先获取通道的创始区块，然后将其发送给 Peer。 在以下示例中，创世区块是从 orderer 中检索的，但也可以从文件中加载。

```javascript
// set the channel up with network endpoints
// 通过网络端点设置通道
var orderer = client.newOrderer(orderer_url, orderer_opts);
channel.addOrderer(orderer);
var peer = client.newPeer(peer_url, peer_opts);
channel.addPeer(peer);

tx_id = client.newTransactionID();
let g_request = {
  txId: tx_id
};

// get the genesis block from the orderer
// 从orderer获得创世区块
channel
  .getGenesisBlock(g_request)
  .then(block => {
    genesis_block = block;
    tx_id = client.newTransactionID();
    let j_request = {
      targets: targets,
      block: genesis_block,
      txId: tx_id
    };

    // send genesis block to the peer
    // 发送创世区块给peer
    return channel.joinChannel(j_request);
  })
  .then(results => {
    if (results && results.response && results.response.status == 200) {
      // join successful
    } else {
      // not good
    }
  });
```

---
