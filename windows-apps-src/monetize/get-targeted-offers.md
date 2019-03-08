---
ms.assetid: A4C6098B-6CB9-4FAF-B2EA-50B03D027FF1
description: 在 Microsoft Store 定向优惠 API 中使用此方法来获取为当前应用上下文中的当前用户提供的定向优惠。
title: 获取定向优惠
ms.date: 10/10/2017
ms.topic: article
keywords: windows 10, uwp, Microsoft Store 服务, Microsoft Store 定向优惠 API, 获取定向优惠
ms.localizationpriority: medium
ms.openlocfilehash: 71cd6ce3b9736b812f8ccdf4d21d35357928c63c
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2019
ms.locfileid: "57622762"
---
# <a name="get-targeted-offers"></a>获取定向优惠

使用此方法来获取为当前用户提供的定向优惠，具体取决于用户是否属于该定向优惠所针对的客户细分的一部分。 有关更多信息，请参阅[使用应用商店服务管理定向优惠](manage-targeted-offers-using-windows-store-services.md)。

## <a name="prerequisites"></a>必备条件

若要使用此方法，你需要先为当前已登录应用的用户[获取 Microsoft 帐户令牌](manage-targeted-offers-using-windows-store-services.md#obtain-a-microsoft-account-token)。 对于此方法，你必须将此令牌传递至 ```Authorization``` 请求标头中。 此令牌供 Microsoft Store 用于为当前用户提供定向优惠。

## <a name="request"></a>请求


### <a name="request-syntax"></a>请求语法

| 方法 | 请求 URI                                                                |
|--------|----------------------------------------------------------------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v2.0/my/storeoffers/user``` |


### <a name="request-header"></a>请求头

| 标头        | 在任务栏的搜索框中键入   | 描述  |
|---------------|--------|--------------|
| 授权 | 字符串 | 必需。 在窗体应用程序的当前登录的用户的 Microsoft 帐户令牌**持有者** &lt;*令牌*&gt;。 |


### <a name="request-parameters"></a>请求参数

此方法没有 UI 或查询参数。

### <a name="request-example"></a>请求示例

```syntax
GET https://manage.devcenter.microsoft.com/v2.0/my/storeoffers/user HTTP/1.1
Authorization: Bearer <Microsoft Account token>
```

## <a name="response"></a>响应

此方法将会返回一个 JSON 格式的响应正文，其中包含一个对象数组及以下字段。 数组中的每个对象代表为指定用户（特定客户细分的一部分）提供的定向优惠。

| 字段      | 在任务栏的搜索框中键入   | 描述         |
|------------|--------|------------------|
| offers      | 数组  | 与针对当前用户提供的定向优惠关联的加载项产品 ID 数组。 中指定这些产品 Id**针对产品/服务**在合作伙伴中心应用程序页。            |
| trackingId  | 字符串 | 一个 GUID，可在你自己的代码或服务中选择用来跟踪定向优惠。 |


### <a name="example"></a>示例

以下示例举例说明此请求的 JSON 响应正文。

```json
[
  {
    "offers": [
      "10x gold coins",
      "100x gold coins"
    ],
    "trackingId": "5de5dd29-6dce-4e68-b45e-d8ee6c2cd203"
  }
]
```

## <a name="related-topics"></a>相关主题

* [管理使用存储服务的目标产品/服务](manage-targeted-offers-using-windows-store-services.md)

 

 
