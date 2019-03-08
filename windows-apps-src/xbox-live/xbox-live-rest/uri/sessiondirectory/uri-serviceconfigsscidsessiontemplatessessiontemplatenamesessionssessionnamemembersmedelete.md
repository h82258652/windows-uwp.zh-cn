---
title: DELETE (/serviceconfigs/{scid}/sessiontemplates/{sessionTemplateName}/sessions/{sessionName}/members/me)
assetID: aa5de623-7787-a47c-b7e4-305693b9fe35
permalink: en-us/docs/xboxlive/rest/uri-serviceconfigsscidsessiontemplatessessiontemplatenamesessionssessionnamemembersmedelete.html
description: " DELETE (/serviceconfigs/{scid}/sessiontemplates/{sessionTemplateName}/sessions/{sessionName}/members/me)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 3de35398f4685a0b0cfda1a251c65ed6a74956d7
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2019
ms.locfileid: "57637192"
---
# <a name="delete-serviceconfigsscidsessiontemplatessessiontemplatenamesessionssessionnamemembersme"></a>DELETE (/serviceconfigs/{scid}/sessiontemplates/{sessionTemplateName}/sessions/{sessionName}/members/me)
从会话中删除成员。

> [!IMPORTANT]
> 此 URI 方法需要 X Xbl 约定版本标头元素：104/105 或更高版本上的每个请求。

  * [备注](#ID4ET)
  * [URI 参数](#ID4E3)
  * [HTTP 状态代码](#ID4EHB)
  * [请求正文](#ID4ENB)
  * [响应正文](#ID4EYB)

<a id="ID4ET"></a>


## <a name="remarks"></a>备注
所有会话成员资源操作都需要 Xbox 用户 ID (XUID) 授权。  
<a id="ID4E3"></a>


## <a name="uri-parameters"></a>URI 参数

| 参数| 在任务栏的搜索框中键入| 描述|
| --- | --- | --- | --- |
| scid| GUID| 服务配置标识符 (SCID)。 会话标识符的第 1 部分中。|
| sessionTemplateName| 字符串| 会话模板的当前实例的名称。 会话标识符的第 2 部分中。|
| sessionName| GUID| 会话的唯一 ID。 会话标识符的第 3 部分。|

<a id="ID4EHB"></a>


## <a name="http-status-codes"></a>HTTP 状态代码
同样适用于 MPSD，服务将返回 HTTP 状态代码。  
<a id="ID4ENB"></a>


## <a name="request-body"></a>请求正文

此请求的正文中不发送的任何对象。

<a id="ID4EYB"></a>


## <a name="response-body"></a>响应正文
请参阅中的响应结构[MultiplayerSession (JSON)](../../json/json-multiplayersession.md)。  
<a id="ID4EBC"></a>


## <a name="see-also"></a>另请参阅

<a id="ID4EDC"></a>


##### <a name="parent"></a>Parent 的子磁盘）

[/serviceconfigs/{scid}/sessiontemplates/{sessionTemplateName}/sessions/{sessionName}/members/me](uri-serviceconfigsscidsessiontemplatessessiontemplatenamesessionssessionnamemembersme.md)
