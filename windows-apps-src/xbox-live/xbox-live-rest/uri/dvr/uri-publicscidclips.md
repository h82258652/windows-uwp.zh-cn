---
title: /public/scids/{scid}/clips
assetID: 55a1f0ae-08bb-6d1e-a1da-cbeb481c42ee
permalink: en-us/docs/xboxlive/rest/uri-publicscidclips.html
description: " /public/scids/{scid}/clips"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: db279d546780ed40158894f73ecb84687ef35ba6
ms.sourcegitcommit: d2517e522cacc5240f7dffd5bc1eaa278e3f7768
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/02/2018
ms.locfileid: "8331090"
---
# <a name="publicscidsscidclips"></a>/public/scids/{scid}/clips
访问公开的剪辑。 实际上可以在两种形式，指定此 URI`/public/scids/{scid}/clips`和`/public/titles/{titleId}/clips`。 有关详细信息，请参阅下方。 此 URI 的域是`gameclipsmetadata.xboxlive.com`。
 
  * [URI 参数](#ID4E1)
 
<a id="ID4E1"></a>

 
## <a name="uri-parameters"></a>URI 参数
 
| 参数| 类型| 描述| 
| --- | --- | --- | 
| scid| 字符串| 公共剪辑主要服务配置标识符。| 
| titleid| 字符串| 公共剪辑的职务 Id。 不能在相同的 URI 的 scid 中指定。 如果已指定，将用于查找主 SCID。| 
  
<a id="ID4E6B"></a>

 
## <a name="valid-methods"></a>有效的方法

[GET (/public/scids/{scid}/clips)](uri-publicscidclipsget.md)

&nbsp;&nbsp;列出公共剪辑。
 
<a id="ID4EJC"></a>

 
## <a name="see-also"></a>另请参阅
 
<a id="ID4ELC"></a>

 
##### <a name="parent"></a>Parent 的子磁盘） 

[市场 URI](../marketplace/atoc-reference-marketplace.md)

   