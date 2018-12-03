---
title: /users/{requestorId}/permission/validate
assetID: 400a9721-bf43-76df-4cd1-9f2ae6ca5035
permalink: en-us/docs/xboxlive/rest/uri-privacyusersrequestoridpermissionvalidate.html
description: " /users/{requestorId}/permission/validate"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 4a062fd417bae37fd66c944e0e534ef7a50de5fa
ms.sourcegitcommit: d2517e522cacc5240f7dffd5bc1eaa278e3f7768
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/02/2018
ms.locfileid: "8325724"
---
# <a name="usersrequestoridpermissionvalidate"></a>/users/{requestorId}/permission/validate
 
  * [URI 参数](#ID4EQ)
 
<a id="ID4EQ"></a>

 
## <a name="uri-parameters"></a>URI 参数
 
| 参数| 类型| 描述| 
| --- | --- | --- | 
| requestorId| 字符串| 必需。 执行该操作的用户的标识符。 可能的值为<code>xuid({xuid})</code>和<code>me</code>。 这必须是已登录的用户。 示例值： <code>xuid(0987654321)</code>。| 
  
<a id="ID4ETB"></a>

 
## <a name="valid-methods"></a>有效的方法

[GET (/users/{requestorId}/permission/validate)](uri-privacyusersrequestoridpermissionvalidateget.md)

&nbsp;&nbsp;获取有关是否允许用户执行与目标用户指定的操作 yes 或 no 答案。

[POST (/users/{requestorId}/permission/validate)](uri-privacyusersrequestoridpermissionvalidatepost.md)

&nbsp;&nbsp;获取一套 yes 或 no 常见问题解答是否允许用户执行一组的目标用户指定的操作。
 
<a id="ID4EAC"></a>

 
## <a name="see-also"></a>另请参阅
 
<a id="ID4ECC"></a>

   [隐私 URI](atoc-reference-privacyv2.md)

 [PermissionId 枚举](../../enums/privacy-enum-permissionid.md)

   