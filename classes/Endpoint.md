<!--
 * @Author: LongZhiQiu
 * @Date: 2020-02-06 22:45:23
 * @Version: Do not edit
 * @Description: file content
 * @LastEditors: LongZhiQiu
 * @LastEditTime: 2020-02-13 18:10:00
 * @Auditor: LongZhiQiu
 -->

# Endpoint

## Endpoint

Endpoint 类代表一个远程 grpc 或 grpcs 目标

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

确定此端点是否使用 TLS。

返回结果

- 如果此端点使用 TLS，则为 true，否则为 false。

  - 类型

    boolean

---
