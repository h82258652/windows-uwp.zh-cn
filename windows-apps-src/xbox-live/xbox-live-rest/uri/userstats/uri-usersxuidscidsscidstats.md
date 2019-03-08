---
title: /users/xuid({xuid})/scids/{scid}/stats
assetID: 3cf9ffd4-9a8b-2658-402b-2e933f7f6f1b
permalink: en-us/docs/xboxlive/rest/uri-usersxuidscidsscidstats.html
description: " /users/xuid({xuid})/scids/{scid}/stats"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 53a6c7bb0e7390b024b01e221d8061316a80509e
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2019
ms.locfileid: "57650412"
---
# <a name="usersxuidxuidscidsscidstats"></a>/users/xuid({xuid})/scids/{scid}/stats
访问作用域由代表指定用户的用户统计信息名称的逗号分隔列表的服务配置。 这些 Uri 的域是`userstats.xboxlive.com`。
 
  * [URI 参数](#ID4EV)
 
<a id="ID4EV"></a>

 
## <a name="uri-parameters"></a>URI 参数
 
| 参数| 在任务栏的搜索框中键入| 描述| 
| --- | --- | --- | 
| xuid| GUID| Xbox 用户 ID (XUID) 的用户的名义访问服务配置。| 
| scid| GUID| 服务配置，其中包含要访问的资源的标识符。| 
  
<a id="ID4E4B"></a>

 
## <a name="valid-methods"></a>有效的方法

[GET](uri-usersxuidscidsscidstatsget.md)

&nbsp;&nbsp;获取代表指定用户的用户统计信息名称的逗号分隔列表的范围扩展的服务配置。

[获取与值的元数据](uri-usersxuidscidsscidstatsgetvaluemetadata.md)

&nbsp;&nbsp;获取指定的统计信息，包括与统计信息值，指定的服务配置中的用户相关联的元数据的列表。
 
<a id="ID4EKC"></a>

 
## <a name="see-also"></a>另请参阅
 
<a id="ID4EMC"></a>

 
##### <a name="parent"></a>Parent 的子磁盘） 

[用户统计信息的 Uri](atoc-reference-userstats.md)

   