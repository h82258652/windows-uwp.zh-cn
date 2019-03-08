---
title: GameResult (JSON)
assetID: 43d863c0-2179-ae46-5d4a-2f08cd44b667
permalink: en-us/docs/xboxlive/rest/json-gameresult.html
description: " GameResult (JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: b408b1aaae5e6f54958a016575c4a2c37765f1e9
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2019
ms.locfileid: "57602332"
---
# <a name="gameresult-json"></a>GameResult (JSON)
表示描述游戏会话结果的数据的 JSON 对象。 
<a id="ID4EN"></a>

  
 
GameResult JSON 对象具有以下成员。
 
| 成员| 在任务栏的搜索框中键入| 描述| 
| --- | --- | --- | 
| blob| 8 位无符号整数数组| 自定义特定于标题的结果数据。| 
| 结果| 字符串| 播放机的参与游戏会话的结果。 有效值为"胜出"、"丢失"绑定"。 | 
| 分数| 64 位有符号的整数| 进行游戏的会话中的播放机收到的分数。| 
| 时间| 64 位有符号的整数| 游戏会话的播放机的时间。| 
| xuid| 64 位无符号的整数| 向其应用结果的播放机 Xbox 用户 ID。| 
  
<a id="ID4EPC"></a>

 
## <a name="sample-json-syntax"></a>示例 JSON 语法
 

```json
{
   "xuid": 2533274790412952,
   "outcome": "Win",
   "score": 100
}
    
```

  
<a id="ID4EYC"></a>

 
## <a name="see-also"></a>另请参阅
 
<a id="ID4E1C"></a>

 
##### <a name="parent"></a>Parent 的子磁盘） 

[JavaScript 对象表示法 (JSON) 对象引用](atoc-xboxlivews-reference-json.md)

   