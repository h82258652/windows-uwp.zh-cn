---
title: MatchTicket (JSON)
assetID: 12617677-47f2-e517-af53-5ab9687eea2a
permalink: en-us/docs/xboxlive/rest/json-matchticket.html
description: " MatchTicket (JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 4bc638dfe7735856295ed92f35e244213be7bc1e
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2019
ms.locfileid: "57608332"
---
# <a name="matchticket-json"></a>MatchTicket (JSON)
表示匹配票证，播放机用来定位其他玩家通过多人游戏会话目录 (MPSD) 的 JSON 对象。 
<a id="ID4EN"></a>

  
 
MatchTicket JSON 对象具有以下规范。
 
| 成员| 在任务栏的搜索框中键入| 描述| 
| --- | --- | --- | 
| serviceConfig| GUID| 服务配置 (SCID) 会话标识符。| 
| hopperName| 字符串| 应在其中放置此票证 hopper 的名称。| 
| giveUpDuration| 32 位有符号的整数| 最长等待时间 （整秒数）。| 
| preserveSession| 枚举| 一个值，该值指示是否会话必须重复使用作为要匹配到的会话。 可能的值为"始终"或"从不"。 | 
| ticketSessionRef| MultiplayerSessionReference| <b>MultiplayerSessionReference</b>会话中的播放机或组当前正在播放的对象。 此成员始终是必需的。 | 
| ticketAttributes| 对象数组| 参与方的用户提供的特性和有关票证的值的集合。| 
| 播放机| 对象数组| Player 对象的集合，每个用户提供的属性的属性包。 | 
  
<a id="ID4EW"></a>

 
## <a name="sample-json-syntax"></a>示例 JSON 语法
 

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

[JavaScript 对象表示法 (JSON) 对象引用](atoc-xboxlivews-reference-json.md)

   