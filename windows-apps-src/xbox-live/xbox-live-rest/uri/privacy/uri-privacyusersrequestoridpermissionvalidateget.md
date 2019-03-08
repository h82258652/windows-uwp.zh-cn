---
title: GET (/users/{requestorId}/permission/validate)
assetID: 8d22c668-af9a-1d24-8d65-830c2ce913d7
permalink: en-us/docs/xboxlive/rest/uri-privacyusersrequestoridpermissionvalidateget.html
description: " GET (/users/{requestorId}/permission/validate)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 07ccac63b3e6681ea35405314b0cd8b93aa60f9a
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2019
ms.locfileid: "57594752"
---
# <a name="get-usersrequestoridpermissionvalidate"></a>GET (/users/{requestorId}/permission/validate)
获取有关是否允许用户执行与目标用户指定的操作是或否答案。

  * [URI 参数](#ID4EQ)
  * [查询字符串参数](#ID4E2)
  * [Authorization](#ID4EDC)
  * [所需的请求标头](#ID4EID)
  * [请求正文](#ID4ETE)
  * [HTTP 状态代码](#ID4E5E)
  * [所需的响应标头](#ID4ETG)
  * [响应正文](#ID4EKAAC)

<a id="ID4EQ"></a>


## <a name="uri-parameters"></a>URI 参数

| 参数| 在任务栏的搜索框中键入| 描述|
| --- | --- | --- |
| requestorId| 字符串| 必需。 在执行操作的用户的标识符。 可能的值为<code>xuid({xuid})</code>和<code>me</code>。 这必须是已登录的用户。 示例值： <code>xuid(0987654321)</code>。|

<a id="ID4E2"></a>


## <a name="query-string-parameters"></a>查询字符串参数

| 参数| 在任务栏的搜索框中键入| 描述|
| --- | --- | --- | --- | --- | --- |
| 设置| 字符串枚举| 要检查所依据的 PermissionId 值。 示例值："CommunicateUsingText"。|
| target| 字符串| 该操作将在其执行的用户的标识符。 可能的值为<code>xuid({xuid})</code>。 示例值： <code>xuid(0987654321)</code>|

<a id="ID4EDC"></a>


## <a name="authorization"></a>授权

使用授权声明 | 声明| 在任务栏的搜索框中键入| 是否为必需？| 示例值|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| xuid| 64 位有符号的整数| 是| 1234567890|

<a id="ID4EID"></a>


## <a name="required-request-headers"></a>所需的请求标头

| 标头| 在任务栏的搜索框中键入| 描述|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 授权| 字符串| HTTP 身份验证的身份验证凭据。 示例值： <code>XBL3.0 x=&lt;userhash>;&lt;token></code>|
| X-RequestedServiceVersion| 字符串| 生成此请求应定向到 Xbox LIVE 的服务的名称/编号。 验证标头中的身份验证令牌等的声明的有效性后仅为将请求路由到该服务。示例值：1.|

<a id="ID4ETE"></a>


## <a name="request-body"></a>请求正文

此请求的正文中不发送的任何对象。

<a id="ID4E5E"></a>


## <a name="http-status-codes"></a>HTTP 状态代码

服务将返回其中一个状态代码在本部分中使用此方法在此资源上发出的请求的响应中。 有关与 Xbox Live 服务一起使用的标准 HTTP 状态代码的完整列表，请参阅[标准 HTTP 状态代码](../../additional/httpstatuscodes.md)。

| 代码| 原因短语| 描述|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 200| 确定| 已成功检索该会话。|
| 400| 请求无效。| 示例： 设置不正确 Id、 不正确的 Uri，等等。|
| 404| 在 URI 中指定的用户不存在。| 找不到指定的资源。|

<a id="ID4ETG"></a>


## <a name="required-response-headers"></a>所需的响应标头

| 标头| 在任务栏的搜索框中键入| 描述|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 内容类型| 字符串| 请求正文的 MIME 类型。 示例值： <code>application/json</code>|
| 内容长度| 字符串| 在响应中发送的字节数。 示例值：34|
| Cache-Control| 字符串| 正常请求从服务器以指定缓存行为。 示例： <code>no-cache, no-store</code>|

<a id="ID4EKAAC"></a>


## <a name="response-body"></a>响应正文

请参阅[PermissionCheckResponse (JSON)](../../json/json-permissioncheckresponse.md)。

<a id="ID4EWAAC"></a>


### <a name="sample-response"></a>示例响应


```cpp
{
    "isAllowed": false,
    "reasons":
    [
        {"reason": "BlockedByRequestor"},
        {"reason": "MissingPrivilege", "restrictedSetting": "VideoCommunications"}
    ]
}

```


<a id="ID4EABAC"></a>


## <a name="see-also"></a>另请参阅

<a id="ID4ECBAC"></a>


##### <a name="parent"></a>Parent 的子磁盘）

[/users/{requestorId}/permission/validate](uri-privacyusersrequestoridpermissionvalidate.md)

 [PermissionId 枚举](../../enums/privacy-enum-permissionid.md)
