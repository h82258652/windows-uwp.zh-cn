---
title: GET (/users/{ownerId}/people/mute)
assetID: 49b6c830-95f7-3200-0e46-0a1af573971c
permalink: en-us/docs/xboxlive/rest/uri-privacyusersowneridpeoplemuteget.html
description: " GET (/users/{ownerId}/people/mute)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 94e2bf4d04619ffa3348ae08fc37964cdc58e7b5
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2019
ms.locfileid: "57661582"
---
# <a name="get-usersowneridpeoplemute"></a>GET (/users/{ownerId}/people/mute)
获取用户静音的列表。

  * [备注](#ID4EQ)
  * [URI 参数](#ID4EZ)
  * [资源上的隐私设置的效果](#ID4EEB)
  * [Authorization](#ID4ENB)
  * [所需的请求标头](#ID4ESC)
  * [请求正文](#ID4EPE)
  * [HTTP 状态代码](#ID4E1E)
  * [所需的响应标头](#ID4E3G)
  * [响应正文](#ID4ETAAC)

<a id="ID4EQ"></a>


## <a name="remarks"></a>备注

如果未指定目标，此 URI 将返回只允许该用户，如果用户是静音的列表中，或者为空，如果用户不是。

<a id="ID4EZ"></a>


## <a name="uri-parameters"></a>URI 参数

| 参数| 在任务栏的搜索框中键入| 描述|
| --- | --- | --- |
| ownerId| 字符串| 必需。 其资源的访问的用户的标识符。 可能的值为"me" <code>xuid({xuid})</code>，或 gt({gamertag})。 必须是经过身份验证的用户。 示例值： <code>xuid(2603643534573581)</code>， <code>gt(SomeGamertag)</code>。 最大大小： 无。 |

<a id="ID4EEB"></a>


## <a name="effect-of-privacy-settings-on-resource"></a>资源上的隐私设置的效果

无。

<a id="ID4ENB"></a>


## <a name="authorization"></a>授权

使用授权声明 | 声明| 在任务栏的搜索框中键入| 是否为必需？| 示例值|
| --- | --- | --- | --- | --- | --- | --- |
| xuid| 64 位有符号的整数| 是| 1234567890|

<a id="ID4ESC"></a>


## <a name="required-request-headers"></a>所需的请求标头

| 标头| 在任务栏的搜索框中键入| 描述|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 授权 | 字符串| HTTP 身份验证的身份验证凭据。 示例值： <code>Xauth=&lt;authtoken></code>。 最大大小： 无。|
| X-RequestedServiceVersion| 字符串| 生成此请求应定向到 Xbox LIVE 的服务的名称/编号。 请求将只路由到的服务验证标头中的授权令牌，声明的有效性后，依次类推。 示例值： <code>1</code>， <code>vnext</code>。 最大大小： 无。|
| 接受| 字符串| 内容类型可接受。 示例值： <code>application/json</code>。 最大大小： 无。|

<a id="ID4EPE"></a>


## <a name="request-body"></a>请求正文

此请求的正文中不发送的任何对象。

<a id="ID4E1E"></a>


## <a name="http-status-codes"></a>HTTP 状态代码

服务将返回其中一个状态代码在本部分中使用此方法在此资源上发出的请求的响应中。 有关与 Xbox Live 服务一起使用的标准 HTTP 状态代码的完整列表，请参阅[标准 HTTP 状态代码](../../additional/httpstatuscodes.md)。

| 代码| 原因短语| 描述|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 200| 确定| 有关静音列表的成功请求。|
| 400| 无效的请求| 在 URI 中指定的目标 ID 无效。|
| 403| 已禁止| 在 URI 中指定的所有者不是经过身份验证的用户。|
| 404| 未找到| 在 URI 中指定的所有者不存在。|

<a id="ID4E3G"></a>


## <a name="required-response-headers"></a>所需的响应标头

| 标头| 在任务栏的搜索框中键入| 描述|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 内容类型| 字符串| 请求正文的 MIME 类型。 示例值： <code>application/json</code>|
| 内容长度| 字符串| 在响应中发送的字节数。 示例值：34|
| Cache-Control| 字符串| 正常请求从服务器以指定缓存行为。 示例： <code>no-cache, no-store</code>|

<a id="ID4ETAAC"></a>


## <a name="response-body"></a>响应正文

<a id="ID4EZAAC"></a>


### <a name="sample-response"></a>示例响应

请参阅[UserList](../../json/json-userlist.md)。


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
