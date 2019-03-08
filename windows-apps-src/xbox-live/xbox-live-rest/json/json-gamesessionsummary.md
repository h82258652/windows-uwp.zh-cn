---
title: GameSessionSummary (JSON)
assetID: 50cf91ba-29d3-1260-7643-bcb3f8d74fc0
permalink: en-us/docs/xboxlive/rest/json-gamesessionsummary.html
description: " GameSessionSummary (JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 8dace19404ae7c8b1d1ef296a21c874e4dd14c6f
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2019
ms.locfileid: "57613382"
---
# <a name="gamesessionsummary-json"></a>GameSessionSummary (JSON)
表示为游戏会话的摘要数据的 JSON 对象。 
<a id="ID4EN"></a>

  
 
GameSessionSummary JSON 对象具有以下规范。
 
| 成员| 在任务栏的搜索框中键入| 描述| 
| --- | --- | --- | 
| creationTime| DateTime| 日期和会话的创建时间，采用 UTC 时间。 | 
| customData| 8 位无符号整数数组| 1024 字节的游戏特定的会话数据。 此值是不透明的服务器。 | 
| displayName| 字符串| 显示名称游戏的会话，最大长度为 128 个字符。 此值是不透明的服务器。 | 
| hasEnded| 布尔值| 如果会话已结束，则为 true 和 false 否则为。 设置阻止进一步的数据提交到该会话，则返回 true 标记游戏会话为只读的此字段。 | 
| sessionId| 字符串会话 id。 | 
| titleId| 32 位无符号的整数| 标题创建游戏会话的 ID。| 
| variant| 32 位有符号的整数| 游戏变体。 此值是不透明的服务器。| 
  
<a id="ID4EID"></a>

 
## <a name="sample-json-syntax"></a>示例 JSON 语法
 

```json
{
    "sessionId": "702e5aaf-e7bd-4a7c-abea-9dd4be10edec",
    "titleId": 1297287259,
    "variant": 1,
    "displayName": "Contoso Cards",
    "creationTime": "2011-06-23T17:13:06Z",
    "customData": null,
    "hasEnded": false,
}
    
```

  
<a id="ID4ERD"></a>

 
## <a name="see-also"></a>另请参阅
 
<a id="ID4ETD"></a>

 
##### <a name="parent"></a>Parent 的子磁盘） 

[JavaScript 对象表示法 (JSON) 对象引用](atoc-xboxlivews-reference-json.md)

  
<a id="ID4E4D"></a>

 
##### <a name="reference"></a>参考 

[GameSession (JSON)](json-gamesession.md)

   