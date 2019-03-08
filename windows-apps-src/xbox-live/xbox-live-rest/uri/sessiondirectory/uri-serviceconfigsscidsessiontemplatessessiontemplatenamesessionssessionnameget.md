---
title: GET (/serviceconfigs/{scid}/sessiontemplates/{sessionTemplateName}/sessions/{sessionName})
assetID: 6a4c4a13-c968-3271-cbc3-b742a8de98b3
permalink: en-us/docs/xboxlive/rest/uri-serviceconfigsscidsessiontemplatessessiontemplatenamesessionssessionnameget.html
description: " GET (/serviceconfigs/{scid}/sessiontemplates/{sessionTemplateName}/sessions/{sessionName})"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 21d534d7b55934d7174c925838ed88980acff609
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2019
ms.locfileid: "57655062"
---
# <a name="get-serviceconfigsscidsessiontemplatessessiontemplatenamesessionssessionname"></a>GET (/serviceconfigs/{scid}/sessiontemplates/{sessionTemplateName}/sessions/{sessionName})
获取会话对象。

> [!IMPORTANT]
> 此 URI 方法需要 X Xbl 约定版本标头元素：104/105 或更高版本上的每个请求。

  * [备注](#ID4ET)
  * [URI 参数](#ID4EMB)
  * [HTTP 状态代码](#ID4EZB)
  * [请求正文](#ID4E6B)
  * [响应正文](#ID4EKC)

<a id="ID4ET"></a>


## <a name="remarks"></a>备注

此 HTTP/REST 方法读取指定的名称的会话文档，并检索该会话。 如果成功，它将返回会话对象，包含所有属性，从服务器获取。 此方法的两端可加**Microsoft.Xbox.Services.Multiplayer.MultiplayerService.GetCurrentSessionAsync**。 中指定的 GET 方法的参数直接并行**MultiplayerSessionReference**对象的会话中传递*sessionReference*参数的**GetCurrentSessionAsync**。

GET 方法的传输格式如下所示。

```cpp
GET /serviceconfigs/00000000-0000-0000-0000-000000000000/sessiontemplates/quick/sessions/00000000-0000-0000-0000-000000000001 HTTP/1.1
      Content-Type: application/json

```



<a id="ID4EMB"></a>


## <a name="uri-parameters"></a>URI 参数

| 参数| 在任务栏的搜索框中键入| 描述|
| --- | --- | --- | --- |
| scid| GUID| 服务配置标识符 (SCID)。 会话标识符的第 1 部分中。|
| sessionTemplateName| 字符串| 会话模板的当前实例的名称。 会话标识符的第 2 部分中。|
| sessionName| GUID| 会话的唯一 ID。 会话标识符的第 3 部分。|

<a id="ID4EZB"></a>


## <a name="http-status-codes"></a>HTTP 状态代码
同样适用于 MPSD，服务将返回 HTTP 状态代码。  
<a id="ID4E6B"></a>


## <a name="request-body"></a>请求正文

此请求的正文中不发送的任何对象。

<a id="ID4EKC"></a>


## <a name="response-body"></a>响应正文
请参阅中的响应结构[MultiplayerSession (JSON)](../../json/json-multiplayersession.md)。  
<a id="ID4ETC"></a>


## <a name="see-also"></a>另请参阅

<a id="ID4EVC"></a>


##### <a name="parent"></a>Parent 的子磁盘）

[/serviceconfigs/{scid}/sessiontemplates/{sessionTemplateName}/sessions/{sessionName}](uri-serviceconfigsscidsessiontemplatessessiontemplatenamesessionssessionname.md)
