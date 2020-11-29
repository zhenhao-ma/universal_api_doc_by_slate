# 版本更新信息

### 版本

版本号 `v1.01`

更新时间 `2020-11-29`

### v1.01 更新

1. 新增消费聊天部分API
2. `/customer/send_email_verification`不再需要提供`email`作为参数。
3. `/customer/register`和`/customer/login`可以额外提供`customerIdentifierForChat`和`customerIdentifierTypeForChat`
参数来合并未登录时候的聊天记录。这两个API返回的`data`格式已经被修改。


