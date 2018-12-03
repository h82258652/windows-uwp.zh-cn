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
ms.sourcegitcommit: d2517e522cacc5240f7dffd5bc1eaa278e3f7768
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/03/2018
ms.locfileid: "8344510"
---
# <a name="quotainfo-json"></a>quotaInfo (JSON)
包含有关游戏组的配额信息。 
<a id="ID4EN"></a>

 
## <a name="quotainfo"></a>quotaInfo
 
QuotaInfo 对象具有以下规范。
 
用于全局存储
 
| 成员| 类型| 描述| 
| --- | --- | --- | 
| quotaBytes| 32 位有符号整数 | 最大可用由游戏的字节数。| 
| usedBytes| 32 位有符号整数 | 游戏使用的字节数。| 
  
<a id="ID4EXB"></a>

 
## <a name="sample-json-syntax"></a>JSON 语法示例
 
以下示例演示了用于全局存储响应：
 

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

   