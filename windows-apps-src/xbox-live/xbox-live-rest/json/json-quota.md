---
title: quotaInfo (JSON)
assetID: 3147ab78-e671-e142-0a3a-688dc4079451
permalink: en-us/docs/xboxlive/rest/json-quota.html
description: " quotaInfo (JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: f3499fdba972d6e953813fc490d080910921698e
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2019
ms.locfileid: "57654112"
---
# <a name="quotainfo-json"></a>quotaInfo (JSON)
包含有关标题组的配额信息。 
<a id="ID4EN"></a>

 
## <a name="quotainfo"></a>quotaInfo
 
QuotaInfo 对象具有以下规范。
 
有关全局存储
 
| 成员| 在任务栏的搜索框中键入| 描述| 
| --- | --- | --- | 
| quotaBytes| 32 位有符号的整数 | 最大标题可用字节数。| 
| usedBytes| 32 位有符号的整数 | 标题使用的字节数。| 
  
<a id="ID4EXB"></a>

 
## <a name="sample-json-syntax"></a>示例 JSON 语法
 
下面的示例演示了全局存储响应：
 

```json
{
    "quotaInfo":
    {
        "usedBytes":4194304,
        "quotaBytes":536870912
    }
}
      
```

  
<a id="ID4ECC"></a>

 
## <a name="see-also"></a>另请参阅
 
<a id="ID4EEC"></a>

 
##### <a name="parent"></a>Parent 的子磁盘） 

[JavaScript 对象表示法 (JSON) 对象引用](atoc-xboxlivews-reference-json.md)

   