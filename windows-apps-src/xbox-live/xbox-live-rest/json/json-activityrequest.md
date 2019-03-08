---
title: ActivityRequest (JSON)
assetID: 9eb03ab7-352c-4465-ec86-d544e76f63f9
permalink: en-us/docs/xboxlive/rest/json-activityrequest.html
description: " ActivityRequest (JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 791a566d278b92aeb34ab36d38719b44e9cc6c8f
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2019
ms.locfileid: "57628152"
---
# <a name="activityrequest-json"></a>ActivityRequest (JSON)
有关一个或多个用户的丰富的状态显示信息请求。 
<a id="ID4EN"></a>

 
## <a name="activityrequest"></a>ActivityRequest
 
ActivityRequest 对象具有以下规范。
 
| 成员| 在任务栏的搜索框中键入| 描述| 
| --- | --- | --- | 
| richPresence| [RichPresenceRequest](json-richpresencerequest.md)| 应使用丰富的状态显示字符串的友好名称。| 
| 媒体| MediaRequest| 用户是观看或收听的媒体信息。| 
  
<a id="ID4EVB"></a>

 
## <a name="sample-json-syntax"></a>示例 JSON 语法
 

```json
{
    richPresence:
    {
      id:"playingMapWeapon",
      scid:"abba0123-08ba-48ca-9f1a-21627b189b0f"
    }
  }
    
```

  
<a id="ID4E5B"></a>

 
## <a name="see-also"></a>另请参阅
 
<a id="ID4EAC"></a>

 
##### <a name="parent"></a>Parent 的子磁盘） 

[JavaScript 对象表示法 (JSON) 对象引用](atoc-xboxlivews-reference-json.md)

   