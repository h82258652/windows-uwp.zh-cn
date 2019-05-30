---
ms.assetid: 934F2DBF-2C7E-4B77-997D-17B9B0535D51
description: 使用此方法中的 Microsoft Store 提交 API 提交新的或更新应用程序提交到合作伙伴中心。
title: 确认应用提交
ms.date: 04/17/2018
ms.topic: article
keywords: windows 10, uwp, Microsoft Store 提交 API, 确认应用提交
ms.localizationpriority: medium
ms.openlocfilehash: 36c645303a86bc28f12fa8e31b8a83653c167664
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/29/2019
ms.locfileid: "66371993"
---
# <a name="commit-an-app-submission"></a>确认应用提交

使用此方法中的 Microsoft Store 提交 API 提交新的或更新应用程序提交到合作伙伴中心。 提交操作警报合作伙伴中心已 （包括任何相关的程序包和图像），上载提交数据。 在响应中，合作伙伴中心提交用于引入和发布的提交数据所做的更改。 提交操作成功后，在合作伙伴中心中显示的提交更改。

有关确认操作如何适用通过使用 Microsoft Store 提交 API 提交应用过程的详细信息，请参阅[管理应用提交](manage-app-submissions.md)。

## <a name="prerequisites"></a>先决条件

若要使用此方法，首先需要执行以下操作：

* 如果尚未开始操作，请先完成 Microsoft Store 提交 API 的所有[先决条件](create-and-manage-submissions-using-windows-store-services.md#prerequisites)。
* [获取 Azure AD 访问令牌](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token)，以供在此方法的请求标头中使用。 获取访问令牌后，在它到期前，你有 60 分钟的使用时间。 该令牌到期后，可以获取新的令牌。
* [创建应用提交](create-an-app-submission.md)，然后通过对提交数据所做的任何必要更改[更新提交](update-an-app-submission.md)。

## <a name="request"></a>请求

此方法具有以下语法。 请参阅以下部分，获取标头和请求正文的使用示例和描述。

| 方法 | 请求 URI                                                      |
|--------|------------------------------------------------------------------|
| 发布    | `https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/submissions/{submissionId}/commit` |


### <a name="request-header"></a>请求头

| Header        | 在任务栏的搜索框中键入   | 描述                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| 授权 | string | 必需。 Azure AD 访问令牌的格式为 **Bearer** *token*&lt;&gt;。 |


### <a name="request-parameters"></a>请求参数

| 名称        | 在任务栏的搜索框中键入   | 描述                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| applicationId | string | 必需。 应用（包含要确认的提交）的应用商店 ID。 有关应用商店 ID 的详细信息，请参阅[查看应用标识详细信息](https://docs.microsoft.com/windows/uwp/publish/view-app-identity-details)。  |
| submissionId | string | 必需。 要确认的提交的 ID。 此 ID 包含在[创建应用提交](create-an-app-submission.md)请求的响应数据中。 在合作伙伴中心创建的提交，此 ID 是也可用在合作伙伴中心中的提交页的 URL。  |

### <a name="request-body"></a>请求正文

请勿为此方法提供请求正文。

### <a name="request-example"></a>请求示例

以下示例演示了如何确认应用提交。

```json
POST https://manage.devcenter.microsoft.com/v1.0/my/applications/9NBLGGH4R315/submissions/1152921504621243610/commit HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>响应

以下示例演示了成功调用此方法的 JSON 响应正文。 有关响应正文中这些值的更多详细信息，请参阅以下部分。

```json
{
  "status": "CommitStarted"
}
```

### <a name="response-body"></a>响应正文

| 值      | 在任务栏的搜索框中键入   | 描述                                                                                                                                                                                                                                                                         |
|------------|--------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| status           | string  | 提交的状态。 这可以是以下值之一： <ul><li>无</li><li>Canceled</li><li>PendingCommit</li><li>CommitStarted</li><li>CommitFailed</li><li>PendingPublication</li><li>Publishing</li><li>Published</li><li>PublishFailed</li><li>PreProcessing</li><li>PreProcessingFailed</li><li>认证</li><li>CertificationFailed</li><li>发行版本</li><li>ReleaseFailed</li></ul>  |

## <a name="error-codes"></a>错误代码

如果无法成功完成请求，该响应中会包含以下 HTTP 错误代码之一。

| 错误代码 |  描述   |
|--------|------------------|
| 400  | 请求参数无效。 |
| 404  | 找不到指定提交。 |
| 409  | 找到指定的提交，但在其当前状态下，可能尚未提交或应用程序使用的合作伙伴中心功能[目前不支持通过 Microsoft Store 提交 API](create-and-manage-submissions-using-windows-store-services.md#not_supported)。 |

## <a name="related-topics"></a>相关主题

* [创建和管理使用 Microsoft Store 服务的提交](create-and-manage-submissions-using-windows-store-services.md)
* [获取应用程序提交](get-an-app-submission.md)
* [创建应用程序提交](create-an-app-submission.md)
* [更新应用程序提交](update-an-app-submission.md)
* [删除应用程序提交](delete-an-app-submission.md)
* [获取应用程序提交的状态](get-status-for-an-app-submission.md)
