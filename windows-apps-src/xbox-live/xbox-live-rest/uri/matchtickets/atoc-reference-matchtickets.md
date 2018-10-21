---
title: 匹配 URI
assetID: 667b02a9-6f34-8165-001b-ee8782575202
permalink: en-us/docs/xboxlive/rest/atoc-reference-matchtickets.html
author: KevinAsgari
description: " 匹配 URI"
ms.author: kevinasg
ms.date: 20-12-2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 7bfeb225c67567c392615686743828941c02f6d2
ms.sourcegitcommit: 72835733ec429a5deb6a11da4112336746e5e9cf
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/21/2018
ms.locfileid: "5171784"
---
# <a name="matchmaking-uris"></a>匹配 URI
 
本部分提供了从 Xbox Live 服务的匹配服务的详细信息的统一资源标识符 (URI) 地址和关联的超文本传输协议 (HTTP) 方法。 
 
<a id="ID4E6"></a>

 
## <a name="domain"></a>域
momatch.xboxlive.com  
<a id="ID4EEB"></a>

 
## <a name="service-version"></a>服务版本
 
这些 HTTP/REST Uri 的调用方必须为 X-Xbl-协定-版本，HTTP 标头，指定的服务版本的娱乐发现服务 (EDS) 传递值 103 或更高版本。 
  
<a id="ID4ELB"></a>

 
## <a name="system-objects-and-properties"></a>系统对象和属性
 
目前，匹配服务的所有配置都发生手动，使用[Xbox 开发人员门户 (XDP)](https://xdp.xboxlive.com)或[Windows 开发人员中心](https://partner.microsoft.com/dashboard/windows/overview)的服务配置部分。 为 MPSD 定义的对象还反映匹配的一些信息。 
 
[MatchTicket (JSON)](../../json/json-matchticket.md)和[HopperStatsResults (JSON)](../../json/json-hopperstatsresults.md)中定义的主要用于配置匹配的 JSON 对象。 请注意所有匹配票证必须都定义一个**ticketSessionRef**对象来提供对包含玩家或想要与其他人匹配的玩家的多人游戏会话的引用。 
  
<a id="ID4EBC"></a>

 
## <a name="in-this-section"></a>本部分内容

[/serviceconfigs/{scid}/hoppers/{hoppername}](uri-serviceconfigsscidhoppershoppername.md)

&nbsp;&nbsp;支持 POST 操作以创建匹配票证。 

[/serviceconfigs/{scid}/hoppers/{name}/stats](uri-serviceconfigsscidhoppershoppernamestats.md)

&nbsp;&nbsp;支持获取操作来检索漏斗的统计信息。

[/serviceconfigs/{scid}/hoppers/{hoppername}/tickets/{ticketid}](uri-scidhoppernameticketid.md)

&nbsp;&nbsp;支持的匹配票证的删除操作。
 
<a id="ID4ENC"></a>

 
## <a name="see-also"></a>另请参阅
 
<a id="ID4EPC"></a>

   [MatchTicket (JSON)](../../json/json-matchticket.md)

 [HopperStatsResults (JSON)](../../json/json-hopperstatsresults.md)

 [会话目录 URI](../sessiondirectory/atoc-reference-sessiondirectory.md)

  
<a id="ID4E2C"></a>

 
##### <a name="parent"></a>Parent 的子磁盘） 

[统一资源标识符 (URI) 参考](../atoc-xboxlivews-reference-uris.md)

   