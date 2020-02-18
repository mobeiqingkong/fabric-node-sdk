# fabric-network.ContractEventListener

## [fabric-network](https://hyperledger.github.io/fabric-sdk-node/release-1.4/module-fabric-network.html)~ ContractEventListener

合约事件监听器处理来自链码的合约事件。

#### new ContractEventListener(contract, listenerName, eventName, eventCallback, options)

Constructor.

- 参数

| 名称          | 类型                                          | 描述                                                                              |
| ------------- | --------------------------------------------- | --------------------------------------------------------------------------------- |
| contract      | Contract                                      | 合约实例                                                                          |
| listenerName  | string                                        | 标识收听者的唯一名称                                                              |
| eventName     | string                                        | 正在监听的合约事件的名称                                                          |
| eventCallback | function                                      | 收到事件时调用事件回调。它具有签名（err，BlockEvent，blockNumber，transactionId） |
| options       | module:fabric-network.Network~ListenerOptions |                                                                                   |

### Methods

#### &lt;async&gt; \_onError(error)

事件失败时触发此回调。如果错误指示事件中心关闭且侦听器仍已注册，它将更新事件中心的 EventHubSelectionStrategy 状态（如果已实现），并找到一个新的事件中心以连接到

- 参数

| 名称  | 类型  | 描述       |
| ----- | ----- | ---------- |
| error | Error | 发出的错误 |

#### &lt;async&gt; \_onEvent(event, blockNumber, transactionId, status [, expectedNumber])

事件成功时触发的回调。一旦回调运行后，检查点将看到的最后一个块和事务；如果提供了取消注册标志，则取消对侦听器的注册

- 参数

| 名称           | 类型                                                                                                    | 描述                      |
| -------------- | ------------------------------------------------------------------------------------------------------- | ------------------------- |
| event          | [ ChaincodeEvent](https://hyperledger.github.io/fabric-sdk-node/release-1.4/global.html#ChaincodeEvent) | 发出事件                  |
| blockNumber    | number                                                                                                  | 此交易已提交的区块号      |
| transactionId  | string                                                                                                  | 发出此事件的交易的交易 ID |
| status         | string                                                                                                  | 交易状态                  |
| expectedNumber | string                                                                                                  | 可选。区块中预期的事件数  |

#### &lt;async&gt; register()

查找并连接到事件中心，然后创建侦听器注册

#### unregister()

从事件中心注销注册

---
