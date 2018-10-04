---
title: Xbox Live SDK 的新增功能 - 2016 年 6 月
author: KevinAsgari
description: Xbox Live SDK 的新增功能 - 2016 年 6 月
ms.assetid: 306e7fa8-14f0-437f-a991-6693f5c0d6a8
ms.author: kevinasg
ms.date: 04/04/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: c7101dd5cb5e481c1ccfb03e6f33f759196bbd1d
ms.sourcegitcommit: e6daa7ff878f2f0c7015aca9787e7f2730abcfbf
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/04/2018
ms.locfileid: "4315050"
---
# <a name="whats-new-for-the-xbox-live-sdk---june-2016"></a>Xbox Live SDK 的新增功能 - 2016 年 6 月

请参阅[新增功能 - 2016 年 4 月](1604-whats-new.md)文章了解 2016 年 4 月版本中增加了哪些功能。

> **注意：** 如果你使用 Xbox 开发人员工具包 (XDK)，那么*新增功能 - 2016 年 4 月*一文中包括了自 3 月 XDK 发布以来增加的其他新功能。

## <a name="os-and-tool-support"></a>操作系统和工具支持
Xbox Live SDK 支持 Windows 10 RTM [版本 10.0.10240] 和 Visual Studio 2015 RTM [版本 14.0.23107.0]。

## <a name="contextual-search"></a>上下文搜索
以下新类已添加到 `contextual_search` API 用来检索游戏剪辑：。

* `contextual_search_game_clip`
* `contextual_search_game_clip_stat`
* `contextual_search_game_clip_thumbnail`
* `contextual_search_game_clip_uri_info`
* `contextual_search_game_clips_result`

有关详细信息，请参阅参考文档。

## <a name="networking"></a>网络
Xbox Live SDK 现在包含能检测是否在单个游戏实例中按用户创建了 5 个或更多 WebSocket 的新声明。

你可以通过调用 `disable_asserts_for_maximum_number_of_websockets_activated()` 禁用此声明。

> **注意：** 如果你在游戏中使用这些功能，社交管理器和多人游戏管理器使用单个组合 Websocket。

## <a name="tools"></a>工具
* Fiddler 的 Xbox Live 复原插件现在包含在 Xbox Live SDK 中。  
它是 Fiddler 的一个加载项，让开发人员能够有选择地阻止对 Xbox Live 的调用。
使用此插件，开发人员可以在他们的游戏中跨部分服务中断测试复原。
该工具包括很多内置的 Xbox Live 服务终结点，以及测试对照的不同错误类型。
所有 Xbox One 和 UWP 游戏均受支持。
