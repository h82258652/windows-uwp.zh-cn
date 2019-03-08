---
title: POST (/users/{ownerId}/people/xuids)
assetID: e20bfb58-9c3b-14ed-6462-85d42fa6fe1a
permalink: en-us/docs/xboxlive/rest/uri-usersowneridpeoplexuidspost.html
description: " POST (/users/{ownerId}/people/xuids)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 1cb160f3276d215e3aba5dfd671c67fa17d883b5
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2019
ms.locfileid: "57589862"
---
# <a name="post-usersowneridpeoplexuids"></a>POST (/users/{ownerId}/people/xuids)
获取由 XUID 人的用户给调用方的集合。 这些 Uri 的域是`social.xboxlive.com`。
 
  * [备注](#ID4EV)
  * [URI 参数](#ID4E5)
  * [Authorization](#ID4EJB)
  * [所需的请求标头](#ID4ERC)
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
 
| 参数| 在任务栏的搜索框中键入| 描述| 
| --- | --- | --- | 
| ownerId| 字符串| 其资源的访问的用户的标识符。 必须与匹配身份验证的用户。 可能的值为"me"、 xuid({xuid}) 或 gt({gamertag})。| 
  
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
| 内容长度| 32 位无符号的整数。 以字节为单位，请求正文的长度。 示例值：22.| 
| 内容类型| 字符串。 请求正文的 MIME 类型。 这必须是<b>应用程序 /json</b>。| 
  
<a id="ID4EBE"></a>

 
## <a name="optional-request-headers"></a>可选的请求标头
 
| 标头| 描述| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| X-RequestedServiceVersion| 生成此请求应定向到 Xbox LIVE 的服务的名称/编号。 验证标头中的身份验证令牌等的声明的有效性后仅为将请求路由到该服务。默认值：1.| 
| 接受| 字符串。 内容类型的调用方接受在响应中。 所有响应都都<b>应用程序 /json</b>。| 
  
<a id="ID4EHF"></a>

 
## <a name="request-body"></a>请求正文
 
<a id="ID4ENF"></a>

 
### <a name="required-members"></a>所需的成员
 
| 成员| 描述| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| XuidList| 标识要从调用方的用户集合中返回的人的 XUIDs 的数组。 请参阅[XuidList (JSON)](../../json/json-xuidlist.md)。| 
  
<a id="ID4EKG"></a>

 
### <a name="optional-members"></a>可选的成员
 
没有为此请求的可选成员。
  
<a id="ID4EVG"></a>

 
### <a name="prohibited-members"></a>禁止的成员
 
所有其他成员禁止在请求中使用。
  
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
 
服务将返回其中一个状态代码在本部分中使用此方法在此资源上发出的请求的响应中。 有关与 Xbox Live 服务一起使用的标准 HTTP 状态代码的完整列表，请参阅[标准 HTTP 状态代码](../../additional/httpstatuscodes.md)。
 
| 代码| 原因短语| 描述| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| 200| 确定| "Get"方法时成功。| 
| 204| 无内容| 成功时的方法是"添加"或者"删除"。| 
| 400| 无效的请求| 方法参数缺失或格式不正确，或用户 Id 的格式不正确。| 
| 403| 已禁止| 无法从授权标头分析 XUID 声明。| 
  
<a id="ID4ENBAC"></a>

 
## <a name="required-response-headers"></a>所需的响应标头
 
| 标头| 在任务栏的搜索框中键入| 描述| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| 内容长度| 32 位无符号的整数| 长度 （字节），响应正文中。 示例值：22.| 
| 内容类型| 字符串| 响应正文的 MIME 类型。 此属性始终为<b>应用程序 /json</b>。| 
  
<a id="ID4EZCAC"></a>

 
## <a name="response-body"></a>响应正文
 
"Get"请求方法时，才会发送响应正文。 没有响应正文的"添加"或"删除"。
 
如果"get"方法调用成功，服务返回的总人数在调用方的人们集合，并且包含调用方的用户集合的数组。 "添加"和"删除"方法会不返回任何响应。 请参阅[PeopleList (JSON)](../../json/json-peoplelist.md)。
 
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

   