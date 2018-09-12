---
title: /scids/ {scid} /leaderboards/ {leaderboardname}
assetID: 16345a17-6025-5453-5694-eaf97f0e83e9
permalink: en-us/docs/xboxlive/rest/uri-scidsscidleaderboardsleaderboardname.html
author: KevinAsgari
description: " /scids/ {scid} /leaderboards/ {leaderboardname}"
ms.author: kevinasg
ms.date: 20-12-2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 797a557b4bb7d443ecfdce1f136f5db2079b1990
ms.sourcegitcommit: 72710baeee8c898b5ab77ceb66d884eaa9db4cb8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/12/2018
ms.locfileid: "3880593"
---
# <a name="scidsscidleaderboardsleaderboardname"></a>/scids/ {scid} /leaderboards/ {leaderboardname}
访问预定义的全球排行榜。 这些 Uri 的域是`leaderboards.xboxlive.com`。
 
  * [URI 参数](#ID4EV)
 
<a id="ID4EV"></a>

 
## <a name="uri-parameters"></a>URI 参数
 
| 参数| 类型| 说明| 
| --- | --- | --- | 
| scid| GUID| 服务配置，其中包含要访问的资源的标识符。| 
| leaderboardname| 字符串| 正在访问的预定义的排行榜资源的唯一标识符。| 
  
<a id="ID4E3B"></a>

 
## <a name="valid-methods"></a>有效的方法

[GET](uri-scidsscidleaderboardsleaderboardnameget.md)

&nbsp;&nbsp;&nbsp;&nbsp;获取预定义的全球排行榜。


[获取与值元数据](uri-scidsscidleaderboardsleaderboardnamegetvaluemetadata.md)

&nbsp;&nbsp;&nbsp;&nbsp;获取预定义的全局排行榜，以及任何与排行榜值关联的元数据。

 
<a id="ID4EJC"></a>

 
## <a name="see-also"></a>另请参阅
 
<a id="ID4ELC"></a>

 
##### <a name="parent"></a>Parent 的子磁盘） 

[排行榜 Uri](atoc-reference-leaderboard.md)

   