---
title: GET (/scids/{scid}/leaderboards/{leaderboardname}?include=valuemetadata)
assetID: ee69a9e3-ea91-3cf5-a10a-811762cba892
permalink: en-us/docs/xboxlive/rest/uri-scidsscidleaderboardsleaderboardnamegetvaluemetadata.html
description: " GET (/scids/{scid}/leaderboards/{leaderboardname}?include=valuemetadata)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: fad57c5e989d933777c913030faaa594c6bbd059
ms.sourcegitcommit: 49d58bc66c1c9f2a4f81473bcb25af79e2b1088d
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/11/2018
ms.locfileid: "8936371"
---
# <a name="get-scidsscidleaderboardsleaderboardnameincludevaluemetadata"></a>GET (/scids/{scid}/leaderboards/{leaderboardname}?include=valuemetadata)
 
获取预定义的全局排行榜，以及任何关联的排行榜值元数据。
 
这些 Uri 的域是`leaderboards.xboxlive.com`。
 
  * [备注](#ID4EY)
  * [URI 参数](#ID4EHB)
  * [查询字符串参数](#ID4ESB)
  * [授权](#ID4EVD)
  * [资源上的隐私设置的效果](#ID4EQE)
  * [需的请求标头](#ID4EZE)
  * [可选的请求标头](#ID4EOG)
  * [HTTP 状态代码](#ID4EOH)
  * [响应标头](#ID4EFDAC)
  * [响应正文](#ID4ECFAC)
 
<a id="ID4EY"></a>

 
## <a name="remarks"></a>备注
 
？ 包括 = valuemetadata 查询参数允许该响应包含任何关联的排行榜值元数据。 值元数据包含客户端指定的值，例如模型和用于实现跑道的最佳时间汽车的颜色与关联的数据。
 
排行榜，基于排行榜本身上不在用户统计数据，定义值元数据。
 
排行榜 Api 都是只读的因此仅支持 GET 动词命令。 它们反映排名和排序"页"的派生通过数据平台编写的单个用户统计数据的编制了索引的玩家统计数据。 完整的排行榜索引可能会相当大，并将永远不会要求调用方看到一个完整，因此此 URI 支持多个允许调用方是哪种类型的视图其想要针对该排行榜，请参阅有关特定的查询字符串参数。
 
获取操作不会修改任何资源，因此如果执行一次或多次，这将产生相同的结果。
  
<a id="ID4EHB"></a>

 
## <a name="uri-parameters"></a>URI 参数
 
| 参数| 类型| 描述| 
| --- | --- | --- | 
| scid| GUID| 服务配置，其中包含所访问的资源的标识符。| 
| leaderboardname| 字符串| 正在访问的预定义的排行榜资源的唯一标识符。| 
  
<a id="ID4ESB"></a>

 
## <a name="query-string-parameters"></a>查询字符串参数
 
| 参数| 类型| 描述| 
| --- | --- | --- | --- | --- | --- | 
| 包括 = valuemetadata| 字符串| 指示该响应包括任何关联的排行榜值的值元数据。| 
| maxItems| 32 位无符号的整数| 排行榜要返回的记录的结果页中的最大数量。 如果未指定，默认数量将会返回 (10)。 MaxItems 的最大值是仍未定义的但我们想要避免大型数据集，因此此值应该可能目标的最大设置的 UI 可以处理每次调用调谐器。| 
| skipToRank| 32 位无符号的整数| 返回结果与指定的排行榜排名启动的页面。 结果的其余部分将按排名排序顺序。 此查询字符串可以返回到后续查询以获取"下一步"的结果页返回一个延续令牌可以馈送。| 
| skipToUser| 字符串| 返回排行榜结果周围的指定玩家 xuid，无论该用户的排名或分数的页面。 该页面将按百分点等级与指定的用户或统计数据的排行榜视图中间中预定义视图的页面的最后一个位置进行排序。 不没有对于此类型的查询提供任何 continuationToken。| 
| ContinuationToken| 字符串| 如果上一个调用返回 continuationToken，然后调用方可以传递回该令牌"按原样"查询字符串以获取结果的下一页。| 
  
<a id="ID4EVD"></a>

 
## <a name="authorization"></a>授权
 
没有针对内容隔离和访问控制方案实现的授权逻辑。
 
   * 可以从任何平台上的客户端读取排行榜和用户的统计数据，前提是调用方提交与请求有效的 XSTS 令牌。 写入进行显然限于受数据平台的客户端。
   * 游戏开发人员可以将统计数据标记为打开或使用 XDP 或合作伙伴中心限制。 排行榜是打开统计信息。 打开统计信息可以访问 Smartglass，以及 iOS、 Android、 Windows、 Windows Phone 和 web 应用程序，只要用户有权访问沙盒。 通过 XDP 或合作伙伴中心管理到沙盒的用户身份验证。
  
检查伪代码如下所示：
 

```cpp
If (!checkAccess(serviceConfigId, resource, CLAIM[userid, deviceid, titleid]))
{
        Reject request as Unauthorized
}

// else accept request.
         
```

  
<a id="ID4EQE"></a>

 
## <a name="effect-of-privacy-settings-on-resource"></a>资源上的隐私设置的效果
 
读取排行榜数据时，会不执行任何隐私检查。
  
<a id="ID4EZE"></a>

 
## <a name="required-request-headers"></a>需的请求标头
 
| 标题| 描述| 
| --- | --- | --- | --- | --- | --- | --- | --- | 
| 授权| 字符串。 HTTP 身份验证的身份验证凭据。 示例值： <b>XBL3.0 x =&lt;userhash >;&lt;令牌 ></b>| 
| X Xbl 协定版本| 字符串。 指示哪个版本的要使用的 API。 为了在响应中包含值元数据，此值必须设置为"3"。| 
| X RequestedServiceVersion| 字符串。 名称/的内部版本号应指向此请求的 Xbox LIVE 的服务。 验证在标头、 身份验证令牌等中的声明的有效性后仅为请求路由到该服务。默认值： 1。| 
| 接受| 字符串。 内容类型的可接受。 示例值： <b>application/json</b>| 
  
<a id="ID4EOG"></a>

 
## <a name="optional-request-headers"></a>可选的请求标头
 
| 标题| 描述| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| If-None-Match| 字符串。 使用如果客户端支持缓存的实体标记。 示例值: <b>"686897696a7c876b7e"</b>|  | 
  
<a id="ID4EOH"></a>

 
## <a name="http-status-codes"></a>HTTP 状态代码
 
此部分中使用此方法对此资源所做的请求的响应，该服务返回的状态代码之一。 有关使用 Xbox Live 服务的标准 HTTP 状态代码的完整列表，请参阅[标准 HTTP 状态代码](../../additional/httpstatuscodes.md)。
 
| 代码| 原因短语| 描述| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| 200| “确定”| 已成功检索会话。| 
| 304| 未修改| 资源不已修改自最后一次请求。| 
| 400| 错误请求| 服务可能不理解格式不正确的请求。 通常无效参数。| 
| 401| 未授权| 请求要求用户身份验证。| 
| 403| 已禁止| 为用户或服务不允许该请求。| 
| 404| 找不到| 找不到指定的资源。| 
| 406| 不允许| 资源版本不受支持。| 
| 408| 请求超时| 资源版本不受支持;应拒绝 MVC 层。| 
  
<a id="ID4EFDAC"></a>

 
## <a name="response-headers"></a>响应标头
 
| 标头| 类型| 描述| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| Content-Type| 字符串| 必需。 响应正文 MIME 类型。 示例： <b>application/json</b>。| 
| Content-Length| 字符串| 必需。 正在发送响应中的字节数。 示例： <b>232</b>。| 
| ETag| 字符串| 可选。 用于缓存优化。 示例： <b>686897696a7c876b7e</b>。| 
  
<a id="ID4ECFAC"></a>

 
## <a name="response-body"></a>响应正文
 
<a id="ID4EIFAC"></a>

 
### <a name="response-members"></a>响应成员
 
pagingInfo | pagingInfo| 部分| 可选。 仅存在时请求中指定 maxItems。| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| ContinuationToken| 64 位无符号的整数| 必需。 指定要返回到<b>skipItems</b>查询参数，以获取该 URI 的下一页结果，如果所需的订阅源的值。 如果没有<b>pagingInfo</b>返回则没有下一页要获取的数据。| 
| totalItems| 64 位无符号的整数| 必需。 排行榜中的条目的总数。 示例值： 1234567890| 
 
leaderboardInfo | leaderboardInfo| 部分| 必需。 始终返回。 包含有关排行榜请求的元数据。| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| displayName| 字符串| 不使用。| 
| totalCount| 字符串 （64 位无符号整数）| 必需。 排行榜中的条目的总数。 示例值： 1234567890| 
| columnDefinition| JSON 对象| 必需。 介绍在排行榜中的列。| 
| columnDefinition.displayName| 字符串| 必需。 在排行榜中列的描述性名称。| 
| columnDefinition.statName| 字符串| 必需。 排行榜基于用户统计数据名称。| 
| columnDefinition.type| 字符串| 必需。 用户统计数据的列中的数据类型。| 
 
userList | userList| 部分| 必需。 始终返回。 包含请求的排行榜的实际用户分数。| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| 玩家代号| 字符串| 必需。 在排行榜条目的用户的玩家代号。| 
| xuid| 64 位无符号的整数| 必需。 在排行榜条目的用户的 Xbox 用户 ID。| 
| 百分比| 字符串| 必需。 在排行榜中的用户的百分比排名。| 
| 排名| 字符串| 必需。 在排行榜中的用户的数字排名。| 
| 值| 字符串| 必需。 排行榜基于统计数据的用户的值。| 
| valueMetadata| 字符串| 必需。 一个字符串的以逗号分隔的字符串对，以格式"\"name\": \"value\"，..."

如果没有元数据，此值为空字符串。 | 
  
<a id="ID4EGLAC"></a>

 
### <a name="sample-response"></a>示例响应
 
以下请求 URI 描述了在全球排行榜上排名到跳过。
 

```cpp
https://leaderboards.xboxlive.com/scids/0FA0D955-56CF-49DE-8668-05D82E6D45C4/leaderboards/TotalFlagsCaptured?include=valuemetadata&maxItems=3&skipToRank=15000
         
```

 
为了将返回值元数据，还必须指定以下标头。
 

```cpp
X-Xbl-Contract-Version : 3
          
```

 
上述 URI 返回以下 JSON 响应。
 

```cpp
{
    "pagingInfo": {
        "continuationToken": "15003",
        "totalItems": 23437
    },
    "leaderboardInfo": {
        "displayName": "Total flags captured",
        "totalCount": 23437,
        "columnDefinition" : 
            {
                "displayName": "Flags Captured",
                "statName": "flagscaptured",
                "type": "Integer"
            }
    },
    "userList": [
        {
            "gamertag": "WarriorSaint",
            "xuid": "1234567890123444",
            "percentile": 0.64,
            "rank": 15000,
            "value": "1002",
            "valuemetadata" : "{\"color\" : \"silver\",\"weight\" : 2000, \"israining\" : false}"
        },
        {
            "gamertag": "xxxSniper39",
            "xuid": "1234567890123555",
            "percentile": 0.64,
            "rank": 15001,
            "value": "993",
            "valuemetadata" : "{\"color\" : \"silver\",\"weight\" : 2020, \"israining\" : true}"
 
        },
        {
            "gamertag": "WhockaWhocka",
            "xuid": "1234567890123666",
            "percentile": 0.64,
            "rank": 15002,
            "value": "700",
            "valuemetadata" : "{\"color\" : \"red\",\"weight\" : 300, \"israining\" : false}"
        }
    ]
}
         
```

   
<a id="ID4E6LAC"></a>

 
## <a name="see-also"></a>另请参阅
 
<a id="ID4EBMAC"></a>

 
##### <a name="parent"></a>Parent 的子磁盘） 

[/scids/{scid}/leaderboards/{leaderboardname}](uri-scidsscidleaderboardsleaderboardname.md)

   