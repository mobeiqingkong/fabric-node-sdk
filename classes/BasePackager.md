# BasePackager

## BasePackager

#### new BasePackager( [keep])

- Constructor
- 参数

| 名称 | 类型 | 必要性 | 描述                   |
| ---- | ---- | ------ | ---------------------- |
| keep | \*   | 可选   | 有效的源文件扩展名数组 |

### Methods

#### findMetadataDescriptors(filePath)

- 查找元数据描述符文件。
- 参数

| 名称     | 类型 | 描述                                                             |
| -------- | ---- | ---------------------------------------------------------------- |
| filePath |      | 包含元数据描述符的顶级目录。结果中仅包含扩展名为“ .json”的文件。 |

- 返回结果

  - 类型

    Promise

#### findSource(filepath)

给定输入'filePath'，以递归方式解析文件系统以查找符合有效链码源条件的所有文件(ISREG + keep)

- 参数

| 名称     | 类型 | 描述 |
| -------- | ---- | ---- |
| filePath |      |      |

#### generateTarGz(descriptors, dest)

根据提供的描述符条目创建一个.tar.gz 流

- 参数

| 名称        | 类型 | 描述 |
| ----------- | ---- | ---- |
| descriptors |      |      |
| dest        |      |      |

- 返回结果

  - 类型

    Promise

#### isMetadata(filePath)

谓词功能，用于确定是否应完全基于文件扩展名将给定路径视为有效的元数据描述符。

- 参数

| 名称     | 类型 | 描述                         |
| -------- | ---- | ---------------------------- |
| filePath |      | 包含元数据描述符的顶级目录。 |

- 返回结果

  为有效的元数据描述符返回 true。

  - 类型

    Promise

#### isSource(filePath)

谓词功能，用于完全基于扩展名来确定是否应将给定路径视为有效源代码。假定已经对文件类型(例如 ISREG)执行了其他检查。

- 参数

| 名称     | 类型 | 描述 |
| -------- | ---- | ---- |
| filePath |      |      |

- 返回结果

  - 类型

    boolean

#### package(chaincodePath, metadataPath)

request.chaincodePath 目录中的所有文件都将包含在存档文件中。

- 参数

| 名称          | 类型 | 描述 |
| ------------- | ---- | ---- |
| chaincodePath |      |      |
| metadataPath  |      |      |

#### packEntry(pack, desc)

给定一个{fqp，name}元组，生成一个 tar 条目，该条目具有明智的标头和从文件系统读取的填充内容。

- 参数

| 名称 | 类型 | 描述 |
| ---- | ---- | ---- |
| pack |      |      |
| desc |      |      |

- 返回结果

  - 类型

    Promise

---
