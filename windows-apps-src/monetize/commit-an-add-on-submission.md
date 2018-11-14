---
author: Xansky
ms.assetid: AC74B4FA-5554-4C03-9683-86EE48546C05
description: 在 Microsoft Store 提交 API 中使用此方法，来确认新的或更新的加载项提交到合作伙伴中心。
title: 确认加载项提交
ms.author: mhopkins
ms.date: 04/17/2018
ms.topic: article
keywords: windows 10, uwp, Microsoft Store 提交 API, 确认加载项提交, 应用内产品, IAP
ms.localizationpriority: medium
ms.openlocfilehash: 6f0d7ed15d3328ceffa9aa65a38c129f735c41ba
ms.sourcegitcommit: 38f06f1714334273d865935d9afb80efffe97a17
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/09/2018
ms.locfileid: "6203095"
---
# <a name="commit-an-add-on-submission"></a>确认加载项提交

在 Microsoft Store 提交 API 中使用此方法，来确认新的或更新的加载项 （也称为应用内产品或 IAP） 提交到合作伙伴中心。 确认操作警报合作伙伴中心，提交数据已上传 （包括任何相关的图标）。 作为响应，合作伙伴中心提交以供引入和发布的提交数据所做的更改。 确认操作成功后，在合作伙伴中心中显示提交更改。

有关确认操作如何适用通过使用 Microsoft Store 提交 API 提交加载项过程的详细信息，请参阅[管理加载项提交](manage-add-on-submissions.md)。

## <a name="prerequisites"></a>先决条件

若要使用此方法，首先需要执行以下操作：

* 如果尚未开始操作，请先完成 Microsoft Store 提交 API 的所有[先决条件](create-and-manage-submissions-using-windows-store-services.md#prerequisites)。
* [获取 Azure AD 访问令牌](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token)，以供在此方法的请求标头中使用。 获取访问令牌后，在它到期前，你有 60 分钟的使用时间。 该令牌到期后，可以获取新的令牌。
* [创建加载项提交](create-an-add-on-submission.md)，然后通过对提交数据所做的任何必要更改[更新提交](update-an-add-on-submission.md)。

## <a name="request"></a>请求

此方法具有以下语法。 请参阅以下部分，获取标头和请求正文的使用示例和描述。

| 方法 | 请求 URI                                                      |
|--------|------------------------------------------------------------------|
| POST    | ```https://manage.devcenter.microsoft.com/v1.0/my/inappproducts/{inAppProductId}/submissions/{submissionId}/commit``` |


### <a name="request-header"></a>请求标头

| 标头        | 类型   | 说明                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| 授权 | 字符串 | 必需。 Azure AD 访问令牌的格式为 **Bearer** &lt;*token*&gt;。 |


### <a name="request-parameters"></a>请求参数

| 名称        | 类型   | 描述                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| inAppProductId | 字符串 | 必需。 加载项（包含要确认的提交）的应用商店 ID。 在合作伙伴中心，会提供应用商店 ID，它包含在响应数据中[获取所有加载项](get-all-add-ons.md)和[创建加载项](create-an-add-on.md)的请求。 |
| submissionId | 字符串 | 必需。 要确认的提交的 ID。 此 ID 包含在[创建加载项提交](create-an-add-on-submission.md)请求的响应数据中。 对于在合作伙伴中心中创建的提交，此 ID 也包含在合作伙伴中心中的提交页面的 URL 中可用。  |


### <a name="request-body"></a>请求正文

请勿为此方法提供请求正文。

### <a name="request-example"></a>请求示例

以下示例演示了如何确认加载项提交。

```
POST https://manage.devcenter.microsoft.com/v1.0/my/inappproducts/9NBLGGH4TNMP/submissions/1152921504621230023/commit HTTP/1.1
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

| 值      | 类型   | 描述                                                                                                                                                                                                                                                                         |
|------------|--------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 状态           | 字符串  | 提交的状态。 这可以是以下值之一： <ul><li>None</li><li>Canceled</li><li>PendingCommit</li><li>CommitStarted</li><li>CommitFailed</li><li>PendingPublication</li><li>Publishing</li><li>Published</li><li>PublishFailed</li><li>PreProcessing</li><li>PreProcessingFailed</li><li>Certification</li><li>CertificationFailed</li><li>Release</li><li>ReleaseFailed</li></ul>  |


## <a name="error-codes"></a>错误代码

如果无法成功完成请求，该响应中会包含以下 HTTP 错误代码之一。

| 错误代码 |  描述   |
|--------|------------------|
| 400  | 请求参数无效。 |
| 404  | 找不到指定提交。 |
| 409  | 指定的提交已找到，但它可能无法确认; 在其当前状态，或者加载项使用[当前不受 Microsoft Store 提交 API](create-and-manage-submissions-using-windows-store-services.md#not_supported)的合作伙伴中心功能。 |


## <a name="related-topics"></a>相关主题

* [使用 Microsoft Store 服务创建和管理提交](create-and-manage-submissions-using-windows-store-services.md)
* [获取加载项提交](get-an-add-on-submission.md)
* [创建加载项提交](create-an-add-on-submission.md)
* [更新加载项提交](update-an-add-on-submission.md)
* [删除加载项提交](delete-an-add-on-submission.md)
* [获取加载项提交的状态](get-status-for-an-add-on-submission.md)
