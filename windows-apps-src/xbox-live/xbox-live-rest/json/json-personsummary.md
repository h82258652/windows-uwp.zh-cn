---
title: PersonSummary (JSON)
assetID: 22fedb5f-5602-98d8-04a6-786fe3905921
permalink: en-us/docs/xboxlive/rest/json-personsummary.html
description: " PersonSummary (JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: a787992507405a70185140e879be731d72806eff
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2019
ms.locfileid: "57612622"
---
# <a name="personsummary-json"></a>PersonSummary (JSON)
集合[Person (JSON)](json-person.md)对象。 
<a id="ID4ER"></a>

 
## <a name="personsummary"></a>PersonSummary
 
PersonSummary 对象具有以下规范。
 
| 成员| 在任务栏的搜索框中键入| 描述| 
| --- | --- | --- | 
| hasCallerMarkedTargetAsFavorite| 布尔值| 是否调用方已标记为收藏的目标。 示例值： true| 
| hasCallerMarkedTargetAsKnown| 布尔值| 是否为已知调用方已标记为目标。 示例值： true| 
| isCallerFollowingTarget| 布尔值| 调用方是否遵循目标。 示例值： true| 
| isTargetFollowingCaller| 布尔值| 无论目标是按照调用方。 示例值： true| 
| legacyFriendStatus| 字符串| 目标由调用方所示的旧友元状态。 可以是"None"、"MutuallyAccepted"、"OutgoingRequest"或"IncomingRequest"。 示例值："MutuallyAccepted"| 
| recentChangeCount| 32 位无符号的整数| 可选。 目标的社交图中的最近更改数。 此值仅在存在时用户正在查看他们自己的摘要。 示例值：5| 
| targetFollowerCount| > 32 位无符号的整数| 以下目标的用户数。 示例值：1308| 
| targetFollowingCount| 32 位无符号的整数| 以下目标的用户数。 示例值：112| 
| 水印| 字符串| 可选。 目标的的最新更改水印。 此值仅在存在时用户正在查看他们自己的摘要。 示例值：5| 
  
<a id="ID4E4D"></a>

 
## <a name="sample-json-syntax"></a>示例 JSON 语法
 

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

[JavaScript 对象表示法 (JSON) 对象引用](atoc-xboxlivews-reference-json.md)

   