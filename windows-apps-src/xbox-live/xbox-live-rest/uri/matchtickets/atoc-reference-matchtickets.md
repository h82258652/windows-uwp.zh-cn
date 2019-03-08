---
title: 匹配 URI
assetID: 667b02a9-6f34-8165-001b-ee8782575202
permalink: en-us/docs/xboxlive/rest/atoc-reference-matchtickets.html
description: " 匹配 URI"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: c52abca7ed49d4a5e14520095ae944938b86f093
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2019
ms.locfileid: "57590462"
---
# <a name="matchmaking-uris"></a>匹配 URI
 
本部分提供从 Xbox Live 服务中的匹配服务针对有关通用资源标识符 (URI) 地址和关联的超文本传输协议 (HTTP) 方法的详细信息。 
 
<a id="ID4E6"></a>

 
## <a name="domain"></a>域
momatch.xboxlive.com  
<a id="ID4EEB"></a>

 
## <a name="service-version"></a>服务版本
 
这些 HTTP/REST Uri 的调用方必须为 X-Xbl-协定的版本，指定服务版本的娱乐发现服务 (EDS) 的 HTTP 标头传递值 103 或更高版本。 
  
<a id="ID4ELB"></a>

 
## <a name="system-objects-and-properties"></a>系统对象和属性
 
目前，手动执行匹配服务的所有配置使用的服务配置部分[Xbox 开发人员门户 (XDP)](https://xdp.xboxlive.com)或[合作伙伴中心](https://partner.microsoft.com/dashboard)。 一些匹配信息也会反映在 MPSD 为定义的对象中。 
 
主要用于配置匹配的 JSON 对象中定义[MatchTicket (JSON)](../../json/json-matchticket.md)并[HopperStatsResults (JSON)](../../json/json-hopperstatsresults.md)。 请注意，所有与匹配票证必须定义**ticketSessionRef**对象提供对包含播放机或想要与其他人相匹配的球员的多玩家会话的引用。 
  
<a id="ID4EBC"></a>

 
## <a name="in-this-section"></a>本部分内容

[/serviceconfigs/{scid}/hoppers/{hoppername}](uri-serviceconfigsscidhoppershoppername.md)

&nbsp;&nbsp;支持创建匹配票证的 POST 操作。 

[/serviceconfigs/{scid}/hoppers/{name}/stats](uri-serviceconfigsscidhoppershoppernamestats.md)

&nbsp;&nbsp;支持 GET 操作用于检索 hopper 的统计信息。

[/serviceconfigs/{scid}/hoppers/{hoppername}/tickets/{ticketid}](uri-scidhoppernameticketid.md)

&nbsp;&nbsp;支持的匹配项票证的删除操作。
 
<a id="ID4ENC"></a>

 
## <a name="see-also"></a>另请参阅
 
<a id="ID4EPC"></a>

   [MatchTicket (JSON)](../../json/json-matchticket.md)

 [HopperStatsResults (JSON)](../../json/json-hopperstatsresults.md)

 [会话目录 Uri](../sessiondirectory/atoc-reference-sessiondirectory.md)

  
<a id="ID4E2C"></a>

 
##### <a name="parent"></a>Parent 的子磁盘） 

[通用资源标识符 (URI) 引用](../atoc-xboxlivews-reference-uris.md)

   