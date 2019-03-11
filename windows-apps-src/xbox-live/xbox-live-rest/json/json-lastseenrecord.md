---
title: LastSeenRecord (JSON)
assetID: 6a93202c-801c-03c6-8386-6acd0f366780
permalink: en-us/docs/xboxlive/rest/json-lastseenrecord.html
description: " LastSeenRecord (JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: e06de31cabaedb68ed57d3d4f2ff30614ceb6317
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2019
ms.locfileid: "57613372"
---
# <a name="lastseenrecord-json"></a>LastSeenRecord (JSON)
有关系统上次看到用户，在用户有无有效 DeviceRecord 时可用的信息。 
<a id="ID4EN"></a>

 
## <a name="lastseenrecord"></a>LastSeenRecord
 
LastSeenRecord 对象具有以下规范。
 
| 成员| 在任务栏的搜索框中键入| 描述| 
| --- | --- | --- | 
| deviceType| 字符串| 用户在其处于最后一个存在的设备的类型。| 
| titleId| 32 位无符号的整数| 标题的用户是最后一个存在的标识符。| 
| titleName| 字符串| 用户在其已存在最后一个标题的名称。| 
| timestamp| DateTime| 指示当用户最后一个存在的 UTC 时间戳。| 
  
<a id="ID4EHC"></a>

 
## <a name="sample-json-syntax"></a>示例 JSON 语法
 

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

[JavaScript 对象表示法 (JSON) 对象引用](atoc-xboxlivews-reference-json.md)

  
<a id="ID4E5C"></a>

 
##### <a name="reference"></a>参考   