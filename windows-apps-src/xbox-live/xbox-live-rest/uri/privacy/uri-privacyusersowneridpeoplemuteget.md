---
title: GET (/users/{ownerId}/people/mute)
assetID: 49b6c830-95f7-3200-0e46-0a1af573971c
permalink: en-us/docs/xboxlive/rest/uri-privacyusersowneridpeoplemuteget.html
author: KevinAsgari
description: " GET (/users/{ownerId}/people/mute)"
ms.author: kevinasg
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: fd9c5a1f95873028d38dfacea9d91393ab7dba12
ms.sourcegitcommit: 753e0a7160a88830d9908b446ef0907cc71c64e7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/30/2018
ms.locfileid: "5762854"
---
# <a name="get-usersowneridpeoplemute"></a>GET (/users/{ownerId}/people/mute)
获取用户的静音的列表。

  * [备注](#ID4EQ)
  * [URI 参数](#ID4EZ)
  * [资源的隐私设置的效果](#ID4EEB)
  * [授权](#ID4ENB)
  * [需的请求标头](#ID4ESC)
  * [请求正文](#ID4EPE)
  * [HTTP 状态代码](#ID4E1E)
  * [所需的响应标头](#ID4E3G)
  * [响应正文](#ID4ETAAC)

<a id="ID4EQ"></a>


## <a name="remarks"></a>备注

如果给定目标，则此 URI 将返回只允许该用户，如果用户是在静音的列表中，也可以为空，如果用户不是。

<a id="ID4EZ"></a>


## <a name="uri-parameters"></a>URI 参数

| 参数| 类型| 说明|
| --- | --- | --- |
| ownerId| 字符串| 必需。 要访问其资源的用户的标识符。 可能的值为"me" <code>xuid({xuid})</code>，或 gt({gamertag})。 必须经过身份验证的用户。 示例值： <code>xuid(2603643534573581)</code>， <code>gt(SomeGamertag)</code>。 最大大小： none。 |

<a id="ID4EEB"></a>


## <a name="effect-of-privacy-settings-on-resource"></a>资源的隐私设置的效果

无。

<a id="ID4ENB"></a>


## <a name="authorization"></a>授权

使用授权声明 | 声明| 类型| 是否必需？| 示例值|
| --- | --- | --- | --- | --- | --- | --- |
| Xuid| 64 位有符号整数| 是| 1234567890|

<a id="ID4ESC"></a>


## <a name="required-request-headers"></a>需的请求标头

| 标头| 类型| 说明|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 授权 | 字符串| HTTP 身份验证的身份验证凭据。 示例值： <code>Xauth=&lt;authtoken></code>。 最大大小： none。|
| X RequestedServiceVersion| 字符串| 名称/的内部版本号应指向此请求的 Xbox LIVE 的服务。 请求将仅可路由到的服务验证该标头，授权令牌中的声明的有效性后，依此类推。 示例值： <code>1</code>， <code>vnext</code>。 最大大小： none。|
| 接受| 字符串| 内容类型可接受。 示例值： <code>application/json</code>。 最大大小： none。|

<a id="ID4EPE"></a>


## <a name="request-body"></a>请求正文

此请求的正文中不发送任何对象。

<a id="ID4E1E"></a>


## <a name="http-status-codes"></a>HTTP 状态代码

此部分中使用此方法对此资源所做的请求的响应，该服务返回其中一个状态代码。 有关使用 Xbox Live 服务的标准 HTTP 状态代码的完整列表，请参阅[标准 HTTP 状态代码](../../additional/httpstatuscodes.md)。

| 代码| 原因短语| 说明|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 200| “确定”| 有关静音列表的成功请求。|
| 400| 错误请求| URI 中指定的目标 ID 不正确。|
| 403| 已禁止| URI 中指定的所有者不是经过身份验证的用户。|
| 404| 找不到| URI 中指定的所有者不存在。|

<a id="ID4E3G"></a>


## <a name="required-response-headers"></a>所需的响应标头

| 标头| 类型| 说明|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Content-Type| 字符串| 请求正文的 MIME 类型。 示例值： <code>application/json</code>|
| Content-Length| 字符串| 在响应中发送的字节数。 示例值： 34|
| 缓存控制| 字符串| 礼貌用语请求从服务器指定缓存行为。 示例： <code>no-cache, no-store</code>|

<a id="ID4ETAAC"></a>


## <a name="response-body"></a>响应正文

<a id="ID4EZAAC"></a>


### <a name="sample-response"></a>示例响应

查看[用户列表](../../json/json-userlist.md)。


```cpp
{
    "users":
    [
        { "xuid":"12345" },
        { "xuid":"23456" }
    ]
}

```


<a id="ID4EJBAC"></a>


## <a name="see-also"></a>另请参阅

<a id="ID4ELBAC"></a>


##### <a name="parent"></a>Parent 的子磁盘）

[/users/{ownerId}/people/mute](uri-privacyusersowneridpeoplemute.md)
