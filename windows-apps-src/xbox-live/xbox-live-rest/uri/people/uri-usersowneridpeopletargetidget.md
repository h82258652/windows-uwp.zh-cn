---
title: GET (/users/{ownerId}/people/{targetid})
assetID: 2fd37b8e-b886-14f2-3399-59f530d85e4e
permalink: en-us/docs/xboxlive/rest/uri-usersowneridpeopletargetidget.html
author: KevinAsgari
description: " GET (/users/{ownerId}/people/{targetid})"
ms.author: kevinasg
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: b9c821e9d2cecc0b9a6bd02da650d40385a3fd91
ms.sourcegitcommit: cd00bb829306871e5103db481cf224ea7fb613f0
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/01/2018
ms.locfileid: "5887138"
---
# <a name="get-usersowneridpeopletargetid"></a>GET (/users/{ownerId}/people/{targetid})
调用方的用户集合中获取目标 ID 由一个人。 这些 Uri 的域是`social.xboxlive.com`。
 
  * [备注](#ID4EV)
  * [URI 参数](#ID4E5)
  * [授权](#ID4EJB)
  * [需的请求标头](#ID4ERC)
  * [可选的请求标头](#ID4EQD)
  * [请求正文](#ID4EWE)
  * [HTTP 状态代码](#ID4EBF)
  * [所需的响应标头](#ID4EDH)
  * [响应正文](#ID4EQAAC)
 
<a id="ID4EV"></a>

 
## <a name="remarks"></a>备注
 
获取操作不会修改任何资源，因此如果执行一次或多次，这将产生相同的结果。
  
<a id="ID4E5"></a>

 
## <a name="uri-parameters"></a>URI 参数
 
| 参数| 类型| 说明| 
| --- | --- | --- | 
| ownerId| 字符串| 要访问其资源的用户的标识符。 必须匹配身份验证的用户。 可能的值为"我"、 xuid({xuid}) 或 gt({gamertag})。| 
| targetid| 字符串| 在从所有者的人脉列表中，Xbox 用户 ID (XUID) 或玩家代号检索其数据的用户的标识符。 示例值： xuid(2603643534573581)、 gt(SomeGamertag)。| 
  
<a id="ID4EJB"></a>

 
## <a name="authorization"></a>授权
 
| 类型| 必需| 描述| 如果缺少的响应| 
| --- | --- | --- | --- | --- | --- | --- | 
| XUID| 是| 调用方都有用户的 Xbox 用户 ID (XUID)。| 401 未授权| 
  
<a id="ID4ERC"></a>

 
## <a name="required-request-headers"></a>需的请求标头
 
| 标题| 说明| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| 授权| 字符串。 授权 Xbox LIVE 的数据。 这通常是加密的 XSTS 令牌。 示例值： <b>XBL3.0 x =&lt;userhash >;&lt;令牌 ></b>。| 
  
<a id="ID4EQD"></a>

 
## <a name="optional-request-headers"></a>可选的请求标头
 
| 标题| 说明| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| X RequestedServiceVersion| 名称/的内部版本号应指向此请求的 Xbox LIVE 的服务。 验证该标头、 身份验证令牌等中的声明的有效性后仅为请求路由到该服务。默认值： 1。| 
| 接受| 字符串。 内容类型的调用方接受在响应中。 所有的响应是<b>application/json</b>。| 
  
<a id="ID4EWE"></a>

 
## <a name="request-body"></a>请求正文
 
此请求的正文中不发送任何对象。
  
<a id="ID4EBF"></a>

 
## <a name="http-status-codes"></a>HTTP 状态代码
 
此部分中使用此方法对此资源所做的请求的响应，该服务返回其中一个状态代码。 有关使用 Xbox Live 服务的标准 HTTP 状态代码的完整列表，请参阅[标准 HTTP 状态代码](../../additional/httpstatuscodes.md)。
 
| 代码| 原因短语| 说明| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| 200| “确定”| 成功。| 
| 400| 错误请求| 用户 Id 的格式不正确。| 
| 403| 已禁止| 无法分析 XUID 声明与授权标头中。| 
| 404| 找不到| 所有者的人脉列表中找不到目标用户。| 
  
<a id="ID4EDH"></a>

 
## <a name="required-response-headers"></a>所需的响应标头
 
| 标头| 类型| 说明| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| Content-Length| 32 位无符号的整数| 长度，以字节为单位，响应正文。 示例值： 22。| 
| Content-Type| 字符串| 响应正文的 MIME 类型。 这将始终为<b>application/json</b>。| 
  
<a id="ID4EQAAC"></a>

 
## <a name="response-body"></a>响应正文
 
如果在调用成功，该服务返回的目标人员。 请参阅[Person (JSON)](../../json/json-person.md)。
 
<a id="ID4E3AAC"></a>

 
### <a name="sample-response"></a>示例响应
 

```cpp
{
    "xuid": "2603643534573581",
    "isFavorite": false,
    "isFollowingCaller": false,
    "socialNetworks": ["LegacyXboxLive"]
}
         
```

   
<a id="ID4EGBAC"></a>

 
## <a name="see-also"></a>另请参阅
 
<a id="ID4EIBAC"></a>

 
##### <a name="parent"></a>Parent 的子磁盘） 

[/users/{ownerId}/people/{targetid}](uri-usersowneridpeopletargetid.md)

   