---
title: 获取 (/scids/ {scid} /leaderboards/ {leaderboardname})
assetID: 4adea46c-e910-8709-c28e-ce9178e712ef
permalink: en-us/docs/xboxlive/rest/uri-scidsscidleaderboardsleaderboardnameget.html
author: KevinAsgari
description: " 获取 (/scids/ {scid} /leaderboards/ {leaderboardname})"
ms.author: kevinasg
ms.date: 20-12-2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 961351e79ca04988c78c8e0be723435b39e5bae8
ms.sourcegitcommit: c8f6866100a4b38fdda8394ea185b02d7af66411
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/13/2018
ms.locfileid: "3959950"
---
# <a name="get-scidsscidleaderboardsleaderboardname"></a>获取 (/scids/ {scid} /leaderboards/ {leaderboardname})
 
获取预定义的全球排行榜。
 
这些 Uri 的域是`leaderboards.xboxlive.com`。
 
  * [备注](#ID4EY)
  * [URI 参数](#ID4EDB)
  * [查询字符串参数](#ID4EOB)
  * [授权](#ID4EID)
  * [资源上的隐私设置的效果](#ID4EDE)
  * [需的请求标头](#ID4EME)
  * [可选的请求标头](#ID4E1F)
  * [HTTP 状态代码](#ID4E1G)
  * [响应标头](#ID4ERCAC)
  * [响应正文](#ID4EOEAC)
 
<a id="ID4EY"></a>

 
## <a name="remarks"></a>备注
 
排行榜 Api 都是只读的因此仅支持获取动词命令。 它们反映排名和排序的"页"派生通过数据平台编写的单个用户统计数据的编制了索引的玩家统计数据。 完整的排行榜索引可能会相当大，并将永远不会要求调用方看到一个完整，因此此 URI 支持多个允许调用方是哪种视图其想要针对的排行榜，请参阅有关特定的查询字符串参数。
 
获取操作不会修改任何资源，因此如果执行一次或多次，这将产生相同的结果。
  
<a id="ID4EDB"></a>

 
## <a name="uri-parameters"></a>URI 参数
 
| 参数| 类型| 说明| 
| --- | --- | --- | 
| scid| GUID| 服务配置，其中包含要访问的资源标识符。| 
| leaderboardname| 字符串| 正在访问的预定义的排行榜资源的唯一标识符。| 
  
<a id="ID4EOB"></a>

 
## <a name="query-string-parameters"></a>查询字符串参数
 
| 参数| 类型| 说明| 
| --- | --- | --- | --- | --- | --- | 
| maxItems| 32 位无符号的整数| 排行榜要返回的记录的结果页中的最大数量。 如果未指定，默认数量将返回 (10)。 MaxItems 的最大值是仍未定义，但我们想要避免大型数据集，所以此值应该可能的最大设置的 UI 可以处理每次调用调谐器。| 
| skipToRank| 32 位无符号的整数| 返回结果与指定的排行榜排名启动的页面。 结果的其余部分将按排名排序顺序。 此查询字符串可以返回到后续查询以获取"下一步"的结果页返回一个延续令牌可以馈送。| 
| skipToUser| 字符串| 返回排行榜结果周围的指定玩家 xuid，无论该用户的排名或分数的页面。 该页面将按百分点等级与指定的用户或统计数据的排行榜视图中间中预定义视图的页面的最后一个位置进行排序。 提供此类查询没有 continuationToken。| 
| ContinuationToken| 字符串| 如果上一个调用返回 continuationToken，然后调用方可以传递回该令牌"按原样"查询字符串以获取结果的下一页。| 
  
<a id="ID4EID"></a>

 
## <a name="authorization"></a>授权
 
没有针对内容隔离和访问控制方案实现的授权逻辑。
 
   * 可以从任何平台上的客户端读取排行榜和用户统计数据，前提是调用方提交请求有效的 XSTS 令牌。 显然限于受数据平台的客户端写入。
   * 游戏开发人员可以将统计数据标记为打开或 XDP 或开发人员中心使用限制。 排行榜是开放的统计信息。 打开统计信息可以访问 Smartglass，以及 iOS、 Android、 Windows、 Windows Phone 和 web 应用程序，只要用户有权沙盒。 通过 XDP 或开发人员中心管理到沙盒的用户身份验证。
  
检查伪代码如下所示：
 

```cpp
If (!checkAccess(serviceConfigId, resource, CLAIM[userid, deviceid, titleid]))
{
        Reject request as Unauthorized
}

// else accept request.
         
```

  
<a id="ID4EDE"></a>

 
## <a name="effect-of-privacy-settings-on-resource"></a>资源上的隐私设置的效果
 
读取排行榜数据时，会不执行任何隐私检查。
  
<a id="ID4EME"></a>

 
## <a name="required-request-headers"></a>需的请求标头
 
| 标题| 说明| 
| --- | --- | --- | --- | --- | --- | --- | --- | 
| 授权| 字符串。 HTTP 身份验证的身份验证凭据。 示例值： <b>XBL3.0 x =&lt;userhash >;&lt;令牌 ></b>| 
| X RequestedServiceVersion| 字符串。 生成此请求应定向到的 Xbox LIVE 的服务的名称/号码。 验证标头、 身份验证令牌等中的声明的有效性后仅为请求路由到该服务。默认值： 1。| 
| 接受| 字符串。 内容类型可接受。 示例值：<b>应用程序/json</b>| 
  
<a id="ID4E1F"></a>

 
## <a name="optional-request-headers"></a>可选的请求标头
 
| 标题| 说明| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| If-None-Match| 字符串。 实体标记，如果客户端支持缓存使用。 示例值: <b>"686897696a7c876b7e"</b>|  | 
  
<a id="ID4E1G"></a>

 
## <a name="http-status-codes"></a>HTTP 状态代码
 
该服务返回的状态代码之一此部分中使用此方法对此资源进行的请求的响应。 有关使用 Xbox Live 服务的标准 HTTP 状态代码的完整列表，请参阅[标准 HTTP 状态代码](../../additional/httpstatuscodes.md)。
 
| 代码| 原因短语| 说明| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| 200| “确定”| 已成功检索会话。| 
| 304| 未修改| 资源不被修改由于最后一次请求。| 
| 400| 错误请求| 服务可能不理解格式不正确的请求。 通常是一个无效的参数。| 
| 401| 未授权| 请求要求用户身份验证。| 
| 403| 已禁止| 为用户或服务不允许该请求。| 
| 404| 找不到| 找不到指定的资源。| 
| 406| 不允许| 不支持资源版本。| 
| 408| 请求超时| 资源版本不受支持;应拒绝 MVC 层。| 
  
<a id="ID4ERCAC"></a>

 
## <a name="response-headers"></a>响应标头
 
| 标头| 类型| 说明| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| Content-Type| 字符串| 必需。 响应正文 MIME 类型。 示例：<b>应用程序/json</b>。| 
| Content-Length| 字符串| 必需。 在响应中发送的字节数。 示例： <b>232</b>。| 
| ETag| 字符串| 可选。 用于缓存优化。 示例： <b>686897696a7c876b7e</b>。| 
  
<a id="ID4EOEAC"></a>

 
## <a name="response-body"></a>响应正文
 
<a id="ID4EUEAC"></a>

 
### <a name="response-members"></a>响应成员
 
pagingInfo | pagingInfo| 部分| 可选。 仅存在时请求中指定 maxItems。| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| ContinuationToken| 64 位无符号的整数| 必需。 指定要返回到<b>skipItems</b>查询参数，以获取该 URI 的下一页结果，如果所需的源的值。 如果不返回任何<b>pagingInfo</b>则没有下一页要获取的数据。| 
| totalCount| 64 位无符号的整数| 必需。 排行榜中的条目的总数。 示例值： 1234567890| 
 
leaderboardInfo | leaderboardInfo| 部分| 必需。 始终返回。 包含有关请求排行榜的元数据。| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| 显示名称| 字符串| 不使用。| 
| totalCount| 字符串 （64 位无符号整数）| 必需。 排行榜中的条目的总数。 示例值： 1234567890| 
| 列| array| 必需。 排行榜中的列。| 
| 显示名称| 字符串| 必需。 对应于排行榜中的列。| 
| statName| 字符串| 必需。 对应于排行榜中的列。| 
| type| 字符串| 必需。 对应于排行榜中的列。| 
 
用户列表 | 用户列表| 部分| 必需。 始终返回。 包含请求的排行榜的实际用户分数。| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| 玩家代号| 字符串| 必需。 对应于排行榜条目中的用户。| 
| xuid| 64 位无符号的整数| 必需。 对应于排行榜条目中的用户。| 
| 百分比| 字符串| 必需。 对应于排行榜条目中的用户。| 
| 排名| 字符串| 必需。 对应于排行榜条目中的用户。| 
| 值| array| 必需。 每个以逗号分隔值对应于排行榜中的列。| 
  
<a id="ID4EGKAC"></a>

 
### <a name="sample-response"></a>示例响应
 
以下请求 URI 描述了在全球排行榜上排名到跳过。
 

```cpp
https://leaderboards.xboxlive.com/scids/0FA0D955-56CF-49DE-8668-05D82E6D45C4/leaderboards/TotalFlagsCaptured/columns/deaths?maxItems=3&skipToRank=15000
         
```

 
上述 URI 返回下面的 JSON 响应。
 

```cpp
{
    "pagingInfo": {
        "continuationToken": "152",
        "totalItems": 23437
    },
    "leaderboardInfo": {
        "displayName": "Total flags captured",
        "totalCount": 23437,
        "columns": [
            {
                "displayName": "Flags Captured",
                "statName": "flagscaptured",
                "type": "Integer"
            },
            {
                "displayName": "Deaths",
                "statName": "deaths",
                "type": "Integer"
            }
        ]
    },
    "userList": [
        {
            "gamertag": "WarriorSaint",
            "xuid": 1234567890123444,
            "percentile": 0.64,
            "rank": 15000,
            "values": [
                1000,
                47
            ]
        },
        {
            "gamertag": "xxxSniper39",
            "xuid": 1234567890123555,
            "percentile": 0.64,
            "rank": 15001,
            "values": [
                998,
                17
            ]
        },
        {
            "gamertag": "WhockaWhocka",
            "xuid": 1234567890123666,
            "percentile": 0.64,
            "rank": 15002,
            "values": [
                996,
                2
            ]
        }
    ]
}
         
```

 
以下请求 URI 描述了在全球排行榜上对用户跳过。
 

```cpp
https://leaderboards.xboxlive.com/scids/0FA0D955-56CF-49DE-8668-05D82E6D45C4/leaderboards/totalflagscaptured?maxItems=3&skipToUser=2343737892734237
         
```

 
上述 URI 返回下面的 JSON 响应。
 

```cpp
{
    "leaderboardInfo": 
    {   
       "displayName": "Total flags captured",
       "totalCount": 23437,
       "columns": [
              {
                  "displayName": "Flags Captured",
                  "statName": "flagscaptured",
                  "type": "Integer"
              }
       ]
    },
    "userList": [
        {
            "gamertag": "AverageJoe",
            "percentile": 55.00,
            "rank": 11718,
            "value": 1219,
            "xuid": 1234567890123444
        },
        {
            "gamertag": "AreUWatchinMe",
            "percentile": 60.00,
            "rank": 14162,
            "value": 1062,
            "xuid": 2343737892734333
        },
         {
            "gamertag": "WarriorSaint",
            "percentile": 64.39,
            "rank": 15000,
            "value ": 1000,
            "xuid": 1234567890123455
        }
    ]
}
         
```

   
<a id="ID4E5KAC"></a>

 
## <a name="see-also"></a>另请参阅
 
<a id="ID4EALAC"></a>

 
##### <a name="parent"></a>Parent 的子磁盘） 

[/scids/ {scid} /leaderboards/ {leaderboardname}](uri-scidsscidleaderboardsleaderboardname.md)

   