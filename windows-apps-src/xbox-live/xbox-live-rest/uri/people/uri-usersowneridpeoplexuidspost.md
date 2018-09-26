---
title: POST (/users/{ownerId}/people/xuids)
assetID: e20bfb58-9c3b-14ed-6462-85d42fa6fe1a
permalink: en-us/docs/xboxlive/rest/uri-usersowneridpeoplexuidspost.html
author: KevinAsgari
description: " POST (/users/{ownerId}/people/xuids)"
ms.author: kevinasg
ms.date: 20-12-2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 27fbc0e209439fca01cf1e7d8c7c3bf98c4b9053
ms.sourcegitcommit: e4f3e1b2d08a02b9920e78e802234e5b674e7223
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/26/2018
ms.locfileid: "4211750"
---
# <a name="post-usersowneridpeoplexuids"></a>POST (/users/{ownerId}/people/xuids)
获取用户的 XUID 从调用方的用户集合。 这些 Uri 的域是`social.xboxlive.com`。
 
  * [备注](#ID4EV)
  * [URI 参数](#ID4E5)
  * [授权](#ID4EJB)
  * [需的请求标头](#ID4ERC)
  * [可选的请求标头](#ID4EBE)
  * [请求正文](#ID4EHF)
  * [HTTP 状态代码](#ID4EKH)
  * [所需的响应标头](#ID4ENBAC)
  * [响应正文](#ID4EZCAC)
 
<a id="ID4EV"></a>

 
## <a name="remarks"></a>备注
 
POST 操作不会修改任何资源，因此如果执行一次或多次，这将产生相同的结果。
  
<a id="ID4E5"></a>

 
## <a name="uri-parameters"></a>URI 参数
 
| 参数| 类型| 说明| 
| --- | --- | --- | 
| ownerId| 字符串| 正在访问其资源的用户的标识符。 必须匹配身份验证的用户。 可能的值为"我"、 xuid({xuid}) 或 gt({gamertag})。| 
  
<a id="ID4EJB"></a>

 
## <a name="authorization"></a>授权
 
| 类型| 必需| 描述| 如果缺少，响应| 
| --- | --- | --- | --- | --- | --- | --- | 
| XUID| 是| 调用方具有用户的 Xbox 用户 ID (XUID)。| 401 未授权| 
  
<a id="ID4ERC"></a>

 
## <a name="required-request-headers"></a>需的请求标头
 
| 标题| 说明| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| 授权| 字符串。 授权 Xbox LIVE 的数据。 这通常是加密的 XSTS 令牌。 示例值： <b>XBL3.0 x =&lt;userhash >;&lt;令牌 ></b>。| 
| Content-Length| 32 位无符号的整数。 长度，以字节为单位，请求正文。 示例值： 22。| 
| Content-Type| 字符串。 请求正文中的 MIME 类型。 这必须是<b>应用程序/json</b>。| 
  
<a id="ID4EBE"></a>

 
## <a name="optional-request-headers"></a>可选的请求标头
 
| 标题| 说明| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| X RequestedServiceVersion| 生成此请求应定向到 Xbox LIVE 的服务的名称/数。 验证在标头、 身份验证令牌等中的声明的有效性后仅为请求路由到该服务。默认值： 1。| 
| 接受| 字符串。 内容类型的调用方接受在响应中。 所有响应都是<b>应用程序/json</b>。| 
  
<a id="ID4EHF"></a>

 
## <a name="request-body"></a>请求正文
 
<a id="ID4ENF"></a>

 
### <a name="required-members"></a>所需的成员
 
| 成员| 说明| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| XuidList| 确定要返回的调用方的用户集合中的用户的 Xuid 的数组。 请参阅[XuidList (JSON)](../../json/json-xuidlist.md)。| 
  
<a id="ID4EKG"></a>

 
### <a name="optional-members"></a>可选的成员
 
没有此请求的可选的成员。
  
<a id="ID4EVG"></a>

 
### <a name="prohibited-members"></a>禁止的成员
 
所有其他成员被禁止在请求中。
  
<a id="ID4EAH"></a>

 
### <a name="sample-request"></a>示例请求
 

```cpp
{
    "xuids": [
        "2533274790395904", 
        "2533274792986770", 
        "2533274794866999"
    ]
}
      
```

   
<a id="ID4EKH"></a>

 
## <a name="http-status-codes"></a>HTTP 状态代码
 
此部分中使用此方法对此资源所做的请求的响应，该服务返回的状态代码之一。 有关使用 Xbox Live 服务的标准 HTTP 状态代码的完整列表，请参阅[标准 HTTP 状态代码](../../additional/httpstatuscodes.md)。
 
| 代码| 原因短语| 说明| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| 200| “确定”| "获取"方法时成功。| 
| 204| 任何内容| 成功时的方法是"添加"或者"删除"。| 
| 400| 错误请求| 方法参数已丢失或格式不正确，或用户 Id 的格式不正确。| 
| 403| 已禁止| 无法分析 XUID 声明与授权标头中。| 
  
<a id="ID4ENBAC"></a>

 
## <a name="required-response-headers"></a>所需的响应标头
 
| 标头| 类型| 说明| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| Content-Length| 32 位无符号的整数| 长度，以字节为单位，响应正文。 示例值： 22。| 
| Content-Type| 字符串| 响应正文的 MIME 类型。 这将始终为<b>应用程序/json</b>。| 
  
<a id="ID4EZCAC"></a>

 
## <a name="response-body"></a>响应正文
 
"获取"请求方法时，将只会发送的响应正文。 没有"添加"或"删除"无响应正文。
 
如果"获取"方法调用成功，该服务集合和数组，其中包含调用方的用户集合返回的用户总数中调用方的人。 无响应"添加"和"删除"方法返回。 请参阅[PeopleList (JSON)](../../json/json-peoplelist.md)。
 
<a id="ID4EHDAC"></a>

 
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
            "isFavorite": false,
            "isFollowingCaller": false
        },
    ],
    "totalCount": 3
}
         
```

   
<a id="ID4ERDAC"></a>

 
## <a name="see-also"></a>另请参阅
 
<a id="ID4ETDAC"></a>

 
##### <a name="parent"></a>Parent 的子磁盘） 

[/users/{ownerId}/people/xuids](uri-usersowneridpeoplexuids.md)

   