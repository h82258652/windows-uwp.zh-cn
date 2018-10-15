---
title: XuidList (JSON)
assetID: 06938a52-e582-a15b-ec7f-4b053dfc28ad
permalink: en-us/docs/xboxlive/rest/json-xuidlist.html
author: KevinAsgari
description: " XuidList (JSON)"
ms.author: kevinasg
ms.date: 20-12-2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 3853140ce5e7c3f7710f489709945fc70b6703b4
ms.sourcegitcommit: 106aec1e59ba41aae2ac00f909b81bf7121a6ef1
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/15/2018
ms.locfileid: "4611204"
---
# <a name="xuidlist-json"></a>XuidList (JSON)
要对其执行操作的 Xuid 列表。 
<a id="ID4EN"></a>

 
## <a name="xuidlist"></a>XuidList
 
XuidList 对象具有以下规范。
 
| 成员| 类型| 描述| 
| --- | --- | --- | 
| xuid| 字符串的数组| Xbox 用户 ID (XUID) 值的应执行的操作，或应返回数据的列表。| 
  
<a id="ID4EMB"></a>

 
## <a name="sample-json-syntax"></a>JSON 语法示例
 

```json
{
    "xuids": [
        "2533274790395904", 
        "2533274792986770", 
        "2533274794866999"
    ]
}
    
```

  
<a id="ID4EVB"></a>

 
## <a name="see-also"></a>另请参阅
 
<a id="ID4EXB"></a>

 
##### <a name="parent"></a>Parent 的子磁盘） 

[JavaScript 对象表示法 (JSON) 对象参考](atoc-xboxlivews-reference-json.md)

  
<a id="ID4EBC"></a>

 
##### <a name="reference"></a>参考 

[POST (/users/{ownerId}/people/xuids)](../uri/people/uri-usersowneridpeoplexuidspost.md)

   