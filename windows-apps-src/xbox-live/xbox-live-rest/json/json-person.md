---
title: 人 (JSON)
assetID: b49234b1-03cd-f16e-c293-c74174382167
permalink: en-us/docs/xboxlive/rest/json-person.html
author: KevinAsgari
description: " 人 (JSON)"
ms.author: kevinasg
ms.date: 20-12-2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 7e8e4ac4e91c4359ca20822297ccb625d09e3d59
ms.sourcegitcommit: c8f6866100a4b38fdda8394ea185b02d7af66411
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/13/2018
ms.locfileid: "3959120"
---
# <a name="person-json"></a>人 (JSON)
人脉系统中的单个用户相关的元数据。 
<a id="ID4EN"></a>

 
## <a name="person"></a>联系人
 
Person 对象具有以下规范。
 
| 成员| 类型| 说明| 
| --- | --- | --- | 
| xuid| 字符串| 必需。 Xbox 用户 ID (XUID) 以十进制格式。 示例值： 2603643534573573。| 
| isFavorite| 布尔值| 必需。 是否此人是一种用户关心的详细信息。 因为用户可以在其人脉列表中有大量的用户，应在体验优先级和先于其他不是收藏夹显示常用联系人。| 
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

[JavaScript 对象表示法 (JSON) 对象引用](atoc-xboxlivews-reference-json.md)

  
<a id="ID4E3C"></a>

 
##### <a name="reference"></a>参考 

[/users/ {ownerId} /people/ {targetid}](../uri/people/uri-usersowneridpeopletargetid.md)

 [/users/ {ownerId} / 人/xuid](../uri/people/uri-usersowneridpeoplexuids.md)

   