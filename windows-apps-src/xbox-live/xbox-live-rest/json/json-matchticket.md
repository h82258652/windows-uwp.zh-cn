---
title: MatchTicket (JSON)
assetID: 12617677-47f2-e517-af53-5ab9687eea2a
permalink: en-us/docs/xboxlive/rest/json-matchticket.html
author: KevinAsgari
description: " MatchTicket (JSON)"
ms.author: kevinasg
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 5dbc410c809358c6df7a4eb2686b81c4017ed550
ms.sourcegitcommit: cd00bb829306871e5103db481cf224ea7fb613f0
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/01/2018
ms.locfileid: "5885889"
---
# <a name="matchticket-json"></a>MatchTicket (JSON)
表示玩家用于查找其他玩家通过多人游戏会话目录 (MPSD) 的匹配票证的 JSON 对象。 
<a id="ID4EN"></a>

  
 
MatchTicket JSON 对象具有以下规范。
 
| 成员| 类型| 说明| 
| --- | --- | --- | 
| serviceConfig| GUID| 服务配置标识符 (SCID) 会话。| 
| hopperName| 字符串| 此票证应放置在其中的漏斗的名称。| 
| giveUpDuration| 32 位有符号整数| 最大的等待时间 （不可或缺的秒数）。| 
| preserveSession| 枚举| 指示会话是否必须为以匹配到会话重复使用的值。 可能值为"始终"或"从不"。 | 
| ticketSessionRef| MultiplayerSessionReference| <b>MultiplayerSessionReference</b>会话的玩家或组当前正在播放的对象。 此成员始终是必需的。 | 
| ticketAttributes| 对象数组| 玩家的用户提供属性和值有关票证集合。| 
| 玩家| 对象数组| 对象集合的玩家，每个都有一个属性包的用户提供的属性。 | 
  
<a id="ID4EW"></a>

 
## <a name="sample-json-syntax"></a>JSON 语法示例
 

```json
{
        "serviceConfig": "07617C5B-3423-4505-B6C6-10A16E1E5DDB",
        "hopperName": "TestHopper",
        "giveUpDuration": 10,
        "preserveSession": "never",
        "ticketSessionRef": {
        "scid": "AFFEABDF-0000-0000-0000-000000000001",
        "templateName": "TestTemplate",
        "sessionName": "5E551041-0000-0000-0000-000000000001"
    },
    "ticketAttributes": {
        "desiredMap": "Test Map",
        "desiredGameType": "Test GameType"
    },
    "players": [
        {
            "xuid": 123412345123,
            "playerAttributes": {
                "skill": 5,
                "ageRange": "teenager"
            }
        },
        {
          "xuid": 123412345124,
          "playerAttributes": {
              "skill": 15,
              "ageRange": "twenty-something"
          }
        }
      ]
    }
  
    
```

  
<a id="ID4EEB"></a>

 
## <a name="see-also"></a>另请参阅
 
<a id="ID4EGB"></a>

 
##### <a name="parent"></a>Parent 的子磁盘） 

[JavaScript 对象表示法 (JSON) 对象参考](atoc-xboxlivews-reference-json.md)

   