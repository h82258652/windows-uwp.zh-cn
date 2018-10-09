---
title: GameSessionSummary (JSON)
assetID: 50cf91ba-29d3-1260-7643-bcb3f8d74fc0
permalink: en-us/docs/xboxlive/rest/json-gamesessionsummary.html
author: KevinAsgari
description: " GameSessionSummary (JSON)"
ms.author: kevinasg
ms.date: 20-12-2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 610f771641352447f9d38fc4217231ba3230e6fb
ms.sourcegitcommit: 49aab071aa2bd88f1c165438ee7e5c854b3e4f61
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/09/2018
ms.locfileid: "4465298"
---
# <a name="gamesessionsummary-json"></a>GameSessionSummary (JSON)
为游戏会话表示摘要数据的 JSON 对象。 
<a id="ID4EN"></a>

  
 
GameSessionSummary JSON 对象具有以下规范。
 
| 成员| 类型| 描述| 
| --- | --- | --- | 
| creationTime| DateTime| 日期和会话的创建时间，采用 UTC 时间。 | 
| customData| 8 位无符号整数数组| 1024 字节的特定于游戏的会话数据。 此值不透明到服务器。 | 
| displayName| 字符串| 显示名称的游戏会话，具有 128 个字符的最大长度。 此值不透明到服务器。 | 
| hasEnded| 布尔值| 如果会话已结束，则为 true 和 false 否则为。 设置为 true 标记为只读，游戏会话提交到会话阻止进一步数据此字段。 | 
| sessionId| 字符串会话 id。 | 
| titleId| 32 位无符号的整数| 游戏创建游戏会话的 ID。| 
| variant| 32 位有符号整数| 游戏的变体。 此值不透明到服务器。| 
  
<a id="ID4EID"></a>

 
## <a name="sample-json-syntax"></a>JSON 语法示例
 

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

[JavaScript 对象表示法 (JSON) 对象参考](atoc-xboxlivews-reference-json.md)

  
<a id="ID4E4D"></a>

 
##### <a name="reference"></a>参考 

[GameSession (JSON)](json-gamesession.md)

   