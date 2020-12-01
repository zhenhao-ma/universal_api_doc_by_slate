# 版本更新信息

### 版本

版本号 `v1.02`

更新时间 `2020-11-30`

### v1.02 更新
1. 修改了Aftershop的整体API设计标准，现在需要提供`deviceName`才能正确调用API。
2. `@Aftershop: 消费者登录`API修改了错误的参数，移除了`userIdentifier`，并且修改了`customerIdentifierForChat`和`customerIdentifierTypeForChat`的描述。
3. `@Aftershop: 发送验证邮箱的链接至消费者邮箱里`改名了，变成了`发送验证邮箱的验证码`。不是链接，而是发送的是验证码。增加了注意事项，修正了参数。
4. 删除了`@Aftershop: 消费者获取消息`不必要的参数`_auth`和`customerIdentifier`。
5. 更正了`@Aftershop: 消费者初始化聊天窗口`下的请求参数`customerIdentifier`
6. 删除了`@Aftershop: 消费者发送消息`下的`fromDevice`，因为已经不再需要这个参数了。本API版本已经增加了`deviceName`。
7. 修改了`@Aftershop: 消费者获取消息`的机制，会根据通用参数`deviceIdentifier`来返回未读消息。

### v1.01 更新

1. 新增消费聊天部分API
2. `/customer/send_email_verification`不再需要提供`email`作为参数。
3. `/customer/register`和`/customer/login`可以额外提供`customerIdentifierForChat`和`customerIdentifierTypeForChat`
参数来合并未登录时候的聊天记录。这两个API返回的`data`格式已经被修改，会返回更多的数据，以及合并消息结果。


