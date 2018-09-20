---
title: /media/{marketplaceId}/metadata/mediaItemTypes/{mediaitemtype}/queryrefiners
assetID: 5a519314-1df1-cbdc-cb04-3a8b663003de
permalink: en-us/docs/xboxlive/rest/uri-medialocalemetadatamediaitemtypequeryrefiners.html
author: KevinAsgari
description: " /media/{marketplaceId}/metadata/mediaItemTypes/{mediaitemtype}/queryrefiners"
ms.author: kevinasg
ms.date: 20-12-2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 9273282ab20f06998a2d510f6c3d2b7e61ecf4a9
ms.sourcegitcommit: 4f6dc806229a8226894c55ceb6d6eab391ec8ab6
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/20/2018
ms.locfileid: "4086413"
---
# <a name="mediamarketplaceidmetadatamediaitemtypesmediaitemtypequeryrefiners"></a>/media/{marketplaceId}/metadata/mediaItemTypes/{mediaitemtype}/queryrefiners
对于给定的媒体项的类型，请访问查询精简将。 这些 Uri 的域是`eds.xboxlive.com`。
 
  * [URI 参数](#ID4EV)
 
<a id="ID4EV"></a>

 
## <a name="uri-parameters"></a>URI 参数
 
| 参数| 类型| 说明| 
| --- | --- | --- | 
| marketplaceId| 字符串| 必需。 字符串从<b>Windows.Xbox.ApplicationModel.Store.Configuration.MarketplaceId</b>获得的值。| 
| mediaitemtype| 字符串| 必需。 从值之一[获取 (/media/ {marketplaceId} / 元数据/mediaGroups / {mediagroup} / mediaItemTypes)](uri-medialocalemetadatamediagroupsmediaitemtypesget.md)。| 
  
<a id="ID4EBC"></a>

 
## <a name="valid-methods"></a>有效的方法

[GET (/media/{marketplaceId}/metadata/mediaItemTypes/{mediaitemtype}/queryrefiners)](uri-medialocalemetadatamediaitemtypequeryrefinersget.md)

&nbsp;&nbsp;列出了查询精简将给定的媒体项类型。
 
<a id="ID4ELC"></a>

 
## <a name="see-also"></a>另请参阅
 
<a id="ID4ENC"></a>

 
##### <a name="parent"></a>Parent 的子磁盘） 

[市场 URI](atoc-reference-marketplace.md)

  
<a id="ID4EXC"></a>

 
##### <a name="further-information"></a>详细信息 

[EDS 通用标头](../../additional/edscommonheaders.md)

 [EDS 参数](../../additional/edsparameters.md)

 [EDS 查询优化器](../../additional/edsqueryrefiners.md)

 [其他参考](../../additional/atoc-xboxlivews-reference-additional.md)

   