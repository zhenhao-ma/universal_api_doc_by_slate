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
  - update
  - errors
  - aftershop__customer
  - aftershop__chat

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
bcid | String | 品牌类目ID | 必须提交
_auth | String | 登陆凭证 | 已登陆用户应该每次请求都提交_auth参数给API
deviceIdentifier | String | 设备ID | 必须提交
deviceName | String | 设备名称 | 必须提交
