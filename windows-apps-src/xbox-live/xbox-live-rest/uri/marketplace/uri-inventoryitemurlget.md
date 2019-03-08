---
title: GET (/inventory/{itemID})
assetID: d3ca14a5-0214-ef42-091e-3f05f2a3482d
permalink: en-us/docs/xboxlive/rest/uri-inventoryitemurlget.html
description: " GET (/inventory/{itemID})"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 446197eb20820304088ddac4a6379fa3b2510873
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2019
ms.locfileid: "57606692"
---
# <a name="get-inventoryitemid"></a>GET (/inventory/{itemID})
提供特定清单项的详细信息的完整集。 这些 Uri 的域是`inventory.xboxlive.com`。
 
  * [备注](#ID4EX)
  * [URI 参数](#ID4EAB)
  * [响应正文](#ID4ELB)
 
<a id="ID4EX"></a>

 
## <a name="remarks"></a>备注
 
无策略检查，强制执行，或筛选将发生此调用的一部分。
  
<a id="ID4EAB"></a>

 
## <a name="uri-parameters"></a>URI 参数
 
| 参数| 在任务栏的搜索框中键入| 描述| 
| --- | --- | --- | 
| itemID| 字符串| 唯一的单数形式的清单项的每个用户 ID| 
  
<a id="ID4ELB"></a>

 
## <a name="response-body"></a>响应正文
 
<a id="ID4ERB"></a>

 
### <a name="sample-response"></a>示例响应
 
对 GET 请求中，假定它将传递身份验证，并分配适当的授权上下文的响应是项属性的完整集的单个清单项。
 

```cpp
{inventoryItem}
         
```

   
<a id="ID4E4B"></a>

 
## <a name="see-also"></a>另请参阅
 
<a id="ID4E6B"></a>

 
##### <a name="parent"></a>Parent 的子磁盘） 

[GET (/inventory/{itemID})](uri-inventoryget.md)

  
<a id="ID4EJC"></a>

 
##### <a name="further-information"></a>详细信息 

[EDS 常见标头](../../additional/edscommonheaders.md)

 [EDS 参数](../../additional/edsparameters.md)

 [EDS 查询精简将](../../additional/edsqueryrefiners.md)

 [Marketplace Uri](atoc-reference-marketplace.md)

 [其他参考](../../additional/atoc-xboxlivews-reference-additional.md)

   