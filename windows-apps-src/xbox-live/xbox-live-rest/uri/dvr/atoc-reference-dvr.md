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
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2019
ms.locfileid: "57617842"
---
# <a name="game-dvr-uris"></a>游戏 DVR URI
 
本部分提供有关通用资源标识符 (URI) 地址和关联的超文本传输协议 (HTTP) 方法的详细信息从 Xbox Live 服务中针对*游戏 DVR*。
 
仅控制台可以记录游戏的剪辑，但可以访问任何设备可以显示一个剪辑。
 
根据 URI 相关的函数，这些 Uri 的域是：
 
   *  gameclipsmetadata.xboxlive.com 
   *  gameclipstransfer.xboxlive.com 
  
<a id="ID4EZB"></a>

 
## <a name="in-this-section"></a>本部分内容

[/public/scids/{scid}/clips](uri-publicscidclips.md)

&nbsp;&nbsp;访问公共剪辑。 实际上可以在两个窗体中指定此 URI`/public/scids/{scid}/clips`和`/public/titles/{titleId}/clips`。 有关详细信息，请参阅下方。

[/{uri}](uri-uri.md)

&nbsp;&nbsp;访问游戏剪辑数据。

[/users/me/scids/{scid}/clips](uri-usersmescidclips.md)

&nbsp;&nbsp;访问一个初始上传请求。

[/users/me/scids/{scid}/clips/{gameClipId}](uri-usersmescidclipsgameclipid.md)

&nbsp;&nbsp;访问游戏剪辑数据和元数据。

[/users/{ownerId}/clips](uri-usersowneridclips.md)

&nbsp;&nbsp;访问用户的剪辑列表。

[/users/{ownerId}/scids/{scid}/clips/{gameClipId}](uri-usersowneridscidclipsgameclipid.md)

&nbsp;&nbsp;从系统访问单个游戏剪辑，如果已知的所有 Id，以找到它。
 
<a id="ID4EOC"></a>

 
## <a name="see-also"></a>另请参阅
 
<a id="ID4EQC"></a>

 
##### <a name="parent"></a>Parent 的子磁盘） 

[通用资源标识符 (URI) 引用](../atoc-xboxlivews-reference-uris.md)

   