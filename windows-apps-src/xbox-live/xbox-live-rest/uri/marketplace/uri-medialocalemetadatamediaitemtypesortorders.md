---
title: /media/{marketplaceId}/metadata/mediaItemTypes/{mediaitemtype}/sortorders
assetID: 221c440d-c448-11e9-f455-6d0f95fc16ef
permalink: en-us/docs/xboxlive/rest/uri-medialocalemetadatamediaitemtypesortorders.html
description: " /media/{marketplaceId}/metadata/mediaItemTypes/{mediaitemtype}/sortorders"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: e807f1a29712f55fc2e404e3905c7c69eb50d166
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2019
ms.locfileid: "57640922"
---
# <a name="mediamarketplaceidmetadatamediaitemtypesmediaitemtypesortorders"></a>/media/{marketplaceId}/metadata/mediaItemTypes/{mediaitemtype}/sortorders
可用的访问对给定的 mediaitem 类型和 EDS 的给定的版本的订单进行排序。 这些 Uri 的域是`eds.xboxlive.com`。
 
  * [URI 参数](#ID4EV)
 
<a id="ID4EV"></a>

 
## <a name="uri-parameters"></a>URI 参数
 
| 参数| 在任务栏的搜索框中键入| 描述| 
| --- | --- | --- | 
| marketplaceId| 字符串| 必需。 从获取值的字符串<b>Windows.Xbox.ApplicationModel.Store.Configuration.MarketplaceId</b>。| 
| mediaitemtype| 字符串| 必需。 中的值之一[获取 (/media/ {marketplaceId} / 元数据/mediaGroups / {mediagroup} / mediaItemTypes)](uri-medialocalemetadatamediagroupsmediaitemtypesget.md)。| 
  
<a id="ID4EBC"></a>

 
## <a name="valid-methods"></a>有效的方法

[GET (/media/{marketplaceId}/metadata/mediaItemTypes/{mediaitemtype}/sortorders)](uri-medialocalemetadatamediaitemtypesortordersget.md)

&nbsp;&nbsp;列出给定的 mediaitem 类型和 EDS 的给定的版本的可用排序顺序。
 
<a id="ID4ELC"></a>

 
## <a name="see-also"></a>另请参阅
 
<a id="ID4ENC"></a>

 
##### <a name="parent"></a>Parent 的子磁盘） 

[Marketplace Uri](atoc-reference-marketplace.md)

  
<a id="ID4EXC"></a>

 
##### <a name="further-information"></a>详细信息 

[EDS 常见标头](../../additional/edscommonheaders.md)

 [EDS 参数](../../additional/edsparameters.md)

 [EDS 查询精简将](../../additional/edsqueryrefiners.md)

 [其他参考](../../additional/atoc-xboxlivews-reference-additional.md)

   