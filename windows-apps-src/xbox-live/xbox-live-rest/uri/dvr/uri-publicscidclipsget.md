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
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2019
ms.locfileid: "57589802"
---
# <a name="get-publicscidsscidclips"></a>GET (/public/scids/{scid}/clips)
列出公共剪辑。 此 URI 的域是`gameclipsmetadata.xboxlive.com`。
 
  * [备注](#ID4EV)
  * [URI 参数](#ID4ECB)
  * [查询字符串参数](#ID4ENB)
 
<a id="ID4EV"></a>

 
## <a name="remarks"></a>备注
 
此 API 允许的列表都是公共的多个剪辑的各种方法。 在隐私检查和针对请求 XUID 内容隔离检查返回的剪辑列表。
 
每个服务配置标识符 (SCID) 优化查询。 进一步指定筛选器或下面列出的默认值以外的排序顺序可能在某些情况下需要更长时间才能返回。 这是对于较大的视频集更为明显。 查询不能指定升序排序顺序。
 
限定符需获取特定集合 ofpublic 剪辑。 发出请求的用户必须有权访问请求 SCID，否则 HTTP 403 将返回。
  
<a id="ID4ECB"></a>

 
## <a name="uri-parameters"></a>URI 参数
 
| 参数| 在任务栏的搜索框中键入| 描述| 
| --- | --- | --- | 
| scid| 字符串| 主服务配置的公共剪辑的标识符。| 
| titleid| 字符串| 公共剪辑的职务 Id。 不能作为 SCID 相同的 URI 中指定。 如果指定，将用于查找主 SCID。| 
  
<a id="ID4ENB"></a>

 
## <a name="query-string-parameters"></a>查询字符串参数
 
| 参数| 在任务栏的搜索框中键入| 描述| 
| --- | --- | --- | --- | --- | --- | 
| <b>?achievementId={achievementId}</b>| 与指定的最新剪辑<b>achievementId</b>。| 不支持其他排序/筛选。| 
| <b>?greatestMomentId={greatestMomentId}</b>| 与指定的最新剪辑<b>greatestMomentId</b>。| 不支持其他排序/筛选。| 
| <b>？ 限定符 = 创建 </b>| 最新| 必需。| 
  
<a id="ID4EDD"></a>

 
## <a name="see-also"></a>另请参阅
 
<a id="ID4EFD"></a>

 
##### <a name="parent"></a>Parent 的子磁盘） 

[/public/scids/{scid}/clips](uri-publicscidclips.md)

   