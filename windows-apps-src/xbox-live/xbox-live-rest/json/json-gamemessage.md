---
title: GameMessage (JSON)
assetID: c11606e6-701f-5807-4aef-5608c98ad831
permalink: en-us/docs/xboxlive/rest/json-gamemessage.html
description: " GameMessage (JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: a2bddd9e26b4716fd1e33c4b5bbde56672b5d3f8
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2019
ms.locfileid: "57651292"
---
# <a name="gamemessage-json"></a>GameMessage (JSON)
JSON 对象，游戏会话的消息队列中定义的消息的数据。 
<a id="ID4EN"></a>

  
 
GameMessage JSON 对象具有以下规范。
 
| 成员| 在任务栏的搜索框中键入| 描述| 
| --- | --- | --- | 
| 数据| 8 位无符号整数数组| Base64 编码的数据的游戏客户端想要发送给其他游戏客户端。 此值是不透明的服务器。 | 
| senderXuid| 64 位无符号的整数| 播放机发送消息的 Xbox 用户 ID。 | 
| sequenceNumber| 32 位有符号的整数| 游戏的消息的序列号。 此值是由服务器分配的。 序列号都保证单调递增，但可能不是连续。 是唯一的序列号中消息队列，但不能跨消息队列。 | 
| queueIndex| 32 位有符号的整数| 会话消息队列的消息的索引。 可能的值为 0-3。| 
| timeStamp| DateTime| 游戏的消息在队列中创建的 utc 服务器时间。 | 
  
<a id="ID4ERC"></a>

 
## <a name="sample-json-syntax"></a>示例 JSON 语法
 

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

[JavaScript 对象表示法 (JSON) 对象引用](atoc-xboxlivews-reference-json.md)

  
<a id="ID4EGD"></a>

 
##### <a name="reference"></a>参考 

[GameSession (JSON)](json-gamesession.md)

   