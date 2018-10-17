---
author: Xansky
ms.assetid: A4C6098B-6CB9-4FAF-B2EA-50B03D027FF1
description: 在 Microsoft Store 定向优惠 API 中使用此方法来获取为当前应用上下文中的当前用户提供的定向优惠。
title: 获取定向优惠
ms.author: mhopkins
ms.date: 10/10/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, Microsoft Store 服务, Microsoft Store 定向优惠 API, 获取定向优惠
ms.localizationpriority: medium
ms.openlocfilehash: 1032831492443460bd63671012a09edfceca2690
ms.sourcegitcommit: 1c6325aa572868b789fcdd2efc9203f67a83872a
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/17/2018
ms.locfileid: "4755988"
---
# <a name="get-targeted-offers"></a>获取定向优惠

使用此方法来获取为当前用户提供的定向优惠，具体取决于用户是否属于该定向优惠所针对的客户细分的一部分。 有关更多信息，请参阅[使用应用商店服务管理定向优惠](manage-targeted-offers-using-windows-store-services.md)。

## <a name="prerequisites"></a>先决条件

若要使用此方法，你需要先为当前已登录应用的用户[获取 Microsoft 帐户令牌](manage-targeted-offers-using-windows-store-services.md#obtain-a-microsoft-account-token)。 对于此方法，你必须将此令牌传递至 ```Authorization``` 请求标头中。 此令牌供 Microsoft Store 用于为当前用户提供定向优惠。

## <a name="request"></a>请求


### <a name="request-syntax"></a>请求语法

| 方法 | 请求 URI                                                                |
|--------|----------------------------------------------------------------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v2.0/my/storeoffers/user``` |


### <a name="request-header"></a>请求头

| 标头        | 类型   | 说明  |
|---------------|--------|--------------|
| 授权 | 字符串 | 必填。 当前已登录应用的用户的 Microsoft 帐户令牌采用的令牌格式为 **Bearer** &lt;*token*&gt;。 |


### <a name="request-parameters"></a>请求参数

此方法没有 UI 或查询参数。

### <a name="request-example"></a>请求示例

```syntax
GET https://manage.devcenter.microsoft.com/v2.0/my/storeoffers/user HTTP/1.1
Authorization: Bearer <Microsoft Account token>
```

## <a name="response"></a>响应

此方法将会返回一个 JSON 格式的响应正文，其中包含一个对象数组及以下字段。 数组中的每个对象代表为指定用户（特定客户细分的一部分）提供的定向优惠。

| 字段      | 类型   | 描述         |
|------------|--------|------------------|
| offers      | array  | 与针对当前用户提供的定向优惠关联的加载项产品 ID 数组。 可以在 Windows 开发人员中心仪表板的**定向优惠**页为应用指定这些产品 ID。            |
| trackingId  | string | 一个 GUID，可在你自己的代码或服务中选择用来跟踪定向优惠。 |


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

* [使用 Microsoft Store 服务管理定向优惠](manage-targeted-offers-using-windows-store-services.md)

 

 
