---
title: 分页参数
assetID: f8d059fd-30e7-be60-0a46-c9a833c400c6
permalink: en-us/docs/xboxlive/rest/pagingparameters.html
description: " 分页参数"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: fe80e1666f9eab8caf7e0bbdb2b537bd7661c9a9
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2019
ms.locfileid: "57627292"
---
# <a name="paging-parameters"></a>分页参数
 
某些 Xbox Live 服务 Uri 返回 JavaScript 对象表示法 (JSON) 对象的集合。 通过指定分页参数作为查询字符串附加到 URI 的一部分，可以通过分页这些集合。 遵循分页参数的完整列表。 允许分页参数的所有 Uri 都链接到此页的底部。
 
<a id="ID4E2"></a>

 
## <a name="query-string-parameters"></a>查询字符串参数 
 
| 参数| 必需| 在任务栏的搜索框中键入| 描述| 
| --- | --- | --- | --- | 
| ContinuationToken| 否| 字符串| 返回从给定的继续标记处开始的项。 | 
| maxItems| 否| 32 位有符号的整数| 要从集合，可以结合返回的项数的最大数目<b>skipItems</b>并<b>continuationToken</b>要返回的项的范围。 该服务可能会提供默认值，如果<b>maxItems</b>不存在，并且可能返回少于<b>maxItems</b>，即使尚未返回结果的最后一页。 | 
| skipItems| 否| 32 位有符号的整数| 返回从给定数目的项后开始的项。 例如， <b>skipItems ="3"</b>将检索项的第四项开头检索。 | 
  
<a id="ID4EDD"></a>

 
## <a name="see-also"></a>另请参阅
 
<a id="ID4EFD"></a>

 
##### <a name="parent"></a>Parent 的子磁盘）  

[其他参考](atoc-xboxlivews-reference-additional.md)

  
<a id="ID4ERD"></a>

 
##### <a name="reference--get-usersxuidxuidachievementsuriachievementsuri-achievementsusersxuidachievementsgetv2md"></a>引用[GET (/users/xuid({xuid})/achievements)](../uri/achievements/uri-achievementsusersxuidachievementsgetv2.md)

   