---
title: 获取 (/users/xuid({xuid}))
assetID: c97ef943-8bea-8a41-90d7-faea874284c8
permalink: en-us/docs/xboxlive/rest/uri-usersxuidget.html
author: KevinAsgari
description: " 获取 (/users/xuid({xuid}))"
ms.author: kevinasg
ms.date: 20-12-2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 308ecbddb5d62ae98d576f56af4cd3f7363c2c5a
ms.sourcegitcommit: 2a63ee6770413bc35ace09b14f56b60007be7433
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/12/2018
ms.locfileid: "3934719"
---
# <a name="get-usersxuidxuid"></a>获取 (/users/xuid({xuid}))
发现其他用户或客户端存在。
这些 Uri 的域是`userpresence.xboxlive.com`。

  * [备注](#ID4EV)
  * [URI 参数](#ID4EDB)
  * [查询字符串参数](#ID4EOB)
  * [授权](#ID4E4C)
  * [资源上的隐私设置的效果](#ID4EAE)
  * [需的请求标头](#ID4EVH)
  * [可选的请求标头](#ID4E1BAC)
  * [请求正文](#ID4E1CAC)
  * [响应正文](#ID4EFDAC)

<a id="ID4EV"></a>


## <a name="remarks"></a>备注

该响应可以进行筛选，以提供的[presencerecord，他的](../../json/json-presencerecord.md)一部分，如果使用者不感兴趣的整个对象。

> [!NOTE] 
> 返回的数据会受到隐私和内容隔离规则。



<a id="ID4EDB"></a>

 
## <a name="uri-parameters"></a>URI 参数

| 参数| 类型| 说明|
| --- | --- | --- | --- |
| xuid| 64 位无符号的整数| Xbox 用户 ID (XUID) 目标用户。|

<a id="ID4EOB"></a>


## <a name="query-string-parameters"></a>查询字符串参数

| 参数| 类型| 描述|
| --- | --- | --- | --- | --- | --- | --- |
| level| 字符串| 可选。 <ul><li><b>用户</b>： 返回仅用户节点。</li><li><b>设备</b>： 返回用户节点和设备节点。</li><li><b>标题</b>： 默认值。 返回除活动的整个树。</li><li><b>所有</b>： 返回整个树中，包括活动级别状态。</li></ul> |

<a id="ID4E4C"></a>


## <a name="authorization"></a>授权

| 类型| 必需| 描述| 如果缺少的响应|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| XUID| 是| 调用方的 Xbox 用户 ID (XUID)| 403 已禁止|

<a id="ID4EAE"></a>


## <a name="effect-of-privacy-settings-on-resource"></a>资源上的隐私设置的效果

此方法始终返回 200 确定，但可能不会在响应正文中返回的内容。

| 发出请求的用户| 目标用户的隐私设置| 行为|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 我| -| 200 OK|
| 好友| 每个人都| 200 OK|
| 好友| 仅好友| 200 OK|
| 好友| 阻止| 200 OK|
| 非好友用户| 每个人都| 200 OK|
| 非好友用户| 仅好友| 200 OK|
| 非好友用户| 阻止| 200 OK|
| 第三方站点| 每个人都| 200 OK|
| 第三方站点| 仅好友| 200 OK|
| 第三方站点| 阻止| 200 OK|

<a id="ID4EVH"></a>


## <a name="required-request-headers"></a>需的请求标头

| 标头| 类型| 说明|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 授权| 字符串| HTTP 身份验证的身份验证凭据。 示例值:"XBL3.0 x =&lt;userhash >;&lt;令牌 >"。|
| x xbl 协定版本| 字符串| 生成此请求应定向到的 Xbox LIVE 的服务的名称/号码。 请求将仅可路由到的服务后验证标头，身份验证令牌中的声明的有效性，依此类推。 示例值： 3，vnext。|
| 接受| 字符串| 内容类型可接受。 只有一个受状态是 application/json，但它必须在标头中指定。|
| 接受的语言| 字符串| 在响应中的字符串的可接受区域设置。 示例值： EN-US。|
| Host| 字符串| 服务器的域名。 示例值： presencebeta.xboxlive.com。|

<a id="ID4E1BAC"></a>


## <a name="optional-request-headers"></a>可选的请求标头

| 标头| 类型| 说明|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| X RequestedServiceVersion|  | 生成此请求应定向到的 Xbox LIVE 的服务的名称/号码。 请求将仅可路由到的服务后验证标头，身份验证令牌中的声明的有效性，依此类推。 默认值： 1。|

<a id="ID4E1CAC"></a>


## <a name="request-body"></a>请求正文

此请求的正文中不发送任何对象。

<a id="ID4EFDAC"></a>


## <a name="response-body"></a>响应正文

<a id="ID4ELDAC"></a>


### <a name="sample-response"></a>示例响应

如果没有现有记录的用户，则返回任何设备的记录。


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


<a id="ID4EXDAC"></a>


## <a name="see-also"></a>另请参阅

<a id="ID4EZDAC"></a>


##### <a name="parent"></a>Parent 的子磁盘）

[/users/xuid({xuid})](uri-usersxuid.md)
