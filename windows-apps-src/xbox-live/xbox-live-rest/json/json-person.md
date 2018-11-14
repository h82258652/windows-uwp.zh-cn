---
title: Person (JSON)
assetID: b49234b1-03cd-f16e-c293-c74174382167
permalink: en-us/docs/xboxlive/rest/json-person.html
author: KevinAsgari
description: " Person (JSON)"
ms.author: kevinasg
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 69ab54fce4631e8b1d0c5361863078d28d0cf3f2
ms.sourcegitcommit: f2c9a050a9137a473f28b613968d5782866142c6
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/10/2018
ms.locfileid: "6281026"
---
# <a name="person-json"></a>Person (JSON)
人脉系统中的单个用户相关的元数据。 
<a id="ID4EN"></a>

 
## <a name="person"></a>联系人
 
Person 对象具有以下规范。
 
| 成员| 类型| 说明| 
| --- | --- | --- | 
| xuid| 字符串| 必需。 Xbox 用户 ID (XUID) 十进制格式。 示例值： 2603643534573573。| 
| isFavorite| 布尔值| 必需。 是否此人是一种用户关注的详细信息。 因为用户可以在其人脉列表中有大量的用户，应在体验优先级和先于其他不是收藏夹显示常用联系人。| 
| isFollowingCaller| 布尔值| 可选。 是否此人是遵循用户的名义 API 调用。| 
| socialNetworks| 字符串的数组| 可选。 内的外部网络用户，并且此人具有关系。| 
  
<a id="ID4EHC"></a>

 
## <a name="sample-json-syntax"></a>JSON 语法示例
 

```json
{
    "xuid": "2603643534573581",
    "isFavorite": false,
    "isFollowingCaller": false,
    "socialNetworks": ["LegacyXboxLive"]
}
    
```

  
<a id="ID4EQC"></a>

 
## <a name="see-also"></a>另请参阅
 
<a id="ID4ESC"></a>

 
##### <a name="parent"></a>Parent 的子磁盘） 

[JavaScript 对象表示法 (JSON) 对象参考](atoc-xboxlivews-reference-json.md)

  
<a id="ID4E3C"></a>

 
##### <a name="reference"></a>参考 

[/users/{ownerId}/people/{targetid}](../uri/people/uri-usersowneridpeopletargetid.md)

 [/users/{ownerId}/people/xuids](../uri/people/uri-usersowneridpeoplexuids.md)

   