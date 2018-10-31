---
title: /media/{marketplaceId}/browse
assetID: 4fedc780-b3c2-c83b-e7af-9e18666a4771
permalink: en-us/docs/xboxlive/rest/uri-medialocalebrowse.html
author: KevinAsgari
description: " /media/{marketplaceId}/browse"
ms.author: kevinasg
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: f8c877988111eb1ee19ee91bf01d4a6077ce4600
ms.sourcegitcommit: ca96031debe1e76d4501621a7680079244ef1c60
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/31/2018
ms.locfileid: "5813625"
---
# <a name="mediamarketplaceidbrowse"></a>/media/{marketplaceId}/browse
允许浏览单个媒体组中的项。 浏览 API 允许客户端浏览的单个媒体组内从项。 非连续而不使用延续令牌使用 skipItems 参数可以访问的数据的页面。
 
此 API 还允许在给定的项的子项的浏览。 例如，通过为 Xbox 360 游戏 ID 和 MediaItemType 参数中传递，这可以浏览和 diltering 该项目，如虚拟形象项目或 DLC 游戏的子元素上。
 
此 API 将接受查询优化器。
 
用于检索子某些方案包括：
 
   * 为轨唱片集
   * 季节系列
   * 季节到剧集：
   * 跟踪音乐视频
   * 照相艺术家
   * 游戏的游戏加载项 （DLC 头像、 主题、 等。）
  
这些 Uri 的域是`eds.xboxlive.com`。
 
  * [URI 参数](#ID4EMB)
 
<a id="ID4EMB"></a>

 
## <a name="uri-parameters"></a>URI 参数
 
| 参数| 类型| 说明| 
| --- | --- | --- | 
| marketplaceId| 字符串| 必需。 字符串从<b>Windows.Xbox.ApplicationModel.Store.Configuration.MarketplaceId</b>获得的值。| 
  
<a id="ID4ENC"></a>

 
## <a name="valid-methods"></a>有效的方法

[GET (media/{marketplaceId}/browse)](uri-medialocalebrowseget.md)

&nbsp;&nbsp;允许浏览单个媒体组中的项。 
 
<a id="ID4EXC"></a>

 
## <a name="see-also"></a>另请参阅
 
<a id="ID4EZC"></a>

 
##### <a name="parent"></a>Parent 的子磁盘） 

[市场 URI](atoc-reference-marketplace.md)

  
<a id="ID4EDD"></a>

 
##### <a name="further-information"></a>详细信息 

[EDS 通用标头](../../additional/edscommonheaders.md)

 [EDS 参数](../../additional/edsparameters.md)

 [EDS 查询优化器](../../additional/edsqueryrefiners.md)

 [其他参考](../../additional/atoc-xboxlivews-reference-additional.md)

   