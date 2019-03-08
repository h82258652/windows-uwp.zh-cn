---
title: POST (/users/batch)
assetID: bd0b18fe-8a6d-d591-5b13-bcd9643e945a
permalink: en-us/docs/xboxlive/rest/uri-usersbatchpost.html
description: " POST (/users/batch)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 47376338a1c515aa00a7e0247df4cc16ee0db8d2
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2019
ms.locfileid: "57655672"
---
# <a name="post-usersbatch"></a>POST (/users/batch)
获取一批用户存在。
这些 Uri 的域是`userpresence.xboxlive.com`。

  * [备注](#ID4EV)
  * [Authorization](#ID4EAB)
  * [资源上的隐私设置的效果](#ID4EDC)
  * [所需的请求标头](#ID4EYF)
  * [可选的请求标头](#ID4EGAAC)
  * [请求正文](#ID4EGBAC)
  * [响应正文](#ID4ESEAC)

<a id="ID4EV"></a>


## <a name="remarks"></a>备注

此方法应使用的任何客户端、 服务或想要学习一批用户的状态信息的标题。

此批处理请求的响应可以是由深度和路径筛选器。 使用者可以使用此以找出并显示有关一组用户存在。 此 API 上的筛选器作为 Or 中某个属性，但 And 对有效属性。

<a id="ID4EAB"></a>


## <a name="authorization"></a>授权

| 在任务栏的搜索框中键入| 必需| 描述| 如果缺少的响应|
| --- | --- | --- | --- |
| XUID| 是| 调用方的 Xbox 用户 ID (XUID)| 403 禁止访问|

<a id="ID4EDC"></a>


## <a name="effect-of-privacy-settings-on-resource"></a>资源上的隐私设置的效果

此方法始终返回 200 正常，但可能不会在响应正文中返回的内容。

| 发出请求的用户| 面向用户的隐私设置| 行为|
| --- | --- | --- | --- | --- | --- | --- |
| 我| -| 200 OK|
| 友元| 每个人| 200 OK|
| 友元| 仅朋友| 200 OK|
| 友元| 已阻止| 200 OK|
| 非友元用户| 每个人| 200 OK|
| 非友元用户| 仅朋友| 200 OK|
| 非友元用户| 已阻止| 200 OK|
| 第三方站点| 每个人| 200 OK|
| 第三方站点| 仅朋友| 200 OK|
| 第三方站点| 已阻止| 200 OK|

<a id="ID4EYF"></a>


## <a name="required-request-headers"></a>所需的请求标头

| 标头| 在任务栏的搜索框中键入| 描述|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 授权| 字符串| HTTP 身份验证的身份验证凭据。 示例值："XBL3.0 x =&lt;userhash >;&lt;令牌 >"。|
| x-xbl-contract-version| 字符串| 生成此请求应定向到 Xbox LIVE 的服务的名称/编号。 请求将只路由到的服务验证标头身份验证令牌中声明的有效性后，依次类推。 示例值：3，vnext。|
| 接受| 字符串| 内容类型可接受。 只有一个支持的状态显示为 application/json，但必须在标头中指定它。|
| Accept-Language| 字符串| 在响应中的字符串的可接受区域设置。 示例值： EN-US。|
| 主机| 字符串| 服务器的域名。 示例值： presencebeta.xboxlive.com。|
| 内容长度| 字符串| 请求正文的长度。 示例值：312.|

<a id="ID4EGAAC"></a>


## <a name="optional-request-headers"></a>可选的请求标头

| 标头| 在任务栏的搜索框中键入| 描述|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| X-RequestedServiceVersion|  | 生成此请求应定向到 Xbox LIVE 的服务的名称/编号。 请求将只路由到的服务验证标头身份验证令牌中声明的有效性后，依次类推。 默认值：1.|

<a id="ID4EGBAC"></a>


## <a name="request-body"></a>请求正文

<a id="ID4EMBAC"></a>


### <a name="required-members"></a>所需的成员

| 成员| 描述|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 用户| 用户想要了解，最多一次 1100 XUIDs 其存在状态的列表 XUIDs。|

<a id="ID4EHCAC"></a>


### <a name="optional-members"></a>可选的成员

| 成员| 描述|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| deviceTypes| 你想要了解有关的用户使用的设备类型的列表。 如果数组为空，则默认为所有可能的设备类型 （即，不过滤掉）。|
| 标题| 你想要了解有关其的用户类型的设备的列表。 如果数组为空，则默认为所有可能的标题 （即，不过滤掉）。|
| level| 可能值： <ul><li>用户-获取用户节点</li><li>设备-获取用户和设备节点</li><li>标题-获取基本标题级别信息</li><li>全部-获取丰富的状态显示信息、 媒体信息和 / 或</li></ul><br> 默认值为"title"。|
| onlineOnly| 如果此属性为 true，批处理操作将筛选出为脱机用户 （包括掩蔽的） 记录。 如果未提供，将返回联机和脱机用户。|

<a id="ID4E4DAC"></a>


### <a name="prohibited-members"></a>禁止的成员

所有其他成员禁止在请求中使用。

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


<a id="ID4EKFAC"></a>


## <a name="see-also"></a>另请参阅

<a id="ID4EMFAC"></a>


##### <a name="parent"></a>Parent 的子磁盘）

[/users/batch](uri-usersbatch.md)
