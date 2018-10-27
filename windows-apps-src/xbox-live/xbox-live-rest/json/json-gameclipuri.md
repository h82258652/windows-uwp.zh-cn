---
title: GameClipUri (JSON)
assetID: 03c097e8-7f29-1026-7a77-5c785b8511e9
permalink: en-us/docs/xboxlive/rest/json-gameclipuri.html
author: KevinAsgari
description: " GameClipUri (JSON)"
ms.author: kevinasg
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: db92c3e405029d76264b0e1e9b159ae2650d6650
ms.sourcegitcommit: 086001cffaf436e6e4324761d59bcc5e598c15ea
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/27/2018
ms.locfileid: "5705526"
---
# <a name="gameclipuri-json"></a>GameClipUri (JSON)
 
<a id="ID4EO"></a>

 
## <a name="gameclipuri"></a>GameClipUri
 
GameClipUri 对象具有以下规范。
 
| 成员| 类型| 说明| 
| --- | --- | --- | 
| <b>uri</b>| 字符串| 为视频资产的位置 URI。| 
| <b>fileSize</b>| 32 位无符号的整数| 缩略图图像的总文件大小。| 
| <b>uriType</b>| GameClipUriType| URI 的类型。| 
| <b>到期</b>| DateTime| 此响应中包含的 URI 的到期时间。 如果 URL 为空，或者被视为过期前播放，调用方应调用 RefreshUrl API。| 
  
<a id="ID4EMC"></a>

 
## <a name="sample-json-syntax"></a>JSON 语法示例
 

```json
{
         "uri": "http://gameclips.xbox.com/clips/7ce5c1a7-1255-46d3-a90e-34a0e2dfab06/clip.mp4",
         "fileSize": 1234565,
         "uriType": "Download",
         "expiration": "9999-12-31T23:59:59.9999999"
       }
    
```

  
<a id="ID4EVC"></a>

 
## <a name="see-also"></a>另请参阅
 
<a id="ID4EXC"></a>

 
##### <a name="parent"></a>Parent 的子磁盘） 

[JavaScript 对象表示法 (JSON) 对象参考](atoc-xboxlivews-reference-json.md)

   