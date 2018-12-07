---
title: GET (/serviceconfigs/{scid}/sessiontemplates/{sessionTemplateName}/sessions)
assetID: 9daac964-0b25-3430-fcfd-0f8658aceee1
permalink: en-us/docs/xboxlive/rest/uri-serviceconfigsscidsessiontemplatessessiontemplatenamesessionsget.html
description: " GET (/serviceconfigs/{scid}/sessiontemplates/{sessionTemplateName}/sessions)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 1020c9d9c378a95070a7b0bf3faeb9d2c6751d51
ms.sourcegitcommit: d7613c791107f74b6a3dc12a372d9de916c0454b
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/06/2018
ms.locfileid: "8740880"
---
# <a name="get-serviceconfigsscidsessiontemplatessessiontemplatenamesessions"></a>GET (/serviceconfigs/{scid}/sessiontemplates/{sessionTemplateName}/sessions)
检索会话模板文档。

> [!IMPORTANT]
> 此 URI 方法需要 X Xbl 协定版本的标头元素： 104/105 或更高版本上的每个请求。

  * [备注](#ID4ET)
  * [URI 参数](#ID4EKB)
  * [HTTP 状态代码](#ID4EXB)
  * [请求正文](#ID4EAC)
  * [响应正文](#ID4EKC)

<a id="ID4ET"></a>


## <a name="remarks"></a>备注

此 HTTP/REST 方法检索会话模板所提供的筛选器的信息。 此方法可以由**Microsoft.Xbox.Services.Multiplayer.MultiplayerService.GetSessionsAsync**包装。


> [!NOTE] 
> 为 2015年多人游戏，由<b>Microsoft.Xbox.Services.Multiplayer.MultiplayerService.GetSessionsForUsersFilterAsync</b>调用此方法。  



> [!NOTE] 
> 每次调用此方法必须包括关键字、 Xbox 用户 ID 筛选器或两者。 如果调用方不具有正确的<i>私钥</i>和<i>预订</i>参数的权限，该方法返回 403 禁止的错误代码确实存在任何此类会话。  


<a id="ID4EKB"></a>


## <a name="uri-parameters"></a>URI 参数

| 参数| 类型| 描述|
| --- | --- | --- | --- | --- | --- |
| scid| GUID| 服务配置标识符 (SCID)。 第 1 部分会话的 id。|
| 关键字| 字符串| 用于筛选结果以标识与该字符串的只是会话的关键字。|
| xuid| GUID| 要检索会话为其用户的 Xbox 用户 Id。 用户必须在会话中处于活动状态。 |
| 预订| 字符串| 值，该值指示如果会话列表包括用户具有不接受。 仅可以将此参数设置为 true。 此设置需要调用方拥有对该会话，服务器级访问权限或调用方的 XUID 声明以匹配的 Xbox 用户 ID 筛选器。 |
| 处于非活动状态| 字符串| 值，该值指示会话列表包括用户已接受，但不会主动玩游戏。 仅可以将此参数设置为 true。 |
| 专用| 字符串| 值，该值指示会话列表是否包含专用会话。 仅可以将此参数设置为 true。 仅在查询你自己的会话，或进行服务器到服务器查询时，它是有效。 将此参数设置为 true 要求调用方拥有对会话，服务器级访问权限或调用方的 XUID 声明以匹配的 Xbox 用户 ID 筛选器。 |
| 可见性| 字符串| 指示用于筛选结果的可见性状态的枚举值。 当前此参数可以仅设置为打开包含打开的会话。 请参阅<b>MultiplayerSessionVisibility</b>。 |
| version| 字符串| 正整数，该值的主要会话版本或较低的会话以包括。 值必须小于或等于 100 模请求的协定版本。 |
| 参加| 字符串| 正整数指示会话最大数量来检索。|

<a id="ID4EXB"></a>


## <a name="http-status-codes"></a>HTTP 状态代码
该服务返回 HTTP 状态代码，因为它适用于 MPSD。  
<a id="ID4EAC"></a>


## <a name="request-body"></a>请求正文

此请求的正文中不发送任何对象。

<a id="ID4EKC"></a>


## <a name="response-body"></a>响应正文

此方法的返回是会话的引用，内含某些会话数据包含以内联方式的 JSON 数组。


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


<a id="ID4EUC"></a>


## <a name="see-also"></a>另请参阅

<a id="ID4EWC"></a>


##### <a name="parent"></a>Parent 的子磁盘）

[/serviceconfigs/{scid}/sessiontemplates/{sessionTemplateName}/sessions](uri-serviceconfigsscidsessiontemplatessessiontemplatenamesessions.md)
