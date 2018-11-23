---
title: GameSession (JSON)
assetID: 325af9ae-a9a9-5b36-8342-1aff5aff01d1
permalink: en-us/docs/xboxlive/rest/json-gamesession.html
author: KevinAsgari
description: " GameSession (JSON)"
ms.author: kevinasg
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: a62dd48857ed9c7ba8601d5ca08179c6a32b0db5
ms.sourcegitcommit: 93c0a60cf531c7d9fe7b00e7cf78df86906f9d6e
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/23/2018
ms.locfileid: "7574989"
---
# <a name="gamesession-json"></a>GameSession (JSON)
多人游戏会话表示游戏数据的 JSON 对象。 
<a id="ID4ER"></a>

  
 
GameSession JSON 对象具有以下规范。
 
| 成员| 类型| 说明| 
| --- | --- | --- | 
| creationTime| DateTime| 日期和时间会话创建时，采用 UTC。 | 
| customData| 8 位无符号整数数组| 1024 字节的特定于游戏的会话数据。 此值不透明到服务器。 | 
| displayName| 字符串| 显示名称的游戏会话，128 个字符的最大长度。 此值不透明到服务器。 | 
| hasEnded| 布尔值| 如果会话已结束，则为 true 和 false 否则为。 设置为 true 标记为只读，游戏会话提交到会话阻止进一步数据此字段。 | 
| 关闭| 布尔值| 如果会话已关闭，并且没有更多的玩家能够添加了，而且 false 否则，则为 true。 如果此值为 true，将拒绝请求加入会话。 | 
| maxPlayers| 32 位有符号整数| 最大可同时在会话中的玩家数。 值的范围是 2-16。 一旦会话包含的最大玩家数时，进一步请求加入会话被拒绝。 | 
| playersCanBeRemovedBy| PlayerAcl| 一个值，指示允许从会话中删除其他玩家的玩家。 可能的值为 NoOne、 自行，和 AnyPlayer。 | 
| 名单| 玩家对象的数组| 在会话中的玩家的数组。 名单包含当前玩家和玩家之前在会话中，但留下的反馈。 名单中玩家的顺序永远不会更改。 新玩家将添加到该数组的末尾。 | 
| seatsAvailable| 32 位有符号整数| 仍可以加入会话的最大玩家数时达到之前的玩家数。 此值是只读的并且始终小于 maxPlayers 字段的值。 | 
| sessionId| 字符串| 在创建会话时分配由 MPSD 会话 ID。 访问存储在会话中的信息时，此值通常包含 URI 中。| 
| titleId| 32 位无符号的整数| 游戏创建游戏会话的 ID。| 
| variant| 32 位有符号整数| 游戏的变体。 此值不透明到服务器。| 
| 可见性| VisibilityLevel| 一个值，指示会话可见性。 可能的值为： PlayersCurrentlyInSession、 PlayersEverInSession，并且所有人。| 
  
<a id="ID4EEF"></a>

 
## <a name="sample-json-syntax"></a>JSON 语法示例
 

```json
{
    "sessionId": "702e5aaf-e7bd-4a7c-abea-9dd4be10edec",
    "titleId": 1297287259,
    "variant": 1,
    "displayName": "Contoso Cards",
    "creationTime": "2011-06-23T17:13:06Z",
    "customData": null,
    "maxPlayers": 4,
    "seatsAvailable": 4,
    "isClosed": false,
    "hasEnded": false,
    "roster": [],
    "visibility": "PlayersCurrentlyInSession",
    "playersCanBeRemovedBy": "Self",
 }
    
```

  
<a id="ID4ENF"></a>

 
## <a name="see-also"></a>另请参阅
 
<a id="ID4EPF"></a>

 
##### <a name="parent"></a>Parent 的子磁盘） 

[JavaScript 对象表示法 (JSON) 对象参考](atoc-xboxlivews-reference-json.md)

  
<a id="ID4EZF"></a>

 
##### <a name="reference"></a>参考 

[GameMessage (JSON)](json-gamemessage.md)

 [GameSessionSummary (JSON)](json-gamesessionsummary.md)

 [Player (JSON)](json-player.md)

   