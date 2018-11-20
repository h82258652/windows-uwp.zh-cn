---
author: Xansky
ms.assetid: 5BD650D2-AA26-4DE9-8243-374FDB7D932B
description: 在 Microsoft Store 提交 API 中使用此方法创建加载项，为注册到你 PartnerCenter 帐户的应用。
title: 创建加载项
ms.author: mhopkins
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, Microsoft Store 提交 API, 创建加载项, 应用内产品, IAP
ms.localizationpriority: medium
ms.openlocfilehash: d262a86c4a177095015c3f1391b19f1a7719d0a4
ms.sourcegitcommit: cbe7cf620622a5e4df7414f9e38dfecec1cfca99
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/20/2018
ms.locfileid: "7419801"
---
# <a name="create-an-add-on"></a>创建加载项

在 Microsoft Store 提交 API 中使用此方法创建加载项 （也称为应用内产品或 IAP），为注册到你的合作伙伴中心帐户的应用。

> [!NOTE]
> 此方法无需任何提交即可创建加载项。 若要创建加载项提交，请参阅[管理加载项提交](manage-add-on-submissions.md)中的方法。

## <a name="prerequisites"></a>先决条件

若要使用此方法，首先需要执行以下操作：

* 如果尚未开始操作，请先完成 Microsoft Store 提交 API 的所有[先决条件](create-and-manage-submissions-using-windows-store-services.md#prerequisites)。
* [获取 Azure AD 访问令牌](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token)，以供在此方法的请求标头中使用。 获取访问令牌后，在它到期前，你有 60 分钟的使用时间。 该令牌到期后，可以获取新的令牌。

## <a name="request"></a>请求

此方法具有以下语法。 请参阅以下部分，获取标头和请求正文的使用示例和描述。

| 方法 | 请求 URI                                                      |
|--------|------------------------------------------------------------------|
| POST    | ```https://manage.devcenter.microsoft.com/v1.0/my/inappproducts``` |


### <a name="request-header"></a>请求标头

| 标头        | 类型   | 说明                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| 授权 | 字符串 | 必需。 Azure AD 访问令牌的格式为 **Bearer** &lt;*token*&gt;。 |


### <a name="request-body"></a>请求正文

请求正文具有以下参数。

|  参数  |  类型  |  说明  |  必需  |
|------|------|------|------|
|  applicationIds  |  数组  |  包含应用（与此加载项相关联）的应用商店 ID 的数组。 只有一个项在此数组中受支持。   |  是  |
|  productId  |  字符串  |  加载项的产品 ID。 这是一个标识符，可在代码中用于引用加载项。 有关详细信息，请参阅[设置你的产品类型和产品 ID](https://msdn.microsoft.com/windows/uwp/publish/set-your-iap-product-id)。  |  是  |
|  productType  |  字符串  |  加载项的产品类型。 支持以下值：**Durable** 和 **Consumable**。  |  是  |


### <a name="request-example"></a>请求示例

以下示例演示了如何为应用创建新易耗型加载项。

```syntax
POST https://manage.devcenter.microsoft.com/v1.0/my/inappproducts HTTP/1.1
Authorization: Bearer eyJ0eXAiOiJKV1Q...
Content-Type: application/json
{
    "applicationIds": [  "9NBLGGH4R315"  ],
    "productId": "my-new-add-on",
    "productType": "Consumable",
}
```

## <a name="response"></a>响应

以下示例演示了成功调用此方法的 JSON 响应正文。 有关响应正文中这些值的更多详细信息，请参阅[加载项资源](manage-add-ons.md#add-on-object)。

```json
{
  "applications": {
    "value": [
      {
        "id": "9NBLGGH4R315",
        "resourceLocation": "applications/9NBLGGH4R315"
      }
    ],
    "totalCount": 1
  },
  "id": "9NBLGGH4TNMP",
  "productId": "my-new-add-on",
  "productType": "Consumable",
}
```

## <a name="error-codes"></a>错误代码

如果无法成功完成请求，该响应中会包含以下 HTTP 错误代码之一。

| 错误代码 |  描述                                                                                                                                                                           |
|--------|------------------|
| 400  | 请求无效。 |
| 409  | 由于其当前状态下，无法创建加载项或加载项使用[当前不受 Microsoft Store 提交 API](create-and-manage-submissions-using-windows-store-services.md#not_supported)的合作伙伴中心功能。 |   


## <a name="related-topics"></a>相关主题

* [使用 Microsoft Store 服务创建和管理提交](create-and-manage-submissions-using-windows-store-services.md)
* [管理加载项提交](manage-add-on-submissions.md)
* [获取所有加载项](get-all-add-ons.md)
* [获取加载项](get-an-add-on.md)
* [删除加载项](delete-an-add-on.md)
