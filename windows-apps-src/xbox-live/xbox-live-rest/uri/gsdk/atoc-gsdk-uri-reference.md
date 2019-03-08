---
title: 游戏服务器统一资源标识符 (URI) 引用
assetID: bbd7e3f3-77ac-6ffd-8951-fe4b8b48eb4c
permalink: en-us/docs/xboxlive/rest/atoc-gsdk-uri-reference.html
description: " 游戏服务器统一资源标识符 (URI) 引用"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: a9a0a38cff9214485b2d7e8b1f8a28acb3207444
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2019
ms.locfileid: "57662592"
---
# <a name="game-server-universal-resource-identifier-uri-reference"></a>游戏服务器统一资源标识符 (URI) 引用
客户端用来创建游戏服务器开发工具包标题的服务器实例的 Uri。 这些 Uri 的域是`gameserverds.xboxlive.com`和`gameserverms.xboxlive.com`。
 
<a id="ID4EY"></a>

 
## <a name="in-this-section"></a>本部分内容

[/qosservers](uri-qosservers.md)

&nbsp;&nbsp;客户端以获取可用的 QoS 服务器的列表用于 Xbox Live 计算调用的 URI。

[/titles/{titleId}/clusters](uri-titlestitleidclusters.md)

&nbsp;&nbsp;允许客户端创建标题的 Xbox Live 计算服务器实例的 URI。

[/titles/{titleId}/variants](uri-titlestitleidvariants.md)

&nbsp;&nbsp;若要获取可用的变体的标题的客户端调用的 URI。

[/titles/{titleId}/sessionhosts](uri-titlestitleidsessionhosts.md)

&nbsp;&nbsp;请求 Xbox Live 计算 sessionhost，并将其分配特定的商品 id。

[/titles/{titleId}/sessions/{sessionId}/allocationStatus](uri-titlestitleidsessionssessionidallocationstatus.md)

&nbsp;&nbsp;对于特定的商品 id 和会话 id，获取票证请求的状态。
 