# fabric-network:如何重播错过的事件(How to replay missed events)

## fabric-network:如何重播错过的事件(How to replay missed events)

# 事件检查点(Event Checkpointing)

本教程描述了 Fabric 网络模块的用户可以选择的方法，用于重播 Peer 发出的丢失事件。

## 概述

提交块时，事件由 Peer 发出。 两种类型的事件都支持检查点:

1. 合约事件(也称为链码事件)-在要发出的交易中定义。 例如。 出售商业票据时发出的事件
2. 块事件(Block Events )-提交块时发出

在应用程序崩溃且事件丢失的情况下，应用程序可能仍想为其丢失的事件执行事件回调。 Fabric 网络中的 Peer 支持事件重播，为此，Fabric 网络模块支持检查点策略，该策略可跟踪客户端已看到的最后一个块和该块中的事务。

### 免责声明(Disclaimer)

当前形式的检查点尚未经过测试以处理所有恢复方案，因此应与现有恢复基础架构一起使用。 module:fabric-network\~FileSystemCheckpointer 专为技术证明项目而设计，因此我们强烈建议您使用 module:fabric-network\~BaseCheckpointer 接口实现自己的检查点。

### 笔记(Notes)

块编号=块高度-1(Block Number`=`Block Height - 1)使用检查点时:

- 仅当 startBlock 小于当前的 Block Number 时，侦听器才会赶上事件
- 如果检查点中的最新块是第 n 个块，则 startBlock 将为 n + 1(例如，对于检查点 blockNumber = 1，startBlock = 2)

## 检查点(Checkpointers)

BaseCheckpoint 类是所有 Checkpoint 类都将使用的接口。 fabric-network 具有一个默认类，模块:fabric-network\~FileSystemCheckpointer，该类作为工厂导出到 [module:fabric-network~CheckpointFactories](https://hyperledger.github.io/fabric-sdk-node/release-1.4/module-fabric-network.html#~CheckpointFactories) 中。 FILE_SYSTEM_CHECKPOINTER 是默认检查点。

检查点工厂是一个函数，该函数返回一个以 BaseCheckpointer 作为父类的实例。 这些类实现了异步(async) save(channelName, listenerName)和异步(async) load()函数。

给事件侦听器的异步回调函数完成处理后，将调用 BaseCheckpointer.save()。

### 自定义检查点(Custom Checkpointer)

配置定制检查点需要创建两个组件:

1. Checkpointer 类
2. Factory(工厂)

```javascript
const fs = require('fs-extra');
const path = require('path');
const { Gateway } = require('fabric-network');

class FileSystemCheckpointer extends BaseCheckpointer {
    constructor(channelName, listenerName, fsOptions) {
        super(channelName, listenerName);
        this.basePath = path.resolve(fsOptions.basePath);
        this.channelName = channelName;
        this.listenerName = listenerName;
    }

    /**
     * Initializes the checkpointer directory structure
     * 初始化checkpointer目录结构
     */
    async _initialize() {
        const cpPath = this._getCheckpointFileName()
    }

    /**
     * Constructs the checkpoint files name
     * 构造checkpoint文件名
     */
    _getCheckpointFileName() {
        let filePath = path.join(this._basePath, this._channelName);
        if (this._chaincodeId) {
            filePath = path.join(filePath, this._chaincodeId);
        }
        return path.join(filePath, this._listenerName);
    }

    async save(transactionId, blockNumber) {
        const cpPath = this._getCheckpointFileName()
        if (!(await fs.exists(cpPath))) {
            await this._initialize();
        }
        const latestCheckpoint = await this.load();
        if (Number(latestCheckpoint.blockNumber) === Number(blockNumber)) {
            const transactionIds = latestCheckpoint.transactionIds;
            latestCheckpoint.transactionIds = transactionIds;
        } else {
            latestCheckpoint.blockNumber = blockNumber;
            latestCheckpoint.transactionIds = [transactionIds];
        }
        await fs.writeFile(cppPath, JSON.stringify(latestCheckpoint));
    }

    async load() {
        const cpPath = this._getCheckpointFileName(this._chaincodeId);
        if (!(await fs.exists(cpPath))) {
            await this._initialize();
        }
        const chkptBuffer = await fs.readFile(cpFile);
        let checkpoint = checkpointBuffer.toString('utf8');
        if (!checkpoint) {
            checkpoint = {};
        } else {
            checkpoint = JSON.parse(checkpoint);
        }
        return checkpoint;
    }
}

function File_SYSTEM_CHECKPOINTER_FACTORY(channelName, listenerName, options) {
    return new FileSystemCheckpointer(channelName, listenerName, options);
}

const gateway = new Gateway();
await gateway.connect({
    checkpointer: {
        factory: FILE_SYSTEM_CHECKPOINTER_FACTORY,
        options: {basePath: '/home/blockchain/checkpoints'} // These options will vary depending on the checkpointer implementation
        // 这些选项将取决于checkpointer的实现
});
```

除了 save()和 load()外，BaseCheckpointer 接口还具有 loadLatestCheckpoint()函数，在 load()返回检查点列表的情况下，该函数将返回最新的不完整检查点(或与特定实施最相关的(or whichever is most relevant for the specific implementation))。

注意:使用文件系统检查指针时，请使用绝对路径而不是相对路径。

为侦听器指定检查点的特定类型时，请在 module:fabric-network.Network\~EventListenerOptions 中使用 checkpointer 选项。

---
