---
title: UserClaims (JSON)
assetID: f88d5ee0-2875-fcfb-3098-3cd6afce8748
permalink: en-us/docs/xboxlive/rest/json-userclaims.html
author: KevinAsgari
description: " UserClaims (JSON)"
ms.author: kevinasg
ms.date: 20-12-2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: a5ec9878845b7d93cd4db18ff9825d728897a5c0
ms.sourcegitcommit: c4d3115348c8b54fcc92aae8e18fdabc3deb301d
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/22/2018
ms.locfileid: "5404045"
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

   