# NetworkConfig_1_0

## NetworkConfig_1_0

这是 NetworkConfig API 的实现。它将用于基于 JSON 的通用连接配置文件的 v1.0.1 版本。 （也称为网络配置）。

#### new NetworkConfig_1_0(network_config)

constructor

- 参数

| 名称           | 类型   | 描述                              |
| -------------- | ------ | --------------------------------- |
| network_config | Object | JSON 对象中表示的通用连接配置文件 |

### 扩展

- module:api.NetworkConfig

### Methods

#### getNetworkConfigLocation()

获取从中加载网络配置的文件系统路径（如果有）。

返回结果

- 加载网络配置的文件系统路径；如果未直接从文件系统加载，则为 null。

  - 类型

    string

---
