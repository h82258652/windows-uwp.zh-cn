---
title: PermissionCheckBatchUserResponse (JSON)
assetID: c587dbc1-9436-4d55-afcb-deb47e3c2664
permalink: en-us/docs/xboxlive/rest/json-permissioncheckbatchuserresponse.html
description: " PermissionCheckBatchUserResponse (JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: c9e20cc195ad737a7e847a8ad41b76247220adfe
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2019
ms.locfileid: "57628352"
---
# <a name="permissioncheckbatchuserresponse-json"></a>PermissionCheckBatchUserResponse (JSON)
批处理权限的原因检查单个目标用户的权限值的列表。 
<a id="ID4EN"></a>

 
## <a name="permissioncheckbatchuserresponse"></a>PermissionCheckBatchUserResponse
 
PermissionCheckBatchUserResponse 对象具有以下规范。
 
| 成员| 在任务栏的搜索框中键入| 描述| 
| --- | --- | --- | 
| 用户| 字符串| 必需。 此成员是<b>，则返回 true</b>如果请求用户允许执行与目标用户请求的操作。| 
| 权限| 数组[PermissionCheckResponse (JSON)](json-permissioncheckresponse.md)| 必需。 一个[PermissionCheckResponse (JSON)](json-permissioncheckresponse.md)中原始请求，如请求中所示的相同顺序请求的每个权限。| 
  
<a id="ID4E4B"></a>

 
## <a name="sample-json-syntax"></a>示例 JSON 语法
 

```json
{
    "User": {"Xuid": "12345"},
    "Permissions":
    [
        {
            "isAllowed": true
        },
        {
            "isAllowed": false
        },
        {
            "isAllowed": false,
            "reasons":
            [
                {"reason": "BlockedByRequestor"},
                {"reason": "MissingPrivilege", "restrictedSetting": "VideoCommunications"}
            ]
        }
    ]
}
    
```

  
<a id="ID4EGC"></a>

 
## <a name="see-also"></a>另请参阅
 
<a id="ID4EIC"></a>

 
##### <a name="parent"></a>Parent 的子磁盘） 

[JavaScript 对象表示法 (JSON) 对象引用](atoc-xboxlivews-reference-json.md)

   