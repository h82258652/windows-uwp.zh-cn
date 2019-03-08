---
title: EDS 反向查找视频
assetID: 773f7a8e-7571-3aec-96d6-478437696ea6
permalink: en-us/docs/xboxlive/rest/edsreverselookup.html
description: " EDS 反向查找视频"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: d535dec8a95eba4d486bfc6946e187e2da66ae49
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2019
ms.locfileid: "57598432"
---
# <a name="eds-reverse-lookup-for-video"></a>EDS 反向查找视频
 
  * [反向查找步骤](#ID4EQ)
 
<a id="ID4EQ"></a>

 
## <a name="reverse-lookup-steps"></a>反向查找步骤
 
娱乐发现服务 (EDS) 反向查找支持的所有视频媒体类型 (**MediaItemType.Movie**， **MediaItemType.TVSeries**， **MediaItemType.TVEpisode**， **MediaItemType.TVSeason**，和**MediaItemType.TVShow**)，以及**MediaItemType.Unknown**。
 
反向查找需要 4 个参数传递： 
   * `idType=ScopedMediaId`
   * `ids=` 提供程序的媒体 ID
   * `ScopeIdType=Title`
   * `ScopeId=` 提供程序标题 ID
 
 
通常反向查找需要 2 个步骤： 
   * 如果不可用，则检索提供程序 （例如，从详细信息调用） 的媒体 id。 

```cpp
GET /media/en-us/details?ids=4eeaf5b4-9af2-56e4-a738-68b48e954494&desiredMediaItemTypes=Movie&desired=Providers
```

 
   * 发出的调用使用反向查找**ProviderMediaId**字段前一响应中： 

```cpp
GET /media/en-us/details?ids=047d19ca-3a7d-462c-bdbb-163543125583&idType=ScopedMediaId&desiredMediaItemTypes=Movie&fields=all&ScopeIdType=Title&ScopeId=0x5848085B
```

 
  
 
如果**ProviderMediaId**不从 EDS 检索字段，则此字段必须是 URL 编码，以便正确传递给 EDS。
  
<a id="ID4EOC"></a>

 
## <a name="see-also"></a>另请参阅
 
<a id="ID4EQC"></a>

 
##### <a name="parent"></a>Parent 的子磁盘）  

[其他参考](atoc-xboxlivews-reference-additional.md)

  
<a id="ID4E3C"></a>

 
##### <a name="further-information"></a>详细信息 

[Marketplace Uri](../uri/marketplace/atoc-reference-marketplace.md)

   