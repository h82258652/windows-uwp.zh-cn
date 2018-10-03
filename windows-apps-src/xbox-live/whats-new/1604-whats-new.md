---
title: Xbox Live SDK 的新增功能 - 2016 年 4 月
author: KevinAsgari
description: Xbox Live SDK 的新增功能 - 2016 年 4 月
ms.assetid: a6f26ffd-f136-4753-b0cd-92b0da522d93
ms.author: kevinasg
ms.date: 04/04/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 51c9aabc844c9971be4afec5023d55905c4261ef
ms.sourcegitcommit: 1938851dc132c60348f9722daf994b86f2ead09e
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/03/2018
ms.locfileid: "4261041"
---
# <a name="whats-new-for-the-xbox-live-sdk---april-2016"></a>Xbox Live SDK 的新增功能 - 2016 年 4 月

请参阅[新增功能 - 2016 年 3 月](1603-whats-new.md)文章了解 1603 中增加了哪些功能

## <a name="os-and-tool-support"></a>操作系统和工具支持
Xbox Live SDK 支持 Windows 10 RTM [版本 10.0.10240] 和 Visual Studio 2015 RTM [版本 14.0.23107.0]。

## <a name="tournaments"></a>锦标赛
- Xbox Live 锦标赛工具现在包含在 SDK 中。  你可以在“工具”目录下找到它，其中还提供了一些有关如何使用的信息。
- 另外还提供了锦标赛 API。  请参见 Xbox::Services::Tournaments 命名空间
- 文档还在“编程指南”中提供。

## <a name="documentation"></a>文档
- [登录疑难解答指南](../using-xbox-live/troubleshooting/troubleshooting-sign-in.md)列出了一些调试登录失败的一般策略，以及根据错误代码要执行的步骤。
- 面向 Xbox One 开发人员的[应用商店](https://developer.microsoft.com/en-us/games/xbox/docs/xboxlive/xbox-live-partners/xbox-marketplace/marketplace-and-downloadable-content)文档现在只能在“编程指南”中找到。  UWP 开发人员应继续咨询 Windows 开发人员中心了解应用商店文档的情况。
- 如果你有兴趣将 Xbox One 游戏带入通用 Windows 平台，可以参阅[从 XDK 到 UWP 的移植指南](../using-xbox-live/porting-xbox-live-code-from-xdk-to-uwp.md)。
- 请参阅[细化速率限制](../using-xbox-live/best-practices/fine-grained-rate-limiting.md)一文，了解如何对各个 Xbox Live 服务终结点和方案强制执行这些限制，以及有关具体限制的信息。

## <a name="multiplayer-manager"></a>多人游戏管理器
[多人游戏管理器](../multiplayer/multiplayer-manager.md)不再进行实验。  我们已采纳了开发人员对于使用此 API 的反馈，并让部分 API 彼此更加一致。  在进行多人游戏开发时，请将多人游戏管理器作为起点，因为该管理器提供了更简单的 API，可以为你管理多人游戏 2015 API 的很多复杂方面。

在以下部分，我们已列出了 API 的一些新功能，以及一小部分重大更改。

#### <a name="completed-events"></a>完成事件
所有 API 现在都具有相应的 ``` _competed``` 事件，不论成功还是失败，都会触发所有事件。 以前的行为是仅在对标题执行的操作失败后触发。 由于调用是批量的，这意味着完成后，标题将获得多个 ```_competed``` 事件。

| API | 返回事件 |
|-----|----------------|
| ```lobby_session()->set_local_member_properties``` |  ```local_member_property_write_completed ```
| ```lobby_session()->set_local_member_connection_address``` | ```local_member_connection_address_write_completed``` |
| ```lobby_session()->set_properties``` | ```session_property_write_completed``` |
| ```lobby_session()->set_synchronized_properties``` | ```session_synchronized_property_write_completed``` |
| ```lobby_session()->set_synchronized_host``` | ```synchronized_host_write_completed``` |

同样适用于 ```game_session()```。

#### <a name="application-defined-context"></a>应用程序定义的上下文
你现在可以传入每个 set_* 方法可选的应用程序定义的上下文，将多人游戏事件与发起调用关联。
例如

```cpp
_XSAPIIMP xbox_live_result<void> set_properties(
        _In_ const string_t& name,
        _In_ const web::json::value& valueJson,
        _In_ context_t context = nullptr
       );
```

上下文随后会通过 ```_completed``` 事件返回到标题。

#### <a name="breaking-changes"></a>重大更改

1.  两个邀请 API (```invite_friends``` & ```invite_users```) 现在是同步的。 完成后，它将返回 invite_sent 事件。

2.  ```write_synchronized_properties_and_commit``` 重命名为 ```set_synchronized_properties```。 完成后，它将返回 ```session_synchronized_property_write_completed``` 事件。

3.  ```write_synchronized_host_and_commit``` 重命名为 ```set_synchronized_host```。 完成后，它将返回 ```synchronized_host_write_completed``` 事件。

4.  开 ```lobby_session()```

  *已删除*

```cpp
_XSAPIIMP const std::unordered_map<string_t, xbox::services:: multiplayer::multiplayer_session_tournaments_server& tournaments_server() const;
```

  *已添加*

```cpp
_XSAPIIMP const std::unordered_map<string_t, xbox::services::tournaments::tournament_team_result>& tournament_team_results() const;
```

5.  开 ```game_session()```

  *已删除*

```cpp
_XSAPIIMP const std::unordered_map<string_t, xbox::services:: multiplayer::multiplayer_session_tournaments_server& tournaments_server() const;
_XSAPIIMP const std::unordered_map<string_t, xbox::services:: multiplayer::multiplayer_session_arbitration_server& arbitration_server() const;
```
  *已添加*

```cpp
_XSAPIIMP const std::unordered_map<string_t, multiplayer_session_reference>& tournament_teams() const;
_XSAPIIMP const std::unordered_map<string_t, xbox::services::tournaments::tournament_team_result>& tournament_team_results() const;
```

6.  事件类型的更改：

  *已删除*

```cpp
write_pending_changes_failed,
tournament_property_changed,
arbitration_property_changed
```

  *已重命名*

  ```write_synchronized_properties_completed``` 重命名为 ```session_synchronized_property_write_completed```

  ```write_synchronized_host_completed``` 重命名为 ```synchronized_host_write_completed```
