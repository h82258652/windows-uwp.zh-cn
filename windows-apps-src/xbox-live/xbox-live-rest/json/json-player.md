---
title: Player (JSON)
assetID: eaf6d082-869b-d2d3-d548-5cef65e54541
permalink: en-us/docs/xboxlive/rest/json-player.html
description: " Player (JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: a5967cbfecd47c5675926bd45939442c45dda7b6
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2019
ms.locfileid: "57622492"
---
# <a name="player-json"></a>Player (JSON)
包含玩家在游戏会话中数据。 
<a id="ID4EN"></a>

 
## <a name="player"></a>玩家
 
Player 对象具有以下规范。
 
| 成员| 在任务栏的搜索框中键入| 描述| 
| --- | --- | --- | 
| customData| 8 位无符号整数数组| 1024 个字节的 Base64 编码的特定于游戏的玩家数据。 此值是不透明的服务器。| 
| 玩家代号| 字符串| 玩家代号，最多包含 15 个字符，播放机。 标识在播放器时，客户端应在 UI 中使用此值。 | 
| isCurrentlyInSession| 布尔值| 指示是否播放机是当前会话中或离开该会话。| 
| seatIndex| 32 位有符号的整数| 播放机在会话中的索引。| 
| xuid| 64 位无符号的整数| Xbox 用户 ID (XUID) 的播放机。| 
  
<a id="ID4E3C"></a>

 
## <a name="sample-json-syntax"></a>示例 JSON 语法
 

```json
{
    "xuid": 2533274790412952,
    "gamertag":"MyTestUser",
    "seatindex": 3
    "customData":"AIHJ2?iE?/jiKE!l5S=T..."
    "isCurrentlyInSession":"true"
}
    
```

  
<a id="ID4EFD"></a>

 
## <a name="see-also"></a>另请参阅
 
<a id="ID4EHD"></a>

 
##### <a name="parent"></a>Parent 的子磁盘） 

[JavaScript 对象表示法 (JSON) 对象引用](atoc-xboxlivews-reference-json.md)

  
<a id="ID4ERD"></a>

 
##### <a name="reference"></a>参考 

[GameSession (JSON)](json-gamesession.md)

   