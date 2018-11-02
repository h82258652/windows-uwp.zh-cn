---
title: PermissionCheckBatchRequest (JSON)
assetID: 3100b17c-1b60-fdf2-3af9-7e424f1903ee
permalink: en-us/docs/xboxlive/rest/json-permissioncheckbatchrequest.html
author: KevinAsgari
description: " PermissionCheckBatchRequest (JSON)"
ms.author: kevinasg
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: d781f472dc9179f25002d3e103af2cedf3ebd931
ms.sourcegitcommit: 70ab58b88d248de2332096b20dbd6a4643d137a4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/02/2018
ms.locfileid: "5944089"
---
# <a name="permissioncheckbatchrequest-json"></a>PermissionCheckBatchRequest (JSON)
PermissionCheckBatchRequest 对象的集合。 
<a id="ID4EP"></a>

 
## <a name="permissioncheckbatchrequest"></a>PermissionCheckBatchRequest
 
PermissionCheckBatchRequest 对象具有以下规范。
 
| 成员| 类型| 说明| 
| --- | --- | --- | 
| 用户| 用户的数组| 必需。 目标来检查针对权限的数组。 此数组中的每个条目是 Xbox 用户 ID (XUID) 或网络关闭匿名用户跨网络方案 ("anonymousUser":"allUsers")。 | 
| 权限| [PermissionId 枚举](../enums/privacy-enum-permissionid.md)的数组| 必需。 要检查每个用户的权限。| 
  
<a id="ID4E3B"></a>

 
## <a name="sample-json-syntax"></a>JSON 语法示例
 

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

[JavaScript 对象表示法 (JSON) 对象参考](atoc-xboxlivews-reference-json.md)

   