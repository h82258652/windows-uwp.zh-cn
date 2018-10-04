---
title: /users/{ownerId}/people/{targetid}
assetID: 9dd19e75-3b48-d7e0-fc65-6760c15ddf62
permalink: en-us/docs/xboxlive/rest/uri-usersowneridpeopletargetid.html
author: KevinAsgari
description: " /users/{ownerId}/people/{targetid}"
ms.author: kevinasg
ms.date: 20-12-2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 7693d9e60a9fdf58eba8aecdd8618c0a78ecef44
ms.sourcegitcommit: e6daa7ff878f2f0c7015aca9787e7f2730abcfbf
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/04/2018
ms.locfileid: "4308456"
---
# <a name="usersowneridpeopletargetid"></a>/users/{ownerId}/people/{targetid}
从调用方的用户集合访问目标 ID 由一个人。 这些 Uri 的域是`social.xboxlive.com`。
 
  * [URI 参数](#ID4EV)
 
<a id="ID4EV"></a>

 
## <a name="uri-parameters"></a>URI 参数
 
| 参数| 类型| 描述| 
| --- | --- | --- | 
| ownerId| 字符串| 正在访问其资源的用户的标识符。 必须匹配身份验证的用户。 可能的值为"我"、 xuid({xuid}) 或 gt({gamertag})。| 
| targetid| 字符串| 正在从所有者的人脉列表中，Xbox 用户 ID (XUID) 或玩家代号检索其数据的用户的标识符。 示例值： xuid(2603643534573581)、 gt(SomeGamertag)。| 
  
<a id="ID4EQB"></a>

 
## <a name="valid-methods"></a>有效的方法

[GET](uri-usersowneridpeopletargetidget.md)

&nbsp;&nbsp;按目标 ID 某人从集合中获取调用方的人。
 
<a id="ID4E1B"></a>

 
## <a name="see-also"></a>另请参阅
 
<a id="ID4E3B"></a>

 
##### <a name="parent"></a>Parent 的子磁盘） 

[统一资源标识符 (URI) 参考](../atoc-xboxlivews-reference-uris.md)

   