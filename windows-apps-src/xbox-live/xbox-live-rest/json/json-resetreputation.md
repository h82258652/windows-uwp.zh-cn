---
title: ResetReputation (JSON)
assetID: 15edb5e7-a00b-4188-9b49-9db5774c4a10
permalink: en-us/docs/xboxlive/rest/json-resetreputation.html
author: KevinAsgari
description: " ResetReputation (JSON)"
ms.author: kevinasg
ms.date: 20-12-2017
ms.topic: article
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 1c2462419559ad179b2824b2735a5aad4ee63725
ms.sourcegitcommit: 6cc275f2151f78db40c11ace381ee2d35f0155f9
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/26/2018
ms.locfileid: "5556544"
---
# <a name="resetreputation-json"></a>ResetReputation (JSON)
包含新的基本信誉评分应更改用户的现有评分。 
<a id="ID4EN"></a>

 
## <a name="resetreputation"></a>ResetReputation
 
ResetReputation 对象具有以下规范。
 
| 成员| 类型| 说明| 
| --- | --- | --- | 
| fairplayReputation| 数字| 所需新基础 （有效范围为 0 到 75） 的用户的公平比赛信誉评分。| 
| commsReputation| 数字| 所需新基础 （有效范围为 0 到 75） 的用户的通信和信誉评分。| 
| userContentReputation| 数字| 所需新基础 UserContent 信誉评分，用户 （有效范围为 0 到 75）。| 
  
<a id="ID4E4B"></a>

 
## <a name="sample-json-syntax"></a>JSON 语法示例
 

```json
{
    "fairplayReputation": 75,
    "commsReputation": 75,
    "userContentReputation": 75
}
    
```

  
<a id="ID4EGC"></a>

 
## <a name="see-also"></a>另请参阅
 
<a id="ID4EIC"></a>

 
##### <a name="parent"></a>Parent 的子磁盘） 

[JavaScript 对象表示法 (JSON) 对象参考](atoc-xboxlivews-reference-json.md)

   