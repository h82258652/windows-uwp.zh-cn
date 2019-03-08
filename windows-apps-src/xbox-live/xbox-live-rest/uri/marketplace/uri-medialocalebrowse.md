---
title: /media/{marketplaceId}/browse
assetID: 4fedc780-b3c2-c83b-e7af-9e18666a4771
permalink: en-us/docs/xboxlive/rest/uri-medialocalebrowse.html
description: " /media/{marketplaceId}/browse"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: f692fb66580e20ffeefb3595b8cf9d795f504311
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2019
ms.locfileid: "57589682"
---
# <a name="mediamarketplaceidbrowse"></a>/media/{marketplaceId}/browse
允许浏览单个媒体组中的项。 浏览 API 允许客户端浏览的单个媒体组中从项。 可以使用而不使用继续标记不按顺序 skipItems 参数访问的数据页。
 
此 API 还允许浏览内给定项的子级。 例如，通过传入一个 ID 和 MediaItemType 参数的 Xbox 360 游戏，这允许浏览和 diltering 对该项目，如虚拟形象项或游戏 DLC 的子对象。
 
此 API 接受查询精简将。
 
检索子级的一些方案包括：
 
   * 唱片集曲目翻录的曲目
   * 序列为季节
   * 季节到剧集
   * 跟踪对音乐视频
   * 到唱片集艺术家
   * 游戏到游戏外接程序 （DLC、 头像、 主题等）
  
这些 Uri 的域是`eds.xboxlive.com`。
 
  * [URI 参数](#ID4EMB)
 
<a id="ID4EMB"></a>

 
## <a name="uri-parameters"></a>URI 参数
 
| 参数| 在任务栏的搜索框中键入| 描述| 
| --- | --- | --- | 
| marketplaceId| 字符串| 必需。 从获取值的字符串<b>Windows.Xbox.ApplicationModel.Store.Configuration.MarketplaceId</b>。| 
  
<a id="ID4ENC"></a>

 
## <a name="valid-methods"></a>有效的方法

[GET (media/{marketplaceId}/browse)](uri-medialocalebrowseget.md)

&nbsp;&nbsp;允许浏览单个媒体组中的项。 
 
<a id="ID4EXC"></a>

 
## <a name="see-also"></a>另请参阅
 
<a id="ID4EZC"></a>

 
##### <a name="parent"></a>Parent 的子磁盘） 

[Marketplace Uri](atoc-reference-marketplace.md)

  
<a id="ID4EDD"></a>

 
##### <a name="further-information"></a>详细信息 

[EDS 常见标头](../../additional/edscommonheaders.md)

 [EDS 参数](../../additional/edsparameters.md)

 [EDS 查询精简将](../../additional/edsqueryrefiners.md)

 [其他参考](../../additional/atoc-xboxlivews-reference-additional.md)

   