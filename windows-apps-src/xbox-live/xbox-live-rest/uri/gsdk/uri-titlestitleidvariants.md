---
title: /titles/{titleId}/variants
assetID: bca30c8f-1f09-729f-4955-38b7809404eb
permalink: en-us/docs/xboxlive/rest/uri-titlestitleidvariants.html
description: " /titles/{titleId}/variants"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 1a0a0df14b442babec363dfebc96a0c33935563e
ms.sourcegitcommit: b4c502d69a13340f6e3c887aa3c26ef2aeee9cee
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/03/2018
ms.locfileid: "8457580"
---
# <a name="titlestitleidvariants"></a>/titles/{titleId}/variants
调用由客户端的游戏获取可用的变体的 URI。 这些 Uri 的域是`gameserverds.xboxlive.com`和`gameserverms.xboxlive.com`。
 
  * [URI 参数](#ID4EU)
  * [主机名](#ID4EIB)
  * [有效的方法](#ID4EPB)
 
<a id="ID4EU"></a>

 
## <a name="uri-parameters"></a>URI 参数
 
| 参数| 描述| 
| --- | --- | 
| titleid| 游戏应在其中操作该请求 ID。| 
  
<a id="ID4EIB"></a>

 
## <a name="host-name"></a>主机名
 
gameserverds.xboxlive.com
  
<a id="ID4EPB"></a>

 
## <a name="valid-methods"></a>有效的方法
  
[POST](uri-titlestitleidvariants-post.md)
 
&nbsp;&nbsp;URI 由客户端检索列表的游戏的变体，为指定的游戏 id。
   