---
title: GET (/serviceconfigs/{scid}/sessions)
assetID: adc65d0b-58dd-bfb9-54c8-9bc9d02e68ec
permalink: en-us/docs/xboxlive/rest/uri-serviceconfigsscidsessionsget.html
description: " GET (/serviceconfigs/{scid}/sessions)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: e54c9cd68a899cfd040bc3e16a05f6deb2daa7c3
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2019
ms.locfileid: "57614102"
---
# <a name="get-serviceconfigsscidsessions"></a>GET (/serviceconfigs/{scid}/sessions)
检索指定的会话信息。

> [!IMPORTANT]
> 此方法现在需要 X Xbl 约定版本标头元素：104/105 或更高版本上的每个请求。

  * [备注](#ID4ET)
  * [URI 参数](#ID4EKB)
  * [HTTP 状态代码](#ID4EXB)
  * [请求正文](#ID4EAC)
  * [响应正文](#ID4ELC)

<a id="ID4ET"></a>


## <a name="remarks"></a>备注

此 HTTP/REST 方法检索所提供的筛选器的会话信息。 此方法的两端可加**Microsoft.Xbox.Services.Multiplayer.MultiplayerService.GetSessionsAsync**。


> [!NOTE] 
> 对于 2015年之多人游戏，调用此方法<b>Microsoft.Xbox.Services.Multiplayer.MultiplayerService.GetSessionsForUsersFilterAsync</b>。  



> [!NOTE] 
> 此方法的每个调用必须包含一个关键字、 Xbox 用户 ID 筛选器，或两者。 如果调用方不具有正确的权限<i>私有</i>并<i>预订</i>参数，该方法返回的错误代码为 403 禁止访问，是否实际存在任何此类会话。  


<a id="ID4EKB"></a>


## <a name="uri-parameters"></a>URI 参数

| 参数| 在任务栏的搜索框中键入| 描述|
| --- | --- | --- | --- | --- | --- |
| scid| GUID| 服务配置标识符 (SCID)。 第 1 部分会话的 id。|
| 关键字| 字符串| 用于结果筛选为只使用该字符串标识的会话的关键字。|
| xuid| GUID| 为其检索会话的用户的 Xbox 用户 Id。 用户必须是会话中处于活动状态。|
| 保留项| 字符串| 值，该值指示如果会话的列表包括用户未接受。 仅可以将此参数设置为 true。 此设置要求调用方拥有对该会话，服务器级访问权限，或调用方的 XUID 声明以与 Xbox 用户 ID 筛选器匹配。 |
| 非活动状态| 字符串| 值，该值指示会话的列表包括那些用户已接受但不是主动播放。 仅可以将此参数设置为 true。|
| 专用| 字符串| 值，该值指示是否会话的列表包括专用会话。 仅可以将此参数设置为 true。 仅当查询您自己的会话，或查询服务器到服务器时，它是有效的。 此参数设置为 true 要求调用方拥有对该会话，服务器级访问权限，或调用方的 XUID 声明以与 Xbox 用户 ID 筛选器匹配。 |
| visibility| 字符串| 一个枚举值，该值指示在筛选结果中使用的可见性状态。 当前此参数可以仅将设置为打开包含打开的会话。 请参阅<b>MultiplayerSessionVisibility</b>。|
| version| 字符串| 一个正整数，该值指示会话主要版本或较低的会话包括。 值必须小于或等于请求的协定版本取模 100。|
| Take| 字符串| 一个正整数，该值指示会话的最大数目来检索。|

<a id="ID4EXB"></a>


## <a name="http-status-codes"></a>HTTP 状态代码
同样适用于 MPSD，服务将返回 HTTP 状态代码。  
<a id="ID4EAC"></a>


## <a name="request-body"></a>请求正文

此请求的正文中不发送的任何对象。

<a id="ID4ELC"></a>


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


<a id="ID4EWC"></a>


## <a name="see-also"></a>另请参阅

<a id="ID4EYC"></a>


##### <a name="parent"></a>Parent 的子磁盘）

[/serviceconfigs/{scid}/sessions](uri-serviceconfigsscidsessions.md)
