---
title: /media/ {marketplaceId} / 浏览
assetID: 4fedc780-b3c2-c83b-e7af-9e18666a4771
permalink: en-us/docs/xboxlive/rest/uri-medialocalebrowse.html
author: KevinAsgari
description: " /media/ {marketplaceId} / 浏览"
ms.author: kevinasg
ms.date: 20-12-2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 776db1cf795ae964621d751d6b4b72d22ba82c2d
ms.sourcegitcommit: 72710baeee8c898b5ab77ceb66d884eaa9db4cb8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/12/2018
ms.locfileid: "3881234"
---
# <a name="mediamarketplaceidbrowse"></a>/media/ {marketplaceId} / 浏览
允许浏览的单个媒体组中的项。 浏览 API 允许客户端浏览的单个媒体组内从项。 非按顺序而不使用延续令牌使用 skipItems 参数可以访问的数据的页面。
 
此 API 还允许在给定的项的子项的浏览。 例如，通过为 Xbox 360 游戏 ID 和 MediaItemType 参数中传递，这可以浏览和 diltering 该项目，如虚拟形象项目或 DLC 游戏的子元素上。
 
此 API 将接受查询精简将。
 
某些情况下，用于检索子元素包括：
 
   * 到轨唱片集
   * 季节系列
   * 季节到剧集：
   * 跟踪对音乐视频
   * 为相册艺术家
   * 为游戏加载项 （DLC、 头像、 主题等） 游戏
  
这些 Uri 的域是`eds.xboxlive.com`。
 
  * [URI 参数](#ID4EMB)
 
<a id="ID4EMB"></a>

 
## <a name="uri-parameters"></a>URI 参数
 
| 参数| 类型| 说明| 
| --- | --- | --- | 
| marketplaceId| 字符串| 必需。 字符串从<b>Windows.Xbox.ApplicationModel.Store.Configuration.MarketplaceId</b>获得的值。| 
  
<a id="ID4ENC"></a>

 
## <a name="valid-methods"></a>有效的方法

[获取 (媒体 / {marketplaceId} / 浏览)](uri-medialocalebrowseget.md)

&nbsp;&nbsp;允许浏览的单个媒体组中的项。 
 
<a id="ID4EXC"></a>

 
## <a name="see-also"></a>另请参阅
 
<a id="ID4EZC"></a>

 
##### <a name="parent"></a>Parent 的子磁盘） 

[市场 Uri](atoc-reference-marketplace.md)

  
<a id="ID4EDD"></a>

 
##### <a name="further-information"></a>详细信息 

[EDS 公共标头](../../additional/edscommonheaders.md)

 [EDS 参数](../../additional/edsparameters.md)

 [EDS 查询精简将](../../additional/edsqueryrefiners.md)

 [其他参考](../../additional/atoc-xboxlivews-reference-additional.md)

   