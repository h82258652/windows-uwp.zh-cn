---
title: 与好友进行多人游戏
author: KevinAsgari
description: 了解如何使用 Xbox Live 多人游戏管理器让玩家与好友进行多人游戏。
ms.assetid: 6eefee0e-6c0d-473a-97e7-f3e45f574712
ms.author: kevinasg
ms.date: 04/04/2017
ms.topic: article
keywords: Xbox live, xbox, 游戏, uwp, windows 10, xbox one, 多人游戏, 多人游戏管理器, 流程图
ms.localizationpriority: medium
ms.openlocfilehash: 8af1979a92821a62a625ea9d637a7dbcf9a59edb
ms.sourcegitcommit: f2c9a050a9137a473f28b613968d5782866142c6
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/10/2018
ms.locfileid: "6271312"
---
# <a name="play-a-multiplayer-game-with-friends"></a>与好友进行多人游戏

更为简单的多人游戏方案之一是允许玩家在线与好友玩游戏。 该方案介绍了需要通过使用多人游戏管理器执行的基本步骤。

## <a name="inviting-friends"></a>邀请好友

使用多人游戏管理器向用户的好友发送邀请，并使该好友加入正在进行的游戏时，涉及四个步骤：

1. [初始化多人游戏管理器](#initialize-multiplayer-manager)
2. [通过添加本地用户创建大厅会话](#create-lobby)
3. [向好友发送邀请](#send-invites)
4. [接受邀请](#accept-invites)
5. [加入来自大厅的游戏会话](#join-game)

在执行邀请的设备上完成步骤 1、2、3 和 5。  通常，会在通过协议激活启动应用后，在被邀请者的计算机上初始化步骤 4。

你可以在此处查看过程流程图：[流程图 - 与好友进行多人/合作游戏](mpm-flowcharts/mpm-play-with-friends.md)。

### <a name="1-initialize-multiplayer-manager-a-nameinitialize-multiplayer-manager"></a>1) 初始化多人游戏管理器 <a name="initialize-multiplayer-manager">

| 调用 | 触发的事件 |
|-----|----------------|
| `multiplayer_manager::initialize(lobbySessionTemplateName)` | 不适用 |

假设已指定有效的会话模板名称（在服务配置中配置），初始化多人游戏管理器后，将自动创建大厅会话对象。 请注意，此操作不会在服务上创建大厅会话实例。

**示例：**

```cpp
auto mpInstance = multiplayer_manager::get_singleton_instance();

mpInstance->initialize(lobbySessionTemplateName);
```


### <a name="2-create-the-lobby-session-by-adding-local-usersa-namecreate-lobby"></a>2) 通过添加本地用户创建大厅会话<a name="create-lobby">

| 方法 | 触发的事件 |
|-----|----------------|
| `multiplayer_lobby_session::add_local_user()` | `user_added_event` |
| `multiplayer_lobby_session::set_local_member_connection_address()` | `local_member_connection_address_write_completed ` |
| `multiplayer_lobby_session::set_local_member_properties()` | `member_property_changed` |

此时，将本地登录 Xbox Live 的用户添加到大厅会话。 此操作会在添加第一个用户时托管新大厅。 对于所有其他用户，他们会被添加到当前大厅作为二级用户。 此 API 还将播发 shell 中的大厅，以便好友加入。 添加本地用户后，你可以发送邀请、设置大厅属性并仅通过 lobby() 访问大厅成员。

加入大厅后，Microsoft 建议设置本地成员的连接地址，以及成员的所有自定义属性。

你必须为本地登录的所有用户重复此过程。

**示例：（单个本地用户）**

```cpp
auto mpInstance = multiplayer_manager::get_singleton_instance();

auto result = mpInstance->lobby_session()->add_local_user(xboxLiveContext->user());
if (result.err())
{
  // handle error
}

// Set member connection address
string_t connectionAddress = L"1.1.1.1";
mpInstance->lobby_session()->set_local_member_connection_address(
    xboxLiveContext->user(),
    connectionAddress);

// Set custom member properties
mpInstance->lobby_session()->set_local_member_properties(xboxLivecontext->user(), ..., ...)
```

**示例：（多个本地用户）**

```cpp
auto mpInstance = multiplayer_manager::get_singleton_instance();
string_t connectionAddress = L"1.1.1.1";

for (User^ user : User::Users)
{
  if( user->IsSignedIn )
  {
    auto result = mpInstance.lobby_session()->add_local_user(user);
    if (result.err())
    {
      // handle error
    }

    // Set member connection address
    mpInstance->lobby_session()->set_local_member_connection_address(
        xboxLiveContext->user(), connectionAddress);

    // Set custom member properties
    mpInstance->lobby_session()->set_local_member_properties(xboxLivecontext->user(), ..., ...)
  }
}

```


在下一个 `do_work()` 调用上对这些更改进行批处理。  
每当将用户添加到大厅会话时，多人游戏管理器都会引发 `user_added` 事件。 建议你检查事件的错误代码，以查看是否已成功添加用户。 如果发生故障，会提供错误消息详细说明故障的原因。

**多人游戏管理器执行的功能**

* 使用 Xbox Live 多人游戏服务注册实时活动和多人游戏订阅
* 创建大厅会话
* 加入处于活动状态的所有本地玩家
* 上载 SDA
* 设置成员属性
* 注册会话更改事件
* 将“大厅会话”设置为“活动会话”

### <a name="3-send-invites-to-friends-a-namesend-invites"></a>3) 向好友发送邀请 <a name="send-invites">

| 方法 | 触发的事件 |
| -----|----------------|
| `multiplayer_lobby_session::invite_friends()` | `invite_sent` |
| `multiplayer_lobby_session::invite_users()` | `invite_sent` |

下一步，你会想要引入用于邀请好友的标准 Xbox UI。 这会显示允许玩家选择好友或最近的玩家以进行游戏邀请的 UI。 玩家点击确认后，多人游戏管理器会向选择的玩家发送邀请。

游戏还可使用 `invite_users()` 方法向一组由其 Xbox Live 用户 Id 定义的用户发送邀请。 如果你更想要使用自己的游戏内 UI，而不是库存 Xbox UI，该方法十分有用。

**示例：**

```cpp
auto result = mpInstance->lobby_session()->invite_friends(xboxLiveContext);
if (result.err())
{
  // handle error
}
```

**多人游戏管理器执行的功能**

* 引入 Xbox 库存游戏可调用 UI (TCUI)
* 直接向选择的玩家发送邀请

### <a name="4-accept-invites-a-nameaccept-invites"></a>4) 接受邀请 <a name="accept-invites">

| 方法 | 触发的事件 |
| -----|----------------|
| `multiplayer_manager::join_lobby(Windows::ApplicationModel::Activation::IProtocolActivatedEventArgs^ args)` | `join_lobby_completed_event` |
| `multiplayer_lobby_session::set_local_member_connection_address()` | `local_member_connection_address_write_completed ` |
| `multiplayer_lobby_session::set_local_member_properties()` | `member_property_changed` |

当邀请的玩家接受游戏邀请或已通过 shell UI 加入好友游戏时，使用协议激活在其设备上启动游戏。 游戏开始后，多人游戏管理器可以使用协议激活的事件参数加入大厅。 或者，如果你尚未通过 lobby_session()::add_local_user() 添加本地用户，则可以通过 join_lobby() API 在用户列表中进行传递。 如果未通过 lobby_session()::add_local_user() 或 join_lobby() 添加邀请的用户，则 join_lobby() 将失败，并提供向其发送邀请的 invited_xbox_user_id() 作为 join_lobby_completed_event_args() 的一部分。

加入大厅后，Microsoft 建议设置本地成员的连接地址，以及成员的所有自定义属性。 如果主机不存在，你还可以通过 set_synchronized_host 设置一个。

最后，如果游戏已经在进行中并且还可以容纳被邀请者，则多人游戏管理器会自动将用户加入游戏会话。 将通过提供相应的错误代码和消息的 join_game_completed 事件通知此游戏。

**示例：**

```cpp
auto result = mpInstance().join_lobby(IProtocolActivatedEventArgs^ args);
if (result.err())
{
  // handle error
}

string_t connectionAddress = L"1.1.1.1";
mpInstance->lobby_session()->set_local_member_connection_address(
    xboxLiveContext->user(), connectionAddress);
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

### <a name="5-join-a-game-session-from-the-lobby-a-namejoin-game"></a>5) 加入来自大厅的游戏会话 <a name="join-game">

| 方法 | 触发的事件 |
|-----|----------------|
| `multiplayer_manager::join_game_from_lobby()` | `join_game_completed_event` |

在接受邀请，且主机已准备好开始游戏后，你可以通过调用 `join_game_from_lobby()` 启动包括大厅会话成员的新游戏。

**示例：**

```cpp
auto result = mpInstance.join_game_from_lobby();
if (result.err())
{
  // handle error
}
```

通过 `join_game_completed` 事件处理错误/成功

**多人游戏管理器执行的功能**

* 创建游戏会话
 * 加入处于活动状态的所有本地玩家
 * 上载 SDA
 * 设置成员属性
* 注册会话更改事件
* 通过大厅会话播发游戏
