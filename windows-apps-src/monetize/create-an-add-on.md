---
ms.assetid: 5BD650D2-AA26-4DE9-8243-374FDB7D932B
description: 在 Microsoft Store 提交 API 中使用此方法创建的应用程序注册到 PartnerCenter 帐户外接程序。
title: 创建加载项
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, Microsoft Store 提交 API, 创建加载项, 应用内产品, IAP
ms.localizationpriority: medium
ms.openlocfilehash: b358eecd1799e76573cf6d254a80e7a7971bc123
ms.sourcegitcommit: 6a7dd4da2fc31ced7d1cdc6f7cf79c2e55dc5833
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/21/2019
ms.locfileid: "58334165"
---
# <a name="create-an-add-on"></a>创建加载项

在 Microsoft Store 提交 API 中使用此方法创建的外接程序 （也称为应用产品或、 IAP） 的应用程序注册到你的合作伙伴中心帐户。

> [!NOTE]
> 此方法无需任何提交即可创建加载项。 若要创建加载项提交，请参阅[管理加载项提交](manage-add-on-submissions.md)中的方法。

## <a name="prerequisites"></a>系统必备

若要使用此方法，首先需要执行以下操作：

* 如果尚未开始操作，请先完成 Microsoft Store 提交 API 的所有[先决条件](create-and-manage-submissions-using-windows-store-services.md#prerequisites)。
* [获取 Azure AD 访问令牌](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token)，以供在此方法的请求标头中使用。 获取访问令牌后，在它到期前，你有 60 分钟的使用时间。 该令牌到期后，可以获取新的令牌。

## <a name="request"></a>请求

此方法具有以下语法。 请参阅以下部分，获取标头和请求正文的使用示例和描述。

| 方法 | 请求 URI                                                      |
|--------|------------------------------------------------------------------|
| 发布    | `https://manage.devcenter.microsoft.com/v1.0/my/inappproducts` |


### <a name="request-header"></a>请求头

| Header        | 在任务栏的搜索框中键入   | 描述                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| 授权 | string | 必需。 Azure AD 访问令牌的格式为 **Bearer** *token*&lt;&gt;。 |


### <a name="request-body"></a>请求正文

请求正文具有以下参数。

|  参数  |  在任务栏的搜索框中键入  |  描述  |  必需  |
|------|------|------|------|
|  applicationIds  |  数组  |  包含应用（与此加载项相关联）的应用商店 ID 的数组。 只有一个项在此数组中受支持。   |  是  |
|  productId  |  string  |  加载项的产品 ID。 这是一个标识符，可在代码中用于引用加载项。 有关详细信息，请参阅[设置你的产品类型和产品 ID](https://msdn.microsoft.com/windows/uwp/publish/set-your-iap-product-id)。  |  是  |
|  productType  |  string  |  加载项的产品类型。 支持以下值：**持久**并**可使用**。  |  是  |


### <a name="request-example"></a>请求示例

以下示例演示了如何为应用创建新易耗型加载项。

```json
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
| 409  | 由于其当前状态，无法创建外接程序或外接程序使用的合作伙伴中心功能[目前不支持通过 Microsoft Store 提交 API](create-and-manage-submissions-using-windows-store-services.md#not_supported)。 |   


## <a name="related-topics"></a>相关主题

* [创建和管理使用 Microsoft Store 服务的提交](create-and-manage-submissions-using-windows-store-services.md)
* [管理外接程序提交](manage-add-on-submissions.md)
* [获取所有外接程序](get-all-add-ons.md)
* [获取外接程序](get-an-add-on.md)
* [删除外接程序](delete-an-add-on.md)
