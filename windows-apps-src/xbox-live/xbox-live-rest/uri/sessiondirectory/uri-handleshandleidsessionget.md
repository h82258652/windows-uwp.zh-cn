---
title: GET (/handles/{handleId}/session)
assetID: 1f22954c-e77b-69c2-63f4-741fbd965f8f
permalink: en-us/docs/xboxlive/rest/uri-handleshandleidsessionget.html
author: KevinAsgari
description: " GET (/handles/{handleId}/session)"
ms.author: kevinasg
ms.date: 20-12-2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 0ab3214ca9b2cb2ff8ace11706ceda22885598e1
ms.sourcegitcommit: 1938851dc132c60348f9722daf994b86f2ead09e
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/03/2018
ms.locfileid: "4261213"
---
# <a name="get-handleshandleidsession"></a>GET (/handles/{handleId}/session)
获取指定的句柄标识符会话对象。

> [!IMPORTANT]
> 此方法使用 2015年多人游戏和应用仅向该多人游戏版本及更高版本。 它旨在用于使用模板合约 104/105 或更高版本，并且需要 X Xbl 协定版本的标头元素： 104/105 或更高版本上每个请求。

  * [备注](#ID4ET)
  * [URI 参数](#ID4EDB)
  * [HTTP 状态代码](#ID4EOB)
  * [请求正文](#ID4EVB)
  * [响应正文](#ID4E6B)

<a id="ID4ET"></a>


## <a name="remarks"></a>备注

此 HTTP/REST 方法从服务器上，使用所提供的服务端指针对会话 （句柄） 检索会话对象。 返回是会话对象，使用所有属性。 此方法可以由**Microsoft.Xbox.Services.Multiplayer.MultiplayerService.GetCurrentSessionByHandleAsync**包装。

此方法的调用方从玩家的**MultiplayerActivityDetails**对象获得的句柄 ID。 或者，调用方获取的 ID 从协议激活后用户已接受游戏邀请。

<a id="ID4EDB"></a>


## <a name="uri-parameters"></a>URI 参数

| 参数| 类型| 描述|
| --- | --- | --- | --- |
| handleId| GUID| 会话句柄的唯一 ID。|

<a id="ID4EOB"></a>


## <a name="http-status-codes"></a>HTTP 状态代码
该服务返回 HTTP 状态代码，因为它适用于 MPSD。  
<a id="ID4EVB"></a>


## <a name="request-body"></a>请求正文

此请求的正文中不发送任何对象。

<a id="ID4E6B"></a>


## <a name="response-body"></a>响应正文
请参阅[MultiplayerSession (JSON)](../../json/json-multiplayersession.md)中的响应结构。  
<a id="ID4EIC"></a>


## <a name="see-also"></a>另请参阅

<a id="ID4EKC"></a>


##### <a name="parent"></a>Parent 的子磁盘）

[/handles/{handleId}/session](uri-handleshandleidsession.md)
