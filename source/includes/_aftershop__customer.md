#API调用

> 示范代码

```python
import requests
BASE_URL = "https://xuggest.com/api/"
def post_api(url, dict_data):
    response = requests.post(url=BASE_URL + url, json=dict_data)
    if response.status_code != 200:
        raise ValueError('返回错误状态码：代码 {}'.format(response.status_code))
    resp = response.json()
    if not resp['status']:
        raise ValueError('返回错误信息：{}'.format(resp['msg']))
    if 'data' in resp:
        return resp['data']
    else:
        return resp['status']
```

> 这是一个基础调用function

开发需要把调用API封装成方法/类，这样能更好的处理异常以及未来的版本迭代修改等。
最值得注意的是，返回时需要检查状态码和`status`参数，然后及时把错误提示显示给用户。

# @Aftershop: 消费者注册

> 请求数据

```json
{
    "email": "bob123@gmail.com",
    "password": "123456",
    "source": "app",
    "deviceIdentifier": "2342asdfasdf3452345",
    "deviceName": "postman",
    "bcid": "2877383e-5551-4708-862b-a0827d294d73"
}
```

> 返回JSON数据

```json
{
    "data": {
        "_auth": "b5867c6JkhT",
        "customer": {
            "avatar": "",
            "bcid": "2877383e-5551-4708-862b-a0827d294d73",
            "clientId": "076e3378-a3f4-441e-a998-4a89ee894d83",
            "createdAt": 0.0,
            "customerId": "g7eVdJny_loHnFRVC_St",
            "email": "bob123@gmail.com",
            "emailVerified": false,
            "freezed": false,
            "history": {},
            "nickname": "Customer",
            "passwordHashed": "$pbkdf2-sha256$29000$iZHSWiuF8D5njHHOmdOa8w$.FkJQiM2zKri4TAZmT7OiAUr2pi2IeGsPkTNr6j9mzQ",
            "source": "app"
        },
        "mergeChatError": "",
        "mergeChatStatus": false
    },
    "status": true
}
```

### 请求

`POST /customer/register`

### 请求参数

参数 | 类型 | 可选 | 描述
--------- | ------- | ----------- | -----------
email | String | - | 邮箱
password | String | - | 密码
source | String | - | 注册者来源，`app`或`website`
registrationCode | String | 可选 | 邮箱验证码，当`forceToCreateWarranty`为True时才有效
orderNumber | String | 可选 | 订单号码，当`forceToCreateWarranty`为True时才有效
registerFrom | String | 可选 | `appId`或者是`jsSnippetId`，当`forceToCreateWarranty`为True时才有效
forceToCreateWarranty | Bool | 可选 | 默认False，为True时将会自动让账号的邮箱验证状态设置成`True`，另外会自动生成一个售后订单号。

### 返回参数

参数 | 类型 | 可选 | 描述
--------- | ------- | ----------- | -----------
_auth | String | - | 登录凭证
customer | Object | - | 用户数据
mergeChatError | String | - | 合并聊天记录如果失败了的失败信息
mergeChatStatus | Boolean | - | 是否成功合并聊天记录
warrantyRegisterError | String | - | 创建售后订单时出现的错误
warrantyRegisterStatus | Boolean | - | 是否有创建成功售后订单
warranty | Object | - | 创建成功后的售后订单对象

# @Aftershop: 消费者登录

> 请求数据

```json
{
    "email": "bob010377.9@gmail.com",
    "password": "wow123",
    "deviceIdentifier": "2342asdfasdf3452345",
    "deviceName": "postman",
    "bcid": "2877383e-5551-4708-862b-a0827d294d73"
}
```

> 返回JSON数据

```json
{
    "data": {
        "_auth": "160594dR6nY6bJg83",
        "customer": {
            "avatar": "",
            "bcid": "2877383e-5551-4708-862b-a0827d294d73",
            "clientId": "076e3378-a3f4-441e-a998-4a89ee894d83",
            "createdAt": 0.0,
            "customerId": "3TnctrAsdF-fVhLYyBUN",
            "email": "bob010377.9@gmail.com",
            "emailVerified": false,
            "freezed": false,
            "history": {},
            "nickname": "Customer",
            "passwordHashed": "$pbkdf2-sha256$29000$j3FubS1FiJHSmhOC8D7n/A$8T7tJvq0WgWhu2uVeGVBTg4x3l4CRTPbQaZLMLvS.cI",
            "source": "app"
        },
        "mergeChatError": "",
        "mergeChatStatus": false
    },
    "status": true
}
```

### 请求

`POST /customer/login`

### 请求参数

参数 | 类型 | 可选 | 描述
--------- | ------- | ----------- | -----------
email | String | - | 邮箱
password | String | - | 密码

### 返回参数

参数 | 类型 | 可选 | 描述
--------- | ------- | ----------- | -----------
_auth | String | - | 登录凭证
customer | Object | - | 用户数据
mergeChatError | String | - | 合并聊天记录如果失败了的失败信息
mergeChatStatus | Boolean | - | 是否成功合并聊天记录

# @Aftershop: 发送邮箱验证码-无需登录/注册时调用

> 请求数据

```json
{
    "deviceIdentifier": "2342asdfasdf3452345",
    "deviceName": "postman",
    "bcid": "2877383e-5551-4708-862b-a0827d294d73",
    "_auth": "",
    "email": "bob01@gmail.com"
}
```

> 返回JSON数据

```json
{
    "status": true
}
```

无需登录，注册时就可以调用本API。邮箱验证码可用于调用注册api`@Aftershop: 消费者注册`时候，以`registrationCode`传给后端。

### 请求

`POST /customer/send_email_registration_code`

### 请求参数

参数 | 类型 | 可选 | 描述
--------- | ------- | ----------- | -----------
email | String | - | 邮箱

# @Aftershop: 发送验证邮箱的验证码至消费者邮箱里

> 请求数据

```json
{
    "deviceIdentifier": "2342asdfasdf3452345",
    "deviceName": "postman",
    "bcid": "2877383e-5551-4708-862b-a0827d294d73",
    "_auth": "a1694737b8fdf917ee770973fc383d24"
}
```

> 返回JSON数据

```json
{
    "status": true
}
```

必须要登录了才能调用本API。将会通过登录凭证来匹配用户的邮箱`email`。系统将会发送验证邮箱所需要的验证码`verifyCode`到邮箱，用户可以通过这个验证码，实现验证邮箱的功能。

### 请求

`POST /customer/send_email_verification`

### 请求参数

参数 | 类型 | 可选 | 描述
--------- | ------- | ----------- | -----------
email | String | - | 邮箱

这里值得注意的几个事情：
1. 验证码和邮箱匹配。如果邮箱修改了，那么验证码就算输入正确，通过本API提交，也会报错。
2. 如果用户通过`@Aftershop: 更新消费者用户信息`更新了邮箱的话，邮箱验证状态会被重置。
3. 如果用户重置了密码，邮箱验证状态也会被设置成`已验证`。

<aside class="notice">
每个用户的邮箱默认是未被验证的，我们在注册流程中并没有强调验证用户邮箱。所以我们后续会在必要的时候要求验证用户的邮箱。而你必须要使用API返回的<code>status</code>来帮助判断是否发件成功。用户会收到验证邮箱所需要的验证码，可以提交到平台上。
</aside>

# @Aftershop: 发送重置密码的验证码至消费者邮箱里

> 请求数据

```json
{
    "email": "bob@gmail.com",
    "deviceIdentifier": "2342asdfasdf3452345",
    "deviceName": "postman",
    "bcid": "2877383e-5551-4708-862b-a0827d294d73",
    "_auth": "a1694737b8fdf917ee770973fc383d24"
}
```

> 返回JSON数据

```json
{
    "status": true
}
```

发送重置密码所需要的验证码`resetCode`到邮箱，用户可以通过这个验证码，实现重置密码的功能。

### 请求

`POST /customer/send_reset_password_code`

### 请求参数

参数 | 类型 | 可选 | 描述
--------- | ------- | ----------- | -----------
email | String | - | 邮箱

### 返回参数

参数 | 类型 | 可选 | 描述
--------- | ------- | ----------- | -----------
status | Boolean | - | API执行成功，也就是发送成功


# @Aftershop: 检查邮箱是否已被注册

> 请求数据

```json
{
    "email": "bob@gmail.com",
    "deviceIdentifier": "2342asdfasdf3452345",
    "deviceName": "postman",
    "bcid": "2877383e-5551-4708-862b-a0827d294d73",
    "_auth": "a1694737b8fdf917ee770973fc383d24"
}
```

> 返回JSON数据

```json
{
    "data": true,
    "status": true
}
```

可以通过这个API，在用户输入邮箱的时候检查，该邮箱是否已被注册。

### 请求

`POST /customer/is_email_registered`

### 请求参数

参数 | 类型 | 可选 | 描述
--------- | ------- | ----------- | -----------
email | String | - | 邮箱

### 返回参数

参数 | 类型 | 可选 | 描述
--------- | ------- | ----------- | -----------
data | Boolean | - | 是否已被注册

<aside class="warning">如果发现用户的邮箱已被注册，而用户感觉是别人盗用了邮箱注册。那么系统应该提示用户使用<code>忘记密码</code>的功能来找回密码。</aside>

# @Aftershop: 确认邮箱验证

> 请求数据

```json
{
    "email": "bob010377.9@gmail.com",
    "deviceIdentifier": "2342asdfasdf3452345",
    "deviceName": "postman",
    "bcid": "2877383e-5551-4708-862b-a0827d294d73",
    "verifyCode": "675165"
}
```

> 返回JSON数据

```json
{
    "data": true,
    "status": true
}
```

本API允许提交`verifyCode`邮箱验证码来确认邮箱验证。配合`/customer/send_email_verification`API，可以实现验证邮箱的功能。

### 请求

`POST /customer/verify_email`

### 请求参数

参数 | 类型 | 可选 | 描述
--------- | ------- | ----------- | -----------
email | String | - | 邮箱

### 返回参数

参数 | 类型 | 可选 | 描述
--------- | ------- | ----------- | -----------
data | Boolean | - | 是否验证成功

# @Aftershop: 确认修改密码

> 请求数据

```json
{
    "email": "bob010377.9@gmail.com",
    "deviceIdentifier": "2342asdfasdf3452345",
    "deviceName": "postman",
    "bcid": "2877383e-5551-4708-862b-a0827d294d73",
    "resetCode": "12392",
    "password": "wow123"
}
```

> 返回JSON数据

```json
{
    "data": "ef136896b85dfd337540c7cc26cb2257",
    "status": true
}
```

本API允许提交`resetCode`密码重置验证码来确认实现修改密码。配合`/customer/send_reset_password_code`API，可以实现修改密码的功能。

### 请求

`POST /customer/reset_password`

### 请求参数

参数 | 类型 | 可选 | 描述
--------- | ------- | ----------- | -----------
resetCode | String | - | 重置密码验证码，通过别的API发送至邮箱获取
password | String | - | 新密码
email | String | - | 邮箱

### 返回参数

参数 | 类型 | 可选 | 描述
--------- | ------- | ----------- | -----------
data | String | - | 登录凭证 _auth

<aside class="notice">成功调用重置密码，重置成功后，用户的邮箱验证状态也将会被设置成验证成功。无需单独再验证多一次邮箱。</aside>

# @Aftershop: 更新消费者用户信息

> 请求数据

```json
{
    "email": "bob@gmail.com",
    "deviceIdentifier": "2342asdfasdf3452345",
    "deviceName": "postman",
    "bcid": "2877383e-5551-4708-862b-a0827d294d73",
    "resetCode": "12392",
    "nickname": "my new nickname",
    "_auth": "ef136896b85dfd337540c7cc26cb2257"
}
```

> 返回JSON数据

```json
{
    "data": {
        "avatar": "",
        "bcid": "2877383e-5551-4708-862b-a0827d294d73",
        "clientId": "076e3378-a3f4-441e-a998-4a89ee894d83",
        "createdAt": 0.0,
        "customerId": "awYcbRqifXnpnesNSfJ2",
        "email": "bob@gmail.com",
        "emailVerified": true,
        "freezed": false,
        "history": {},
        "lastModifiedBy": "awYcbRqifXnpnesNSfJ2",
        "nickname": "my new nickname",
        "passwordHashed": "$pbkdf2-sha256$29000$YswZQ6j1Psc4BwAgxFgrBQ$kUNb9fhAI2mblHrPbjW0//LTphYVOVjUCu8YKsqnsfk",
        "source": "app"
    },
    "status": true
}
```

可以通过本API修改用户的信息，允许用户更改昵称等。

### 请求

`POST /customer/update_information`

### 请求参数

参数 | 类型 | 可选 | 描述
--------- | ------- | ----------- | -----------
nickname | String | 可选 | 昵称
avatar | String | 可选 | 头像

### 返回参数

参数 | 类型 | 可选 | 描述
--------- | ------- | ----------- | -----------
data | Object | - | 更新后的用户数据

<aside class="notice">本API可配合上传文件API实现更换头像。</aside>

# @Aftershop: 消费者上传文件

> 请求数据

```form
Form数据
files: List
bcid: String
_auth: String
```

> 返回JSON数据

```json
{
    "data": [
      {
          "freezed" : false,
          "createdBy" : "kWsLwpKF1Uw17Su99l4d",
          "createdAt" : 1602328489.64209,
          "filePath" : "/Users/mazhenhao/Documents/localhost_file_storage/zizoa-W-5-soccer_1628_urls.csv",
          "fileUrl" : "/Users/mazhenhao/Documents/localhost_file_storage/zizoa-W-5-soccer_1628_urls.csv",
          "prefix" : "/Users/mazhenhao/Documents/localhost_file_storage/",
          "server" : "zhmamac",
          "fid" : "686405a6-33a5-45ee-9cd7-85af8632a55c",
          "uid" : "kWsLwpKF1Uw17Su99l4d",
          "wxid" : "",
          "customerId": "",
          "originalFileName" : "W-5-soccer_1628_urls.csv",
          "source" : "",
          "ready" : true,
          "export" : false,
          "contentType" : "text/csv"
      }
    ],
    "status": true
}
```

本API还未经过严格测试，不确定能正确解析Form表单中的`_auth`和`bcid`。

### 请求

`POST /customer/upload_file`

### 请求参数

参数 | 类型 | 可选 | 描述
--------- | ------- | ----------- | -----------
files | Array | - | 文件列表
bcid | String | - | 标准bcid，以表内参数形式
_auth | String | - | 标准_auth，以表内参数形式

### 返回参数

参数 | 类型 | 可选 | 描述
--------- | ------- | ----------- | -----------
files | Array | - | 上传后的文件列表，含`fileUrl`文件链接。


# @Aftershop: 消费者获取全部售后订单

> 请求数据

```json
{
    "deviceIdentifier": "2342asdfasdf3452345",
    "deviceName": "postman",
    "bcid": "2877383e-5551-4708-862b-a0827d294d73",
    "_auth": "ef136896b85dfd337540c7cc26cb2257",
    "page": 0,
    "pageSize": 1000,
    "filter": {},
    "source": "app"
}
```


> 返回JSON数据

```json
{
    "status": true,
    "data": {
      "warranties": [
          {
              "freezed" : false,
              "createdAt" : 1607832629.966,
              "createdBy" : "3TnctrAsdF-fVhLYyBUN",
              "warrantyId" : "9209b9ae-77c4-4958-9e4a-2c17ab048786",
              "registerFrom" : "4ec676eb-6425-445a-8c62-d32083b2bba2",
              "registerChannel" : "jsSnippet",
              "bcid" : "2877383e-5551-4708-862b-a0827d294d73",
              "clientId" : "076e3378-a3f4-441e-a998-4a89ee894d83",
              "customerId" : "3TnctrAsdF-fVhLYyBUN",
              "orderNumber" : "wow123",
              "salesChannel" : "amazon"
          }
      ]
    }
}
```

### 请求

`POST /customer/get_all_warranties`

### 请求参数

参数 | 类型 | 可选 | 描述
--------- | ------- | ----------- | -----------
page | Int | - | 第几页
pageSize | Int | - | 最多返回多少条
filter | Object | - | 额外的筛选参数，推荐默认为 `{}`

### 返回参数

参数 | 类型 | 可选 | 描述
--------- | ------- | ----------- | -----------
warranties | Array | - | 所有注册后的售后订单


# @Aftershop: 消费者注册售后订单

> 请求数据

```json
{
  "deviceIdentifier": "postman",
  "bcid": "2877383e-5551-4708-862b-a0827d294d73",
  "deviceName": "bob mac",
  "source": "jsSnippet",
  "_auth": "160asdfadsfasd123f123a12s3d12a3s123dfa12323123Z",

  "registerFrom": "2877383e-5551-4708-862b-a0827d294d73",
  "orderNumber": "028-8130362-2901161",
  "salesChannel": "amazon",
  "reviewChallenged": true,
  "rated": true,
  "review": "aspodjf oapisjf oiasjdfop jasopdf jpaosd",
  "rating": 4,
  "giftProductId": "94f8ce93-069d-4417-9874-6e734f52add7",
  "giftImage": "https://m.xuggest.com/upload/static/2021/1/4/uhulh-Amazon.png",
  "giftTitle": "$5 Amazon Gift Card",
  "giftUrl": "https://amazon.com/asd/asdf/asdf/123123",
  "giftRecipientName": "my name",
  "giftRecipientAddress": "asdfpoajsiodpf\nasopijf oipasjdfp\nasdfojasoidf",
  "giftRecipientPhone": "1101019182"
}
```


> 返回JSON数据

```json
{
    "status": true,
    "data": {
              "freezed" : false,
              "createdAt" : 1607832629.966,
              "createdBy" : "3TnctrAsdF-fVhLYyBUN",
              "warrantyId" : "9209b9ae-77c4-4958-9e4a-2c17ab048786",
              "registerFrom" : "4ec676eb-6425-445a-8c62-d32083b2bba2",
              "registerChannel" : "jsSnippet",
              "bcid" : "2877383e-5551-4708-862b-a0827d294d73",
              "clientId" : "076e3378-a3f4-441e-a998-4a89ee894d83",
              "customerId" : "3TnctrAsdF-fVhLYyBUN",
              "orderNumber" : "wow123",
              "salesChannel" : "amazon"
          }
}
```

### 请求

`POST /customer/register_warranty`

### 请求参数

参数 | 类型 | 可选 | 描述
--------- | ------- | ----------- | -----------
registerFrom | String | - | 如果是从`app`中注册的售后，提供这个`appId`，否则如果从web，那么就提交`jsSnippetId`
orderNumber | String | - | 订单号码（和amazonOrderId一致）
reviewChallenged | Bool | - | 是否有尝试邀评
rated | Bool | - | 是否有确认已经留下了评价
review | String | - | 文字评价
rating | Float | - | 邀评后站内留下的星级
giftProductId | String | - | 所选礼物产品Id
giftImage | String | - | 所选礼物图片
giftTitle | String | - | 所选礼物标题
giftUrl | String | - | 所选礼物链接
giftRecipientName | String | - | 礼物邮寄收件人
giftRecipientAddress | String | - | 礼物邮寄地址
giftRecipientPhone | String | - | 礼物收件手机


### 返回参数

参数 | 类型 | 可选 | 描述
--------- | ------- | ----------- | -----------
data | Object | - | 售后对象
