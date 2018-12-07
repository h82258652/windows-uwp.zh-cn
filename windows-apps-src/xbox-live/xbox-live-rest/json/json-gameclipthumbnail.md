---
title: GameClipThumbnail (JSON)
assetID: 3ed87fc1-734c-d8b5-d908-0ae3359769ed
permalink: en-us/docs/xboxlive/rest/json-gameclipthumbnail.html
description: " GameClipThumbnail (JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: a491b70b8e34c1c736667b50271af7b970b6bb2a
ms.sourcegitcommit: a3dc929858415b933943bba5aa7487ffa721899f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/07/2018
ms.locfileid: "8792955"
---
# <a name="gameclipthumbnail-json"></a>GameClipThumbnail (JSON)
包含与单个缩略图相关的信息。 可以有多个大小每个剪辑，并由客户端选择正确显示。 
<a id="ID4EN"></a>

 
## <a name="gameclipthumbnail"></a>GameClipThumbnail
 
GameClipThumbnail 对象具有以下规范。
 
| 成员| 类型| 描述| 
| --- | --- | --- | 
| <b>uri</b>| 字符串| 缩略图图像的 URI。| 
| <b>fileSize</b>| 32 位无符号的整数| 缩略图图像的总文件大小。| 
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

   