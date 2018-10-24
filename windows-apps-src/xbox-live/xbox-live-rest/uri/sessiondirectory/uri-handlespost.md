---
title: POST (/handles)
assetID: 21f3e289-0b0e-2731-befb-bd4c0d71973e
permalink: en-us/docs/xboxlive/rest/uri-handlespost.html
author: KevinAsgari
description: " POST (/handles)"
ms.author: kevinasg
ms.date: 20-12-2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 4eaddef523fcfa3b794c421acbe6c1aac4785b68
ms.sourcegitcommit: 4b97117d3aff38db89d560502a3c372f12bb6ed5
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/24/2018
ms.locfileid: "5434019"
---
# <a name="post-handles"></a>POST (/handles)
设置用户的当前活动，多人游戏会话，并邀请会话成员，如果需要。

> [!IMPORTANT]
> 此方法使用 2015年多人游戏和应用仅向该多人游戏版本及更高版本。 它旨在用于使用模板合约 104/105 或更高版本，并且需要 X Xbl 协定版本的标头元素： 104/105 或更高版本上每个请求。

  * [备注](#ID4ET)
  * [URI 参数](#ID4EHB)
  * [HTTP 状态代码](#ID4EPB)
  * [请求正文](#ID4EVB)
  * [响应正文](#ID4EJC)

<a id="ID4ET"></a>


## <a name="remarks"></a>备注

此 HTTP/REST 方法可用于设置当前活动会话。 在此情况下，该方法可以通过**Microsoft.Xbox.Services.Multiplayer.MultiplayerService.SetActivityAsync**换行。 请求正文必须定义在 JSON 文件中，为"活动"类型字段使用**sessionRef**对象的会话引用。 检索没有响应正文。 会话引用中指定的项目的定义，请参阅**Microsoft.Xbox.Services.Multiplayer.MultiplayerSessionReference**。

此 POST 方法还用于邀请用户指定的会话句柄。 在此情况下，该方法可以通过**Microsoft.Xbox.Services.Multiplayer.MultiplayerService.SendInvitesAsync**换行。 此，请使用的 POST 方法需要你请求正文定义的会话引用，但它具有类型字段设置为"邀请"。 响应正文是邀请句柄。

<a id="ID4EHB"></a>


## <a name="uri-parameters"></a>URI 参数

无

<a id="ID4EPB"></a>


## <a name="http-status-codes"></a>HTTP 状态代码
该服务返回 HTTP 状态代码，因为它适用于 MPSD。  
<a id="ID4EVB"></a>


## <a name="request-body"></a>请求正文

<a id="ID4E1B"></a>


### <a name="request-body-for-setting-activity"></a>请求正文用于设置活动


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


### <a name="request-body-for-sending-invites"></a>请求正文用于发送邀请


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


### <a name="response-body-for-sending-invites"></a>响应正文用于发送邀请
邀请句柄。   
<a id="ID4EXC"></a>


## <a name="see-also"></a>另请参阅

<a id="ID4EZC"></a>


##### <a name="parent"></a>Parent 的子磁盘）

[/handles](uri-handles.md)
