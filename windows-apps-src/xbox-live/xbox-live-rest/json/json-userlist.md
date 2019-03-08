---
title: UserList (JSON)
assetID: 894f5a39-4eed-0c5b-fc45-cb0097dc46fd
permalink: en-us/docs/xboxlive/rest/json-userlist.html
description: " UserList (JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 46e5323f4eae8e91b61295c4112b5bacfc8a1759
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2019
ms.locfileid: "57598492"
---
# <a name="userlist-json"></a>UserList (JSON)
一系列[用户](json-user.md)对象。 
<a id="ID4ER"></a>

 
## <a name="userlist"></a>UserList
 
UserList 对象具有以下规范。
 
| 成员| 在任务栏的搜索框中键入| 描述| 
| --- | --- | --- | 
| 用户| 数组[用户 (JSON)](json-user.md)| 请参阅下面的 JSON 示例。| 
  
<a id="ID4EPB"></a>

 
## <a name="sample-json-syntax"></a>示例 JSON 语法
 

```json
{
    "users":
    [
        { "xuid":"12345" },
        { "xuid":"23456" }
    ] 
}
    
```

  
<a id="ID4EYB"></a>

 
## <a name="see-also"></a>另请参阅
 
<a id="ID4E1B"></a>

 
##### <a name="parent"></a>Parent 的子磁盘） 

[JavaScript 对象表示法 (JSON) 对象引用](atoc-xboxlivews-reference-json.md)

   