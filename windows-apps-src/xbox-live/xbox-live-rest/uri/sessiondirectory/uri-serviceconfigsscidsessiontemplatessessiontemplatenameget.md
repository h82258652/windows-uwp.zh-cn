---
title: GET (/serviceconfigs/{scid}/sessiontemplates/{sessionTemplateName})
assetID: 81139619-dc27-1601-30ba-08f6c45aaaca
permalink: en-us/docs/xboxlive/rest/uri-serviceconfigsscidsessiontemplatessessiontemplatenameget.html
author: KevinAsgari
description: " GET (/serviceconfigs/{scid}/sessiontemplates/{sessionTemplateName})"
ms.author: kevinasg
ms.date: 20-12-2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: eaa7314b2cb2c8459f4bd2aae78794685c277c70
ms.sourcegitcommit: 9e2c34a5ed3134aeca7eb9490f05b20eb9a3e5df
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/17/2018
ms.locfileid: "3983429"
---
# <a name="get-serviceconfigsscidsessiontemplatessessiontemplatename"></a>GET (/serviceconfigs/{scid}/sessiontemplates/{sessionTemplateName})
检索一组会话模板名称。

> [!IMPORTANT]
> 此 URI 方法需要 X Xbl 协定版本的标头元素： 104/105 或更高版本上的每个请求。

  * [URI 参数](#ID4ET)
  * [HTTP 状态代码](#ID4E5)
  * [请求正文](#ID4EFB)
  * [响应正文](#ID4EQB)

<a id="ID4ET"></a>


## <a name="uri-parameters"></a>URI 参数

| 参数| 类型| 说明|
| --- | --- | --- | --- |
| scid| GUID| 服务配置标识符 (SCID)。 第 1 部分会话的 id。|
| sessionTemplateName| 字符串| 会话模板的当前实例的名称。 第 2 部分会话的 id。 |

<a id="ID4E5"></a>


## <a name="http-status-codes"></a>HTTP 状态代码
该服务返回 HTTP 状态代码，因为它适用于 MPSD。  
<a id="ID4EFB"></a>


## <a name="request-body"></a>请求正文

此请求的正文中不发送任何对象。

<a id="ID4EQB"></a>


## <a name="response-body"></a>响应正文


```cpp
{
    "results": [ {
            "xuid": "9876",    // If the session was found from a xuid, that xuid.
            "startTime": "2009-06-15T13:45:30.0900000Z",
            "sessionRef": {
                "scid": "foo",
                "templateName": "bar",
                "name": "session-seven"
            },
            "accepted": 4,    // Approximate number of non-reserved members.
            "status": "active",    // or "reserved" or "inactive". This is the state of the user in the session, not the session itself. Only present if the session was found using a xuid.
            "visibility": "open",    // or "private", "visible", or "full"
            "joinRestriction": "local",    // or "followed". Only present if 'visibility' is "open" or "full" and the session has a join restriction.
            "myTurn": true,    // Not present is the same as 'false'. Only present if the session was found using a xuid.
            "keywords": [ "one", "two" ]
        }
    ]
}

```


<a id="ID4EZB"></a>


## <a name="see-also"></a>另请参阅

<a id="ID4E2B"></a>


##### <a name="parent"></a>Parent 的子磁盘）

[/serviceconfigs/{scid}/sessiontemplates/{sessionTemplateName}](uri-serviceconfigsscidsessiontemplatessessiontemplatename.md)
