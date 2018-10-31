---
title: GET (/users/me)
assetID: 726c279b-73fb-02ea-cbff-700ff2dc31af
permalink: en-us/docs/xboxlive/rest/uri-usersmeget.html
author: KevinAsgari
description: " GET (/users/me)"
ms.author: kevinasg
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 825b101ef5b450910f0bd9b2ab84991daa8074a7
ms.sourcegitcommit: ca96031debe1e76d4501621a7680079244ef1c60
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/31/2018
ms.locfileid: "5836665"
---
# <a name="get-usersme"></a>GET (/users/me)
获取当前用户的[PresenceRecord](../../json/json-presencerecord.md) ，而无需知道用户的 XUID。
这些 Uri 的域是`userpresence.xboxlive.com`。

  * [查询字符串参数](#ID4EZ)
  * [授权](#ID4EIC)
  * [需的请求标头](#ID4ELD)
  * [可选的请求标头](#ID4EPF)
  * [请求正文](#ID4EPG)
  * [响应正文](#ID4E1G)

<a id="ID4EZ"></a>


## <a name="query-string-parameters"></a>查询字符串参数

| 参数| 类型| 描述|
| --- | --- | --- |
| level| 字符串| 可选。 <ul><li><b>用户</b>： 返回仅用户节点。</li><li><b>设备</b>： 返回用户节点和设备节点。</li><li><b>标题</b>： 默认值。 返回除活动的整个树。</li><li><b>所有</b>： 返回整个树中，包括活动级别状态。</li></ul> | 

<a id="ID4EIC"></a>


## <a name="authorization"></a>授权

| 类型| 必需| 描述| 如果缺少的响应|
| --- | --- | --- | --- | --- | --- | --- |
| XUID| 是| 调用方的 Xbox 用户 ID (XUID)| 403 已禁止|

<a id="ID4ELD"></a>


## <a name="required-request-headers"></a>需的请求标头

| 标头| 类型| 说明|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 授权| 字符串| HTTP 身份验证的身份验证凭据。 示例值:"XBL3.0 x =&lt;userhash >;&lt;令牌 >"。|
| x xbl 协定版本| 字符串| 名称/的内部版本号应指向此请求的 Xbox LIVE 的服务。 请求将仅可路由到的服务验证该标头，身份验证令牌中的声明的有效性后，依此类推。 示例值： 3，vnext。|
| 接受| 字符串| 内容类型可接受。 支持的唯一一个是 application/json，但它必须在标头中指定。|
| 接受的语言| 字符串| 在响应中的字符串的可接受区域设置。 示例值： EN-US。|
| Host| 字符串| 服务器的域名。 示例值： presencebeta.xboxlive.com。|

<a id="ID4EPF"></a>


## <a name="optional-request-headers"></a>可选的请求标头

| 标头| 类型| 说明|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| X RequestedServiceVersion|  | 名称/的内部版本号应指向此请求的 Xbox LIVE 的服务。 请求将仅可路由到的服务验证该标头，身份验证令牌中的声明的有效性后，依此类推。 默认值： 1。|

<a id="ID4EPG"></a>


## <a name="request-body"></a>请求正文

此请求的正文中不发送任何对象。

<a id="ID4E1G"></a>


## <a name="response-body"></a>响应正文

<a id="ID4EAH"></a>


### <a name="sample-response"></a>示例响应

此方法返回[presencerecord，他的](../../json/json-presencerecord.md)。


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
