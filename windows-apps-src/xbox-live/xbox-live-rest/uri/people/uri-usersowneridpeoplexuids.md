---
title: /users/ {ownerId} / 人员/xuid
assetID: db2faec7-9f6c-f240-586a-45d6ed596e88
permalink: en-us/docs/xboxlive/rest/uri-usersowneridpeoplexuids.html
author: KevinAsgari
description: " /users/ {ownerId} / 人员/xuid"
ms.author: kevinasg
ms.date: 20-12-2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 715659b8bb001697fc9386be6ec587b3682793c5
ms.sourcegitcommit: 72710baeee8c898b5ab77ceb66d884eaa9db4cb8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/12/2018
ms.locfileid: "3881324"
---
# <a name="usersowneridpeoplexuids"></a>/users/ {ownerId} / 人员/xuid
访问用户的 XUID 调用方的用户集合。 这些 Uri 的域是`social.xboxlive.com`。
 
  * [URI 参数](#ID4EV)
 
<a id="ID4EV"></a>

 
## <a name="uri-parameters"></a>URI 参数
 
| 参数| 类型| 说明| 
| --- | --- | --- | 
| ownerId| 字符串| 所访问的资源的用户的标识符。 必须匹配身份验证的用户。 可能的值为"我"、 xuid({xuid}) 或 gt({gamertag})。| 
  
<a id="ID4EOB"></a>

 
## <a name="valid-methods"></a>有效的方法

[POST](uri-usersowneridpeoplexuidspost.md)

&nbsp;&nbsp;获取用户的 XUID 从调用方的用户集合。
 
<a id="ID4EYB"></a>

 
## <a name="see-also"></a>另请参阅
 
<a id="ID4E1B"></a>

 
##### <a name="parent"></a>Parent 的子磁盘） 

[统一资源标识符 (URI) 引用](../atoc-xboxlivews-reference-uris.md)

   