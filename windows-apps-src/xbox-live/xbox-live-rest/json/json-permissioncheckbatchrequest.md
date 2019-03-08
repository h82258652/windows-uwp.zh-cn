---
title: PermissionCheckBatchRequest (JSON)
assetID: 3100b17c-1b60-fdf2-3af9-7e424f1903ee
permalink: en-us/docs/xboxlive/rest/json-permissioncheckbatchrequest.html
description: " PermissionCheckBatchRequest (JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 538547c85648970ab3e9fe3ae413e8a03df814ad
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2019
ms.locfileid: "57623502"
---
# <a name="permissioncheckbatchrequest-json"></a>PermissionCheckBatchRequest (JSON)
PermissionCheckBatchRequest 对象的集合。 
<a id="ID4EP"></a>

 
## <a name="permissioncheckbatchrequest"></a>PermissionCheckBatchRequest
 
PermissionCheckBatchRequest 对象具有以下规范。
 
| 成员| 在任务栏的搜索框中键入| 描述| 
| --- | --- | --- | 
| 用户| 用户的数组| 必需。 要检查针对权限的目标数组。 此数组中的每个条目是 Xbox 用户 ID (XUID) 或匿名关闭网络用户跨网络方案 ("anonymousUser":"allUsers")。 | 
| 权限| 数组[PermissionId 枚举](../enums/privacy-enum-permissionid.md)| 必需。 要检查针对每个用户的权限。| 
  
<a id="ID4E3B"></a>

 
## <a name="sample-json-syntax"></a>示例 JSON 语法
 

```json
{
    "users":
    [
        {"xuid":"12345"},
        {"xuid":"54321"}
    ],
    "permissions":
    [
        "ShareFriendList",
        "ShareGameHistory"
    ]
}
    
```

  
<a id="ID4EFC"></a>

 
## <a name="see-also"></a>另请参阅
 
<a id="ID4EHC"></a>

 
##### <a name="parent"></a>Parent 的子磁盘） 

[JavaScript 对象表示法 (JSON) 对象引用](atoc-xboxlivews-reference-json.md)

   