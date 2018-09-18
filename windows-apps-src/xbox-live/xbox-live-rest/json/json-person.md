---
title: Person (JSON)
assetID: b49234b1-03cd-f16e-c293-c74174382167
permalink: en-us/docs/xboxlive/rest/json-person.html
author: KevinAsgari
description: " Person (JSON)"
ms.author: kevinasg
ms.date: 20-12-2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 7e8e4ac4e91c4359ca20822297ccb625d09e3d59
ms.sourcegitcommit: f5321b525034e2b3af202709e9b942ad5557e193
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/18/2018
ms.locfileid: "4018972"
---
# <a name="person-json"></a>Person (JSON)
有关单个人员人脉系统中的元数据。 
<a id="ID4EN"></a>

 
## <a name="person"></a>联系人
 
人对象具有以下规范。
 
| 成员| 类型| 说明| 
| --- | --- | --- | 
| xuid| 字符串| 必需。 Xbox 用户 ID (XUID) 十进制格式。 示例值： 2603643534573573。| 
| isFavorite| 布尔值| 必需。 是否此人是一种用户关注的详细信息。 因为用户可以在其人脉列表中有大量的用户，应在体验优先和显示之前的不是收藏夹其他常用联系人。| 
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

   