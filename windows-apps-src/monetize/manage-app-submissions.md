---
author: mcleanbyron
ms.assetid: C7428551-4B31-4259-93CD-EE229007C4B8
description: "在 Windows 应用商店提交 API 中使用这些方法，来管理已注册到 Windows 开发人员中心帐户的应用提交。"
title: "使用 Windows 应用商店提交 API 管理应用提交"
translationtype: Human Translation
ms.sourcegitcommit: 9b76a11adfab838b21713cb384cdf31eada3286e
ms.openlocfilehash: 49d60048a0dd5dae3e80abb9fd4e21b8cf7b417e

---

# 使用 Windows 应用商店提交 API 管理应用提交


在 Windows 应用商店提交 API 中使用以下方法，来管理已注册到 Windows 开发人员中心帐户的应用提交。 有关 Windows 应用商店提交 API 的介绍（包括使用 API 的先决条件），请参阅[使用 Windows 应用商店服务创建和管理提交](create-and-manage-submissions-using-windows-store-services.md)。

>
            **注意**
            &nbsp;&nbsp;这些方法只能用于已授予使用 Windows 应用商店提交 API 权限的 Windows 开发人员中心帐户。 并非所有帐户都已启用此权限。

>
            **重要提示**
            &nbsp;&nbsp;在不久的将来，Microsoft 将更改 Windows 开发人员中心中的应用提交的定价数据模型。 实现此更改后，**定价**资源将不再受支持，并且你将暂时无法使用 Windows 应用商店提交 API 获取或修改应用提交的试用期、定价和销售数据。 我们将在将来更新该 API，以引入以编程方式访问应用提交的定价信息的新方法。 有关详细信息，请参阅[定价资源](#pricing-object)部分。


| 方法        | URI    | 描述                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| GET | ```https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/submissions/{submissionId}``` | 获取现有应用提交的数据。 有关详细信息，请参阅[此文章](get-an-app-submission.md)。 |
| GET | ```https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/submissions/{submissionId}/status``` | 获取现有应用提交的状态。 有关详细信息，请参阅[此文章](get-status-for-an-app-submission.md)。 |
| POST | ```https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/submissions``` | 为已注册到 Windows 开发人员中心帐户的应用创建新提交。 有关详细信息，请参阅[此文章](create-an-app-submission.md)。 |
| POST | ```https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/submissions/{submissionId}/commit``` | 将新的或更新的应用提交到 Windows 开发人员中心。 有关详细信息，请参阅[此文章](commit-an-app-submission.md)。 |
| PUT | ```https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/submissions/{submissionId}``` | 更新现有的应用提交。 有关详细信息，请参阅[此文章](update-an-app-submission.md)。 |
| DELETE | ```https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/submissions/{submissionId}``` | 删除应用提交。 有关详细信息，请参阅[此文章](delete-an-app-submission.md)。 |
| GET | ```https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/submissions/{submissionId}/packagerollout``` | 获取应用提交的逐步推出信息。 有关详细信息，请参阅[此文章](get-package-rollout-info-for-an-app-submission.md)。 |
| POST | ```https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/submissions/{submissionId}/updatepackagerolloutpercentage``` | 更新应用提交的逐步推出百分比。 有关详细信息，请参阅[此文章](update-the-package-rollout-percentage-for-an-app-submission.md)。 |
| POST | ```https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/submissions/{submissionId}/haltpackagerollout``` | 终止应用提交的逐步推出。 有关详细信息，请参阅[此文章](halt-the-package-rollout-for-an-app-submission.md)。 |
| POST | ```https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/submissions/{submissionId}/finalizepackagerollout``` | 完成应用提交的逐步推出。 有关详细信息，请参阅[此文章](finalize-the-package-rollout-for-an-app-submission.md)。 |

<span id="create-an-app-submission">
## 创建应用提交

若要创建应用提交，请遵循此过程。

1. 如果尚未开始操作，请先完成 Windows 应用商店提交 API 的所有[先决条件](create-and-manage-submissions-using-windows-store-services.md#prerequisites)。

  >
            **注意**
            &nbsp;&nbsp;确保应用至少已有一个已完成的提交并且已完成[年龄分级](https://msdn.microsoft.com/windows/uwp/publish/age-ratings)信息。

2. 
            [获取 Azure AD 访问令牌](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token)。 在 Windows 应用商店提交 API 中，必须将此访问令牌传递给相关方法。 获取访问令牌后，在它到期前，你有 60 分钟的使用时间。 该令牌到期后，可以获取新的令牌。

3. 通过执行 Windows 应用商店提交 API 中的以下方法[创建应用提交](create-an-app-submission.md)。 此方法创建新的正在进行的提交，这是你上次发布的提交的副本。

  ```
  POST https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/submissions
  ```

  响应正文包含三个项目：新的提交 ID、新的提交数据（包括所有列表和定价信息），以及用于上传提交的任何应用包和列表图像的共享访问签名 (SAS) URI。 有关 SAS 的详细信息，请参阅[共享访问签名，第 1 部分：了解 SAS 模型](https://azure.microsoft.com/documentation/articles/storage-dotnet-shared-access-signature-part-1/)。

4. 如果要为提交添加新的程序包或图像，请[准备应用包](https://msdn.microsoft.com/windows/uwp/publish/app-package-requirements)并[准备应用屏幕截图和图像](https://msdn.microsoft.com/windows/uwp/publish/app-screenshots-and-images)。 将所有这些文件添加到 ZIP 存档。

5. 使用新提交所需的任何更改来修改提交数据，并执行以下方法来[更新应用提交](update-an-app-submission.md)。

  ```
  PUT https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/submissions/{submissionId}
  ```

  >
            **注意**
            &nbsp;&nbsp;如果要为提交添加新软件包或图像，请确保更新提交数据在 ZIP 存档中以引用这些文件的名称和相对路径。

4. 如果要为提交添加新的程序包或图像，请将 ZIP 存档上载到 SAS URI，该 URI 已在步骤 2 中调用的 POST 方法的响应正文中提供。 有关详细信息，请参阅[共享访问签名，第 2 部分：使用 Blob 存储创建和使用 SAS](https://azure.microsoft.com/documentation/articles/storage-dotnet-shared-access-signature-part-2/)。

  以下代码片段演示了如何在用于 .NET 的 Azure 存储客户端库中使用 [CloudBlockBlob](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.blob.cloudblockblob.aspx) 类上传存档。

  ```csharp
  string sasUrl = "https://productingestionbin1.blob.core.windows.net/ingestion/26920f66-b592-4439-9a9d-fb0f014902ec?sv=2014-02-14&sr=b&sig=usAN0kNFNnYE2tGQBI%2BARQWejX1Guiz7hdFtRhyK%2Bog%3D&se=2016-06-17T20:45:51Z&sp=rwl";

  Microsoft.WindowsAzure.Storage.Blob.CloudBlockBlob blockBob =
      new Microsoft.WindowsAzure.Storage.Blob.CloudBlockBlob(new System.Uri(sasUrl));
  await blockBob.UploadFromStreamAsync(stream);
  ```

5. 通过执行以下方法[确认应用提交](commit-an-app-submission.md)。 这将向开发人员中心发出警报：已完成提交，更新现在应该应用到了帐户。

  ```
  POST https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/submissions/{submissionId}/commit
  ```

6. 通过执行以下方法来检查提交状态以[获取应用提交的状态](get-status-for-an-app-submission.md)。

    ```
    GET https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/submissions/{submissionId}/status
    ```

    若要确认提交状态，请查看响应正文中的 *status* 值。 如果请求成功，此值应该从 **CommitStarted** 更改为 **PreProcessing**；如果请求中存在错误，此值应该更改为 **CommitFailed**。 如果存在错误，*statusDetails* 字段将包含有关错误的更多详细信息。

7. 在提交成功完成之后，提交会发送至应用商店以供引入。 可以通过使用前面的方法，或者通过访问开发人员中心仪表板，继续监视提交进度。

<span id="manage-gradual-package-rollout">
## 管理应用提交的逐步软件包推出

可在应用提交中逐步向 Windows10 上一定比例的应用客户推出已更新的软件包。 这使你可以监视特定软件包的反馈和分析数据，从而确保在更广泛地推出更新前对此更新放心。 可更改已发布提交的推出百分比（或终止更新），而无需创建新提交。 有关详细信息，包括有关如何在开发人员中心仪表板中启用和管理逐步软件包推出的说明，请参阅[本文章](../publish/gradual-package-rollout.md)。

还可使用 Windows 应用商店提交 API 中的以下方法以编程方式启用和管理应用提交的逐步软件包推出。

* 若要启用应用提交的逐步软件包推出：

  1. 
            [创建应用提交](create-an-app-submission.md)或[获取应用提交](get-an-app-submission.md)。
  2. 在响应数据中，找到 [packageRollout](#package-rollout-object) 资源、将“isPackageRollout”字段设置为 true，然后将“packageRolloutPercentage”字段设置为应获取已更新的软件包的应用客户百分比。
  3. 将已更新的应用提交数据传递到[更新应用提交](update-an-app-submission.md)方法。

<span/>

* 若要[获取应用提交的软件包推出信息](get-package-rollout-info-for-an-app-submission.md)，请执行以下方法。

  ```
  GET https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/submissions/{submissionId}/packagerollout
  ```

<span/>

* 若要[更新应用提交的软件包推出百分比](update-the-package-rollout-percentage-for-an-app-submission.md)，请执行以下方法。

  ```
  POST https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/submissions/{submissionId}/updatepackagerolloutpercentage  
  ```

<span/>

* 若要[终止应用提交的软件包推出](halt-the-package-rollout-for-an-app-submission.md)，请执行以下方法。

  ```
  POST https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/submissions/{submissionId}/haltpackagerollout   
  ```  

<span/>

* 若要[完成应用提交的软件包推出](finalize-the-package-rollout-for-an-app-submission.md)，请执行以下方法。

  ```
  POST https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/submissions/{submissionId}/finalizepackagerollout
  ```

## 资源

这些方法使用以下资源设置数据格式。

<span id="app-submission-object" />
### 应用提交

此资源表示应用的提交。 以下示例演示了此资源的格式。

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
        "keywords": [],
        "licenseTerms": "",
        "privacyPolicy": "",
        "supportContact": "",
        "websiteUrl": "",
        "description": "Description",
        "features": [],
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
        "title": "ApiTestApp For Devbox"
      },
      "platformOverrides": {}
    }
  },
  "hardwarePreferences": [
    "Touch"
  ],
  "automaticBackupEnabled": false,
  "canInstallOnRemovableMedia": true,
  "isGameDvrEnabled": false,
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
      "fileStatus": "Uploaded",
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
        "packageRolloutPercentage": 0,
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
  "friendlyName": "Submission 2"
}
```

此资源具有以下值。

| 值      | 类型   | 描述      |
|------------|--------|-------------------|
| id            | 字符串  | 提交的 ID。  |
| applicationCategory           | 字符串  |   为应用指定[类别和/或子类别](https://msdn.microsoft.com/windows/uwp/publish/category-and-subcategory-table)的字符串。 通过下划线“_”字符将类别和子类别组合为单个字符串，例如 **BooksAndReference_EReader**。      |  
| pricing           |  对象  | 包含应用定价信息的对象。 有关详细信息，请参阅下面的[定价资源](#pricing-object)部分。       |   
| visibility           |  字符串  |  应用的可见性。 这可以是以下值之一： <ul><li>Hidden</li><li>Public</li><li>Private</li><li>NotSet</li></ul>       |   
| targetPublishMode           | 字符串  | 提交的发布模式。 这可以是以下值之一： <ul><li>Immediate</li><li>Manual</li><li>SpecificDate</li></ul> |
| targetPublishDate           | 字符串  | 提交的发布日期采用 ISO 8601 格式（如果 *targetPublishMode* 设为“SpecificDate”）。  |  
| listings           |   对象  |  键值对字典，其中每个键为国家/地区代码，而每个值为包含应用一览信息的[一览资源](#listing-object)对象。       |   
| hardwarePreferences           |  数组  |   一组用于定义应用的[硬件首选项](https://msdn.microsoft.com/windows/uwp/publish/enter-app-properties#hardware_preferences)的字符串。 这可以是以下值之一： <ul><li>Touch</li><li>Keyboard</li><li>Mouse</li><li>Camera</li><li>NfcHce</li><li>Nfc</li><li>BluetoothLE</li><li>Telephony</li></ul>     |   
| automaticBackupEnabled           |  布尔型  |   指示 Windows 是否可以将应用的数据包含在 OneDrive 的自动备份中。 有关详细信息，请参阅[应用声明](https://msdn.microsoft.com/windows/uwp/publish/app-declarations)。   |   
| canInstallOnRemovableMedia           |  布尔型  |   指示客户是否可以将应用安装到可移动存储。 有关详细信息，请参阅[应用声明](https://msdn.microsoft.com/windows/uwp/publish/app-declarations)。     |   
| isGameDvrEnabled           |  布尔型 |   指示是否可以为应用启用游戏 DVR。    |   
| hasExternalInAppProducts           |     布尔型          |   指示应用是否允许用户在 Windows 应用商店商务系统之外进行购买。 有关详细信息，请参阅[应用声明](https://msdn.microsoft.com/windows/uwp/publish/app-declarations)。     |   
| meetAccessibilityGuidelines           |    布尔型           |  指示应用是否经测试符合辅助功能准则。 有关详细信息，请参阅[应用声明](https://msdn.microsoft.com/windows/uwp/publish/app-declarations)。      |   
| notesForCertification           |  字符串  |   包含应用的[认证说明](https://msdn.microsoft.com/windows/uwp/publish/notes-for-certification)。    |    
| status           |   字符串  |  提交的状态。 这可以是以下值之一： <ul><li>None</li><li>Canceled</li><li>PendingCommit</li><li>CommitStarted</li><li>CommitFailed</li><li>PendingPublication</li><li>Publishing</li><li>Published</li><li>PublishFailed</li><li>PreProcessing</li><li>PreProcessingFailed</li><li>Certification</li><li>CertificationFailed</li><li>Release</li><li>ReleaseFailed</li></ul>      |    
| statusDetails           |   对象  |  包含有关提交状态的附加详细信息，其中包括任何错误的相关信息。 有关详细信息，请参阅下面[状态详细信息](#status-details-object)部分。       |    
| fileUploadUrl           |   字符串  | 用于为提交上载任何程序包的共享访问签名 (SAS) URI。 如果要为提交添加新的程序包或图像，请将包含这些程序包和图像的 ZIP 存档上载到此 URI。 有关详细信息，请参阅[创建应用提交](#create-an-app-submission)。       |    
| applicationPackages           |   数组  | 包含提供提交中关于每个程序包详细信息的对象。 有关详细信息，请参阅下面的[应用软件包](#application-package-object)部分。 |    
| packageDeliveryOptions    | 对象  | 包含提交的逐步软件包推出和强制更新设置。 有关详细信息，请参阅下面的[软件包传递选项对象](#package-delivery-options-object)部分。  |
| enterpriseLicensing           |  字符串  |  
            [企业授权值](#enterprise-licensing)的其中一个值，它指示应用的企业授权行为。  |    
| allowMicrosftDecideAppAvailabilityToFutureDeviceFamilies           |  布尔型   |  指示是否允许 Microsoft [将应用提供给未来 Windows10 设备系列](https://msdn.microsoft.com/windows/uwp/publish/set-app-pricing-and-availability#windows-10-device-families)。    |    
| allowTargetFutureDeviceFamilies           | 对象   |  键值对字典，其中每个键为 [Windows10 设备系列](https://msdn.microsoft.com/windows/uwp/publish/set-app-pricing-and-availability#windows-10-device-families)，而每个值为布尔型，指示是否允许应用面向指定的设备系列。     |    
| friendlyName           |   字符串  |  用于显示的应用友好名称。       |  


<span id="listing-object" />
### 列表

此资源包含应用的列表信息。 此资源具有以下值。

| 值           | 类型    | 描述                                                                                                                                                                                                                          |
|-----------------|---------|------|
|  baseListing               |   对象      |  应用的[基本列表](#base-listing-object)信息，它定义了所有平台的默认列表信息。   |     
|  platformOverrides               | 对象 |   键值对字典，其中每个键为字符串，用于标识要替代其列表信息的平台；而每个值为[基本列表](#base-listing-object)对象（仅将描述中的值包含在标题中），用于指定要为指定平台替代的列表信息。 键可以具有以下值： <ul><li>Unknown</li><li>Windows80</li><li>Windows81</li><li>WindowsPhone71</li><li>WindowsPhone80</li><li>WindowsPhone81</li></ul>     |      |     

<span id="base-listing-object" />
### 基本列表

此资源包含应用的基本列表信息。 此资源具有以下值。

| 值           | 类型    | 描述       |
|-----------------|---------|------|
|  copyrightAndTrademarkInfo                |   字符串      |  可选的[版权和/或商标信息](https://msdn.microsoft.com/windows/uwp/publish/create-app-descriptions#copyright-and-trademark-info)。  |
|  keywords                |  数组       |  
            [关键字](https://msdn.microsoft.com/windows/uwp/publish/create-app-descriptions#keywords)数组，用于帮助应用出现在搜索结果中。    |
|  licenseTerms                |    字符串     | 可选的应用[许可条款](https://msdn.microsoft.com/windows/uwp/publish/create-app-descriptions#additional-license-terms)。     |
|  privacyPolicy                |   字符串      |   应用的[隐私策略](https://msdn.microsoft.com/windows/uwp/publish/privacy-policy)的 URL。    |
|  supportContact                |   字符串      |  应用的[支持人员联系信息](https://msdn.microsoft.com/windows/uwp/publish/support-contact-info)的 URL 或电子邮件地址。     |
|  websiteUrl                |   字符串      |  应用的[网页](https://msdn.microsoft.com/windows/uwp/publish/create-app-descriptions#website) URL。    |    
|  description               |    字符串     |   应用一览的[描述](https://msdn.microsoft.com/windows/uwp/publish/create-app-descriptions#description)。   |     
|  features               |    数组     |  一个最多 20 个字符串的数组，用于列出应用的[功能](https://msdn.microsoft.com/windows/uwp/publish/create-app-descriptions#app-features)。     |
|  releaseNotes               |  字符串       |  应用的[发行说明](https://msdn.microsoft.com/windows/uwp/publish/create-app-descriptions#release-notes)。    |
|  images               |   数组      |  应用一览的[图像和图标](#image-object)数据的数组。  |
|  recommendedHardware               |   数组      |  一个最多 11 个字符串的数组，用于列出应用的[推荐硬件配置](https://msdn.microsoft.com/windows/uwp/publish/create-app-descriptions#recommended-hardware)。     |
|  title               |     字符串    |   应用一览的标题。   |  


<span id="image-object" />
### 图像

此资源包含应用一览的图像和图标数据。 有关应用一览的图像和图标的详细信息，请参阅[应用屏幕截图和图像](https://msdn.microsoft.com/windows/uwp/publish/app-screenshots-and-images)。 此资源具有以下值。

| 值           | 类型    | 描述           |
|-----------------|---------|------|
|  fileName               |    字符串     |   ZIP 存档中已为提交上载的图像文件的名称。    |     
|  fileStatus               |   字符串      |  图像文件的状态。 这可以是以下值之一： <ul><li>None</li><li>PendingUpload</li><li>Uploaded</li><li>PendingDelete</li></ul>   |
|  id  |  字符串  | 由开发人员中心指定的图像 ID。  |
|  description  |  字符串  | 图像的描述。  |
|  imageType  |  字符串  | 下列字符串之一，用于指示图像类型： <ul><li>Unknown</li><li>屏幕截图</li><li>PromotionalArtwork414X180</li><li>PromotionalArtwork846X468</li><li>PromotionalArtwork558X756</li><li>PromotionalArtwork414X468</li><li>PromotionalArtwork558X558</li><li>PromotionalArtwork2400X1200</li><li>图标</li><li>WideIcon358X173</li><li>BackgroundImage1000X800</li><li>SquareIcon358X358</li><li>MobileScreenshot</li><li>XboxScreenshot</li><li>SurfaceHubScreenshot</li><li>HoloLensScreenshot</li></ul>      |


<span id="pricing-object" />
### 定价

此资源包含应用的定价信息。

>
            **重要提示**
            &nbsp;&nbsp;在不久的将来，Microsoft 将更改 Windows 开发人员中心中的应用提交的定价数据模型。 实现此更改后，**定价**资源将不再受支持，并且你将暂时无法使用 Windows 应用商店提交 API 获取或修改应用提交的试用期、定价和销售数据。 你将注意到以下行为更改：

   > * 调用 [GET 方法以获取应用提交](get-an-app-submission.md)后，**定价**资源将为空。 可继续使用开发人员中心仪表板获取应用提交的定价数据。
   > * 调用 [PUT 方法更新应用提交](update-an-app-submission.md)时，将忽略**定价**资源中的信息。 可继续使用开发人员中心仪表板更改应用提交的定价数据。

> 将来，我们将更新 Windows 应用商店提交 API，以引入以编程方式获取和更新应用提交的定价信息的新方法。

此资源具有以下值。

| 值           | 类型    | 描述                                                                                                                                                                                                                          |
|-----------------|---------|------|
|  trialPeriod               |    字符串     |  一个指定应用试用期的字符串。 这可以是以下值之一： <ul><li>NoFreeTrial</li><li>OneDay</li><li>TrialNeverExpires</li><li>SevenDays</li><li>FifteenDays</li><li>ThirtyDays</li></ul>    |
|  marketSpecificPricings               |    对象     |  键值对字典，其中每个键为两个字母的 ISO 3166-1 二字母国家/地区代码，而每个值为[价格段](#price-tiers)。 这些项表示[应用在特定市场中的自定义价格](https://msdn.microsoft.com/windows/uwp/publish/define-pricing-and-market-selection#markets-and-custom-prices)。 此字典中的任何项替代 *priceId* 值针对特定市场所指定的基价。      |     
|  sales               |   数组      |  包含应用销售信息的对象数组。 有关详细信息，请参阅下面的[销售](#sale-object)部分。    |     
|  priceId               |   字符串      |  用于指定应用[基价](https://msdn.microsoft.com/windows/uwp/publish/define-pricing-and-market-selection#base-price)的[价格段](#price-tiers)。   |


<span id="sale-object" />
### 销售

此资源包含应用的销售信息。

>
            **重要提示**
            &nbsp;&nbsp;在不久的将来，Microsoft 将更改 Windows 开发人员中心中的应用提交的定价数据模型。 实现此更改后，**销售**资源将不再受支持，并且你将暂时无法使用 Windows 应用商店提交 API 获取或修改应用提交的销售数据。 我们将在将来更新该 API，以引入以编程方式访问应用提交的销售信息的新方法。 有关详细信息，请参阅[定价资源](#pricing-object)部分。

此资源具有以下值。

| 值           | 类型    | 描述                                                                                                                                                                                                                          |
|-----------------|---------|------|
|  name               |    字符串     |   销售的名称。    |     
|  basePriceId               |   字符串      |  要用于销售基价的[价格段](#price-tiers)。    |     
|  startDate               |   字符串      |   采用 ISO 8601 格式的销售的开始日期。  |     
|  endDate               |   字符串      |  采用 ISO 8601 格式的销售的结束日期。      |     
|  marketSpecificPricings               |   对象      |   键值对字典，其中每个键为两个字母的 ISO 3166-1 二字母国家/地区代码，而每个值为[价格段](#price-tiers)。 这些项表示[应用在特定市场中的自定义价格](https://msdn.microsoft.com/windows/uwp/publish/define-pricing-and-market-selection#markets-and-custom-prices)。 此字典中的任何项替代 *basePriceId* 值针对特定市场所指定的基价。    |


<span id="status-details-object" />
### 状态详细信息

此资源包含有关提交状态的附加详细信息。 此资源具有以下值。

| 值           | 类型    | 描述                                                                                                                                                                                                                          |
|-----------------|---------|------|
|  errors               |    对象     |   包含提交的错误详细信息的对象数组。 有关详细信息，请参阅下面的[状态详细信息](#status-detail-object)部分。   |     
|  warnings               |   对象      | 包含提交的警告详细信息的对象数组。 有关详细信息，请参阅下面的[状态详细信息](#status-detail-object)部分。     |
|  certificationReports               |     对象    |   提供对提交的认证报告数据的访问权限的对象数组。 如果认证失败，可检查这些报告，获取详细信息。 有关详细信息，请参阅下面的[认证报告](#certification-report-object)部分。   |  


<span id="status-detail-object" />
### 状态详细信息

此资源包含关于提交的任何相关错误或警告的附加详细信息。 此资源具有以下值。

| 值           | 类型    | 描述                                                                                                                                                                                                                          |
|-----------------|---------|------|
|  code               |    字符串     |   描述错误或警告类型的字符串。 有关详细信息，请参阅下面的[提交状态代码](#submission-status-code)部分。   |     
|  details               |     字符串    |  包含有关问题的更多详细信息的消息。     |


<span id="application-package-object" />
### 应用程序包

此资源包含有关提交的应用包的详细信息。 以下示例演示了此资源的格式。

```json
{
  "applicationPackages": [
    {
      "fileName": "contoso_app.appx",
      "fileStatus": "Uploaded",
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
}
```

此资源具有以下值。  

>
            **注意**
            &nbsp;&nbsp;当调用[更新应用提交](update-an-app-submission.md)方法时，请求正文中仅需要此对象的 *fileName*、*fileStatus*、*minimumDirectXVersion* 和 *minimumSystemRam* 值。 其他值由开发人员中心进行填充。

| 值           | 类型    | 描述                   |
|-----------------|---------|------|
| fileName   |   字符串      |  程序包的名称。    |  
| fileStatus    | 字符串    |  程序包的状态。 这可以是以下值之一： <ul><li>None</li><li>PendingUpload</li><li>Uploaded</li><li>PendingDelete</li></ul>    |  
| id    |  字符串   |  唯一标识程序包的 ID。 此值由开发人员中心使用。   |     
| version    |  字符串   |  应用包的版本。 有关详细信息，请参阅[程序包版本编号](https://msdn.microsoft.com/windows/uwp/publish/package-version-numbering)。   |   
| architecture    |  字符串   |  程序包的体系结构（例如 ARM）。   |     
| languages    | 数组    |  应用所支持的语言的语言代码数组。 有关详细信息，请参阅[支持的语言](https://msdn.microsoft.com/windows/uwp/publish/supported-languages)。    |     
| capabilities    |  数组   |  程序包所需的功能数组。 有关功能的详细信息，请参阅[应用功能声明](https://msdn.microsoft.com/windows/uwp/packaging/app-capability-declarations)。   |     
| minimumDirectXVersion    |  字符串   |  应用包支持的最低 DirectX 版本。 这可以仅针对面向 Windows8.x 的应用进行设置；对于面向其他版本的应用，它将忽略。 这可以是以下值之一： <ul><li>None</li><li>DirectX93</li><li>DirectX100</li></ul>   |     
| minimumSystemRam    | 字符串    |  应用包所需的最小 RAM。 这可以仅针对面向 Windows8.x 的应用进行设置；对于面向其他版本的应用，它将忽略。 这可以是以下值之一： <ul><li>None</li><li>Memory2GB</li></ul>   |       
| targetDeviceFamilies    | 数组    |  一个字符串数组，它表示程序包所面向的设备系列。 此值仅用于面向 Windows10 的程序包；对于面向早期版本的程序包，此值具有值 **None**。 Windows10 程序包当前支持以下设备系列字符串，其中 *{0}* 是 Windows10 版本字符串（例如 10.0.10240.0、10.0.10586.0 或 10.0.14393.0）： <ul><li>Windows.Universal 最低版本 *{0}*</li><li>Windows.Desktop 最低版本 *{0}*</li><li>Windows.Mobile 最低版本 *{0}*</li><li>Windows.Xbox 最低版本 *{0}*</li><li>Windows.Holographic 最低版本 *{0}*</li></ul>   |    

<span/>

<span id="certification-report-object" />
### 认证报告

此资源提供对提交的认证报告数据的访问权限。 此资源具有以下值。

| 值           | 类型    | 说明                                                                                                                                                                                                                          |
|-----------------|---------|------|
|     date            |    字符串     |  报告生成的日期和时间，采用 ISO 8601 格式。    |
|     reportUrl            |    字符串     |  用于访问报告的 URL。    |


<span id="package-delivery-options-object" />
### 软件包传递选项对象

此资源包含提交的逐步软件包推出和强制更新设置。 以下示例演示了此资源的格式。

```json
{
  "packageDeliveryOptions": {
    "packageRollout": {
        "isPackageRollout": false,
        "packageRolloutPercentage": 0,
        "packageRolloutStatus": "PackageRolloutNotStarted",
        "fallbackSubmissionId": "0"
    },
    "isMandatoryUpdate": false,
    "mandatoryUpdateEffectiveDate": "1601-01-01T00:00:00.0000000Z"
  },
}
```

此资源具有以下值。

| 值           | 类型    | 说明        |
|-----------------|---------|------|
| packageRollout   |   对象      |  包含提交的逐步软件包推出设置。 有关详细信息，请参阅[软件包推出对象](#package-rollout-object)部分。    |  
| isMandatoryUpdate    | 布尔值    |  指示是否要将此提交中的软件包视为对自行安装的应用更新强制。 有关自行安装的应用更新的强制软件包的详细信息，请参阅[为应用下载并安装包更新](../packaging/self-install-package-updates.md)。    |  
| mandatoryUpdateEffectiveDate    |  日期   |  此提交中的软件包变为强制的日期和时间，采用 ISO 8601 格式和 UTC 时区。   |        

<span id="package-rollout-object" />
### 软件包推出对象

此资源包含提交的逐步[软件包推出设置](#manage-gradual-package-rollout)。 此资源具有以下值。

| 值           | 类型    | 说明        |
|-----------------|---------|------|
| isPackageRollout   |   布尔值      |  指示是否为提交启用逐步软件包推出。    |  
| packageRolloutPercentage    | 浮点数    |  将在逐步推出中收到软件包的用户百分比。    |  
| packageRolloutStatus    |  字符串   |  以下指示逐步软件包推出状态的字符串之一： <ul><li>PackageRolloutNotStarted</li><li>PackageRolloutInProgress</li><li>PackageRolloutComplete</li><li>PackageRolloutStopped</li></ul>  |  
| fallbackSubmissionId    |  字符串   |  将由不获取逐步推出软件包的客户接收的提交 ID。   |          

<span/>

## Enums

这些方法使用以下枚举。


<span id="price-tiers" />
### 价格段

以下值表示应用提交的可用价格段。

| 值           | 描述                                                                                                                                                                                                                          |
|-----------------|------|
|  Base               |   未设置价格段；使用应用的基价。      |     
|  NotAvailable              |   应用在特定区域中不可用。    |     
|  Free              |   应用是免费的。    |    
|  Tier2 到 Tier194               |   Tier2 表示 0.99 美元价格段。 每个附加价格段表示附加增量（1.29 美元、1.49 美元、1.99 美元等）。    |


<span id="enterprise-licensing" />
### 企业授权值

以下值表示应用的企业授权行为。 有关这些选项的详细信息，请参阅[组织许可选项](https://msdn.microsoft.com/windows/uwp/publish/organizational-licensing)。

| 值           |  描述      |
|-----------------|---------------|
| None            |     不向具有应用商店托管（联机）批量许可的企业提供应用。         |     
| Online        |     向具有应用商店托管（联机）批量许可的企业提供应用。  |
| OnlineAndOffline | 向具有应用商店托管（联机）批量许可的企业提供应用，并通过断开连接的（脱机）许可向企业提供应用。 |


<span id="submission-status-code" />
### 提交状态代码

以下值表示提交的状态代码。

| 值           |  描述      |
|-----------------|---------------|
| None            |     未指定任何代码。         |     
| InvalidArchive        |     包含程序包的 ZIP 存档无效或具有无法识别的存档格式。  |
| MissingFiles | ZIP 存档未包含提交数据中列出的所有文件，或者它们位于存档中的错误位置。 |
| PackageValidationFailed | 提交中的一个或多个程序包验证失败。 |
| InvalidParameterValue | 请求正文中的某一个参数无效。 |
| InvalidOperation | 所尝试的操作无效。 |
| InvalidState | 所尝试的操作对当前状态的软件包外部测试版无效。 |
| ResourceNotFound | 找不到指定的软件包外部测试版。 |
| ServiceError | 导致请求失败的内部服务错误。 重新尝试请求。 |
| ListingOptOutWarning | 开发人员已从以前的提交中删除了某个列表，或者未包含程序包支持的列表信息。 |
| ListingOptInWarning  | 开发人员添加了一个列表。 |
| UpdateOnlyWarning | 开发人员正在尝试插入仅具有更新支持的某些内容。 |
| Other  | 提交处于无法识别或未分类的状态。 |
| PackageValidationWarning | 程序包验证过程导致出现警告。 |

<span/>

## 相关主题

* [使用 Windows 应用商店服务创建和管理提交](create-and-manage-submissions-using-windows-store-services.md)
* [使用 Windows 应用商店提交 API 获取应用数据](get-app-data.md)
* [开发人员中心仪表板中的应用提交](https://msdn.microsoft.com/windows/uwp/publish/app-submissions)



<!--HONumber=Nov16_HO1-->


