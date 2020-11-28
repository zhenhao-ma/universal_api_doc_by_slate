---
title: API Reference

language_tabs: # must be one of https://git.io/vQNgJ
  - python

toc_footers:
  - <a href='https://xuggest.com' target="_blank">Project Official Website</a>
  - <a href='https://github.com/slatedocs/slate/wiki/Markdown-Syntax'  target="_blank">Documentation editing standard</a>
  - <a href='https://github.com/slatedocs/slate/wiki/Using-Slate-Natively'  target="_blank">Deploy this api project</a>
  - <a href='https://xmgseo.com'  target="_blank">文档由马振豪维护</a>

includes:
  - errors

search: true

code_clipboard: true
---

# 介绍

这个是Xuggest的API文档，涵盖了Xuggest旗下大部分的产品以及项目。

1. xuggest.com
2. aftershopapp.com 和 aftershop.app
3. m.xuggest.com
4. and a time table project will be released later on.

目前规划是xuggest.com和aftershopapp.com作为中文版官网。
而xuggest.net和aftershop.app将作为全球英语官网。

# API 标准

所有API的设计严格遵守Restful API的标准。

### 请求标准

基础URL一般为：
1. xuggest.com/api/
2. m.xuggest.com/api/
3. aftershop.app/api/
3. aftershopapp.com/api/

全部API都将以`/api/`为开头。请求时，必须要以`POST`请求。请求数据将会以`Json`为格式，除了文件上传API将会使用 `Form` 数据格式。

### 返回标准

全部API都将以Json的格式返回数据。
Json标准返回是：

1. `status`: Boolean
2. `data`: 请求数据，可以是任何数据类型。只有当status === true才会提供。
3. `msg`: String。只有当status === false才会提供。提示错误的信息，可以直接显示给用户看。

# 设计标准

### 通用参数

后端以及数据库框架内，有部分属性是通用的。

参数 | 类型 | 描述
--------- | ------- | -----------
created_by | String | 由哪个用户ID创建的，用户可以是管理员/消费者/客户等
created_at | Int | 什么时候创建的
freezed | Boolean | 是否已经被隐藏

### Aftershop项目设计标准

Xuggest下的Aftershop项目中常见参数。一般来说，1个客户会有多个品牌类目。所以1个`clientId`下会有多个`bcid`。而每个`bcid`会单独作为一个项目进行。也可以把品牌类目理解为客户的项目Project。

参数 | 类型 | 描述
--------- | ------- | -----------
clientId | String | 客户ID（购买了aftershop app的客户）
bcid | String | 品牌类目ID

<aside class="notice">
而每个品牌类目<code>bcid</code>，将会对应不同的App，嵌入式JS，消费者群等。
</aside>

系统中统一术语是：

1. 客户：client，也就是购买了我们产品的企业。
2. 客户用户：clientUser，俗称User，也就是够买了我们产品的企业的人员。
3. 消费者：customer，也就是购买了客户企业的产品的消费者，一般为海外用户。
4. CRM：我们的提供给客户使用的后台系统，用于管理，营销运营，以及编辑设置。
5. App：终端App，客户链接消费者的渠道。
6. Jss 或 JsSnippet：嵌入式Javascript，类似于Pixel，但加强了功能。客户可以通过Jss增强独立站功能。

### Aftershop - 消费者customer API标准

在消费者customer的API中，需要进行授权，一般在Post请求的Json数据中添加参数_auth，或者是Form请求中添加Form键_auth。
下面参数是应该通过拦截器interceptor的方式的形式，让每次请求都自动携带_auth和bcid至API服务器。

参数 | 类型 | 描述 | 携带场景
--------- | ------- | ----------- | -----------
bcid | String | 品牌类目ID | 任何时候，一个bcid对应一个被激活的app/jss
_auth | String | 登陆凭证 | 已登陆用户应该每次请求都提交_auth参数给API

而API特殊状态码处理

状态码 | 描述 | 建议处理方式
--------- | ------- | ------- 
200 | 正常 | 无需处理
401 | 未登陆 | 弹出登陆窗口
403 | 登陆了，但邮箱未验证 | 弹出邮箱验证窗口

#API调用

开发需要把调用API封装成方法/类，这样能更好的处理异常以及未来的版本迭代修改等。
最值得注意的是，返回时需要检查状态码和`status`参数，然后及时把错误提示显示给用户。

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


# @Aftershop: 消费者注册

### 请求

`POST /customer/register`

### 请求参数

参数 | 类型 | 可选 | 描述
--------- | ------- | ----------- | -----------
email | String | - | 邮箱
password | String | - | 密码
source | String | - | 注册者来源，`app`或`website`
deviceIdentifier | String | - | 设备ID，一般为手机ID或浏览器UA

### 返回参数

参数 | 类型 | 可选 | 描述
--------- | ------- | ----------- | -----------
data | Object | - | 用户数据

> 请求数据

```json
{
    "email": "bob@gmail.com",
    "password": "Temp1234566",
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


Kittn uses API keys to allow access to the API. You can register a new Kittn API key at our [developer portal](http://example.com/developers).

Kittn expects for the API key to be included in all API requests to the server in a header that looks like the following:

`Authorization: meowmeowmeow`

<aside class="notice">
You must replace <code>meowmeowmeow</code> with your personal API key.
</aside>

# Kittens

## Get All Kittens

```ruby
require 'kittn'

api = Kittn::APIClient.authorize!('meowmeowmeow')
api.kittens.get
```

```python
import kittn

api = kittn.authorize('meowmeowmeow')
api.kittens.get()
```

```shell
curl "http://example.com/api/kittens" \
  -H "Authorization: meowmeowmeow"
```

```javascript
const kittn = require('kittn');

let api = kittn.authorize('meowmeowmeow');
let kittens = api.kittens.get();
```

> The above command returns JSON structured like this:

```json
[
  {
    "id": 1,
    "name": "Fluffums",
    "breed": "calico",
    "fluffiness": 6,
    "cuteness": 7
  },
  {
    "id": 2,
    "name": "Max",
    "breed": "unknown",
    "fluffiness": 5,
    "cuteness": 10
  }
]
```

This endpoint retrieves all kittens.

### HTTP Request

`GET http://example.com/api/kittens`

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
include_cats | false | If set to true, the result will also include cats.
available | true | If set to false, the result will include kittens that have already been adopted.

<aside class="success">
Remember — a happy kitten is an authenticated kitten!
</aside>

## Get a Specific Kitten

```ruby
require 'kittn'

api = Kittn::APIClient.authorize!('meowmeowmeow')
api.kittens.get(2)
```

```python
import kittn

api = kittn.authorize('meowmeowmeow')
api.kittens.get(2)
```

```shell
curl "http://example.com/api/kittens/2" \
  -H "Authorization: meowmeowmeow"
```

```javascript
const kittn = require('kittn');

let api = kittn.authorize('meowmeowmeow');
let max = api.kittens.get(2);
```

> The above command returns JSON structured like this:

```json
{
  "id": 2,
  "name": "Max",
  "breed": "unknown",
  "fluffiness": 5,
  "cuteness": 10
}
```

This endpoint retrieves a specific kitten.

<aside class="warning">Inside HTML code blocks like this one, you can't use Markdown, so use <code>&lt;code&gt;</code> blocks to denote code.</aside>

### HTTP Request

`GET http://example.com/kittens/<ID>`

### URL Parameters

Parameter | Description
--------- | -----------
ID | The ID of the kitten to retrieve

## Delete a Specific Kitten

```ruby
require 'kittn'

api = Kittn::APIClient.authorize!('meowmeowmeow')
api.kittens.delete(2)
```

```python
import kittn

api = kittn.authorize('meowmeowmeow')
api.kittens.delete(2)
```

```shell
curl "http://example.com/api/kittens/2" \
  -X DELETE \
  -H "Authorization: meowmeowmeow"
```

```javascript
const kittn = require('kittn');

let api = kittn.authorize('meowmeowmeow');
let max = api.kittens.delete(2);
```

> The above command returns JSON structured like this:

```json
{
  "id": 2,
  "deleted" : ":("
}
```

This endpoint deletes a specific kitten.

### HTTP Request

`DELETE http://example.com/kittens/<ID>`

### URL Parameters

Parameter | Description
--------- | -----------
ID | The ID of the kitten to delete

