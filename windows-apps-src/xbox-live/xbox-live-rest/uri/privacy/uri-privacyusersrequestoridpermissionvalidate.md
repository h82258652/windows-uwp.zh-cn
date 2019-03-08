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
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2019
ms.locfileid: "57599362"
---
# <a name="usersrequestoridpermissionvalidate"></a>/users/{requestorId}/permission/validate
 
  * [URI 参数](#ID4EQ)
 
<a id="ID4EQ"></a>

 
## <a name="uri-parameters"></a>URI 参数
 
| 参数| 在任务栏的搜索框中键入| 描述| 
| --- | --- | --- | 
| requestorId| 字符串| 必需。 在执行操作的用户的标识符。 可能的值为<code>xuid({xuid})</code>和<code>me</code>。 这必须是已登录的用户。 示例值： <code>xuid(0987654321)</code>。| 
  
<a id="ID4ETB"></a>

 
## <a name="valid-methods"></a>有效的方法

[GET (/users/{requestorId}/permission/validate)](uri-privacyusersrequestoridpermissionvalidateget.md)

&nbsp;&nbsp;获取有关是否允许用户执行与目标用户指定的操作是或否答案。

[POST (/users/{requestorId}/permission/validate)](uri-privacyusersrequestoridpermissionvalidatepost.md)

&nbsp;&nbsp;获取有关是否允许用户执行指定的操作的目标用户集使用的是或否答案的一组。
 
<a id="ID4EAC"></a>

 
## <a name="see-also"></a>另请参阅
 
<a id="ID4ECC"></a>

   [隐私 Uri](atoc-reference-privacyv2.md)

 [PermissionId 枚举](../../enums/privacy-enum-permissionid.md)

   