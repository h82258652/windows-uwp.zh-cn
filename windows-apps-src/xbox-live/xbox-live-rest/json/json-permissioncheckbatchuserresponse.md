---
title: PermissionCheckBatchUserResponse (JSON)
assetID: c587dbc1-9436-4d55-afcb-deb47e3c2664
permalink: en-us/docs/xboxlive/rest/json-permissioncheckbatchuserresponse.html
author: KevinAsgari
description: " PermissionCheckBatchUserResponse (JSON)"
ms.author: kevinasg
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: da918cbbf76a757e16aea10cf4fbc6b2646de9fc
ms.sourcegitcommit: e38b334edb82bf2b1474ba686990f4299b8f59c7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/14/2018
ms.locfileid: "6834269"
---
# <a name="permissioncheckbatchuserresponse-json"></a>PermissionCheckBatchUserResponse (JSON)
单个目标用户的权限值的列表中检查批处理权限的原因。 
<a id="ID4EN"></a>

 
## <a name="permissioncheckbatchuserresponse"></a>PermissionCheckBatchUserResponse
 
PermissionCheckBatchUserResponse 对象具有以下规范。
 
| 成员| 类型| 说明| 
| --- | --- | --- | 
| 用户| 字符串| 必需。 如果请求用户允许执行与目标用户请求的操作，此成员为<b>true</b> 。| 
| 权限| [PermissionCheckResponse (JSON)](json-permissioncheckresponse.md)的数组| 必需。 原始请求中，如请求中所示的相同顺序要求提供每项权限[PermissionCheckResponse (JSON)](json-permissioncheckresponse.md) 。| 
  
<a id="ID4E4B"></a>

 
## <a name="sample-json-syntax"></a>JSON 语法示例
 

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

[JavaScript 对象表示法 (JSON) 对象参考](atoc-xboxlivews-reference-json.md)

   