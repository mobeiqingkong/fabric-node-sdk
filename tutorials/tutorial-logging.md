# fabric-common:如何使用日志记录(How to use logging)

## 说明

本教程说明了如何使用 Hyperledger Fabric Node.js 客户端日志记录功能。

### 概述

Hyperledger Fabric Node.js 客户端日志记录使用 Node.js "winston"包。 当 Node.js 应用程序首次加载 Hyperledger Fabric 软件包时，将初始化日志记录。 所有 Hyperledger Fabric 客户端对象将使用相同的设置(Peer,Orderer,ChannelEventHub)。

```javascript
const Client = require('fabric-client');
// the logging is now set
// 已设置日志
```

日志记录分为四个级别

- info
- warn
- error
- debug

默认情况下，info，warn 和 error 日志条目将发送到'console'.。debug 将不会被记录。

### 如何更改日志

Hyperledger Fabric 客户端的日志记录由配置设置 hfc-logging 和环境设置 HFC_LOGGING 控制。

- 使用以下条目在 default.json 配置文件中设置日志记录设置:

```json
"hfc-logging": "{'debug':'console', 'info':'console'}"
```

- 使用环境设置将覆盖配置设置:

```shell
export HFC_LOGGING='{"debug":"console","info":"console"}'
```

- 通过指定文件位置作为级别值，日志记录可以使用文件写入条目。

```shell
export HFC_LOGGING='{"debug":"/temp/debug.log","info":"console"}'
```

### 使用来自应用程序的日志

当需要将应用程序代码中的条目与 Hyperledger Fabric 客户端条目一起记录时，请使用以下内容访问同一记录器。

从 1.2 开始

```javascript
const logger = Client.getLogger("APPLICATION");
```

1.2 之前

```javascript
const sdkUtils = require("fabric-client/lib/utils.js");
const logger = sdkUtils.getLogger("APPLICATION");
```

记录

```javascript
const log_info = "Sometext";

logger.info("%s infotext", log_info);
// will log
// info: [APPLICATION]: Sometext infotext

logger.warn("%s warntext", log_info);
// will log
// warn: [APPLICATION]: Sometext warntext

logger.error("%s errortext", log_info);
// will log
// error: [APPLICATION]: Sometext errortext

logger.debug("%s debugtext", log_info);
// will log
// debug: [APPLICATION]: Sometext debugtext
```

---
