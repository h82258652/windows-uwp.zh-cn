---
title: / 用户/me/scid / {scid} /clips/ {gameClipId}
assetID: f5bead69-4fc9-f551-39cb-c8754645ac88
permalink: en-us/docs/xboxlive/rest/uri-usersmescidclipsgameclipid.html
author: KevinAsgari
description: " / 用户/me/scid / {scid} /clips/ {gameClipId}"
ms.author: kevinasg
ms.date: 20-12-2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 6b5d33a8bb8b0f21ea05ac22a7d15ccb4b160b9a
ms.sourcegitcommit: c8f6866100a4b38fdda8394ea185b02d7af66411
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/13/2018
ms.locfileid: "3956474"
---
# <a name="usersmescidsscidclipsgameclipid"></a>/ 用户/me/scid / {scid} /clips/ {gameClipId}
访问游戏剪辑数据和元数据。 这些 Uri 的域是`gameclipsmetadata.xboxlive.com`和`gameclipstransfer.xboxlive.com`，则根据问题的 URI 的函数。
 
  * [URI 参数](#ID4EX)
 
<a id="ID4EX"></a>

 
## <a name="uri-parameters"></a>URI 参数
 
| 参数| 类型| 说明| 
| --- | --- | --- | 
| scid| 字符串| 正在访问的资源的服务配置 ID。 必须匹配的身份验证的用户的 SCID。| 
| gameClipId| 字符串| 正在访问的资源 GameClip ID。| 
  
<a id="ID4E3B"></a>

 
## <a name="valid-methods"></a>有效的方法

[删除 (/ 用户/me/scid / {scid} /clips/ {gameClipId})](uri-usersmescidclipsgameclipiddelete.md)

&nbsp;&nbsp;删除游戏剪辑

[POST (/ 用户/me/scid / {scid} /clips/ {gameClipId})](uri-usersmescidclipsgameclipidpost.md)

&nbsp;&nbsp;更新用户自己的数据的游戏剪辑元数据。
 
<a id="ID4EJC"></a>

 
## <a name="see-also"></a>另请参阅
 
<a id="ID4ELC"></a>

 
##### <a name="parent"></a>Parent 的子磁盘） 

[游戏 DVR Uri](atoc-reference-dvr.md)

   