---
title: GET (/public/scids/{scid}/clips)
assetID: 15b3e873-1f96-b1da-2f79-6dac1369a4c0
permalink: en-us/docs/xboxlive/rest/uri-publicscidclipsget.html
description: " GET (/public/scids/{scid}/clips)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 5bce1dd261e0ad1172068a0287519cd0480da85f
ms.sourcegitcommit: d2517e522cacc5240f7dffd5bc1eaa278e3f7768
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/02/2018
ms.locfileid: "8334368"
---
# <a name="get-publicscidsscidclips"></a>GET (/public/scids/{scid}/clips)
列出公共剪辑。 此 URI 的域是`gameclipsmetadata.xboxlive.com`。
 
  * [备注](#ID4EV)
  * [URI 参数](#ID4ECB)
  * [查询字符串参数](#ID4ENB)
 
<a id="ID4EV"></a>

 
## <a name="remarks"></a>备注
 
此 API 支持不同方法是公共的列表剪辑。 隐私检查和防止请求的 XUID 的内容隔离检查返回的剪辑列表。
 
查询每个服务配置标识符 (SCID) 进行了优化。 指定进一步筛选器或下面列出的默认值以外的排序顺序可以在某些情况下长返回。 这会更明显的较大的视频集。 查询不能指定升序排序顺序。
 
若要获取特定集合 ofpublic 剪辑，需要使用限定符。 请求的用户必须能够接触到请求的 SCID，否则 HTTP 403 会返回。
  
<a id="ID4ECB"></a>

 
## <a name="uri-parameters"></a>URI 参数
 
| 参数| 类型| 描述| 
| --- | --- | --- | 
| scid| 字符串| 公共剪辑主要服务配置标识符。| 
| titleid| 字符串| 公共剪辑的职务 Id。 不能在相同的 URI 的 scid 中指定。 如果已指定，将用于查找主 SCID。| 
  
<a id="ID4ENB"></a>

 
## <a name="query-string-parameters"></a>查询字符串参数
 
| 参数| 类型| 描述| 
| --- | --- | --- | --- | --- | --- | 
| <b>？ achievementId = {achievementId}</b>| 匹配指定的<b>achievementId</b>最新的剪辑。| 不支持其他排序/筛选。| 
| <b>？ greatestMomentId = {greatestMomentId}</b>| 匹配指定的<b>greatestMomentId</b>最新的剪辑。| 不支持其他排序/筛选。| 
| <b>？ 限定符 = 创建 </b>| 最新| 必需。| 
  
<a id="ID4EDD"></a>

 
## <a name="see-also"></a>另请参阅
 
<a id="ID4EFD"></a>

 
##### <a name="parent"></a>Parent 的子磁盘） 

[/public/scids/{scid}/clips](uri-publicscidclips.md)

   