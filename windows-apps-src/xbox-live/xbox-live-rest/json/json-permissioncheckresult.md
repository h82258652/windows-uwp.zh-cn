---
title: PermissionCheckResult (JSON)
assetID: 1cf147fa-4ff1-3299-0822-0fc1726d1600
permalink: en-us/docs/xboxlive/rest/json-permissioncheckresult.html
description: " PermissionCheckResult (JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: dc13826be1b6f81201d069f5ade7ea5ba6668cd0
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2019
ms.locfileid: "57598442"
---
# <a name="permissioncheckresult-json"></a>PermissionCheckResult (JSON)
从单个用户针对单个目标用户的单个权限设置的检查结果。 
<a id="ID4EP"></a>

 
## <a name="permissioncheckresult"></a>PermissionCheckResult
 
PermissionCheckResult 对象具有以下规范。
 
| 成员| 在任务栏的搜索框中键入| 描述| 
| --- | --- | --- | 
| reason| 字符串| 可选。 一个<b>PermissionResultCode</b>值，该值指示的权限被拒绝为什么如果<b>IsAllowed</b>是 false。| 
| restrictedSetting| 字符串| 可选。 如果<b>PermissionResultCode</b>中的值<b>原因</b>成员指示请求者的权限检查失败，这指示哪些权限失败。| 
  
<a id="ID4E6B"></a>

 
## <a name="sample-json-syntax"></a>示例 JSON 语法
 

```json
{
    "reason": "MissingPrivilege",
    "restrictedSetting": "VideoCommunications"
}
    
```

  
<a id="ID4EIC"></a>

 
## <a name="see-also"></a>另请参阅
 
<a id="ID4EKC"></a>

 
##### <a name="parent"></a>Parent 的子磁盘） 

[JavaScript 对象表示法 (JSON) 对象引用](atoc-xboxlivews-reference-json.md)

   