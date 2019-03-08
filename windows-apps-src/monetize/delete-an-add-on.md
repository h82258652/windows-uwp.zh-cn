---
ms.assetid: 16D4C3B9-FC9B-46ED-9F87-1517E1B549FA
description: 在 Microsoft Store 提交 API 中使用此方法，若要删除到合作伙伴中心帐户注册的应用外接程序。
title: 删除加载项
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, Microsoft Store 提交 API, 加载项, 删除, 应用内产品, IAP
ms.localizationpriority: medium
ms.openlocfilehash: 837cbc19268a88be986068f4a5e60002a1eb55e2
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2019
ms.locfileid: "57657612"
---
# <a name="delete-an-add-on"></a>删除加载项

在 Microsoft Store 提交 API 中使用此方法，若要删除的外接程序 （也称为应用产品或、 IAP） 的应用程序注册到你的合作伙伴中心帐户。

## <a name="prerequisites"></a>必备条件

若要使用此方法，首先需要执行以下操作：

* 如果尚未开始操作，请先完成 Microsoft Store 提交 API 的所有[先决条件](create-and-manage-submissions-using-windows-store-services.md#prerequisites)。
* [获取 Azure AD 访问令牌](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token)，以供在此方法的请求标头中使用。 获取访问令牌后，在它到期前，你有 60 分钟的使用时间。 该令牌到期后，可以获取新的令牌。

## <a name="request"></a>请求

此方法具有以下语法。 请参阅以下部分，获取标头和请求正文的使用示例和描述。

| 方法 | 请求 URI                                                      |
|--------|------------------------------------------------------------------|
| DELETE    | ```https://manage.devcenter.microsoft.com/v1.0/my/inappproducts/{inAppProductId}``` |


### <a name="request-header"></a>请求头

| 标头        | 在任务栏的搜索框中键入   | 描述                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| 授权 | 字符串 | 必需。 Azure AD 访问令牌的格式为 **Bearer** *token*&lt;&gt;。 |


### <a name="request-parameters"></a>请求参数

| 名称        | 在任务栏的搜索框中键入   | 描述                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| id | 字符串 | 必需。 要删除的加载项的应用商店 ID。 合作伙伴中心中提供了 Store ID。  |


### <a name="request-body"></a>请求正文

请勿为此方法提供请求正文。


### <a name="request-example"></a>请求示例

以下示例演示了如何删除加载项。

```
DELETE https://manage.devcenter.microsoft.com/v1.0/my/inappproducts/9NBLGGH4TNMP HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>响应

如果成功，此方法会返回空的响应正文。

## <a name="error-codes"></a>错误代码

如果无法成功完成请求，该响应中会包含以下 HTTP 错误代码之一。

| 错误代码 |  描述                                                                                                                                                                           |
|--------|------------------|
| 400  | 请求无效。 |
| 404  | 找不到指定的加载项。  |
| 409  | 找到指定的加载项，但无法在其当前状态下，删除或外接程序使用的合作伙伴中心功能[目前不支持通过 Microsoft Store 提交 API](create-and-manage-submissions-using-windows-store-services.md#not_supported)。 |   


## <a name="related-topics"></a>相关主题

* [创建和管理使用 Microsoft Store 服务的提交](create-and-manage-submissions-using-windows-store-services.md)
* [获取所有外接程序](get-all-add-ons.md)
* [获取外接程序](get-an-add-on.md)
* [创建外接程序](create-an-add-on.md)
