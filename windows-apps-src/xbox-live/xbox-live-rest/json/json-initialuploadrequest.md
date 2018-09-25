---
title: InitialUploadRequest (JSON)
assetID: 8b8bce98-cb5f-bbaf-5564-9be2f58d749b
permalink: en-us/docs/xboxlive/rest/json-initialuploadrequest.html
author: KevinAsgari
description: " InitialUploadRequest (JSON)"
ms.author: kevinasg
ms.date: 20-12-2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: eac2405b8668179fa60921ca45012a417e61b352
ms.sourcegitcommit: 232543fba1fb30bb1489b053310ed6bd4b8f15d5
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/25/2018
ms.locfileid: "4175333"
---
# <a name="initialuploadrequest-json"></a>InitialUploadRequest (JSON)
POST GameClip 的正文上传请求。 
<a id="ID4EN"></a>

 
## <a name="initialuploadrequest"></a>InitialUploadRequest
 
InitialUploadRequest 对象具有以下规范。
 
| 成员| 类型| 说明| 
| --- | --- | --- | 
| <b>greatestMomentId</b>| 字符串| 若要使用该剪辑的名称作为文本字符串 ID。 这是托管，本地化标题的配置文件中的游戏开发人员。| 
| <b>userCaption</b>| 字符串| 可选。 最多 250 个字符的最大长度的游戏剪辑的备用用户输入名称。| 
| <b>sessionRef</b>| 字符串| 可选。 在这期间完成录制的游戏会话引用。| 
| <b>dateRecorded</b>| DateTime| 录制的启动时间，采用 UTC。 封送作为字符串采用 ISO 8601 格式 （有关详细信息，请参阅<a href="http://www.w3.org/TR/NOTE-datetime">日期和时间格式</a>）。| 
| <b>durationInSeconds</b>| 32 位无符号的整数| 剪辑以秒为单位的长度。| 
| <b>expectedBlocks</b>| 32 位无符号的整数| 可选。 文件将划分到其中的块数量。 如果将在单个请求传输文件，省略。| 
| <b>文件大小</b>| 32 位无符号的整数| 以字节为单位的视频将上传的文件大小。| 
| <b>type</b>| [GameClipType 枚举](../enums/gvr-enum-gamecliptypes.md)| 剪辑，作为枚举的以逗号分隔的字符串值封送的类型。| 
| <b>源</b>| [GameClipSource 枚举](../enums/gvr-enum-gameclipsource.md)| 指定该剪辑的来源如何，作为枚举的字符串值封送。| 
| <b>可见性</b>| [GameClipVisibility 枚举](../enums/gvr-enum-gameclipvisibility.md)| 它在系统中发布后，请指定游戏剪辑的可见性。| 
| <b>titleData</b>| 字符串| 可选。 与此代码片段关联的特定于游戏的属性的属性包。 存储和作为返回的是。 游戏开发人员可以使用此字段保留有关剪辑自己元数据。| 
| <b>titleData</b>| 字符串| 可选。 与此代码片段关联的特定于控制台的属性的属性包。 存储和作为返回的是。 控制台平台可以使用此字段保留有关剪辑自己元数据。| 
| <b>systemProperties</b>| 字符串| 可选。 与此代码片段关联的特定于控制台的属性的属性包。 存储并返回原样。 控制台平台可以使用此字段保留有关剪辑自己元数据。| 
| <b>usersInSession</b>| 字符串的数组| 可选。 当前会话中的用户的列表。| 
| <b>thumbnailSource</b>| [ThumbnailSource 枚举](../enums/gvr-enum-thumbnailsource.md)| 可选。 缩略图的源。| 
| <b>thumbnailOffsetMillseconds</b>| 32 位有符号整数| 指定偏移生成的缩略图的偏移量 （以毫秒为单位）。 仅指定当<b>thumbnailSource</b>设置为偏移量。| 
| <b>savedByUser</b>| 布尔值| 可选。 设置要保存到用户的配额，而不是 FIFO 存储的剪辑。 默认值为 false。| 
  
<a id="ID4ERH"></a>

 
## <a name="sample-json-syntax"></a>JSON 语法示例
 

```json
{
   "greatestMomentId": "123abc",
   "userCaption": "OMG Look at this!",
   "sessionRef": "4587552a-a5ad-4c4c-a787-5bc5af70e4c9",
   "dateRecorded": "2012-12-23T11:08:08Z",
   "durationInSeconds": 27,
   "expectedBlocks": 7,
   "fileSize": 1234567,
   "type": "MagicMoment, Achievement",
   "source": "Console",
   "visibility": "Default",
   "titleData": "{ 'Boss': 'The Invincible' }",
   "systemProperties": "{ 'Id': '123456', 'Location': 'C:\\videos\\123456.mp4' }",
   "thumbnailSource": "Offset",
   "thumbnailOffsetMillseconds": 20000,
   "savedByUser": false
 }
    
```

  
<a id="ID4E1H"></a>

 
## <a name="see-also"></a>另请参阅
 
<a id="ID4E3H"></a>

 
##### <a name="parent"></a>Parent 的子磁盘） 

[JavaScript 对象表示法 (JSON) 对象参考](atoc-xboxlivews-reference-json.md)

   