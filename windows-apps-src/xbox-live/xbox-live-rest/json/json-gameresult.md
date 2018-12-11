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
ms.sourcegitcommit: 8921a9cc0dd3e5665345ae8eca7ab7aeb83ccc6f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/11/2018
ms.locfileid: "8895444"
---
# <a name="gameresult-json"></a>GameResult (JSON)
表示数据，以描述的游戏会话的结果的 JSON 对象。 
<a id="ID4EN"></a>

  
 
GameResult JSON 对象具有以下成员。
 
| 成员| 类型| 描述| 
| --- | --- | --- | 
| blob| 8 位无符号整数数组| 自定义特定于游戏的结果数据。| 
| 结果| 字符串| 游戏会话的玩家的参与的结果。 有效值为"Win"、"丢失"绑定"。 | 
| 分数| 64 位有符号整数| 玩家收到游戏会话中的高分。| 
| time| 64 位有符号整数| 游戏会话的玩家的时间。| 
| xuid| 64 位无符号的整数| 结果将应用到其玩家的 Xbox 用户 ID。| 
  
<a id="ID4EPC"></a>

 
## <a name="sample-json-syntax"></a>JSON 语法示例
 

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

[JavaScript 对象表示法 (JSON) 对象参考](atoc-xboxlivews-reference-json.md)

   