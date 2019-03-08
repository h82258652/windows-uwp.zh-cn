---
title: GameSession (JSON)
assetID: 325af9ae-a9a9-5b36-8342-1aff5aff01d1
permalink: en-us/docs/xboxlive/rest/json-gamesession.html
description: " GameSession (JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: ca7276ccdc13d896d19873811b4fa9df9a831cd1
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2019
ms.locfileid: "57646512"
---
# <a name="gamesession-json"></a>GameSession (JSON)
多玩家会话表示游戏数据的 JSON 对象。 
<a id="ID4ER"></a>

  
 
GameSession JSON 对象具有以下规范。
 
| 成员| 在任务栏的搜索框中键入| 描述| 
| --- | --- | --- | 
| creationTime| DateTime| 日期和会话的创建时间，采用 UTC 时间。 | 
| customData| 8 位无符号整数数组| 1024 字节的游戏特定的会话数据。 此值是不透明的服务器。 | 
| displayName| 字符串| 显示名称游戏的会话，最大长度为 128 个字符。 此值是不透明的服务器。 | 
| hasEnded| 布尔值| 如果会话已结束，则为 true 和 false 否则为。 设置阻止进一步的数据提交到该会话，则返回 true 标记游戏会话为只读的此字段。 | 
| isClosed| 布尔值| 如果会话已关闭，并且没有更多游戏玩家可以否则是已添加，和 false，则为 true。 如果此值为 true，将拒绝请求要加入会话。 | 
| maxPlayers| 32 位有符号的整数| 可以在会话中并发的播放机的最大数目。 值的范围是 2-16。 此会话包含玩家的最大数目，一旦进一步拒绝加入会话的请求。 | 
| playersCanBeRemovedBy| PlayerAcl| 一个值，指示允许从会话中删除其他播放机播放机。 可能的值为 NoOne、 Self 和 AnyPlayer。 | 
| 名册| player 对象的数组| 在会话中的播放机的数组。 名册包含当前播放机和球员以前在会话中，但已离开。 永远不会更改播放机在名单中的顺序。 新的播放机将添加到数组末尾。 | 
| seatsAvailable| 32 位有符号的整数| 仍可以加入会话之前，在达到的播放机的最大数目的球员的数。 此值是只读的并且将始终小于 maxPlayers 字段的值。 | 
| sessionId| 字符串| 创建会话时由 MPSD 分配的会话 ID。 访问存储在会话中的信息时，此值通常包含在 URI 中。| 
| titleId| 32 位无符号的整数| 标题创建游戏会话的 ID。| 
| variant| 32 位有符号的整数| 游戏变体。 此值是不透明的服务器。| 
| visibility| VisibilityLevel| 一个值，指示会话可见性。 可能的值为：PlayersCurrentlyInSession、 PlayersEverInSession，和每个人。| 
  
<a id="ID4EEF"></a>

 
## <a name="sample-json-syntax"></a>示例 JSON 语法
 

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

[JavaScript 对象表示法 (JSON) 对象引用](atoc-xboxlivews-reference-json.md)

  
<a id="ID4EZF"></a>

 
##### <a name="reference"></a>参考 

[GameMessage (JSON)](json-gamemessage.md)

 [GameSessionSummary (JSON)](json-gamesessionsummary.md)

 [播放机 (JSON)](json-player.md)

   