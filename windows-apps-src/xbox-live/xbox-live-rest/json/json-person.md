---
title: Person (JSON)
assetID: b49234b1-03cd-f16e-c293-c74174382167
permalink: en-us/docs/xboxlive/rest/json-person.html
description: " Person (JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 175d66ffc7744ca8203fe7681fcb0167e150f012
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2019
ms.locfileid: "57640862"
---
# <a name="person-json"></a>Person (JSON)
用户系统中的单个用户相关的元数据。 
<a id="ID4EN"></a>

 
## <a name="person"></a>Person
 
Person 对象具有以下规范。
 
| 成员| 在任务栏的搜索框中键入| 描述| 
| --- | --- | --- | 
| xuid| 字符串| 必需。 Xbox 用户 ID (XUID) 以十进制格式。 示例值：2603643534573573.| 
| isFavorite| 布尔值| 必需。 无论此人是一个用户关心的详细信息。 因为用户可以非常大量的人在其用户列表中，应优先体验中并先于其他不是收藏夹显示常用联系人。| 
| isFollowingCaller| 布尔值| 可选。 此人是关注用户的用户是否以其名义 API 调用。| 
| socialNetworks| 字符串数组| 可选。 中的外部网络用户和此人具有关系。| 
  
<a id="ID4EHC"></a>

 
## <a name="sample-json-syntax"></a>示例 JSON 语法
 

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

[/users/{ownerId}/people/{targetid}](../uri/people/uri-usersowneridpeopletargetid.md)

 [/users/{ownerId}/people/xuids](../uri/people/uri-usersowneridpeoplexuids.md)

   