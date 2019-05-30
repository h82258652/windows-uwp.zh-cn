---
ms.assetid: CD866083-EB7F-4389-A907-FC43DC2FCB5E
description: 在 Microsoft Store 提交 API 中使用此方法创建新的包航班提交到合作伙伴中心帐户注册的应用。
title: 创建软件包外部测试版提交
ms.date: 08/03/2017
ms.topic: article
keywords: windows 10, uwp, Microsoft Store 提交 API, 创建外部测试版提交
ms.localizationpriority: medium
ms.openlocfilehash: b6474c566795043a435e70b2b41d4f5b59280c15
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/29/2019
ms.locfileid: "66371979"
---
# <a name="create-a-package-flight-submission"></a>创建软件包外部测试版提交

在 Microsoft Store 提交 API 中使用此方法，为应用的软件包外部测试版创建新提交。 使用此方法成功创建新提交后，[更新提交](update-a-flight-submission.md)以对提交数据进行任何必要更改，然后[确认提交](commit-a-flight-submission.md)以供引入和发布。

有关此方法如何适用通过使用 Microsoft Store 提交 API 创建软件包外部测试版提交过程的详细信息，请参阅[管理软件包外部测试版提交](manage-flight-submissions.md)。

> [!NOTE]
> 此方法可为现有软件包外部测试版创建提交。 若要创建软件包外部测试版，请使用[创建软件包外部测试版](create-a-flight.md)方法。

## <a name="prerequisites"></a>系统必备

若要使用此方法，首先需要执行以下操作：

* 如果尚未开始操作，请先完成 Microsoft Store 提交 API 的所有[先决条件](create-and-manage-submissions-using-windows-store-services.md#prerequisites)。
* [获取 Azure AD 访问令牌](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token)，以供在此方法的请求标头中使用。 获取访问令牌后，在它到期前，你有 60 分钟的使用时间。 该令牌到期后，可以获取新的令牌。
* 创建应用包航班。 可以执行此操作在合作伙伴中心，也可以执行此操作通过使用[创建包航班](create-a-flight.md)方法。

## <a name="request"></a>请求

此方法具有以下语法。 请参阅以下部分，获取标头和请求正文的使用示例和描述。

| 方法 | 请求 URI                                                      |
|--------|------------------------------------------------------------------|
| 发布    | `https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}/submissions` |


### <a name="request-header"></a>请求头

| Header        | 在任务栏的搜索框中键入   | 描述                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| 授权 | string | 必需。 Azure AD 访问令牌的格式为 **Bearer** *token*&lt;&gt;。 |


### <a name="request-parameters"></a>请求参数

| 名称        | 在任务栏的搜索框中键入   | 描述                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| applicationId | string | 必需。 要创建软件包外部测试版提交的应用的应用商店 ID。 有关应用商店 ID 的详细信息，请参阅[查看应用标识详细信息](https://docs.microsoft.com/windows/uwp/publish/view-app-identity-details)。  |
| flightId | string | 必需。 要添加提交的软件包外部测试版的 ID。 [创建软件包外部测试版](create-a-flight.md)和[获取应用的软件包外部测试版](get-flights-for-an-app.md)请求的响应数据中包含此 ID。  |


### <a name="request-body"></a>请求正文

请勿为此方法提供请求正文。

### <a name="request-example"></a>请求示例

以下示例演示了如何为具有应用商店 ID 9WZDNCRD91MD 的应用创建全新的软件包外部测试版提交。

```json
POST https://manage.devcenter.microsoft.com/v1.0/my/applications/9NBLGGH4R315/flights/43e448df-97c9-4a43-a0bc-2a445e736bcd/submissions HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>响应

以下示例演示了成功调用此方法的 JSON 响应正文。 该响应正文包含新提交的相关信息。 有关响应正文中这些值的更多详细信息，请参阅[软件包外部测试版提交资源](manage-flight-submissions.md#flight-submission-object)。

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
| 400  | 由于请求无效，无法创建软件包外部测试版提交。 |
| 409  | 由于应用程序中的当前状态，无法创建包航班提交或应用程序使用的合作伙伴中心功能[目前不支持通过 Microsoft Store 提交 API](create-and-manage-submissions-using-windows-store-services.md#not_supported)。 |   

## <a name="related-topics"></a>相关主题

* [创建和管理使用 Microsoft Store 服务的提交](create-and-manage-submissions-using-windows-store-services.md)
* [管理包航班提交](manage-flight-submissions.md)
* [获取包航班提交](get-a-flight-submission.md)
* [提交包航班提交](commit-a-flight-submission.md)
* [更新包航班提交](update-a-flight-submission.md)
* [删除包航班提交](delete-a-flight-submission.md)
* [获取包航班提交状态](get-status-for-a-flight-submission.md)
