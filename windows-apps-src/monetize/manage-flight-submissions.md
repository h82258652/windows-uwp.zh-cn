---
author: mcleanbyron
ms.assetid: 2A454057-FF14-40D2-8ED2-CEB5F27E0226
description: "在 Windows 应用商店提交 API 中使用这些方法，为注册到 Windows 开发人员中心帐户的应用管理软件包外部测试版提交。"
title: "使用 Windows 应用商店提交 API 管理软件包外部测试版提交"
translationtype: Human Translation
ms.sourcegitcommit: 9b76a11adfab838b21713cb384cdf31eada3286e
ms.openlocfilehash: 7b59bb255774c8050232831e7f0d7a78a921ec6d

---

# 使用 Windows 应用商店提交 API 管理软件包外部测试版提交




在 Windows 应用商店提交 API 中使用以下方法，为注册到 Windows 开发人员中心帐户的应用管理软件包外部测试版提交。 有关 Windows 应用商店提交 API 的介绍（包括使用 API 的先决条件），请参阅[使用 Windows 应用商店服务创建和管理提交](create-and-manage-submissions-using-windows-store-services.md)。

>**注意**
              这些方法只能用于已授予使用 Windows 应用商店提交 API 权限的 Windows 开发人员中心帐户。 并非所有帐户都已启用此权限。 在使用这些方法创建或管理软件包外部测试版的提交前，该软件包外部测试版必须已存在于开发人员中心帐户中。 通过[使用开发人员中心仪表板](https://msdn.microsoft.com/windows/uwp/publish/package-flights)，或者使用[管理软件包外部测试版](manage-flights.md)中所述的 Windows 应用商店提交 API 方法，可以创建软件包外部测试版。

| 方法        | URI    | 描述                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| GET | ```https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}/submissions/{submissionId}``` | 获取现有软件包外部测试版提交的数据。 有关详细信息，请参阅[此文章](get-a-flight-submission.md)。 |
| GET | ```https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}/submissions/{submissionId}/status``` | 获取现有软件包外部测试版提交的状态。 有关详细信息，请参阅[此文章](get-status-for-a-flight-submission.md)。 |
| POST | ```https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}/submissions``` | 为注册到 Windows 开发人员中心帐户的应用创建新的软件包外部测试版提交。 有关详细信息，请参阅[此文章](create-a-flight-submission.md)。 |
| POST | ```https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}/submissions/{submissionId}/commit``` | 向 Windows 开发人员中心确认新的或更新的软件包外部测试版提交。 有关详细信息，请参阅[此文章](commit-a-flight-submission.md)。 |
| PUT | ```https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}/submissions/{submissionId}``` | 更新现有软件包外部测试版提交。 有关详细信息，请参阅[此文章](update-a-flight-submission.md)。 |
| DELETE | ```https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}/submissions/{submissionId}``` | 删除软件包外部测试版提交。 有关详细信息，请参阅[此文章](delete-a-flight-submission.md)。 |
| GET | ```https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}/submissions/{submissionId}/packagerollout``` | 获取软件包外部测试版提交的逐步推出信息。 有关详细信息，请参阅[此文章](get-package-rollout-info-for-a-flight-submission.md)。 |
| POST | ```https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}/submissions/{submissionId}/updatepackagerolloutpercentage``` | 更新软件包外部测试版提交的逐步推出百分比。 有关详细信息，请参阅[此文章](update-the-package-rollout-percentage-for-a-flight-submission.md)。 |
| POST | ```https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}/submissions/{submissionId}/haltpackagerollout``` | 终止软件包外部测试版提交的逐步推出。 有关详细信息，请参阅[此文章](halt-the-package-rollout-for-a-flight-submission.md)。 |
| POST | ```https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}/submissions/{submissionId}/finalizepackagerollout``` | 完成软件包外部测试版提交的逐步推出。 有关详细信息，请参阅[此文章](finalize-the-package-rollout-for-a-flight-submission.md)。 |

<span id="create-a-package-flight-submission">
## 创建软件包外部测试版提交

若要创建软件包外部测试版提交，请遵循此过程。

1. 如果尚未执行此操作，请完成[使用 Windows 应用商店服务创建和管理提交](create-and-manage-submissions-using-windows-store-services.md)中所述的先决条件，包括将 Azure AD 应用程序与 Windows 开发人员中心帐户相关联并获取客户端 ID 和密钥。 你只需执行此操作一次；有了客户端 ID 和密钥后，当你需要创建新的 Azure AD 访问令牌时，可以随时重复使用它们。  

2. [获取 Azure AD 访问令牌](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token)。 在 Windows 应用商店提交 API 中，必须将此访问令牌传递给相关方法。 获取访问令牌后，在它到期前，你有 60 分钟的使用时间。 该令牌到期后，可以获取新的令牌。

3. 通过执行 Windows 应用商店提交 API 中的以下方法[创建软件包外部测试版提交](create-a-flight-submission.md)。 此方法创建新的正在进行的提交，这是你上次发布的提交的副本。

  ```
  POST https://manage.devcenter.microsoft.com/v1.0/my/applications{applicationId}/flights/{flightId}/submissions
  ```

  响应正文包含三个项目：新提交的 ID、新提交的数据（包括所有列表和定价信息），以及用于上传提交的任何软件包的共享访问签名 (SAS) URI。 有关 SAS 的详细信息，请参阅[共享访问签名，第 1 部分：了解 SAS 模型](https://azure.microsoft.com/documentation/articles/storage-dotnet-shared-access-signature-part-1/)。

4. 如果要为提交添加新的软件包，请[准备软件包](https://msdn.microsoft.com/windows/uwp/publish/app-package-requirements)并将它们添加到 ZIP 存档。

5. 使用新提交所需的任何更改修订提交数据，并执行以下方法来[更新软件包外部测试版提交](update-a-flight-submission.md)。

  ```
  PUT https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}/submissions/{submissionId}
  ```

  >**注意**
              如果要为提交添加新软件包，请确保更新提交数据，以便在 ZIP 存档中引用这些文件的名称和相对路径。

4. 如果要为提交添加新软件包，请将 ZIP 存档上传到 SAS URI，该 URI 已在步骤 2 中调用的 POST 方法的响应正文中提供。 有关详细信息，请参阅[共享访问签名，第 2 部分：使用 Blob 存储创建和使用 SAS](https://azure.microsoft.com/documentation/articles/storage-dotnet-shared-access-signature-part-2/)。

  以下代码片段演示了如何在用于 .NET 的 Azure 存储客户端库中使用 [CloudBlockBlob](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.blob.cloudblockblob.aspx) 类上传存档。

  ```csharp
  string sasUrl = "https://productingestionbin1.blob.core.windows.net/ingestion/26920f66-b592-4439-9a9d-fb0f014902ec?sv=2014-02-14&sr=b&sig=usAN0kNFNnYE2tGQBI%2BARQWejX1Guiz7hdFtRhyK%2Bog%3D&se=2016-06-17T20:45:51Z&sp=rwl";

  Microsoft.WindowsAzure.Storage.Blob.CloudBlockBlob blockBob =
      new Microsoft.WindowsAzure.Storage.Blob.CloudBlockBlob(new System.Uri(sasUrl));
  await blockBob.UploadFromStreamAsync(stream);
  ```

5. 通过执行以下方法[确认软件包外部测试版提交](commit-a-flight-submission.md)。 这将向开发人员中心发出警报：已完成提交，更新现在应该应用到了帐户。

  ```
  POST https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}/submissions/{submissionId}/commit
  ```

6. 通过执行以下方法来检查提交状态以[获取软件包外部测试版提交的状态](get-status-for-a-flight-submission.md)。

  ```
  GET https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}/submissions/{submissionId}/status
  ```

  若要确认提交状态，请查看响应正文中的 *status* 值。 如果请求成功，此值应该从 **CommitStarted** 更改为 **PreProcessing**；如果请求中存在错误，此值应该更改为 **CommitFailed**。 如果存在错误，*statusDetails* 字段将包含有关错误的更多详细信息。

7. 在提交成功完成之后，提交会发送至应用商店以供引入。 可以通过使用前面的方法，或者通过访问开发人员中心仪表板，继续监视提交进度。

<span id="manage-gradual-package-rollout">
## 管理软件包外部测试版提交的逐步软件包推出

可在软件包外部测试版提交中逐步向 Windows 10 上一定比例的应用客户推出已更新的软件包。 这使你可以监视特定软件包的反馈和分析数据，从而确保在更广泛地推出更新前对此更新放心。 可更改已发布提交的推出百分比（或终止更新），而无需创建新提交。 有关详细信息，包括有关如何在开发人员中心仪表板中启用和管理逐步软件包推出的说明，请参阅[本文章](../publish/gradual-package-rollout.md)。

还可使用 Windows 应用商店提交 API 中的以下方法以编程方式启用和管理软件包外部测试版提交的逐步软件包推出。

* 若要启用软件包外部测试版提交的逐步软件包推出：

  1. [创建软件包外部测试版提交](create-a-flight-submission.md)或[获取软件包外部测试版提交](get-a-flight-submission.md)。
  2. 在响应数据中，找到 [packageRollout](#package-rollout-object) 资源、将“isPackageRollout”字段设置为 true，然后将“packageRolloutPercentage”字段设置为应获取已更新的软件包的应用客户百分比。
  3. 将已更新的软件包外部测试版提交数据传递到[更新软件包外部测试版提交](update-a-flight-submission.md)方法。

<span/>

* 若要[获取软件包外部测试版提交的软件包推出信息](get-package-rollout-info-for-a-flight-submission.md)，请执行以下方法。

  ```
  GET https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}/submissions/{submissionId}/packagerollout
  ```

<span/>

* 若要[更新软件包外部测试版提交的软件包推出百分比](update-the-package-rollout-percentage-for-a-flight-submission.md)，请执行以下方法。

  ```
  POST https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}/submissions/{submissionId}/updatepackagerolloutpercentage  
  ```

<span/>

* 若要[终止软件包外部测试版提交的软件包推出](halt-the-package-rollout-for-a-flight-submission.md)，请执行以下方法。

  ```
  POST https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}/submissions/{submissionId}/haltpackagerollout   
  ```  

<span/>

* 若要[完成软件包外部测试版提交的软件包推出](finalize-the-package-rollout-for-a-flight-submission.md)，请执行以下方法。

  ```
  POST https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}/submissions/{submissionId}/finalizepackagerollout
  ```

## 资源

这些方法使用以下数据资源。

<span id="flight-submission-object" />
### 软件包外部测试版提交

此资源表示软件包外部测试版的提交。 以下示例演示了此资源的格式。

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
        "packageRolloutPercentage": 0,
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

| 值      | 类型   | 描述                                                                                                                                                                                                                                                                         |
|------------|--------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| id            | 字符串  | 提交的 ID。  |
| flightId           | 字符串  |  提交相关联的软件包外部测试版的 ID。  |  
| status           | 字符串  | 提交的状态。 这可以是以下值之一： <ul><li>None</li><li>Canceled</li><li>PendingCommit</li><li>CommitStarted</li><li>CommitFailed</li><li>PendingPublication</li><li>Publishing</li><li>Published</li><li>PublishFailed</li><li>PreProcessing</li><li>PreProcessingFailed</li><li>Certification</li><li>CertificationFailed</li><li>Release</li><li>ReleaseFailed</li></ul>   |
| statusDetails           | 对象  |  包含有关提交状态的附加详细信息，其中包括任何错误的相关信息。 有关详细信息，请参阅下面[状态详细信息](#status-details-object)部分。 |
| flightPackages           | 数组  | 包含提供提交中关于每个程序包详细信息的对象。 有关详细信息，请参阅下面的[外部测试版软件包](#flight-package-object)部分。  |
| packageDeliveryOptions    | 对象  | 包含提交的逐步软件包推出和强制更新设置。 有关详细信息，请参阅下面的[软件包传递选项对象](#package-delivery-options-object)部分。  |
| fileUploadUrl           | 字符串  | 用于为提交上载任何程序包的共享访问签名 (SAS) URI。 如果要为提交添加新的程序包，请将包含这些程序包的 ZIP 存档上载到此 URI。 有关详细信息，请参阅[创建软件包外部测试版提交](#create-a-package-flight-submission)。  |
| targetPublishMode           | 字符串  | 提交的发布模式。 这可以是以下值之一： <ul><li>Immediate</li><li>Manual</li><li>SpecificDate</li></ul> |
| targetPublishDate           | 字符串  | 提交的发布日期采用 ISO 8601 格式（如果 *targetPublishMode* 设为“SpecificDate”）。  |
| notesForCertification           | 字符串  |  提供认证测试人员的其他信息，例如测试帐户凭据以及访问和验证功能的步骤。 有关详细信息，请参阅[认证说明](https://msdn.microsoft.com/windows/uwp/publish/notes-for-certification)。 |

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


<span id="certification-report-object" />
### 认证报告

此资源提供对提交的认证报告数据的访问权限。 此资源具有以下值。

| 值           | 类型    | 说明                                                                                                                                                                                                                          |
|-----------------|---------|------|
|     date            |    字符串     |  报告生成的日期和时间，采用 ISO 8601 格式。    |
|     reportUrl            |    字符串     |  用于访问报告的 URL。    |


<span id="flight-package-object" />
### 外部测试版软件包

此资源提供有关提交中的程序包的详细信息。 以下示例演示了此资源的格式。

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

>**注意**
              当调用[更新软件包外部测试版提交](update-a-flight-submission.md)方法时，请求正文中仅需要此对象的 *fileName*、*fileStatus*、*minimumDirectXVersion* 和 *minimumSystemRam* 值。 其他值由开发人员中心进行填充。

| 值           | 类型    | 描述                                                                                                                                                                                                                          |
|-----------------|---------|------|
| fileName   |   字符串      |  程序包的名称。    |  
| fileStatus    | 字符串    |  程序包的状态。 这可以是以下值之一： <ul><li>None</li><li>PendingUpload</li><li>Uploaded</li><li>PendingDelete</li></ul>    |  
| id    |  字符串   |  唯一标识程序包的 ID。 此值由开发人员中心使用。   |     
| version    |  字符串   |  应用包的版本。 有关详细信息，请参阅[程序包版本编号](https://msdn.microsoft.com/windows/uwp/publish/package-version-numbering)。   |   
| architecture    |  字符串   |  应用包的体系结构（例如 ARM）。   |     
| languages    | 数组    |  应用所支持的语言的语言代码数组。 有关详细信息，请参阅[支持的语言](https://msdn.microsoft.com/windows/uwp/publish/supported-languages)。    |     
| capabilities    |  数组   |  程序包所需的功能数组。 有关功能的详细信息，请参阅[应用功能声明](https://msdn.microsoft.com/windows/uwp/packaging/app-capability-declarations)。   |     
| minimumDirectXVersion    |  字符串   |  应用包支持的最低 DirectX 版本。 这可以仅针对面向 Windows 8.x 的应用进行设置；对于面向其他版本的应用，它将忽略。 这可以是以下值之一： <ul><li>None</li><li>DirectX93</li><li>DirectX100</li></ul>   |     
| minimumSystemRam    | 字符串    |  应用包所需的最小 RAM。 这可以仅针对面向 Windows 8.x 的应用进行设置；对于面向其他版本的应用，它将忽略。 这可以是以下值之一： <ul><li>None</li><li>Memory2GB</li></ul>   |    


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

<span id="submission-status-code" />
### 提交状态代码

以下代码表示提交的状态。

| 代码           |  描述      |
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

## 相关主题

* [使用 Windows 应用商店服务创建和管理提交](create-and-manage-submissions-using-windows-store-services.md)
* [使用 Windows 应用商店提交 API 管理软件包外部测试版](manage-flights.md)
* [获取软件包外部测试版提交](get-a-flight-submission.md)
* [创建软件包外部测试版提交](create-a-flight-submission.md)
* [更新软件包外部测试版提交](update-a-flight-submission.md)
* [确认软件包外部测试版提交](commit-a-flight-submission.md)
* [删除软件包外部测试版提交](delete-a-flight-submission.md)
* [获取软件包外部测试版提交的状态](get-status-for-a-flight-submission.md)



<!--HONumber=Nov16_HO1-->


