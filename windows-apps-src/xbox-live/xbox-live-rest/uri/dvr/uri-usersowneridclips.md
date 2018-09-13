---
title: /users/ {ownerId} / 剪辑
assetID: cc200b89-dc63-9ab5-b037-7a910f46dae9
permalink: en-us/docs/xboxlive/rest/uri-usersowneridclips.html
author: KevinAsgari
description: " /users/ {ownerId} / 剪辑"
ms.author: kevinasg
ms.date: 20-12-2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: b0819ab8f0014b945a2340ebf7252bbe9d8d8726
ms.sourcegitcommit: c8f6866100a4b38fdda8394ea185b02d7af66411
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/13/2018
ms.locfileid: "3955906"
---
# <a name="usersowneridclips"></a>/users/ {ownerId} / 剪辑
访问权限的用户的剪辑的列表。 这些 Uri 的域是`gameclipsmetadata.xboxlive.com`和`gameclipstransfer.xboxlive.com`，则根据问题的 URI 的函数。
 
  * [URI 参数](#ID4EX)
 
<a id="ID4EX"></a>

 
## <a name="uri-parameters"></a>URI 参数
 
| 参数| 类型| 说明| 
| --- | --- | --- | 
| ownerId| 字符串| 正在访问其资源的用户的用户身份。 支持的格式:"me"或"xuid(123456789)"。 最大长度： 16。| 
  
<a id="ID4EVB"></a>

 
## <a name="valid-methods"></a>有效的方法

[获取 (/users/ {ownerId} / 剪辑)](uri-usersowneridclipsget.md)

&nbsp;&nbsp;检索用户的剪辑的列表。
 
<a id="ID4E6B"></a>

 
## <a name="see-also"></a>另请参阅
 
<a id="ID4EBC"></a>

 
##### <a name="parent"></a>Parent 的子磁盘） 

[游戏 DVR Uri](atoc-reference-dvr.md)

   