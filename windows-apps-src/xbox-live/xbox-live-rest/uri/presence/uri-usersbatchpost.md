---
title: POST (/users/batch)
assetID: bd0b18fe-8a6d-d591-5b13-bcd9643e945a
permalink: en-us/docs/xboxlive/rest/uri-usersbatchpost.html
author: KevinAsgari
description: " POST (/users/batch)"
ms.author: kevinasg
ms.date: 20-12-2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 4dd1b1859de81724a97fa40d9acdc3a1847d9421
ms.sourcegitcommit: 106aec1e59ba41aae2ac00f909b81bf7121a6ef1
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/15/2018
ms.locfileid: "4611336"
---
# <a name="post-usersbatch"></a>POST (/users/batch)
获取一批用户状态。
这些 Uri 的域是`userpresence.xboxlive.com`。

  * [备注](#ID4EV)
  * [授权](#ID4EAB)
  * [资源的隐私设置的效果](#ID4EDC)
  * [需的请求标头](#ID4EYF)
  * [可选的请求标头](#ID4EGAAC)
  * [请求正文](#ID4EGBAC)
  * [响应正文](#ID4ESEAC)

<a id="ID4EV"></a>


## <a name="remarks"></a>备注

应通过任何客户端、 服务或想要了解一批用户的状态信息的游戏使用此方法。

为此批处理请求的响应可以由深度和路径的筛选器。 消费者可以使用此找出并显示有关一组用户的状态。 此 API 上的筛选器为 Or 中一个属性，而 And 跨工作属性。

<a id="ID4EAB"></a>


## <a name="authorization"></a>授权

| 类型| 必需| 描述| 如果缺少的响应|
| --- | --- | --- | --- |
| XUID| 是| 调用方的 Xbox 用户 ID (XUID)| 403 已禁止|

<a id="ID4EDC"></a>


## <a name="effect-of-privacy-settings-on-resource"></a>资源的隐私设置的效果

此方法始终返回 200 确定，但可能不会在响应正文中返回的内容。

| 请求的用户| 目标用户的隐私设置| 行为|
| --- | --- | --- | --- | --- | --- | --- |
| 我| -| 200 OK|
| 好友| 每个人都| 200 OK|
| 好友| 仅好友| 200 OK|
| 好友| 阻止| 200 OK|
| 非好友用户| 每个人都| 200 OK|
| 非好友用户| 仅好友| 200 OK|
| 非好友用户| 阻止| 200 OK|
| 第三方网站| 每个人都| 200 OK|
| 第三方网站| 仅好友| 200 OK|
| 第三方网站| 阻止| 200 OK|

<a id="ID4EYF"></a>


## <a name="required-request-headers"></a>需的请求标头

| 标头| 类型| 描述|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 授权| 字符串| HTTP 身份验证的身份验证凭据。 示例值:"XBL3.0 x =&lt;userhash >;&lt;令牌 >"。|
| x xbl 协定版本| 字符串| 名称/的内部版本号应指向此请求的 Xbox LIVE 的服务。 请求将仅可路由到的服务验证该标头，身份验证令牌中的声明的有效性后，依此类推。 示例值： 3，vnext。|
| 接受| 字符串| 内容类型可接受。 支持的唯一一个是 application/json，但它必须在标头中指定。|
| 接受的语言| 字符串| 在响应中的字符串的可接受区域设置。 示例值： EN-US。|
| Host| 字符串| 服务器的域名。 示例值： presencebeta.xboxlive.com。|
| Content-Length| 字符串| 请求正文的长度。 示例值： 312。|

<a id="ID4EGAAC"></a>


## <a name="optional-request-headers"></a>可选的请求标头

| 标头| 类型| 描述|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| X RequestedServiceVersion|  | 名称/的内部版本号应指向此请求的 Xbox LIVE 的服务。 请求将仅可路由到的服务验证该标头，身份验证令牌中的声明的有效性后，依此类推。 默认值： 1。|

<a id="ID4EGBAC"></a>


## <a name="request-body"></a>请求正文

<a id="ID4EMBAC"></a>


### <a name="required-members"></a>所需的成员

| 成员| 描述|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 用户| 用户想要了解，最多个一次 1100 Xuid 其状态的列表 XUIDs。|

<a id="ID4EHCAC"></a>


### <a name="optional-members"></a>可选的成员

| 成员| 描述|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| deviceTypes| 使用你想要了解有关的用户的设备类型的列表。 如果该数组留空，则默认为所有可能的设备类型 （即，无筛选）。|
| 主题作品| 设备的列表类型你想要了解有关其的用户。 如果该数组留空，则默认为所有可能的游戏 （即，无筛选）。|
| level| 可能值： <ul><li>用户-获取用户节点</li><li>设备-获取用户和设备节点</li><li>游戏-获取基本标题级别信息</li><li>所有-获取完整状态信息和媒体信息</li></ul><br> 默认值为"标题"。|
| onlineOnly| 如果此属性为 true，批处理操作会筛选掉记录脱机用户 （包括遮盖的）。 如果它不提供，则将返回在线和离线用户。|

<a id="ID4E4DAC"></a>


### <a name="prohibited-members"></a>禁止的成员

所有其他成员被禁止在请求中。

<a id="ID4EIEAC"></a>


### <a name="sample-request"></a>示例请求


```cpp
{
  users:
  [
    "1234567890",
    "4567890123",
    "7890123456"
  ]
}

```


<a id="ID4ESEAC"></a>


## <a name="response-body"></a>响应正文

<a id="ID4E1EAC"></a>


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


<a id="ID4EKFAC"></a>


## <a name="see-also"></a>另请参阅

<a id="ID4EMFAC"></a>


##### <a name="parent"></a>Parent 的子磁盘）

[/users/batch](uri-usersbatch.md)
