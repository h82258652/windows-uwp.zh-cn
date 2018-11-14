---
author: Xansky
ms.assetid: A0DFF26B-FE06-459B-ABDC-3EA4FEB7A21E
description: 在 Microsoft Store 提交 API 中使用此方法，可获取现有软件包外部测试版提交的数据。
title: 获取软件包外部测试版提交
ms.author: mhopkins
ms.date: 04/17/2018
ms.topic: article
keywords: windows 10, uwp, Microsoft Store 提交 API, 外部测试版提交
ms.localizationpriority: medium
ms.openlocfilehash: f9f40219503c0a57f76fcee81858acf51f59b1df
ms.sourcegitcommit: 71e8eae5c077a7740e5606298951bb78fc42b22c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/13/2018
ms.locfileid: "6645738"
---
# <a name="get-a-package-flight-submission"></a>获取软件包外部测试版提交

在 Microsoft Store 提交 API 中使用此方法，可获取现有软件包外部测试版提交的数据。 有关通过使用 Microsoft Store 提交 API 创建软件包外部测试版提交过程的详细信息，请参阅[管理软件包外部测试版提交](manage-flight-submissions.md)。

## <a name="prerequisites"></a>先决条件

若要使用此方法，首先需要执行以下操作：

* 如果尚未开始操作，请先完成 Microsoft Store 提交 API 的所有[先决条件](create-and-manage-submissions-using-windows-store-services.md#prerequisites)。
* [获取 Azure AD 访问令牌](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token)，以供在此方法的请求标头中使用。 获取访问令牌后，在它到期前，你有 60 分钟的使用时间。 该令牌到期后，可以获取新的令牌。
* 在合作伙伴中心中创建软件包外部测试版提交的应用。 你可以执行此操作在合作伙伴中心，或者你可以执行此操作通过使用[创建软件包外部测试版提交](create-a-flight-submission.md)的方法。

## <a name="request"></a>请求

此方法具有以下语法。 请参阅以下部分，获取标头和请求正文的使用示例和描述。

| 方法 | 请求 URI                                                      |
|--------|------------------------------------------------------------------|
| GET   | ```https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}/submissions{submissionId}``` |


### <a name="request-header"></a>请求头

| 标头        | 类型   | 说明                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| 授权 | 字符串 | 必需。 Azure AD 访问令牌的格式为 **Bearer** &lt;*token*&gt;。 |


### <a name="request-parameters"></a>请求参数

| 名称        | 类型   | 描述                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| applicationId | 字符串 | 必需。 应用（包含要获取的软件包外部测试版提交）的应用商店 ID。 有关应用商店 ID 的详细信息，请参阅[查看应用标识详细信息](https://msdn.microsoft.com/windows/uwp/publish/view-app-identity-details)。  |
| flightId | 字符串 | 必需。 软件包外部测试版（包含要获取的提交）的 ID。 在[创建软件包外部测试版](create-a-flight.md)和[获取应用的软件包外部测试版](get-flights-for-an-app.md)请求的响应数据中提供了此 ID。 在合作伙伴中心中创建外部测试版，此 ID 也包含在合作伙伴中心中的外部测试版页面的 URL 中可用。  |
| submissionId | 字符串 | 必需。 要获取的提交的 ID。 此 ID 包含在[创建软件包外部测试版提交](create-a-flight-submission.md)请求的响应数据中。 对于在合作伙伴中心中创建的提交，此 ID 也包含在合作伙伴中心中的提交页面的 URL 中可用。  |


### <a name="request-body"></a>请求正文

请勿为此方法提供请求正文。

### <a name="request-example"></a>请求示例

以下示例演示如何为具有应用商店 ID 9WZDNCRD91MD 的应用获取软件包外部测试版提交。

```
POST https://manage.devcenter.microsoft.com/v1.0/my/applications/9NBLGGH4R315/flights/43e448df-97c9-4a43-a0bc-2a445e736bcd/submissions/1152921504621243649 HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>响应

以下示例演示了成功调用此方法的 JSON 响应正文。 响应正文包含指定提交的相关信息。 有关响应正文中这些值的更多详细信息，请参阅[软件包外部测试版提交资源](manage-flight-submissions.md#flight-submission-object)。

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

## <a name="error-codes"></a>错误代码

如果无法成功完成请求，该响应中会包含以下 HTTP 错误代码之一。

| 错误代码 |  描述   |
|--------|------------------|
| 404  | 找不到软件包外部测试版提交。 |
| 409  | 软件包外部测试版提交不属于指定的软件包外部测试版，或者应用使用[当前不受 Microsoft Store 提交 API](create-and-manage-submissions-using-windows-store-services.md#not_supported)的合作伙伴中心功能。 |   


## <a name="related-topics"></a>相关主题

* [使用 Microsoft Store 服务创建和管理提交](create-and-manage-submissions-using-windows-store-services.md)
* [管理软件包外部测试版提交](manage-flight-submissions.md)
* [创建软件包外部测试版提交](create-a-flight-submission.md)
* [确认软件包外部测试版提交](commit-a-flight-submission.md)
* [更新软件包外部测试版提交](update-a-flight-submission.md)
* [删除软件包外部测试版提交](delete-a-flight-submission.md)
