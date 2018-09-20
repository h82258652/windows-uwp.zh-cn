---
title: InitialUploadResponse (JSON)
assetID: 6abb7d37-2c35-2cc3-d9e5-eff695235262
permalink: en-us/docs/xboxlive/rest/json-initialuploadresponse.html
author: KevinAsgari
description: " InitialUploadResponse (JSON)"
ms.author: kevinasg
ms.date: 20-12-2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 3a643775f835a87b4c1287b0954f698c4c987c10
ms.sourcegitcommit: 4f6dc806229a8226894c55ceb6d6eab391ec8ab6
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/20/2018
ms.locfileid: "4088505"
---
# <a name="initialuploadresponse-json"></a>InitialUploadResponse (JSON)
 
<a id="ID4EO"></a>

 
## <a name="initialuploadresponse"></a>InitialUploadResponse
 
InitialUploadResponse 对象具有以下规范。
 
| 成员| 类型| 说明| 
| --- | --- | --- | 
| <b>gameClipId</b>| 字符串| 上传数据请求的分配的 ID。| 
| <b>uploadUri</b>| URI| 游戏剪辑应可上传到的位置。| 
| <b>largeThumbnailUri</b>| URI| 可选。 较大的缩略图应可上传到的位置。 存在此字段中 （当将会出现在指定上载） <b>InitialUploadRequest</b> [ThumbnailSource 枚举](../enums/gvr-enum-thumbnailsource.md)值来确定。| 
| <b>smallThumbnailUri</b>| URI| 可选。 较小缩略图应可上传到的位置。 存在此字段中 （当将会出现在指定上载） <b>InitialUploadRequest</b> [ThumbnailSource 枚举](../enums/gvr-enum-thumbnailsource.md)值来确定。| 
  
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

   