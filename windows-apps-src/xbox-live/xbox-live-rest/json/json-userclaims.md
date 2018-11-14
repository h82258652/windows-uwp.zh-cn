---
title: UserClaims (JSON)
assetID: f88d5ee0-2875-fcfb-3098-3cd6afce8748
permalink: en-us/docs/xboxlive/rest/json-userclaims.html
author: KevinAsgari
description: " UserClaims (JSON)"
ms.author: kevinasg
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 718c5847a1588e492c5d290d042dd4d5a9ca4283
ms.sourcegitcommit: f2c9a050a9137a473f28b613968d5782866142c6
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/10/2018
ms.locfileid: "6257962"
---
# <a name="userclaims-json"></a>UserClaims (JSON)
返回当前身份验证的用户信息。 
<a id="ID4EN"></a>

 
## <a name="userclaims"></a>UserClaims
 
UserClaims 对象具有以下规范。
 
| 成员| 类型| 说明| 
| --- | --- | --- | 
| 玩家代号| 字符串| 用户的玩家代号。| 
| xuid| 64 位无符号的整数| Xbox 用户 ID (XUID) 的用户。| 
  
<a id="ID4EZB"></a>

 
## <a name="sample-json-syntax"></a>JSON 语法示例
 

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

[JavaScript 对象表示法 (JSON) 对象参考](atoc-xboxlivews-reference-json.md)

   