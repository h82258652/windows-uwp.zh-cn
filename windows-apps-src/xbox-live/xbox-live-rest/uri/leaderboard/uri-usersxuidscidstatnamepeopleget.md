---
title: GET (/users/xuid({xuid})/scids/{scid}/stats/{statname)/people/{all|favorite})
assetID: 942cf0d7-f988-0495-cf28-cdac608b8109
permalink: en-us/docs/xboxlive/rest/uri-usersxuidscidstatnamepeopleget.html
description: " GET (/users/xuid({xuid})/scids/{scid}/stats/{statname)/people/{all|favorite})"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 674e52d15d115560d4813edcd9687c2fe9694c9b
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2019
ms.locfileid: "57650762"
---
# <a name="get-usersxuidxuidscidsscidstatsstatnamepeopleallfavorite"></a>GET (/users/xuid({xuid})/scids/{scid}/stats/{statname)/people/{all|favorite})
通过排名统计信息值 （分数） 为当前用户是所有已知的联系人或仅由该用户指定为最喜欢的人的联系人返回社交排行榜。
这些 Uri 的域是`leaderboards.xboxlive.com`。

  * [备注](#ID4EV)
  * [URI 参数](#ID4EAB)
  * [查询字符串参数](#ID4ELB)
  * [Authorization](#ID4EQD)
  * [所需的请求标头](#ID4EGE)
  * [可选的请求标头](#ID4EXF)
  * [请求正文](#ID4ETG)
  * [HTTP 状态代码](#ID4ECEAC)
  * [所需的响应标头](#ID4E1HAC)
  * [可选的响应标头](#ID4EDJAC)
  * [响应正文](#ID4EDKAC)

<a id="ID4EV"></a>


## <a name="remarks"></a>备注

排行榜 Api 都是只读的因此 （通过 HTTPS) 仅支持 GET 谓词。 它们反映排名和排序"页"的派生自通过数据平台编写的单个用户统计信息的索引的播放机统计信息。 完整排行榜索引可能会很大，并将永远不会要求调用方查看完整的其中一个，因此此 URI 也支持几个允许调用方是什么类型的视图，它想要查看针对该排行榜特定的查询字符串参数。

GET 操作不会修改任何资源，因此如果执行一次或多次，这将产生相同的结果。

<a id="ID4EAB"></a>


## <a name="uri-parameters"></a>URI 参数

| 参数| 在任务栏的搜索框中键入| 描述|
| --- | --- | --- |
| xuid| 字符串| 用户的标识符。|
| scid| 字符串| 服务配置，其中包含要访问的资源的标识符。|
| statname| 字符串| 要访问的用户统计信息资源的唯一标识符。|
| 全部|收藏夹| 枚举| 排名统计信息是否为当前用户的所有已知的联系人或仅由该用户指定为最喜欢的人的联系人的值 （分数）。|

<a id="ID4ELB"></a>


## <a name="query-string-parameters"></a>查询字符串参数

| 参数| 在任务栏的搜索框中键入| 描述|
| --- | --- | --- | --- | --- | --- |
| maxItems| 32 位无符号的整数| 排行榜要返回的记录的结果页中的最大数目。 如果未指定，(10) 将返回默认数量。 MaxItems 的最大值是仍不确定，但我们希望避免大型数据集，因此此值可能应为目标的最大设置的 UI 可处理每个调用的调谐器。 |
| skipToRank| 32 位无符号的整数| 返回从指定的排行榜排名的结果的页面。 结果的其余部分将按排名的排序顺序。 此查询字符串可以恢复到后续查询以获取"下一页"的结果返回可以提供一个继续标记。 |
| skipToUser| 字符串| 返回围绕指定的玩家 xuid，而不考虑该用户的级别或评分的排行榜结果的页面。 使用指定的用户在最后一个位置的页的预定义的视图，或 stat 排行榜视图中间的百分位排名将按排序页。 没有任何<b>continuationToken</b>提供此类型的查询。 |
| ContinuationToken| 字符串| 如果上一次调用返回<b>continuationToken</b>，然后调用方可以将传递回该令牌"按原样"查询字符串以获取下一页结果。 |
| sort| 字符串| 指定是否对从低到高值顺序进行排序 （"升序"） 的玩家列表或高到低值 （"降序"） 的顺序。 这是一个可选参数;默认值为降序顺序。|

<a id="ID4EQD"></a>


## <a name="authorization"></a>授权

Xuid 授权是必需的。

内容隔离和访问控制方案用于实现授权逻辑。

可以从任何平台上的客户端读取排行榜和用户统计信息，前提是调用方将提交具有其请求的有效 XSTS 令牌。 （这一点显而易见） 限制为客户端支持的数据平台。 写入是。

标题开发人员可以将统计信息标记为开放或受限 XDP 或合作伙伴中心。 排行榜是打开的统计信息。 打开统计信息可以通过 Smartglass，以及 iOS、 Android、 Windows、 Windows Phone 和 web 应用程序访问，只要用户授权到沙盒。 用户授权给沙盒通过 XDP 或合作伙伴中心管理。

<a id="ID4EGE"></a>


## <a name="required-request-headers"></a>所需的请求标头

| 标头| 描述|
| --- | --- | --- | --- | --- | --- | --- | --- |
| 授权| 字符串。 HTTP 身份验证的身份验证凭据。 示例值："XBL3.0 x =&lt;userhash >;&lt;令牌 >"。|
| 内容类型| 字符串。 请求正文的 MIME 类型。 示例值:"application/json"。|
| X-RequestedServiceVersion| 生成此请求应定向到 Xbox LIVE 的服务的名称/编号。 请求将只路由到的服务验证标头身份验证令牌中声明的有效性后，依次类推。 默认值：1.|
| 接受| 字符串。 可接受的内容类型值。 示例值:"application/json"。|

<a id="ID4EXF"></a>


## <a name="optional-request-headers"></a>可选的请求标头

| 标头| 描述|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| If-None-Match| 字符串。 实体标记-如果客户端支持缓存使用。 示例值："686897696a7c876b7e"。|

<a id="ID4ETG"></a>


## <a name="request-body"></a>请求正文

若要最大化的任何调用方能够理解它重新获得正确显示的数据，请为每个用户的每个统计信息值将返回以在其中应显示，并设置格式与匹配的区域设置，接受语言中指定的格式的字符串在请求中的标头。 为该排行榜 statname 将还返回本地化"显示名称"。

<a id="ID4E2G"></a>


### <a name="required-members"></a>所需的成员

| 成员| 描述|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| <b>pagingInfo</b>| 部分| 可选。 在页中的最后一项的排名低于 totalItems 时返回。 本部分中也不返回 skipToUser 指定请求中时。|
| ContinuationToken| 字符串| 必需。 指定要返回到"continuationToken"查询参数，若要获得该 URI 的下一个结果页，如果所需的源的值。 如果返回没有 pagingInfo 则没有要获取的数据的"下一步页"。|
| totalItems| 64 位无符号的整数| 必需。 排行榜中的条目的总数。|
| <b>leaderboardInfo</b>| 部分| 必需。 始终返回。 包含有关请求排行榜的元数据。|
| displayName| 字符串| 必需。 预定义的排行榜的本地化的显示名称。 示例值："总标志捕获"。|
| totalCount| 字符串| 必需。 排行榜中的条目的总数。|
| 列| 数组| 必需。|
| displayName| 字符串| 必需。 对应于在排行榜中的列。|
| statName| 字符串| 必需。 对应于在排行榜中的列。|
| type| 字符串| 必需。 对应于在排行榜中的列。|
| <b>userList</b>| 部分| 必需。 始终返回。 包含请求排行榜的实际用户分数。|
| 玩家代号| 字符串| 必需。 对应于排行榜条目中的用户。|
| xuid| 32 位有符号的整数| 必需。 对应于排行榜条目中的用户。|
| 百分位数| 字符串| 必需。 对应于排行榜条目中的用户。|
| 排名| 字符串| 必需。 对应于排行榜条目中的用户。|
| 值| 数组| 必需。 以逗号分隔的每个值对应于排行榜中的列。|

<a id="ID4ECEAC"></a>


## <a name="http-status-codes"></a>HTTP 状态代码

服务将返回其中一个状态代码在本部分中使用此方法在此资源上发出的请求的响应中。 有关与 Xbox Live 服务一起使用的标准 HTTP 状态代码的完整列表，请参阅[标准 HTTP 状态代码](../../additional/httpstatuscodes.md)。

| 代码| 原因短语| 描述|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 200| 确定| 已成功检索该会话。|
| 304| 未修改|  |
| 400| 无效的请求| | 服务无法理解请求格式不正确。 通常是一个无效的参数。|
| 401| 未经授权| | 请求需要用户身份验证。|
| 403| 已禁止| 为用户或服务不允许该请求。|
| 404| 未找到| 找不到指定的资源。|
| 406| 不可接受| 不支持资源版本;应拒绝的 MVC 层。|
| 408| 请求超时| 服务无法理解请求格式不正确。 通常是一个无效的参数。|

<a id="ID4E1HAC"></a>


## <a name="required-response-headers"></a>所需的响应标头

| 标头| 在任务栏的搜索框中键入| 描述|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 内容类型| 字符串| 响应正文的 mime 类型。 示例值:"application/json"。|
| 内容长度| 字符串| 在响应中发送的字节数。 示例值："232".|

<a id="ID4EDJAC"></a>


## <a name="optional-response-headers"></a>可选的响应标头

| 标头| 在任务栏的搜索框中键入| 描述|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| ETag| 字符串| 用于缓存优化。 示例值："686897696a7c876b7e"。|

<a id="ID4EDKAC"></a>


## <a name="response-body"></a>响应正文

有关社交排行榜，没有分页请求：

https://leaderboards.xboxlive.com/users/xuid(2533274916402282)/scids/c1ba92a9-0000-0000-0000-000000000000/stats/EnemyDefeats/people/all?sort=descending

<a id="ID4ENKAC"></a>


### <a name="sample-response"></a>示例响应


```cpp
{
    "pagingInfo": null,
    "leaderboardInfo": {
        "displayName": "Kills",
        "totalCount": 3,
        "columns": [
            {
                "displayName": "Kills",
                "statName": "enemydefeats",
                "type": "integer"
            }
        ]
    },
    "userList": [
        {
            "gamertag":"xxxSniper39",
            "xuid":"1234567890123555",
            "percentile":1.0,
            "rank":1,
            "values": [
                "47"
            ]
        },
        {
            "gamertag":"WarriorSaint",
            "xuid":"2533274916402282",
            "percentile":0.66,
            "rank":2,
            "values": [
                "42"
            ]
        },
        {
            "gamertag":"WhockaWhocka",
            "xuid":"1234567890123666",
            "percentile":0.33,
            "rank":3,
            "values": [
                "12"
            ]
        }
    ]
}

```


<a id="ID4EXKAC"></a>


## <a name="see-also"></a>另请参阅

<a id="ID4EZKAC"></a>


##### <a name="parent"></a>Parent 的子磁盘）

[/users/xuid({xuid})/scids/{scid}/stats/{statname)/people/{all\|favorite}](uri-usersxuidscidstatnamepeople.md)
