---
title: PermissionCheckResponse (JSON)
assetID: 7a749001-b569-b0e0-2a35-f299474c8710
permalink: en-us/docs/xboxlive/rest/json-permissioncheckresponse.html
description: " PermissionCheckResponse (JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 7623a45fa21e30a015bd5c6a7c1f5add19cc189b
ms.sourcegitcommit: a3dc929858415b933943bba5aa7487ffa721899f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/07/2018
ms.locfileid: "8797908"
---
# <a name="permissioncheckresponse-json"></a>PermissionCheckResponse (JSON)
从单个权限设置针对单个目标用户的单个用户检查的结果。 
<a id="ID4EN"></a>

 
## <a name="permissioncheckresponse"></a>PermissionCheckResponse
 
PermissionCheckResponse 对象具有以下规范。
 
| 成员| 类型| 描述| 
| --- | --- | --- | 
| IsAllowed| 布尔值| 必需。 如果请求的用户允许执行与目标用户所请求的操作，此成员为<b>true</b> 。| 
| 结果| [PermissionCheckResult (JSON)](json-permissioncheckresult.md)的数组| 可选。 如果<b>IsAllowed</b>已 false 检查已被并被拒绝请求者相关的内容，指示的权限被拒绝的原因。| 
  
<a id="ID4E3B"></a>

 
## <a name="sample-json-syntax"></a>JSON 语法示例
 

```json
{
    "isAllowed": false,
    "reasons":
    [
        {"reason": "BlockedByRequestor"},
        {"reason": "MissingPrivilege", "restrictedSetting": "VideoCommunications"}
    ]
}
    
```

  
<a id="ID4EFC"></a>

 
## <a name="see-also"></a>另请参阅
 
<a id="ID4EHC"></a>

 
##### <a name="parent"></a>Parent 的子磁盘） 

[JavaScript 对象表示法 (JSON) 对象参考](atoc-xboxlivews-reference-json.md)

   