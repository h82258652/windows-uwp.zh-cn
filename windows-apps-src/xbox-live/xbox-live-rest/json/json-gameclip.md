---
title: GameClip (JSON)
assetID: 204cb702-4ce4-85a8-f231-3b4fb243405f
permalink: en-us/docs/xboxlive/rest/json-gameclip.html
author: KevinAsgari
description: " GameClip (JSON)"
ms.author: kevinasg
ms.date: 20-12-2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 047f7287578f52591c48ee059e72efb559b41c87
ms.sourcegitcommit: 72835733ec429a5deb6a11da4112336746e5e9cf
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/21/2018
ms.locfileid: "5160351"
---
# <a name="gameclip-json"></a>GameClip (JSON)
 
<a id="ID4EO"></a>

 
## <a name="gameclip"></a>GameClip
 
GameClip 对象具有以下规范。
 
| 成员| 类型| 说明| 
| --- | --- | --- | 
| <b>gameClipId</b>| 字符串| 分配到游戏剪辑的 ID。| 
| <b>状态</b>| GameClipState| 在系统中的游戏剪辑的状态。| 
| <b>dateRecorded</b>| DateTime| 日期和时间开始录制，采用 UTC （ISO 8601 格式）。| 
| <b>lastModified</b>| DateTime| 上次修改时间的游戏剪辑或其元数据，采用 UTC （ISO 8601 格式）。| 
| <b>userCaption</b>| 字符串| 用户输入非本地化字符串的游戏剪辑。| 
| <b>type</b>| GameClipTypes| 剪辑的类型。 可以是多个值，并将逗号分隔的如果是这样。| 
| <b>源</b>| GameClipSource| 如何确定该剪辑的源。| 
| <b>可见性</b>| GameClipVisibility| 游戏剪辑后在系统中发布的可见性。| 
| <b>durationInSeconds</b>| 32 位无符号的整数| 持续时间的游戏剪辑以秒为单位。| 
| <b>scid</b>| 字符串| 到游戏剪辑相关联的 SCID。| 
| <b>rating</b>| 双精度浮点数| 与游戏剪辑，0.0 到 5.0 的范围内评分。| 
| <b>ratingCount</b>| 32 位无符号的整数| 此代码片段进行分级次数。| 
| <b>视图</b>| 32 位无符号的整数| 游戏剪辑关联的视图数。| 
| <b>titleData</b>| 字符串| 特定于游戏的属性包中。| 
| <b>titleData</b>| 字符串| 特定于控制台的属性包中。| 
| <b>缩略图</b>| GameClipThumbnail 的数组| GameClipThumbnail 对象数组。| 
| <b>gameClipUris</b>| GameClipUri 的数组| GameClipUri 对象数组。| 
| <b>xuid</b>| 字符串| 游戏剪辑，作为字符串封送的所有者的 XUID。| 
| <b>clipName</b>| 字符串| 该剪辑的名称，基于将请求作为输入区域设置的本地化的版本从标题管理系统中查找。| 
  
<a id="ID4ERH"></a>

 
## <a name="sample-json-syntax"></a>JSON 语法示例
 

```json
{
     "id": "7ce5c1a7-1255-46d3-a90e-34a0e2dfab06",
     "xuid": "2716903703773872",
     "state": "Published", 
     "dateRecorded": "2012-12-23T12:00:00Z",
     "lastModified": "2012-10-31T10:45:00Z",
     "clipName": "Forza 4",
     "userCaption": "My awesome car flip",
     "type": "DeveloperInitiated, Achievement",
     "source": "TitleDirect",
     "visibility": "Default",
     "durationInSeconds": 30,
     "scid": "ecb5497e-76d4-4a8a-870d-e76a26306b7d",
     "rating": 1.0,
     "views": 5,
     "thumbnails": [
       {
         "uri": "http://gameclips.xbox.com/thumbnails/7ce5c1a7-1255-46d3-a90e-34a0e2dfab06/small.jpg",
         "fileSize": 123,
         "width": 120,
         "height": 250
       }
     ],
     "gameClipUris": [
       {
         "uri": "http://gameclips.xbox.com/clips/7ce5c1a7-1255-46d3-a90e-34a0e2dfab06/clip.mp4",
         "fileSize": 1234565,
         "uriType": "Download",
         "expiration": "9999-12-31T23:59:59.9999999"
       }
     ]
   }
    
```

  
<a id="ID4E1H"></a>

 
## <a name="see-also"></a>另请参阅
 
<a id="ID4E3H"></a>

 
##### <a name="parent"></a>Parent 的子磁盘） 

[JavaScript 对象表示法 (JSON) 对象参考](atoc-xboxlivews-reference-json.md)

   