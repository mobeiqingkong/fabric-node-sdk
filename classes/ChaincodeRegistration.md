# ChaincodeRegistration

## ChaincodeRegistration

#### new ChaincodeRegistration(chaincode_id, event_name, event_reg, as_array)

构造一个chaincode回调条目

- 参数:

| 名称         | 类型                 | 描述                                                         |
| ------------ | -------------------- | ------------------------------------------------------------ |
| chaincode_id | string               | chaincode的id                                                |
| event_name   | string &#124; RegExp | 用于过滤事件的正则表达式                                     |
| event_reg    | EventRegistration    | 事件注册回调                                                 |
| as_array     | as_array             | 应该将所有找到的与此定义匹配的链码事件作为数组发送到回调，或者分别为每个回调调用回调。 |

***