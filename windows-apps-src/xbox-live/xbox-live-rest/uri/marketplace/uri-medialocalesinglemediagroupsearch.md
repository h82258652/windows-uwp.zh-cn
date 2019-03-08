---
title: /media/{marketplaceId}/singleMediaGroupSearch
assetID: f5599db7-4050-640e-db96-2df01a007c07
permalink: en-us/docs/xboxlive/rest/uri-medialocalesinglemediagroupsearch.html
description: " /media/{marketplaceId}/singleMediaGroupSearch"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: b26b4c2dc51ef5591480372aa9908a49d2f8cbe2
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2019
ms.locfileid: "57661752"
---
# <a name="mediamarketplaceidsinglemediagroupsearch"></a>/media/{marketplaceId}/singleMediaGroupSearch
允许单个媒体组内的项的搜索。 请注意，可以使用而不使用继续标记不按顺序 skipItems 参数访问页面的搜索返回的数据。 此 API 接受查询精简将。
 
这些 Uri 的域是`eds.xboxlive.com`。
 
  * [URI 参数](#ID4EX)
 
<a id="ID4EX"></a>

 
## <a name="uri-parameters"></a>URI 参数
 
| 参数| 在任务栏的搜索框中键入| 描述| 
| --- | --- | --- | 
| marketplaceId| 字符串| 必需。 从获取值的字符串<b>Windows.Xbox.ApplicationModel.Store.Configuration.MarketplaceId</b>。| 
  
<a id="ID4EYB"></a>

 
## <a name="valid-methods"></a>有效的方法

[GET (media/{marketplaceId}/singleMediaGroupSearch)](uri-medialocalesinglemediagroupsearchget.md)

&nbsp;&nbsp;允许单个媒体组内的项的搜索。 
 
<a id="ID4ECC"></a>

 
## <a name="see-also"></a>另请参阅
 
<a id="ID4EEC"></a>

 
##### <a name="parent"></a>Parent 的子磁盘） 

[Marketplace Uri](atoc-reference-marketplace.md)

  
<a id="ID4EOC"></a>

 
##### <a name="further-information"></a>详细信息 

[EDS 常见标头](../../additional/edscommonheaders.md)

 [EDS 参数](../../additional/edsparameters.md)

 [EDS 查询精简将](../../additional/edsqueryrefiners.md)

 [其他参考](../../additional/atoc-xboxlivews-reference-additional.md)

   