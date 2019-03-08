---
title: MultiplayerActivityDetails (JSON)
assetID: f982aa5e-2694-4ef9-bc55-6c099a3cf9ec
permalink: en-us/docs/xboxlive/rest/json-multiplayeractivitydetails.html
description: " MultiplayerActivityDetails (JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 188bcebb8d6bff879f30dcc83d7039fbcbfae0b2
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2019
ms.locfileid: "57658102"
---
# <a name="multiplayeractivitydetails-json"></a>MultiplayerActivityDetails (JSON)
一个 JSON 对象，表示**Microsoft.Xbox.Services.Multiplayer.MultiplayerActivityDetails**。 

> [!NOTE] 
> 此对象由 2015年之多人游戏实现，并仅适用于该多玩家版本和更高版本。 它旨在用于模板协定 104/105 或更高版本。  

 
<a id="ID4ES"></a>

  
 
MultiplayerActivityDetails JSON 对象具有以下规范。
 
| 成员| 在任务栏的搜索框中键入| 描述| 
| --- | --- | --- | --- | 
| SessionReference| MultiplayerSessionReference| 一个<b>Microsoft.Xbox.Services.Multiplayer.MultiplayerSessionReference</b>对象，表示会话的标识信息。| 
| HandleId| 64 位无符号的整数| 向活动相对应的句柄 ID。| 
| TitleId| 32 位无符号的整数| 应启动才能加入活动标题 ID。| 
| 可见性| MultiplayerSessionVisibility| 一个<b>Microsoft.Xbox.Services.Multiplayer.MultiplayerSessionVisibility</b>值，该值指示该会话的可见性状态。| 
| JoinRestriction| MultiplayerSessionJoinRestriction| 一个<b>Microsoft.Xbox.Services.Multiplayer.MultiplayerSessionJoinRestriction</b>值，该值指示该会话的联接限制。 如果可见性字段设置为"打开"，应用此限制。| 
| 关闭| 布尔值| 如果在会话临时关闭加入，和 false 否则，则为 true。| 
| OwnerXboxUserId| 64 位无符号的整数| Xbox 拥有该活动的成员的用户 ID。| 
| MaxMembersCount| 32 位无符号的整数| 总的槽数。| 
| MembersCount| 32 位无符号的整数| 占用的槽数。| 
  
<a id="ID4E3D"></a>

 
## <a name="sample-json-syntax"></a>示例 JSON 语法
 

```json
{
  "results": [{
    "id": "11111111-ebe0-42da-885f-033860a818f6",
    "type": "activity",
    "version": 1,
    "sessionRef": {
      "scid": "8dfb0100-ebe0-42da-885f-033860a818f6",
      "templateName": "party",
      "name": "e3a836aeac6f4cbe9bcab985494d3175"
    },
    "titleId": "1234567",
    "ownerXuid": "3212",

    // Only if ?include=relatedInfo
    "relatedInfo": {
      "visibility": "open",
      "joinRestriction": "followed",
      "closed": true,
      "maxMembersCount": 8,
      "membersCount": 4,
    }
  },
  {
    "id": "11111111-ebe0-42da-885f-033860a818f7",
    "type": "activity",
    "version": 1,
    "sessionRef": {
      "scid": "8dfb0100-ebe0-42da-885f-033860a818f6",
      "templateName": "TitleStorageTestDefault",
      "name": "795fcaa7-8377-4281-bd7e-e86c12843632"
    },
    "titleId": "1234567",
    "ownerXuid": "3212",

    // Only if ?include=relatedInfo
    "relatedInfo": {
      "visibility": "open",
      "joinRestriction": "followed",
      "closed": false,
      "maxMembersCount": 8,
      "membersCount": 4,
    }
  }]
}
    
```

  
<a id="ID4EFE"></a>

 
## <a name="see-also"></a>另请参阅
 
<a id="ID4EHE"></a>

 
##### <a name="parent"></a>Parent 的子磁盘） 

[JavaScript 对象表示法 (JSON) 对象引用](atoc-xboxlivews-reference-json.md)

   