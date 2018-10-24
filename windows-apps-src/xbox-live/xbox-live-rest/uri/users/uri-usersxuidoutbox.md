---
title: /users/xuid({xuid})/outbox
assetID: 0b66b885-15ff-be55-f8be-e6e9d85d087e
permalink: en-us/docs/xboxlive/rest/uri-usersxuidoutbox.html
author: KevinAsgari
description: " /users/xuid({xuid})/outbox"
ms.author: kevinasg
ms.date: 20-12-2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 584acb4b74fa74dd91e9f8044b59647d8e5d8787
ms.sourcegitcommit: 4b97117d3aff38db89d560502a3c372f12bb6ed5
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/24/2018
ms.locfileid: "5430580"
---
# <a name="usersxuidxuidoutbox"></a>/users/xuid({xuid})/outbox
提供的邮件只能发送的用户对访问 Xbox LIVE 服务的发件箱。 这些 Uri 的域是`msg.xboxlive.com`。
 
  * [URI 参数](#ID4EV)
 
<a id="ID4EV"></a>

 
## <a name="uri-parameters"></a>URI 参数 
 
| 参数| 类型| 说明| 
| --- | --- | --- | 
| xuid | 64 位无符号的整数 | Xbox 用户 ID (XUID) 发出请求的玩家。 | 
  
<a id="ID4EXB"></a>

 
## <a name="valid-methods"></a>有效的方法 

[POST (/users/xuid({xuid})/outbox)](uri-usersxuidoutboxpost.md)

&nbsp;&nbsp;指定的消息发送到收件人的列表。 
 
<a id="ID4EFC"></a>

 
## <a name="see-also"></a>另请参阅
 
<a id="ID4EHC"></a>

 
##### <a name="parent"></a>Parent 的子磁盘）  

[用户 URI](atoc-reference-users.md)

   