---
author: Xansky
ms.assetid: C7428551-4B31-4259-93CD-EE229007C4B8
description: 在 Microsoft Store 提交 API 中使用这些方法来管理已注册到你的合作伙伴中心帐户的应用的提交。
title: 管理应用提交
ms.author: mhopkins
ms.date: 04/30/2018
ms.topic: article
keywords: windows 10, uwp, Microsoft Store 提交 API, 应用提交
ms.localizationpriority: medium
ms.openlocfilehash: 76bc7932665e3f9893c6f0aa9644b9edc07a6dcf
ms.sourcegitcommit: e38b334edb82bf2b1474ba686990f4299b8f59c7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/15/2018
ms.locfileid: "6836139"
---
# <a name="manage-app-submissions"></a>管理应用提交

Microsoft Store 提交 API 提供可用于管理应用提交的方法，包括逐步推出软件包。 有关 Microsoft Store 提交 API 的介绍（包括使用 API 的先决条件），请参阅[使用 Microsoft Store 服务创建和管理提交](create-and-manage-submissions-using-windows-store-services.md)。

> [!IMPORTANT]
> 如果你使用 Microsoft Store 提交 API 创建应用提交，请务必对进行进一步更改提交只能通过使用 API，而不是合作伙伴中心。 如果你使用合作伙伴中心更改你最初使用 API 创建一个提交，你将不再能够更改或提交该提交使用该 API。 在某些情况下，在提交过程中无法继续进行时，提交可能会处于错误状态。 如果发生这种情况，你必须删除提交并创建新的提交。

> [!IMPORTANT]
> 你无法使用此 API 来直接向企业发布[通过适用于企业的 Microsoft Store 和适用于教育的 Microsoft Store 批量购买](../publish/organizational-licensing.md)的提交或发布 [LOB 应用](../publish/distribute-lob-apps-to-enterprises.md)的提交。 对于这两种方案中，你必须使用合作伙伴中心发布该提交。


<span id="methods-for-app-submissions" />

## <a name="methods-for-managing-app-submissions"></a>管理应用提交的方法

使用以下方法获取、创建、更新、提交或删除应用提交。 你可以使用这些方法之前，应用必须已存在于你的合作伙伴中心帐户和必须先在合作伙伴中心中创建应用的提交。 有关详细信息，请参阅[先决条件](create-and-manage-submissions-using-windows-store-services.md#prerequisites)。

<table>
<colgroup>
<col width="10%" />
<col width="30%" />
<col width="60%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">方法</th>
<th align="left">URI</th>
<th align="left">说明</th>
</tr>
</thead>
<tbody>
<tr>
<td align="left">GET</td>
<td align="left">https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/submissions/{submissionId}</td>
<td align="left"><a href="get-an-app-submission.md">获取现有的应用提交</a></td>
</tr>
<tr>
<td align="left">GET</td>
<td align="left">https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/submissions/{submissionId}/status</td>
<td align="left"><a href="get-status-for-an-app-submission.md">获取现有应用提交的状态</a></td>
</tr>
<tr>
<td align="left">POST</td>
<td align="left">https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/submissions</td>
<td align="left"><a href="create-an-app-submission.md">创建新的应用提交</a></td>
</tr>
<tr>
<td align="left">PUT</td>
<td align="left">https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/submissions/{submissionId}</td>
<td align="left"><a href="update-an-app-submission.md">更新现有的应用提交</a></td>
</tr>
<tr>
<td align="left">POST</td>
<td align="left">https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/submissions/{submissionId}/commit</td>
<td align="left"><a href="commit-an-app-submission.md">提交新建或已更新的应用提交</a></td>
</tr>
<tr>
<td align="left">DELETE</td>
<td align="left">https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/submissions/{submissionId}</td>
<td align="left"><a href="delete-an-app-submission.md">删除应用提交</a></td>
</tr>
</tbody>
</table>

<span id="create-an-app-submission">

### <a name="create-an-app-submission"></a>创建应用提交

若要创建应用提交，请遵循此过程。

1. 如果尚未开始操作，请先完成 Microsoft Store 提交 API 的所有[先决条件](create-and-manage-submissions-using-windows-store-services.md#prerequisites)。
    > [!NOTE]
    > 确保应用至少已有一个已完成的提交并且已完成[年龄分级](https://msdn.microsoft.com/windows/uwp/publish/age-ratings)信息。

2. [获取 Azure AD 访问令牌](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token)。 在 Microsoft Store 提交 API 中，必须将此访问令牌传递给相关方法。 获取访问令牌后，在它到期前，你有 60 分钟的使用时间。 该令牌到期后，可以获取新的令牌。

3. 通过执行 Microsoft Store 提交 API 中的以下方法[创建应用提交](create-an-app-submission.md)。 此方法创建新的正在进行的提交，这是你上次发布的提交的副本。

    ```
    POST https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/submissions
    ```

    响应正文包含[应用提交](#app-submission-object)资源（包括新提交的 ID）、共享访问签名 (SAS) URI（用于将提交的任何相关文件上传到 Azure Blob 存储，如应用包、应用一览图像、预告片文件等）和新提交的所有数据（如应用一览和定价信息）。

    > [!NOTE]
    > SAS URI 提供对 Azure 存储中的安全资源的访问权限（无需帐户密钥）。 有关 SAS URI 以及借助 Azure Blob 使用这些 URI 的背景信息，请参阅[共享访问签名，第 1 部分：了解 SAS 模型](https://azure.microsoft.com/documentation/articles/storage-dotnet-shared-access-signature-part-1)和[共享访问签名，第 2 部分：使用 Blob 存储创建和使用 SAS](https://azure.microsoft.com/documentation/articles/storage-dotnet-shared-access-signature-part-2/)。

4. 如果要为提交添加新的程序包、应用一览图像或预告片文件，请[准备应用包](https://msdn.microsoft.com/windows/uwp/publish/app-package-requirements)并[准备应用屏幕截图、图像和预告片](https://msdn.microsoft.com/windows/uwp/publish/app-screenshots-and-images)。 将所有这些文件添加到 ZIP 存档。

5. 使用新提交所需的任何更改来修改[应用提交](#app-submission-object)数据，并执行以下方法来[更新应用提交](update-an-app-submission.md)。

    ```
    PUT https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/submissions/{submissionId}
    ```
      > [!NOTE]
      > 如果要为提交添加新的文件，请确保更新提交数据以便在 ZIP 存档中引用这些文件的名称和相对路径。

4. 如果要为提交添加新的程序包、应用一览图像或预告片文件，请使用 SAS URI 将 ZIP 存档上传到 [Azure Blob 存储](https://docs.microsoft.com/azure/storage/storage-introduction#blob-storage)，该 URI 已在之前调用的 POST 方法的响应正文中提供。 你可以使用不同的 Azure 库在多个平台上进行此操作，包括：

    * [适用于 .NET 的 Azure 存储客户端库](https://docs.microsoft.com/azure/storage/storage-dotnet-how-to-use-blobs)
    * [适用于 Java 的 Azure 存储 SDK](https://docs.microsoft.com/azure/storage/storage-java-how-to-use-blob-storage)
    * [适用于 Python 的 Azure 存储 SDK](https://docs.microsoft.com/azure/storage/storage-python-how-to-use-blob-storage)

    以下 C# 代码示例演示如何在用于 .NET 的 Azure 存储客户端库中使用 [CloudBlockBlob](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.blob.cloudblockblob.aspx) 类将 ZIP 存档上载到 Azure Blob 存储。 此示例假定 ZIP 存档已写入流对象。

    ```csharp
    string sasUrl = "https://productingestionbin1.blob.core.windows.net/ingestion/26920f66-b592-4439-9a9d-fb0f014902ec?sv=2014-02-14&sr=b&sig=usAN0kNFNnYE2tGQBI%2BARQWejX1Guiz7hdFtRhyK%2Bog%3D&se=2016-06-17T20:45:51Z&sp=rwl";
    Microsoft.WindowsAzure.Storage.Blob.CloudBlockBlob blockBob =
        new Microsoft.WindowsAzure.Storage.Blob.CloudBlockBlob(new System.Uri(sasUrl));
    await blockBob.UploadFromStreamAsync(stream);
    ```

5. 通过执行以下方法[确认应用提交](commit-an-app-submission.md)。 这将向合作伙伴中心警报你完成提交，更新现在应该应用到你的帐户。

    ```
    POST https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/submissions/{submissionId}/commit
    ```

6. 通过执行以下方法来检查提交状态以[获取应用提交的状态](get-status-for-an-app-submission.md)。

    ```
    GET https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/submissions/{submissionId}/status
    ```

    若要确认提交状态，请查看响应正文中的 *status* 值。 如果请求成功，此值应该从 **CommitStarted** 更改为 **PreProcessing**；如果请求中存在错误，此值应该更改为 **CommitFailed**。 如果存在错误，*statusDetails* 字段将包含有关错误的更多详细信息。

7. 在提交成功完成之后，提交会发送至应用商店以供引入。 你可以继续监视提交进度，通过使用前面的方法，或者通过访问合作伙伴中心。

<span id="manage-gradual-package-rollout">

## <a name="methods-for-managing-a-gradual-package-rollout"></a>管理逐步软件包推出的方法

可在应用提交中逐步向 Windows 10 上一定比例的应用客户推出已更新的软件包。 这使你可以监视特定软件包的反馈和分析数据，从而确保在更广泛地推出更新前对此更新放心。 可更改已发布提交的推出百分比（或终止更新），而无需创建新提交。 有关详细信息，包括有关如何启用和管理合作伙伴中心中的逐步软件包推出，请参阅[本文](../publish/gradual-package-rollout.md)说明。

若要以编程方式启用应用提交的逐步软件包推出，请遵循此过程使用 Microsoft Store 提交 API 中的以下方法：

  1. [创建应用提交](create-an-app-submission.md)或[获取现有应用提交](get-an-app-submission.md)。
  2. 在响应数据中，找到 [packageRollout](#package-rollout-object) 资源，将 *isPackageRollout* 字段设置为 **true**，然后将 *packageRolloutPercentage* 字段设置为应获取已更新的软件包的应用客户百分比。
  3. 将已更新的应用提交数据传递到[更新应用提交](update-an-app-submission.md)方法。

针对应用提交启用逐步软件包推出后，你可以使用以下方法以编程方式获取、更新、终止或完成逐步推出。

<table>
<colgroup>
<col width="10%" />
<col width="30%" />
<col width="60%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">方法</th>
<th align="left">URI</th>
<th align="left">说明</th>
</tr>
</thead>
<tbody>
<tr>
<td align="left">GET</td>
<td align="left">https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/submissions/{submissionId}/packagerollout</td>
<td align="left"><a href="get-package-rollout-info-for-an-app-submission.md">获取应用提交的逐步推出信息</a></td>
</tr>
<tr>
<td align="left">POST</td>
<td align="left">https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/submissions/{submissionId}/updatepackagerolloutpercentage</td>
<td align="left"><a href="update-the-package-rollout-percentage-for-an-app-submission.md">更新应用提交的逐步推出百分比</a></td>
</tr>
<tr>
<td align="left">POST</td>
<td align="left">https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/submissions/{submissionId}/haltpackagerollout</td>
<td align="left"><a href="halt-the-package-rollout-for-an-app-submission.md">终止应用提交的逐步推出</a></td>
</tr>
<tr>
<td align="left">POST</td>
<td align="left">https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/submissions/{submissionId}/finalizepackagerollout</td>
<td align="left"><a href="finalize-the-package-rollout-for-an-app-submission.md">完成应用提交的逐步推出</a></td>
</tr>
</tbody>
</table>


## <a name="code-examples-for-managing-app-submissions"></a>管理应用提交的代码示例

下文中提供了详细的代码示例，演示了如何以多种不同的编程语言创建应用提交：

* [C# 示例：应用、加载项和外部测试版的提交](csharp-code-examples-for-the-windows-store-submission-api.md)
* [C# 示例：使用游戏选项和预告片的应用提交](csharp-code-examples-for-submissions-game-options-and-trailers.md)
* [Java 示例：应用、加载项和外部测试版的提交](java-code-examples-for-the-windows-store-submission-api.md)
* [Java 示例：使用游戏选项和预告片的应用提交](java-code-examples-for-submissions-game-options-and-trailers.md)
* [Python 示例：应用、加载项和外部测试版的提交](python-code-examples-for-the-windows-store-submission-api.md)
* [Python 示例：使用游戏选项和预告片的应用提交](python-code-examples-for-submissions-game-options-and-trailers.md)

## <a name="storebroker-powershell-module"></a>StoreBroker PowerShell 模块

除了直接调用 Microsoft Store 提交 API 的方式外，我们还提供在该 API 之上实现命令行界面的开源 PowerShell 模块。 此模块称为 [StoreBroker](https://aka.ms/storebroker)。 你可以使用此模块从命令行管理你的应用、外部测试版和加载项提交，而不是通过直接调用 Microsoft Store 提交 API，或者你可以浏览源以查看更多有关如何调用此 API 的示例。 在 Microsoft 内，StoreBroker 模块作为将许多第一方应用程序提交到 Microsoft Store 的主要方式被频繁使用。

有关详细信息，请参阅我们 [GitHub 上的 StoreBroker 页面](https://aka.ms/storebroker)。

<span/>

## <a name="data-resources"></a>数据资源
用于管理应用提交的 Microsoft Store 提交 API 方法使用以下 JSON 数据资源。

<span id="app-submission-object" />

### <a name="app-submission-resource"></a>应用提交资源

此资源描述应用提交。

```json
{
  "id": "1152921504621243540",
  "applicationCategory": "BooksAndReference_EReader",
  "pricing": {
    "trialPeriod": "FifteenDays",
    "marketSpecificPricings": {},
    "sales": [],
    "priceId": "Tier2",
    "isAdvancedPricingModel": true
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
            "description": "Main page",
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

此资源具有以下值。

| 值      | 类型   | 描述      |
|------------|--------|-------------------|
| id            | 字符串  | 提交的 ID。 此 ID 包含在 [create an app submission](create-an-app-submission.md)、[get all apps](get-all-apps.md) 和 [get an app](get-an-app.md) 请求的响应数据中。 对于在合作伙伴中心中创建的提交，此 ID 也包含在合作伙伴中心中的提交页面的 URL 中可用。  |
| applicationCategory           | 字符串  |   为应用指定[类别和/或子类别](https://msdn.microsoft.com/windows/uwp/publish/category-and-subcategory-table)的字符串。 通过下划线“_”字符将类别和子类别组合为单个字符串，例如 **BooksAndReference_EReader**。      |  
| pricing           |  对象  | 包含应用的定价信息的[定价资源](#pricing-object)。        |   
| visibility           |  字符串  |  应用的可见性。 这可以是以下值之一： <ul><li>Hidden</li><li>Public</li><li>Private</li><li>NotSet</li></ul>       |   
| targetPublishMode           | 字符串  | 提交的发布模式。 这可以是以下值之一： <ul><li>Immediate</li><li>Manual</li><li>SpecificDate</li></ul> |
| targetPublishDate           | 字符串  | 提交的发布日期采用 ISO 8601 格式（如果 *targetPublishMode* 设为“SpecificDate”）。  |  
| listings           |   对象  |  键值对字典，其中每个键为国家/地区代码，而每个值为包含应用列表信息的[列表资源](#listing-object)。       |   
| hardwarePreferences           |  数组  |   一组用于定义应用的[硬件首选项](https://msdn.microsoft.com/windows/uwp/publish/enter-app-properties#hardware_preferences)的字符串。 这可以是以下值之一： <ul><li>Touch</li><li>Keyboard</li><li>Mouse</li><li>Camera</li><li>NfcHce</li><li>Nfc</li><li>BluetoothLE</li><li>Telephony</li></ul>     |   
| automaticBackupEnabled           |  布尔型  |   指示 Windows 是否可以将应用的数据包含在 OneDrive 的自动备份中。 有关详细信息，请参阅[应用声明](https://msdn.microsoft.com/windows/uwp/publish/app-declarations)。   |   
| canInstallOnRemovableMedia           |  布尔型  |   指示客户是否可以将应用安装到可移动存储。 有关详细信息，请参阅[应用声明](https://msdn.microsoft.com/windows/uwp/publish/app-declarations)。     |   
| isGameDvrEnabled           |  布尔型 |   指示是否可以为应用启用游戏 DVR。    |   
| gamingOptions           |  数组 |   一个包含[游戏选项资源](#gaming-options-object)的数组，用于为应用定义游戏相关设置。     |   
| hasExternalInAppProducts           |     布尔型          |   指示应用是否允许用户在 Microsoft Store 商务系统之外进行购买。 有关详细信息，请参阅[应用声明](https://msdn.microsoft.com/windows/uwp/publish/app-declarations)。     |   
| meetAccessibilityGuidelines           |    布尔型           |  指示应用是否经测试符合辅助功能准则。 有关详细信息，请参阅[应用声明](https://msdn.microsoft.com/windows/uwp/publish/app-declarations)。      |   
| notesForCertification           |  字符串  |   包含应用的[认证说明](https://msdn.microsoft.com/windows/uwp/publish/notes-for-certification)。    |    
| status           |   字符串  |  提交的状态。 这可以是以下值之一： <ul><li>None</li><li>Canceled</li><li>PendingCommit</li><li>CommitStarted</li><li>CommitFailed</li><li>PendingPublication</li><li>Publishing</li><li>Published</li><li>PublishFailed</li><li>PreProcessing</li><li>PreProcessingFailed</li><li>Certification</li><li>CertificationFailed</li><li>Release</li><li>ReleaseFailed</li></ul>      |    
| statusDetails           |   对象  | 包含有关提交状态的附加详细信息的[状态详细信息资源](#status-details-object)，其中包括任何错误的相关信息。       |    
| fileUploadUrl           |   字符串  | 用于为提交上传任何程序包的共享访问签名 (SAS) URI。 如果要为提交添加新的程序包、应用一览图像或预告片文件，请将包含这些程序包和图像的 ZIP 存档上传到此 URI。 有关详细信息，请参阅[创建应用提交](#create-an-app-submission)。       |    
| applicationPackages           |   数组  | 提供有关提交中每个包的详细信息的[应用程序包资源](#application-package-object)数组。 |    
| packageDeliveryOptions    | 对象  | 包含提交的逐步软件包推出和强制更新设置的[软件包递送选项资源](#package-delivery-options-object)。  |
| enterpriseLicensing           |  字符串  |  [企业授权值](#enterprise-licensing)的其中一个值，它指示应用的企业授权行为。  |    
| allowMicrosoftDecideAppAvailabilityToFutureDeviceFamilies           |  布尔型   |  指示是否允许 Microsoft [将应用提供给未来 Windows10 设备系列](https://msdn.microsoft.com/windows/uwp/publish/set-app-pricing-and-availability#windows-10-device-families)。    |    
| allowTargetFutureDeviceFamilies           | 对象   |  键值对字典，其中每个键为 [Windows10 设备系列](https://msdn.microsoft.com/windows/uwp/publish/set-app-pricing-and-availability#windows-10-device-families)，而每个值为布尔值，指示是否允许应用面向指定的设备系列。     |    
| friendlyName           |   字符串  |  提交，在合作伙伴中心中所示的友好名称。 当你创建提交时，系统会为你生成此值。       |  
| trailers           |  数组 |   一个包含最多 15 个[预告片资源](#trailer-object)的数组，用于表示应用一览的视频预告片。<br/><br/>   |  


<span id="pricing-object" />

### <a name="pricing-resource"></a>定价资源

此资源包含应用的定价信息。 此资源具有以下值。

| 值           | 类型    | 说明        |
|-----------------|---------|------|
|  trialPeriod               |    字符串     |  一个指定应用试用期的字符串。 这可以是以下值之一： <ul><li>NoFreeTrial</li><li>OneDay</li><li>TrialNeverExpires</li><li>SevenDays</li><li>FifteenDays</li><li>ThirtyDays</li></ul>    |
|  marketSpecificPricings               |    对象     |  键值对字典，其中每个键为两个字母的 ISO 3166-1 二字母国家/地区代码，而每个值为[价格段](#price-tiers)。 这些项表示[应用在特定市场中的自定义价格](https://msdn.microsoft.com/windows/uwp/publish/define-pricing-and-market-selection#markets-and-custom-prices)。 此字典中的任何项替代 *priceId* 值针对特定市场所指定的基价。      |     
|  sales               |   数组      |  **已弃用**。 包含应用销售信息的[销售资源](#sale-object)数组。   |     
|  priceId               |   字符串      |  用于指定应用[基价](https://msdn.microsoft.com/windows/uwp/publish/define-pricing-and-market-selection#base-price)的[价格段](#price-tiers)。   |     
|  isAdvancedPricingModel               |   布尔型      |  如果为 **true**，你的开发人员帐户可以使用从 0.99 美元到 1999.99 美元的扩展价格段。 如果为 **false**，你的开发人员帐户可以使用从 0.99 美元到 999.99 美元的原始价格段。 有关其他价格段的详细信息，请参阅[价格段](#price-tiers)。<br/><br/>**注意**&nbsp;&nbsp;此字段为只读字段。   |


<span id="sale-object" />

### <a name="sale-resource"></a>销售资源

此资源包含应用的销售信息。

> [!IMPORTANT]
> **销售**资源不再受支持，并且当前不能使用 Microsoft Store 提交 API 获取或修改应用提交的销售数据。 将来，我们将更新 Microsoft Store 提交 API，以引入以编程方式访问应用提交的销售信息的新方法。
>    * 调用 [GET 方法以获取应用提交](get-an-app-submission.md)后，*销售*值将为空。 你可以继续使用合作伙伴中心以获取应用提交的销售数据。
>    * 调用 [PUT 方法更新应用提交](update-an-app-submission.md)时，将忽略*销售*值中的信息。 你可以继续使用合作伙伴中心来更改应用提交的销售数据。

此资源具有以下值。

| 值           | 类型    | 说明    |
|-----------------|---------|------|
|  name               |    字符串     |   销售的名称。    |     
|  basePriceId               |   字符串      |  要用于销售基价的[价格段](#price-tiers)。    |     
|  startDate               |   字符串      |   采用 ISO 8601 格式的销售的开始日期。  |     
|  endDate               |   字符串      |  采用 ISO 8601 格式的销售的结束日期。      |     
|  marketSpecificPricings               |   对象      |   键值对字典，其中每个键为两个字母的 ISO 3166-1 二字母国家/地区代码，而每个值为[价格段](#price-tiers)。 这些项表示[应用在特定市场中的自定义价格](https://msdn.microsoft.com/windows/uwp/publish/define-pricing-and-market-selection#markets-and-custom-prices)。 此字典中的任何项替代 *basePriceId* 值针对特定市场所指定的基价。    |


<span id="listing-object" />

### <a name="listing-resource"></a>应用一览资源

此资源包含应用的列表信息。 此资源具有以下值。

| 值           | 类型    | 说明                  |
|-----------------|---------|------|
|  baseListing               |   对象      |  应用的[基本列表](#base-listing-object)信息，它定义了所有平台的默认列表信息。   |     
|  platformOverrides               | 对象 |   键值对字典，其中每个键为字符串，用于标识要替代其列表信息的平台；而每个值为[基本列表](#base-listing-object)资源（仅将描述中的值包含在标题中），用于指定要为指定平台替代的列表信息。 键可以具有以下值： <ul><li>Unknown</li><li>Windows80</li><li>Windows81</li><li>WindowsPhone71</li><li>WindowsPhone80</li><li>WindowsPhone81</li></ul>     |      |     

<span id="base-listing-object" />

### <a name="base-listing-resource"></a>基本列表资源

此资源包含应用的基本列表信息。 此资源具有以下值。

| 值           | 类型    | 说明       |
|-----------------|---------|------|
|  copyrightAndTrademarkInfo                |   字符串      |  可选的[版权和/或商标信息](https://msdn.microsoft.com/windows/uwp/publish/create-app-descriptions#copyright-and-trademark-info)。  |
|  keywords                |  数组       |  [关键字](https://msdn.microsoft.com/windows/uwp/publish/create-app-descriptions#keywords)数组，用于帮助应用出现在搜索结果中。    |
|  licenseTerms                |    字符串     | 可选的应用[许可条款](https://msdn.microsoft.com/windows/uwp/publish/create-app-descriptions#additional-license-terms)。     |
|  privacyPolicy                |   字符串      |   此值已作废。 若要设置或更改你的应用的隐私策略 URL，你必须执行此操作在合作伙伴中心中的[属性](../publish/enter-app-properties.md#privacy-policy-url)页上。 在调用提交 API 时可以忽略此值。 如果设置此值，它将被忽略。       |
|  supportContact                |   字符串      |  此值已作废。 若要设置或更改支持部门联系 URL 或电子邮件地址为你的应用，你必须执行此操作在合作伙伴中心中的[属性](../publish/enter-app-properties.md#support-contact-info)页上。 在调用提交 API 时可以忽略此值。 如果设置此值，它将被忽略。        |
|  websiteUrl                |   字符串      |  此值已作废。 若要设置或更改你的应用的网页的 URL，你必须执行此操作在合作伙伴中心中的[属性](../publish/enter-app-properties.md#website)页上。 在调用提交 API 时可以忽略此值。 如果设置此值，它将被忽略。      |    
|  说明               |    字符串     |   应用一览的[说明](https://msdn.microsoft.com/windows/uwp/publish/create-app-descriptions#description)。   |     
|  features               |    数组     |  一个最多 20 个字符串的数组，用于列出应用的[功能](https://msdn.microsoft.com/windows/uwp/publish/create-app-descriptions#app-features)。     |
|  releaseNotes               |  字符串       |  应用的[发行说明](https://msdn.microsoft.com/windows/uwp/publish/create-app-descriptions#release-notes)。    |
|  images               |   数组      |  应用一览的[图像和图标](#image-object)资源的数组。  |
|  recommendedHardware               |   数组      |  一个最多 11 个字符串的数组，用于列出应用的[推荐硬件配置](../publish/create-app-store-listings.md#additional-information)。     |
|  minimumHardware               |     字符串    |  一个最多 11 个字符串的数组，用于列出应用的[最低硬件配置](../publish/create-app-store-listings.md#additional-information)。    |  
|  title               |     字符串    |   应用一览的标题。   |  
|  shortDescription               |     字符串    |  仅用于游戏。 此说明出现在 Xbox One 上游戏中心的**信息**部分，帮助客户了解有关你的游戏的更多信息。   |  
|  shortTitle               |     字符串    |  产品名称的较短版本。 如果提供，此简短名称可以替代产品的完整标题出现在 Xbox One 上的不同位置（安装过程中、成就中等）。    |  
|  sortTitle               |     字符串    |   如果你的产品可以通过不同方式按字母顺序排序，你可以在此处输入另一个版本。 这可以帮助客户在搜索时更快地找到此产品。    |  
|  voiceTitle               |     字符串    |   产品的替代名称，如果提供，在使用 Kinect 或耳机时可以用于 Xbox One 的音频体验。    |  
|  devStudio               |     字符串    |   若要在应用一览中包含**开发者**字段，请指定此值。 （不管你有没有提供 *devStudio* 值，**发布者**字段都将列出与你的帐户关联的发布者显示名称。）    |  

<span id="image-object" />

### <a name="image-resource"></a>图像资源

此资源包含应用一览的图像和图标数据。 有关应用一览的图像和图标的详细信息，请参阅[应用屏幕截图和图像](../publish/app-screenshots-and-images.md)。 此资源具有以下值。

| 值           | 类型    | 说明           |
|-----------------|---------|------|
|  fileName               |    字符串     |   ZIP 存档中已为提交上载的图像文件的名称。    |     
|  fileStatus               |   字符串      |  图像文件的状态。 这可以是以下值之一： <ul><li>None</li><li>PendingUpload</li><li>Uploaded</li><li>PendingDelete</li></ul>   |
|  id  |  字符串  | 图像的 ID。 此值由合作伙伴中心提供。  |
|  description  |  字符串  | 图像的描述。  |
|  imageType  |  字符串  | 指示图像的类型。 当前支持以下字符串。 <p/>[屏幕截图图像](../publish/app-screenshots-and-images.md#screenshots)： <ul><li>屏幕截图（对于桌面屏幕截图，请使用此值）</li><li>MobileScreenshot</li><li>XboxScreenshot</li><li>SurfaceHubScreenshot</li><li>HoloLensScreenshot</li></ul><p/>[应用商店徽标](../publish/app-screenshots-and-images.md#store-logos)：<ul><li>StoreLogo9x16 </li><li>StoreLogoSquare</li><li>图标（对于 1:1 300 x 300 像素徽标，请使用此值）</li></ul><p/>[促销图像](../publish/app-screenshots-and-images.md#promotional-images)： <ul><li>PromotionalArt16x9</li><li>PromotionalArtwork2400X1200</li></ul><p/>[Xbox 图像](../publish/app-screenshots-and-images.md#xbox-images)： <ul><li>XboxBrandedKeyArt</li><li>XboxTitledHeroArt</li><li>XboxFeaturedPromotionalArt</li></ul><p/>[可选的促销图像](../publish/app-screenshots-and-images.md#optional-promotional-images)： <ul><li>SquareIcon358X358</li><li>BackgroundImage1000X800</li><li>PromotionalArtwork414X180</li></ul><p/> <!-- The following strings are also recognized for this field, but they correspond to image types that are no longer for listings in the Store.<ul><li>PromotionalArtwork846X468</li><li>PromotionalArtwork558X756</li><li>PromotionalArtwork414X468</li><li>PromotionalArtwork558X558</li><li>WideIcon358X173</li><li>Unknown</li></ul> -->   |


<span id="gaming-options-object" />

### <a name="gaming-options-resource"></a>游戏选项资源

此资源包含应用的游戏相关设置。 此资源中的值对应于[游戏设置](../publish/enter-app-properties.md#game-settings)中的合作伙伴中心提交。

```json
{
  "gamingOptions": [
    {
      "genres": [
        "Games_ActionAndAdventure",
        "Games_Casino"
      ],
      "isLocalMultiplayer": true,
      "isLocalCooperative": true,
      "isOnlineMultiplayer": false,
      "isOnlineCooperative": false,
      "localMultiplayerMinPlayers": 2,
      "localMultiplayerMaxPlayers": 12,
      "localCooperativeMinPlayers": 2,
      "localCooperativeMaxPlayers": 12,
      "isBroadcastingPrivilegeGranted": true,
      "isCrossPlayEnabled": false,
      "kinectDataForExternal": "Enabled"
    }
  ],
}
```

此资源具有以下值。

| 值           | 类型    | 说明        |
|-----------------|---------|------|
|  genres               |    数组     |  包含一个或多个下列字符串的数组，用于描述游戏类型： <ul><li>Games_ActionAndAdventure</li><li>Games_CardAndBoard</li><li>Games_Casino</li><li>Games_Educational</li><li>Games_FamilyAndKids</li><li>Games_Fighting</li><li>Games_Music</li><li>Games_Platformer</li><li>Games_PuzzleAndTrivia</li><li>Games_RacingAndFlying</li><li>Games_RolePlaying</li><li>Games_Shooter</li><li>Games_Simulation</li><li>Games_Sports</li><li>Games_Strategy</li><li>Games_Word</li></ul>    |
|  isLocalMultiplayer               |    布尔     |  指示游戏是否支持本地多人游戏。      |     
|  isLocalCooperative               |   布尔      |  指示游戏是否支持本地合作游戏。    |     
|  isOnlineMultiplayer               |   布尔      |  指示游戏是否支持在线多人游戏。    |     
|  isOnlineCooperative               |   布尔      |  指示游戏是否支持在线合作游戏。    |     
|  localMultiplayerMinPlayers               |   整数      |   指定游戏支持进行本地多人游戏的最小玩家人数。   |     
|  localMultiplayerMaxPlayers               |   整数      |   指定游戏支持进行本地多人游戏的最大玩家人数。  |     
|  localCooperativeMinPlayers               |   整数      |   指定游戏支持进行本地合作游戏的最小玩家人数。  |     
|  localCooperativeMaxPlayers               |   整数      |   指定游戏支持进行本地合作游戏的最大玩家人数。  |     
|  isBroadcastingPrivilegeGranted               |   布尔      |  指示游戏是否支持广播。   |     
|  isCrossPlayEnabled               |   布尔      |   指示游戏是否支持 Windows 10 电脑和 Xbox 玩家之间的多人游戏会话。  |     
|  kinectDataForExternal               |   字符串      |  指定以下字符串值之一，表示游戏是否可以收集 Kinect 数据并将其发送到外部服务： <ul><li>NotSet</li><li>Unknown</li><li>Enabled</li><li>禁用</li></ul>   |

> [!NOTE]
> Microsoft Store 提交 API 首次面向开发人员发布后，在 2017 年 5 月添加了 *GamingOptions* 资源。 如果你在此资源推出前通过提交 API 为应用创建了提交并且此提交仍在进行中，则在成功确认提交或删除提交前，此资源将为 null 且无法为应用提交。 如果 *gamingOptions* 资源不可用于应用提交，则 [get an app](get-an-app.md) 方法返回的[应用程序资源](get-app-data.md#application_object) 的 *hasAdvancedListingPermission* 字段为 false。

<span id="status-details-object" />

### <a name="status-details-resource"></a>状态详细信息资源

此资源包含有关提交状态的附加详细信息。 此资源具有以下值。

| 值           | 类型    | 说明         |
|-----------------|---------|------|
|  errors               |    对象     |   包含提交的错误详细信息的[状态详细信息资源](#status-detail-object)数组。    |     
|  warnings               |   对象      | 包含提交的警告详细信息的[状态详细信息资源](#status-detail-object)数组。      |
|  certificationReports               |     对象    |   提供对提交的认证报告数据的访问权限的[认证报告资源](#certification-report-object)数组。 如果认证失败，可检查这些报告，获取详细信息。   |  


<span id="status-detail-object" />

### <a name="status-detail-resource"></a>状态详细信息资源

此资源包含关于提交的任何相关错误或警告的附加详细信息。 此资源具有以下值。

| 值           | 类型    | 说明        |
|-----------------|---------|------|
|  code               |    字符串     |   描述错误或警告类型的[提交状态代码](#submission-status-code)。   |     
|  details               |     字符串    |  包含有关问题的更多详细信息的消息。     |


<span id="application-package-object" />

### <a name="application-package-resource"></a>应用程序包资源

此资源包含有关提交的应用包的详细信息。

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

> [!NOTE]
> 当调用[更新应用提交](update-an-app-submission.md)法时，请求正文中仅需要此对象的 *fileName*、*fileStatus*、*minimumDirectXVersion* 和 *minimumSystemRam* 值。 其他值由合作伙伴中心进行填充。

| 值           | 类型    | 说明                   |
|-----------------|---------|------|
| fileName   |   字符串      |  程序包的名称。    |  
| fileStatus    | 字符串    |  程序包的状态。 这可以是以下值之一： <ul><li>None</li><li>PendingUpload</li><li>Uploaded</li><li>PendingDelete</li></ul>    |  
| id    |  字符串   |  唯一标识程序包的 ID。 此值由合作伙伴中心提供。   |     
| version    |  字符串   |  应用包的版本。 有关详细信息，请参阅[程序包版本编号](https://msdn.microsoft.com/windows/uwp/publish/package-version-numbering)。   |   
| architecture    |  字符串   |  程序包的体系结构（例如 ARM）。   |     
| languages    | 数组    |  语言代码数组，用于指示应用支持的语言。 有关详细信息，请参阅[支持的语言](https://msdn.microsoft.com/windows/uwp/publish/supported-languages)。    |     
| capabilities    |  数组   |  程序包所需的功能数组。 有关功能的详细信息，请参阅[应用功能声明](https://msdn.microsoft.com/windows/uwp/packaging/app-capability-declarations)。   |     
| minimumDirectXVersion    |  字符串   |  应用包支持的最低 DirectX 版本。 这只能为面向 Windows 8.x 的应用设置。 对于面向其他操作系统版本的应用，调用 [update an app submission交](update-an-app-submission.md) 方法时，此值必须存在，但会忽略你指定的值。 这可以是以下值之一： <ul><li>None</li><li>DirectX93</li><li>DirectX100</li></ul>   |     
| minimumSystemRam    | 字符串    |  应用包所需的最小 RAM。 这只能为面向 Windows 8.x 的应用设置。 对于面向其他操作系统版本的应用，调用 [update an app submission交](update-an-app-submission.md) 方法时，此值必须存在，但会忽略你指定的值。 这可以是以下值之一： <ul><li>无</li><li>Memory2GB</li></ul>   |       
| targetDeviceFamilies    | 数组    |  一个字符串数组，它表示程序包所面向的设备系列。 此值仅用于面向 Windows10 的程序包；对于面向早期版本的程序包，此值具有值 **None**。 Windows10 程序包当前支持以下设备系列字符串，其中 *{0}* 是 Windows10 版本字符串（例如 10.0.10240.0、10.0.10586.0 或 10.0.14393.0）： <ul><li>Windows.Universal 最低版本 *{0}*</li><li>Windows.Desktop 最低版本 *{0}*</li><li>Windows.Mobile 最低版本 *{0}*</li><li>Windows.Xbox 最低版本 *{0}*</li><li>Windows.Holographic 最低版本 *{0}*</li></ul>   |    

<span/>

<span id="certification-report-object" />

### <a name="certification-report-resource"></a>认证报告资源

此资源提供对提交的认证报告数据的访问权限。 此资源具有以下值。

| 值           | 类型    | 说明             |
|-----------------|---------|------|
|     date            |    字符串     |  日期和报告生成的时间，采用 ISO 8601 格式。    |
|     reportUrl            |    字符串     |  用于访问报告的 URL。    |


<span id="package-delivery-options-object" />

### <a name="package-delivery-options-resource"></a>软件包递送选项资源

此资源包含提交的逐步软件包推出和强制更新设置。

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
| packageRollout   |   对象      |  包含提交的逐步软件包推出设置的[软件包推出资源](#package-rollout-object)。   |  
| isMandatoryUpdate    | 布尔值    |  指示是否要将此提交中的软件包视为对自行安装的应用更新强制。 有关自行安装的应用更新的强制软件包的详细信息，请参阅[为应用下载并安装包更新](../packaging/self-install-package-updates.md)。    |  
| mandatoryUpdateEffectiveDate    |  日期   |  此提交中的软件包变为强制的日期和时间，采用 ISO 8601 格式和 UTC 时区。   |        

<span id="package-rollout-object" />

### <a name="package-rollout-resource"></a>软件包推出资源

此资源包含提交的逐步[软件包推出设置](#manage-gradual-package-rollout)。 此资源具有以下值。

| 值           | 类型    | 说明        |
|-----------------|---------|------|
| isPackageRollout   |   布尔值      |  指示是否为提交启用逐步软件包推出。    |  
| packageRolloutPercentage    | 浮点数    |  将在逐步推出中收到软件包的用户百分比。    |  
| packageRolloutStatus    |  字符串   |  以下指示逐步软件包推出状态的字符串之一： <ul><li>PackageRolloutNotStarted</li><li>PackageRolloutInProgress</li><li>PackageRolloutComplete</li><li>PackageRolloutStopped</li></ul>  |  
| fallbackSubmissionId    |  字符串   |  将由不获取逐步推出软件包的客户接收的提交 ID。   |          

> [!NOTE]
> *PackageRolloutStatus*和*fallbackSubmissionId*值由合作伙伴中心分配，但不是由开发人员设置。 如果已将这些值包括在请求正文中，则将忽略这些值。

<span id="trailer-object" />

### <a name="trailers-resource"></a>预告片资源

此资源表示应用一览的视频预告片。 此资源中的值对应于合作伙伴中心中的提交的[预告片](../publish/app-screenshots-and-images.md#trailers)选项。

可以向[应用提交资源](#app-submission-object)中的 *trailers* 数组添加最多 15 个预告片资源。 要为提交上传预告片视频文件和缩略图，请将这些文件添加到包含提交的程序包和应用一览图像的 ZIP 存档中，然后将此 ZIP 存档上传到提交的共享访问签名 (SAS) URI。 有关将 ZIP 存档上传到 SAS URI 的详细信息，请参阅[创建应用提交](#create-an-app-submission)。

```json
{
  "trailers": [
    {
      "id": "1158943556954955699",
      "videoFileName": "Trailers\\ContosoGameTrailer.mp4",
      "videoFileId": "1159761554639123258",
      "trailerAssets": {
        "en-us": {
          "title": "Contoso Game",
          "imageList": [
            {
              "fileName": "Images\\ContosoGame-Thumbnail.png",
              "id": "1155546904097346923",
              "description": "This is a still image from the video."
            }
          ]
        }
      }
    }
  ]
}
```

此资源具有以下值。

| 值           | 类型    | 描述        |
|-----------------|---------|------|
|  id               |    字符串     |   预告片的 ID。 此值由合作伙伴中心提供。   |
|  videoFileName               |    字符串     |    包含提交文件的 ZIP 存档中预告片视频文件的名称。    |     
|  videoFileId               |   字符串      |  预告片视频文件的 ID。 此值由合作伙伴中心提供。   |     
|  trailerAssets               |   对象      |  键值对字典，其中每个键为语言代码，每个值为[预告片资产资源](#trailer-assets-object)（包含预告片的其他区域设置特定资产）。 有关支持的语言代码的详细信息，请参阅[支持的语言](https://msdn.microsoft.com/windows/uwp/publish/supported-languages)。    |     

> [!NOTE]
> Microsoft Store 提交 API 首次面向开发人员发布后，在 2017 年 5 月添加了 *trailers* 资源。 如果你在此资源推出前通过提交 API 为应用创建了提交并且此提交仍在进行中，则在成功确认提交或删除提交前，此资源将为 null 且无法为应用提交。 如果 *trailers* 资源不可用于应用提交，则 [get an app](get-an-app.md) 方法返回的[应用程序资源](get-app-data.md#application_object) 的 *hasAdvancedListingPermission* 字段为 false。

<span id="trailer-assets-object" />

### <a name="trailer-assets-resource"></a>预告片资产资源

此资源包含在[预告片资源](#trailer-object)中定义的预告片的其他区域设置特定资产。 此资源具有以下值。

| 值           | 类型    | 说明        |
|-----------------|---------|------|
| title   |   字符串      |  预告片的本地化标题。 当用户以全屏模式播放预告片时，显示此标题。     |  
| imageList    | 数组    |   包含[图像](#image-for-trailer-object)资源的数组，用于为预告片提供缩略图。 此数组只能包含一个[图像](#image-for-trailer-object)资源。  |   


<span id="image-for-trailer-object" />

### <a name="image-resource-for-a-trailer"></a>（预告片的）图像资源

此资源描述了预告片的缩略图。 此资源具有以下值。

| 值           | 类型    | 说明           |
|-----------------|---------|------|
|  fileName               |    字符串     |   你为提交上传的 ZIP 存档中的缩略图文件的名称。    |     
|  id  |  字符串  | 缩略图的 ID。 此值由合作伙伴中心提供。  |
|  description  |  字符串  | 缩略图的描述。 此值仅为元数据，不向用户显示。   |

<span/>

## <a name="enums"></a>枚举

这些方法使用以下枚举。

<span id="price-tiers" />

### <a name="price-tiers"></a>价格段

以下值表示应用提交的[定价资源](#pricing-object)中可用的价格段。

| 值           | 说明        |
|-----------------|------|
|  Base               |   未设置价格段；使用应用的基价。      |     
|  NotAvailable              |   应用在特定区域中不可用。    |     
|  Free              |   应用是免费的。    |    
|  Tier*xxx*               |   一个字符串，用于为应用指定价格段（**Tier<em>xxxx</em>** 格式）。 目前，支持以下价格段范围：<br/><br/><ul><li>如果[定价资源](#pricing-object)的 *isAdvancedPricingModel* 值为 **true**，则你的帐户的可用价格段值为 **Tier1012** - **Tier1424**。</li><li>如果[定价资源](#pricing-object)的 *isAdvancedPricingModel* 值为 **false**，则你的帐户的可用价格段值为 **Tier2** - **Tier96**。</li></ul>若要查看完整的价格层可用于开发者帐户，包括与每个层关联的特定于市场的价格，转到任何你在合作伙伴中心在应用提交的**定价和可用性**页面和单击**市场和自定义价格**部分中的**查看表**链接 （对于某些开发人员帐户，此链接是在**定价**部分）。    |


<span id="enterprise-licensing" />

### <a name="enterprise-licensing-values"></a>企业授权值

以下值表示应用的组织授权行为。 有关这些选项的详细信息，请参阅[组织许可选项](https://msdn.microsoft.com/windows/uwp/publish/organizational-licensing)。

> [!NOTE]
> 尽管可以通过提交 API 配置组织授权选项进行应用提交，但你无法使用此 API 发布[通过适用于企业的 Microsoft Store 和适用于教育的 Microsoft Store 批量购买](../publish/organizational-licensing.md)的提交。 若要发布提交到适用于企业的 Microsoft 应用商店和适用于教育的 Microsoft Store，你必须使用合作伙伴中心。


| 值           |  说明      |
|-----------------|---------------|
| None            |     不向具有应用商店托管（联机）批量许可的企业提供应用。         |     
| Online        |     向具有应用商店托管（联机）批量许可的企业提供应用。  |
| OnlineAndOffline | 向具有应用商店托管（联机）批量许可的企业提供应用，并通过断开连接的（脱机）许可向企业提供应用。 |


<span id="submission-status-code" />

### <a name="submission-status-code"></a>提交状态代码

以下值表示提交的状态代码。

| 值           |  说明      |
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

## <a name="related-topics"></a>相关主题

* [使用 Microsoft Store 服务创建和管理提交](create-and-manage-submissions-using-windows-store-services.md)
* [使用 Microsoft Store 提交 API 获取应用数据](get-app-data.md)
* [在合作伙伴中心中的应用提交](https://msdn.microsoft.com/windows/uwp/publish/app-submissions)
