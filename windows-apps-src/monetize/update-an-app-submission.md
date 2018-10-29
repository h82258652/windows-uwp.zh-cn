---
author: Xansky
ms.assetid: E8751EBF-AE0F-4107-80A1-23C186453B1C
description: 在 Microsoft Store 提交 API 中使用此方法，可更新现有应用提交。
title: 更新应用提交
ms.author: mhopkins
ms.date: 04/17/2018
ms.topic: article
keywords: windows 10, uwp, Microsoft Store 提交 API, 应用提交, 更新
ms.localizationpriority: medium
ms.openlocfilehash: 3bf6ed961f4f84cbc2bbef4a4fe79625dcadc326
ms.sourcegitcommit: 753e0a7160a88830d9908b446ef0907cc71c64e7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/29/2018
ms.locfileid: "5742227"
---
# <a name="update-an-app-submission"></a>更新应用提交

在 Microsoft Store 提交 API 中使用此方法，可更新现有应用提交。 使用此方法成功更新提交后，必须[确认提交](commit-an-app-submission.md)才可以实现引入和发布。

有关此方法如何适用通过使用 Microsoft Store 提交 API 创建应用提交过程的详细信息，请参阅[管理应用提交](manage-app-submissions.md)。

## <a name="prerequisites"></a>先决条件

若要使用此方法，首先需要执行以下操作：

* 如果尚未开始操作，请先完成 Microsoft Store 提交 API 的所有[先决条件](create-and-manage-submissions-using-windows-store-services.md#prerequisites)。
* [获取 Azure AD 访问令牌](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token)，以供在此方法的请求标头中使用。 获取访问令牌后，在它到期前，你有 60 分钟的使用时间。 该令牌到期后，可以获取新的令牌。
* 使用你的开发人员中心帐户为应用创建提交。 可以使用开发人员中心仪表板执行此操作，也可以通过以下方式执行此操作：使用[创建应用提交](create-an-app-submission.md)方法。

## <a name="request"></a>请求

此方法具有以下语法。 请参阅以下部分，获取标头和请求正文的使用示例和描述。

| 方法 | 请求 URI                                                      |
|--------|------------------------------------------------------------------|
| PUT   | ```https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/submissions/{submissionId}  ``` |


### <a name="request-header"></a>请求标头

| 标头        | 类型   | 说明                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| 授权 | 字符串 | 必需。 Azure AD 访问令牌的格式为 **Bearer** &lt;*token*&gt;。 |


### <a name="request-parameters"></a>请求参数

| 名称        | 类型   | 描述                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| applicationId | 字符串 | 必需。 要更新提交的应用的应用商店 ID。 有关应用商店 ID 的详细信息，请参阅[查看应用标识详细信息](https://msdn.microsoft.com/windows/uwp/publish/view-app-identity-details)。  |
| submissionId | 字符串 | 必需。 要更新的提交的 ID。 此 ID 包含在[创建应用提交](create-an-app-submission.md)请求的响应数据中。 对于在开发人员中心仪表板中创建的提交，此 ID 也包含在仪表板中的提交页面的 URL 中。  |


### <a name="request-body"></a>请求正文

请求正文具有以下参数。

| 值      | 类型   | 描述                                                                                                                                                                                                                                                                         |
|------------|--------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| applicationCategory           | 字符串  |   为应用指定[类别和/或子类别](https://msdn.microsoft.com/windows/uwp/publish/category-and-subcategory-table)的字符串。 通过下划线“_”字符将类别和子类别组合为单个字符串，例如 **BooksAndReference_EReader**。      |  
| pricing           |  对象  | 包含应用定价信息的对象。 有关详细信息，请参阅[定价资源](manage-app-submissions.md#pricing-object)部分。       |   
| visibility           |  字符串  |  应用的可见性。 这可以是以下值之一： <ul><li>Hidden</li><li>Public</li><li>Private</li><li>NotSet</li></ul>       |   
| targetPublishMode           | 字符串  | 提交的发布模式。 这可以是以下值之一： <ul><li>Immediate</li><li>Manual</li><li>SpecificDate</li></ul> |
| targetPublishDate           | 字符串  | 提交的发布日期采用 ISO 8601 格式（如果 *targetPublishMode* 设为“SpecificDate”）。  |  
| listings           |   对象  |  键值对字典，其中每个键为国家/地区代码，而每个值为包含应用一览信息的[一览资源](manage-app-submissions.md#listing-object)对象。       |   
| hardwarePreferences           |  数组  |   一组用于定义应用的[硬件首选项](https://msdn.microsoft.com/windows/uwp/publish/enter-app-properties#hardware_preferences)的字符串。 这可以是以下值之一： <ul><li>Touch</li><li>Keyboard</li><li>Mouse</li><li>Camera</li><li>NfcHce</li><li>Nfc</li><li>BluetoothLE</li><li>Telephony</li></ul>     |   
| automaticBackupEnabled           |  布尔型  |   指示 Windows 是否可以将应用的数据包含在 OneDrive 的自动备份中。 有关详细信息，请参阅[应用声明](https://msdn.microsoft.com/windows/uwp/publish/app-declarations)。   |   
| canInstallOnRemovableMedia           |  布尔型  |   指示客户是否可以将应用安装到可移动存储。 有关详细信息，请参阅[应用声明](https://msdn.microsoft.com/windows/uwp/publish/app-declarations)。     |   
| isGameDvrEnabled           |  布尔型 |   指示是否可以为应用启用游戏 DVR。    |   
| gamingOptions           |  object |   一个包含[游戏选项资源](manage-app-submissions.md#gaming-options-object)的数组，用于为应用定义游戏相关设置。     |   
| hasExternalInAppProducts           |     布尔型          |   指示应用是否允许用户在 Microsoft Store 商务系统之外进行购买。 有关详细信息，请参阅[应用声明](https://msdn.microsoft.com/windows/uwp/publish/app-declarations)。     |   
| meetAccessibilityGuidelines           |    布尔型           |  指示应用是否经测试符合辅助功能准则。 有关详细信息，请参阅[应用声明](https://msdn.microsoft.com/windows/uwp/publish/app-declarations)。      |   
| notesForCertification           |  字符串  |   包含应用的[认证说明](https://msdn.microsoft.com/windows/uwp/publish/notes-for-certification)。    |    
| applicationPackages           |   数组  | 包含提供提交中关于每个程序包详细信息的对象。 有关详细信息，请参阅[应用程序包](manage-app-submissions.md#application-package-object)部分。 调用此方法更新应用提交时，请求正文中仅需要这些对象的 *fileName*、*fileStatus*、*minimumDirectXVersion* 和 *minimumSystemRam* 值。 其他值由开发人员中心填充。   |    
| packageDeliveryOptions    | 对象  | 包含提交的逐步软件包推出和强制更新设置。 有关详细信息，请参阅[软件包传递选项对象](manage-app-submissions.md#package-delivery-options-object)部分。  |
| enterpriseLicensing           |  字符串  |  [企业授权值](manage-app-submissions.md#enterprise-licensing)的其中一个值，它指示应用的企业授权行为。  |    
| allowMicrosftDecideAppAvailabilityToFutureDeviceFamilies           |  布尔型   |  指示是否允许 Microsoft [将应用提供给未来 Windows10 设备系列](https://msdn.microsoft.com/windows/uwp/publish/set-app-pricing-and-availability#windows-10-device-families)。    |    
| allowTargetFutureDeviceFamilies           | 布尔型   |  指示是否允许应用[以未来 Windows10 设备系列为目标](https://msdn.microsoft.com/windows/uwp/publish/set-app-pricing-and-availability#windows-10-device-families)。     |   
| trailers           |  array |   一个包含[预告片资源](manage-app-submissions.md#trailer-object)的数组，用于表示应用一览的视频预告片。   |   


### <a name="request-example"></a>请求示例

以下示例演示如何更新应用提交。

```json
PUT https://manage.devcenter.microsoft.com/v1.0/my/applications/9NBLGGH4R315/submissions/1152921504621230023 HTTP/1.1
Authorization: Bearer <your access token>
Content-Type: application/json
{
  "applicationCategory": "BooksAndReference_EReader",
  "pricing": {
    "trialPeriod": "FifteenDays",
    "marketSpecificPricings": {},
    "sales": [],
    "priceId": "Tier2"
  },
  "visibility": "Public",
  "targetPublishMode": "Manual",
  "targetPublishDate": "1601-01-01T00:00:00Z",
  "listings": {
    "en-us": {
      "baseListing": {
        "copyrightAndTrademarkInfo": "",
        "keywords": [
              "epub"
            ],
        "licenseTerms": "",
        "privacyPolicy": "",
        "supportContact": "",
        "websiteUrl": "",
        "description": "Description",
        "features": [
              "Free ebook reader"
            ],
        "releaseNotes": "",
        "images": [
          {
            "fileName": "contoso.png",
            "fileStatus": "Uploaded",
            "id": "1152921504672272757",
            "imageType": "Screenshot"
          }
        ],
        "recommendedHardware": [],
        "title": "Contoso ebook reader"
      },
      "platformOverrides": {
        "Windows81": {
          "description": "Ebook reader for Windows 8.1"
        }
      }
    }
  },
  "hardwarePreferences": [
    "Touch"
  ],
  "automaticBackupEnabled": false,
  "canInstallOnRemovableMedia": true,
  "isGameDvrEnabled": false,
  "gamingOptions": [],
  "hasExternalInAppProducts": false,
  "meetAccessibilityGuidelines": true,
  "notesForCertification": "",
  "applicationPackages": [
    {
      "fileName": "contoso_app.appx",
      "fileStatus": "PendingUpload",
      "minimumDirectXVersion": "None",
      "minimumSystemRam": "None"
    }
  ],
  "packageDeliveryOptions": {
    "packageRollout": {
        "isPackageRollout": false,
        "packageRolloutPercentage": 0.0,
        "packageRolloutStatus": "PackageRolloutNotStarted",
        "fallbackSubmissionId": "0"
    },
    "isMandatoryUpdate": false,
    "mandatoryUpdateEffectiveDate": "1601-01-01T00:00:00.0000000Z"
  },
  "enterpriseLicensing": "Online",
  "allowMicrosoftDecideAppAvailabilityToFutureDeviceFamilies": true,
  "allowTargetFutureDeviceFamilies": {
    "Desktop": false,
    "Mobile": true,
    "Holographic": true,
    "Xbox": false,
    "Team": true
  },
  "trailers": []
}
```

## <a name="response"></a>响应

以下示例演示了成功调用此方法的 JSON 响应正文。 该响应正文包含已更新提交的相关信息。 有关响应正文中这些值的更多详细信息，请参阅[应用提交资源](manage-app-submissions.md#app-submission-object)。

```json
{
  "id": "1152921504621243540",
  "applicationCategory": "BooksAndReference_EReader",
  "pricing": {
    "trialPeriod": "FifteenDays",
    "marketSpecificPricings": {},
    "sales": [],
    "priceId": "Tier2"
  },
  "visibility": "Public",
  "targetPublishMode": "Manual",
  "targetPublishDate": "1601-01-01T00:00:00Z",
  "listings": {
    "en-us": {
      "baseListing": {
        "copyrightAndTrademarkInfo": "",
        "keywords": [
           "epub"
        ],
        "licenseTerms": "",
        "privacyPolicy": "",
        "supportContact": "",
        "websiteUrl": "",
        "description": "Description",
        "features": [
          "Free ebook reader"
        ],
        "releaseNotes": "",
        "images": [
          {
            "fileName": "contoso.png",
            "fileStatus": "Uploaded",
            "id": "1152921504672272757",
            "imageType": "Screenshot"
          }
        ],
        "recommendedHardware": [],
        "title": "Contoso ebook reader"
      },
      "platformOverrides": {
        "Windows81": {
          "description": "Ebook reader for Windows 8.1",
        }
      }
    }
  },
  "hardwarePreferences": [
    "Touch"
  ],
  "automaticBackupEnabled": false,
  "canInstallOnRemovableMedia": true,
  "isGameDvrEnabled": false,
  "gamingOptions": [],
  "hasExternalInAppProducts": false,
  "meetAccessibilityGuidelines": true,
  "notesForCertification": "",
  "status": "PendingCommit",
  "statusDetails": {
    "errors": [],
    "warnings": [],
    "certificationReports": []
  },
  "fileUploadUrl": "https://productingestionbin1.blob.core.windows.net/ingestion/387a9ea8-a412-43a9-8fb3-a38d03eb483d?sv=2014-02-14&sr=b&sig=sdd12JmoaT6BhvC%2BZUrwRweA%2Fkvj%2BEBCY09C2SZZowg%3D&se=2016-06-17T18:32:26Z&sp=rwl",
  "applicationPackages": [
    {
      "fileName": "contoso_app.appx",
      "fileStatus": "PendingUpload",
      "id": "1152921504620138797",
      "version": "1.0.0.0",
      "architecture": "ARM",
      "languages": [
        "en-US"
      ],
      "capabilities": [
        "ID_RESOLUTION_HD720P",
        "ID_RESOLUTION_WVGA",
        "ID_RESOLUTION_WXGA"
      ],
      "minimumDirectXVersion": "None",
      "minimumSystemRam": "None",
      "targetDeviceFamilies": [
        "Windows.Mobile min version 10.0.10240.0"
      ]
    }
  ],
  "packageDeliveryOptions": {
    "packageRollout": {
        "isPackageRollout": false,
        "packageRolloutPercentage": 0.0,
        "packageRolloutStatus": "PackageRolloutNotStarted",
        "fallbackSubmissionId": "0"
    },
    "isMandatoryUpdate": false,
    "mandatoryUpdateEffectiveDate": "1601-01-01T00:00:00.0000000Z"
  },
  "enterpriseLicensing": "Online",
  "allowMicrosoftDecideAppAvailabilityToFutureDeviceFamilies": true,
  "allowTargetFutureDeviceFamilies": {
    "Desktop": false,
    "Mobile": true,
    "Holographic": true,
    "Xbox": false,
    "Team": true
  },
  "friendlyName": "Submission 2",
  "trailers": []
}
```

## <a name="error-codes"></a>错误代码

如果无法成功完成请求，该响应中会包含以下 HTTP 错误代码之一。

| 错误代码 |  描述   |
|--------|------------------|
| 400  | 由于请求无效，无法更新提交。 |
| 409  | 由于应用的当前状态，或者应用使用的开发人员中心仪表板功能[当前不受 Microsoft Store 提交 API 支持](create-and-manage-submissions-using-windows-store-services.md#not_supported)，无法更新提交。 |   


## <a name="related-topics"></a>相关主题

* [使用 Microsoft Store 服务创建和管理提交](create-and-manage-submissions-using-windows-store-services.md)
* [获取应用提交](get-an-app-submission.md)
* [创建应用提交](create-an-app-submission.md)
* [确认应用提交](commit-an-app-submission.md)
* [删除应用提交](delete-an-app-submission.md)
* [获取应用提交的状态](get-status-for-an-app-submission.md)
