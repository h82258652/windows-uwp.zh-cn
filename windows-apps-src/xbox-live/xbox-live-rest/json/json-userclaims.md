---
title: UserClaims (JSON)
assetID: f88d5ee0-2875-fcfb-3098-3cd6afce8748
permalink: en-us/docs/xboxlive/rest/json-userclaims.html
description: " UserClaims (JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 21b4322d002747145c3b09e0f3cd7eb03874380b
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2019
ms.locfileid: "57623342"
---
# <a name="userclaims-json"></a>UserClaims (JSON)
返回有关当前经过身份验证用户的信息。 
<a id="ID4EN"></a>

 
## <a name="userclaims"></a>UserClaims
 
UserClaims 对象具有以下规范。
 
| 成员| 在任务栏的搜索框中键入| 描述| 
| --- | --- | --- | 
| 玩家代号| 字符串| 用户的玩家代号。| 
| xuid| 64 位无符号的整数| Xbox 用户 ID (XUID) 的用户。| 
  
<a id="ID4EZB"></a>

 
## <a name="sample-json-syntax"></a>示例 JSON 语法
 

```json
{
   "xuid" : 2533274790412952,
   "gamertag" : "gamer"

}
    
```

  
<a id="ID4ECC"></a>

 
## <a name="see-also"></a>另请参阅
 
<a id="ID4EEC"></a>

 
##### <a name="parent"></a>Parent 的子磁盘） 

[JavaScript 对象表示法 (JSON) 对象引用](atoc-xboxlivews-reference-json.md)

   