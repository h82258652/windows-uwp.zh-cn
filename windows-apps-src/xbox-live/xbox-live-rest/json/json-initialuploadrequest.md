---
title: InitialUploadRequest (JSON)
assetID: 8b8bce98-cb5f-bbaf-5564-9be2f58d749b
permalink: en-us/docs/xboxlive/rest/json-initialuploadrequest.html
description: " InitialUploadRequest (JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 2fbb2417f743d97b487ad16abd241fcb5eea62bb
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2019
ms.locfileid: "57646632"
---
# <a name="initialuploadrequest-json"></a>InitialUploadRequest (JSON)
开机自检 GameClip 正文上载请求。 
<a id="ID4EN"></a>

 
## <a name="initialuploadrequest"></a>InitialUploadRequest
 
InitialUploadRequest 对象具有以下规范。
 
| 成员| 在任务栏的搜索框中键入| 描述| 
| --- | --- | --- | 
| <b>greatestMomentId</b>| 字符串| 字符串文本要使用的 ID 作为剪辑的名称。 这是托管，本地化标题在配置文件中的标题的开发人员。| 
| <b>userCaption</b>| 字符串| 可选。 对于最大长度为 250 个字符的游戏剪辑的替换用户输入名称。| 
| <b>sessionRef</b>| 字符串| 可选。 在此期间完成录制游戏会话的引用。| 
| <b>dateRecorded</b>| DateTime| 开始录制，utc 格式的时间。 封送为采用 ISO 8601 格式字符串 (请参阅<a href="https://www.w3.org/TR/NOTE-datetime">日期和时间格式</a>有关详细信息)。| 
| <b>durationInSeconds</b>| 32 位无符号的整数| 以秒为单位的剪辑的长度。| 
| <b>expectedBlocks</b>| 32 位无符号的整数| 可选。 块文件被划分成的数目。 如果文件不会传输到单个请求中，省略。| 
| <b>fileSize</b>| 32 位无符号的整数| 文件大小 （字节） 将上传的视频。| 
| <b>type</b>| [GameClipType 枚举](../enums/gvr-enum-gamecliptypes.md)| 剪辑，是以逗号分隔的枚举的字符串值作为封送的类型。| 
| <b>source</b>| [GameClipSource 枚举](../enums/gvr-enum-gameclipsource.md)| 指定如何剪辑已指明其出处，封送为枚举的一个字符串值。| 
| <b>visibility</b>| [GameClipVisibility 枚举](../enums/gvr-enum-gameclipvisibility.md)| 在系统中发布后，请指定游戏剪辑的可见性。| 
| <b>titleData</b>| 字符串| 可选。 与此代码片段相关联的特定于标题的属性的属性包。 存储并作为返回的是。 标题开发人员可以使用此字段保留其自己剪辑有关的元数据。| 
| <b>titleData</b>| 字符串| 可选。 与此代码片段相关联的特定于控制台的属性的属性包。 存储并作为返回的是。 控制台平台可以使用此字段保留其自己剪辑有关的元数据。| 
| <b>systemProperties</b>| 字符串| 可选。 与此代码片段相关联的特定于控制台的属性的属性包。 存储和原样返回。 控制台平台可以使用此字段保留其自己剪辑有关的元数据。| 
| <b>usersInSession</b>| 字符串数组| 可选。 当前会话中的用户的列表。| 
| <b>thumbnailSource</b>| [ThumbnailSource 枚举](../enums/gvr-enum-thumbnailsource.md)| 可选。 缩略图的源。| 
| <b>thumbnailOffsetMillseconds</b>| 32 位有符号的整数| 指定偏移量生成缩略图的偏移量 （以毫秒为单位）。 仅指定何时<b>thumbnailSource</b>设置为偏移量。| 
| <b>savedByUser</b>| 布尔值| 可选。 设置保存到用户的配额，而不是先进先出存储的剪辑。 默认值为 false。| 
  
<a id="ID4ERH"></a>

 
## <a name="sample-json-syntax"></a>示例 JSON 语法
 

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

[JavaScript 对象表示法 (JSON) 对象引用](atoc-xboxlivews-reference-json.md)

   