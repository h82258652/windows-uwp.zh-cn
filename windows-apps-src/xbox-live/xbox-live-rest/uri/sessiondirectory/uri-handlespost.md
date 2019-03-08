---
title: POST (/handles)
assetID: 21f3e289-0b0e-2731-befb-bd4c0d71973e
permalink: en-us/docs/xboxlive/rest/uri-handlespost.html
description: " POST (/handles)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: ed3482b8e629749d294ed25944db16372cc7fee6
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2019
ms.locfileid: "57594742"
---
# <a name="post-handles"></a>POST (/handles)
设置用户的当前活动的多玩家会话，并根据需要会话成员。

> [!IMPORTANT]
> 此方法由 2015年之多人游戏，并且仅适用于该多玩家版本和更高版本。 它旨在用于具有模板协定 104/105 或更高版本，并需要标头元素的 X Xbl 协定版本：104/105 或更高版本上的每个请求。

  * [备注](#ID4ET)
  * [URI 参数](#ID4EHB)
  * [HTTP 状态代码](#ID4EPB)
  * [请求正文](#ID4EVB)
  * [响应正文](#ID4EJC)

<a id="ID4ET"></a>


## <a name="remarks"></a>备注

此 HTTP/REST 方法可以用于设置当前活动会话。 在这种情况下，该方法可以被包装**Microsoft.Xbox.Services.Multiplayer.MultiplayerService.SetActivityAsync**。 请求正文必须定义会话使用的引用**sessionRef**在 JSON 文件中，使用"活动"的类型字段的对象。 不检索任何响应正文。 中的会话引用指定的项的定义，请参阅**Microsoft.Xbox.Services.Multiplayer.MultiplayerSessionReference**。

此 POST 方法还可以用于邀请到会话句柄所指定的用户。 在这种情况下，该方法可以被包装**Microsoft.Xbox.Services.Multiplayer.MultiplayerService.SendInvitesAsync**。 POST 方法的这种用法需要请求正文来定义的会话引用，但与类型字段设置为"邀请"。 响应正文是邀请句柄。

<a id="ID4EHB"></a>


## <a name="uri-parameters"></a>URI 参数

无

<a id="ID4EPB"></a>


## <a name="http-status-codes"></a>HTTP 状态代码
同样适用于 MPSD，服务将返回 HTTP 状态代码。  
<a id="ID4EVB"></a>


## <a name="request-body"></a>请求正文

<a id="ID4E1B"></a>


### <a name="request-body-for-setting-activity"></a>请求正文来设置活动


```cpp
{
  "version": 1,
  "type": "activity",
  "sessionRef": {
    "scid": "bd6c41c3-01c3-468a-a3b5-3e0fe8133862",
    "templateName": "deathmatch",
    // The session name is optional in a POST; if not specified, MPSD fills in a GUID.//
    "name": "session-49"
  },
}

```


<a id="ID4EBC"></a>


### <a name="request-body-for-sending-invites"></a>请求正文发送邀请


```cpp
{
  // Common handle fields
  "id: "47ca0049-a5ba-4bc1-aa22-fcf834ce4c13",
  "version": 1,
  "type": "invite",
  "sessionRef": {
    "scid": "bd6c41c3-01c3-468a-a3b5-3e0fe8133862",
    "templateName": "deathmatch",
    "name": "session-49"
   },
   "inviteAttributes": {
     "titleId" : {titleId}, // The title being invited to, in decimal uint32. This value is used to find the title name and/or image.
     "context" : {context}, // The title defined context token. Must be 256 characters or less when URI-encoded.
     "contextString" : {contextstring}, // The string name of a custom invite string to display in the invite notification.
     "senderString" : {sender} // The string name of the sender when the sender is a service.
   },
   "invitedXuid": "3210",
}

```


<a id="ID4EJC"></a>


## <a name="response-body"></a>响应正文

<a id="ID4EOC"></a>


### <a name="response-body-for-setting-activity"></a>用于设置活动的响应正文
无。  
<a id="ID4ESC"></a>


### <a name="response-body-for-sending-invites"></a>发送邀请的响应正文
邀请句柄。   
<a id="ID4EXC"></a>


## <a name="see-also"></a>另请参阅

<a id="ID4EZC"></a>


##### <a name="parent"></a>Parent 的子磁盘）

[/handles](uri-handles.md)
