---
title: LastSeenRecord (JSON)
assetID: 6a93202c-801c-03c6-8386-6acd0f366780
permalink: en-us/docs/xboxlive/rest/json-lastseenrecord.html
author: KevinAsgari
description: " LastSeenRecord (JSON)"
ms.author: kevinasg
ms.date: 20-12-2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: de0ffd7c9c6c42f2a0ebf633ebcbba8a89a1b8b8
ms.sourcegitcommit: 1c6325aa572868b789fcdd2efc9203f67a83872a
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/18/2018
ms.locfileid: "4754896"
---
# <a name="lastseenrecord-json"></a>LastSeenRecord (JSON)
有关系统上次看到的用户，当用户在没有有效 DeviceRecord 可用的信息。 
<a id="ID4EN"></a>

 
## <a name="lastseenrecord"></a>LastSeenRecord
 
LastSeenRecord 对象具有以下规范。
 
| 成员| 类型| 说明| 
| --- | --- | --- | 
| deviceType| 字符串| 用户在其上已过去存在的设备类型。| 
| titleId| 32 位无符号的整数| 用户在其已过去存在标题的标识符。| 
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