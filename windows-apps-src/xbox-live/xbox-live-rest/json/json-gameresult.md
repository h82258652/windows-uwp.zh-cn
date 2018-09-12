---
title: GameResult (JSON)
assetID: 43d863c0-2179-ae46-5d4a-2f08cd44b667
permalink: en-us/docs/xboxlive/rest/json-gameresult.html
author: KevinAsgari
description: " GameResult (JSON)"
ms.author: kevinasg
ms.date: 20-12-2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 24f5af1639f5348fe20c36c56c1301f723d832f7
ms.sourcegitcommit: 72710baeee8c898b5ab77ceb66d884eaa9db4cb8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/12/2018
ms.locfileid: "3880770"
---
# <a name="gameresult-json"></a>GameResult (JSON)
表示数据，以描述游戏会话的结果的 JSON 对象。 
<a id="ID4EN"></a>

  
 
GameResult JSON 对象具有以下成员。
 
| 成员| 类型| 说明| 
| --- | --- | --- | 
| blob| 8 位无符号整数数组| 自定义特定于游戏的结果数据。| 
| 结果| 字符串| 游戏会话的玩家的参与的结果。 有效值为"Win"、"丢失"绑定"。 | 
| 分数| 64 位有符号整数| 评分，玩家收到游戏会话中。| 
| 时间| 64 位有符号整数| 游戏会话的玩家的时间。| 
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

[JavaScript 对象表示法 (JSON) 对象引用](atoc-xboxlivews-reference-json.md)

   