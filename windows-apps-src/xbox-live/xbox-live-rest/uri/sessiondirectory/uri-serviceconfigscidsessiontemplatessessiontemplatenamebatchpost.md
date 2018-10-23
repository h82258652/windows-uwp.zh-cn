---
title: POST (/serviceconfigs/{scid}/sessiontemplates/{sessionTemplateName}/batch)
assetID: 1a0a62ca-e120-e705-3c93-efd4697e2ccf
permalink: en-us/docs/xboxlive/rest/uri-serviceconfigscidsessiontemplatessessiontemplatenamebatchpost.html
author: KevinAsgari
description: " POST (/serviceconfigs/{scid}/sessiontemplates/{sessionTemplateName}/batch)"
ms.author: kevinasg
ms.date: 20-12-2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 0ed9a461b630f1ec277190c43efa99aa74492b0e
ms.sourcegitcommit: c4d3115348c8b54fcc92aae8e18fdabc3deb301d
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/22/2018
ms.locfileid: "5404728"
---
# <a name="post-serviceconfigsscidsessiontemplatessessiontemplatenamebatch"></a>POST (/serviceconfigs/{scid}/sessiontemplates/{sessionTemplateName}/batch)
在多个 Xbox 用户 Id 创建批处理查询。

> [!IMPORTANT]
> 此方法使用 2015年多人游戏和应用仅向该多人游戏版本及更高版本。 它旨在用于使用模板合约 104/105 或更高版本，并且需要 X Xbl 协定版本的标头元素： 104/105 或更高版本上每个请求。

  * [备注](#ID4ET)
  * [URI 参数](#ID4EKB)
  * [查询字符串参数](#ID4EVB)
  * [HTTP 状态代码](#ID4EGF)
  * [请求正文](#ID4ENF)
  * [响应正文](#ID4EWF)

<a id="ID4ET"></a>


## <a name="remarks"></a>备注

在多个 Xbox 用户 Id 在会话模板级别上，此 HTTP/REST 方法创建批查询。 此方法可以通过**Microsoft.Xbox.Services.Multiplayer.MultiplayerService.GetSessionsForUsersFilterAsync**包装。

为 2015年多人游戏，你可以合并许多查询为单次调用所有查询都均相同，但如果*xuid*查询字符串参数表示不同的 Xbox 用户 Id (Xuid)、 处理。 请注意，查询字符串参数，以及的响应，相同的常规查询和批处理查询。

对于批处理查询，属于每个 XUID 的会话写入顺序相同响应请求中提供的*xuid*参数。 很可能在同一个会话多次出现在响应中，它可以匹配每个*xuid*的一次。

<a id="ID4EKB"></a>


## <a name="uri-parameters"></a>URI 参数

| 参数| 类型| 说明|
| --- | --- | --- | --- |
| scid| GUID| 服务配置标识符 (SCID)。 第 1 部分会话标识符。|
| sessionTemplateName| 字符串| 会话模板的当前实例的名称。 第 2 部分会话标识符。|

<a id="ID4EVB"></a>


## <a name="query-string-parameters"></a>查询字符串参数

可以使用下表中的查询字符串参数修改查询。

| <b>参数</b>| <b>类型</b>| <b>说明</b>|
| --- | --- | --- | --- | --- | --- | --- |
| 关键字| 字符串| 一个关键字，例如，"foo"，它们是否要检索必须在会话或模板中找到的。 |
| xuid| 64 位无符号的整数| Xbox 用户 ID，例如，"123"，以在查询中包括的会话。 默认情况下，用户必须处于活动状态为它要包括的会话中。 |
| 预订| 布尔型| 若要包括的会话用户设置为保留玩家，但并未加入成为活动玩家。 在查询你自己的会话，或特定用户的会话服务器到服务器查询时，仅使用此参数。 |
| 处于非活动状态| 布尔型| 若要包括用户已接受但不是会主动玩游戏的会话，则为 true。 计数为非活动状态的用户已"准备好"但不是"活动"的会话。 |
| 专用| 布尔型| 如果为 true，以包含专用会话。 在查询你自己的会话，或特定用户的会话服务器到服务器查询时，仅使用此参数。 |
| 可见性| 字符串| 会话的可见性状态。 由<b>MultiplayerSessionVisibility</b>定义可能的值。 如果此参数设置为"打开"，该查询应包括仅打开的会话。 如果它被设置为"private"，<i>专用</i>参数必须设置为 true。 |
| version| 32 位有符号整数| 应包含最大会话版本。 例如，值为 2 指定主要的会话版本的 2 的唯一会话或更少应包含。 版本号必须小于或等于请求的协定版本 mod 100。 |
| 参加| 32 位有符号整数| 若要检索的会话最大数量。 此数字必须介于 0 和 100 之间。 |


设置为 true 的*私有*或*预订*需要对会话的服务器级访问权限。 或者，这些设置要求调用方的 XUID 声明以匹配 URI 中的 XUID 筛选器。 否则，HTTP/403 状态代码返回，无论确实存在任何此类会话。

<a id="ID4EGF"></a>


## <a name="http-status-codes"></a>HTTP 状态代码
该服务返回 HTTP 状态代码，因为它适用于 MPSD。  
<a id="ID4ENF"></a>


## <a name="request-body"></a>请求正文


```cpp
{ "xuids": [ "1234", "5678" ] }

```


<a id="ID4EWF"></a>


## <a name="response-body"></a>响应正文

此方法的返回的会话引用，内含某些会话数据包含以内联方式的 JSON 数组。


```cpp
{
    "results": [ {
      "xuid": "9876",    // If the session was found from a xuid, that xuid.
      "startTime": "2009-06-15T13:45:30.0900000Z",
      "sessionRef": {
        "scid": "foo",
        "templateName": "bar",
        "name": "session-seven"
      },
      "accepted": 4,    // Approximate number of non-reserved members.
      "status": "active",    // or "reserved" or "inactive". This is the state of the user in the session, not the session itself. Only present if the session was found using a xuid.
      "visibility": "open",    // or "private", "visible", or "full"
      "joinRestriction": "local",    // or "followed". Only present if 'visibility' is "open" or "full" and the session has a join restriction.
      "myTurn": true,    // Not present is the same as 'false'. Only present if the session was found using a xuid.
      "keywords": [ "one", "two" ]
    }
  ]
}
```


<a id="ID4EDG"></a>


## <a name="see-also"></a>另请参阅

<a id="ID4EFG"></a>


##### <a name="parent"></a>Parent 的子磁盘）

[/serviceconfigs/{scid}/sessiontemplates/{sessionTemplateName}/batch](uri-serviceconfigscidsessiontemplatessessiontemplatenamebatch.md)
