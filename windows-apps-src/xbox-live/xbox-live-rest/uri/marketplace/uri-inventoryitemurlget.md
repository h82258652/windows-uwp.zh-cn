---
title: 获取 (/inventory/ {itemID})
assetID: d3ca14a5-0214-ef42-091e-3f05f2a3482d
permalink: en-us/docs/xboxlive/rest/uri-inventoryitemurlget.html
author: KevinAsgari
description: " 获取 (/inventory/ {itemID})"
ms.author: kevinasg
ms.date: 20-12-2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 4a94493243178a503ae846608b172af598bf97dd
ms.sourcegitcommit: 2a63ee6770413bc35ace09b14f56b60007be7433
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/12/2018
ms.locfileid: "3933461"
---
# <a name="get-inventoryitemid"></a>获取 (/inventory/ {itemID})
提供特定的清单项完整的详细信息集。 这些 Uri 的域是`inventory.xboxlive.com`。
 
  * [备注](#ID4EX)
  * [URI 参数](#ID4EAB)
  * [响应正文](#ID4ELB)
 
<a id="ID4EX"></a>

 
## <a name="remarks"></a>备注
 
没有策略检查，强制执行，否则筛选会出现作为此调用的一部分。
  
<a id="ID4EAB"></a>

 
## <a name="uri-parameters"></a>URI 参数
 
| 参数| 类型| 说明| 
| --- | --- | --- | 
| itemID| 字符串| 唯一单数库存项目的每个用户的 ID| 
  
<a id="ID4ELB"></a>

 
## <a name="response-body"></a>响应正文
 
<a id="ID4ERB"></a>

 
### <a name="sample-response"></a>示例响应
 
GET 请求，假设传递身份验证并分配适当授权上下文中，该响应是具有完整的项目属性集的单个清单项。
 

```cpp
{inventoryItem}
         
```

   
<a id="ID4E4B"></a>

 
## <a name="see-also"></a>另请参阅
 
<a id="ID4E6B"></a>

 
##### <a name="parent"></a>Parent 的子磁盘） 

[获取 (/inventory/ {itemID})]()

  
<a id="ID4EJC"></a>

 
##### <a name="further-information"></a>详细信息 

[EDS 公共标头](../../additional/edscommonheaders.md)

 [EDS 参数](../../additional/edsparameters.md)

 [EDS 查询精简将](../../additional/edsqueryrefiners.md)

 [市场 Uri](atoc-reference-marketplace.md)

 [其他参考](../../additional/atoc-xboxlivews-reference-additional.md)

   