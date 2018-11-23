---
title: InitialUploadResponse (JSON)
assetID: 6abb7d37-2c35-2cc3-d9e5-eff695235262
permalink: en-us/docs/xboxlive/rest/json-initialuploadresponse.html
author: KevinAsgari
description: " InitialUploadResponse (JSON)"
ms.author: kevinasg
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 24dc0b140991b7aab27472e291646da6f564a789
ms.sourcegitcommit: 93c0a60cf531c7d9fe7b00e7cf78df86906f9d6e
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/23/2018
ms.locfileid: "7576940"
---
# <a name="initialuploadresponse-json"></a>InitialUploadResponse (JSON)
 
<a id="ID4EO"></a>

 
## <a name="initialuploadresponse"></a>InitialUploadResponse
 
InitialUploadResponse 对象具有以下规范。
 
| 成员| 类型| 说明| 
| --- | --- | --- | 
| <b>gameClipId</b>| 字符串| 上传数据请求的分配的 ID。| 
| <b>uploadUri</b>| URI| 游戏剪辑应上传到的位置。| 
| <b>largeThumbnailUri</b>| URI| 可选。 较大的缩略图应上传到的位置。 此字段存在由<b>InitialUploadRequest</b> （当将会出现指定上传） 中的[ThumbnailSource 枚举](../enums/gvr-enum-thumbnailsource.md)值确定。| 
| <b>smallThumbnailUri</b>| URI| 可选。 较小缩略图应上传到的位置。 此字段存在由<b>InitialUploadRequest</b> （当将会出现指定上传） 中的[ThumbnailSource 枚举](../enums/gvr-enum-thumbnailsource.md)值确定。| 
  
<a id="ID4EYC"></a>

 
## <a name="sample-json-syntax"></a>JSON 语法示例
 

```json
{
   "gameClipId": "6b364924-5650-480f-86a7-fc002a1ee752"  ,  
   "uploadUri": "https://gameclips.xbox.live/upload/xuid(2716903703773872)/6b364924-5650-480f-86a7-fc002a1ee752/container",
   "largeThumbnailUri": "https://gameclips.xbox.live/upload/xuid(2716903703773872)/6b364924-5650-480f-86a7-fc002a1ee752/container/thumbnails/large",
   "smallThumbnailUri": "https://gameclips.xbox.live/upload/xuid(2716903703773872)/6b364924-5650-480f-86a7-fc002a1ee752/container/thumbnails/small"
 }
    
```

  
<a id="ID4EBD"></a>

 
## <a name="see-also"></a>另请参阅
 
<a id="ID4EDD"></a>

 
##### <a name="parent"></a>Parent 的子磁盘） 

[JavaScript 对象表示法 (JSON) 对象参考](atoc-xboxlivews-reference-json.md)

   