---
title: LastSeenRecord (JSON)
assetID: 6a93202c-801c-03c6-8386-6acd0f366780
permalink: en-us/docs/xboxlive/rest/json-lastseenrecord.html
author: KevinAsgari
description: " LastSeenRecord (JSON)"
ms.author: kevinasg
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 8d4889ced5f8942c080b3336bda8c0d8d9b25af2
ms.sourcegitcommit: cd00bb829306871e5103db481cf224ea7fb613f0
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/01/2018
ms.locfileid: "5859354"
---
# <a name="lastseenrecord-json"></a>LastSeenRecord (JSON)
有关系统上次看到的用户，当用户在没有有效 DeviceRecord 提供的信息。 
<a id="ID4EN"></a>

 
## <a name="lastseenrecord"></a>LastSeenRecord
 
LastSeenRecord 对象具有以下规范。
 
| 成员| 类型| 说明| 
| --- | --- | --- | 
| deviceType| 字符串| 用户在其上已过去存在的设备类型。| 
| titleId| 32 位无符号的整数| 游戏的用户的最后一个存在的标识符。| 
| titleName| 字符串| 用户在其已过去存在的标题的名称。| 
| 时间戳| DateTime| 用于指示已过去存在用户的 UTC 时间戳。| 
  
<a id="ID4EHC"></a>

 
## <a name="sample-json-syntax"></a>JSON 语法示例
 

```json
{
  deviceType:W8,    
  titleId:"23452345",
  titleName:"My Awesome Game",
  timestamp:"2012-09-17T07:15:23.4930000"
}
    
```

  
<a id="ID4EQC"></a>

 
## <a name="see-also"></a>另请参阅
 
<a id="ID4ESC"></a>

 
##### <a name="parent"></a>Parent 的子磁盘） 

[JavaScript 对象表示法 (JSON) 对象参考](atoc-xboxlivews-reference-json.md)

  
<a id="ID4E5C"></a>

 
##### <a name="reference"></a>参考   