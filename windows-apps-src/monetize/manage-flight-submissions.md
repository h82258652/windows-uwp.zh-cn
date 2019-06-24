---
ms.assetid: 2A454057-FF14-40D2-8ED2-CEB5F27E0226
description: Microsoft Store 提交 API 中使用这些方法来管理包航班提交到合作伙伴中心帐户注册的应用。
title: 管理软件包外部测试版提交
ms.date: 04/16/2018
ms.topic: article
keywords: windows 10, uwp, Microsoft Store 提交 API, 外部测试版提交
ms.localizationpriority: medium
ms.openlocfilehash: 11fb2427ece0f0e37fb2a5f2759094d6e04930c8
ms.sourcegitcommit: 6f32604876ed480e8238c86101366a8d106c7d4e
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/21/2019
ms.locfileid: "67320150"
---
# <a name="manage-package-flight-submissions"></a>管理软件包外部测试版提交

Microsoft Store 提交 API 提供可用于管理针对应用的软件包外部测试版提交的方法，包括逐步软件包推出。 有关 Microsoft Store 提交 API 的介绍（包括使用 API 的先决条件），请参阅[使用 Microsoft Store 服务创建和管理提交](create-and-manage-submissions-using-windows-store-services.md)。

> [!IMPORTANT]
> 如果使用的 Microsoft Store 提交 API 来创建包航班的提交，请务必进行进一步更改提交到只能通过使用 API，而不是合作伙伴中心。 如果你使用仪表板更改你最初使用此 API 创建的提交，则将无法再使用此 API 更改或提交该提交。 在某些情况下，在提交过程中无法继续进行时，提交可能会处于错误状态。 如果发生这种情况，你必须删除提交并创建新的提交。

<span id="methods-for-package-flight-submissions" />

## <a name="methods-for-managing-package-flight-submissions"></a>管理软件包外部测试版提交的方法

使用以下方法获取、创建、更新、提交或删除软件包外部测试版提交。 可以使用这些方法之前，必须在合作伙伴中心上已存在包航班。 您可以创建包航班[在合作伙伴中心](https://docs.microsoft.com/windows/uwp/publish/package-flights)或通过使用中的 Microsoft Store 提交 API 方法中所述[管理包航班](manage-flights.md)。

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
<th align="left">描述</th>
</tr>
</thead>
<tbody>
<tr>
<td align="left">GET</td>
<td align="left">https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}/submissions/{submissionId}</td>
<td align="left"><a href="get-a-flight-submission.md">获取现有的包航班提交</a></td>
</tr>
<tr>
<td align="left">GET</td>
<td align="left">https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}/submissions/{submissionId}/status</td>
<td align="left"><a href="get-status-for-a-flight-submission.md">获取现有的包航班提交状态</a></td>
</tr>
<tr>
<td align="left">发布</td>
<td align="left">https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}/submissions</td>
<td align="left"><a href="create-a-flight-submission.md">创建新的包航班提交</a></td>
</tr>
<tr>
<td align="left">PUT</td>
<td align="left">https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}/submissions/{submissionId}</td>
<td align="left"><a href="update-a-flight-submission.md">更新现有的包航班提交</a></td>
</tr>
<tr>
<td align="left">发布</td>
<td align="left">https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}/submissions/{submissionId}/commit</td>
<td align="left"><a href="commit-a-flight-submission.md">提交新的或更新包航班提交</a></td>
</tr>
<tr>
<td align="left">DELETE</td>
<td align="left">https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}/submissions/{submissionId}</td>
<td align="left"><a href="delete-a-flight-submission.md">删除包航班提交</a></td>
</tr>
</tbody>
</table>

<span id="create-a-package-flight-submission">

## <a name="create-a-package-flight-submission"></a>创建软件包外部测试版提交

若要创建软件包外部测试版提交，请遵循此过程。

1. 如果尚未这样做，完成先决条件中所述[创建和管理使用 Microsoft Store 服务的提交](create-and-manage-submissions-using-windows-store-services.md)，包括将 Azure AD 应用程序与你的合作伙伴中心帐户相关联并获取应用客户端 ID 和密钥。 你只需执行此操作一次；有了客户端 ID 和密钥后，当你需要创建新的 Azure AD 访问令牌时，可以随时重复使用它们。  

2. [获取 Azure AD 访问令牌](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token)。 在 Microsoft Store 提交 API 中，必须将此访问令牌传递给相关方法。 获取访问令牌后，在它到期前，你有 60 分钟的使用时间。 该令牌到期后，可以获取新的令牌。

3. 通过执行 Microsoft Store 提交 API 中的以下方法[创建软件包外部测试版提交](create-a-flight-submission.md)。 此方法会创建新的正在进行的提交，这是你上一发布的提交副本。

    ```json
    POST https://manage.devcenter.microsoft.com/v1.0/my/applications{applicationId}/flights/{flightId}/submissions
    ```

    响应正文包含[外部测试版提交](#flight-submission-object)资源（包括新提交的 ID、用于将提交的任何程序包上传到 Azure Blob 存储的共享访问签名 (SAS) URI）和新提交的数据（包括所有应用一览和定价信息）。

    > [!NOTE]
    > SAS URI 提供对 Azure 存储中的安全资源的访问权限（无需帐户密钥）。 有关 SAS URI 及其与 Azure Blob 存储一起使用的背景信息，请参阅[共享访问签名（第 1 部分）：了解 SAS 模型](https://azure.microsoft.com/documentation/articles/storage-dotnet-shared-access-signature-part-1/)和[共享访问签名，第 2 部分：创建并将 SAS 用于 Blob 存储](https://azure.microsoft.com/documentation/articles/storage-dotnet-shared-access-signature-part-2/)。

4. 如果要为提交添加新的软件包，请[准备软件包](https://docs.microsoft.com/windows/uwp/publish/app-package-requirements)并将它们添加到 ZIP 存档。

5. 使用新提交所需的任何更改修订[外部测试版提交](#flight-submission-object)数据，并执行以下方法来[更新软件包外部测试版提交](update-a-flight-submission.md)。

    ```json
    PUT https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}/submissions/{submissionId}
    ```
      > [!NOTE]
      > 如果要为提交添加新的软件包，请确保更新提交数据以便在 ZIP 存档中引用这些文件的名称和相对路径。

4. 如果要为提交添加新包，请使用 SAS URI 将 ZIP 存档上载到 [Azure Blob 存储](https://docs.microsoft.com/azure/storage/storage-introduction#blob-storage)，该 URI 已在之前调用的 POST 方法的响应正文中提供。 你可以使用不同的 Azure 库在多个平台上进行此操作，包括：

    * [适用于.NET 的 azure 存储客户端库](https://docs.microsoft.com/azure/storage/storage-dotnet-how-to-use-blobs)
    * [Azure Storage SDK for Java](https://docs.microsoft.com/azure/storage/storage-java-how-to-use-blob-storage)
    * [Azure Storage SDK for Python](https://docs.microsoft.com/azure/storage/storage-python-how-to-use-blob-storage)

    以下 C# 代码示例演示了如何在用于 .NET 的 Azure 存储客户端库中使用 [CloudBlockBlob](https://docs.microsoft.com/dotnet/api/microsoft.windowsazure.storage.blob.cloudblockblob?redirectedfrom=MSDN) 类将 ZIP 存档上载到 Azure Blob 存储。 此示例假定 ZIP 存档已写入流对象。

    ```csharp
    string sasUrl = "https://productingestionbin1.blob.core.windows.net/ingestion/26920f66-b592-4439-9a9d-fb0f014902ec?sv=2014-02-14&sr=b&sig=usAN0kNFNnYE2tGQBI%2BARQWejX1Guiz7hdFtRhyK%2Bog%3D&se=2016-06-17T20:45:51Z&sp=rwl";
    Microsoft.WindowsAzure.Storage.Blob.CloudBlockBlob blockBob =
        new Microsoft.WindowsAzure.Storage.Blob.CloudBlockBlob(new System.Uri(sasUrl));
    await blockBob.UploadFromStreamAsync(stream);
    ```

5. 通过执行以下方法[确认软件包外部测试版提交](commit-a-flight-submission.md)。 完成与您的提交和更新现在应该应用到你的帐户，这将发出警报合作伙伴中心。

    ```json
    POST https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}/submissions/{submissionId}/commit
    ```

6. 通过执行以下方法来检查提交状态以[获取软件包外部测试版提交的状态](get-status-for-a-flight-submission.md)。

    ```json
    GET https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}/submissions/{submissionId}/status
    ```

    若要确认提交状态，请查看响应正文中的 *status* 值。 如果请求成功，此值应该从 **CommitStarted** 更改为 **PreProcessing**；如果请求中存在错误，此值应该更改为 **CommitFailed**。 如果存在错误，*statusDetails* 字段将包含有关错误的更多详细信息。

7. 在提交成功完成之后，提交会发送至应用商店以供引入。 您可以继续使用在前面的方法或通过访问合作伙伴中心监视提交进度。

<span/>

## <a name="code-examples"></a>代码示例

下文中提供了详细的代码示例，演示如何以不同的多种编程语言创建软件包外部测试版提交。

* [C#代码示例](csharp-code-examples-for-the-windows-store-submission-api.md)
* [Java 代码示例](java-code-examples-for-the-windows-store-submission-api.md)
* [Python 代码示例](python-code-examples-for-the-windows-store-submission-api.md)

## <a name="storebroker-powershell-module"></a>StoreBroker PowerShell 模块

除了直接调用 Microsoft Store 提交 API 的方式外，我们还提供在该 API 之上实现命令行界面的开源 PowerShell 模块。 此模块称为 [StoreBroker](https://aka.ms/storebroker)。 你可以使用此模块从命令行管理你的应用、外部测试版和加载项提交，而不是通过直接调用 Microsoft Store 提交 API，或者你可以浏览源以查看更多有关如何调用此 API 的示例。 在 Microsoft 内，StoreBroker 模块作为将许多第一方应用程序提交到 Microsoft Store 的主要方式被频繁使用。

有关详细信息，请参阅我们 [GitHub 上的 StoreBroker 页面](https://aka.ms/storebroker)。

<span id="manage-gradual-package-rollout">

## <a name="manage-a-gradual-package-rollout-for-a-package-flight-submission"></a>管理软件包外部测试版提交的逐步软件包推出

可在软件包外部测试版提交中逐步向 Windows 10 上一定比例的应用客户推出已更新的软件包。 这使你可以监视特定程序包的反馈和分析数据，从而确保在更广泛地推出更新前对此更新无虑。 可更改已发布提交的推出百分比（或终止更新），而无需创建新提交。 有关详细信息，包括有关如何启用和管理在合作伙伴中心的逐步包推出的说明，请参阅[这篇文章](../publish/gradual-package-rollout.md)。

若要以编程方式启用软件包外部测试版提交的逐步软件包推出，请遵循此过程使用 Microsoft Store 提交 API 中的以下方法：

  1. [创建软件包外部测试版提交](create-a-flight-submission.md)或[获取软件包外部测试版提交](get-a-flight-submission.md)。
  2. 在响应数据中，找到 [packageRollout](#package-rollout-object) 资源、将*isPackageRollout*字段设置为 true，然后将*packageRolloutPercentage*字段设置为应获取已更新的软件包的应用客户百分比。
  3. 将已更新的软件包外部测试版提交数据传递到[更新软件包外部测试版提交](update-a-flight-submission.md)方法。

针对软件包外部测试版提交启用逐步软件包推出后，你可以使用以下方法以编程方式获取、更新、终止或完成逐步推出。

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
<th align="left">描述</th>
</tr>
</thead>
<tbody>
<tr>
<td align="left">GET</td>
<td align="left">https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}/submissions/{submissionId}/packagerollout</td>
<td align="left"><a href="get-package-rollout-info-for-a-flight-submission.md">获取包航班提交逐渐推出信息</a></td>
</tr>
<tr>
<td align="left">发布</td>
<td align="left">https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}/submissions/{submissionId}/updatepackagerolloutpercentage</td>
<td align="left"><a href="update-the-package-rollout-percentage-for-a-flight-submission.md">更新包航班提交逐渐推出百分比</a></td>
</tr>
<tr>
<td align="left">发布</td>
<td align="left">https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}/submissions/{submissionId}/haltpackagerollout</td>
<td align="left"><a href="halt-the-package-rollout-for-a-flight-submission.md">停止包航班提交逐渐推出</a></td>
</tr>
<tr>
<td align="left">发布</td>
<td align="left">https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}/submissions/{submissionId}/finalizepackagerollout</td>
<td align="left"><a href="finalize-the-package-rollout-for-a-flight-submission.md">完成包航班提交逐渐推出</a></td>
</tr>
</tbody>
</table>

<span/>

## <a name="data-resources"></a>数据资源

管理软件包外部测试版提交的 Microsoft Store 提交 API 方法使用以下 JSON 数据资源。

<span id="flight-submission-object" />

### <a name="flight-submission-resource"></a>外部测试版提交资源

此资源描述了软件包外部测试版提交。

```json
{
  "id": "1152921504621243649",
  "flightId": "cd2e368a-0da5-4026-9f34-0e7934bc6f23",
  "status": "PendingCommit",
  "statusDetails": {
    "errors": [],
    "warnings": [],
    "certificationReports": []
  },
  "flightPackages": [
    {
      "fileName": "newPackage.appx",
      "fileStatus": "PendingUpload",
      "id": "",
      "version": "1.0.0.0",
      "languages": ["en-us"],
      "capabilities": [],
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
  "fileUploadUrl": "https://productingestionbin1.blob.core.windows.net/ingestion/8b389577-5d5e-4cbe-a744-1ff2e97a9eb8?sv=2014-02-14&sr=b&sig=wgMCQPjPDkuuxNLkeG35rfHaMToebCxBNMPw7WABdXU%3D&se=2016-06-17T21:29:44Z&sp=rwl",
  "targetPublishMode": "Immediate",
  "targetPublishDate": "",
  "notesForCertification": "No special steps are required for certification of this app."
}
```

此资源具有以下值。

| ReplTest1      | 在任务栏的搜索框中键入   | 描述              |
|------------|--------|------------------------------|
| id            | string  | 提交的 ID。  |
| flightId           | string  |  提交相关联的软件包外部测试版的 ID。  |  
| status           | string  | 提交的状态。 这可以是以下值之一： <ul><li>无</li><li>Canceled</li><li>PendingCommit</li><li>CommitStarted</li><li>CommitFailed</li><li>PendingPublication</li><li>Publishing</li><li>Published</li><li>PublishFailed</li><li>PreProcessing</li><li>PreProcessingFailed</li><li>认证</li><li>CertificationFailed</li><li>发行版本</li><li>ReleaseFailed</li></ul>   |
| statusDetails           | 对象  |  包含有关提交状态的附加详细信息的[状态详细信息资源](#status-details-object)，其中包括任何错误的相关信息。  |
| flightPackages           | array  | 包含提供提交中关于每个程序包详细信息的[软件包外部测试版资源](#flight-package-object)。   |
| packageDeliveryOptions    | 对象  | 包含提交的逐步软件包推出和强制更新设置的[软件包递送选项资源](#package-delivery-options-object)。   |
| fileUploadUrl           | string  | 用于为提交上载任何程序包的共享访问签名 (SAS) URI。 如果要为提交添加新的程序包，请将包含这些程序包的 ZIP 存档上载到此 URI。 有关详细信息，请参阅[创建软件包外部测试版提交](#create-a-package-flight-submission)。  |
| targetPublishMode           | string  | 提交的发布模式。 这可以是以下值之一： <ul><li>立即</li><li>Manual</li><li>SpecificDate</li></ul> |
| targetPublishDate           | string  | 提交的发布日期采用 ISO 8601 格式（如果 *targetPublishMode* 设为“SpecificDate”）。  |
| notesForCertification           | string  |  提供认证测试人员的其他信息，例如测试帐户凭据以及访问和验证功能的步骤。 有关详细信息，请参阅[认证说明](https://docs.microsoft.com/windows/uwp/publish/notes-for-certification)。 |

<span id="status-details-object" />

### <a name="status-details-resource"></a>状态详细信息资源

此资源包含有关提交状态的附加详细信息。 此资源具有以下值。

| ReplTest1           | 在任务栏的搜索框中键入    | 描述                   |
|-----------------|---------|------|
|  errors               |    对象     |   包含提交的错误详细信息的[状态详细信息资源](#status-detail-object)数组。   |     
|  warnings               |   对象      | 包含提交的警告详细信息的[状态详细信息资源](#status-detail-object)数组。     |
|  certificationReports               |     对象    |   提供对提交的认证报告数据的访问权限的[认证报告资源](#certification-report-object)数组。 如果认证失败，可检查这些报告，获取详细信息。    |  


<span id="status-detail-object" />

### <a name="status-detail-resource"></a>状态详细信息资源

此资源包含关于提交的任何相关错误或警告的附加详细信息。 此资源具有以下值。

| ReplTest1           | 在任务栏的搜索框中键入    | 描述       |
|-----------------|---------|------|
|  code               |    string     |   描述错误或警告类型的[提交状态代码](#submission-status-code)。 |  
|  details               |     string    |  包含有关问题的更多详细信息的消息。     |


<span id="certification-report-object" />

### <a name="certification-report-resource"></a>认证报告资源

此资源提供对提交的认证报告数据的访问权限。 此资源具有以下值。

| ReplTest1           | 在任务栏的搜索框中键入    | 描述         |
|-----------------|---------|------|
|     date            |    string     |  日期和时间生成报表，采用 ISO 8601 格式。    |
|     reportUrl            |    string     |  用于访问报告的 URL。    |


<span id="flight-package-object" />

### <a name="flight-package-resource"></a>软件包外部测试版资源

此资源提供有关提交中的程序包的详细信息。

```json
{
  "flightPackages": [
    {
      "fileName": "newPackage.appx",
      "fileStatus": "PendingUpload",
      "id": "",
      "version": "1.0.0.0",
      "languages": ["en-us"],
      "capabilities": [],
      "minimumDirectXVersion": "None",
      "minimumSystemRam": "None"
    }
  ],
}
```

此资源具有以下值。

> [!NOTE]
> 当调用[更新应用提交](update-a-flight-submission.md)方法时，请求正文中仅需要此对象的 *fileName*、*fileStatus*、*minimumDirectXVersion* 和 *minimumSystemRam* 值。 通过合作伙伴中心填充其他值。

| ReplTest1           | 在任务栏的搜索框中键入    | 描述              |
|-----------------|---------|------|
| fileName   |   string      |  程序包的名称。    |  
| fileStatus    | string    |  程序包的状态。 这可以是以下值之一： <ul><li>无</li><li>PendingUpload</li><li>Uploaded</li><li>PendingDelete</li></ul>    |  
| id    |  string   |  唯一标识程序包的 ID。 通过合作伙伴中心使用此值。   |     
| version    |  string   |  应用包的版本。 有关详细信息，请参阅[程序包版本编号](https://docs.microsoft.com/windows/uwp/publish/package-version-numbering)。   |   
| architecture    |  string   |  应用包的体系结构（例如 ARM）。   |     
| languages    | array    |  应用所支持的语言的语言代码数组。 有关详细信息，请参阅[支持的语言](https://docs.microsoft.com/windows/uwp/publish/supported-languages)。    |     
| capabilities    |  array   |  程序包所需的功能数组。 有关功能的详细信息，请参阅[应用功能声明](https://docs.microsoft.com/windows/uwp/packaging/app-capability-declarations)。   |     
| minimumDirectXVersion    |  string   |  应用包支持的最低 DirectX 版本。 这可以仅针对面向 Windows 8.x 的应用进行设置；对于面向其他版本的应用，它将忽略。 这可以是以下值之一： <ul><li>无</li><li>DirectX93</li><li>DirectX100</li></ul>   |     
| minimumSystemRam    | string    |  应用包所需的最小 RAM。 这可以仅针对面向 Windows 8.x 的应用进行设置；对于面向其他版本的应用，它将忽略。 这可以是以下值之一： <ul><li>无</li><li>Memory2GB</li></ul>   |    


<span id="package-delivery-options-object" />

### <a name="package-delivery-options-resource"></a>软件包递送选项资源

此资源包含提交的逐步软件包推出和强制更新设置。

```json
{
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
}
```

此资源具有以下值。

| ReplTest1           | 在任务栏的搜索框中键入    | 描述        |
|-----------------|---------|------|
| packageRollout   |   对象      |   包含提交的逐步软件包推出设置的[软件包推出资源](#package-rollout-object)。    |  
| isMandatoryUpdate    | boolean    |  指示是否要将此提交中的软件包视为对自行安装的应用更新强制。 有关自行安装的应用更新的强制软件包的详细信息，请参阅[为应用下载并安装包更新](../packaging/self-install-package-updates.md)。    |  
| mandatoryUpdateEffectiveDate    |  date   |  此提交中的软件包变为强制的日期和时间，采用 ISO 8601 格式和 UTC 时区。   |        

<span id="package-rollout-object" />

### <a name="package-rollout-resource"></a>软件包推出资源

此资源包含提交的逐步[软件包推出设置](#manage-gradual-package-rollout)。 此资源具有以下值。

| ReplTest1           | 在任务栏的搜索框中键入    | 描述        |
|-----------------|---------|------|
| isPackageRollout   |   boolean      |  指示是否为提交启用逐步软件包推出。    |  
| packageRolloutPercentage    | 浮点数    |  将在逐步推出中收到软件包的用户百分比。    |  
| packageRolloutStatus    |  string   |  以下指示逐步软件包推出状态的字符串之一： <ul><li>PackageRolloutNotStarted</li><li>PackageRolloutInProgress</li><li>PackageRolloutComplete</li><li>PackageRolloutStopped</li></ul>  |  
| fallbackSubmissionId    |  string   |  将由不获取逐步推出软件包的客户接收的提交 ID。   |          

> [!NOTE]
> *PackageRolloutStatus*并*fallbackSubmissionId*值通过合作伙伴中心分配，并且不应由开发人员设置。 如果已将这些值包括在请求正文中，则将忽略这些值。

<span/>

## <a name="enums"></a>枚举

这些方法使用以下枚举。

<span id="submission-status-code" />

### <a name="submission-status-code"></a>提交状态代码

以下代码表示提交的状态。

| 代码           |  描述      |
|-----------------|---------------|
|  无            |     未指定任何代码。         |     
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
| 其他  | 提交处于无法识别或未分类的状态。 |
| PackageValidationWarning | 程序包验证过程导致出现警告。 |

<span/>

## <a name="related-topics"></a>相关主题

* [创建和管理使用 Microsoft Store 服务的提交](create-and-manage-submissions-using-windows-store-services.md)
* [管理包航班使用 Microsoft Store 提交 API](manage-flights.md)
* [获取包航班提交](get-a-flight-submission.md)
* [创建包航班提交](create-a-flight-submission.md)
* [更新包航班提交](update-a-flight-submission.md)
* [提交包航班提交](commit-a-flight-submission.md)
* [删除包航班提交](delete-a-flight-submission.md)
* [获取包航班提交状态](get-status-for-a-flight-submission.md)
