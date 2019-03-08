---
title: GET (/handles/{handle-id})
assetID: c95b5ab5-d56a-f70d-20d8-afb48d122ccd
permalink: en-us/docs/xboxlive/rest/uri-handleshandleidget.html
description: " GET (/handles/{handle-id})"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 501d36f4d1ac079af15d6bb7f35a90d5328fc8db
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2019
ms.locfileid: "57598462"
---
# <a name="get-handleshandle-id"></a>GET (/handles/{handle-id})
检索句柄指定的句柄 id。

> [!IMPORTANT]
> 此方法由 2015年之多人游戏，并且仅适用于该多玩家版本和更高版本。 它旨在用于具有模板协定 104/105 或更高版本，并需要标头元素的 X Xbl 协定版本：104/105 或更高版本上的每个请求。

  * [备注](#ID4ET)
  * [URI 参数](#ID4EDB)
  * [HTTP 状态代码](#ID4EOB)
  * [请求正文](#ID4EUB)
  * [响应正文](#ID4E5B)

<a id="ID4ET"></a>


## <a name="remarks"></a>备注

此 HTTP/REST 方法获取用户的当前活动会话，用于指定句柄。 返回是会话对象，包含所有属性。 此方法的两端可加**Microsoft.Xbox.Services.Multiplayer.MultiplayerService.GetCurrentSessionByHandleAsync**。

此方法的调用方的播放器从获取句柄 ID **MultiplayerActivityDetails**对象。 或者，调用方获取的 ID 从协议激活后用户已接受游戏的邀请。

<a id="ID4EDB"></a>


## <a name="uri-parameters"></a>URI 参数

| 参数| 在任务栏的搜索框中键入| 描述|
| --- | --- | --- | --- |
| handleId| GUID| 会话句柄的唯一 ID。|

<a id="ID4EOB"></a>


## <a name="http-status-codes"></a>HTTP 状态代码
同样适用于 MPSD，服务将返回 HTTP 状态代码。  
<a id="ID4EUB"></a>


## <a name="request-body"></a>请求正文

此请求的正文中不发送的任何对象。

<a id="ID4E5B"></a>


## <a name="response-body"></a>响应正文
请参阅中的响应结构[MultiplayerSession (JSON)](../../json/json-multiplayersession.md)。  
<a id="ID4EKC"></a>


## <a name="see-also"></a>另请参阅

<a id="ID4EMC"></a>


##### <a name="parent"></a>Parent 的子磁盘）

[/handles/{handleId}](uri-handleshandleid.md)
