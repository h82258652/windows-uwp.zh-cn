---
title: 游戏服务器统一资源标识符 (URI) 引用
assetID: bbd7e3f3-77ac-6ffd-8951-fe4b8b48eb4c
permalink: en-us/docs/xboxlive/rest/atoc-gsdk-uri-reference.html
author: KevinAsgari
description: " 游戏服务器统一资源标识符 (URI) 引用"
ms.author: kevinasg
ms.date: 20-12-2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 912c3febd0a29a9aca326761ae63e61a0bdfada0
ms.sourcegitcommit: 9e2c34a5ed3134aeca7eb9490f05b20eb9a3e5df
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/17/2018
ms.locfileid: "3984247"
---
# <a name="game-server-universal-resource-identifier-uri-reference"></a>游戏服务器统一资源标识符 (URI) 引用
客户端用于创建游戏的游戏服务器开发工具包服务器实例的 Uri。 有关这些 Uri 域是`gameserverds.xboxlive.com`和`gameserverms.xboxlive.com`。
 
<a id="ID4EY"></a>

 
## <a name="in-this-section"></a>本部分内容

[/qosservers](uri-qosservers.md)

&nbsp;&nbsp;通过适用于 Xbox Live 计算获取可用的 QoS 服务器列表中的客户端调用的 URI。

[/titles/{titleId}/clusters](uri-titlestitleidclusters.md)

&nbsp;&nbsp;允许客户端创建游戏的 Xbox Live 计算服务器实例的 URI。

[/titles/{titleId}/variants](uri-titlestitleidvariants.md)

&nbsp;&nbsp;调用由客户端的游戏获取可用的变体的 URI。

[/titles/{titleId}/sessionhosts](uri-titlestitleidsessionhosts.md)

&nbsp;&nbsp;请求 Xbox Live 计算 sessionhost，为给定的主题作品 id 分配。

[/titles/{titleId}/sessions/{sessionId}/allocationStatus](uri-titlestitleidsessionssessionidallocationstatus.md)

&nbsp;&nbsp;对于给定的作品 id 和会话 id，获取票证请求的状态。
 