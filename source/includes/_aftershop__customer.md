### Aftershop - 消费者customer API标准

在消费者customer的API中，需要进行授权，一般在Post请求的Json数据中添加参数_auth，或者是Form请求中添加Form键_auth。
下面参数是应该通过拦截器interceptor的方式的形式，让每次请求都自动携带_auth和bcid至API服务器。

参数 | 类型 | 描述 | 携带场景
--------- | ------- | ----------- | -----------
bcid | String | 品牌类目ID | 必须提交
_auth | String | 登陆凭证 | 已登陆用户应该每次请求都提交_auth参数给API
deviceIdentifier | String | 设备ID | 必须提交

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
    return resp['data']
```

> 这是一个基础调用function

开发需要把调用API封装成方法/类，这样能更好的处理异常以及未来的版本迭代修改等。
最值得注意的是，返回时需要检查状态码和`status`参数，然后及时把错误提示显示给用户。

# @Aftershop: 消费者注册

> 请求数据

```json
{
    "email": "bob@gmail.com",
    "password": "123456",
    "source": "app",
    "deviceIdentifier": "postman",
    "bcid": "2877383e-5551-4708-862b-a0827d294d73"
}
```

> 返回JSON数据

```json
{
    "data": {
        "_auth": "3cc6edf3dce411926ee479e677767a29",
        "customer": {
            "avatar": "",
            "bcid": "2871383e-5551-4708-862b-a0827d294d73",
            "clientId": "176e3378-a3f4-441e-a998-4a89ee894d83",
            "createdAt": 0.0,
            "customerId": "awYcbRq2fXnpnesNSfJ1",
            "email": "bob@gmail.com",
            "emailVerified": false,
            "freezed": false,
            "history": {},
            "nickname": "Customer",
            "passwordHashed": "$pbkdf2-sha256$29011$JYTQ3vvfe4.xNqZ0Tsn5Pw$aoiOoi3RzqgorbkGMJuyO4kQlnG3m4VHbIwzYdw612c",
            "source": "app"
        }
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

### 返回参数

参数 | 类型 | 可选 | 描述
--------- | ------- | ----------- | -----------
data | Object | - | 用户数据

# @Aftershop: 消费者登录

> 请求数据

```json
{
    "email": "bob@gmail.com",
    "password": "123456",
    "deviceIdentifier": "postman",
    "bcid": "2877383e-5551-4708-862b-a0827d294d73"
}
```

> 返回JSON数据

```json
{
    "data": "b2b9b89d8039da5c4d42434a0f1de4b7",
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
data | Object | - | _auth 登录凭证

# @Aftershop: 发送验证邮箱的链接至消费者邮箱里

> 请求数据

```json
{
    "email": "bob@gmail.com",
    "deviceIdentifier": "postman",
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

发送验证邮箱所需要的验证码`verifyCode`到邮箱，用户可以通过这个验证码，实现验证邮箱的功能。

### 请求

`POST /customer/send_email_verification`

### 请求参数

参数 | 类型 | 可选 | 描述
--------- | ------- | ----------- | -----------
email | String | - | 邮箱

<aside class="notice">
每个用户的邮箱默认是未被验证的，我们在注册流程中并没有强调验证用户邮箱。所以我们后续会在必要的时候要求验证用户的邮箱。而你必须要使用API返回的<code>status</code>来帮助判断是否发件成功。用户会收到验证邮箱所需要的验证码，可以提交到平台上。
</aside>

# @Aftershop: 发送重置密码的验证码至消费者邮箱里

> 请求数据

```json
{
    "email": "bob@gmail.com",
    "deviceIdentifier": "postman",
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

# @Aftershop: 检查邮箱是否已被注册

> 请求数据

```json
{
    "email": "bob@gmail.com",
    "deviceIdentifier": "postman",
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
    "deviceIdentifier": "postman",
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
    "deviceIdentifier": "postman",
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
    "deviceIdentifier": "postman",
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
data | Array | - | 上传后的文件列表，含`fileUrl`文件链接。
