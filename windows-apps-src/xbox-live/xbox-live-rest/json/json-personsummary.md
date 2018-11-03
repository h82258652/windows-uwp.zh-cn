---
title: PersonSummary (JSON)
assetID: 22fedb5f-5602-98d8-04a6-786fe3905921
permalink: en-us/docs/xboxlive/rest/json-personsummary.html
author: KevinAsgari
description: " PersonSummary (JSON)"
ms.author: kevinasg
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 33b1cb2adaafba2370e27eb98a10a5143166f0ce
ms.sourcegitcommit: 144f5f127fc4fbd852f2f6780ef26054192d68fc
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/03/2018
ms.locfileid: "5980000"
---
# <a name="personsummary-json"></a>PersonSummary (JSON)
[人 (JSON)](json-person.md)对象的集合。 
<a id="ID4ER"></a>

 
## <a name="personsummary"></a>PersonSummary
 
PersonSummary 对象具有以下规范。
 
| 成员| 类型| 说明| 
| --- | --- | --- | 
| hasCallerMarkedTargetAsFavorite| 布尔值| 是否调用方已标记为常用的目标。 示例值： true| 
| hasCallerMarkedTargetAsKnown| 布尔值| 是否已知调用方已标记为目标。 示例值： true| 
| isCallerFollowingTarget| 布尔值| 是否调用方关注目标。 示例值： true| 
| isTargetFollowingCaller| 布尔值| 目标是否关注调用方。 示例值： true| 
| legacyFriendStatus| 字符串| 目标所示的调用方的旧好友状态。 可以是"None"，"MutuallyAccepted"、"OutgoingRequest"或"IncomingRequest"。 示例值:"MutuallyAccepted"| 
| recentChangeCount| 32 位无符号的整数| 可选。 在目标的社交图片的最新更改的数量。 此值将仅存在时用户正在查看其自己的摘要。 示例值： 5| 
| targetFollowerCount| > 32 位无符号的整数| 遵循的是目标的用户数。 示例值： 1308年| 
| targetFollowingCount| 32 位无符号的整数| 目标关注的用户数。 示例值： 112| 
| 水印| 字符串| 可选。 目标的的最新更改水印。 此值将仅存在时用户正在查看其自己的摘要。 示例值： 5| 
  
<a id="ID4E4D"></a>

 
## <a name="sample-json-syntax"></a>JSON 语法示例
 

```json
{
    "targetFollowingCount": 87,
    "targetFollowerCount": 19,
    "isCallerFollowingTarget": true,
    "isTargetFollowingCaller": false,
    "hasCallerMarkedTargetAsFavorite": true,
    "hasCallerMarkedTargetAsKnown": true,
    "legacyFriendStatus": "None",
    "recentChangeCount": 5,
    "watermark": "5246775845000319351"
}
    
```

  
<a id="ID4EGE"></a>

 
## <a name="see-also"></a>另请参阅
 
<a id="ID4EIE"></a>

 
##### <a name="parent"></a>Parent 的子磁盘） 

[JavaScript 对象表示法 (JSON) 对象参考](atoc-xboxlivews-reference-json.md)

   