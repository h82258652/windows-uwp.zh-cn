---
title: 游戏 DVR URI
assetID: 472f705e-bf28-7894-b1ba-80933d8746a6
permalink: en-us/docs/xboxlive/rest/atoc-reference-dvr.html
description: " 游戏 DVR URI"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: b4bfd6e51efce4c6ec85db99a10a44a776dcb840
ms.sourcegitcommit: d2517e522cacc5240f7dffd5bc1eaa278e3f7768
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/03/2018
ms.locfileid: "8348637"
---
# <a name="game-dvr-uris"></a>游戏 DVR URI
 
从 Xbox Live 服务的*游戏 DVR*，本部分提供有关统一资源标识符 (URI) 地址和关联的超文本传输协议 (HTTP) 方法的详细信息。
 
仅主机可以录制游戏剪辑，但可以访问的任何设备可以显示剪裁。
 
根据问题的 URI 的函数，这些 Uri 的域是：
 
   *  gameclipsmetadata.xboxlive.com 
   *  gameclipstransfer.xboxlive.com 
  
<a id="ID4EZB"></a>

 
## <a name="in-this-section"></a>本部分内容

[/public/scids/{scid}/clips](uri-publicscidclips.md)

&nbsp;&nbsp;访问公开的剪辑。 实际上可以在两种形式，指定此 URI`/public/scids/{scid}/clips`和`/public/titles/{titleId}/clips`。 有关详细信息，请参阅下方。

[/{uri}](uri-uri.md)

&nbsp;&nbsp;访问游戏剪辑数据。

[/users/me/scids/{scid}/clips](uri-usersmescidclips.md)

&nbsp;&nbsp;访问初始上传请求。

[/users/me/scids/{scid}/clips/{gameClipId}](uri-usersmescidclipsgameclipid.md)

&nbsp;&nbsp;访问游戏剪辑数据和元数据。

[/users/{ownerId}/clips](uri-usersowneridclips.md)

&nbsp;&nbsp;访问权限的用户的剪辑的列表。

[/users/{ownerId}/scids/{scid}/clips/{gameClipId}](uri-usersowneridscidclipsgameclipid.md)

&nbsp;&nbsp;如果已知的所有 Id，以找到它，请从系统中访问单个游戏剪辑。
 
<a id="ID4EOC"></a>

 
## <a name="see-also"></a>另请参阅
 
<a id="ID4EQC"></a>

 
##### <a name="parent"></a>Parent 的子磁盘） 

[统一资源标识符 (URI) 参考](../atoc-xboxlivews-reference-uris.md)

   