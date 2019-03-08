---
title: InitialUploadResponse (JSON)
assetID: 6abb7d37-2c35-2cc3-d9e5-eff695235262
permalink: en-us/docs/xboxlive/rest/json-initialuploadresponse.html
description: " InitialUploadResponse (JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: dab59fefb389cf550a1bc4fc6429f6b0970f50ab
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2019
ms.locfileid: "57589832"
---
# <a name="initialuploadresponse-json"></a>InitialUploadResponse (JSON)
 
<a id="ID4EO"></a>

 
## <a name="initialuploadresponse"></a>InitialUploadResponse
 
InitialUploadResponse 对象具有以下规范。
 
| 成员| 在任务栏的搜索框中键入| 描述| 
| --- | --- | --- | 
| <b>gameClipId</b>| 字符串| 分配的上传数据请求的 ID。| 
| <b>uploadUri</b>| URI| 游戏剪辑应上传到的位置。| 
| <b>largeThumbnailUri</b>| URI| 可选。 大型缩略图应上传到的位置。 此字段是否存在由[ThumbnailSource 枚举](../enums/gvr-enum-thumbnailsource.md)中的值<b>InitialUploadRequest</b> （将指定上传时存在）。| 
| <b>smallThumbnailUri</b>| URI| 可选。 小缩略图应上传到的位置。 此字段是否存在由[ThumbnailSource 枚举](../enums/gvr-enum-thumbnailsource.md)中的值<b>InitialUploadRequest</b> （将指定上传时存在）。| 
  
<a id="ID4EYC"></a>

 
## <a name="sample-json-syntax"></a>示例 JSON 语法
 

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

[JavaScript 对象表示法 (JSON) 对象引用](atoc-xboxlivews-reference-json.md)

   