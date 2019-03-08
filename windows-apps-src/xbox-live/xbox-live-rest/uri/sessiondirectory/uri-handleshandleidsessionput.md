---
title: PUT (/handles/{handle-id}/session)
assetID: d8a3d473-1192-ec0c-3935-c301f4f61e03
permalink: en-us/docs/xboxlive/rest/uri-handleshandleidsessionput.html
description: " PUT (/handles/{handle-id}/session)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 3a1872857d8b8e692f67e3c7b2a067ae86663c00
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2019
ms.locfileid: "57641602"
---
# <a name="put-handleshandle-idsession"></a>PUT (/handles/{handle-id}/session)
创建或更新会话的取消引用句柄。

> [!IMPORTANT]
> 此方法由 2015年之多人游戏，并且仅适用于该多玩家版本和更高版本。 它旨在用于具有模板协定 104/105 或更高版本，并需要标头元素的 X Xbl 协定版本：104/105 或更高版本上的每个请求。

  * [备注](#ID4ET)
  * [URI 参数](#ID4ECB)
  * [HTTP 状态代码](#ID4ENB)
  * [请求正文](#ID4EUB)
  * [响应正文](#ID4E6B)

<a id="ID4ET"></a>


## <a name="remarks"></a>备注

此 HTTP/REST 方法将新的或更新会话写入到多玩家服务，使用提供的会话句柄 id。 结果是一个表示新的或更新会话，如从服务器返回的对象。 此方法的两端可加**Microsoft.Xbox.Services.Multiplayer.MultiplayerService.WriteSessionByHandleAsync**。

此方法的调用方的播放器从获取句柄 ID **MultiplayerActivityDetails**对象。 或者，调用方获取的 ID 从协议激活后用户已接受游戏的邀请。

<a id="ID4ECB"></a>


## <a name="uri-parameters"></a>URI 参数

| 参数| 在任务栏的搜索框中键入| 描述|
| --- | --- | --- | --- |
| handleId| GUID| 会话句柄的唯一 ID。|

<a id="ID4ENB"></a>


## <a name="http-status-codes"></a>HTTP 状态代码
同样适用于 MPSD，服务将返回 HTTP 状态代码。  
<a id="ID4EUB"></a>


## <a name="request-body"></a>请求正文

此请求的正文中不发送的任何对象。

<a id="ID4E6B"></a>


## <a name="response-body"></a>响应正文

响应正文中不发送任何对象。

<a id="ID4EKC"></a>


## <a name="see-also"></a>另请参阅

<a id="ID4EMC"></a>


##### <a name="parent"></a>Parent 的子磁盘）

[/handles/{handleId}/session](uri-handleshandleidsession.md)
