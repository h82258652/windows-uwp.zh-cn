---
author: Xansky
ms.assetid: 038903d6-efab-4da6-96b5-046c7431e6e7
description: 在 Microsoft Store 评价 API 中使用此方法，提交针对应用评价的回复。
title: 提交评价回复
ms.author: mhopkins
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, Microsoft Store 服务, Microsoft Store 评价 API, 加载项购置
ms.localizationpriority: medium
ms.openlocfilehash: 0fdfe811a90eae1e67ef7f626815be1ef78a4c61
ms.sourcegitcommit: 753e0a7160a88830d9908b446ef0907cc71c64e7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/29/2018
ms.locfileid: "5758753"
---
# <a name="submit-responses-to-reviews"></a>提交评价回复


在 Microsoft Store 评价 API 中使用此方法，以编程方式回复应用评价。 调用此方法时，必须指定要回复的评价 ID。 评价 ID 位于 Microsoft Store 分析 API 中的[获取应用评价](get-app-reviews.md)方法的回复数据中，以及[评价报告](../publish/reviews-report.md)的[脱机下载](../publish/download-analytic-reports.md)中。

当客户提交评价时，他们可以选择不接收针对其评价的回复。 如果你尝试回复客户已选择不接收回复的评价，此方法的回复正文将显示回复尝试未成功。 调用此方法前，你可以选择使用[获取应用评价的回复信息](get-response-info-for-app-reviews.md)方法确定是否可以回复给定评价。

> [!NOTE]
> 除了使用此方法以编程方式回复评价以外，还可以[使用 Windows 开发人员中心仪表板](../publish/respond-to-customer-reviews.md)回复评价。

## <a name="prerequisites"></a>先决条件

若要使用此方法，首先需要执行以下操作：

* 如果尚未开始操作，请先完成 Microsoft Store 评价 API 的所有[先决条件](respond-to-reviews-using-windows-store-services.md#prerequisites)。
* [获取 Azure AD 访问令牌](respond-to-reviews-using-windows-store-services.md#obtain-an-azure-ad-access-token)，以供在此方法的请求标头中使用。 获取访问令牌后，在它到期前，你有 60 分钟的使用时间。 该令牌到期后，可以获取新的令牌。
* 获取要回复的评价 ID。 评价 ID 位于 Microsoft Store 分析 API 中的[获取应用评价](get-app-reviews.md)方法的回复数据中，以及[评价报告](../publish/reviews-report.md)的[脱机下载](../publish/download-analytic-reports.md)中。

## <a name="request"></a>请求

### <a name="request-syntax"></a>请求语法

| 方法 | 请求 URI                                                      |
|--------|------------------------------------------------------------------|
| POST    | ```https://manage.devcenter.microsoft.com/v1.0/my/reviews/responses``` |


### <a name="request-header"></a>请求标头

| 标头        | 类型   | 说明                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| 授权 | 字符串 | 必需。 Azure AD 访问令牌的格式为 **Bearer** &lt;*token*&gt;。 |


### <a name="request-parameters"></a>请求参数

此方法没有请求参数。


### <a name="request-body"></a>请求正文

请求正文具有以下值。

| 值        | 类型   | 描述                                                                 |
|---------------|--------|-----------------------------------------|
| Responses | 数组 | 包含要提交的回复数据的对象数组。 有关每个对象中的数据的详细信息，请参阅以下表格。 |


*Responses* 数组中的每个对象包含以下值。

| 值        | 类型   | 说明           |  必需  |
|---------------|--------|-----------------------------|-----|
| ApplicationId | 字符串 |  要回复评价的应用的应用商店 ID。 应用商店 ID 位于开发人员中心仪表板的[应用标识页](../publish/view-app-identity-details.md)。 Store ID 示例：9WZDNCRFJ3Q8。   |  是  |
| ReviewId | 字符串 |  要回复的评价 ID（这是一个 GUID）。 评价 ID 位于 Microsoft Store 分析 API 中的[获取应用评价](get-app-reviews.md)方法的回复数据中，以及[评价报告](../publish/reviews-report.md)的[脱机下载](../publish/download-analytic-reports.md)中。   |  是  |
| ResponseText | 字符串 | 要提交的回复。 你的回复必须遵循[以下准则](../publish/respond-to-customer-reviews.md#guidelines-for-responses)。   |  是  |
| SupportEmail | 字符串 | 应用的支持电子邮件地址，客户可以用它来直接与你联系。 必须为有效的电子邮件地址。     |  是  |
| IsPublic | 布尔值 |  如果你指定**为 true**，你的回复将显示在你的应用的应用商店一览中，客户评价的正下方，并且将向所有客户可见。 如果你指定**为 false** ，且用户未选择退出接收电子邮件回复，你的回复将发送给客户通过电子邮件，并且它不会对你的应用的应用商店一览中的其他客户可见。 如果你指定**为 false** ，用户已选择不接收电子邮件回复，则将返回错误。   |  是  |


### <a name="request-example"></a>请求示例

以下示例演示了如何使用此方法提交多个评价的回复。

```json
POST https://manage.devcenter.microsoft.com/v1.0/my/reviews/responses HTTP/1.1
Authorization: Bearer <your access token>
Content-Type: application/json
{
  "Responses": [
    {
      "ApplicationId": "9WZDNCRFJ3Q8",
      "ReviewId": "6be543ff-1c9c-4534-aced-af8b4fbe0316",
      "ResponseText": "Thank you for pointing out this bug. I fixed it and published an update, you should have the fix soon",
      "SupportEmail": "support@contoso.com",
      "IsPublic": "true"
    },
    {
      "ApplicationId": "9NBLGGH1RP08",
      "ReviewId": "80c9671a-96c2-4278-bcbc-be0ce5a32a7c",
      "ResponseText": "Thank you for submitting your review. Can you tell more about what you were doing in the app when it froze? Thanks very much for your help.",
      "SupportEmail": "support@contoso.com",
      "IsPublic": "false"
    }
  ]
}
```

## <a name="response"></a>回复

### <a name="response-body"></a>响应正文

| 值        | 类型   | 描述            |
|---------------|--------|---------------------|
| Result | 数组 | 包含每个已提交回复的相关数据的对象数组。 有关每个对象中的数据的详细信息，请参阅以下表格。  |


*Result* 数组中的每个对象包含以下值。

| 值        | 类型   | 描述                                                                 |
|---------------|--------|-----------------------------------------------|
| ApplicationId | 字符串 |  已回复评价的应用的应用商店 ID。 应用商店 ID 的一个示例是 9WZDNCRFJ3Q8。   |
| ReviewId | 字符串 |  已回复评价的 ID。 这是一个 GUID。   |
| Successful | 字符串 | 值 **true** 表示你的回复已发送成功。 值 **false** 表示你的回复失败。    |
| FailureReason | 字符串 | 如果 **Successful** 为 **false**，此值包含失败的原因。 如果 **Successful** 为 **true**，此值为空。      |


### <a name="response-example"></a>回复示例

以下示例举例说明此请求的 JSON 响应正文。

```json
{
  "Result": [
    {
      "ApplicationId": "9WZDNCRFJ3Q8",
      "ReviewId": "6be543ff-1c9c-4534-aced-af8b4fbe0316",
      "Successful": "true",
      "FailureReason": ""
    },
    {
      "ApplicationId": "9NBLGGH1RP08",
      "ReviewId": "80c9671a-96c2-4278-bcbc-be0ce5a32a7c",
      "Successful": "false",
      "FailureReason": "No Permission"
    }
  ]
}
```

## <a name="related-topics"></a>相关主题

* [使用开发人员中心仪表板回复客户评价](../publish/respond-to-customer-reviews.md)
* [使用 Microsoft Store 服务回复评价](respond-to-reviews-using-windows-store-services.md)
* [获取应用评价的回复信息](get-response-info-for-app-reviews.md)
* [获取应用评价](get-app-reviews.md)
