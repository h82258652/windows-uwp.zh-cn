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
ms.sourcegitcommit: d2517e522cacc5240f7dffd5bc1eaa278e3f7768
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/02/2018
ms.locfileid: "8325216"
---
# <a name="permissioncheckresult-json"></a>PermissionCheckResult (JSON)
从单个权限设置针对单个目标用户的单个用户检查的结果。 
<a id="ID4EP"></a>

 
## <a name="permissioncheckresult"></a>PermissionCheckResult
 
PermissionCheckResult 对象具有以下规范。
 
| 成员| 类型| 描述| 
| --- | --- | --- | 
| 原因| 字符串| 可选。 <b>PermissionResultCode</b>值，该值指示的权限被拒绝为什么<b>IsAllowed</b>是否 false。| 
| restrictedSetting| 字符串| 可选。 如果<b>原因</b>成员<b>PermissionResultCode</b>的值指示请求者权限检查失败，这指示的权限失败。| 
  
<a id="ID4E6B"></a>

 
## <a name="sample-json-syntax"></a>JSON 语法示例
 

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

[JavaScript 对象表示法 (JSON) 对象参考](atoc-xboxlivews-reference-json.md)

   