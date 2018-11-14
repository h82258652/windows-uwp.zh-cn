---
title: GET (/serviceconfigs/{scid}/sessiontemplates/{sessionTemplateName}/sessions/{sessionName})
assetID: 6a4c4a13-c968-3271-cbc3-b742a8de98b3
permalink: en-us/docs/xboxlive/rest/uri-serviceconfigsscidsessiontemplatessessiontemplatenamesessionssessionnameget.html
author: KevinAsgari
description: " GET (/serviceconfigs/{scid}/sessiontemplates/{sessionTemplateName}/sessions/{sessionName})"
ms.author: kevinasg
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: d1bee897396236e0bb4423517371ebc2d25482c3
ms.sourcegitcommit: f2c9a050a9137a473f28b613968d5782866142c6
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/10/2018
ms.locfileid: "6264758"
---
# <a name="get-serviceconfigsscidsessiontemplatessessiontemplatenamesessionssessionname"></a>GET (/serviceconfigs/{scid}/sessiontemplates/{sessionTemplateName}/sessions/{sessionName})
获取会话对象。

> [!IMPORTANT]
> 此 URI 方法需要 X Xbl 协定版本的标头元素： 104/105 或更高版本上每个请求。

  * [备注](#ID4ET)
  * [URI 参数](#ID4EMB)
  * [HTTP 状态代码](#ID4EZB)
  * [请求正文](#ID4E6B)
  * [响应正文](#ID4EKC)

<a id="ID4ET"></a>


## <a name="remarks"></a>备注

此 HTTP/REST 方法读取会话文档为指定的名称，并检索会话。 成功时，它将返回该会话对象，与所有属性，从服务器获取。 此方法可以通过**Microsoft.Xbox.Services.Multiplayer.MultiplayerService.GetCurrentSessionAsync**换行。 GET 方法的参数直接并行这些对象中指定**MultiplayerSessionReference**会话，在**GetCurrentSessionAsync** *sessionReference*参数中传递。

GET 方法的线格式如下所示。

```cpp
GET /serviceconfigs/00000000-0000-0000-0000-000000000000/sessiontemplates/quick/sessions/00000000-0000-0000-0000-000000000001 HTTP/1.1
      Content-Type: application/json

```



<a id="ID4EMB"></a>


## <a name="uri-parameters"></a>URI 参数

| 参数| 类型| 说明|
| --- | --- | --- | --- |
| scid| GUID| 服务配置标识符 (SCID)。 第 1 部分会话标识符。|
| sessionTemplateName| 字符串| 会话模板的当前实例的名称。 第 2 部分会话标识符。|
| 会话名| GUID| 在会话的唯一 ID。 会话标识符的第 3 部分。|

<a id="ID4EZB"></a>


## <a name="http-status-codes"></a>HTTP 状态代码
该服务返回 HTTP 状态代码，因为它适用于 MPSD。  
<a id="ID4E6B"></a>


## <a name="request-body"></a>请求正文

此请求的正文中不发送任何对象。

<a id="ID4EKC"></a>


## <a name="response-body"></a>响应正文
请参阅[MultiplayerSession (JSON)](../../json/json-multiplayersession.md)中的响应结构。  
<a id="ID4ETC"></a>


## <a name="see-also"></a>另请参阅

<a id="ID4EVC"></a>


##### <a name="parent"></a>Parent 的子磁盘）

[/serviceconfigs/{scid}/sessiontemplates/{sessionTemplateName}/sessions/{sessionName}](uri-serviceconfigsscidsessiontemplatessessiontemplatenamesessionssessionname.md)
