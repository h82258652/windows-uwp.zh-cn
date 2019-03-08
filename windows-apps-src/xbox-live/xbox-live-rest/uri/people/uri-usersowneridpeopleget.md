---
title: GET (/users/{ownerId}/people)
assetID: c948d031-ec19-7571-31ef-23cb9c5ebfaf
permalink: en-us/docs/xboxlive/rest/uri-usersowneridpeopleget.html
description: " GET (/users/{ownerId}/people)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 6c8672188a93b2e8d27a081ae068387e7ee7aa42
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2019
ms.locfileid: "57623762"
---
# <a name="get-usersowneridpeople"></a>GET (/users/{ownerId}/people)
获取调用方的用户集合。
这些 Uri 的域是`social.xboxlive.com`。

  * [备注](#ID4EV)
  * [URI 参数](#ID4E5)
  * [查询字符串参数](#ID4EJB)
  * [Authorization](#ID4ERD)
  * [所需的请求标头](#ID4EZE)
  * [可选的请求标头](#ID4EYF)
  * [请求正文](#ID4E5G)
  * [HTTP 状态代码](#ID4EJH)
  * [所需的响应标头](#ID4EBBAC)
  * [响应正文](#ID4ENCAC)

<a id="ID4EV"></a>


## <a name="remarks"></a>备注

GET 操作不会修改任何资源，因此如果执行一次或多次，这将产生相同的结果。

<a id="ID4E5"></a>


## <a name="uri-parameters"></a>URI 参数

| 参数| 在任务栏的搜索框中键入| 描述|
| --- | --- | --- |
| ownerId| 字符串| 其资源的访问的用户的标识符。 必须与匹配身份验证的用户。 可能的值为"me"、 xuid({xuid}) 或 gt({gamertag})。|

<a id="ID4EJB"></a>


## <a name="query-string-parameters"></a>查询字符串参数

| 参数| 在任务栏的搜索框中键入| 描述|
| --- | --- | --- | --- | --- | --- |
| 视图| 字符串| 返回与视图关联的人员。 默认值为"全部"。 可能的值为： <ul><li><b>所有</b>-返回有关用户的人员列表的所有人。 这是默认值。</li><li><b>最喜欢</b>-在用户的用户列表中的用户具有收藏夹属性返回的所有人员。</li><li><b>LegacyXboxLiveFriends</b> -返回有关用户的用户列表中的用户也是旧的 Xbox LIVE 好友的所有人员。</li></br>**注意：** 仅**所有**则不同于拥有用户调用用户时，支持值。|
| startIndex| 32 位无符号的整数| 返回从给定索引处开始的项。  
| maxItems| 32 位无符号的整数| 若要从开始的起始索引从集合中返回的人员的最大数目。 该服务可能会提供默认值，如果<b>maxItems</b>不存在，可能会返回少于<b>maxItems</b> （即使尚未返回结果的最后一页）。|

<a id="ID4ERD"></a>


## <a name="authorization"></a>授权

| 在任务栏的搜索框中键入| 必需| 描述| 如果缺少的响应|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| XUID| 是| 调用方具有用户的 Xbox 用户 ID (XUID)。| 401 未经授权|

<a id="ID4EZE"></a>


## <a name="required-request-headers"></a>所需的请求标头

| 标头| 描述|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 授权| 字符串。 用于 Xbox LIVE 的授权数据。 这通常是加密的 XSTS 令牌。 示例值：<b>XBL3.0 x=&lt;userhash>;&lt;token></b>.|

<a id="ID4EYF"></a>


## <a name="optional-request-headers"></a>可选的请求标头

| 标头| 描述|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| X-RequestedServiceVersion| 生成此请求应定向到 Xbox LIVE 的服务的名称/编号。 验证标头中的身份验证令牌等的声明的有效性后仅为将请求路由到该服务。默认值：1.|
| 接受| 字符串。 内容类型的调用方接受在响应中。 所有响应都都<b>应用程序 /json</b>。|

<a id="ID4E5G"></a>


## <a name="request-body"></a>请求正文

此请求的正文中不发送的任何对象。

<a id="ID4EJH"></a>


## <a name="http-status-codes"></a>HTTP 状态代码

服务将返回其中一个状态代码在本部分中使用此方法在此资源上发出的请求的响应中。 有关与 Xbox Live 服务一起使用的标准 HTTP 状态代码的完整列表，请参阅[标准 HTTP 状态代码](../../additional/httpstatuscodes.md)。

| 代码| 原因短语| 描述|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 200| 确定| 成功。|
| 400| 无效的请求| 查询参数或用户 Id 的格式不正确。|
| 403| 已禁止| 无法从授权标头分析 XUID 声明。|

<a id="ID4EBBAC"></a>


## <a name="required-response-headers"></a>所需的响应标头

| 标头| 在任务栏的搜索框中键入| 描述|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 内容长度| 32 位无符号的整数| 长度 （字节），响应正文中。 示例值：22.|
| 内容类型| 字符串| 响应正文的 MIME 类型。 此属性始终为<b>应用程序 /json</b>。|

<a id="ID4ENCAC"></a>


## <a name="response-body"></a>响应正文

如果调用成功，服务将返回调用方的用户集合和数组，其中包含调用方的用户集合中的用户总数。 请参阅[PeopleList (JSON)](../../json/json-peoplelist.md)。

<a id="ID4EZCAC"></a>


### <a name="sample-response"></a>示例响应


```cpp
{
    "people": [
        {
            "xuid": "2603643534573573",
            "isFavorite": true,
            "isFollowingCaller": false,
            "socialNetworks": ["LegacyXboxLive"]
        },
        {
            "xuid": "2603643534573572",
            "isFavorite": true,
            "isFollowingCaller": false,
            "socialNetworks": ["LegacyXboxLive"]
        },
        {
            "xuid": "2603643534573577",
            "isFollowingCaller": false,
            "isFavorite": false
        },
    ],
    "totalCount": 3
}

```


<a id="ID4EDDAC"></a>


## <a name="see-also"></a>另请参阅

<a id="ID4EFDAC"></a>


##### <a name="parent"></a>Parent 的子磁盘）

[/users/{ownerId}/people](uri-usersowneridpeople.md)
