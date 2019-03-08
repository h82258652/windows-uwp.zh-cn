---
title: /media/{marketplaceId}/metadata/mediaItemTypes/{mediaitemtype}/queryrefiners/{queryrefinername}
assetID: 1e2f5fd3-3d14-9a97-ffbf-ab2a18ff4f15
permalink: en-us/docs/xboxlive/rest/uri-medialocalemetadatamediaitemtypequeryrefinersqueryrefinername.html
description: " /media/{marketplaceId}/metadata/mediaItemTypes/{mediaitemtype}/queryrefiners/{queryrefinername}"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 356c7d6eb30eb14156e434f717530d6f4f48a990
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2019
ms.locfileid: "57611472"
---
# <a name="mediamarketplaceidmetadatamediaitemtypesmediaitemtypequeryrefinersqueryrefinername"></a>/media/{marketplaceId}/metadata/mediaItemTypes/{mediaitemtype}/queryrefiners/{queryrefinername}
访问给定的查询精选器名称和给定媒体项类型的可接受值。 这些 Uri 的域是`eds.xboxlive.com`。
 
  * [URI 参数](#ID4EV)
 
<a id="ID4EV"></a>

 
## <a name="uri-parameters"></a>URI 参数
 
| 参数| 在任务栏的搜索框中键入| 描述| 
| --- | --- | --- | 
| marketplaceId| 字符串| 必需。 从获取值的字符串<b>Windows.Xbox.ApplicationModel.Store.Configuration.MarketplaceId</b>。| 
| mediaitemtype| 字符串| 必需。 中的值之一[获取 (/media/ {marketplaceId} / 元数据/mediaGroups / {mediagroup} / mediaItemTypes)](uri-medialocalemetadatamediagroupsmediaitemtypesget.md)。| 
| queryrefinername| 字符串| 必需。 名称查询精选器所需的值，例如"Genre"或"十年"。 请参阅 QueryRefiners。| 
  
<a id="ID4EKC"></a>

 
## <a name="valid-methods"></a>有效的方法

[GET (/media/{marketplaceId}/metadata/mediaItemTypes/{mediaitemtype}/queryrefiners/{queryrefinername})](uri-medialocalemetadatamediaitemtypequeryrefinersqueryrefinernameget.md)

&nbsp;&nbsp;列出了可接受的值为给定的查询精选器名称和给定媒体项类型。
 
<a id="ID4EUC"></a>

 
## <a name="see-also"></a>另请参阅
 
<a id="ID4EWC"></a>

 
##### <a name="parent"></a>Parent 的子磁盘） 

[Marketplace Uri](atoc-reference-marketplace.md)

  
<a id="ID4EAD"></a>

 
##### <a name="further-information"></a>详细信息 

[EDS 常见标头](../../additional/edscommonheaders.md)

 [EDS 参数](../../additional/edsparameters.md)

 [EDS 查询精简将](../../additional/edsqueryrefiners.md)

 [其他参考](../../additional/atoc-xboxlivews-reference-additional.md)

   