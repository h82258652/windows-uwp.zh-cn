---
description: 使用 Microsoft Store 提交 API 中的此方法终止应用提交的软件包推出。
title: 终止应用提交的推出
ms.date: 04/17/2018
ms.topic: article
keywords: windows 10, uwp, Microsoft Store 提交 API, 软件包推出, 应用提交, 终止
ms.assetid: 4ce79fe3-deda-4d31-b938-d672c3869051
ms.localizationpriority: medium
ms.openlocfilehash: 08450b7aa9608e610a31d114059dd49e3ef3e10c
ms.sourcegitcommit: 8921a9cc0dd3e5665345ae8eca7ab7aeb83ccc6f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/11/2018
ms.locfileid: "8888339"
---
# <a name="halt-the-rollout-for-an-app-submission"></a>终止应用提交的推出


使用 Microsoft Store 提交 API 中的此方法为应用提交[终止软件包推出](../publish/gradual-package-rollout.md#completing-the-rollout)。 有关通过使用 Microsoft Store 提交 API 创建应用提交过程的详细信息，请参阅[管理应用提交](manage-app-submissions.md)。

> [!NOTE]
> 如果终止应用提交的推出，然后[创建新的应用提交](create-an-app-submission.md)，则新提交将是已终止提交的克隆。


## <a name="prerequisites"></a>必备条件

若要使用此方法，首先需要执行以下操作：

* 如果尚未开始操作，请先完成 Microsoft Store 提交 API 的所有[先决条件](create-and-manage-submissions-using-windows-store-services.md#prerequisites)。
* [获取 Azure AD 访问令牌](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token)，以供在此方法的请求标头中使用。 获取访问令牌后，在它到期前，你有 60 分钟的使用时间。 该令牌到期后，可以获取新的令牌。
* 创建一个应用提交。 你可以执行此操作在合作伙伴中心，或者你可以执行此操作通过使用[创建应用提交](create-an-app-submission.md)的方法。
* 启用提交的逐步软件包推出。 你可以执行此[合作伙伴中心中](../publish/gradual-package-rollout.md)，或者你可以执行此操作通过[使用 Microsoft Store 提交 API](manage-app-submissions.md#manage-gradual-package-rollout)。

## <a name="request"></a>请求

此方法具有以下语法。 请参阅以下部分，获取标头和请求参数的使用示例和描述。

| 方法 | 请求 URI                                                      |
|--------|------------------------------------------------------------------|
| POST   | ```https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/submissions/{submissionId}/haltpackagerollout``` |


### <a name="request-header"></a>请求标头

| 标头        | 类型   | 描述                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| 授权 | 字符串 | 必需。 Azure AD 访问令牌的格式为 **Bearer** &lt;*token*&gt;。 |


### <a name="request-parameters"></a>请求参数

| 名称        | 类型   | 描述                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| applicationId | 字符串 | 必需。 应用（包含要终止软件包推出的提交）的应用商店 ID。 有关应用商店 ID 的详细信息，请参阅[查看应用标识详细信息](https://msdn.microsoft.com/windows/uwp/publish/view-app-identity-details)。  |
| submissionId | 字符串 | 必需。 要终止软件包推出的提交的 ID。 此 ID 包含在[创建应用提交](create-an-app-submission.md)请求的响应数据中。 对于在合作伙伴中心中创建的提交，此 ID 也包含在合作伙伴中心中的提交页面的 URL 中可用。  |


### <a name="request-body"></a>请求正文

请勿为此方法提供请求正文。

### <a name="request-example"></a>请求示例

以下示例演示了如何终止应用提交的软件包推出。

```
POST https://manage.devcenter.microsoft.com/v1.0/my/applications/9NBLGGH4R315/submissions/1152921504621243680/haltpackagerollout HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>响应

以下示例演示了成功调用此方法的 JSON 响应正文。 有关响应正文中这些值的更多详细信息，请参阅[软件包推出资源](manage-app-submissions.md#package-rollout-object)。

```json
{
    "isPackageRollout": true,
    "packageRolloutPercentage": 0.0,
    "packageRolloutStatus": "PackageRolloutStopped",
    "fallbackSubmissionId": "1212922684621243058"
}
```

## <a name="error-codes"></a>错误代码

如果无法成功完成请求，该响应中会包含以下 HTTP 错误代码之一。

| 错误代码 |  描述   |
|--------|------------------|
| 404  | 找不到提交。 |
| 409  | 此代码指示以下错误之一：<br/><br/><ul><li>对于逐步推出操作，该提交未处于有效状态（在调用此方法之前，必须发布该提交，并且必须将 [packageRolloutStatus](manage-app-submissions.md#package-rollout-object) 值设置为 **PackageRolloutInProgress**）。</li><li>提交不属于指定应用。</li><li>应用使用[当前不受 Microsoft Store 提交 API](create-and-manage-submissions-using-windows-store-services.md#not_supported)的合作伙伴中心功能。</li></ul> |   


## <a name="related-topics"></a>相关主题

* [逐步程序包推出](../publish/gradual-package-rollout.md)
* [使用 Microsoft Store 提交 API 管理应用提交](manage-app-submissions.md)
* [使用 Microsoft Store 服务创建和管理提交](create-and-manage-submissions-using-windows-store-services.md)
