---
title: 信誉 URI
assetID: d1cb76c0-86a4-8c51-19f6-5f223b517d46
permalink: en-us/docs/xboxlive/rest/atoc-reference-reputation.html
description: " 信誉 URI"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 93d6d6e6acfd8fa39bd9d26c87ed99362d2c88d6
ms.sourcegitcommit: 89ff8ff88ef58f4fe6d3b1368fe94f62e59118ad
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/29/2018
ms.locfileid: "8217906"
---
# <a name="reputation-uris"></a>信誉 URI
 
本部分提供有关统一资源标识符 (URI) 地址和关联的超文本传输协议 (HTTP) 方法的详细信息从**Microsoft.Xbox.Services.Social.ReputationService**的 Xbox Live 服务。 信誉 Uri 的域是 reputation.xboxlive.com。 典型的 URI 表示可能是https://reputation.xboxlive.com/users/xuid(2533274790412952)/feedback。 
 
信誉服务使用的反馈，[反馈 (JSON)](../../json/json-feedback.md)，所述计算信誉评分。 该分数保存在项 ReputationOverall 下用户的统计信息区域中。 有关检索用户统计信息的详细信息，请参阅[获取 (/users/xuid({xuid})/scids/{scid}/stats)](../userstats/uri-usersxuidscidsscidstatsget.md)。 
 
在所有平台上的游戏可以使用信誉服务。
 
<a id="ID4EMB"></a>

 
## <a name="in-this-section"></a>本部分内容

[/users/xuid({xuid})/feedback](uri-reputationusersxuidfeedback.md)

&nbsp;&nbsp;如果你希望能够在游戏中，而不是使用 shell 添加反馈选项，用于从你的游戏。

[/users/batchfeedback](uri-reputationusersbatchfeedback.md)

&nbsp;&nbsp;使用你的游戏服务来在你的游戏界面之外的批次窗体发送反馈。

[/users/me/resetreputation](uri-usersmeresetreputation.md)

&nbsp;&nbsp;使访问当前用户的信誉评分，强制执行团队。

[/users/xuid({xuid})/deleteuserdata](uri-usersxuiddeleteuserdata.md)

&nbsp;&nbsp;完全重置为测试用户信誉数据。 仅供测试。

[/users/xuid({xuid})/resetreputation](uri-usersxuidresetreputation.md)

&nbsp;&nbsp;允许访问指定的用户的信誉评分，强制执行团队。
 
<a id="ID4E5B"></a>

 
## <a name="see-also"></a>另请参阅
 
<a id="ID4EAC"></a>

 
##### <a name="parent"></a>Parent 的子磁盘） 

[统一资源标识符 (URI) 参考](../atoc-xboxlivews-reference-uris.md)

   