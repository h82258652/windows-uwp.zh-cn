---
title: EDS 查询优化器
assetID: ab5c066a-a48b-3042-351d-d0a15f663276
permalink: en-us/docs/xboxlive/rest/edsqueryrefiners.html
description: " EDS 查询优化器"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: c00ff971e05003ec88c47d3803e565f6e9406c47
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2019
ms.locfileid: "57611462"
---
# <a name="eds-query-refiners"></a>EDS 查询优化器
 
<a id="ID4EO"></a>

  
 
可以使用以下参数来优化对更有针对性的一组项的娱乐发现服务 (EDS) 查询。 所有这些参数必需的任何 API 中，但他们正在接受接受查询精简将任何 API 中。
 
参数名称可以作为传入值到任何"queryRefiners"参数。 此然后会返回应用请求重复使用查询精选器时会返回的项的数目，按每个值的查询精选细分。
 
下面是这实际上可能起的作用：
 
   * 进行浏览 api 调用，包括参数"queryRefiners = 流派"。
   * API 会返回八个游戏。 除了项之外，每个流派的项的列表将返回，以及多少个项属于该类型。 对于游戏，这可能是"射击：3，测验题：5".
   * 进行第二个查询。 它等同于第一个，只不过"流派 = Shooter"添加。
   * 响应现在包含只有三个游戏，所有这些属于"射击"类别。
  
| 参数| 数据类型| 描述| 
| --- | --- | --- | 
| <b>decade</b>| 字符串| 在其中的所有项必须已都发布在十年。| 
| <b>genre</b>| 字符串数组| 所有项都必须都具有的类型的列表。| 
| <b>labelOwner</b>| 字符串| 使用艺术家、 唱片集或跟踪相关联的音乐标签。| 
| <b>network</b>| 字符串数组| 创建项的网络。| 
| <b>studio</b>| 字符串数组| 创建项 studio。| 
| <b>xboxAppCategories</b>| 字符串数组| 必须具有所有 Xbox 应用的类别的列表。| 
| <b>xboxAvatarClothes</b>| 字符串数组| Xbox 虚拟形象的所有项必须都具有服装类型的列表。| 
| <b>xboxAvatarStores</b>| 字符串数组| 存储到的所有 Xbox 头像项必须属于的列表。| 
| <b>xboxGamePublisherBits</b>| 字符串数组| 必须在所有 GameType 项或 AppType 项设置的游戏发行者位的列表。| 
| <b>xboxIsBrowsable</b>| 布尔值| 如果<b>，则返回 true</b>，将返回完整的游戏不是直接操作除了可操作的内容。 默认情况下<b>false</b>。| 
| <b>xboxHasChildMediaItemTypes</b>| 字符串数组| 与游戏的媒体组所有返回的项必须具有的子级的媒体项类型是提供的值之一。| 
  
<a id="ID4EEF"></a>

 
## <a name="see-also"></a>另请参阅
 
<a id="ID4EGF"></a>

 
##### <a name="parent"></a>Parent 的子磁盘）  

[其他参考](atoc-xboxlivews-reference-additional.md)

  
<a id="ID4ESF"></a>

 
##### <a name="further-information"></a>详细信息 

[Marketplace Uri](../uri/marketplace/atoc-reference-marketplace.md)

   