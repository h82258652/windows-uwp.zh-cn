---
title: XuidList (JSON)
assetID: 06938a52-e582-a15b-ec7f-4b053dfc28ad
permalink: en-us/docs/xboxlive/rest/json-xuidlist.html
description: " XuidList (JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 1d8172063d40f8df77827ab845c4dfd0c0799811
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2019
ms.locfileid: "57627912"
---
# <a name="xuidlist-json"></a>XuidList (JSON)
要对其执行操作的 XUIDs 列表。 
<a id="ID4EN"></a>

 
## <a name="xuidlist"></a>XuidList
 
XuidList 对象具有以下规范。
 
| 成员| 在任务栏的搜索框中键入| 描述| 
| --- | --- | --- | 
| xuids| 字符串数组| Xbox 用户 ID (XUID) 值在其应执行的操作，或应返回数据的列表。| 
  
<a id="ID4EMB"></a>

 
## <a name="sample-json-syntax"></a>示例 JSON 语法
 

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

[JavaScript 对象表示法 (JSON) 对象引用](atoc-xboxlivews-reference-json.md)

  
<a id="ID4EBC"></a>

 
##### <a name="reference"></a>参考 

[POST (/users/{ownerId}/people/xuids)](../uri/people/uri-usersowneridpeoplexuidspost.md)

   