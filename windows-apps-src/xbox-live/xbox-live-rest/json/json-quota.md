---
title: quotaInfo (JSON)
assetID: 3147ab78-e671-e142-0a3a-688dc4079451
permalink: en-us/docs/xboxlive/rest/json-quota.html
author: KevinAsgari
description: " quotaInfo (JSON)"
ms.author: kevinasg
ms.date: 20-12-2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 4308c148a530233e06d666da5ec446821ba6ee26
ms.sourcegitcommit: 4b97117d3aff38db89d560502a3c372f12bb6ed5
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/24/2018
ms.locfileid: "5443721"
---
# <a name="quotainfo-json"></a>quotaInfo (JSON)
包含有关标题组的配额信息。 
<a id="ID4EN"></a>

 
## <a name="quotainfo"></a>quotaInfo
 
QuotaInfo 对象具有以下规范。
 
用于全局存储
 
| 成员| 类型| 说明| 
| --- | --- | --- | 
| quotaBytes| 32 位有符号整数 | 最大可用的标题的字节数。| 
| usedBytes| 32 位有符号整数 | 游戏使用的字节数。| 
  
<a id="ID4EXB"></a>

 
## <a name="sample-json-syntax"></a>JSON 语法示例
 
以下示例演示了全局存储的响应：
 

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

[JavaScript 对象表示法 (JSON) 对象参考](atoc-xboxlivews-reference-json.md)

   