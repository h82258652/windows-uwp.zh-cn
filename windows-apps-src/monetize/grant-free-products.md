---
author: mcleanbyron
ms.assetid: FA55C65C-584A-4B9B-8451-E9C659882EDE
description: "在 Windows 应用商店购买 API 中使用此方法，以向给定用户授予免费应用或应用内产品 (IAP)。"
title: "授予免费产品"
ms.sourcegitcommit: 2f4351d6f9bdc0b9a131ad5ead10ffba7e76c437
ms.openlocfilehash: 9bce5649fc1a9400371e1f9bb67809f1c6288ec6

---

# 授予免费产品

\[ 已针对 Windows 10 上的 UWP 应用更新。 有关 Windows 8.x 文章，请参阅[存档](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

在 Windows 应用商店购买 API 中使用此方法，以向给定用户授予免费应用或应用内产品 (IAP)。

当前，你仅可以授予免费产品。 如果你的服务尝试使用此方法授予付费产品，此方法会返回一个错误。

## 先决条件

若要使用此方法，你需要：

-   使用 `https://onestore.microsoft.com` 受众 URI 创建的 Azure AD 访问令牌。
-   通过在应用中从客户端代码调用 [**GetCustomerPurchaseIdAsync**](https://msdn.microsoft.com/library/windows/apps/mt608675) 方法生成的 Windows 应用商店 ID 密钥。

有关详细信息，请参阅[从服务查看和授予产品](view-and-grant-products-from-a-service.md)。

## 请求


### 请求语法

| 方法 | 请求 URI                                            |
|--------|--------------------------------------------------------|
| POST   | `https://purchase.mp.microsoft.com/v6.0/purchases/grant` |

<br/> 

### 请求标头

| 标头         | 类型   | 说明                                                                                           |
|----------------|--------|-------------------------------------------------------------------------------------------------------|
| 授权  | 字符串 | 必需。 Azure AD 访问令牌的格式为 **Bearer**&lt;*token*&gt;。                           |
| Host           | 字符串 | 必须设置为值 **collections.mp.microsoft.com**。                                            |
| Content-Length | 数字 | 请求正文的长度。                                                                       |
| Content-Type   | 字符串 | 指定请求和响应类型。 当前，唯一受支持的值为 **application/json**。 |

<br/>

### 请求正文

| 参数      | 类型   | 说明                                                                                                                                                                                                                                                                                                            | 必需 |
|----------------|--------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------|
| availabilityId | 字符串 | 从 Windows 应用商店目录中购买的产品的可用性 ID。                                                                                                                                                                                                                                     | 是      |
| b2bKey         | 字符串 | 表示客户标识的 Windows 应用商店 ID 密钥。                                                                                                                                                                                                                                                        | 是      |
| devOfferId     | 字符串 | 购买后显示在“集合”项中的开发人员指定的优惠 ID。                                                                                                                                                                                                                                 | 否       |
| language       | 字符串 | 用户的语言。                                                                                                                                                                                                                                                                                              | 是      |
| market         | 字符串 | 用户的市场。                                                                                                                                                                                                                                                                                                | 是      |
| orderId        | GUID   | 为订单生成的 GUID。 此值对用户而言是唯一的，但不 要求对所有订单都唯一。                                                                                                                                                                                              | 是      |
| productId      | 字符串 | Windows 应用商店目录中的存储 ID。 存储 ID 在开发人员中心仪表板的[应用标识页](../publish/view-app-identity-details.md)上提供。 存储 ID 的一个示例是 9WZDNCRFJ3Q8。 | 是      |
| quantity       | int    | 要购买的数量。 当前，唯一受支持的值为 1。 如果未指定，默认值为 1。                                                                                                                                                                                                                | 否       |
| skuId          | 字符串 | Windows 应用商店目录中的 SKU ID。 SKU ID 的一个示例为“0010”。                                                                                                                                                                                                                                                | 是      |

<br/> 

### 请求示例

```syntax
POST https://purchase.mp.microsoft.com/v6.0/purchases/grant HTTP/1.1
Authorization: Bearer eyJ0eXAiOiJK……
Content-Length: 1863
Content-Type: application/json

{
    "b2bKey" : "eyJ0eXAiOiJK……",
    "availabilityId" : "9RT7C09D5J3W",
    "productId" : "9NBLGGH5WVP6",
    "skuId" : "0010",
    "language" : "en-us",
    "market" : "us",
    "orderId" : "3eea1529-611e-4aee-915c-345494e4ee76",
}
```

## 响应


### 响应正文

| 参数                 | 类型                        | 说明                                                                                                                                              | 必需 |
|---------------------------|-----------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------|----------|
| clientContext             | ClientContextV6             | 此订单的客户端上下文信息。 这将分配到 Azure AD 中的 *clientID* 值。                                     | 是      |
| createdtime               | datetimeoffset              | 创建订单的时间。                                                                                                                          | 是      |
| currencyCode              | 字符串                      | *totalAmount* 和 *totalTaxAmount* 的货币代码。 不适用于免费项目。                                                                                | 是      |
| friendlyName              | 字符串                      | 订单的友好名称。 不适用于使用 Windows 应用商店购买 API 生成的订单。                                                               | 是      |
| isPIRequired              | 布尔型                     | 指示付款方式 (PI) 是否需要作为 购买订单的一部分。                                                                   | 是      |
| language                  | 字符串                      | 订单的语言 ID（例如“en”）。                                                                                                       | 是      |
| market                    | 字符串                      | 订单的市场 ID（例如“US”）。                                                                                                         | 是      |
| orderId                   | 字符串                      | 标识特定用户的订单的 ID。                                                                                                   | 是      |
| orderLineItems            | list&lt;OrderLineItemV6&gt; | 订单的行项列表。 每个订单通常有 1 个行项。                                                                          | 是      |
| orderState                | 字符串                      | 订单的状态。 有效状态为 **Editing**、**CheckingOut**、**Pending**、**Purchased**、**Refunded**、**ChargedBack** 和 **Cancelled**。 | 是      |
| orderValidityEndTime      | 字符串                      | 在提交前，订单定价 有效期的结束时间。 不适用于免费应用。                                                                      | 是      |
| orderValidityStartTime    | 字符串                      | 在提交前，订单定价 有效期的开始时间。 不适用于免费应用。                                                                     | 是      |
| 购买者                 | IdentityV6                  | 描述购买者标识的对象。                                                                                                  | 是      |
| totalAmount               | 十进制                     | 订单中所有项的总购买额（含税）。                                                                                       | 是      |
| totalAmountBeforeTax      | 十进制                     | 订单中所有项的总购买额（税前）。                                                                                              | 是      |
| totalChargedToCsvTopOffPI | 十进制                     | 如果使用单独的付款方式 (PI) 和存储 值 (CSV)，金额将从 CSV 中扣除。                                                                | 是      |
| totalTaxAmount            | 十进制                     | 所有行项的税款总额。                                                                                                              | 是      |

<br/> 

ClientContext 对象包含以下参数。

| 参数 | 类型   | 说明                           | 必需 |
|-----------|--------|---------------------------------------|----------|
| client    | 字符串 | 创建订单的客户端 ID。 | 否       |

<br/> 

OrderLineItemV6 对象包含以下参数。

| 参数               | 类型           | 说明                                                                                                  | 必需 |
|-------------------------|----------------|--------------------------------------------------------------------------------------------------------------|----------|
| agent                   | IdentityV6     | 最后编辑行项的代理。 有关此对象的详细信息，请参阅下表。       | 否       |
| availabilityId          | 字符串         | 从 Windows 应用商店目录中购买的产品的可用性 ID。                           | 是      |
| 受益人             | IdentityV6     | 订单的受益人标识。                                                                | 否       |
| billingState            | 字符串         | 订单的帐单状态。 在完成时，此值设置为 **Charged**。                                   | 否       |
| campaignId              | 字符串         | 此订单的市场活动 ID。                                                                              | 否       |
| currencyCode            | 字符串         | 用于显示价格明细的货币代码。                                                                    | 是      |
| 说明             | 字符串         | 行项的本地化说明。                                                                    | 是      |
| devofferId              | 字符串         | 特定订单的优惠 ID（如果存在）。                                                           | 否       |
| fulfillmentDate         | datetimeoffset | 完成日期。                                                                           | 否       |
| fulfillmentState        | 字符串         | 此项的完成状态。 在完成时，此值设置为 **Fulfilled**。                      | 否       |
| isPIRequired            | 布尔型        | 指示此行项是否需要 付款方式。                                       | 是      |
| isTaxIncluded           | 布尔型        | 指示税款是否包括在项目的价格明细中。                                        | 是      |
| legacyBillingOrderId    | 字符串         | 传统的帐单 ID。                                                                                       | 否       |
| lineItemId              | 字符串         | 此订单中项目的行项 ID。                                                                 | 是      |
| listPrice               | 十进制        | 此订单中项目的价目表。                                                                    | 是      |
| productId               | 字符串         | 行项的 Windows 应用商店产品 ID。                                                               | 是      |
| productType             | 字符串         | 产品的类型。 支持的值包括 **Durable**、**Application** 和 **UnmanagedConsumable**。 | 是      |
| Quantity                | int            | 订购项的数量。                                                                            | 是      |
| retailPrice             | 十进制        | 订购项的零售价格。                                                                        | 是      |
| revenueRecognitionState | 字符串         | 收入识别状态。                                                                               | 是      |
| skuId                   | 字符串         | 行项的 Windows 应用商店 SKU ID。                                                                   | 是      |
| taxAmount               | 十进制        | 行项的税额。                                                                            | 是      |
| taxType                 | 字符串         | 适用税款的税款类型。                                                                       | 是      |
| Title                   | 字符串         | 行项的本地化标题。                                                                        | 是      |
| totalAmount             | 十进制        | 行项的总购买额（含税）。                                                    | 是      |

<br/> 

IdentityV6 对象包含以下参数。

| 参数     | 类型   | 说明                                                                        | 必需 |
|---------------|--------|------------------------------------------------------------------------------------|----------|
| IdentityType  | 字符串 | 包含值**“pub”**。                                                      | 是      |
| identityValue | 字符串 | 指定的 Windows 应用商店 ID 密钥的 *publisherUserId* 字符串值。 | 是      |

<br/> 

### 响应示例

```syntax
Content-Length: 1203
Content-Type: application/json
MS-CorrelationId: fb2e69bc-f26a-4aab-a823-7586c19f5762
MS-RequestId: c1bc832c-f742-47e4-a76c-cf061402f698
MS-CV: XjfuNWLQlEuxj6Mt.8
MS-ServerId: 030032362
Date: Tue, 13 Oct 2015 21:21:51 GMT

{
    "clientContext": {
        "client": "86b78998-d05a-487b-b380-6c738f6553ea"
    },
    "createdTime": "2015-10-13T21:21:51.1863494+00:00",
    "currencyCode": "USD",
    "isPIRequired": false,
    "language": "en-us",
    "market": "us",
    "orderId": "3eea1529-611e-4aee-915c-345494e4ee76",
    "orderLineItems": [{
        "availabilityId": "9RT7C09D5J3W",
        "beneficiary": {
            "identityType": "pub",
            "identityValue": "user1"
        },
        "billingState": "Charged",
        "currencyCode": "USD",
        "description": "Jewels, Jewels, Jewels - Consumable 2",
        "fulfillmentDate": "2015-10-13T21:21:51.639478+00:00",
        "fulfillmentState": "Fulfilled",
        "isPIRequired": false,
        "isTaxIncluded": true,
        "lineItemId": "2814d758-3ee3-46b3-9671-4fb3bdae9ffe",
        "listPrice": 0.0,
        "payments": [],
        "productId": "9NBLGGH5WVP6",
        "productType": "UnmanagedConsumable",
        "quantity": 1,
        "retailPrice": 0.0,
        "revenueRecognitionState": "None",
        "skuId": "0010",
        "taxAmount": 0.0,
        "taxType": "NoApplicableTaxes",
        "title": "Jewels, Jewels, Jewels - Consumable 2",
        "totalAmount": 0.0
    }],
    "orderState": "Purchased",
    "orderValidityEndTime": "2015-10-14T21:21:51.1863494+00:00",
    "orderValidityStartTime": "2015-10-13T21:21:51.1863494+00:00",
    "purchaser": {
        "identityType": "pub",
        "identityValue": "user1"
    },
    "testScenarios": "None",
    "totalAmount": 0.0,
    "totalTaxAmount": 0.0
}
```

## 错误代码


| 代码 | 错误        | 内部错误代码           | 描述                                                                                                                                                                           |
|------|--------------|----------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 401  | 未授权 | AuthenticationTokenInvalid | Azure AD 访问令牌无效。 在某些情况下，ServiceError 的详细信息包含更多信息，例如令牌到期或 *appid* 声明丢失的时间。 |
| 401  | 未授权 | PartnerAadTicketRequired   | 在授权标头中，Azure AD 访问令牌不会 传递到服务。                                                                                                   |
| 401  | 未授权 | InconsistentClientId       | 请求正文的 Windows 应用商店 ID 密钥中的 *clientId* 声明与授权标头的 Azure AD 访问令牌中的 *appid* 声明不匹配。                     |
| 400  | BadRequest   | InvalidParameter           | 详细信息包含有关请求正文和具有无效值的字段的信息。                                                                                    |

<br/> 

## 相关主题


* [从服务查看和授予产品](view-and-grant-products-from-a-service.md)
* [查询产品](query-for-products.md)
* [将可消费产品报告为已完成。](report-consumable-products-as-fulfilled.md)
* [续订 Windows 应用商店 ID 密钥](renew-a-windows-store-id-key.md)
 

 



<!--HONumber=Jun16_HO5-->


