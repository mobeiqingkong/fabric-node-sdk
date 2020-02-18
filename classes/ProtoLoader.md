# ProtoLoader

## ProtoLoader

一个用于在运行时轻松地从.proto 文件加载服务描述符和客户端存根定义的类。

#### new ProtoLoader()

### Methods

#### &lt;static&gt; load(filename [, options])

从.proto 文件加载服务描述符和客户端存根定义。

- 参数

| 名称     | 类型   | 描述                              |
| -------- | ------ | --------------------------------- |
| filename | string | .proto 文件的文件名。             |
| options  | Object | 可选。用于加载.proto 文件的选项。 |

返回结果

- 加载的服务描述符和客户端存根定义。

  - 类型

    \*

---
