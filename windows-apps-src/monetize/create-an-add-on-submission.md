---
author: Xansky
ms.assetid: C09F4B7C-6324-4973-980A-A60035792EFC
description: 在 Microsoft Store 提交 API 中使用此方法，为注册到 Windows 开发人员中心帐户的应用创建新的加载项提交。
title: 创建加载项提交
ms.author: mhopkins
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, Microsoft Store 提交 API, 创建加载项提交, 应用内产品, IAP
ms.localizationpriority: medium
ms.openlocfilehash: 39af744d8c8e4b9d26cd9d82a9489396857ffa5b
ms.sourcegitcommit: 4b97117d3aff38db89d560502a3c372f12bb6ed5
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/23/2018
ms.locfileid: "5438090"
---
# <a name="create-an-add-on-submission"></a>创建加载项提交


在 Microsoft Store 提交 API 中使用此方法，为注册到 Windows 开发人员中心帐户的应用创建新的加载项（也称为应用内产品或 IAP）提交。 使用此方法成功创建新提交后，[更新提交](update-an-add-on-submission.md)以对提交数据进行任何必要更改，然后[确认提交](commit-an-add-on-submission.md)以供引入和发布。

有关此方法如何适用通过使用 Microsoft Store 提交 API 创建应用提交过程的详细信息，请参阅[管理加载项提交](manage-add-on-submissions.md)。

> [!NOTE]
> 此方法可为现有加载项创建提交。 若要创建加载项，请使用[创建加载项](create-an-add-on.md)方法。

## <a name="prerequisites"></a>先决条件

若要使用此方法，首先需要执行以下操作：

* 如果尚未开始操作，请先完成 Microsoft Store 提交 API 的所有[先决条件](create-and-manage-submissions-using-windows-store-services.md#prerequisites)。
* [获取 Azure AD 访问令牌](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token)，以供在此方法的请求标头中使用。 获取访问令牌后，在它到期前，你有 60 分钟的使用时间。 该令牌到期后，可以获取新的令牌。
* 使用你的开发人员中心帐户为应用创建加载项。 可以使用开发人员中心仪表板执行此操作，也可以使用[创建加载项](create-an-add-on.md)方法执行此操作。

## <a name="request"></a>请求

此方法具有以下语法。 请参阅以下部分，获取标头和请求正文的使用示例和描述。

| 方法 | 请求 URI                                                      |
|--------|------------------------------------------------------------------|
| POST    | ```https://manage.devcenter.microsoft.com/v1.0/my/inappproducts/{inAppProductId}/submissions``` |


### <a name="request-header"></a>请求标头

| 标头        | 类型   | 说明                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| 授权 | 字符串 | 必需。 Azure AD 访问令牌的格式为 **Bearer** &lt;*token*&gt;。 |


### <a name="request-parameters"></a>请求参数

| 名称        | 类型   | 描述                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| inAppProductId | 字符串 | 必需。 要创建提交的加载项的应用商店 ID。 可通过开发人员中心仪表板获取应用商店 ID，它包含在[创建加载项](create-an-add-on.md)或[获取加载项详细信息](get-all-add-ons.md)请求的响应数据中。  |


### <a name="request-body"></a>请求正文

请勿为此方法提供请求正文。

### <a name="request-example"></a>请求示例

以下示例演示了如何为加载项创建新提交。

```
POST https://manage.devcenter.microsoft.com/v1.0/my/inappproducts/9NBLGGH4TNMP/submissions HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>响应

以下示例演示了成功调用此方法的 JSON 响应正文。 该响应正文包含新提交的相关信息。 有关响应正文中这些值的更多详细信息，请参阅[加载项提交资源](manage-add-on-submissions.md#add-on-submission-object)。

```json
{
  "id": "1152921504621243680",
  "contentType": "EMagazine",
  "keywords": [
    "books"
  ],
  "lifetime": "FiveDays",
  "listings": {
    "en": {
      "description": "English add-on description",
      "icon": {
        "fileName": "add-on-en-us-listing2.png",
        "fileStatus": "Uploaded"
      },
      "title": "Add-on Title (English)"
    },
    "ru": {
      "description": "Russian add-on description",
      "icon": {
        "fileName": "add-on-ru-listing.png",
        "fileStatus": "Uploaded"
      },
      "title": "Add-on Title (Russian)"
    }
  },
  "pricing": {
    "marketSpecificPricings": {
      "RU": "Tier3",
      "US": "Tier4",
    },
    "sales": [
      {
         "name": "Sale1",
         "basePriceId": "Free",
         "startDate": "2016-05-21T18:40:11.7369008Z",
         "endDate": "2016-05-22T18:40:11.7369008Z",
         "marketSpecificPricings": {
            "RU": "NotAvailable"
         }
      }
    ],
    "priceId": "Free",
    "isAdvancedPricingModel": true
  },
  "targetPublishDate": "2016-03-15T05:10:58.047Z",
  "targetPublishMode": "Immediate",
  "tag": "SampleTag",
  "visibility": "Public",
  "status": "PendingCommit",
  "statusDetails": {
    "errors": [
      {
        "code": "None",
        "details": "string"
      }
    ],
    "warnings": [
      {
        "code": "ListingOptOutWarning",
        "details": "You have removed listing language(s): []"
      }
    ],
    "certificationReports": [
      {
      }
    ]
  },
  "fileUploadUrl": "https://productingestionbin1.blob.core.windows.net/ingestion/26920f66-b592-4439-9a9d-fb0f014902ec?sv=2014-02-14&sr=b&sig=usAN0kNFNnYE2tGQBI%2BARQWejX1Guiz7hdFtRhyK%2Bog%3D&se=2016-06-17T20:45:51Z&sp=rwl",
  "friendlyName": "Submission 2"
}
```

## <a name="error-codes"></a>错误代码

如果无法成功完成请求，该响应中会包含以下 HTTP 错误代码之一。

| 错误代码 |  描述   |
|--------|------------------|
| 400  | 由于请求无效，无法创建提交。 |
| 409  | 由于应用的当前状态，或者应用使用的开发人员中心仪表板功能[当前不受 Microsoft Store 提交 API 支持](create-and-manage-submissions-using-windows-store-services.md#not_supported)，无法创建提交。 |   


## <a name="related-topics"></a>相关主题

* [使用 Microsoft Store 服务创建和管理提交](create-and-manage-submissions-using-windows-store-services.md)
* [管理加载项提交](manage-add-on-submissions.md)
* [获取加载项提交](get-an-add-on-submission.md)
* [确认加载项提交](commit-an-add-on-submission.md)
* [更新加载项提交](update-an-add-on-submission.md)
* [删除加载项提交](delete-an-add-on-submission.md)
* [获取加载项提交的状态](get-status-for-an-add-on-submission.md)
