---
title: GameClipThumbnail (JSON)
assetID: 3ed87fc1-734c-d8b5-d908-0ae3359769ed
permalink: en-us/docs/xboxlive/rest/json-gameclipthumbnail.html
author: KevinAsgari
description: " GameClipThumbnail (JSON)"
ms.author: kevinasg
ms.date: 20-12-2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 681a269cd861f741e2bbde3554acc1b25104d90d
ms.sourcegitcommit: 194ab5aa395226580753869c6b66fce88be83522
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2018
ms.locfileid: "4152685"
---
# <a name="gameclipthumbnail-json"></a>GameClipThumbnail (JSON)
包含一个单独的缩略图的相关的信息。 可以有多个大小每个剪辑，并由客户端选择正确显示。 
<a id="ID4EN"></a>

 
## <a name="gameclipthumbnail"></a>GameClipThumbnail
 
GameClipThumbnail 对象具有以下规范。
 
| 成员| 类型| 说明| 
| --- | --- | --- | 
| <b>uri</b>| 字符串| 缩略图图像的 URI。| 
| <b>文件大小</b>| 32 位无符号的整数| 缩略图图像的总文件大小。| 
| <b>thumbnailType</b>| ThumbnailType| 缩略图图像的类型。| 
  
<a id="ID4EAC"></a>

 
## <a name="sample-json-syntax"></a>JSON 语法示例
 

```json
{
         "uri": "http://gameclips.xbox.com/thumbnails/7ce5c1a7-1255-46d3-a90e-34a0e2dfab06/small.jpg",
         "fileSize": 123,
         "width": 120,
         "height": 250
       }
    
```

  
<a id="ID4EJC"></a>

 
## <a name="see-also"></a>另请参阅
 
<a id="ID4ELC"></a>

 
##### <a name="parent"></a>Parent 的子磁盘） 

[JavaScript 对象表示法 (JSON) 对象参考](atoc-xboxlivews-reference-json.md)

   