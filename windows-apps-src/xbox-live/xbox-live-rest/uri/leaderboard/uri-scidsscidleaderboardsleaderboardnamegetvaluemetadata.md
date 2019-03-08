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
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2019
ms.locfileid: "57623432"
---
# <a name="get-scidsscidleaderboardsleaderboardnameincludevaluemetadata"></a>GET (/scids/{scid}/leaderboards/{leaderboardname}?include=valuemetadata)
 
获取预定义的全局排行榜以及任何与排行榜值相关联的元数据。
 
这些 Uri 的域是`leaderboards.xboxlive.com`。
 
  * [备注](#ID4EY)
  * [URI 参数](#ID4EHB)
  * [查询字符串参数](#ID4ESB)
  * [Authorization](#ID4EVD)
  * [资源上的隐私设置的效果](#ID4EQE)
  * [所需的请求标头](#ID4EZE)
  * [可选的请求标头](#ID4EOG)
  * [HTTP 状态代码](#ID4EOH)
  * [响应标头](#ID4EFDAC)
  * [响应正文](#ID4ECFAC)
 
<a id="ID4EY"></a>

 
## <a name="remarks"></a>备注
 
？ 包括 = valuemetadata 查询参数，响应包含任何与排行榜值相关联的元数据。 值元数据包含客户端指定的值，如模型和用于实现在争用轨上的最佳时间一辆汽车的颜色与关联的数据。
 
值元数据定义排行榜，基于不在 leaderboard 本身上的用户状态。
 
排行榜 Api 都是只读的因此仅支持 GET 谓词。 它们反映排名和排序"页"的派生自通过数据平台编写的单个用户统计信息的索引的播放机统计信息。 完整排行榜索引可能会很大，并将永远不会要求调用方查看完整的其中一个，因此此 URI 也支持几个允许调用方是什么类型的视图，它想要查看针对该排行榜特定的查询字符串参数。
 
GET 操作不会修改任何资源，因此如果执行一次或多次，这将产生相同的结果。
  
<a id="ID4EHB"></a>

 
## <a name="uri-parameters"></a>URI 参数
 
| 参数| 在任务栏的搜索框中键入| 描述| 
| --- | --- | --- | 
| scid| GUID| 服务配置，其中包含要访问的资源的标识符。| 
| leaderboardname| 字符串| 要访问的预定义的排行榜资源的唯一标识符。| 
  
<a id="ID4ESB"></a>

 
## <a name="query-string-parameters"></a>查询字符串参数
 
| 参数| 在任务栏的搜索框中键入| 描述| 
| --- | --- | --- | --- | --- | --- | 
| include=valuemetadata| 字符串| 指示响应包含与排行榜值相关联的任何值元数据。| 
| maxItems| 32 位无符号的整数| 排行榜要返回的记录的结果页中的最大数目。 如果未指定，(10) 将返回默认数量。 MaxItems 的最大值是仍不确定，但我们希望避免大型数据集，因此此值可能应为目标的最大设置的 UI 可处理每个调用的调谐器。| 
| skipToRank| 32 位无符号的整数| 返回从指定的排行榜排名的结果的页面。 结果的其余部分将按排名的排序顺序。 此查询字符串可以恢复到后续查询以获取"下一页"的结果返回可以提供一个继续标记。| 
| skipToUser| 字符串| 返回围绕指定的玩家 xuid，而不考虑该用户的级别或评分的排行榜结果的页面。 使用指定的用户在最后一个位置的页的预定义的视图，或 stat 排行榜视图中间的百分位排名将按排序页。 不没有为此类型的查询提供任何 continuationToken。| 
| ContinuationToken| 字符串| 如果上一次调用返回 continuationToken，然后调用方可以将传递回该令牌"按原样"查询字符串以获取下一页结果。| 
  
<a id="ID4EVD"></a>

 
## <a name="authorization"></a>授权
 
没有为内容隔离和访问控制方案而实现的授权逻辑。
 
   * 可以从任何平台上的客户端读取排行榜和用户统计信息，前提是调用方将提交与请求的有效 XSTS 令牌。 写入是很明显限制为客户端支持的数据平台。
   * 标题开发人员可以将统计信息标记为开放或受限 XDP 或合作伙伴中心。 排行榜是打开的统计信息。 打开统计信息可以通过 Smartglass，以及 iOS、 Android、 Windows、 Windows Phone 和 web 应用程序访问，只要用户授权到沙盒。 用户授权给沙盒通过 XDP 或合作伙伴中心管理。
  
用于检查的伪代码如下所示：
 

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

 
## <a name="required-request-headers"></a>所需的请求标头
 
| 标头| 描述| 
| --- | --- | --- | --- | --- | --- | --- | --- | 
| 授权| 字符串。 HTTP 身份验证的身份验证凭据。 示例值：<b>XBL3.0 x=&lt;userhash>;&lt;token></b>| 
| X-Xbl-Contract-Version| 字符串。 指示要使用的 API 版本。 若要在响应中包含值的元数据，此值必须设置为"3"。| 
| X-RequestedServiceVersion| 字符串。 生成此请求应定向到 Xbox LIVE 的服务的名称/编号。 验证标头中的身份验证令牌等的声明的有效性后仅为将请求路由到该服务。默认值：1.| 
| 接受| 字符串。 内容类型可接受。 示例值： <b>application/json</b>| 
  
<a id="ID4EOG"></a>

 
## <a name="optional-request-headers"></a>可选的请求标头
 
| 标头| 描述| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| If-None-Match| 字符串。 如果客户端支持缓存使用的实体标记。 示例值：<b>"686897696a7c876b7e"</b>|  | 
  
<a id="ID4EOH"></a>

 
## <a name="http-status-codes"></a>HTTP 状态代码
 
服务将返回其中一个状态代码在本部分中使用此方法在此资源上发出的请求的响应中。 有关与 Xbox Live 服务一起使用的标准 HTTP 状态代码的完整列表，请参阅[标准 HTTP 状态代码](../../additional/httpstatuscodes.md)。
 
| 代码| 原因短语| 描述| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| 200| 确定| 已成功检索该会话。| 
| 304| 未修改| 资源不被修改自最后一次请求。| 
| 400| 无效的请求| 服务无法理解请求格式不正确。 通常是一个无效的参数。| 
| 401| 未经授权| 请求需要用户身份验证。| 
| 403| 已禁止| 为用户或服务不允许该请求。| 
| 404| 未找到| 找不到指定的资源。| 
| 406| 不可接受| 不支持资源版本。| 
| 408| 请求超时| 不支持资源版本;应拒绝的 MVC 层。| 
  
<a id="ID4EFDAC"></a>

 
## <a name="response-headers"></a>响应标头
 
| 标头| 在任务栏的搜索框中键入| 描述| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| 内容类型| 字符串| 必需。 响应正文的 MIME 类型。 示例：<b>应用程序 /json</b>。| 
| 内容长度| 字符串| 必需。 在响应中发送的字节数。 示例：<b>232</b>.| 
| ETag| 字符串| 可选。 用于缓存优化。 示例：<b>686897696a7c876b7e</b>。| 
  
<a id="ID4ECFAC"></a>

 
## <a name="response-body"></a>响应正文
 
<a id="ID4EIFAC"></a>

 
### <a name="response-members"></a>响应成员
 
pagingInfo | pagingInfo| 部分| 可选。 仅当请求中指定 maxItems 时所显示。| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| ContinuationToken| 64 位无符号的整数| 必需。 指定要返回到源的值<b>skipItems</b>查询参数，以根据需要为该 URI 获取下一页结果。 如果没有<b>pagingInfo</b>返回，则没有下一步要获取的数据页。| 
| totalItems| 64 位无符号的整数| 必需。 排行榜中的条目的总数。 示例值：1234567890| 
 
leaderboardInfo | leaderboardInfo| 部分| 必需。 始终返回。 包含有关请求排行榜的元数据。| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| displayName| 字符串| 不使用。| 
| totalCount| 字符串 （64 位无符号整数）| 必需。 排行榜中的条目的总数。 示例值：1234567890| 
| columnDefinition| JSON 对象| 必需。 排行榜中的列的说明。| 
| columnDefinition.displayName| 字符串| 必需。 排行榜中列的描述性名称。| 
| columnDefinition.statName| 字符串| 必需。 排行榜为基础的用户状态的名称。| 
| columnDefinition.type| 字符串| 必需。 列中的用户状态数据类型。| 
 
userList | userList| 部分| 必需。 始终返回。 包含请求排行榜的实际用户分数。| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| 玩家代号| 字符串| 必需。 排行榜条目中用户的玩家代号。| 
| xuid| 64 位无符号的整数| 必需。 排行榜条目中用户的 Xbox 用户 ID。| 
| 百分位数| 字符串| 必需。 排行榜中的用户的百分位排名。| 
| 排名| 字符串| 必需。 用户的数值等级排行榜中。| 
| value| 字符串| 必需。 排行榜所基于的状态的用户的值。| 
| valueMetadata| 字符串| 必需。 一个字符串以逗号分隔字符串对，格式"\"名称\":\"值\"，..."

如果没有任何元数据，此值为空字符串。 | 
  
<a id="ID4EGLAC"></a>

 
### <a name="sample-response"></a>示例响应
 
以下请求 URI 描绘了正在跳过为级别上的全局排行榜。
 

```cpp
https://leaderboards.xboxlive.com/scids/0FA0D955-56CF-49DE-8668-05D82E6D45C4/leaderboards/TotalFlagsCaptured?include=valuemetadata&maxItems=3&skipToRank=15000
         
```

 
为了返回值的元数据，还必须指定以下标头。
 

```cpp
X-Xbl-Contract-Version : 3
          
```

 
上面的 URI 返回以下 JSON 响应。
 

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

   