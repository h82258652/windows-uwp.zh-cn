---
title: GameMessage (JSON)
assetID: c11606e6-701f-5807-4aef-5608c98ad831
permalink: en-us/docs/xboxlive/rest/json-gamemessage.html
author: KevinAsgari
description: " GameMessage (JSON)"
ms.author: kevinasg
ms.date: 20-12-2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 089d2a492c8878e79bd60de1226c948e1eee7e0f
ms.sourcegitcommit: c4d3115348c8b54fcc92aae8e18fdabc3deb301d
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/22/2018
ms.locfileid: "5400762"
---
# <a name="gamemessage-json"></a>GameMessage (JSON)
一个游戏会话的消息队列中定义为一条消息的数据的 JSON 对象。 
<a id="ID4EN"></a>

  
 
GameMessage JSON 对象具有以下规范。
 
| 成员| 类型| 说明| 
| --- | --- | --- | 
| 数据| 8 位无符号整数数组| Base64 编码的数据的游戏客户端想要发送到其他游戏客户端。 此值不透明到服务器。 | 
| senderXuid| 64 位无符号的整数| 玩家发送消息的 Xbox 用户 ID。 | 
| 序列号| 32 位有符号整数| 该游戏的消息的序列号。 服务器分配此值。 序列号保证单调递增，但可能不会连续。 序列号是唯一内消息队列，但不能在消息队列。 | 
| queueIndex| 32 位有符号整数| 消息的会话消息队列的索引。 可能的值为 0-3。| 
| 时间戳| DateTime| 游戏的消息队列中创建的服务器，采用 UTC 时间。 | 
  
<a id="ID4ERC"></a>

 
## <a name="sample-json-syntax"></a>JSON 语法示例
 

```json
{
    "queueIndex": 0,
    "sequenceNumber": 5,
    "senderXuid": 65536,
    "timestamp": "2011-06-23T18:49:50Z",
    "data": null
}
    
```

  
<a id="ID4E1C"></a>

 
## <a name="see-also"></a>另请参阅
 
<a id="ID4E3C"></a>

 
##### <a name="parent"></a>Parent 的子磁盘） 

[JavaScript 对象表示法 (JSON) 对象参考](atoc-xboxlivews-reference-json.md)

  
<a id="ID4EGD"></a>

 
##### <a name="reference"></a>参考 

[GameSession (JSON)](json-gamesession.md)

   