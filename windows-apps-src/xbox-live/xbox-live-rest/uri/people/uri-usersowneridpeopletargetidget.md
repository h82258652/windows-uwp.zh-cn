---
title: GET (/users/{ownerId}/people/{targetid})
assetID: 2fd37b8e-b886-14f2-3399-59f530d85e4e
permalink: en-us/docs/xboxlive/rest/uri-usersowneridpeopletargetidget.html
description: " GET (/users/{ownerId}/people/{targetid})"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 408b4df30f53e27b04e2a1e654e9686d2b359637
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2019
ms.locfileid: "57632672"
---
# <a name="get-usersowneridpeopletargetid"></a>GET (/users/{ownerId}/people/{targetid})
从调用方的用户集合中获取目标 ID 的人员。 这些 Uri 的域是`social.xboxlive.com`。
 
  * [备注](#ID4EV)
  * [URI 参数](#ID4E5)
  * [Authorization](#ID4EJB)
  * [所需的请求标头](#ID4ERC)
  * [可选的请求标头](#ID4EQD)
  * [请求正文](#ID4EWE)
  * [HTTP 状态代码](#ID4EBF)
  * [所需的响应标头](#ID4EDH)
  * [响应正文](#ID4EQAAC)
 
<a id="ID4EV"></a>

 
## <a name="remarks"></a>备注
 
GET 操作不会修改任何资源，因此如果执行一次或多次，这将产生相同的结果。
  
<a id="ID4E5"></a>

 
## <a name="uri-parameters"></a>URI 参数
 
| 参数| 在任务栏的搜索框中键入| 描述| 
| --- | --- | --- | 
| ownerId| 字符串| 其资源的访问的用户的标识符。 必须与匹配身份验证的用户。 可能的值为"me"、 xuid({xuid}) 或 gt({gamertag})。| 
| targetid| 字符串| 正在从所有者的用户列表中，Xbox 用户 ID (XUID) 或玩家代号检索其数据的用户的标识符。 示例值： xuid(2603643534573581)、 gt(SomeGamertag)。| 
  
<a id="ID4EJB"></a>

 
## <a name="authorization"></a>授权
 
| 在任务栏的搜索框中键入| 必需| 描述| 如果缺少的响应| 
| --- | --- | --- | --- | --- | --- | --- | 
| XUID| 是| 调用方具有用户的 Xbox 用户 ID (XUID)。| 401 未经授权| 
  
<a id="ID4ERC"></a>

 
## <a name="required-request-headers"></a>所需的请求标头
 
| 标头| 描述| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| 授权| 字符串。 用于 Xbox LIVE 的授权数据。 这通常是加密的 XSTS 令牌。 示例值：<b>XBL3.0 x=&lt;userhash>;&lt;token></b>.| 
  
<a id="ID4EQD"></a>

 
## <a name="optional-request-headers"></a>可选的请求标头
 
| 标头| 描述| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| X-RequestedServiceVersion| 生成此请求应定向到 Xbox LIVE 的服务的名称/编号。 验证标头中的身份验证令牌等的声明的有效性后仅为将请求路由到该服务。默认值：1.| 
| 接受| 字符串。 内容类型的调用方接受在响应中。 所有响应都都<b>应用程序 /json</b>。| 
  
<a id="ID4EWE"></a>

 
## <a name="request-body"></a>请求正文
 
此请求的正文中不发送的任何对象。
  
<a id="ID4EBF"></a>

 
## <a name="http-status-codes"></a>HTTP 状态代码
 
服务将返回其中一个状态代码在本部分中使用此方法在此资源上发出的请求的响应中。 有关与 Xbox Live 服务一起使用的标准 HTTP 状态代码的完整列表，请参阅[标准 HTTP 状态代码](../../additional/httpstatuscodes.md)。
 
| 代码| 原因短语| 描述| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| 200| 确定| 成功。| 
| 400| 无效的请求| 用户 Id 的格式不正确。| 
| 403| 已禁止| 无法从授权标头分析 XUID 声明。| 
| 404| 未找到| 所有者的用户列表中找不到目标用户。| 
  
<a id="ID4EDH"></a>

 
## <a name="required-response-headers"></a>所需的响应标头
 
| 标头| 在任务栏的搜索框中键入| 描述| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| 内容长度| 32 位无符号的整数| 长度 （字节），响应正文中。 示例值：22.| 
| 内容类型| 字符串| 响应正文的 MIME 类型。 此属性始终为<b>应用程序 /json</b>。| 
  
<a id="ID4EQAAC"></a>

 
## <a name="response-body"></a>响应正文
 
如果调用成功，服务将返回目标用户。 请参阅[Person (JSON)](../../json/json-person.md)。
 
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

   