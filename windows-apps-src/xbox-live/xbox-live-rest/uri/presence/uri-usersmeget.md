---
title: GET (/users/me)
assetID: 726c279b-73fb-02ea-cbff-700ff2dc31af
permalink: en-us/docs/xboxlive/rest/uri-usersmeget.html
description: " GET (/users/me)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: b06305fde989d0c30570beda5d4b0aabe7bf0518
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2019
ms.locfileid: "57606722"
---
# <a name="get-usersme"></a>GET (/users/me)
获取当前用户的[PresenceRecord](../../json/json-presencerecord.md)而无需知道用户的 XUID。
这些 Uri 的域是`userpresence.xboxlive.com`。

  * [查询字符串参数](#ID4EZ)
  * [Authorization](#ID4EIC)
  * [所需的请求标头](#ID4ELD)
  * [可选的请求标头](#ID4EPF)
  * [请求正文](#ID4EPG)
  * [响应正文](#ID4E1G)

<a id="ID4EZ"></a>


## <a name="query-string-parameters"></a>查询字符串参数

| 参数| 在任务栏的搜索框中键入| 描述|
| --- | --- | --- |
| level| 字符串| 可选。 <ul><li><b>用户</b>:仅返回用户节点。</li><li><b>设备</b>:返回用户节点和设备节点。</li><li><b>标题</b>:默认值。 返回除活动的整个树。</li><li><b>所有</b>:返回整个树，包括活动级别存在。</li></ul> | 

<a id="ID4EIC"></a>


## <a name="authorization"></a>授权

| 在任务栏的搜索框中键入| 必需| 描述| 如果缺少的响应|
| --- | --- | --- | --- | --- | --- | --- |
| XUID| 是| 调用方的 Xbox 用户 ID (XUID)| 403 禁止访问|

<a id="ID4ELD"></a>


## <a name="required-request-headers"></a>所需的请求标头

| 标头| 在任务栏的搜索框中键入| 描述|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 授权| 字符串| HTTP 身份验证的身份验证凭据。 示例值："XBL3.0 x =&lt;userhash >;&lt;令牌 >"。|
| x-xbl-contract-version| 字符串| 生成此请求应定向到 Xbox LIVE 的服务的名称/编号。 请求将只路由到的服务验证标头身份验证令牌中声明的有效性后，依次类推。 示例值：3，vnext。|
| 接受| 字符串| 内容类型可接受。 只有一个支持的状态显示为 application/json，但必须在标头中指定它。|
| Accept-Language| 字符串| 在响应中的字符串的可接受区域设置。 示例值： EN-US。|
| 主机| 字符串| 服务器的域名。 示例值： presencebeta.xboxlive.com。|

<a id="ID4EPF"></a>


## <a name="optional-request-headers"></a>可选的请求标头

| 标头| 在任务栏的搜索框中键入| 描述|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| X-RequestedServiceVersion|  | 生成此请求应定向到 Xbox LIVE 的服务的名称/编号。 请求将只路由到的服务验证标头身份验证令牌中声明的有效性后，依次类推。 默认值：1.|

<a id="ID4EPG"></a>


## <a name="request-body"></a>请求正文

此请求的正文中不发送的任何对象。

<a id="ID4E1G"></a>


## <a name="response-body"></a>响应正文

<a id="ID4EAH"></a>


### <a name="sample-response"></a>示例响应

此方法返回[PresenceRecord](../../json/json-presencerecord.md)。


```cpp
{
  xuid:"0123456789",
  state:"online",
  devices:
  [{
    type:"D",
    titles:
    [{
      id:"12341234",
      name:"Contoso 5",
      state:"active",
      placement:"fill",
      timestamp:"2012-09-17T07:15:23.4930000",
      activity:
      {
        richPresence:"Team Deathmatch on Nirvana"
      }
    },
    {
      id:"12341235",
      name:"Contoso Waypoint",
      timestamp:"2012-09-17T07:15:23.4930000",
      placement:"snapped",
      state:"active",
      activity:
      {
        richPresence:"Using radar"
      }
    }]
  },
  {
    type:W8,
    titles:
    [{
      id:"23452345",
      name:"Contoso Gamehelp",
      state:"active",
      placement:"full",
      timestamp:"2012-09-17T07:15:23.4930000",
      activity:
      {
        richPresence:"Nirvana page",
      }
    }]
  }]
}

```


<a id="ID4EQH"></a>


## <a name="see-also"></a>另请参阅

<a id="ID4ESH"></a>


##### <a name="parent"></a>Parent 的子磁盘）

[/users/me](uri-usersme.md)
