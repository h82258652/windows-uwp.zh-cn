---
title: /users/xuid({xuid})/outbox
assetID: 0b66b885-15ff-be55-f8be-e6e9d85d087e
permalink: en-us/docs/xboxlive/rest/uri-usersxuidoutbox.html
description: " /users/xuid({xuid})/outbox"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 88f3f3753aeac99db0a8a53e0a2ddde21d034ac5
ms.sourcegitcommit: d2517e522cacc5240f7dffd5bc1eaa278e3f7768
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/02/2018
ms.locfileid: "8324074"
---
# <a name="usersxuidxuidoutbox"></a>/users/xuid({xuid})/outbox
提供的消息仅发送给用户的访问权限的 Xbox LIVE 服务的发件箱。 这些 Uri 的域是`msg.xboxlive.com`。
 
  * [URI 参数](#ID4EV)
 
<a id="ID4EV"></a>

 
## <a name="uri-parameters"></a>URI 参数 
 
| 参数| 类型| 描述| 
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

   