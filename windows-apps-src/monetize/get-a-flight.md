---
author: Xansky
ms.assetid: 87708690-079A-443D-807E-D2BF9F614DDF
description: 在 Microsoft Store 提交 API 中使用此方法，可获取软件包外部测试，为注册到你的合作伙伴中心帐户的应用的数据。
title: 获取软件包外部测试版
ms.author: mhopkins
ms.date: 04/17/2018
ms.topic: article
keywords: windows 10, uwp, Microsoft Store 提交 API, 外部测试版, 软件包外部测试版
ms.localizationpriority: medium
ms.openlocfilehash: 09fd5c703e4a601ad28a05156aec9133444cfd9e
ms.sourcegitcommit: ed0304b8a214c03b8aab74b8ef12c9f82b8e3c5f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/19/2018
ms.locfileid: "7295270"
---
# <a name="get-a-package-flight"></a>获取软件包外部测试版

在 Microsoft Store 提交 API 中使用此方法，可获取软件包外部测试，为注册到你的合作伙伴中心帐户的应用的数据。

## <a name="prerequisites"></a>先决条件

若要使用此方法，首先需要执行以下操作：

* 如果尚未开始操作，请先完成 Microsoft Store 提交 API 的所有[先决条件](create-and-manage-submissions-using-windows-store-services.md#prerequisites)。
* [获取 Azure AD 访问令牌](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token)，以供在此方法的请求标头中使用。 获取访问令牌后，在它到期前，你有 60 分钟的使用时间。 该令牌到期后，可以获取新的令牌。

## <a name="request"></a>请求

此方法具有以下语法。 请参阅以下部分，获取标头和请求正文的使用示例和描述。

| 方法 | 请求 URI                                                      |
|--------|------------------------------------------------------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}``` |


### <a name="request-header"></a>请求头

| 标头        | 类型   | 说明                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| 授权 | 字符串 | 必需。 Azure AD 访问令牌的格式为 **Bearer** &lt;*token*&gt;。 |


### <a name="request-parameters"></a>请求参数

| 名称        | 类型   | 描述                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| applicationId | 字符串 | 必需。 应用（包含要获取的软件包外部测试版）的应用商店 ID。 在合作伙伴中心中会提供应用应用商店 ID。  |
| flightId | 字符串 | 必需。 要获取的软件包外部测试版的 ID。 在[创建软件包外部测试版](create-a-flight.md)和[获取应用的软件包外部测试版](get-flights-for-an-app.md)请求的响应数据中提供了此 ID。 对于在合作伙伴中心中创建的外部测试版，此 ID 也包含在合作伙伴中心中的外部测试版页面的 URL 中可用。  |


### <a name="request-body"></a>请求正文

请勿为此方法提供请求正文。

### <a name="request-example"></a>请求示例

以下示例演示如何检索有关应用商店 ID 值为 9WZDNCRD91MD 的应用的软件包外部测试版（ID 为 43e448df-97c9-4a43-a0bc-2a445e736bcd）的信息。

```
GET https://manage.devcenter.microsoft.com/v1.0/my/applications/9NBLGGH4R315/flights/43e448df-97c9-4a43-a0bc-2a445e736bcd HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>响应

以下示例演示了成功调用此方法的 JSON 响应正文。 有关响应正文中这些值的更多详细信息，请参阅以下部分。

```json
{
  "flightId": "43e448df-97c9-4a43-a0bc-2a445e736bcd",
  "friendlyName": "myflight",
  "lastPublishedFlightSubmission": {
    "id": "1152921504621086517",
    "resourceLocation": "flights/43e448df-97c9-4a43-a0bc-2a445e736bcd/submissions/1152921504621086517"
  },
  "pendingFlightSubmission": {
    "id": "115292150462124364",
    "resourceLocation": "flights/43e448df-97c9-4a43-a0bc-2a445e736bcd/submissions/1152921504621243647"
  },
  "groupIds": [
    "0"
  ],
  "rankHigherThan": "671c2857-725e-4faf-9e9e-ea1191ef879c"
}
```

### <a name="response-body"></a>响应正文

| 值      | 类型   | 描述                                                                                                                                                                                                                                                                         |
|------------|--------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| flightId            | 字符串  | 软件包外部测试版的 ID。 此值由合作伙伴中心提供。  |
| friendlyName           | 字符串  | 软件包外部测试版的名称，如开发人员所指定。   |  
| lastPublishedFlightSubmission       | 对象 | 提供有关软件包外部测试版的上次发布提交的信息的对象。 有关详细信息，请参阅下面的[提交对象](#submission_object)部分。  |
| pendingFlightSubmission        | 对象  |  提供有关软件包外部测试版的当前挂起提交的信息的对象。 有关详细信息，请参阅下面的[提交对象](#submission_object)部分。  |   
| groupIds           | 数组  | 包含与软件包外部测试版关联的外部测试版组 ID 的字符串数组。 有关外部测试版组的详细信息，请参阅[软件包外部测试版](https://msdn.microsoft.com/windows/uwp/publish/package-flights)。   |
| rankHigherThan           | 字符串  | 排名紧跟在当前软件包外部测试版之后的软件包外部测试版的友好名称。 有关排名的外部测试版组的详细信息，请参阅[软件包外部测试版](https://msdn.microsoft.com/windows/uwp/publish/package-flights)。  |


<span id="submission_object" />

### <a name="submission-object"></a>提交对象

响应正文中的 *lastPublishedFlightSubmission* 和 *pendingFlightSubmission* 值包含提供有关软件包外部测试版提交的资源信息的对象。 这些对象具有以下值。

| 值           | 类型    | 描述                                                                                                                                                                                                                          |
|-----------------|---------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| id            | 字符串  | 提交的 ID。    |
| resourceLocation   | 字符串  | 可追加到基本 ```https://manage.devcenter.microsoft.com/v1.0/my/``` 请求 URI 的相对路径，用于检索提交的完整数据。               |


## <a name="error-codes"></a>错误代码

如果无法成功完成请求，该响应中会包含以下 HTTP 错误代码之一。

| 错误代码 |  描述     |
|--------|---------------------  |
| 400  | 请求无效。 |
| 404  | 找不到指定的软件包外部测试版。   |   
| 409  | 应用使用[当前不受 Microsoft Store 提交 API](create-and-manage-submissions-using-windows-store-services.md#not_supported)的合作伙伴中心功能。 |                                                                                                 


## <a name="related-topics"></a>相关主题

* [使用 Microsoft Store 服务创建和管理提交](create-and-manage-submissions-using-windows-store-services.md)
* [创建软件包外部测试版](create-a-flight.md)
* [删除软件包外部测试版](delete-a-flight.md)
