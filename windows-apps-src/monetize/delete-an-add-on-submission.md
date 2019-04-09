---
ms.assetid: D677E126-C3D6-46B6-87A5-6237EBEDF1A9
description: 在 Microsoft Store 提交 API 中使用此方法，可删除现有加载项提交。
title: 删除加载项提交
ms.date: 04/17/2018
ms.topic: article
keywords: windows 10, uwp, Microsoft Store 提交 API, 加载项提交, 删除, 应用内产品, IAP
ms.localizationpriority: medium
ms.openlocfilehash: 4a694846c745fbfbc175781dd76cc983e402b676
ms.sourcegitcommit: 6a7dd4da2fc31ced7d1cdc6f7cf79c2e55dc5833
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/21/2019
ms.locfileid: "58334915"
---
# <a name="delete-an-add-on-submission"></a>删除加载项提交

在 Microsoft Store 提交 API 中使用此方法，可删除现有加载项（也称为应用内产品或 IAP）提交。

## <a name="prerequisites"></a>系统必备

若要使用此方法，首先需要执行以下操作：

* 如果尚未开始操作，请先完成 Microsoft Store 提交 API 的所有[先决条件](create-and-manage-submissions-using-windows-store-services.md#prerequisites)。
* [获取 Azure AD 访问令牌](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token)，以供在此方法的请求标头中使用。 获取访问令牌后，在它到期前，你有 60 分钟的使用时间。 该令牌到期后，可以获取新的令牌。

## <a name="request"></a>请求

此方法具有以下语法。 请参阅以下部分，获取标头和请求正文的使用示例和描述。

| 方法 | 请求 URI                                                      |
|--------|------------------------------------------------------------------|
| DELETE    | `https://manage.devcenter.microsoft.com/v1.0/my/inappproducts/{inAppProductId}/submissions/{submissionId}` |


### <a name="request-header"></a>请求头

| Header        | 在任务栏的搜索框中键入   | 描述                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| 授权 | string | 必需。 Azure AD 访问令牌的格式为 **Bearer** *token*&lt;&gt;。 |


### <a name="request-parameters"></a>请求参数

| 名称        | 在任务栏的搜索框中键入   | 描述                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| inAppProductId | string | 必需。 加载项（包含要删除的提交）的应用商店 ID。 合作伙伴中心中提供了 Store ID。  |
| submissionId | string | 必需。 要删除的提交的 ID。 此 ID 包含在[创建加载项提交](create-an-add-on-submission.md)请求的响应数据中。 在合作伙伴中心创建的提交，此 ID 是也可用在合作伙伴中心中的提交页的 URL。  |


### <a name="request-body"></a>请求正文

请勿为此方法提供请求正文。


### <a name="request-example"></a>请求示例

以下示例演示了如何删除加载项提交。

```json
DELETE https://manage.devcenter.microsoft.com/v1.0/my/inappproducts/9NBLGGH4TNMP/submissions/1152921504621230023 HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>响应

如果成功，此方法会返回空的响应正文。

## <a name="error-codes"></a>错误代码

如果无法成功完成请求，该响应中会包含以下 HTTP 错误代码之一。

| 错误代码 |  描述   |
|--------|------------------|
| 400  | 请求参数无效。 |
| 404  | 找不到指定提交。 |
| 409  | 找到指定的提交，但无法在其当前状态下，删除或外接程序使用的合作伙伴中心功能[目前不支持通过 Microsoft Store 提交 API](create-and-manage-submissions-using-windows-store-services.md#not_supported)。 |


## <a name="related-topics"></a>相关主题

* [创建和管理使用 Microsoft Store 服务的提交](create-and-manage-submissions-using-windows-store-services.md)
* [获取外接程序提交](get-an-add-on-submission.md)
* [创建外接程序提交](create-an-add-on-submission.md)
* [提交外接程序提交](commit-an-add-on-submission.md)
* [更新外接程序提交](update-an-add-on-submission.md)
* [获取外接程序提交的状态](get-status-for-an-add-on-submission.md)
