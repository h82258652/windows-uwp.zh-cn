---
title: TitleRequest (JSON)
assetID: 43aeb6f9-726d-9260-e2ba-f005ea688bf1
permalink: en-us/docs/xboxlive/rest/json-titlerequest.html
description: " TitleRequest (JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: a90f42c2f830ba6f04f77a1acaba067a2746a062
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2019
ms.locfileid: "57593792"
---
# <a name="titlerequest-json"></a>TitleRequest (JSON)
有关标题信息的请求。 
<a id="ID4EN"></a>

 
## <a name="titlerequest"></a>TitleRequest
 
TitleRequest 对象具有以下规范。
 
| 成员| 在任务栏的搜索框中键入| 描述| 
| --- | --- | --- | 
| id| 32 位无符号的整数| 标题的标识符。| 
| 活动| [ActivityRequest](json-activityrequest.md)| 标题中的信息，包括丰富的状态显示和媒体信息，如果可用。| 
| 状态| 字符串| 无论用户是否处于活动状态。 此字段需要将标记为非活动用户。 默认值为"活动"。| 
| 放置| 字符串| 标题位置模式。 可能值包括"完整"、"填充"，"对齐"或"后台"。 默认值为"full"。| 
  
<a id="ID4EJC"></a>

 
## <a name="sample-json-syntax"></a>示例 JSON 语法
 

```json
{
  id:"12341234",
  placement:"snapped",
  state:"active",
  activity:
  {
    richPresence:
    {
      id:"playingMapWeapon",
      scid:"82b11353-08ba-48ca-9f1a-21627b189b0f"
    }
  }
}
    
```

  
<a id="ID4ESC"></a>

 
## <a name="see-also"></a>另请参阅
 
<a id="ID4EUC"></a>

 
##### <a name="parent"></a>Parent 的子磁盘） 

[JavaScript 对象表示法 (JSON) 对象引用](atoc-xboxlivews-reference-json.md)

  
<a id="ID4E5C"></a>

 
##### <a name="reference"></a>参考 

[POST (/users/xuid({xuid})/devices/current/titles/current)](../uri/presence/uri-usersxuiddevicescurrenttitlescurrentpost.md)

   