---
title: MultiplayerActivityDetails (JSON)
assetID: f982aa5e-2694-4ef9-bc55-6c099a3cf9ec
permalink: en-us/docs/xboxlive/rest/json-multiplayeractivitydetails.html
author: KevinAsgari
description: " MultiplayerActivityDetails (JSON)"
ms.author: kevinasg
ms.date: 20-12-2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 4de72a24c34af1a5f145c44b2acfa11a7bd07f95
ms.sourcegitcommit: 5c9a47b135c5f587214675e39c1ac058c0380f4c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/04/2018
ms.locfileid: "4358661"
---
# <a name="multiplayeractivitydetails-json"></a>MultiplayerActivityDetails (JSON)
表示**Microsoft.Xbox.Services.Multiplayer.MultiplayerActivityDetails**的 JSON 对象。 

> [!NOTE] 
> 此对象由 2015年多人游戏和应用仅向该多人游戏版本及更高版本。 它旨在与模板合约 104/105 或更高版本一起使用。  

 
<a id="ID4ES"></a>

  
 
MultiplayerActivityDetails JSON 对象具有以下规范。
 
| 成员| 类型| 描述| 
| --- | --- | --- | --- | 
| SessionReference| MultiplayerSessionReference| 表示会话的标识信息的<b>Microsoft.Xbox.Services.Multiplayer.MultiplayerSessionReference</b>对象。| 
| HandleId| 64 位无符号的整数| 对应于活动句柄 ID。| 
| TitleId| 32 位无符号的整数| 应启动才能加入活动主题作品 ID。| 
| 可见性| MultiplayerSessionVisibility| 指示会话的可见性状态的<b>Microsoft.Xbox.Services.Multiplayer.MultiplayerSessionVisibility</b>值。| 
| JoinRestriction| MultiplayerSessionJoinRestriction| 指示会话加入限制的<b>Microsoft.Xbox.Services.Multiplayer.MultiplayerSessionJoinRestriction</b>值。 如果可见性字段设置为"打开"，将应用此限制。| 
| 已关闭| 布尔值| 如果会话暂时关闭为加入，以及 false 否则，则为 true。| 
| OwnerXboxUserId| 64 位无符号的整数| 拥有活动的成员的 Xbox 用户 ID。| 
| MaxMembersCount| 32 位无符号的整数| 总插槽数。| 
| MembersCount| 32 位无符号的整数| 占用的插槽数。| 
  
<a id="ID4E3D"></a>

 
## <a name="sample-json-syntax"></a>JSON 语法示例
 

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

[JavaScript 对象表示法 (JSON) 对象参考](atoc-xboxlivews-reference-json.md)

   