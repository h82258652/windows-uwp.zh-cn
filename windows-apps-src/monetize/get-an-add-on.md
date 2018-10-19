---
author: Xansky
ms.assetid: 78278741-09A4-4406-A112-9AF3C73F5C16
description: 在 Microsoft Store 提交 API 中使用此方法，可检索注册到 Windows 开发人员中心帐户的应用的加载项相关信息。
title: 获取加载项
ms.author: mhopkins
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, Microsoft Store 提交 API, 加载项, 应用内产品, IAP
ms.localizationpriority: medium
ms.openlocfilehash: 95d4b30c29bdfdec086bffb953b02dce7e3e7c66
ms.sourcegitcommit: e16c9845b52d5bd43fc02bbe92296a9682d96926
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/19/2018
ms.locfileid: "4956023"
---
# <a name="get-an-add-on"></a>获取加载项

在 Microsoft Store 提交 API 中使用此方法，可检索注册到 Windows 开发人员中心帐户的应用的加载项（也称为应用内产品或 IAP）相关信息。

## <a name="prerequisites"></a>先决条件

若要使用此方法，首先需要执行以下操作：

* 如果尚未开始操作，请先完成 Microsoft Store 提交 API 的所有[先决条件](create-and-manage-submissions-using-windows-store-services.md#prerequisites)。
* [获取 Azure AD 访问令牌](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token)，以供在此方法的请求标头中使用。 获取访问令牌后，在它到期前，你有 60 分钟的使用时间。 该令牌到期后，可以获取新的令牌。

## <a name="request"></a>请求

此方法具有以下语法。 请参阅以下部分，获取标头和请求正文的使用示例和描述。

| 方法 | 请求 URI                                                      |
|--------|------------------------------------------------------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/inappproducts/{inAppProductId}``` |


### <a name="request-header"></a>请求头

| 标头        | 类型   | 说明                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| 授权 | 字符串 | 必需。 Azure AD 访问令牌的格式为 **Bearer** &lt;*token*&gt;。 |


### <a name="request-parameters"></a>请求参数

| 名称        | 类型   | 描述                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| id | 字符串 | 必需。 要检索的加载项的应用商店 ID。 开发人员中心仪表板上会提供该应用商店 ID。  |


### <a name="request-body"></a>请求正文

请勿为此方法提供请求正文。


### <a name="request-example"></a>请求示例

以下示例演示了如何检索加载项相关信息。

```
GET https://manage.devcenter.microsoft.com/v1.0/my/inappproducts/9NBLGGH4TNMP HTTP/1.1
Authorization: Bearer <your access token>
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
  "productId": "Test-add-on",
  "productType": "Durable",
  "pendingInAppProductSubmission": {
    "id": "1152921504621243619",
    "resourceLocation": "inappproducts/9NBLGGH4TNMP/submissions/1152921504621243619"
  },
  "lastPublishedInAppProductSubmission": {
    "id": "1152921504621243705",
    "resourceLocation": "inappproducts/9NBLGGH4TNMP/submissions/1152921504621243705"
  }
}
```

## <a name="error-codes"></a>错误代码

如果无法成功完成请求，该响应中会包含以下 HTTP 错误代码之一。

| 错误代码 |  描述   |
|--------|------------------|
| 404  | 找不到指定的加载项。 |
| 409  | 加载项使用的开发人员中心仪表板功能[当前不受 Microsoft Store 提交 API 支持](create-and-manage-submissions-using-windows-store-services.md#not_supported)。  |


## <a name="related-topics"></a>相关主题

* [使用 Microsoft Store 服务创建和管理提交](create-and-manage-submissions-using-windows-store-services.md)
* [管理加载项提交](manage-add-on-submissions.md)
* [获取所有加载项](get-all-add-ons.md)
* [创建加载项](create-an-add-on.md)
* [删除加载项](delete-an-add-on.md)
