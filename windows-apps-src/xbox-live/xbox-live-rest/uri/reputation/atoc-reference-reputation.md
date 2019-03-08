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
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2019
ms.locfileid: "57614002"
---
# <a name="reputation-uris"></a>信誉 URI
 
本部分提供有关通用资源标识符 (URI) 地址和关联的超文本传输协议 (HTTP) 方法的详细信息从 Xbox Live 服务中针对**Microsoft.Xbox.Services.Social.ReputationService**. Uri 的信誉的域是 reputation.xboxlive.com。 典型 URI 表示形式可能是 https://reputation.xboxlive.com/users/xuid(2533274790412952)/feedback。 
 
信誉服务使用的反馈，如中所述[反馈 (JSON)](../../json/json-feedback.md)，用于计算名声分值。 此分数将保存在项 ReputationOverall 下用户的统计信息的区域。 检索用户统计信息的详细信息，请参阅[获取 (/users/xuid({xuid})/scids/{scid}/stats)](../userstats/uri-usersxuidscidsscidstatsget.md)。 
 
在所有平台上的游戏可以使用信誉服务。
 
<a id="ID4EMB"></a>

 
## <a name="in-this-section"></a>本部分内容

[/users/xuid({xuid})/feedback](uri-reputationusersxuidfeedback.md)

&nbsp;&nbsp;如果要在您的游戏，而不被使用命令行程序中添加反馈选项，则使用从您的标题。

[/users/batchfeedback](uri-reputationusersbatchfeedback.md)

&nbsp;&nbsp;标题的服务用于标题的接口之外的批处理形式发送反馈。

[/users/me/resetreputation](uri-usersmeresetreputation.md)

&nbsp;&nbsp;利用强制执行团队可以访问当前用户的信誉分数。

[/users/xuid({xuid})/deleteuserdata](uri-usersxuiddeleteuserdata.md)

&nbsp;&nbsp;完全重置的测试用户的信誉数据。 仅用于测试。

[/users/xuid({xuid})/resetreputation](uri-usersxuidresetreputation.md)

&nbsp;&nbsp;利用强制执行团队可以访问指定的用户的信誉分数。
 
<a id="ID4E5B"></a>

 
## <a name="see-also"></a>另请参阅
 
<a id="ID4EAC"></a>

 
##### <a name="parent"></a>Parent 的子磁盘） 

[通用资源标识符 (URI) 引用](../atoc-xboxlivews-reference-uris.md)

   