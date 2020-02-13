# FabricCAClient

## (未完成)FabricCAClient

与Fabric CA API通信的客户端

#### new FabricCAClient(connect_opts)

constructor

- 参数

| 名称         | 类型   | 描述                                          |
| :----------- | :----- | --------------------------------------------- |
| connect_opts | object | 与Fabric CA服务器通信的连接选项，属性表见下表 |

- 属性表

| 名称       | 类型                                                         | 描述                                                         |
| :--------- | :----------------------------------------------------------- | ------------------------------------------------------------ |
| protocol   | string                                                       | 使用的协议（HTTP或HTTPS）                                    |
| hostname   | string                                                       | Fabric CA服务器终结点的主机名                                |
| port       | number                                                       | Fabric CA服务器端点的端口                                    |
| tlsOptions | [TLSOptions](https://hyperledger.github.io/fabric-sdk-node/release-1.4/global.html#TLSOptions) | Fabric CA端点使用“ https”时要使用的TLS设置                   |
| caname     | string                                                       | CA的可选名称。 Fabric-ca服务器从单个服务器支持多个证书颁发机构。如果省略，为null或为空字符串，则默认CA为请求的目标 |

##### Parameters:



##### Throws: