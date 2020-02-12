# Endpoint

## Endpoint

Endpoint类代表一个远程grpc或grpcs目标

#### new Endpoint(url, pem, clientKey, clientCert)

- 参数

| 名称       | 类型   | 描述 |
| :--------- | :----- | ---- |
| url        | string |      |
| pem        | string |      |
| clientKey  | string |      |
| clientCert | string |      |

### Methods

#### isTLS()

确定此端点是否使用TLS。

返回结果

- 如果此端点使用TLS，则为true，否则为false。

  - 类型

    boolean