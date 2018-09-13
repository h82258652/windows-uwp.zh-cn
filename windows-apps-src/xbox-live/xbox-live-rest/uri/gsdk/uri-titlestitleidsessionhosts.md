---
title: /titles/ {titleId} / sessionhosts
assetID: 92d9bdd2-5c8f-761b-3f9a-50f8db7b843c
permalink: en-us/docs/xboxlive/rest/uri-titlestitleidsessionhosts.html
author: KevinAsgari
description: " /titles/ {titleId} / sessionhosts"
ms.author: kevinasg
ms.date: 20-12-2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: a93e134dba9ce66b8b6b547308f926112f6a577f
ms.sourcegitcommit: c8f6866100a4b38fdda8394ea185b02d7af66411
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/13/2018
ms.locfileid: "3960578"
---
# <a name="titlestitleidsessionhosts"></a>/titles/ {titleId} / sessionhosts
请求 Xbox Live 计算 sessionhost 为给定的作品 id 分配。这些 Uri 的域是`gameserverds.xboxlive.com`和`gameserverms.xboxlive.com`。
 
  * [URI 参数](#ID4EU)
  * [主机名](#ID4EIB)
  * [有效的方法](#ID4EPB)
 
<a id="ID4EU"></a>

 
## <a name="uri-parameters"></a>URI 参数
 
| 参数| 描述| 
| --- | --- | 
| titleId| 游戏应在其中操作请求 ID。| 
  
<a id="ID4EIB"></a>

 
## <a name="host-name"></a>主机名
 
gameserverms.xboxlive.com
  
<a id="ID4EPB"></a>

 
## <a name="valid-methods"></a>有效的方法
  
[POST](uri-titlestitleidsessionhosts-post.md)
 
&nbsp;&nbsp;创建新群集请求。
   