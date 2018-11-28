---
ms.assetid: 66400066-24BF-4AF2-B52A-577F5C3CA474
description: 在 Microsoft Store 提交 API 中使用这些方法来管理已注册到你的合作伙伴中心帐户的应用的加载项提交。
title: 管理加载项提交
ms.date: 04/17/2018
ms.topic: article
keywords: windows 10, uwp, Microsoft Store 提交 API, 加载项提交, 应用内产品, IAP
ms.localizationpriority: medium
ms.openlocfilehash: 45fc2274ac22eee4a4c249397f25c1b0405cb856
ms.sourcegitcommit: b5c9c18e70625ab770946b8243f3465ee1013184
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/28/2018
ms.locfileid: "7967307"
---
# <a name="manage-add-on-submissions"></a>管理加载项提交

Microsoft Store 提交 API 将提供可用于管理针对应用的加载项（也称为应用内产品或 IAP）提交的方法。 有关 Microsoft Store 提交 API 的介绍（包括使用 API 的先决条件），请参阅[使用 Microsoft Store 服务创建和管理提交](create-and-manage-submissions-using-windows-store-services.md)。

> [!IMPORTANT]
> 如果你使用 Microsoft Store 提交 API 创建加载项提交，请务必只进行进一步更改提交到使用此 API 而不是在合作伙伴中心中进行更改。 如果你使用合作伙伴中心更改你最初使用 API 创建一个提交，你将不再能够更改或提交该提交使用该 API。 在某些情况下，在提交过程中无法继续进行时，提交可能会处于错误状态。 如果发生这种情况，你必须删除提交并创建新的提交。

<span id="methods-for-add-on-submissions" />

## <a name="methods-for-managing-add-on-submissions"></a>管理加载项提交的方法

使用以下方法获取、创建、更新、提交或删除加载项提交。 你可以使用这些方法之前，该加载项必须已存在于你的合作伙伴中心帐户中。 通过[定义其产品类型和产品 ID](../publish/set-your-add-on-product-id.md)或中所述[管理加载项](manage-add-ons.md)使用的 Microsoft 应用商店提交 API 方法，你可以在合作伙伴中心中创建加载项。

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
<td align="left">https://manage.devcenter.microsoft.com/v1.0/my/inappproducts/{id}/submissions/{submissionId}</td>
<td align="left"><a href="get-an-add-on-submission.md">获取现有的加载项提交</a></td>
</tr>
<tr>
<td align="left">GET</td>
<td align="left">https://manage.devcenter.microsoft.com/v1.0/my/inappproducts/{id}/submissions/{submissionId}/status</td>
<td align="left"><a href="get-status-for-an-add-on-submission.md">获取现有加载项提交的状态</a></td>
</tr>
<tr>
<td align="left">POST</td>
<td align="left">https://manage.devcenter.microsoft.com/v1.0/my/inappproducts/{id}/submissions</td>
<td align="left"><a href="create-an-add-on-submission.md">创建新加载项提交</a></td>
</tr>
<tr>
<td align="left">PUT</td>
<td align="left">https://manage.devcenter.microsoft.com/v1.0/my/inappproducts/{id}/submissions/{submissionId}</td>
<td align="left"><a href="update-an-add-on-submission.md">更新现有的加载项提交</a></td>
</tr>
<tr>
<td align="left">POST</td>
<td align="left">https://manage.devcenter.microsoft.com/v1.0/my/inappproducts/{id}/submissions/{submissionId}/commit</td>
<td align="left"><a href="commit-an-add-on-submission.md">提交新建或已更新的加载项提交</a></td>
</tr>
<tr>
<td align="left">DELETE</td>
<td align="left">https://manage.devcenter.microsoft.com/v1.0/my/inappproducts/{id}/submissions/{submissionId}</td>
<td align="left"><a href="delete-an-add-on-submission.md">删除加载项提交</a></td>
</tr>
</tbody>
</table>

<span id="create-an-add-on-submission">

## <a name="create-an-add-on-submission"></a>创建加载项提交

若要创建加载项的提交，请遵循此过程。

1. 如果尚未不执行此操作，完整必备条件中所述[创建使用 Microsoft Store 服务和管理提交](create-and-manage-submissions-using-windows-store-services.md)，包括将 Azure AD 应用程序与你的合作伙伴中心帐户相关联并获取客户端 ID 和密钥。 你只需执行此操作一次；有了客户端 ID 和密钥后，当你需要创建新的 Azure AD 访问令牌时，可以随时重复使用它们。  

2. [获取 Azure AD 访问令牌](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token)。 在 Microsoft Store 提交 API 中，必须将此访问令牌传递给相关方法。 获取访问令牌后，在它到期前，你有 60 分钟的使用时间。 该令牌到期后，可以获取新的令牌。

3. 在 Microsoft Store 提交 API 中执行以下方法。 此方法会创建新的正在进行的提交，这是你上一发布的提交副本。 有关详细信息，请参阅[创建加载项提交](create-an-add-on-submission.md)。

    ```
    POST https://manage.devcenter.microsoft.com/v1.0/my/inappproducts/{id}/submissions
    ```

    响应正文包含[加载项提交](#add-on-submission-object)资源，该资源包括新的提交 ID、共享访问签名 (SAS) URI（用于将提交的所有加载项图标上载到 Azure Blob 存储）以及所有新的提交数据（如列表和定价信息）。

    > [!NOTE]
    > SAS URI 提供对 Azure 存储中的安全资源的访问权限（无需帐户密钥）。 有关 SAS URI 以及借助 Azure Blob 使用这些 URI 的背景信息，请参阅[共享访问签名，第 1 部分：了解 SAS 模型](https://azure.microsoft.com/documentation/articles/storage-dotnet-shared-access-signature-part-1)和[共享访问签名，第 2 部分：使用 Blob 存储创建和使用 SAS](https://azure.microsoft.com/documentation/articles/storage-dotnet-shared-access-signature-part-2/)。

4. 如果要为提交添加新的图标，请[准备图标](https://msdn.microsoft.com/windows/uwp/publish/create-iap-descriptions#icon)并将它们添加到 ZIP 存档。

5. 使用新提交所需的任何更改来更新[加载项提交](#add-on-submission-object)数据，并执行以下方法来更新提交。 有关详细信息，请参阅[更新加载项提交](update-an-add-on-submission.md)。

    ```
    PUT https://manage.devcenter.microsoft.com/v1.0/my/inappproducts/{id}/submissions/{submissionId}
    ```
      > [!NOTE]
      > 如果要为提交添加新的图标，请确保更新提交数据以便在 ZIP 存档中引用这些文件的名称和相对路径。

4. 如果要为提交添加新的图标，请使用 SAS URI 将 ZIP 存档上载到 [Azure Blob 存储](https://docs.microsoft.com/azure/storage/storage-introduction#blob-storage)，该 URI 已在之前调用的 POST 方法的响应正文中提供。 你可以使用不同的 Azure 库在多个平台上进行此操作，包括：

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

5. 通过执行以下方法确认提交。 这将向合作伙伴中心警报你完成提交，更新现在应该应用到你的帐户。 有关详细信息，请参阅[确认加载项提交](commit-an-add-on-submission.md)。

    ```
    POST https://manage.devcenter.microsoft.com/v1.0/my/inappproducts/{id}/submissions/{submissionId}/commit
    ```

6. 通过执行以下方法来检查提交状态。 有关详细信息，请参阅[获取加载项提交的状态](get-status-for-an-add-on-submission.md)。

    ```
    GET https://manage.devcenter.microsoft.com/v1.0/my/inappproducts/{id}/submissions/{submissionId}/status
    ```

    若要确认提交状态，请查看响应正文中的 *status* 值。 如果请求成功，此值应该从 **CommitStarted** 更改为 **PreProcessing**；如果请求中存在错误，此值应该更改为 **CommitFailed**。 如果存在错误，*statusDetails* 字段将包含有关错误的更多详细信息。

7. 在提交成功完成之后，提交会发送至应用商店以供引入。 你可以继续监视提交进度，通过使用前面的方法，或者通过访问合作伙伴中心。

<span/>

## <a name="code-examples"></a>代码示例

下文中提供了详细的代码示例，演示了如何以多种不同的编程语言创建加载项提交：

* [C# 代码示例](csharp-code-examples-for-the-windows-store-submission-api.md)
* [Java 代码示例](java-code-examples-for-the-windows-store-submission-api.md)
* [Python 代码示例](python-code-examples-for-the-windows-store-submission-api.md)

## <a name="storebroker-powershell-module"></a>StoreBroker PowerShell 模块

除了直接调用 Microsoft Store 提交 API 的方式外，我们还提供在该 API 之上实现命令行界面的开源 PowerShell 模块。 此模块称为 [StoreBroker](https://aka.ms/storebroker)。 你可以使用此模块从命令行管理你的应用、外部测试版和加载项提交，而不是通过直接调用 Microsoft Store 提交 API，或者你可以浏览源以查看更多有关如何调用此 API 的示例。 在 Microsoft 内，StoreBroker 模块作为将许多第一方应用程序提交到 Microsoft Store 的主要方式被频繁使用。

有关详细信息，请参阅我们 [GitHub 上的 StoreBroker 页面](https://aka.ms/storebroker)。

<span/>

## <a name="data-resources"></a>数据资源

管理加载项提交的 Microsoft Store 提交 API 方法使用以下 JSON 数据资源。

<span id="add-on-submission-object" />

### <a name="add-on-submission-resource"></a>加载项提交资源

此资源描述加载项提交。

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

此资源具有以下值。

| 值      | 类型   | 描述        |
|------------|--------|----------------------|
| id            | 字符串  | 提交的 ID。 此 ID 包含在 [create an add-on submission](create-an-add-on-submission.md)、[get all add-ons](get-all-add-ons.md) 和 [get an add-on](get-an-add-on.md) 请求的响应数据中。 对于在合作伙伴中心中创建的提交，此 ID 也包含在合作伙伴中心中的提交页面的 URL 中可用。  |
| contentType           | 字符串  |  加载项中提供的[内容类型](../publish/enter-add-on-properties.md#content-type)。 这可以是以下值之一： <ul><li>NotSet</li><li>BookDownload</li><li>EMagazine</li><li>ENewspaper</li><li>MusicDownload</li><li>MusicStream</li><li>OnlineDataStorage</li><li>VideoDownload</li><li>VideoStream</li><li>Asp</li><li>OnlineDownload</li></ul> |  
| keywords           | 数组  | 字符串数组，其中最多包含加载项的 10 个[关键字](../publish/enter-add-on-properties.md#keywords)。 应用可以使用这些关键字来查询加载项。   |
| lifetime           | 字符串  |  加载项的生存期。 这可以是以下值之一： <ul><li>Forever</li><li>OneDay</li><li>ThreeDays</li><li>FiveDays</li><li>OneWeek</li><li>TwoWeeks</li><li>OneMonth</li><li>TwoMonths</li><li>ThreeMonths</li><li>SixMonths</li><li>OneYear</li></ul> |
| listings           | 对象  |  键值对字典，其中每个键为两个字母的 ISO 3166-1 二字母国家/地区代码，而每个值为含有加载项列表信息的[列表资源](#listing-object)对象。  |
| pricing           | 对象  | 包含加载项的定价信息的[定价资源](#pricing-object)。   |
| targetPublishMode           | 字符串  | 提交的发布模式。 这可以是以下值之一： <ul><li>Immediate</li><li>Manual</li><li>SpecificDate</li></ul> |
| targetPublishDate           | 字符串  | 提交的发布日期采用 ISO 8601 格式（如果 *targetPublishMode* 设为“SpecificDate”）。  |
| tag           | 字符串  |  加载项的[自定义开发人员数据](../publish/enter-add-on-properties.md#custom-developer-data)（此信息之前称为 *tag*）。   |
| 可见性  | 字符串  |  加载项的可见性。 这可以是以下值之一： <ul><li>Hidden</li><li>Public</li><li>Private</li><li>NotSet</li></ul>  |
| status  | 字符串  |  提交的状态。 这可以是以下值之一： <ul><li>None</li><li>Canceled</li><li>PendingCommit</li><li>CommitStarted</li><li>CommitFailed</li><li>PendingPublication</li><li>Publishing</li><li>Published</li><li>PublishFailed</li><li>PreProcessing</li><li>PreProcessingFailed</li><li>Certification</li><li>CertificationFailed</li><li>Release</li><li>ReleaseFailed</li></ul>   |
| statusDetails           | 对象  |  包含有关提交状态的附加详细信息的[状态详细信息资源](#status-details-object)，其中包括任何错误的相关信息。 |
| fileUploadUrl           | 字符串  | 用于为提交上载任何程序包的共享访问签名 (SAS) URI。 如果要为提交添加新的程序包，请将包含这些程序包的 ZIP 存档上载到此 URI。 有关详细信息，请参阅[创建加载项提交](#create-an-add-on-submission)。  |
| friendlyName  | 字符串  |  在合作伙伴中心中所示的提交的友好名称。 当你创建提交时，系统会为你生成此值。  |

<span id="listing-object" />

### <a name="listing-resource"></a>列表资源

此资源包含[加载项的列表信息](../publish/create-add-on-store-listings.md)。 此资源具有以下值。

| 值           | 类型    | 说明       |
|-----------------|---------|------|
|  description               |    字符串     |   加载项列表的描述。   |     
|  icon               |   对象      |包含加载项列表的图标数据的[图标资源](#icon-object)。    |
|  title               |     字符串    |   加载项列表的标题。   |  

<span id="icon-object" />

### <a name="icon-resource"></a>图标资源

此资源包含加载项列表的图标数据。 此资源具有以下值。

| 值           | 类型    | 说明     |
|-----------------|---------|------|
|  fileName               |    字符串     |   ZIP 存档中已为提交上载的图标文件的名称。 图标必须是大小正好为 300 x 300 像素的 .png 文件。   |     
|  fileStatus               |   字符串      |  图标文件的状态。 这可以是以下值之一： <ul><li>None</li><li>PendingUpload</li><li>Uploaded</li><li>PendingDelete</li></ul>   |

<span id="pricing-object" />

### <a name="pricing-resource"></a>定价资源

此资源包含加载项的定价信息。 此资源具有以下值。

| 值           | 类型    | 说明    |
|-----------------|---------|------|
|  marketSpecificPricings               |    对象     |  键值对字典，其中每个键为两个字母的 ISO 3166-1 二字母国家/地区代码，而每个值为[价格段](#price-tiers)。 这些项表示[加载项在特定市场中的自定义价格](https://msdn.microsoft.com/windows/uwp/publish/set-iap-pricing-and-availability#markets-and-custom-prices)。 此字典中的任何项替代 *priceId* 值针对特定市场所指定的基价。     |     
|  sales               |   数组      |  **已弃用**。 包含加载项销售信息的[销售资源](#sale-object)数组。     |     
|  priceId               |   字符串      |  用于指定加载项[基价](https://msdn.microsoft.com/windows/uwp/publish/set-iap-pricing-and-availability#base-price)的[价格段](#price-tiers)。    |    
|  isAdvancedPricingModel               |   布尔型      |  如果为 **true**，你的开发人员帐户可以使用从 0.99 美元到 1999.99 美元的扩展价格段。 如果为 **false**，你的开发人员帐户可以使用从 0.99 美元到 999.99 美元的原始价格段。 有关其他价格段的详细信息，请参阅[价格段](#price-tiers)。<br/><br/>**注意**&nbsp;&nbsp;此字段为只读字段。   |


<span id="sale-object" />

### <a name="sale-resource"></a>销售资源

此资源包含加载项的销售信息。

> [!IMPORTANT]
> **销售**资源不再受支持，并且当前不能使用 Microsoft Store 提交 API 获取或修改加载项提交的销售数据。 将来，我们将更新 Microsoft Store 提交 API，以引入以编程方式访问加载项提交的销售信息的新方法。
>    * 调用 [GET 方法以获取加载项提交](get-an-add-on-submission.md)后，*销售*值将为空。 你可以继续使用合作伙伴中心以获取加载项提交的销售数据。
>    * 调用 [PUT 方法以更新加载项提交](update-an-add-on-submission.md)时，将忽略*销售*值中的信息。 你可以继续使用合作伙伴中心更改加载项提交的销售数据。

此资源具有以下值。

| 值           | 类型    | 说明           |
|-----------------|---------|------|
|  name               |    字符串     |   销售的名称。    |     
|  basePriceId               |   字符串      |  要用于销售基价的[价格段](#price-tiers)。    |     
|  startDate               |   字符串      |   采用 ISO 8601 格式的销售的开始日期。  |     
|  endDate               |   字符串      |  采用 ISO 8601 格式的销售的结束日期。      |     
|  marketSpecificPricings               |   对象      |   键值对字典，其中每个键为两个字母的 ISO 3166-1 二字母国家/地区代码，而每个值为[价格段](#price-tiers)。 这些项表示[加载项在特定市场中的自定义价格](https://msdn.microsoft.com/windows/uwp/publish/set-iap-pricing-and-availability#markets-and-custom-pricess)。 此字典中的任何项替代 *basePriceId* 值针对特定市场所指定的基价。    |

<span id="status-details-object" />

### <a name="status-details-resource"></a>状态详细信息资源

此资源包含有关提交状态的附加详细信息。 此资源具有以下值。

| 值           | 类型    | 说明       |
|-----------------|---------|------|
|  errors               |    对象     |   包含提交的错误详细信息的[状态详细信息资源](#status-detail-object)数组。   |     
|  warnings               |   对象      | 包含提交的警告详细信息的[状态详细信息资源](#status-detail-object)数组。     |
|  certificationReports               |     对象    |   提供对提交的认证报告数据的访问权限的[认证报告资源](#certification-report-object)数组。 如果认证失败，可检查这些报告，获取详细信息。    |  

<span id="status-detail-object" />

### <a name="status-detail-resource"></a>状态详细信息资源

此资源包含关于提交的任何相关错误或警告的附加详细信息。 此资源具有以下值。

| 值           | 类型    | 说明    |
|-----------------|---------|------|
|  code               |    字符串     |   描述错误或警告类型的[提交状态代码](#submission-status-code)。   |     
|  details               |     字符串    |  包含有关问题的更多详细信息的消息。     |

<span id="certification-report-object" />

### <a name="certification-report-resource"></a>认证报告资源

此资源提供对提交的认证报告数据的访问权限。 此资源具有以下值。

| 值           | 类型    | 说明               |
|-----------------|---------|------|
|     date            |    字符串     |  日期和报告生成的时间，采用 ISO 8601 格式。    |
|     reportUrl            |    字符串     |  用于访问报告的 URL。    |

## <a name="enums"></a>枚举

这些方法使用以下枚举。

<span id="price-tiers" />

### <a name="price-tiers"></a>价格段

以下值表示加载项提交的[定价资源](#pricing-object)中可用的价格段。

| 值           | 说明       |
|-----------------|------|
|  Base               |   未设置价格段；使用加载项的基价。      |     
|  NotAvailable              |   加载项在特定区域中不可用。    |     
|  Free              |   加载项是免费的。    |    
|  Tier*xxxx*               |   一个字符串，用于为加载项指定价格段（**Tier<em>xxxx</em>** 格式）。 目前，支持以下价格段范围：<br/><br/><ul><li>如果[定价资源](#pricing-object)的 *isAdvancedPricingModel* 值为 **true**，则你的帐户的可用价格段值为 **Tier1012** - **Tier1424**。</li><li>如果[定价资源](#pricing-object)的 *isAdvancedPricingModel* 值为 **false**，则你的帐户的可用价格段值为 **Tier2** - **Tier96**。</li></ul>若要查看完整的价格适用于你的开发人员帐户，包括与每个层关联的特定于市场的价格段转到任何你在合作伙伴中心在应用提交的**定价和可用性**页面和单击**市场和自定义价格**部分中的**查看表**链接 （对于某些开发人员帐户，此链接是在**定价**部分）。     |

<span id="submission-status-code" />

### <a name="submission-status-code"></a>提交状态代码

以下值表示提交的状态代码。

| 值           |  说明      |
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

* [使用 Microsoft Store 服务创建和管理提交](create-and-manage-submissions-using-windows-store-services.md)
* [使用 Microsoft Store 提交 API 管理加载项](manage-add-ons.md)
* [在合作伙伴中心中的加载项提交](https://msdn.microsoft.com/windows/uwp/publish/iap-submissions)
