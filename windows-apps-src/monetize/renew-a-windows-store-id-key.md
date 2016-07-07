---
author: mcleanbyron
ms.assetid: 3569C505-8D8C-4D85-B383-4839F13B2466
description: "使用此方法续订 Windows 应用商店密钥。"
title: "续订 Windows 应用商店 ID 密钥"
ms.sourcegitcommit: 2f4351d6f9bdc0b9a131ad5ead10ffba7e76c437
ms.openlocfilehash: 6255346c568ed24e17c795834ab182f73707c4de

---

# 续订 Windows 应用商店 ID 密钥


\[ 已针对 Windows 10 上的 UWP 应用更新。 有关 Windows 8.x 文章，请参阅[存档](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

使用此方法续订 Windows 应用商店密钥。 在你通过调用 [**GetCustomerCollectionsIdAsync**](https://msdn.microsoft.com/library/windows/apps/mt608674) 或 [**GetCustomerPurchaseIdAsync**](https://msdn.microsoft.com/library/windows/apps/mt608675) 方法生成 Windows 应用商店 ID 密钥时，该密钥在 90 天内有效。 在密钥过期后，你可以通过此方法使用已过期的密钥重新协商一个新密钥。

## 先决条件


若要使用此方法，你需要：

-   使用 `https://onestore.microsoft.com` 受众 URI 创建的 Azure AD 访问令牌。
-   通过在应用中从客户端代码调用 [**GetCustomerCollectionsIdAsync**](https://msdn.microsoft.com/library/windows/apps/mt608674) 或 [**GetCustomerPurchaseIdAsync**](https://msdn.microsoft.com/library/windows/apps/mt608675) 方法所生成的已过期 Windows 应用商店 ID 密钥。

有关详细信息，请参阅[从服务查看和授予产品](view-and-grant-products-from-a-service.md)。

## 请求


### 请求语法

| 密钥类型    | 方法 | 请求 URI                                              |
|-------------|--------|----------------------------------------------------------|
| 收集 | POST   | `https://collections.mp.microsoft.com/v6.0/b2b/keys/renew` |
| 购买    | POST   | `https://purchase.mp.microsoft.com/v6.0/b2b/keys/renew`    |

<br/> 

### 请求标头

| 标头         | 类型   | 描述                                                                                           |
|----------------|--------|-------------------------------------------------------------------------------------------------------|
| Host           | 字符串 | 必须设置为值 **collections.mp.microsoft.com** 或 **purchase.mp.microsoft.com**。           |
| Content-Length | 数字 | 请求正文的长度。                                                                       |
| Content-Type   | 字符串 | 指定请求和响应类型。 当前，唯一受支持的值为 **application/json**。 |

<br/> 

### 请求正文

| 参数     | 类型   | 说明                       | 必需 |
|---------------|--------|-----------------------------------|----------|
| serviceTicket | 字符串 | Azure AD 访问令牌。        | 是      |
| 密钥           | 字符串 | 过期的 Windows 应用商店 ID 密钥。 | 否       |

<br/> 

### 请求示例

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

## 响应


### 响应正文

| 参数 | 类型   | 说明                                                                                                            | 必需 |
|-----------|--------|------------------------------------------------------------------------------------------------------------------------|----------|
| 密钥       | 字符串 | 已刷新的 Windows 应用商店密钥，可在对 Windows 应用商店收集 API 或购买 API 的将来调用中使用。 | 否       |

<br/> 

### 响应示例

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

## 错误代码


| 代码 | 错误        | 内部错误代码           | 描述                                                                                                                                                                           |
|------|--------------|----------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 401  | 未授权 | AuthenticationTokenInvalid | Azure AD 访问令牌无效。 在某些情况下，ServiceError 的详细信息包含更多信息，例如令牌到期或 *appid* 声明丢失的时间。 |
| 401  | 未授权 | InconsistentClientId       | Windows 应用商店 ID 密钥中的 *clientId* 声明与 Azure AD 访问令牌中的 *appid* 声明不匹配。                                                                     |

<br/> 

## 相关主题


* [从服务查看和授予产品](view-and-grant-products-from-a-service.md)
* [查询产品](query-for-products.md)
* [将可消费产品报告为已完成](report-consumable-products-as-fulfilled.md)
* [授予免费产品](grant-free-products.md)



<!--HONumber=Jun16_HO5-->


