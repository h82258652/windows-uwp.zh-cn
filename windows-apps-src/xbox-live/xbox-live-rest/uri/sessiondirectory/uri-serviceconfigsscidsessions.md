---
title: /serviceconfigs/{scid}/sessions
assetID: d1932817-dcce-5a5c-d7e4-9fd97e1d287c
permalink: en-us/docs/xboxlive/rest/uri-serviceconfigsscidsessions.html
description: " /serviceconfigs/{scid}/sessions"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 700575ee9e9afa7bc7a1d34388dc071873d3b9ed
ms.sourcegitcommit: d2517e522cacc5240f7dffd5bc1eaa278e3f7768
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/02/2018
ms.locfileid: "8328174"
---
# <a name="serviceconfigsscidsessions"></a>/serviceconfigs/{scid}/sessions
支持 GET 操作以检索一组会话文档。 
<a id="ID4EO"></a>

 
## <a name="domain"></a>域
sessiondirectory.xboxlive.com  
<a id="ID4ET"></a>

 
## <a name="uri-parameters"></a>URI 参数
 
| 参数| 类型| 描述| 
| --- | --- | --- | 
| scid| GUID| 服务配置标识符 (SCID)。 第 1 部分会话的 id。| 
| 关键字| 字符串| 用来筛选结果以标识与该字符串的只是会话的关键字。| 
| xuid| GUID| 要检索会话为其用户的 Xbox 用户 Id。 用户必须在会话中处于活动状态。| 
| 预订| 字符串| 指示会话列表包括那些不接受用户具有的值。 仅可以将此参数设置为 true。 此设置需要调用方拥有对会话，服务器级访问权限或调用方的 XUID 声明以匹配的 Xbox 用户 ID 筛选器。 | 
| 处于非活动状态| 字符串| 指示会话列表包括用户已接受，但不会主动玩游戏的值。 仅可以将此参数设置为 true。| 
| 专用| 字符串| 值，该值指示会话列表是否包含专用会话。 仅可以将此参数设置为 true。 仅在查询你自己的会话，或进行服务器到服务器查询时，它是有效的。 将此参数设置为 true 要求调用方拥有对会话的服务器级访问权限或调用方的 XUID 声明以匹配的 Xbox 用户 ID 筛选器。 | 
| 可见性| 字符串| 枚举值，指示用于筛选结果的可见性状态。 当前此参数可以仅设置为打开包含开放会话。 请参阅<b>MultiplayerSessionVisibility</b>。| 
| version| 字符串| 正整数，该值的主要的会话版本或较低的会话以包括。 值必须小于或等于 100 模请求的协定版本。| 
| 参加| 字符串| 正整数指示会话的最大数量来检索。| 
  
<a id="ID4E1D"></a>

 
## <a name="valid-methods"></a>有效的方法

[GET (/serviceconfigs/{scid}/sessions)](uri-serviceconfigsscidsessionsget.md)

&nbsp;&nbsp;检索指定会话信息。
 
<a id="ID4EEE"></a>

 
## <a name="see-also"></a>另请参阅
 
<a id="ID4EGE"></a>

 
##### <a name="parent"></a>Parent 的子磁盘） 

[会话目录 URI](atoc-reference-sessiondirectory.md)

   