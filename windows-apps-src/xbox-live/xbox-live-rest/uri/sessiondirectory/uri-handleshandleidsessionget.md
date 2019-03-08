---
title: GET (/handles/{handleId}/session)
assetID: 1f22954c-e77b-69c2-63f4-741fbd965f8f
permalink: en-us/docs/xboxlive/rest/uri-handleshandleidsessionget.html
description: " GET (/handles/{handleId}/session)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 41911dc53b316f4f323b9859d9101581ec88e497
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2019
ms.locfileid: "57593892"
---
# <a name="get-handleshandleidsession"></a>GET (/handles/{handleId}/session)
获取会话对象的指定句柄标识符。

> [!IMPORTANT]
> 此方法由 2015年之多人游戏，并且仅适用于该多玩家版本和更高版本。 它旨在用于具有模板协定 104/105 或更高版本，并需要标头元素的 X Xbl 协定版本：104/105 或更高版本上的每个请求。

  * [备注](#ID4ET)
  * [URI 参数](#ID4EDB)
  * [HTTP 状态代码](#ID4EOB)
  * [请求正文](#ID4EVB)
  * [响应正文](#ID4E6B)

<a id="ID4ET"></a>


## <a name="remarks"></a>备注

此 HTTP/REST 方法从服务器中，使用提供的服务端将指向会话 （句柄） 检索会话对象。 返回是会话对象，包含所有属性。 此方法的两端可加**Microsoft.Xbox.Services.Multiplayer.MultiplayerService.GetCurrentSessionByHandleAsync**。

此方法的调用方的播放器从获取句柄 ID **MultiplayerActivityDetails**对象。 或者，调用方获取的 ID 从协议激活后用户已接受游戏的邀请。

<a id="ID4EDB"></a>


## <a name="uri-parameters"></a>URI 参数

| 参数| 在任务栏的搜索框中键入| 描述|
| --- | --- | --- | --- |
| handleId| GUID| 会话句柄的唯一 ID。|

<a id="ID4EOB"></a>


## <a name="http-status-codes"></a>HTTP 状态代码
同样适用于 MPSD，服务将返回 HTTP 状态代码。  
<a id="ID4EVB"></a>


## <a name="request-body"></a>请求正文

此请求的正文中不发送的任何对象。

<a id="ID4E6B"></a>


## <a name="response-body"></a>响应正文
请参阅中的响应结构[MultiplayerSession (JSON)](../../json/json-multiplayersession.md)。  
<a id="ID4EIC"></a>


## <a name="see-also"></a>另请参阅

<a id="ID4EKC"></a>


##### <a name="parent"></a>Parent 的子磁盘）

[/handles/{handleId}/session](uri-handleshandleidsession.md)
