# BaseClient

## BaseClient

使用 CryptoSuite 签名和哈希的客户端的基类。它还包含用于构造 [CryptoKeyStore](CryptoKeyStore.md), [CryptoSuite](module-api.CryptoSuite.md) and [KeyValueStore](module-api.KeyValueStore.md) 的新实例的实用程序方法。

#### new BaseClient()

### Methods

#### &lt;static&gt; addConfigFile(path)

将文件添加到属于分层配置的配置设置文件列表的顶部 。这些文件将覆盖默认设置，并被环境，命令行参数和以编程方式设置为配置设置的设置覆盖。分层配置设置的搜索顺序:请参阅 [BaseClient.getConfigSetting](BaseClient.md#getConfigSetting)

- 参数

| 名称 | 类型   | 描述                                 |
| ---- | ------ | ------------------------------------ |
| path | String | 要添加到配置文件列表顶部的文件的路径 |

#### &lt;static&gt; getConfigSetting(name, default_value)

从分层配置中检索设置，如果找不到，则将返回提供的默认值。

分层配置设置对设置 aa-bb 的搜索顺序:

1. 内存:如果设置已添加如下:

```shell
Client.setConfigSetting('aa-bb', 'value')
```

2. 命令行参数，示例:

   ```shell
   node app.js --aa-bb value
   ```

3. 环境变量 :

```shell
AA_BB=value node app.js
```

4. 自定义文件:添加时用 addConfigFile(path)添加的所有文件将按添加顺序排序，其中，之后添加的文件中的设置将覆盖之前添加的那些相同的设置

5. 带有默认设置的文件位于 lib/config/default.json

- 参数:

| 名称          | 类型   | 描述                                       |
| ------------- | ------ | ------------------------------------------ |
| name          | String | 设置名称                                   |
| default_value | Object | 如果在分层配置中找不到设置的值则采用这个值 |

#### &lt;static&gt; getLogger(name)

使用此方法来获取日志记录，该日志记录会将条目添加到 Hyperledger Fabric 客户端所使用的相同位置。

- 参数

| 名称 | 类型   | 描述                                                 |
| ---- | ------ | ---------------------------------------------------- |
| name | string | 要添加到日志条目的标签名称。帮助识别日志条目的来源。 |

- 返回结果:

  可以使用“ info()”，“ warn()”，“ error()”和“ debug()”方法记录整个日志的记录器，以标记日志条目的类型。

  - 类型

    Logger

#### &lt;static&gt; newCryptoKeyStore(KVSImplClass, opts)

这是工厂方法。它返回 CryptoKeyStore 的新实例。当应用程序需要使用默认存储以外的其他密钥存储时，应创建一个新的 CryptoKeyStore 并将其设置在 CryptoSuite 上。

**cryptosuite.setCryptoKeyStore(Client.newCryptoKeyStore(KVSImplClass, opts))**

- 参数

| 名称         | 类型              | 描述                                                                                                                                     |
| ------------ | ----------------- | ---------------------------------------------------------------------------------------------------------------------------------------- |
| KVSImplClass | api.KeyValueStore | 可选。内置密钥存储区可保存私钥。密钥库可以由不同的 KeyValueStore 实现来支持。如果指定，则参数的值必须指向实现 KeyValueStore 接口的模块。 |
| opts         | Object            | 构造函数中使用的特定于实现的选项对象                                                                                                     |

- 返回结果:

  CryptoKeystore 的新实例

  - 类型

    [CryptoKeyStore](https://hyperledger.github.io/fabric-sdk-node/release-1.4/CryptoKeyStore.html)

#### &lt;static&gt; newCryptoSuite(setting)

这是工厂方法。它基于传入的“setting”(如果不存在，则基于 [CryptoSetting](https://hyperledger.github.io/fabric-sdk-node/release-1.4/global.html#CryptoSetting) 属性的默认值)返回 CryptoSuite API 实现的新实例。

- 参数

| 名称    | 类型                                                                                                 | 描述   |
| ------- | ---------------------------------------------------------------------------------------------------- | ------ |
| setting | [CryptoSetting](https://hyperledger.github.io/fabric-sdk-node/release-1.4/global.html#CryptoSetting) | 可选。 |

- 返回结果:

  CryptoSuite API 实现的新实例

  - 类型

    [module:api.CryptoSuite](https://hyperledger.github.io/fabric-sdk-node/release-1.4/module-api.CryptoSuite.html)

#### &lt;static&gt; newDefaultKeyValueStore(options)

获取 KeyValueStore 类的实例。默认情况下，它返回基于文件(FileKeyValueStore)的内置实现。 可以使用配置设置键值存储来覆盖它，其值是用于替代实现的 CommonJS 模块的完整路径。

- 参数

| 名称    | 类型   | 描述                                                                                                   |
| ------- | ------ | ------------------------------------------------------------------------------------------------------ |
| options | Object | 用于初始化实例的特定实现。对于基于文件的内置实现，需要存储的顶级文件夹(top-level folder)的单个属性路径 |

- 返回结果:

  KeyValueStore 实现的 [module:api.KeyValueStore](https://hyperledger.github.io/fabric-sdk-node/release-1.4/module-api.KeyValueStore.html) 实例的 Promise

  - 类型

    Promise

#### &lt;static&gt; normalizeX509(raw)

修复了可能格式不正确的证书字符串。确保以'----- BEGIN CERTIFICATE -----'作为开始行，以'----- END CERTIFICATE -----'作为结束行，以便与 x509 解析器兼容。将删除或添加所需的换行符和回车符。

- 参数

| 名称 | 类型   | 描述                   |
| ---- | ------ | ---------------------- |
| raw  | string | 包含 X509 证书的字符串 |

- 返回结果:

  提示开头和结尾部分不正确的 Error。

  - 类型

    Error

#### &lt;static&gt; setConfigSetting(name, value)

添加设置以覆盖属于分层配置的所有设置。 分层配置设置的搜索顺序:请参阅 [BaseClient.getConfigSetting](https://hyperledger.github.io/fabric-sdk-node/release-1.4/BaseClient.html#.getConfigSetting)

- 参数

| 名称  | 类型   | 描述       |
| ----- | ------ | ---------- |
| name  | String | 设置的名称 |
| value | Object | 设置的值   |

#### &lt;static&gt; setLogger(logger)

给整个 SDK 配置一个日志记录器，以使用并覆盖默认日志记录器。除非调用此方法，否则 SDK 将使用基于 [winston](https://www.npmjs.com/package/winston) 的默认日志记录器。使用内置的基于 Winston 的记录器时，请使用配置设置 hfc-logging 以以下格式传递配置:

```
{
  'error': 'error.log', // 将'error'日志打印到与node.js当前工作目录相关的文件error.log
  'debug': '/tmp/myapp/debug.log',  // 'debug'和其他更关键的内容('info', 'warn', 'error')也可以是绝对路径
  'info': 'console'         // 'console'是用于登录控制台的关键字
}
```

- 参数

| 名称   | 类型   | 描述                                                                                                                                                                           |
| ------ | ------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| logger | Object | 一个 logger 实例，该实例定义了以下方法:debug()，info()，warn()，error()，以及诸如[util.format](https://nodejs.org/api/util.html#util_util_format_format)之类的字符串插值方法。 |

- #### getCryptoSuite()

  返回此客户端实例使用的 CryptoSuite 对象

- 返回结果:

  CryptoKeystore 的新实例

  - 类型

    [module:api.CryptoSuite](https://hyperledger.github.io/fabric-sdk-node/release-1.4/module-api.CryptoSuite.html)

- #### setCryptoSuite(cryptoSuite)

  设置客户端实例以使用 CryptoSuite 对象进行签名和哈希创建，设置 CryptoSuite 是可选的，因为客户端将基于默认配置设置构造实例:

  - crypto-hsm:使用硬件安全模块(如果设置为 true)或基于软件的密钥管理(如果设置为 false)的实现
  - crypto-keysize:与数字签名公钥算法一起使用的安全级别或密钥大小。当前支持 ECDSA，有效密钥大小为 256 和 384
  - crypto-hash-algo:哈希算法
  - key-value-store :某些 CryptoSuite 实现需要密钥存储来保留私钥。为此提供了一个 [CryptoKeyStore](https://hyperledger.github.io/fabric-sdk-node/release-1.4/CryptoKeyStore.html) ， 可以在 KeyValueStore 接口的任何实现之上使用，例如基于文件的存储或基于数据库的存储 。 具体的实现方式取决于此配置设置的值。

- 参数

| 名称        | 类型                                                                                                            | 描述             |
| ----------- | --------------------------------------------------------------------------------------------------------------- | ---------------- |
| cryptoSuite | [module:api.CryptoSuite](https://hyperledger.github.io/fabric-sdk-node/release-1.4/module-api.CryptoSuite.html) | CryptoSuite 对象 |

---
