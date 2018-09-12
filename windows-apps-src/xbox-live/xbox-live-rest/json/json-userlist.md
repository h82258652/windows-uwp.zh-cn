---
title: 用户列表 (JSON)
assetID: 894f5a39-4eed-0c5b-fc45-cb0097dc46fd
permalink: en-us/docs/xboxlive/rest/json-userlist.html
author: KevinAsgari
description: " 用户列表 (JSON)"
ms.author: kevinasg
ms.date: 20-12-2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 51dd19ebed394bb0c3c8b5f4649dd5c83a58027c
ms.sourcegitcommit: 2a63ee6770413bc35ace09b14f56b60007be7433
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/12/2018
ms.locfileid: "3934375"
---
# <a name="userlist-json"></a>用户列表 (JSON)
[用户](json-user.md)对象的集合。 
<a id="ID4ER"></a>

 
## <a name="userlist"></a>用户列表
 
用户列表对象具有以下规范。
 
| 成员| 类型| 说明| 
| --- | --- | --- | 
| 用户| [用户 (JSON)](json-user.md)的数组| 请参阅 JSON 下面的示例。| 
  
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

[JavaScript 对象表示法 (JSON) 对象引用](atoc-xboxlivews-reference-json.md)

   