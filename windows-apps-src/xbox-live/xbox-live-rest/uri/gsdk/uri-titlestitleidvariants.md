---
title: /titles/{titleId}/variants
assetID: bca30c8f-1f09-729f-4955-38b7809404eb
permalink: en-us/docs/xboxlive/rest/uri-titlestitleidvariants.html
author: KevinAsgari
description: " /titles/{titleId}/variants"
ms.author: kevinasg
ms.date: 20-12-2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 9a11cf42c068883368db159e5cf679e4f38755ec
ms.sourcegitcommit: 9e2c34a5ed3134aeca7eb9490f05b20eb9a3e5df
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/17/2018
ms.locfileid: "3984029"
---
# <a name="titlestitleidvariants"></a>/titles/{titleId}/variants
调用由客户端的游戏获取可用的变体的 URI。 有关这些 Uri 域是`gameserverds.xboxlive.com`和`gameserverms.xboxlive.com`。
 
  * [URI 参数](#ID4EU)
  * [主机名](#ID4EIB)
  * [有效的方法](#ID4EPB)
 
<a id="ID4EU"></a>

 
## <a name="uri-parameters"></a>URI 参数
 
| 参数| 说明| 
| --- | --- | 
| titleid| 游戏应在其中操作该请求 ID。| 
  
<a id="ID4EIB"></a>

 
## <a name="host-name"></a>主机名
 
gameserverds.xboxlive.com
  
<a id="ID4EPB"></a>

 
## <a name="valid-methods"></a>有效的方法
  
[POST](uri-titlestitleidvariants-post.md)
 
&nbsp;&nbsp;URI 由客户端检索列表的游戏的变体，为指定的游戏 id。
   