---
title: GET (/users/{ownerId}/people/avoid)
assetID: e3420658-4738-8e80-44da-8281726fce01
permalink: en-us/docs/xboxlive/rest/uri-privacyusersxuidpeopleavoidget.html
author: KevinAsgari
description: " GET (/users/{ownerId}/people/avoid)"
ms.author: kevinasg
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 68237ed101870a8fed4b7b5fb298006f784a0910
ms.sourcegitcommit: f2c9a050a9137a473f28b613968d5782866142c6
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/10/2018
ms.locfileid: "6269923"
---
# <a name="get-usersowneridpeopleavoid"></a>GET (/users/{ownerId}/people/avoid)
获取用户避免列表。

  * [备注](#ID4EQ)
  * [URI 参数](#ID4EZ)
  * [授权](#ID4EEB)
  * [需的请求标头](#ID4EJC)
  * [HTTP 状态代码](#ID4EYD)
  * [所需的响应标头](#ID4E1F)
  * [响应正文](#ID4ESH)

<a id="ID4EQ"></a>


## <a name="remarks"></a>备注

如果给定目标，仅返回该用户，如果它们不在阻止列表中，也可以为空如果它们不。

<a id="ID4EZ"></a>


## <a name="uri-parameters"></a>URI 参数

| 参数| 类型| 说明|
| --- | --- | --- |
| ownerId| 字符串| 必需。 正在访问其资源的用户的标识符。 可能的值为<code>xuid({xuid})</code>。 必须经过身份验证的用户。 示例值： <code>xuid(2603643534573581)</code>。 最大大小： none。 |

<a id="ID4EEB"></a>


## <a name="authorization"></a>授权

使用授权声明 | 声明| 类型| 是否必需？| 示例值|
| --- | --- | --- | --- | --- | --- | --- |
| Xuid| 64 位有符号整数| 是| 1234567890|

<a id="ID4EJC"></a>


## <a name="required-request-headers"></a>需的请求标头

| 标头| 类型| 说明|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 授权 | 字符串| HTTP 身份验证的身份验证凭据。 示例值： <code>Xauth=&lt;authtoken></code>。 最大大小： none。|
| 接受| 字符串| 内容类型可接受。 示例值： <code>application/json</code>。 最大大小： none。|

<a id="ID4EYD"></a>


## <a name="http-status-codes"></a>HTTP 状态代码

此部分中使用此方法对此资源进行的请求的响应，该服务返回的状态代码之一。 有关使用 Xbox Live 服务的标准 HTTP 状态代码的完整列表，请参阅[标准 HTTP 状态代码](../../additional/httpstatuscodes.md)。

| 代码| 原因短语| 说明|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 200| “确定”| 已成功检索会话。|
| 400| 错误请求| URI 中指定的目标 ID 不正确。|
| 403| 已禁止| URI 中指定的所有者不是经过身份验证的用户。|
| 404| 找不到| URI 中指定的所有者不存在。|

<a id="ID4E1F"></a>


## <a name="required-response-headers"></a>所需的响应标头

| 标头| 类型| 说明|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Content-Type| 字符串| 请求正文的 MIME 类型。 示例值： <code>application/json</code>。 最大大小： none。|
| Content-Length| 字符串| 在响应中发送的字节数。 示例值： 34。 最大大小： none。|
| 缓存控制| 字符串| 礼貌用语请求从服务器指定缓存行为。 示例值： <code>application/json</code>。 最大大小： none。|

<a id="ID4ESH"></a>


## <a name="response-body"></a>响应正文

<a id="ID4EYH"></a>


### <a name="sample-response"></a>示例响应


```cpp
{
    "users":
    [
        { "xuid":"12345" },
        { "xuid":"23456" }
    ]
}

```


<a id="ID4EDAAC"></a>


## <a name="see-also"></a>另请参阅

<a id="ID4EFAAC"></a>


##### <a name="parent"></a>Parent 的子磁盘）

[/users/{ownerId}/people/avoid](uri-privacyusersxuidpeopleavoid.md)
