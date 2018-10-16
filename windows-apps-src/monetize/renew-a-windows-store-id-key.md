---
author: Xansky
ms.assetid: 3569C505-8D8C-4D85-B383-4839F13B2466
description: 使用此方法续订 Microsoft Store 密钥。
title: 续订 Microsoft Store ID 密钥
ms.author: mhopkins
ms.date: 03/16/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, Microsoft Store 收集 API, Microsoft Store 购买 API, Microsoft Store ID 密钥, 续订
ms.localizationpriority: medium
ms.openlocfilehash: 70bda5022e52c0b18a43563a0492bd56d09b88a0
ms.sourcegitcommit: 9354909f9351b9635bee9bb2dc62db60d2d70107
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/16/2018
ms.locfileid: "4690187"
---
# <a name="renew-a-microsoft-store-id-key"></a>续订 Microsoft Store ID 密钥


使用此方法续订 Microsoft Store 密钥。 [生成 Microsoft Store ID 密钥时](view-and-grant-products-from-a-service.md#step-4)，该密钥在 90 天内有效。 在密钥过期后，你可以通过此方法使用已过期的密钥重新协商一个新密钥。

## <a name="prerequisites"></a>先决条件


若要使用此方法，你需要：

* 受众 URI 值为 `https://onestore.microsoft.com` 的 Azure AD 访问令牌。
* [从应用中的客户端代码生成](view-and-grant-products-from-a-service.md#step-4)的过期 Microsoft Store ID 密钥。

有关详细信息，请参阅[管理来自服务的产品授权](view-and-grant-products-from-a-service.md)。

## <a name="request"></a>请求

### <a name="request-syntax"></a>请求语法

| 密钥类型    | 方法 | 请求 URI                                              |
|-------------|--------|----------------------------------------------------------|
| 收集 | POST   | ```https://collections.mp.microsoft.com/v6.0/b2b/keys/renew``` |
| 购买    | POST   | ```https://purchase.mp.microsoft.com/v6.0/b2b/keys/renew```    |


### <a name="request-header"></a>请求标头

| 标头         | 类型   | 描述                                                                                           |
|----------------|--------|-------------------------------------------------------------------------------------------------------|
| Host           | 字符串 | 必须设置为值 **collections.mp.microsoft.com** 或 **purchase.mp.microsoft.com**。           |
| Content-Length | 数字 | 请求正文的长度。                                                                       |
| Content-Type   | 字符串 | 指定请求和响应类型。 当前，唯一受支持的值为 **application/json**。 |


### <a name="request-body"></a>请求正文

| 参数     | 类型   | 说明                       | 必需 |
|---------------|--------|-----------------------------------|----------|
| serviceTicket | 字符串 | Azure AD 访问令牌。        | 是      |
| 密钥           | 字符串 | 过期的 Microsoft Store ID 密钥。 | 是       |


### <a name="request-example"></a>请求示例

```syntax
POST https://collections.mp.microsoft.com/v6.0/b2b/keys/renew HTTP/1.1
Content-Length: 2774
Content-Type: application/json
Host: collections.mp.microsoft.com

{
    "serviceTicket": "eyJ0eXAiOiJKV1QiLCJhb….",
    "Key": "eyJ0eXAiOiJKV1QiLCJhbG…."
}
```

## <a name="response"></a>响应


### <a name="response-body"></a>响应正文

| 参数 | 类型   | 说明                                                                                                            |
|-----------|--------|------------------------------------------------------------------------------------------------------------------------|
| 键       | 字符串 | 已刷新的 Microsoft Store 密钥，可在将来调用 Microsoft Store 收集 API 或购买 API 时使用。 |


### <a name="response-example"></a>响应示例

```syntax
HTTP/1.1 200 OK
Content-Length: 1646
Content-Type: application/json
MS-CorrelationId: bfebe80c-ff89-4c4b-8897-67b45b916e47
MS-RequestId: 1b5fa630-d672-4971-b2c0-3713f4ea6c85
MS-CV: xu2HW6SrSkyfHyFh.0.0
MS-ServerId: 030011428
Date: Tue, 13 Sep 2015 07:31:12 GMT

{
    "key":"eyJ0eXAi….."
}
```

## <a name="error-codes"></a>错误代码


| 代码 | 错误        | 内部错误代码           | 描述   |
|------|--------------|----------------------------|---------------|
| 401  | 未授权 | AuthenticationTokenInvalid | Azure AD 访问令牌无效。 在某些情况下，ServiceError 的详细信息包含更多信息，例如令牌到期或 *appid* 声明丢失的时间。 |
| 401  | 未授权 | InconsistentClientId       | Microsoft Store ID 密钥中的 *clientId* 声明与 Azure AD 访问令牌中的 *appid* 声明不匹配。                                                                     |


## <a name="related-topics"></a>相关主题


* [管理来自服务的产品授权](view-and-grant-products-from-a-service.md)
* [查询产品](query-for-products.md)
* [将可消费产品报告为已完成](report-consumable-products-as-fulfilled.md)
* [授予免费产品](grant-free-products.md)
