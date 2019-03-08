---
title: PermissionCheckBatchResponse (JSON)
assetID: f157ed8d-7483-1b34-bc3d-e3fcf6a7d055
permalink: en-us/docs/xboxlive/rest/json-permissioncheckbatchresponse.html
description: " PermissionCheckBatchResponse (JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 6ac79d3361e83993c372b1d651e97e900788d39f
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2019
ms.locfileid: "57613342"
---
# <a name="permissioncheckbatchresponse-json"></a>PermissionCheckBatchResponse (JSON)
结果的批处理权限检查的多个用户的权限值的列表。 
<a id="ID4EN"></a>

 
## <a name="permissioncheckbatchresponse"></a>PermissionCheckBatchResponse
 
PermissionCheckBatchResponse 对象具有以下规范。
 
| 成员| 在任务栏的搜索框中键入| 描述| 
| --- | --- | --- | 
| Responses| 数组[PermissionCheckBatchUserResponse (JSON)](json-permissioncheckbatchuserresponse.md)| 必需。 一个[PermissionCheckBatchUserResponse (JSON)](json-permissioncheckbatchuserresponse.md)中原始请求，如该请求中所示的相同顺序请求的每个权限的对象。| 
  
<a id="ID4EQB"></a>

 
## <a name="sample-json-syntax"></a>示例 JSON 语法
 

```json
{
    "responses":
    [
        {
            "user": {"xuid":"12345"},
            "permissions":
            [
                {
                    "isAllowed":true
                },
                {
                    "isAllowed":true
                }
            ]
        },
        {
            "user": {"xuid":"54321"},
            "permissions":
            [
                {
                    "isAllowed":false,
                    "reasons":
                    [
                        {"reason":"NotAllowed"}
                    ]
                },
                {
                    "isAllowed":false,
                    "reasons":
                    [
                        {"reason":"PrivilegeRestrictsTarget", "restrictedSetting":"AllowProfileViewing"}
                    ]
                }
            ]
        }
    ]
}
    
```

  
<a id="ID4EZB"></a>

 
## <a name="see-also"></a>另请参阅
 
<a id="ID4E2B"></a>

 
##### <a name="parent"></a>Parent 的子磁盘） 

[JavaScript 对象表示法 (JSON) 对象引用](atoc-xboxlivews-reference-json.md)

   