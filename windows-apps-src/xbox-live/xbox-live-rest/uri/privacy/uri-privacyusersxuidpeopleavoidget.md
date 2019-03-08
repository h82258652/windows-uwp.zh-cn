---
title: GET (/users/{ownerId}/people/avoid)
assetID: e3420658-4738-8e80-44da-8281726fce01
permalink: en-us/docs/xboxlive/rest/uri-privacyusersxuidpeopleavoidget.html
description: " GET (/users/{ownerId}/people/avoid)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 745893b4b975b5fbf64fe76591ec15d18af59d73
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2019
ms.locfileid: "57641612"
---
# <a name="get-usersowneridpeopleavoid"></a>GET (/users/{ownerId}/people/avoid)
获取用户避免列表。

  * [备注](#ID4EQ)
  * [URI 参数](#ID4EZ)
  * [Authorization](#ID4EEB)
  * [所需的请求标头](#ID4EJC)
  * [HTTP 状态代码](#ID4EYD)
  * [所需的响应标头](#ID4E1F)
  * [响应正文](#ID4ESH)

<a id="ID4EQ"></a>


## <a name="remarks"></a>备注

如果未指定目标，只返回该用户，如果它们的块列表或者空它们是否不。

<a id="ID4EZ"></a>


## <a name="uri-parameters"></a>URI 参数

| 参数| 在任务栏的搜索框中键入| 描述|
| --- | --- | --- |
| ownerId| 字符串| 必需。 其资源的访问的用户的标识符。 可能的值为<code>xuid({xuid})</code>。 必须是经过身份验证的用户。 示例值： <code>xuid(2603643534573581)</code>。 最大大小： 无。 |

<a id="ID4EEB"></a>


## <a name="authorization"></a>授权

使用授权声明 | 声明| 在任务栏的搜索框中键入| 是否为必需？| 示例值|
| --- | --- | --- | --- | --- | --- | --- |
| xuid| 64 位有符号的整数| 是| 1234567890|

<a id="ID4EJC"></a>


## <a name="required-request-headers"></a>所需的请求标头

| 标头| 在任务栏的搜索框中键入| 描述|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 授权 | 字符串| HTTP 身份验证的身份验证凭据。 示例值： <code>Xauth=&lt;authtoken></code>。 最大大小： 无。|
| 接受| 字符串| 内容类型可接受。 示例值： <code>application/json</code>。 最大大小： 无。|

<a id="ID4EYD"></a>


## <a name="http-status-codes"></a>HTTP 状态代码

服务将返回其中一个状态代码在本部分中使用此方法在此资源上发出的请求的响应中。 有关与 Xbox Live 服务一起使用的标准 HTTP 状态代码的完整列表，请参阅[标准 HTTP 状态代码](../../additional/httpstatuscodes.md)。

| 代码| 原因短语| 描述|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 200| 确定| 已成功检索该会话。|
| 400| 无效的请求| 在 URI 中指定的目标 ID 无效。|
| 403| 已禁止| 在 URI 中指定的所有者不是经过身份验证的用户。|
| 404| 未找到| 在 URI 中指定的所有者不存在。|

<a id="ID4E1F"></a>


## <a name="required-response-headers"></a>所需的响应标头

| 标头| 在任务栏的搜索框中键入| 描述|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 内容类型| 字符串| 请求正文的 MIME 类型。 示例值： <code>application/json</code>。 最大大小： 无。|
| 内容长度| 字符串| 在响应中发送的字节数。 示例值：34. 最大大小： 无。|
| Cache-Control| 字符串| 正常请求从服务器以指定缓存行为。 示例值： <code>application/json</code>。 最大大小： 无。|

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
