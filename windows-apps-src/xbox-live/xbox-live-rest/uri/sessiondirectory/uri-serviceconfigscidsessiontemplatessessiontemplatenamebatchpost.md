---
title: POST (/serviceconfigs/{scid}/sessiontemplates/{sessionTemplateName}/batch)
assetID: 1a0a62ca-e120-e705-3c93-efd4697e2ccf
permalink: en-us/docs/xboxlive/rest/uri-serviceconfigscidsessiontemplatessessiontemplatenamebatchpost.html
description: " POST (/serviceconfigs/{scid}/sessiontemplates/{sessionTemplateName}/batch)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 833b3c6b532dd65856342678d646798c5f24a6c1
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2019
ms.locfileid: "57637182"
---
# <a name="post-serviceconfigsscidsessiontemplatessessiontemplatenamebatch"></a>POST (/serviceconfigs/{scid}/sessiontemplates/{sessionTemplateName}/batch)
在多个 Xbox 用户 Id 创建批查询中。

> [!IMPORTANT]
> 此方法由 2015年之多人游戏，并且仅适用于该多玩家版本和更高版本。 它旨在用于具有模板协定 104/105 或更高版本，并需要标头元素的 X Xbl 协定版本：104/105 或更高版本上的每个请求。

  * [备注](#ID4ET)
  * [URI 参数](#ID4EKB)
  * [查询字符串参数](#ID4EVB)
  * [HTTP 状态代码](#ID4EGF)
  * [请求正文](#ID4ENF)
  * [响应正文](#ID4EWF)

<a id="ID4ET"></a>


## <a name="remarks"></a>备注

此 HTTP/REST 方法上多个 Xbox 用户级别的会话的模板 Id 创建批处理查询。 此方法的两端可加**Microsoft.Xbox.Services.Multiplayer.MultiplayerService.GetSessionsForUsersFilterAsync**。

对于 2015年之多人游戏，可以将许多查询合并为单个调用所有查询都均相同，但如果处理不同的 Xbox 用户 Id (XUIDs)，如中所示*xuid*查询字符串参数。 请注意，查询字符串参数和响应，相同的正则查询和批处理的查询。

为批查询中，属于每个 XUID 会话写入到响应中的顺序相同*xuid*参数显示在请求中。 同一会话中进行多次一次出现在响应中，为每个可能*xuid*它与匹配。

<a id="ID4EKB"></a>


## <a name="uri-parameters"></a>URI 参数

| 参数| 在任务栏的搜索框中键入| 描述|
| --- | --- | --- | --- |
| scid| GUID| 服务配置标识符 (SCID)。 会话标识符的第 1 部分中。|
| sessionTemplateName| 字符串| 会话模板的当前实例的名称。 会话标识符的第 2 部分中。|

<a id="ID4EVB"></a>


## <a name="query-string-parameters"></a>查询字符串参数

在下一个表中使用查询字符串参数，可以修改查询。

| <b>参数</b>| <b>Type</b>| <b>描述</b>|
| --- | --- | --- | --- | --- | --- | --- |
| 关键字| 字符串| 一个关键字，例如，"foo"，若要检索必须在会话或模板中找到的。 |
| xuid| 64 位无符号的整数| Xbox 用户 ID，例如，"123"，对于要包含在查询中的会话。 默认情况下，用户必须是它要包含的会话中处于活动状态。 |
| 保留项| 布尔值| 为 true，包括会话为其用户设置为保留的播放机，但未联接变为活动状态的播放机。 当查询您自己的会话，或查询特定用户的会话的服务器时，才使用此参数。 |
| 非活动状态| 布尔值| 若要包括的用户已接受但不是播放会话，则为 true。 在其中用户已"就绪"，但不是"活动"的会话计数为非活动状态。 |
| 专用| 布尔值| 若要包含专用会话，则为 true。 当查询您自己的会话，或查询特定用户的会话的服务器时，才使用此参数。 |
| visibility| 字符串| 会话的可见性状态。 可能的值定义由<b>MultiplayerSessionVisibility</b>。 如果此参数设置为"打开"，该查询应包含仅打开的会话。 如果将其设置为"专用，"<i>专用</i>参数必须设置为 true。 |
| version| 32 位有符号的整数| 应包含会话最大版本。 例如，值 2 指定 2 的主要会话版本具有该唯一会话或小于应将包括在内。 版本号必须小于或等于请求的协定版本 mod 100。 |
| Take| 32 位有符号的整数| 若要检索的会话的数目上限。 此数字必须介于 0 和 100 之间。 |


设置*私有*或*预订*以 true 需要对该会话的服务器级访问权限。 或者，这些设置要求调用方的 XUID 声明以在 URI 中的 XUID 筛选器匹配。 否则，HTTP/403 状态代码返回，无论实际存在任何此类会话。

<a id="ID4EGF"></a>


## <a name="http-status-codes"></a>HTTP 状态代码
同样适用于 MPSD，服务将返回 HTTP 状态代码。  
<a id="ID4ENF"></a>


## <a name="request-body"></a>请求正文


```cpp
{ "xuids": [ "1234", "5678" ] }

```


<a id="ID4EWF"></a>


## <a name="response-body"></a>响应正文

此方法的返回是具有一些会话数据包含内联的会话引用的 JSON 数组。


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
