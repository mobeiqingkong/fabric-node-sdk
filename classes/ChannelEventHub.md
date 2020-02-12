# ChannelEventHub

## ChannelEventHub

#### new ChannelEventHub(channel, peer)

构造一个ChannelEventHub对象

- 参数

| 名称    | 类型                                                         | 描述                                           |
| :------ | :----------------------------------------------------------- | ---------------------------------------------- |
| channel | [Channel](https://hyperledger.github.io/fabric-sdk-node/release-1.4/Channel.html) | Channel类的一个实例，该ChannelEventHub将接收块 |
| peer    | [Peer](https://hyperledger.github.io/fabric-sdk-node/release-1.4/Peer.html) | 可选。 ChannelEventHub连接的Peer类的实例。     |

返回结果

- 这个类的实例。

  - 类型

    [ChannelEventHub](https://hyperledger.github.io/fabric-sdk-node/release-1.4/ChannelEventHub.html)

### Methods

#### checkConnection(force_reconnect)

- 参数

| 名称            | 类型    | 描述                                     |
| :-------------- | :------ | ---------------------------------------- |
| force_reconnect | boolean | 如果流不处于“ READY”状态，则尝试重新连接 |

#### close()

将ChannelEventHub与光纤对等服务断开连接。将关闭所有事件侦听器，并向提供“ onError”回调的所有侦听器发送带有消息“ ChannelEventHub已关闭”的EventHubDisconnectError对象。

将ChannelEventHub与fabric peer服务断开连接。将关闭所有事件侦听器，并向提供“ onError”回调的所有侦听器发送带有消息“ ChannelEventHub已关闭”的EventHubDisconnectError对象。



#### connect(options, connectCallback)

与fabric peer服务建立连接。连接将异步建立。如果无法建立连接，则将通过提供的“ connectCallback”通知应用程序。另外，如果提供了来自registerXXXEvent()方法的错误回调，则将得到通知。对于运行时连接问题，建议在“ connectCallback”上中继应用程序以确定连接状态，并在事件侦听器的“ errCallback”上中继。在调用connect()之后，通过调用[registerBlockEvent](https://hyperledger.github.io/fabric-sdk-node/release-1.4/ChannelEventHub.html#registerBlockEvent), [registerTxEvent](https://hyperledger.github.io/fabric-sdk-node/release-1.4/ChannelEventHub.html#registerTxEvent) 或 [registerChaincodeEvent](https://hyperledger.github.io/fabric-sdk-node/release-1.4/ChannelEventHub.html#registerChaincodeEvent)方法中的任何一种来注册事件侦听器和错误回调。

- 参数

| 名称            | 类型                                                         | 描述                                                         |
| :-------------- | :----------------------------------------------------------- | ------------------------------------------------------------ |
| options         | [ConnectOptions](https://hyperledger.github.io/fabric-sdk-node/release-1.4/global.html#ConnectOptions)&#124;boolean | 可选。如果类型为布尔值，则将假定如何连接以接收完整(true)或已过滤(false)块。 |
| connectCallback | function                                                     | 可选。该回调将报告与peer的连接完成，或报告与对等方连接期间遇到的任何错误。发生错误时，此ChannelEventHub将关闭(disconnected)。回调函数应采用两个参数作为(error, value)。 |

#### disconnect()

断开ChannelEventHub与peer事件源的连接。将关闭所有事件侦听器，并向提供"onError"回调的所有侦听器发送带有消息"ChannelEventHub已关闭"的EventHubDisconnectError对象。

#### generateUnsignedRegistration(options)

生成未签名的fabric服务注册，该注册应由身份的私钥签名。

- 参数

| 名称    | 类型                                                         | 描述                                                         |
| :------ | :----------------------------------------------------------- | ------------------------------------------------------------ |
| options | [EventHubRegistrationRequest](https://hyperledger.github.io/fabric-sdk-node/release-1.4/global.html#EventHubRegistrationRequest) | 向fabric peer服务注册此ChannelEventHub的选项。请注意，这些选项应同时包含标识和txId或证书和mspId |

返回结果

- 字节数组包含要签名的注册有效负载。

  - 类型

    Buffer

#### getName()

返回此ChannelEventHub的名称，将是关联peer的名称。

#### getPeerAddr()

返回peer的url

#### isconnected()

此ChannelEventHub是否已连接到fabric服务？

返回结果

- 如果连接到事件源，则为true，否则为false。


#### reconnect(options, connectCallback)

重新建立与fabric peer服务的连接。

- 参数

| 名称            | 类型                                                         | 描述                                                         |
| :-------------- | :----------------------------------------------------------- | ------------------------------------------------------------ |
| options         | [ConnectOptions](https://hyperledger.github.io/fabric-sdk-node/release-1.4/global.html#ConnectOptions) | 可选。                                                       |
| connectCallback | function                                                     | 可选。该回调将报告与peer的连接完成，或报告与peer连接期间遇到的任何错误。发生错误时，此ChannelEventHub将关闭(disconnected)。回调函数应采用两个参数作为(error, value)。 |

#### registerBlockEvent(onEvent, onError, options)

注册一个侦听器以接收提交给该通道的所有块。在每个块到达时，将调用侦听器的“ onEvent”回调。

异步运行的连接建立中可能会发生错误。最佳实践是在此ChannelEventHub出现问题时提供“ onError”回调以通知。

- 参数

| 名称    | 类型                                                         | 描述                                                         |
| :------ | :----------------------------------------------------------- | ------------------------------------------------------------ |
| onEvent | function                                                     | 带有[Block](https://hyperledger.github.io/fabric-sdk-node/release-1.4/global.html#Block)对象的单个参数的回调函数 |
| onError | function                                                     | 关闭此ChannelEventHub时将通知的可选回调函数。关闭原因可能是网络连接错误，"disconnect()"方法的调用，或者fabric服务在此ChannelEventHub结束连接引起的。由于重播并请求endBlock为"newest"，则由于接收到最后一个块而关闭ChannelEventHub时，也会调用此回调。 |
| options | [RegistrationOpts](https://hyperledger.github.io/fabric-sdk-node/release-1.4/global.html#RegistrationOpts) | 注册选项允许开始和结束程序段号，自动注销并自动断开连接。     |

返回结果

- 这是必须用来注销此块侦听器的块注册号。参见[unregisterBlockEvent()](https://hyperledger.github.io/fabric-sdk-node/release-1.4/ChannelEventHub.html#unregisterBlockEvent)。

  - 类型

    int

#### registerChaincodeEvent(chaincode_id, event_name, onEvent, onError, options)

注册一个侦听器以接收链码事件。

异步运行的连接建立中可能会发生错误。最佳实践是在此ChannelEventHub出现问题时提供"onError"回调以通知。

- 参数

| 名称         | 类型                                                         | 描述                                                         |
| :----------- | :----------------------------------------------------------- | ------------------------------------------------------------ |
| chaincode_id | string                                                       | 链码的ID Id of the chaincode of interest                     |
| event_name   | string&#124;RegExp                                           | 链码事件或正则表达式的确切名称，它将与目标链码的调用stub.SetEvent(name, payload)的名称相匹配 |
| onEvent      | function                                                     | 匹配事件的回调函数。不使用"as_array"时将使用四个参数调用它。<br/>- ChaincodeEvent-链码产生的链码事件，<br/>- {Long}-包含此链码事件的块号<br/>- {string}-包含此chaincode事件的交易ID<br/>- {string}-包含此chaincode事件的交易的交易状态<br/>当使用"as_array: true"选项时，事件对象数组的一个参数将具有上述值，可以在以下示例中使用。 |
| onError      | function                                                     | 关闭此ChannelEventHub时将通知的可选回调函数。关闭原因可能是网络连接错误，"disconnect()"方法的调用，或者fabric服务在此ChannelEventHub结束连接引起的。由于重播并请求endBlock为"newest"，则由于接收到最后一个块而关闭ChannelEventHub时，也会调用此回调。 |
| options      | [RegistrationOpts](https://hyperledger.github.io/fabric-sdk-node/release-1.4/global.html#RegistrationOpts) | 注册选项允许开始和结束程序段号，自动注销并自动断开连接。链码事件侦听器也可以使用“ as_array”选项来表明，找到的所有与此定义匹配的链码事件都将作为数组发送到回调，或者分别为每个回调调用回调。 |

返回结果

- 应该被视为用于注销的不透明句柄的对象（请参见 [unregisterChaincodeEvent()](https://hyperledger.github.io/fabric-sdk-node/release-1.4/ChannelEventHub.html#unregisterChaincodeEvent)）。

  - 类型

    Object

- 示例

```
function myCallback(...events) {
          for ({chaincode_event, block_num, tx_id, tx_status} of events) {
              // 处理链码事件
          }
       }
```

#### registerTxEvent(txid, onEvent, onError, options)

注册一个回调函数，以在给定ID的事务已提交到一个块中时接收通知。使用特殊字符串"all"将指示此侦听器将通知 (call) 从fabric服务接收到的每个事务的回调。

异步运行的连接建立中可能会发生错误。最佳实践是在此ChannelEventHub出现问题时提供" onError"回调以通知。

- 参数

| 名称    | 类型                                                         | 描述                                                         |
| :------ | :----------------------------------------------------------- | ------------------------------------------------------------ |
| txid    | string                                                       | 交易ID字符串或"all"                                          |
| onEvent | function                                                     | 带有事务ID参数，指示事务状态的字符串参数以及已提交到分类帐的该事务的块号的回调函数 |
| onError | function                                                     | 关闭此ChannelEventHub时将通知的可选回调函数。关闭原因可能是网络连接错误，"disconnect()"方法的调用，或者fabric服务在此ChannelEventHub结束连接引起的。由于重播并请求endBlock为"newest"，则由于接收到最后一个块而关闭ChannelEventHub时，也会调用此回调。 |
| options | [RegistrationOpts](https://hyperledger.github.io/fabric-sdk-node/release-1.4/global.html#RegistrationOpts) | 注册选项允许开始和结束程序段号，自动注销并自动断开连接。     |

返回结果

- 用于注册此事件侦听器的事务ID。可以用于注销此事件侦听器。

  - 类型

    string

#### unregisterBlockEvent(block_registration_number, throwError)

使用通过调用registerBlockEvent() 方法返回的块注册号注销块事件侦听器。

- 参数

| 名称                      | 类型    | 描述                                                 |
| :------------------------ | :------ | ---------------------------------------------------- |
| block_registration_number | int     | 注册期间返回的块注册号。                             |
| throwError                | boolean | 可选。如果不存在块注册，则抛出错误，默认不抛出错误。 |

#### unregisterChaincodeEvent(listener_handle, throwError)

注销由registerChaincodeEvent() 方法返回的listener_handle对象表示的链码事件侦听器。

- 参数

| 名称            | 类型    | 描述                                                 |
| :-------------- | :------ | ---------------------------------------------------- |
| listener_handle | Object  | 从对registerChaincodeEvent的调用返回的句柄对象。     |
| throwError      | boolean | 可选。如果不存在块注册，则抛出错误，默认不抛出错误。 |

#### unregisterBlockEvent(block_registration_number, throwError)

注销事务事件侦听器以获取事务ID。

- 参数

| 名称       | 类型    | 描述                                                 |
| :--------- | :------ | ---------------------------------------------------- |
| txid       | string  | 交易编号。                                           |
| throwError | boolean | 可选。如果不存在块注册，则抛出错误，默认不抛出错误。 |