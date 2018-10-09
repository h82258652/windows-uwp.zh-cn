---
title: 多人游戏管理器
author: KevinAsgari
description: 了解 Xbox Live 多人游戏管理器，一种旨在更轻松地实现多人游戏的高级 API。
ms.assetid: f3a6c8bc-4f73-4b99-ac51-aadee73c8cfa
ms.author: kevinasg
ms.date: 04/04/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: bf931a2811a9a627c40a7dc45178688236437fa8
ms.sourcegitcommit: fbdc9372dea898a01c7686be54bea47125bab6c0
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/08/2018
ms.locfileid: "4428457"
---
# <a name="multiplayer-manager"></a>多人游戏管理器

Xbox Live 为将多人游戏功能到添加你的作品、允许你的作品连接全球 Xbox Live 成员提供了广泛的支持。  这包括丰富的匹配方案、玩家加入好友正在进行的游戏的功能等。 但是，通过直接使用多人游戏 2015 API 执行 Xbox Live 可能会是一项复杂的任务，它要求进行大量设计和测试，以确认你是否遵循最佳做法并满足认证要求。

通过管理会话和匹配，以及提供基于状态和事件的编程模型，多人游戏管理器可轻松地将多人游戏功能添加到你的游戏中。 它是一组旨在为你的 Xbox Live 游戏轻松执行多人游戏方案的 API。 它提供的 API 面向常见的多人游戏方案，例如，与好友进行多人游戏、处理游戏邀请、处理加入进度、匹配等。 如果你使用的是第三方匹配服务，它支持多个本地用户，并让你的作品可以更轻松地与多人游戏会话目录集成。 只需调用若干 API 即可完成其中很多方案。

## <a name="key-features"></a>关键功能
以下是多人游戏管理器 API 的主要功能：

* 轻松的会话管理和 Xbox Live 匹配
* 基于状态和事件的编程模型。
* 确保 Xbox Live 最佳做法并符合多人游戏 XR。
* 同时支持 Xbox One XDK 和 UWP 作品。
* 执行[多人游戏 2015 流程图](https://developer.xboxlive.com/en-us/platform/development/education/Documents/Xbox%20One%20Multiplayer%202015%20Developer%20Flowcharts.aspx)。
* 与传统多人游戏 2015 API 一起使用。

>**重要提示** - 你的游戏必须仍为在线多人游戏执行所需事件才能通过认证。

## <a name="overview"></a>概述
多人游戏管理器面向几个关键概念：
* `lobby_session` ：用于管理设备本地用户和你希望一起玩的受邀好友的持续性对话。 小组可能会玩多个游戏回合、地图、级别等，大厅会话会跟踪好友的核心组（包括设备的本地玩家）。 通常，会在主机可能正在浏览菜单，与组成员聊天以确定他们想要运行的游戏模式时形成此组。

* `game_session` ：跟踪正在运行特定游戏实例的玩家。 例如，竞赛、地图或级别。 你可以通过将成员包含在大厅会话中的 `join_game_from_lobby` 创建新游戏会话。  当成员接受邀请时，他们会被添加到大厅和游戏会话中（如果还有空位）。 启用匹配后，可能会将其他玩家添加到游戏会话中，但不会将他们添加到大厅会话中。 这意味着，游戏结束后，大厅会话中的玩家仍然在一起，但匹配多出的玩家不在一起。

* `multiplayer_member` ：表示在本地或远程设备上登录的个人用户。

* `do_work` ：确保在游戏和 Xbox Live 多人游戏服务之间保持正确的游戏状态更新。 若要确保最佳性能，必须频繁调用 do_work()，例如，每帧一次。 它为你提供了需要游戏处理的 `multiplayer_event` 回调事件列表。

## <a name="state-machine"></a>状态机
需要 `do_work()` 来确保保持最新状态。  为了让多人游戏管理器起作用，开发人员必须定期调用 `do_work()` 方法。 执行此操作的最可靠的方法是至少每帧调用一次。 `do_work()` 当它不工作时会快速返回，因此没必要担心调用过于频繁。

不应认为多人游戏管理器 API 返回的所有对象都是线程安全的。 但是，如果你从多个线程中调用它，它会为你提供控制权以执行线程同步。 库拥有内部多线程保护，但是，如果你需要一个线程访问任意值（例如，当另一个线程可能在调用 `do_work()` 时浏览 members() 列表），则仍需要执行自身锁定。

当玩家加入、离开或在更新会话时，多人游戏管理器会保持基于状态的模型一直更新后台中的会话。 为了帮助避免 UI 线程和游戏线程之间的线程同步问题，多人游戏管理器不会更新会话的应用可见状态，直到调用 `do_work()` 方法。 传统上来讲，你会收到有关后台线程上的会话更改等事件的通知，然后，必须将其与 UI 线程同步才能显示这些更改。 通过多人游戏管理器，可以在后台完成此项工作。  你可以在选择的同时，在主线程上调用 `do_work()`，以获取多人游戏管理器在后台为你缓冲的最新状态快照。

## <a name="events-and-notifications"></a>事件和通知
多人游戏管理器定义了一组重要事件（参阅 `multiplayer_event_type`），并在事件发生时通过 `do_work()` 方法通知游戏。 远程玩家加入或离开、成员属性更改或会话状态更改等事件。 所有多人游戏管理器 API 都是同步的。 完成这些异步操作后，`do_work()` 方法会返回事件列表。 你的作品应按你认为合适的方式处理这些事件。 请参阅 `multiplayer_event` 类文档获取更多详细信息。

对于每个事件，根据事件类型，你必须将 `event_args` 转换为相应的事件参数类以获取事件属性。 以下示例演示了如何使用 `do_work()` 来处理事件：

```cpp
auto eventQueue = mpInstance.do_work();
for (auto& event : eventQueue)
{
    switch (event.event_type())
    {
      case multiplayer_event_type::member_joined:
      {
        auto memberJoinedArgs =  std::dynamic_pointer_cast<member_joined_event_args>(event.event_args());
        HandleMemberJoined(memberJoinedArgs);
        break;
      }
      case multiplayer_event_type::session_property_changed:
      {
        auto sessionPropertyChangedArgs =  std::dynamic_pointer_cast<session_property_changed_event_args>(event.event_args());
        HandlePropertiesChanged(sessionPropertyChangedArgs);
        break;
      }
      . . .
    }
}

```

## <a name="scenarios"></a>方案

在此部分中，我们将介绍一些常见方案和你会在各方案中调用的 API。  还会提供一些有关多人游戏管理器在后台运行的内容的信息。

* [与好友一起玩](multiplayer-manager/play-multiplayer-with-friends.md)
* [查找匹配](multiplayer-manager/play-multiplayer-with-matchmaking.md)
* [发送游戏邀请](multiplayer-manager/send-game-invites.md)
* [处理协议激活](multiplayer-manager/handle-protocol-activation.md)

可在[多人游戏管理器 API 概述](multiplayer-manager/multiplayer-manager-api-overview.md)中查找 API 的高级概述。

## <a name="what-multiplayer-manager-does-not-do"></a>多人游戏管理器不执行的操作
虽然多人游戏管理器可更轻松地执行多人游戏方案并从开发人员处提取一些数据，但是，还是有一些它无法处理或不是最合适它处理的事项。

* 永久在线服务器游戏（例如 MMO）或其他要求大型会话（会话中的玩家超过 100 个）的游戏类型。
* 服务器到服务器会话管理

>多人游戏管理器不会与任何特定网络技术绑定，并应该与所有网络层一起使用。

## <a name="next-steps"></a>后续步骤

请参阅 C++ 或 WinRT *多人游戏*示例获取此 API 的使用示例。

可在 Microsoft::Xbox::Services::Multiplayer::Manager 命名空间中的 C++ 或 WinRT 指南中查找 API 文档。  你还可以参阅 `multiplayer_manager.h` 标题。

如果你有任何疑问、 反馈或遇到任何使用多人游戏管理器的问题，请联系你的 DAM，或在的论坛上发布支持线程[https://forums.xboxlive.com](https://forums.xboxlive.com)。
