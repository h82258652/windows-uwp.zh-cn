---
author: mcleanbyron
ms.assetid: 66400066-24BF-4AF2-B52A-577F5C3CA474
description: "在 Windows 应用商店提交 API 中使用这些方法，来管理已注册到 Windows 开发人员中心帐户的应用的加载项提交。"
title: "使用 Windows 应用商店提交 API 管理加载项提交"
translationtype: Human Translation
ms.sourcegitcommit: f52059a37194b78db2f9bb29a5e8959b2df435b4
ms.openlocfilehash: a5e1f8940f53f228808e5a6540759199c4440645

---

# <a name="manage-add-on-submissions-using-the-windows-store-submission-api"></a>使用 Windows 应用商店提交 API 管理加载项提交



在 Windows 应用商店提交 API 中使用以下方法，来管理已注册到 Windows 开发人员中心帐户的应用的加载项（也称为应用内产品或 IAP）提交。 有关 Windows 应用商店提交 API 的介绍（包括使用 API 的先决条件），请参阅[使用 Windows 应用商店服务创建和管理提交](create-and-manage-submissions-using-windows-store-services.md)。

>**注意**  这些方法只能用于已授予使用 Windows 应用商店提交 API 权限的 Windows 开发人员中心帐户。 并非所有帐户都已启用此权限。 在使用这些方法来创建或管理加载项的提交前，该加载项必须已存在于开发人员中心帐户中。 通过[使用开发人员中心仪表板](https://msdn.microsoft.com/windows/uwp/publish/iap-submissions)，或者使用[管理加载项](manage-add-ons.md)中所述的 Windows 应用商店提交 API 方法，可以创建加载项。

| 方法        | URI    | 描述                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| GET | ```https://manage.devcenter.microsoft.com/v1.0/my/inappproducts/{id}/submissions/{submissionId}``` | 获取现有加载项提交的数据。 有关详细信息，请参阅[获取加载项提交](get-an-add-on-submission.md)。 |
| GET | ```https://manage.devcenter.microsoft.com/v1.0/my/inappproducts/{id}/submissions/{submissionId}/status``` | 获取现有加载项提交的状态。 有关详细信息，请参阅[获取加载项提交的状态](get-status-for-an-add-on-submission.md)。 |
| POST | ```https://manage.devcenter.microsoft.com/v1.0/my/inappproducts/{id}/submissions``` | 为已注册到 Windows 开发人员中心帐户的应用创建新的加载项提交。 有关详细信息，请参阅[创建加载项提交](create-an-add-on-submission.md)。 |
| POST | ```https://manage.devcenter.microsoft.com/v1.0/my/inappproducts/{id}/submissions/{submissionId}/commit``` | 将新的或更新的加载项提交到 Windows 开发人员中心。 有关详细信息，请参阅[确认加载项提交](commit-an-add-on-submission.md)。 |
| PUT | ```https://manage.devcenter.microsoft.com/v1.0/my/inappproducts/{id}/submissions/{submissionId}``` | 更新现有的加载项提交。 有关详细信息，请参阅[更新加载项提交](update-an-add-on-submission.md)。 |
| DELETE | ```https://manage.devcenter.microsoft.com/v1.0/my/inappproducts/{id}/submissions/{submissionId}``` | 删除加载项提交。 有关详细信息，请参阅[删除加载项提交](delete-an-add-on-submission.md)。 |

<span id="create-an-add-on-submission">
## <a name="create-an-add-on-submission"></a>创建加载项提交

若要创建加载项的提交，请遵循此过程。

1. 如果尚未执行此操作，请完成[使用 Windows 应用商店服务创建和管理提交](create-and-manage-submissions-using-windows-store-services.md)中所述的先决条件，包括将 Azure AD 应用程序与 Windows 开发人员中心帐户相关联并获取客户端 ID 和密钥。 你只需执行此操作一次；有了客户端 ID 和密钥后，当你需要创建新的 Azure AD 访问令牌时，可以随时重复使用它们。  

2. [获取 Azure AD 访问令牌](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token)。 在 Windows 应用商店提交 API 中，必须将此访问令牌传递给相关方法。 获取访问令牌后，在它到期前，你有 60 分钟的使用时间。 该令牌到期后，可以获取新的令牌。

3. 在 Windows 应用商店提交 API 中执行以下方法。 此方法会创建新的正在进行的提交，这是你上一发布的提交副本。 有关详细信息，请参阅[创建加载项提交](create-an-add-on-submission.md)。

  ```
  POST https://manage.devcenter.microsoft.com/v1.0/my/inappproducts/{id}/submissions
  ```

  响应正文包含三个项目：新的提交 ID、新的提交数据（包括所有列表和定价信息），以及用于上载提交的任何加载项图标的共享访问签名 (SAS) URI。 有关 SAS 的详细信息，请参阅[共享访问签名，第 1 部分：了解 SAS 模型](https://azure.microsoft.com/documentation/articles/storage-dotnet-shared-access-signature-part-1/)。

4. 如果要为提交添加新的图标，请[准备图标](https://msdn.microsoft.com/windows/uwp/publish/create-iap-descriptions#icon)并将它们添加到 ZIP 存档。

5. 使用新提交所需的任何更改来更新提交数据，并执行以下方法来更新提交。 有关详细信息，请参阅[更新加载项提交](update-an-add-on-submission.md)。

  ```
  PUT https://manage.devcenter.microsoft.com/v1.0/my/inappproducts/{id}/submissions/{submissionId}
  ```

  >**注意**  如果要为提交添加新的图标，请确保更新提交数据以便在 ZIP 存档中引用这些文件的名称和相对路径。

4. 如果要为提交添加新的图标，请将 ZIP 存档上载到 SAS URI，该 URI 已在步骤 2 中调用的 POST 方法的响应正文中提供。 有关详细信息，请参阅[共享访问签名，第 2 部分：使用 Blob 存储创建和使用 SAS](https://azure.microsoft.com/documentation/articles/storage-dotnet-shared-access-signature-part-2/)。

  以下代码片段演示了如何在用于 .NET 的 Azure 存储客户端库中使用 [CloudBlockBlob](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.blob.cloudblockblob.aspx) 类上传存档。

  ```csharp
  string sasUrl = "https://productingestionbin1.blob.core.windows.net/ingestion/26920f66-b592-4439-9a9d-fb0f014902ec?sv=2014-02-14&sr=b&sig=usAN0kNFNnYE2tGQBI%2BARQWejX1Guiz7hdFtRhyK%2Bog%3D&se=2016-06-17T20:45:51Z&sp=rwl";

  Microsoft.WindowsAzure.Storage.Blob.CloudBlockBlob blockBob =
      new Microsoft.WindowsAzure.Storage.Blob.CloudBlockBlob(new System.Uri(sasUrl));
  await blockBob.UploadFromStreamAsync(stream);
  ```

5. 通过执行以下方法确认提交。 这将向开发人员中心发出警报：已完成提交，更新现在应该应用到了帐户。 有关详细信息，请参阅[确认加载项提交](commit-an-add-on-submission.md)。

  ```
  POST https://manage.devcenter.microsoft.com/v1.0/my/inappproducts/{id}/submissions/{submissionId}/commit
  ```

6. 通过执行以下方法来检查提交状态。 有关详细信息，请参阅[获取加载项提交的状态](get-status-for-an-add-on-submission.md)。

  ```
  GET https://manage.devcenter.microsoft.com/v1.0/my/inappproducts/{id}/submissions/{submissionId}/status
  ```

  若要确认提交状态，请查看响应正文中的 *status* 值。 如果请求成功，此值应该从 **CommitStarted** 更改为 **PreProcessing**；如果请求中存在错误，此值应该更改为 **CommitFailed**。 如果存在错误，*statusDetails* 字段将包含有关错误的更多详细信息。

7. 在提交成功完成之后，提交会发送至应用商店以供引入。 可以通过使用前面的方法，或者通过访问开发人员中心仪表板，继续监视提交进度。

## <a name="resources"></a>资源

这些方法使用以下资源设置数据格式。

<span id="add-on-submission-object" />
### <a name="add-on-submission"></a>加载项提交

此资源表示加载项的提交。 以下示例演示了此资源的格式。

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
    "sales": [],
    "priceId": "Free"
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

此资源具有以下值。

| 值      | 类型   | 描述                                                                                                                                                                                                                                                                         |
|------------|--------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| id            | 字符串  | 提交的 ID。  |
| contentType           | 字符串  |  加载项中提供的[内容类型](../publish/enter-add-on-properties.md#content-type)。 这可以是以下值之一： <ul><li>NotSet</li><li>BookDownload</li><li>EMagazine</li><li>ENewspaper</li><li>MusicDownload</li><li>MusicStream</li><li>OnlineDataStorage</li><li>VideoDownload</li><li>VideoStream</li><li>Asp</li><li>OnlineDownload</li></ul> |  
| keywords           | 数组  | 字符串数组，其中最多包含加载项的 10 个[关键字](../publish/enter-add-on-properties.md#keywords)。 应用可以使用这些关键字来查询加载项。   |
| lifetime           | 字符串  |  加载项的生存期。 这可以是以下值之一： <ul><li>Forever</li><li>OneDay</li><li>ThreeDays</li><li>FiveDays</li><li>OneWeek</li><li>TwoWeeks</li><li>OneMonth</li><li>TwoMonths</li><li>ThreeMonths</li><li>SixMonths</li><li>OneYear</li></ul> |
| listings           | 对象  |  键值对字典，其中每个键为两个字母的 ISO 3166-1 二字母国家/地区代码，而每个值为含有加载项列表信息的[列表资源](#listing-object)对象。  |
| pricing           | 对象  | 包含加载项定价信息的对象。 有关详细信息，请参阅下面的[定价资源](#pricing-object)部分。  |
| targetPublishMode           | 字符串  | 提交的发布模式。 这可以是以下值之一： <ul><li>Immediate</li><li>Manual</li><li>SpecificDate</li></ul> |
| targetPublishDate           | 字符串  | 提交的发布日期采用 ISO 8601 格式（如果 *targetPublishMode* 设为“SpecificDate”）。  |
| tag           | 字符串  |  加载项的[自定义开发人员数据](../publish/enter-add-on-properties.md#custom-developer-data)（此信息之前称为 *tag*）。   |
| 可见性  | 字符串  |  加载项的可见性。 这可以是以下值之一： <ul><li>Hidden</li><li>Public</li><li>Private</li><li>NotSet</li></ul>  |
| status  | 字符串  |  提交的状态。 这可以是以下值之一： <ul><li>None</li><li>Canceled</li><li>PendingCommit</li><li>CommitStarted</li><li>CommitFailed</li><li>PendingPublication</li><li>Publishing</li><li>Published</li><li>PublishFailed</li><li>PreProcessing</li><li>PreProcessingFailed</li><li>Certification</li><li>CertificationFailed</li><li>Release</li><li>ReleaseFailed</li></ul>   |
| statusDetails           | 对象  |  包含有关提交状态的附加详细信息，其中包括任何错误的相关信息。 有关详细信息，请参阅下面[状态详细信息](#status-details-object)部分。 |
| fileUploadUrl           | 字符串  | 用于为提交上载任何程序包的共享访问签名 (SAS) URI。 如果要为提交添加新的程序包，请将包含这些程序包的 ZIP 存档上载到此 URI。 有关详细信息，请参阅[创建加载项提交](#create-an-add-on-submission)。  |
| friendlyName  | 字符串  |  用于显示的加载项友好名称。  |

<span id="listing-object" />
### <a name="listing"></a>列表

此资源包含加载项的列表信息。 此资源具有以下值。

| 值           | 类型    | 描述                                                                                                                                                                                                                          |
|-----------------|---------|------|
|  description               |    字符串     |   加载项列表的描述。   |     
|  icon               |   对象      |  包含加载项列表的图标数据。 有关详细信息，请参阅下面的[图标](#icon-object)部分。   |
|  title               |     字符串    |   加载项列表的标题。   |  

<span id="icon-object" />
### <a name="icon"></a>图标

此资源包含加载项列表的图标数据。 此资源具有以下值。

| 值           | 类型    | 描述                                                                                                                                                                                                                          |
|-----------------|---------|------|
|  fileName               |    字符串     |   ZIP 存档中已为提交上载的图标文件的名称。    |     
|  fileStatus               |   字符串      |  图标文件的状态。 这可以是以下值之一： <ul><li>None</li><li>PendingUpload</li><li>Uploaded</li><li>PendingDelete</li></ul>   |

<span id="pricing-object" />
### <a name="pricing"></a>定价

此资源包含加载项的定价信息。 此资源具有以下值。

| 值           | 类型    | 描述               |
|-----------------|---------|------|
|  marketSpecificPricings               |    对象     |  键值对字典，其中每个键为两个字母的 ISO 3166-1 二字母国家/地区代码，而每个值为[价格段](#price-tiers)。 这些项表示[加载项在特定市场中的自定义价格](https://msdn.microsoft.com/windows/uwp/publish/set-iap-pricing-and-availability#markets-and-custom-prices)。 此字典中的任何项替代 *priceId* 值针对特定市场所指定的基价。     |     
|  sales               |   array      |  **已弃用**。 包含加载项销售信息的对象数组。 有关详细信息，请参阅下面的[销售](#sale-object)部分。    |     
|  priceId               |   字符串      |  用于指定加载项[基价](https://msdn.microsoft.com/windows/uwp/publish/set-iap-pricing-and-availability#base-price)的[价格段](#price-tiers)。    |


<span id="sale-object" />
### <a name="sale"></a>销售

此资源包含加载项的销售信息。

>**重要信息**   **销售**资源不再受支持，并且当前不能使用 Windows 应用商店提交 API 获取或修改加载项提交的销售数据：

   > * 调用 [GET 方法以获取加载项提交](get-an-add-on-submission.md)后，*销售*值将为空。 你可以继续使用开发人员中心仪表板获取加载项提交的销售数据。
   > * 调用 [PUT 方法以更新加载项提交](update-an-add-on-submission.md)时，将忽略*销售*值中的信息。 你可以继续使用开发人员中心仪表板更改加载项提交的销售数据。

> 将来，我们将更新 Windows 应用商店提交 API，以引入以编程方式访问加载项提交的销售信息的新方法。

此资源具有以下值。

| 值           | 类型    | 描述                                                                                                                                                                                                                          |
|-----------------|---------|------|
|  name               |    字符串     |   销售的名称。    |     
|  basePriceId               |   字符串      |  要用于销售基价的[价格段](#price-tiers)。    |     
|  startDate               |   字符串      |   采用 ISO 8601 格式的销售的开始日期。  |     
|  endDate               |   字符串      |  采用 ISO 8601 格式的销售的结束日期。      |     
|  marketSpecificPricings               |   对象      |   键值对字典，其中每个键为两个字母的 ISO 3166-1 二字母国家/地区代码，而每个值为[价格段](#price-tiers)。 这些项表示[加载项在特定市场中的自定义价格](https://msdn.microsoft.com/windows/uwp/publish/set-iap-pricing-and-availability#markets-and-custom-pricess)。 此字典中的任何项替代 *basePriceId* 值针对特定市场所指定的基价。    |



<span id="status-details-object" />
### <a name="status-details"></a>状态详细信息

此资源包含有关提交状态的附加详细信息。 此资源具有以下值。

| 值           | 类型    | 描述                                                                                                                                                                                                                          |
|-----------------|---------|------|
|  errors               |    对象     |   包含提交的错误详细信息的对象数组。 有关详细信息，请参阅下面的[状态详细信息](#status-detail-object)部分。   |     
|  warnings               |   对象      | 包含提交的警告详细信息的对象数组。 有关详细信息，请参阅下面的[状态详细信息](#status-detail-object)部分。     |
|  certificationReports               |     对象    |   提供对提交的认证报告数据的访问权限的对象数组。 如果认证失败，可检查这些报告，获取详细信息。 有关详细信息，请参阅下面的[认证报告](#certification-report-object)部分。   |  


<span id="status-detail-object" />
### <a name="status-detail"></a>状态详细信息

此资源包含关于提交的任何相关错误或警告的附加详细信息。 此资源具有以下值。

| 值           | 类型    | 描述                                                                                                                                                                                                                          |
|-----------------|---------|------|
|  code               |    字符串     |   描述错误或警告类型的字符串。 有关详细信息，请参阅下面的[提交状态代码](#submission-status-code)部分。   |     
|  details               |     字符串    |  包含有关问题的更多详细信息的消息。     |


<span id="certification-report-object" />
### <a name="certification-report"></a>认证报告

此资源提供对提交的认证报告数据的访问权限。 此资源具有以下值。

| 值           | 类型    | 说明                                                                                                                                                                                                                          |
|-----------------|---------|------|
|     date            |    字符串     |  报告生成的日期和时间，采用 ISO 8601 格式。    |
|     reportUrl            |    字符串     |  用于访问报告的 URL。    |



## <a name="enums"></a>枚举

这些方法使用以下枚举。


<span id="price-tiers" />
### <a name="price-tiers"></a>价格段

以下值表示加载项提交的可用价格段。

| 值           | 描述                                                                                                                                                                                                                          |
|-----------------|------|
|  Base               |   未设置价格段；使用加载项的基价。      |     
|  NotAvailable              |   加载项在特定区域中不可用。    |     
|  Free              |   加载项是免费的。    |    
|  Tier2 到 Tier194               |   Tier2 表示 0.99 美元价格段。 每个附加价格段表示附加增量（1.29 美元、1.49 美元、1.99 美元等）。    |


<span id="submission-status-code" />
### <a name="submission-status-code"></a>提交状态代码

以下值表示提交的状态代码。

| 值           |  描述      |
|-----------------|---------------|
|  None            |     未指定任何代码。         |     
|      InvalidArchive        |     包含程序包的 ZIP 存档无效或具有无法识别的存档格式。  |
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

* [使用 Windows 应用商店服务创建和管理提交](create-and-manage-submissions-using-windows-store-services.md)
* [使用 Windows 应用商店提交 API 管理加载项](manage-add-ons.md)
* [开发人员中心仪表板中的加载项提交](https://msdn.microsoft.com/windows/uwp/publish/iap-submissions)



<!--HONumber=Dec16_HO1-->


