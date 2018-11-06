---
title: 处理协议激活
author: KevinAsgari
description: 了解如何使用 Xbox Live 多人游戏管理器处理协议激活。
ms.assetid: e514bcb8-4302-4eeb-8c5b-176e23f3929f
ms.author: kevinasg
ms.date: 04/04/2017
ms.topic: article
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one, 多人游戏管理器, 协议激活
ms.localizationpriority: medium
ms.openlocfilehash: 9046686a9db1428935e834e28f02269d710cd8ad
ms.sourcegitcommit: e814a13978f33654d8e995584f4b047cb53e0aef
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/06/2018
ms.locfileid: "6032548"
---
# <a name="handle-protocol-activation"></a>处理协议激活

当系统自动开始游戏响应其他操作时执行协议激活（通常是在一名玩家接受另一名玩家的游戏邀请时）。

你的作品可通过以下方式激活协议：

* 当用户接受游戏邀请时
* 当用户从玩家的玩家卡中选择“加入游戏”时。

此场景涵盖了如何在发布游戏和加入大厅以及游戏（如果存在）时处理协议激活。

你可以在此处查看过程流程图：[流程图 - 处理协议激活玩家](mpm-flowcharts/mpm-on-protocol-activation.md)。

| 方法 | 触发的事件 |
| -----|----------------|
| `multiplayer_manager::join_lobby(IProtocolActivatedEventArgs^ args, User^)` | `join_lobby_completed_event` |
| `multiplayer_lobby_session::set_local_member_connection_address()` | `local_member_connection_address_write_completed ` |
| `multiplayer_lobby_session::set_local_member_properties()` | `member_property_changed` |

当玩家通过玩家的玩家卡接受游戏邀请或加入好友的游戏时，使用协议激活在其设备上启动游戏。 游戏开始后，多人游戏管理器可以使用协议激活的事件参数加入大厅。 或者，如果你尚未通过 `lobby_session()::add_local_user()` 添加本地用户，则可以通过 `join_lobby()` API 在用户列表中进行传递。 如果未添加邀请的用户，或者如果邀请的是其他用户而不是添加的用户，则 `join_lobby()` 将失败并提供向其发送邀请的 `invited_xbox_user_id()` 作为 `join_lobby_completed_event_args` 的一部分。

加入大厅后，Microsoft 建议设置本地成员的连接地址，以及成员的所有自定义属性。 如果主机不存在的话，你还可以通过 `set_synchronized_host` 设置一个。

最后，如果游戏已经在进行中并且还可以容纳被邀请者，则多人游戏管理器会自动将用户加入游戏会话。 将通过提供相应的错误代码和消息的 `join_game_completed` 事件通知此游戏。

**示例：**

```cpp
auto result = mpInstance().join_lobby(IProtocolActivatedEventArgs^ args, users);
if (result.err())
{
  // handle error
}

string_t connectionAddress = L"1.1.1.1";
mpInstance->lobby_session()->set_local_member_connection_address(
    xboxLiveContext->user(),
    connectionAddress);
```

通过 `join_lobby_completed` 事件处理错误/成功

**多人游戏管理器执行的功能**

* 注册 RTA 和多人游戏订阅
* 加入大厅会话
 * 当前大厅状态清理
 * 加入处于活动状态的所有本地玩家
 * 上载 SDA
 * 设置成员属性
* 注册会话更改事件
* 将“大厅会话”设置为“活动会话”
* 加入游戏会话（如果存在）
 * 使用转移句柄
