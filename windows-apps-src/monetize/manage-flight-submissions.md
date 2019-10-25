---
ms.assetid: 2A454057-FF14-40D2-8ED2-CEB5F27E0226
description: 使用 Microsoft Store 提交 API 中的这些方法为注册到合作伙伴中心帐户的应用管理包航班提交。
title: 管理包航班提交
ms.date: 04/16/2018
ms.topic: article
keywords: windows 10，uwp，Microsoft Store 提交 API，航班提交
ms.localizationpriority: medium
ms.openlocfilehash: 50596fdadae2ac4a0625687e7c8acaf985ccfaa7
ms.sourcegitcommit: f561efbda5c1d47b85601d91d70d86c5332bbf8c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/21/2019
ms.locfileid: "72690377"
---
# <a name="manage-package-flight-submissions"></a>管理包航班提交

Microsoft Store 提交 API 提供了可用于管理应用的包航班提交的方法，包括逐步包的部署。 有关 Microsoft Store 提交 API 的简介，包括使用 API 的先决条件，请参阅[使用 Microsoft Store 服务创建和管理提交](create-and-manage-submissions-using-windows-store-services.md)。

> [!IMPORTANT]
> 如果使用 Microsoft Store 提交 API 来创建包航班的提交，请确保只使用 API 而不是合作伙伴中心对提交进行进一步更改。 如果使用该仪表板更改最初使用 API 创建的提交，将无法再使用 API 更改或提交该提交。 在某些情况下，提交可能处于错误状态，在这种情况下，无法在提交过程中继续进行。 如果出现这种情况，则必须删除提交并创建新的提交。

<span id="methods-for-package-flight-submissions" />

## <a name="methods-for-managing-package-flight-submissions"></a>用于管理包航班提交的方法

使用以下方法可获取、创建、更新、提交或删除包航班提交。 在可以使用这些方法之前，包航班必须已存在于合作伙伴中心。 可以在[合作伙伴中心](https://docs.microsoft.com/windows/uwp/publish/package-flights)创建包航班，或使用[管理包航班](manage-flights.md)中所述的 Microsoft Store 提交 API 方法。

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
<td align="left">https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}/submissions/{submissionId}</td>
<td align="left"><a href="get-a-flight-submission.md">获取现有包航班提交</a></td>
</tr>
<tr>
<td align="left">GET</td>
<td align="left">https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}/submissions/{submissionId}/status</td>
<td align="left"><a href="get-status-for-a-flight-submission.md">获取现有包航班提交状态</a></td>
</tr>
<tr>
<td align="left">POST</td>
<td align="left">https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}/submissions</td>
<td align="left"><a href="create-a-flight-submission.md">创建新的包航班提交</a></td>
</tr>
<tr>
<td align="left">PUT</td>
<td align="left">https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}/submissions/{submissionId}</td>
<td align="left"><a href="update-a-flight-submission.md">更新现有的包航班提交</a></td>
</tr>
<tr>
<td align="left">POST</td>
<td align="left">https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}/submissions/{submissionId}/commit</td>
<td align="left"><a href="commit-a-flight-submission.md">提交新的或更新的包航班提交</a></td>
</tr>
<tr>
<td align="left">删除</td>
<td align="left">https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}/submissions/{submissionId}</td>
<td align="left"><a href="delete-a-flight-submission.md">删除包航班提交</a></td>
</tr>
</tbody>
</table>

<span id="create-a-package-flight-submission">

## <a name="create-a-package-flight-submission"></a>创建包航班提交

若要创建包航班的提交，请按照此过程操作。

1. 如果尚未执行此操作，请完成[使用 Microsoft Store 服务创建和管理提交](create-and-manage-submissions-using-windows-store-services.md)中所述的先决条件，包括将 Azure AD 应用程序与合作伙伴中心帐户关联并获取客户端 ID 和密钥。 只需执行此操作一次;获得客户端 ID 和密钥后，无论何时需要创建新的 Azure AD 访问令牌，都可以重复使用。  

2. [获取 Azure AD 的访问令牌](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token)。 必须将此访问令牌传递到 Microsoft Store 提交 API 中的方法。 获取访问令牌后，你将有60分钟的时间，以便在过期前使用它。 令牌过期后，你可以获取一个新令牌。

3. 通过在 Microsoft Store 提交 API 中执行以下方法来[创建包航班提交](create-a-flight-submission.md)。 此方法会创建一个新的正在进行的提交，这是你上次发布的提交的副本。

    ```json
    POST https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}/submissions
    ```

    响应正文包含[航班提交](#flight-submission-object)资源，其中包括新提交的 ID、用于上传到 Azure Blob 存储的任何包的共享访问签名（SAS） URI 以及新提交的数据（包括所有列表和定价信息）。

    > [!NOTE]
    > SAS URI 提供对 Azure 存储中的安全资源的访问权限，而无需帐户密钥。 有关 SAS Uri 及其与 Azure Blob 存储一起使用的背景信息，请参阅[共享访问签名，第1部分：了解 sas 模型](https://azure.microsoft.com/documentation/articles/storage-dotnet-shared-access-signature-part-1/)和[共享访问签名，第2部分：创建 SAS 并将 sas 用于 Blob 存储](https://azure.microsoft.com/documentation/articles/storage-dotnet-shared-access-signature-part-2/)。

4. 如果要为提交添加新包，请[准备包](https://docs.microsoft.com/windows/uwp/publish/app-package-requirements)并将其添加到 ZIP 存档。

5. 为新提交的任何所需更改修改[航班提交](#flight-submission-object)数据，并执行以下方法来[更新包航班提交](update-a-flight-submission.md)。

    ```json
    PUT https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}/submissions/{submissionId}
    ```
      > [!NOTE]
      > 如果要为提交添加新包，请确保更新提交数据，以引用 ZIP 存档中这些文件的名称和相对路径。

4. 如果要为提交添加新包，请使用之前调用的 POST 方法的响应正文中提供的 SAS URI 将 ZIP 存档上传到[Azure Blob 存储](https://docs.microsoft.com/azure/storage/storage-introduction#blob-storage)。 可以使用不同的 Azure 库在各种平台上执行此操作，包括：

    * [适用于 .NET 的 Azure 存储客户端库](https://docs.microsoft.com/azure/storage/storage-dotnet-how-to-use-blobs)
    * [用于 Java 的 Azure 存储 SDK](https://docs.microsoft.com/azure/storage/storage-java-how-to-use-blob-storage)
    * [用于 Python 的 Azure 存储 SDK](https://docs.microsoft.com/azure/storage/storage-python-how-to-use-blob-storage)

    下面C#的代码示例演示了如何使用用于 .Net 的 Azure 存储客户端库中的[CLOUDBLOCKBLOB](https://docs.microsoft.com/dotnet/api/microsoft.windowsazure.storage.blob.cloudblockblob)类将 ZIP 存档上传到 azure Blob 存储。 此示例假定 ZIP 存档已写入流对象。

    ```csharp
    string sasUrl = "https://productingestionbin1.blob.core.windows.net/ingestion/26920f66-b592-4439-9a9d-fb0f014902ec?sv=2014-02-14&sr=b&sig=usAN0kNFNnYE2tGQBI%2BARQWejX1Guiz7hdFtRhyK%2Bog%3D&se=2016-06-17T20:45:51Z&sp=rwl";
    Microsoft.WindowsAzure.Storage.Blob.CloudBlockBlob blockBob =
        new Microsoft.WindowsAzure.Storage.Blob.CloudBlockBlob(new System.Uri(sasUrl));
    await blockBob.UploadFromStreamAsync(stream);
    ```

5. 通过执行以下方法[提交包航班提交](commit-a-flight-submission.md)。 这会提醒合作伙伴中心你已完成提交，你的更新现在应应用到你的帐户。

    ```json
    POST https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}/submissions/{submissionId}/commit
    ```

6. 通过执行以下方法来查看提交状态，以[获取包航班提交状态](get-status-for-a-flight-submission.md)。

    ```json
    GET https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}/submissions/{submissionId}/status
    ```

    若要确认提交状态，请在响应正文中查看*状态值*。 如果请求成功，则此值应更改为**CommitStarted** ，**或者如果请求**中存在错误，则为**CommitFailed** 。 如果有错误， *statusDetails*字段将包含有关错误的更多详细信息。

7. 成功完成提交后，会将提交发送到存储区进行引入。 你可以继续使用以前的方法或通过访问合作伙伴中心来监视提交进度。

<span/>

## <a name="code-examples"></a>代码示例

以下文章提供了详细的代码示例，演示如何使用几种不同的编程语言创建包航班提交：

* [C#代码示例](csharp-code-examples-for-the-windows-store-submission-api.md)
* [Java 代码示例](java-code-examples-for-the-windows-store-submission-api.md)
* [Python 代码示例](python-code-examples-for-the-windows-store-submission-api.md)

## <a name="storebroker-powershell-module"></a>StoreBroker PowerShell 模块

作为直接调用 Microsoft Store 提交 API 的替代方法，我们还提供了一个开源 PowerShell 模块，该模块在 API 的基础上实现一个命令行接口。 此模块名为[StoreBroker](https://aka.ms/storebroker)。 你可以使用此模块从命令行管理应用、航班和加载项提交，而不是直接调用 Microsoft Store 提交 API，也可以直接浏览源以查看有关如何调用此 API 的更多示例。 在 Microsoft 中，StoreBroker 模块积极地用于将许多第一方应用程序提交到应用商店的主要方式。

有关详细信息，请参阅[GitHub 上的 StoreBroker 页](https://aka.ms/storebroker)。

<span id="manage-gradual-package-rollout">

## <a name="manage-a-gradual-package-rollout-for-a-package-flight-submission"></a>管理包航班提交的逐步包推出

你可以在 Windows 10 上，将包航班提交中的更新包逐步推出到应用的客户所占的百分比。 这样，你就可以监视特定包的反馈和分析数据，以确保在更广泛地推出更新之前对更新十分自信。 你可以更改已发布提交的推出百分比（或停止更新），而无需创建新的提交。 有关更多详细信息，包括如何在合作伙伴中心启用和管理逐步包推出的说明，请参阅[此文](../publish/gradual-package-rollout.md)。

若要以编程方式为包航班提交启用逐步包推出，请使用 Microsoft Store 提交 API 中的方法执行此过程：

  1. [创建包航班提交](create-a-flight-submission.md)或[获取包航班提交](get-a-flight-submission.md)。
  2. 在响应数据中，找到 " [packageRollout](#package-rollout-object) " 资源，将 " *isPackageRollout* " 字段设置为 "true"，并将 " *packageRolloutPercentage* " 字段设置为应该获取更新包的应用客户的百分比。
  3. 将更新的包航班提交数据传递到[更新包航班提交](update-a-flight-submission.md)方法。

为包航班提交启用逐步包推出后，可以使用以下方法以编程方式获取、更新、暂停或完成逐步推出。

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
<td align="left">https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}/submissions/{submissionId}/packagerollout</td>
<td align="left"><a href="get-package-rollout-info-for-a-flight-submission.md">获取包航班提交的逐步推出信息</a></td>
</tr>
<tr>
<td align="left">POST</td>
<td align="left">https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}/submissions/{submissionId}/updatepackagerolloutpercentage</td>
<td align="left"><a href="update-the-package-rollout-percentage-for-a-flight-submission.md">更新包航班提交的逐步推出百分比</a></td>
</tr>
<tr>
<td align="left">POST</td>
<td align="left">https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}/submissions/{submissionId}/haltpackagerollout</td>
<td align="left"><a href="halt-the-package-rollout-for-a-flight-submission.md">暂停包航班提交的逐步推出</a></td>
</tr>
<tr>
<td align="left">POST</td>
<td align="left">https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}/submissions/{submissionId}/finalizepackagerollout</td>
<td align="left"><a href="finalize-the-package-rollout-for-a-flight-submission.md">完成包航班提交的逐步推出</a></td>
</tr>
</tbody>
</table>

<span/>

## <a name="data-resources"></a>数据资源

用于管理包航班提交的 Microsoft Store 提交 API 方法使用以下 JSON 数据资源。

<span id="flight-submission-object" />

### <a name="flight-submission-resource"></a>航班提交资源

此资源描述了包航班提交。

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

| 值      | 类型   | 说明              |
|------------|--------|------------------------------|
| id            | 字符串  | 提交的 ID。  |
| flightId           | 字符串  |  与提交关联的包航班的 ID。  |  
| status           | 字符串  | 提交状态。 这可以是以下值之一： <ul><li>无</li><li>已取消</li><li>PendingCommit</li><li>CommitStarted</li><li>CommitFailed</li><li>PendingPublication</li><li>发布</li><li>已发布</li><li>PublishFailed</li><li>预处理</li><li>PreProcessingFailed</li><li>认证</li><li>CertificationFailed</li><li>发布</li><li>ReleaseFailed</li></ul>   |
| statusDetails           | 对象  |  "[状态详细信息" 资源](#status-details-object)，其中包含有关提交状态的其他详细信息，包括有关任何错误的信息。  |
| flightPackages           | Array  | 包含提供有关提交中每个包的详细信息的[航班包资源](#flight-package-object)。   |
| packageDeliveryOptions    | 对象  | [包传递选项资源](#package-delivery-options-object)，其中包含用于提交的逐步包推出和强制更新设置。   |
| fileUploadUrl           | 字符串  | 用于上传提交的任何包的共享访问签名（SAS） URI。 如果要为提交添加新包，请将包含包的 ZIP 存档上传到此 URI。 有关详细信息，请参阅[创建包航班提交](#create-a-package-flight-submission)。  |
| targetPublishMode           | 字符串  | 提交的发布模式。 这可以是以下值之一： <ul><li>即时</li><li>手动</li><li>SpecificDate</li></ul> |
| targetPublishDate           | 字符串  | 如果*targetPublishMode*设置为 SpecificDate，则以 ISO 8601 格式提交的发布日期。  |
| notesForCertification           | 字符串  |  提供认证测试人员的其他信息，例如测试帐户凭据和访问和验证功能的步骤。 有关详细信息，请参阅[认证说明](https://docs.microsoft.com/windows/uwp/publish/notes-for-certification)。 |

<span id="status-details-object" />

### <a name="status-details-resource"></a>状态详细信息资源

此资源包含有关提交状态的其他详细信息。 此资源具有以下值。

| 值           | 类型    | 说明                   |
|-----------------|---------|------|
|  错误               |    对象     |   一组[状态详细信息资源](#status-detail-object)，其中包含提交的错误详细信息。   |     
|  列出               |   对象      | 一组[状态详细信息资源](#status-detail-object)，其中包含提交的警告详细信息。     |
|  certificationReports               |     对象    |   [证书报表资源](#certification-report-object)的数组，这些资源提供对提交的证书报表数据的访问。 如果证书失败，你可以检查这些报表以获取详细信息。    |  


<span id="status-detail-object" />

### <a name="status-detail-resource"></a>状态详细信息资源

此资源包含有关提交的任何相关错误或警告的其他信息。 此资源具有以下值。

| 值           | 类型    | 说明       |
|-----------------|---------|------|
|  代码               |    字符串     |   描述错误或警告类型的[提交状态代码](#submission-status-code)。 |  
|  详细信息               |     字符串    |  包含有关问题的更多详细信息的消息。     |


<span id="certification-report-object" />

### <a name="certification-report-resource"></a>证书报表资源

此资源提供对用于提交的证书报表数据的访问。 此资源具有以下值。

| 值           | 类型    | 说明         |
|-----------------|---------|------|
|     日期            |    字符串     |  生成报表的日期和时间，采用 ISO 8601 格式。    |
|     reportUrl            |    字符串     |  可以访问报表的 URL。    |


<span id="flight-package-object" />

### <a name="flight-package-resource"></a>航班包资源

此资源提供有关提交中的包的详细信息。

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
> 当调用[更新包航班提交](update-a-flight-submission.md)方法时，请求正文中只需要此对象的*fileName*、 *fileStatus*、 *minimumDirectXVersion*和*minimumSystemRam*值。 其他值由合作伙伴中心填充。

| 值           | 类型    | 说明              |
|-----------------|---------|------|
| fileName   |   字符串      |  包的名称。    |  
| fileStatus    | 字符串    |  包的状态。 这可以是以下值之一： <ul><li>无</li><li>PendingUpload</li><li>已上传</li><li>PendingDelete</li></ul>    |  
| id    |  字符串   |  唯一标识包的 ID。 此值由合作伙伴中心使用。   |     
| version    |  字符串   |  应用包的版本。 有关详细信息，请参阅[包版本编号](https://docs.microsoft.com/windows/uwp/publish/package-version-numbering)。   |   
| 体系结构    |  字符串   |  应用程序包（例如 ARM）的体系结构。   |     
| 语言    | Array    |  应用支持的语言的语言代码的数组。 有关详细信息，请参阅有关详细信息，请参阅[支持的语言](https://docs.microsoft.com/windows/uwp/publish/supported-languages)。    |     
| capabilities    |  Array   |  包所需的一组功能。 有关功能的详细信息，请参阅[应用功能声明](https://docs.microsoft.com/windows/uwp/packaging/app-capability-declarations)。   |     
| minimumDirectXVersion    |  字符串   |  应用包支持的最低 DirectX 版本。 此设置仅适用于面向 Windows 8.x 的应用;对于面向其他版本的应用，它将被忽略。 这可以是以下值之一： <ul><li>无</li><li>DirectX93</li><li>DirectX100</li></ul>   |     
| minimumSystemRam    | 字符串    |  应用程序包所需的最小 RAM。 此设置仅适用于面向 Windows 8.x 的应用;对于面向其他版本的应用，它将被忽略。 这可以是以下值之一： <ul><li>无</li><li>Memory2GB</li></ul>   |    


<span id="package-delivery-options-object" />

### <a name="package-delivery-options-resource"></a>包传递选项资源

此资源包含逐步包的推出和强制更新设置以进行提交。

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

| 值           | 类型    | 说明        |
|-----------------|---------|------|
| packageRollout   |   对象      |   [包推出资源](#package-rollout-object)，其中包含用于提交的逐步包推出设置。    |  
| isMandatoryUpdate    | 布尔值    |  指示是否要将此提交中的包视为自行安装应用更新所必需的。 有关自行安装应用更新的必需包的详细信息，请参阅[下载并安装应用的包更新](../packaging/self-install-package-updates.md)。    |  
| mandatoryUpdateEffectiveDate    |  日期   |  此提交中的包变为必需的日期和时间，采用 ISO 8601 格式和 UTC 时区。   |        

<span id="package-rollout-object" />

### <a name="package-rollout-resource"></a>包推出资源

此资源包含用于提交的逐步[包推出设置](#manage-gradual-package-rollout)。 此资源具有以下值。

| 值           | 类型    | 说明        |
|-----------------|---------|------|
| isPackageRollout   |   布尔值      |  指示是否为提交启用了逐步包推出。    |  
| packageRolloutPercentage    | float    |  将在逐步推出中接收包的用户的百分比。    |  
| packageRolloutStatus    |  字符串   |  以下指示逐步包推出状态的字符串之一： <ul><li>PackageRolloutNotStarted</li><li>PackageRolloutInProgress</li><li>PackageRolloutComplete</li><li>PackageRolloutStopped</li></ul>  |  
| fallbackSubmissionId    |  字符串   |  没有获得逐步推出包的客户将收到的提交 ID。   |          

> [!NOTE]
> *PackageRolloutStatus*和*FallbackSubmissionId*值由合作伙伴中心分配，不应由开发人员设置。 如果在请求正文中包含这些值，则这些值将被忽略。

<span/>

## <a name="enums"></a>枚举

这些方法使用以下枚举。

<span id="submission-status-code" />

### <a name="submission-status-code"></a>提交状态代码

以下代码表示提交的状态。

| 代码           |  说明      |
|-----------------|---------------|
|  无            |     未指定任何代码。         |     
|      InvalidArchive        |     包含包的 ZIP 存档无效或具有无法识别的存档格式。  |
| MissingFiles | ZIP 存档没有在提交数据中列出的所有文件，或者这些文件位于存档中的错误位置。 |
| PackageValidationFailed | 提交中的一个或多个包验证失败。 |
| InvalidParameterValue | 请求正文中的一个参数无效。 |
| InvalidOperation | 尝试的操作无效。 |
| InvalidState | 尝试的操作对包航班的当前状态无效。 |
| ResourceNotFound | 找不到指定的包飞行。 |
| ServiceError | 内部服务错误阻止请求成功。 请重试该请求。 |
| ListingOptOutWarning | 开发人员从上一个提交中删除了一个列表，或未包含包所支持的列表信息。 |
| ListingOptInWarning  | 开发人员添加了一个列表。 |
| UpdateOnlyWarning | 开发人员试图插入仅具有更新支持的内容。 |
| 其他  | 提交处于无法识别或未分类状态。 |
| PackageValidationWarning | 包验证过程导致了警告。 |

<span/>

## <a name="related-topics"></a>相关主题

* [使用 Microsoft Store services 创建和管理提交](create-and-manage-submissions-using-windows-store-services.md)
* [使用 Microsoft Store 提交 API 管理包航班](manage-flights.md)
* [获取包航班提交](get-a-flight-submission.md)
* [创建包航班提交](create-a-flight-submission.md)
* [更新包航班提交](update-a-flight-submission.md)
* [提交包航班提交](commit-a-flight-submission.md)
* [删除包航班提交](delete-a-flight-submission.md)
* [获取包航班提交状态](get-status-for-a-flight-submission.md)
