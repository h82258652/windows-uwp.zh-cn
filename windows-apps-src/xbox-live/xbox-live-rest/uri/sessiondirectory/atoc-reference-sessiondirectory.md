---
title: 会话目录 URI
assetID: e3ba951d-b21f-0014-c358-2603d549d118
permalink: en-us/docs/xboxlive/rest/atoc-reference-sessiondirectory.html
description: " 会话目录 URI"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 9492ff3272af830404a546c9b01d62178adbac96
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2019
ms.locfileid: "57651192"
---
# <a name="session-directory-uris"></a>会话目录 URI

本部分提供有关通用资源标识符 (URI) 地址和关联的超文本传输协议 (HTTP) 方法的详细信息从 Xbox Live 服务的多人游戏会话目录 (MPSD)。


> [!NOTE] 
> 仅在 Xbox 360 上、 在 Windows Phone 设备上，或在 Xbox.com 上运行的游戏标题可以使用会话目录 Uri。  


  * [Domain](#ID4EUB)
  * [服务版本](#ID4EZB)
  * [系统对象和属性](#ID4EAC)
  * [句柄](#ID4EBE)

<a id="ID4EUB"></a>


## <a name="domain"></a>域
sessiondirectory.xboxlive.com  
<a id="ID4EZB"></a>


## <a name="service-version"></a>服务版本

这些 REST Uri 的调用方必须为 X-Xbl-协定的版本，指定服务版本的娱乐发现服务 (EDS) 的 HTTP 标头传递值 104/105 或更高版本。

<a id="ID4EAC"></a>


## <a name="system-objects-and-properties"></a>系统对象和属性

对于其会话和模板的配置，MPSD 具有固定架构的目录强制实施并解释使用多个会话符合的 JSON 对象。 在对支持各种会话目录的 Uri 的方法调用中，这些对象是验证和合并，基于支持的架构。 是具有多玩家配置相关联的主要 JSON 对象：

   *  [MultiplayerActivityDetails (JSON)](../../json/json-multiplayeractivitydetails.md)
   *  [MultiplayerSession (JSON)](../../json/json-multiplayersession.md)
   *  [MultiplayerSessionReference (JSON)](../../json/json-multiplayersessionreference.md)
   *  [MultiplayerSessionRequest (JSON)](../../json/json-multiplayersessionrequest.md)


关联的 JSON 对象专门与游戏有这样担心的是：

   *  [GameMessage (JSON)](../../json/json-gamemessage.md)
   *  [GameResult (JSON)](../../json/json-gameresult.md)
   *  [GameSession (JSON)](../../json/json-gamesession.md)
   *  [GameSessionSummary (JSON)](../../json/json-gamesessionsummary.md)


<a id="ID4EBE"></a>


## <a name="handles"></a>句柄

有关 2015年之多人游戏仅，会话可以访问通过会话句柄。 添加了多个 Uri，以提供功能以支持句柄。  
<a id="ID4EFE"></a>


## <a name="in-this-section"></a>本部分内容

[/handles](uri-handles.md)

&nbsp;&nbsp;支持将显示在 Xbox One 的仪表板的用户体验，并邀请会话成员，如果需要用户的当前活动会话设置的 POST 操作。

[/handles/{handleId}](uri-handleshandleid.md)

&nbsp;&nbsp;支持 DELETE 和 GET 操作的标识符指定的会话句柄。

[/handles/{handleId}/session](uri-handleshandleidsession.md)

&nbsp;&nbsp;支持 PUT 和 GET 操作适用于会话中，使用句柄取消引用。

[/handles/query](uri-handlesquery.md)

&nbsp;&nbsp;支持 POST 操作创建的会话句柄的查询。

[/serviceconfigs/{scid}/batch](uri-serviceconfigsscidbatch.md)

&nbsp;&nbsp;支持在服务配置标识符级别批查询的 POST 操作。

[/serviceconfigs/{scid}/sessions](uri-serviceconfigsscidsessions.md)

&nbsp;&nbsp;支持 GET 操作来检索一组会话文档。

[/serviceconfigs/{scid}/sessiontemplates](uri-serviceconfigsscidsessiontemplates.md)

&nbsp;&nbsp;支持 GET 操作来检索一组 MPSD 会话模板。

[/serviceconfigs/{scid}/sessiontemplates/{sessionTemplateName}](uri-serviceconfigsscidsessiontemplatessessiontemplatename.md)

&nbsp;&nbsp;支持 GET 操作来检索一组会话模板名称。

[/serviceconfigs/{scid}/sessiontemplates/{sessionTemplateName}/batch](uri-serviceconfigscidsessiontemplatessessiontemplatenamebatch.md)

&nbsp;&nbsp;支持会话级别的模板创建批查询的 POST 操作。

[/serviceconfigs/{scid}/sessiontemplates/{sessionTemplateName}/sessions](uri-serviceconfigsscidsessiontemplatessessiontemplatenamesessions.md)

&nbsp;&nbsp;支持 GET 操作来检索一组会话模板使用指定的模板名称。

[/serviceconfigs/{scid}/sessiontemplates/{sessionTemplateName}/sessions/{sessionName}](uri-serviceconfigsscidsessiontemplatessessiontemplatenamesessionssessionname.md)

&nbsp;&nbsp;支持 PUT 和 GET 操作来创建和检索会话。

[/serviceconfigs/{scid}/sessiontemplates/{sessionTemplateName}/sessions/{sessionName}/members/{index}](uri-serviceconfigsscidsessiontemplatessessiontemplatenamesessionnamemembersindex.md)

&nbsp;&nbsp;支持删除操作删除指定的会话成员。

[/serviceconfigs/{scid}/sessiontemplates/{sessionTemplateName}/sessions/{sessionName}/members/me](uri-serviceconfigsscidsessiontemplatessessiontemplatenamesessionssessionnamemembersme.md)

&nbsp;&nbsp;支持删除操作删除会话成员。

[/serviceconfigs/{scid}/sessiontemplates/{sessionTemplateName}/sessions/{sessionName}/servers/{server-name}](uri-serviceconfigsscidsessiontemplatessessiontemplatenamesessionnamemembersservername.md)

&nbsp;&nbsp;支持删除操作删除指定的服务器的会话。

<a id="ID4ESF"></a>


## <a name="see-also"></a>另请参阅

<a id="ID4EUF"></a>

   [匹配 Uri](../matchtickets/atoc-reference-matchtickets.md)


<a id="ID4E1F"></a>


##### <a name="parent"></a>Parent 的子磁盘）

[通用资源标识符 (URI) 引用](../atoc-xboxlivews-reference-uris.md)
