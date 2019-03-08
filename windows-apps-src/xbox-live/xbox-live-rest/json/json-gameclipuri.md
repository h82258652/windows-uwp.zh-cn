---
title: GameClipUri (JSON)
assetID: 03c097e8-7f29-1026-7a77-5c785b8511e9
permalink: en-us/docs/xboxlive/rest/json-gameclipuri.html
description: " GameClipUri (JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 1c74d0831c3b841ad2a1366bd2e03fb8a9b0448d
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2019
ms.locfileid: "57623492"
---
# <a name="gameclipuri-json"></a>GameClipUri (JSON)
 
<a id="ID4EO"></a>

 
## <a name="gameclipuri"></a>GameClipUri
 
GameClipUri 对象具有以下规范。
 
| 成员| 在任务栏的搜索框中键入| 描述| 
| --- | --- | --- | 
| <b>uri</b>| 字符串| 视频资产位置 URI。| 
| <b>fileSize</b>| 32 位无符号的整数| 总文件大小的缩略图。| 
| <b>uriType</b>| GameClipUriType| URI 的类型。| 
| <b>expiration</b>| DateTime| 此响应中包括的 uri 的过期时间。 如果 URL 为空或被认为在播放前到期，调用方应调用 RefreshUrl API。| 
  
<a id="ID4EMC"></a>

 
## <a name="sample-json-syntax"></a>示例 JSON 语法
 

```json
{
         "uri": "https://gameclips.xbox.com/clips/7ce5c1a7-1255-46d3-a90e-34a0e2dfab06/clip.mp4",
         "fileSize": 1234565,
         "uriType": "Download",
         "expiration": "9999-12-31T23:59:59.9999999"
       }
    
```

  
<a id="ID4EVC"></a>

 
## <a name="see-also"></a>另请参阅
 
<a id="ID4EXC"></a>

 
##### <a name="parent"></a>Parent 的子磁盘） 

[JavaScript 对象表示法 (JSON) 对象引用](atoc-xboxlivews-reference-json.md)

   