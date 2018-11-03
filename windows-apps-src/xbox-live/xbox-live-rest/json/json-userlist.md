---
title: UserList (JSON)
assetID: 894f5a39-4eed-0c5b-fc45-cb0097dc46fd
permalink: en-us/docs/xboxlive/rest/json-userlist.html
author: KevinAsgari
description: " UserList (JSON)"
ms.author: kevinasg
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: fdbf983413ef68bb3afa0ca6f23fc182a6fece5e
ms.sourcegitcommit: 144f5f127fc4fbd852f2f6780ef26054192d68fc
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/03/2018
ms.locfileid: "5971856"
---
# <a name="userlist-json"></a>UserList (JSON)
[用户](json-user.md)对象的集合。 
<a id="ID4ER"></a>

 
## <a name="userlist"></a>UserList
 
UserList 对象具有以下规范。
 
| 成员| 类型| 说明| 
| --- | --- | --- | 
| 用户| [User (JSON)](json-user.md)的数组| 请参阅 JSON 下面的示例。| 
  
<a id="ID4EPB"></a>

 
## <a name="sample-json-syntax"></a>JSON 语法示例
 

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

[JavaScript 对象表示法 (JSON) 对象参考](atoc-xboxlivews-reference-json.md)

   