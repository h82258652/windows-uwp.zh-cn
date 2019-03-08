---
title: /users/{ownerId}/people/{targetid}
assetID: 9dd19e75-3b48-d7e0-fc65-6760c15ddf62
permalink: en-us/docs/xboxlive/rest/uri-usersowneridpeopletargetid.html
description: " /users/{ownerId}/people/{targetid}"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 6238996abdeaca9b7a9a7a20d3f1ae9702e95a73
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2019
ms.locfileid: "57661742"
---
# <a name="usersowneridpeopletargetid"></a>/users/{ownerId}/people/{targetid}
从调用方的用户集合访问目标 ID 的人员。 这些 Uri 的域是`social.xboxlive.com`。
 
  * [URI 参数](#ID4EV)
 
<a id="ID4EV"></a>

 
## <a name="uri-parameters"></a>URI 参数
 
| 参数| 在任务栏的搜索框中键入| 描述| 
| --- | --- | --- | 
| ownerId| 字符串| 其资源的访问的用户的标识符。 必须与匹配身份验证的用户。 可能的值为"me"、 xuid({xuid}) 或 gt({gamertag})。| 
| targetid| 字符串| 正在从所有者的用户列表中，Xbox 用户 ID (XUID) 或玩家代号检索其数据的用户的标识符。 示例值： xuid(2603643534573581)、 gt(SomeGamertag)。| 
  
<a id="ID4EQB"></a>

 
## <a name="valid-methods"></a>有效的方法

[GET](uri-usersowneridpeopletargetidget.md)

&nbsp;&nbsp;从调用方的用户集合中获取目标 ID 的人员。
 
<a id="ID4E1B"></a>

 
## <a name="see-also"></a>另请参阅
 
<a id="ID4E3B"></a>

 
##### <a name="parent"></a>Parent 的子磁盘） 

[通用资源标识符 (URI) 引用](../atoc-xboxlivews-reference-uris.md)

   