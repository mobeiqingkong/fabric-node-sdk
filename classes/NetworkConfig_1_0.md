# NetworkConfig_1_0

## NetworkConfig_1_0

这是NetworkConfig API的实现。它将用于基于JSON的通用连接配置文件的v1.0.1版本。 （也称为网络配置）。

#### new NetworkConfig_1_0(network_config)

constructor

- 参数

| 名称           | 类型   | 描述                             |
| -------------- | ------ | -------------------------------- |
| network_config | Object | JSON对象中表示的通用连接配置文件 |

### 扩展

- module:api.NetworkConfig

### Methods

#### getNetworkConfigLocation()

获取从中加载网络配置的文件系统路径（如果有）。

返回结果

- 加载网络配置的文件系统路径；如果未直接从文件系统加载，则为null。
  - 类型

    string