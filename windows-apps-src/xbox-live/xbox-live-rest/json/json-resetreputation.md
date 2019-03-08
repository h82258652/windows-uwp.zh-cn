---
title: ResetReputation (JSON)
assetID: 15edb5e7-a00b-4188-9b49-9db5774c4a10
permalink: en-us/docs/xboxlive/rest/json-resetreputation.html
description: " ResetReputation (JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: d09c8bbc1130f91dfea3d4c35e391dcf9adcf127
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2019
ms.locfileid: "57649412"
---
# <a name="resetreputation-json"></a>ResetReputation (JSON)
包含用户的现有分数应更改到新的基本信誉分数。 
<a id="ID4EN"></a>

 
## <a name="resetreputation"></a>ResetReputation
 
ResetReputation 对象具有以下规范。
 
| 成员| 在任务栏的搜索框中键入| 描述| 
| --- | --- | --- | 
| fairplayReputation| 数字| 所需新的基本用户 （有效范围 0 到 75） Fairplay 名声分值。| 
| commsReputation| 数字| 所需新的基本用户 （有效范围 0 到 75） 的通信方式名声分值。| 
| userContentReputation| 数字| 所需新的基本用户 （有效范围 0 到 75） UserContent 名声分值。| 
  
<a id="ID4E4B"></a>

 
## <a name="sample-json-syntax"></a>示例 JSON 语法
 

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

[JavaScript 对象表示法 (JSON) 对象引用](atoc-xboxlivews-reference-json.md)

   