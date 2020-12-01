# @Aftershop: 消费者初始化聊天窗口

> 请求数据

```json
{
    "deviceIdentifier": "2342asdfasdf3452345",
    "deviceName": "postman",
    "bcid": "2877383e-5551-4708-862b-a0827d294d73",
    "_auth": "160594db23a06982362160594db23a06982362730JzWxGPY"
}
```

> 返回JSON数据

```json
{
    "data": {
        "messages": [],
        "sessionId": "kHkHRvMhtORbvOuVSOBzRvkHRvMhtORbvOuVSOBzMhtORbvOuVSOBz"
    },
    "status": true
}
```

获得聊天室的`sessionId`，这个参数应该要随时传回给其他聊天的API。

### 请求

`POST /message/customer/init_message_session`

### 请求参数

参数 | 类型 | 可选 | 描述
--------- | ------- | ----------- | -----------
_auth | String | 2选1 | 标准`_auth`，只要提供了`_auth`，就不会考虑`customerIdentifier`
customerIdentifier | String | 2选1 | 未登录游客情况下，给游客自动生成一个访客ID。用这个访客ID去申请聊天室。

### 返回参数

参数 | 类型 | 可选 | 描述
--------- | ------- | ----------- | -----------
messages | Array | - | 最新的消息，默认10条最新的。
sessionId | String | - | 聊天室ID

# @Aftershop: 消费者发送消息

> 请求数据

```json
{
    "deviceIdentifier": "2342asdfasdf3452345",
    "deviceName": "postman",
    "bcid": "2877383e-5551-4708-862b-a0827d294d73",
    "_auth": "1605DKhO3BfV81rJzWxGPY",
    "sessionId": "kHJ4nasdfasdfasdfasdfOuVSOBz",
    "value": "this is a text",
    "sendAt": 1,
    "type": "text",
    "fromDevice": "Macbook pro"
}
```

> 返回JSON数据

```json
{
    "data": {
        "bcid": "2877383e-5551-4708-862b-a0827d294d73",
        "chatRoomMessageId": "7c804077-2200-4ae0-9d19-a7c0db59ed11",
        "clientId": "076e3378-a3f4-441e-a998-4a89ee894d83",
        "createdAt": 1606644054.303437,
        "createdBy": "3TnctrAsdF-fVhLYyBUN",
        "customerIdentifier": "3TnctrAsdF-fVhLYyBUN",
        "customerIdentifierType": "customer",
        "freezed": false,
        "fromDevice": "Macbook pro",
        "fromIp": "127.0.0.1",
        "handlerIdentifier": "",
        "handlerIdentifierType": "recipient",
        "revoked": false,
        "sendAt": 1.0,
        "sendBy": "customer",
        "sessionId": "kHRvMBiVCshRGdSb075OgThtORbvOuVSOBz",
        "type": "text",
        "value": "this is a text"
    },
    "status": true
}
```

获得聊天室的`sessionId`，这个参数应该要随时传回给其他聊天的API。

### 请求

`POST /message/customer/send_message`

### 请求参数

参数 | 类型 | 可选 | 描述
--------- | ------- | ----------- | -----------
fromDevice | String | - | 设备的系列，例如`三星手机`，`iPhone`，`chrome浏览器`
sessionId | String | - | 聊天室ID
type | String | - | 聊天所发送的内容类别，只能是`text`, `video`, `image`
value | String | - | 聊天所发送的值，跟`type`相对应。
sendAt | Float | - | 内容发出时间，为`Unix epoch time`，可以上网了解。单位为秒，请精准到微秒。

### 返回参数

参数 | 类型 | 可选 | 描述
--------- | ------- | ----------- | -----------
data | Object | - | 消息发送成功后的数据。


# @Aftershop: 消费者获取消息

> 请求数据

```json
{
    "deviceIdentifier": "2342asdfasdf3452345",
    "deviceName": "postman",
    "bcid": "2877383e-5551-4708-862b-a0827d294d73",
    "nextOrPrevious": "next",
    "_auth": "1605bFlEecvWnviBSTWKpS1crUEXzyODKhO3BfV81rJzWxGPY",
    "sessionId": "kHRQSzsGy1zQi5ChGlHD4PER6sebCNezzdcBDzLbBTnda8D9RQVFs8yFlCh1qwKqnfsDtiWjGf0E1E5P7veDVMHICvBMtWeI43WWFrhgGWCspmv7GsqpagLTMf4Y4IknVCshRGdSb075OgThtORbvOuVSOBz"
}
```

> 返回JSON数据

```json
{
    "data": [
        {
            "bcid": "2877383e-5551-4708-862b-a0827d294d73",
            "chatRoomMessageId": "e7411d4c-34c1-43de-aca9-5dbfed46a90e",
            "clientId": "076e3378-a3f4-441e-a998-4a89ee894d83",
            "createdAt": 1606648647.198173,
            "createdBy": "3TnctrAsdF-fVhLYyBUN",
            "customerIdentifier": "3TnctrAsdF-fVhLYyBUN",
            "customerIdentifierType": "customer",
            "fromDevice": "postman api",
            "fromIp": "127.0.0.1",
            "handlerIdentifier": "",
            "handlerIdentifierType": "recipient",
            "revoked": false,
            "sendAt": 1.0,
            "sendBy": "customer",
            "sessionId": "kHRQSzsGy1zQi5ChGlHD4PER6sebCNezzdcBDzLbBTnda8D9RQVFs8yFlCh1qwKqnfsDtiWjGf0E1E5P7veDVMHICvBMtWeI43WWFrhgGWCspmv7GsqpagLTMf4Y4IknVCshRGdSb075OgThtORbvOuVSOBz",
            "type": "text",
            "value": "this is a text"
        },
        {
            "bcid": "2877383e-5551-4708-862b-a0827d294d73",
            "chatRoomMessageId": "71ee0b97-52b7-44f6-8b2b-25dfc8be7255",
            "clientId": "076e3378-a3f4-441e-a998-4a89ee894d83",
            "createdAt": 1606649770.858025,
            "createdBy": "3TnctrAsdF-fVhLYyBUN",
            "customerIdentifier": "3TnctrAsdF-fVhLYyBUN",
            "customerIdentifierType": "customer",
            "fromDevice": "postman api",
            "fromIp": "127.0.0.1",
            "handlerIdentifier": "",
            "handlerIdentifierType": "recipient",
            "revoked": false,
            "sendAt": 2.0,
            "sendBy": "customer",
            "sessionId": "kHRQSzsGy1zQi5ChGlHD4PER6sebCNezzdcBDzLbBTnda8D9RQVFs8yFlCh1qwKqnfsDtiWjGf0E1E5P7veDVMHICvBMtWeI43WWFrhgGWCspmv7GsqpagLTMf4Y4IknVCshRGdSb075OgThtORbvOuVSOBz",
            "type": "video",
            "value": "https://videourl.com/aaa"
        }
    ],
    "status": true
}
```

> 可以设置size，并且给出上一次最新的一条消息的id，得到最新未读的消息。

```json
{
    "deviceIdentifier": "2342asdfasdf3452345",
    "deviceName": "postman",
    "bcid": "2877383e-5551-4708-862b-a0827d294d73",
    "size": 1,
    "chatRoomMessageId": "71ee0b97-52b7-44f6-8b2b-25dfc8be7255",
    "nextOrPrevious": "next",
    "_auth": "160594db23a069823627308ae37a3ac8_and_t8n9iXuML6KKLoRY6b4N3UEVos2TIgzLQNG4BnZFErQORRaFBEonxRjAZkepNcKACTrPlCCdatQwOXUY0q6pTFNnmus2QV9DWvZmWzJhyDjYqgp8UKlM3kTfggMCLPUHFaYYJAnjxioh4toXmCh8HZdVgxbMMhmoxdOj8CIqkqgnmw9vfX9m46fosTyXOASd2qTVK9HVWT9RqMknQ7V8P8ODbfryExVgY9WNlrvHC6j8mVHUBj5mrhS8SYM8kV6Ck9RV3AVaeMqIgWdOuElxVfpaJMQhAai3yR666zysqpaG4Fp0F5VEQECvHCvwxEfQZjKAwRr3UlYfjni7tosD2xUuHHm2oJnL5jimj1bvAtiyXiTRcts15mgyscuMpkA7oGuLLgyxl5gu1NEvpASZBaMWZjJIlEDicXRG5n21z75onvVrJyz0zt5jV5AjNw6HMG5Oo8V4RBQg5ogki0FPY9t9xrzzH8OpaSDGKFY3AyM4pHCewKoLGwh4tM9qFHF7obfV5U6TsXJ7UIQhDxMnChN1lpywiKWXLw8VYSYt8z9Jsrt6s5bRRKisg5UIbfJwRfyg9RGJyGF8C9c5JUVAq0qjAaYvthPwNfq5b8DLLlea7H2iIZCuPidHCy4svGJVdzLXkLR2k2D5tQWR3fRjtiDh72XxWNnzesDz8JQ3lbQxjIcYg0fvH6WtSwQgNU0iXYbRJDbSrJHdSSZkaPml0jXBNENQzERDsOiSL5y9GN1X39WFperXvP5BXsDscwYvdIjwOUZOynlNK4mur2IwIhGgXS8ciaDFBDif24QGT2yqFqJo74IFWlxS1hzStb762ELMRuSImnK2Yc0RUo8rinMIOJry1vdSwSdTyFez9S0hWpHCtYDcogJrKo8xTsERlpPz4O6QPJP7AgYbFlEecvWnvJM9SzXNd3qeDykH8EXYWCdHxu7rRK6g3WoadI6QbbLFfiBSTWKpS1crUEXzyODKhO3BfV81rJzWxGPY",
    "sessionId": "kHRvMBvOuVSOBz"
}
```

> 这样子就会根据size返回，并且是最新未读的。

```json
{
    "data": [
        {
            "bcid": "2877383e-5551-4708-862b-a0827d294d73",
            "chatRoomMessageId": "ccf943ef-bb3c-4567-a2e5-613c35f02a85",
            "clientId": "076e3378-a3f4-441e-a998-4a89ee894d83",
            "createdAt": 1606651568.606339,
            "createdBy": "fatSs5w_-pluAiGT3Wnu",
            "customerIdentifier": "",
            "customerIdentifierType": "recipient",
            "fromDevice": "Chrome browser 2019 version 112.123",
            "fromIp": "127.0.0.1",
            "handlerIdentifier": "fatSs5w_-pluAiGT3Wnu",
            "handlerIdentifierType": "clientUser",
            "revoked": false,
            "sendAt": 3.0,
            "sendBy": "handler",
            "sessionId": "kHRvMBvOuVSOBz",
            "type": "text",
            "value": "this is a reply!!! hahaha"
        }
    ],
    "status": true
}
```

通过聊天室的`sessionId`，获取消息。

### 请求

`POST /message/customer/get_messages`

### 请求参数

参数 | 类型 | 可选 | 描述
--------- | ------- | ----------- | -----------
sessionId | String | - | 聊天室ID
size | Int | 可选 | 最多返回多少条，默认 10
nextOrPrevious | String | - | 方向，只能是`next`或`previous`，类似于App中的向上滚动/或向下滚动。
chatRoomMessageId | String | 可选 | 当`nextOrPrevious`为`previous`时提供。返回`chatRoomMessageId`这条消息之前的消息。

### 返回参数

参数 | 类型 | 可选 | 描述
--------- | ------- | ----------- | -----------
data | Array | - | 消息列表，最早发的，会在列表最前，最晚最新的，会在列表最后。

# @Aftershop: 客户成员clientUser发送消息回复

> 请求数据

```json
{
    "sessionId": "kHRvMBvOuVSOBz",
    "sendAt": 3,
    "value": "this is a reply!!! hahaha",
    "type": "text",
    "bcid": "2877383e-5551-4708-862b-a0827d294d73"
}
```

> 返回JSON数据

```json
{
    "data": {
        "bcid": "2877383e-5551-4708-862b-a0827d294d73",
        "chatRoomMessageId": "ccf943ef-bb3c-4567-a2e5-613c35f02a85",
        "clientId": "076e3378-a3f4-441e-a998-4a89ee894d83",
        "createdAt": 1606651568.606339,
        "createdBy": "fatSs5w_-pluAiGT3Wnu",
        "customerIdentifier": "",
        "customerIdentifierType": "recipient",
        "freezed": false,
        "fromDevice": "Chrome browser 2019 version 112.123",
        "fromIp": "127.0.0.1",
        "handlerIdentifier": "fatSs5w_-pluAiGT3Wnu",
        "handlerIdentifierType": "clientUser",
        "revoked": false,
        "sendAt": 3.0,
        "sendBy": "handler",
        "sessionId": "kHRvMBvOuVSOBz",
        "type": "text",
        "value": "this is a reply!!! hahaha"
    },
    "status": true
}
```

通过聊天室的`sessionId`，获取消息。

### 请求

`POST /message/client/send_message`

### 请求参数

参数 | 类型 | 可选 | 描述
--------- | ------- | ----------- | -----------
sessionId | String | - | 聊天室ID
type | String | - | 聊天所发送的内容类别，只能是`text`, `video`, `image`
value | String | - | 聊天所发送的值，跟`type`相对应。
sendAt | Float | - | 内容发出时间，为`Unix epoch time`，可以上网了解。单位为秒，请精准到微秒。

### 返回参数

参数 | 类型 | 可选 | 描述
--------- | ------- | ----------- | -----------
data | Object | - | 消息发送成功后的数据。

<aside class="notice">可以通过这个clientUser的API，回复消息，来实现开发debug。调用这个API，需要先获取access token。建议用Postman执行。</aside>
