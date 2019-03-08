---
title: Profile (JSON)
assetID: b92b1750-c2df-39b6-6c5c-f9e8068c8097
permalink: en-us/docs/xboxlive/rest/json-profile.html
description: " Profile (JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 7299fcb4d375a3fc35ad67306b70f5fa4afde963
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2019
ms.locfileid: "57607512"
---
# <a name="profile-json"></a>Profile (JSON)
用户个人配置文件设置。 
<a id="ID4EN"></a>

 
## <a name="profile"></a>配置文件
 
该配置文件对象具有以下规范。
 
| 成员| 在任务栏的搜索框中键入| 描述| 
| --- | --- | --- | 
| AppDisplayName| 字符串| 在应用中显示的名称。 这可能是用户的"实际名称"或其玩家代号，具体取决于隐私。 此设置表示应该用于显示在应用中的用户的标识字符串。| 
| GameDisplayName| 字符串| 在游戏中显示的名称。 这可能是用户的"实际名称"或其玩家代号，具体取决于隐私。 此设置表示应该用于显示在游戏中的用户的标识字符串。| 
| Gamertag| 字符串| 用户的玩家代号。| 
| AppDisplayPicRaw| 字符串| 原始应用程序显示 pic URL （见下文）。| 
| GameDisplayPicRaw| 字符串| 原始游戏显示 pic URL （见下文）。| 
| AccountTier| 字符串| 用户具有的帐户类型？ 金牌、 银牌或 FamilyGold？| 
| TenureLevel| 32 位无符号的整数| 用户已在 Xbox Live 数年？| 
| 玩家分数| 32 位无符号的整数| 用户的 Gamerscore。| 
  


> [!NOTE] 
> 图片可以是用户的真实的图片或其 XboxOne gamerpic，具体取决于隐私。 这些设置表示应该用于在客户端上显示的用户的图片 url。 此映像可能为空 （指示用户尚未设置任何图片）。 


 
原始 URL 是可调整大小的 URL。 它可用于指定下列任一调整大小和格式使用通过追加`&format={format}&w={width}&h={height}`到 URI:
 
格式： png
 
大小：64 x 64、 208 x 208、 424 x 424
 
<a id="ID4E2D"></a>

 
## <a name="see-also"></a>另请参阅
 
<a id="ID4E4D"></a>

 
##### <a name="parent"></a>Parent 的子磁盘） 

[JavaScript 对象表示法 (JSON) 对象引用](atoc-xboxlivews-reference-json.md)

   