---
author: Xansky
ms.assetid: 55315F38-6EC5-4889-A14E-7D8EC282FE98
description: 在 Microsoft Store 提交 API 中使用此方法，可获取加载项提交的状态。
title: 获取加载项提交的状态
ms.author: mhopkins
ms.date: 04/17/2018
ms.topic: article
keywords: windows 10, uwp, Microsoft Store 提交 API, 加载项提交, 状态
ms.localizationpriority: medium
ms.openlocfilehash: 0a656d06c2be720e4231ccf4ee0e5626466b5fd8
ms.sourcegitcommit: 70ab58b88d248de2332096b20dbd6a4643d137a4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/01/2018
ms.locfileid: "5929745"
---
# <a name="get-the-status-of-an-add-on-submission"></a>获取加载项提交的状态

在 Microsoft Store 提交 API 中使用此方法，可获取加载项（也称为应用内产品或 IAP）提交的状态。 有关通过使用 Microsoft Store 提交 API 创建加载项提交过程的详细信息，请参阅[管理加载项提交](manage-add-on-submissions.md)。

## <a name="prerequisites"></a>先决条件

若要使用此方法，首先需要执行以下操作：

* 如果尚未开始操作，请先完成 Microsoft Store 提交 API 的所有[先决条件](create-and-manage-submissions-using-windows-store-services.md#prerequisites)。
* [获取 Azure AD 访问令牌](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token)，以供在此方法的请求标头中使用。 获取访问令牌后，在它到期前，你有 60 分钟的使用时间。 该令牌到期后，可以获取新的令牌。
* 使用你的开发人员中心帐户为应用创建加载项提交。 可以使用开发人员中心仪表板执行此操作，也可以通过以下方式执行此操作：使用[创建加载项提交](create-an-add-on-submission.md)方法。

## <a name="request"></a>请求

此方法具有以下语法。 请参阅以下部分，获取标头和请求正文的使用示例和描述。

| 方法 | 请求 URI                                                      |
|--------|------------------------------------------------------------------|
| GET   | ```https://manage.devcenter.microsoft.com/v1.0/my/inappproducts/{inAppProductId}/submissions/{submissionId}/status``` |


### <a name="request-header"></a>请求头

| 标头        | 类型   | 说明                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| 授权 | 字符串 | 必需。 Azure AD 访问令牌的格式为 **Bearer** &lt;*token*&gt;。 |


### <a name="request-parameters"></a>请求参数

| 名称        | 类型   | 描述                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| inAppProductId | 字符串 | 必需。 加载项（包含要获取状态的提交）的应用商店 ID。 开发人员中心仪表板上会提供该应用商店 ID。  |
| submissionId | 字符串 | 必需。 要获取状态的提交的 ID。 此 ID 包含在[创建加载项提交](create-an-add-on-submission.md)请求的响应数据中。 对于在开发人员中心仪表板中创建的提交，此 ID 也包含在仪表板中的提交页面的 URL 中。  |


### <a name="request-body"></a>请求正文

请勿为此方法提供请求正文。

### <a name="request-example"></a>请求示例

以下示例演示了如何获取加载项提交的状态。

```
GET https://manage.devcenter.microsoft.com/v1.0/my/inappproducts/9NBLGGH4TNMP/submissions/1152921504621243680/status HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>响应

以下示例演示了成功调用此方法的 JSON 响应正文。 响应正文包含指定提交的相关信息。 有关响应正文中这些值的更多详细信息，请参阅以下部分。

```json
{
  "status": "PendingCommit",
  "statusDetails": {
    "errors": [],
    "warnings": [],
    "certificationReports": []
  },
}
```

### <a name="response-body"></a>响应正文

| 值      | 类型   | 描述                                                                                                                                                                                                                                                                         |
|------------|--------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 状态           | 字符串  | 提交的状态。 这可以是以下值之一： <ul><li>None</li><li>Canceled</li><li>PendingCommit</li><li>CommitStarted</li><li>CommitFailed</li><li>PendingPublication</li><li>Publishing</li><li>Published</li><li>PublishFailed</li><li>PreProcessing</li><li>PreProcessingFailed</li><li>Certification</li><li>CertificationFailed</li><li>Release</li><li>ReleaseFailed</li></ul>   |
| statusDetails           | 对象  |  包含有关提交状态的附加详细信息，其中包括任何错误的相关信息。 有关详细信息，请参阅[状态详细信息资源](manage-add-on-submissions.md#status-details-object)。 |


## <a name="error-codes"></a>错误代码

如果无法成功完成请求，该响应中会包含以下 HTTP 错误代码之一。

| 错误代码 |  描述   |
|--------|------------------|
| 404  | 找不到提交。 |
| 409  | 加载项使用的开发人员中心仪表板功能[当前不受 Microsoft Store 提交 API 支持](create-and-manage-submissions-using-windows-store-services.md#not_supported)。  |


## <a name="related-topics"></a>相关主题

* [使用 Microsoft Store 服务创建和管理提交](create-and-manage-submissions-using-windows-store-services.md)
* [获取加载项提交](get-an-add-on-submission.md)
* [创建加载项提交](create-an-add-on-submission.md)
* [确认加载项提交](commit-an-add-on-submission.md)
* [更新加载项提交](update-an-add-on-submission.md)
* [删除加载项提交](delete-an-add-on-submission.md)
