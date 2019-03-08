---
title: DELETE (/handles/{handleId})
assetID: 71cceff4-3a74-dc05-12db-cfe3f20a8aea
permalink: en-us/docs/xboxlive/rest/uri-handleshandleiddelete.html
description: " DELETE (/handles/{handleId})"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 354f3563c48139edc5d5cc041e8304998af55620
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2019
ms.locfileid: "57613402"
---
# <a name="delete-handleshandleid"></a>DELETE (/handles/{handleId})
删除句柄指定的句柄 id。

> [!IMPORTANT]
> 此方法由 2015年之多人游戏，并且仅适用于该多玩家版本和更高版本。 它旨在用于具有模板协定 104/105 或更高版本，并需要标头元素的 X Xbl 协定版本：104/105 或更高版本上的每个请求。

  * [备注](#ID4ET)
  * [URI 参数](#ID4EAB)
  * [HTTP 状态代码](#ID4ELB)
  * [请求正文](#ID4ESB)
  * [响应正文](#ID4E4B)

<a id="ID4ET"></a>


## <a name="remarks"></a>备注
此 HTTP/REST 方法删除句柄指定的 id，并清除用户的当前活动会话。 此方法的两端可加**Microsoft.Xbox.Services.Multiplayer.MultiplayerService.ClearActivityAsync**。  
<a id="ID4EAB"></a>


## <a name="uri-parameters"></a>URI 参数

| 参数| 在任务栏的搜索框中键入| 描述|
| --- | --- | --- | --- |
| handleId| GUID| 会话句柄的唯一 ID。|

<a id="ID4ELB"></a>


## <a name="http-status-codes"></a>HTTP 状态代码
同样适用于 MPSD，服务将返回 HTTP 状态代码。  
<a id="ID4ESB"></a>


## <a name="request-body"></a>请求正文

此请求的正文中不发送的任何对象。

<a id="ID4E4B"></a>


## <a name="response-body"></a>响应正文

响应正文中不发送任何对象。

<a id="ID4EIC"></a>


## <a name="see-also"></a>另请参阅

<a id="ID4EKC"></a>


##### <a name="parent"></a>Parent 的子磁盘）

[/handles/{handleId}](uri-handleshandleid.md)
