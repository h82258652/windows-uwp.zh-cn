---
title: EDS 查询优化器
assetID: ab5c066a-a48b-3042-351d-d0a15f663276
permalink: en-us/docs/xboxlive/rest/edsqueryrefiners.html
author: KevinAsgari
description: " EDS 查询优化器"
ms.author: kevinasg
ms.date: 20-12-2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: b049965d619a7c25108e2b6308b18f1e402fecab
ms.sourcegitcommit: 9e2c34a5ed3134aeca7eb9490f05b20eb9a3e5df
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/17/2018
ms.locfileid: "3981201"
---
# <a name="eds-query-refiners"></a>EDS 查询优化器
 
<a id="ID4EO"></a>

  
 
以下参数可以用于优化到更有针对性的一组项娱乐发现服务 (EDS) 查询。 所有这些参数必需的在任何 API 中，但是要接受接受查询精简将任何 API 中。
 
参数名称可以作为传递值到任何"queryRefiners"参数。 此随后返回如果与查询精选重复请求将返回的项数应用，按查询精选的每个值。
 
下面是这实际上可能工作的方式：
 
   * 浏览 api 调用，包括参数"queryRefiners = 流派"。
   * 该 API 将返回八个游戏。 除了项，每个流派包含多个项的列表将返回，以及有多少项属于该流派。 对于游戏时，这可能是"射击： 3，拼图： 5"。
   * 由另一个查询。 它等同于第一，不同之处在于，"流派 = 射击游戏"添加。
   * 现在，则响应将包含仅涉及三个游戏，所有这些属于"射击游戏"类别。
  
| 参数| 数据类型| 说明| 
| --- | --- | --- | 
| <b>十年期</b>| 字符串| 在其中的所有项必须已都发布的十年期。| 
| <b>流派</b>| 字符串的数组| 所有项都必须都有的流派的列表。| 
| <b>labelOwner</b>| 字符串| 与艺术家、 唱片集或跟踪关联的音乐标签。| 
| <b>网络</b>| 字符串的数组| 创建项目的网络。| 
| <b>studio</b>| 字符串的数组| 创建项目 studio。| 
| <b>xboxAppCategories</b>| 字符串的数组| 所有 Xbox 应用都必须都包含的类别的列表。| 
| <b>xboxAvatarClothes</b>| 字符串的数组| 所有 Xbox 头像项必须都具有衣服类型的列表。| 
| <b>xboxAvatarStores</b>| 字符串的数组| 属于哪个所有 Xbox 头像项目必须的存储的列表。| 
| <b>xboxGamePublisherBits</b>| 字符串的数组| 游戏发布者位，必须在所有 GameType 项目或 AppType 项目上设置的列表。| 
| <b>xboxIsBrowsable</b>| 布尔值| 如果<b>true</b>，将返回完整的游戏不是直接操作除了可操作的内容。 默认值为<b>false</b>。| 
| <b>xboxHasChildMediaItemTypes</b>| 字符串的数组| 游戏的媒体组的所有返回的项必须具有其媒体项目类型是一个提供的值的子元素。| 
  
<a id="ID4EEF"></a>

 
## <a name="see-also"></a>另请参阅
 
<a id="ID4EGF"></a>

 
##### <a name="parent"></a>Parent 的子磁盘）  

[其他参考](atoc-xboxlivews-reference-additional.md)

  
<a id="ID4ESF"></a>

 
##### <a name="further-information"></a>详细信息 

[市场 URI](../uri/marketplace/atoc-reference-marketplace.md)

   