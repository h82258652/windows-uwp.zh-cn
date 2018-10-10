---
title: /users/{requestorId}/permission/validate
assetID: 400a9721-bf43-76df-4cd1-9f2ae6ca5035
permalink: en-us/docs/xboxlive/rest/uri-privacyusersrequestoridpermissionvalidate.html
author: KevinAsgari
description: " /users/{requestorId}/permission/validate"
ms.author: kevinasg
ms.date: 20-12-2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: faa0325a8540e1e3df9674a4acab2ab33e93dceb
ms.sourcegitcommit: 49aab071aa2bd88f1c165438ee7e5c854b3e4f61
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/09/2018
ms.locfileid: "4470370"
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

&nbsp;&nbsp;获取有关是否允许用户执行与目标用户指定的操作或否答案。

[POST (/users/{requestorId}/permission/validate)](uri-privacyusersrequestoridpermissionvalidatepost.md)

&nbsp;&nbsp;获取有关是否允许用户执行一组的目标用户的指定的操作或否答案的一组。
 
<a id="ID4EAC"></a>

 
## <a name="see-also"></a>另请参阅
 
<a id="ID4ECC"></a>

   [隐私 URI](atoc-reference-privacyv2.md)

 [PermissionId 枚举](../../enums/privacy-enum-permissionid.md)

   