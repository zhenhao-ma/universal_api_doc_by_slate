# @Aftershop: 消费者验证订单

> 请求数据

```json
{
    "deviceIdentifier": "postman",
    "bcid": "2877383e-5551-4708-862b-a0827d294d73",
    "deviceName": "bob mac",
    "source": "jsSnippet",
    "_auth": "160asdfadsfasd123f123a12s3d12a3s123dfa12323123Z",
    "amazonOrderId": "028-8130362-2901161",
    "redeemCode": "12345"
}
```

> 返回JSON数据

```json
{
    "data": {
        "amazonOrderId": "028-8130362-2901161",
        "asin": "B016CDK113",
        "asinTitle": "3D Ultraschall Glas 7 Farben LED Diffusoren für ätherische Öle, Aroma Diffuser Luftbefeuchter, Vernebler Duftlampe Öle Diffusor mit Wasserlose",
        "clientId": "076e3378-a3f4-441e-a998-4a89ee894d83",
        "createdAt": 1610022048.528844,
        "createdBy": "fatSs5w_-pluAiGT3Wnu",
        "freezed": false,
        "fulfillmentChannel": "AFN",
        "giftWrapPrice": 0.0,
        "giftWrapTax": 0.0,
        "itemPrice": 0.0,
        "itemPromotionDiscount": 0.0,
        "itemTax": 0.0,
        "lastModifiedBy": "fatSs5w_-pluAiGT3Wnu",
        "lastUpdatedDate": 1602265690.0,
        "merchantOrderId": "028-8630362-2901160",
        "orderId": "1195f274-e3e6-4fbe-a381-b8555d4e7d72",
        "orderStatus": "Shipped",
        "purchaseDate": 1602241848.0,
        "quantity": 2,
        "salesChannel": "Amazon.de",
        "shipPromotionDiscount": 0.0,
        "shipServiceLevel": "Expedited",
        "shippingPrice": 0.0,
        "shippingTax": 0.0,
        "storeId": "07ace7aa-b583-48eb-9dc2-0cbc5b760516"
    },
    "status": true
}
```

### 请求

`POST /public/other/validate_amazon_order`

### 请求参数

参数 | 类型 | 可选 | 描述
--------- | ------- | ----------- | -----------
amazonOrderId | String | - | 亚马逊订单号
redeemCode | String | - | 兑换码

### 返回参数

参数 | 类型 | 可选 | 描述
--------- | ------- | ----------- | -----------
amazonOrderId | String | - | 亚马逊订单号
asin | String | - | 产品ID（可用于跳转亚马逊url或App）
asinTitle | String | - | 产品标题
fulfillmentChannel | String | - | 配送方式
orderStatus | String | - | 订单状态
