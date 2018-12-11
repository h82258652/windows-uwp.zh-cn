---
title: 多人游戏管理器 API 概述
description: 了解 Xbox Live 多人游戏管理器 API。
ms.assetid: 658babf5-d43e-4f5d-a5c5-18c08fe69a66
ms.date: 04/04/2017
ms.topic: article
keywords: Xbox live, xbox, 游戏, uwp, windows 10, xbox one, 多人游戏, 多人游戏管理器
ms.localizationpriority: medium
ms.openlocfilehash: 7838de6845bc6c49acf649c0e859cd0d7020490f
ms.sourcegitcommit: 8921a9cc0dd3e5665345ae8eca7ab7aeb83ccc6f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/11/2018
ms.locfileid: "8892645"
---
# <a name="multiplayer-manager-api-overview"></a>多人游戏管理器 API 概述

本页介绍了多人游戏管理器 API 的广泛概述以及它们在游戏中的使用方法。 它会展示 API 中最重要的类和方法。 有关详细的 API 信息，请参阅参考文档。 有关如何在应用程序中使用这些 API 的示例，请参阅多人游戏示例。

## <a name="namespace"></a>命名空间
多人游戏管理器类包含在下列命名空间中：

| 语言 | 命名空间 |
| --- | --- |
| C++/CX | Microsoft::Xbox::Services::Multiplayer::Manager |
| C++ | xbox::services::multiplayer::manager |

以下列表描述了你应该了解的主要类：

* [多人游戏管理器类](#multiplayer-manager-class)
* [多人游戏事件类](#multiplayer-event-class)
* [多人游戏成员类](#multiplayer-member-class)
* [多人游戏大厅会话类](#multiplayer-lobby-session-class)
* [多人游戏会话类](#multiplayer-game-session-class)

## <a name="multiplayer-manager-class-a-namemultiplayer-manager-class"></a>多人游戏管理器类 <a name="multiplayer-manager-class">

| 语言 | 类 |
| --- | --- |
| C++/CX | MultiplayerManager |
| C++ | multiplayer_manager |

此类是与多人游戏管理器交互的主类。 它是一个单一实例类，因此仅需创建和初始化此类一次。
此类包含单个大厅会话对象和单个游戏会话对象。

你至少必须在此类上调用 `initialize()` 和 `do_work()` 才能使多人游戏管理器运行。

下表介绍了一些（但不是全部）适用于此类的较常用的方法和属性。 有关类成员的完整描述列表，请参阅参考。

| C++/CX | C++ | 描述 |
| --- | --- | --- |
| **方法** | | |
| Initialize() | initialize() | 初始化多人游戏管理器。 你必须在使用多人游戏管理器之前调用此方法。 |
| DoWork() | do_work() | 更新应用可见会话状态。 你至少应该每帧调用一次此方法，且你的游戏应处理此方法返回的多人游戏事件。 |
| JoinLobby() | join_lobby() | 通过唯一标识你要加入的大厅的 handleId，或当用户接受导致作品激活协议的邀请时，可为你提供加入好友的大厅会话的方法。 |
| JoinGameFromLobby() | join_game_from_lobby() | 如果存在一个大厅的游戏会话且其中有空位，则加入此大厅的游戏会话。 如果会话不存在，它会使用当前大厅成员创建新游戏会话。 这不会将当前大厅会话属性迁移到游戏会话。 加入后，你可以通过 `game_session()::set_synchronized_*` API 设置属性或主机。 游戏必须在想要加入游戏会话的所有客户端上调用此 API。|
| JoinGame() | join_game() | 加入带有指定的全局唯一会话名称的现有游戏会话（通常通过第三方匹配服务查找）。 你可以在要将其作为游戏的一部分的 xbox 用户 ID 列表中传递。|
| FindMatch() | find_match() | 使用 Xbox Live 匹配查找并加入游戏。 |
| LeaveGame() | leave_game() | 离开游戏，并将此成员和所有本地成员返回到大厅。 |
| **属性** | | |
| LobbySession | lobby_session() | 表示大厅会话的对象的句柄。 |
| GameSession |  game_session() | 表示游戏会话的对象句柄。 |

## <a name="multiplayer-event-class-a-namemultiplayer-event-class"></a>多人游戏事件类 <a name="multiplayer-event-class">

| 语言 | 类 |
| --- | --- |
| C++/CX | MultiplayerEvent |
| C++ | multiplayer_event |

当你调用 `do_work()` 时，多人游戏管理器会返回一个事件列表，此列表表示自上次调用 `do_work()` 后会话发生的更改。 这些事件包括会员加入会话、成员离开会话、成员属性更改、主机客户端更改等更改。

有关所有可能的事件类型列表，请参阅 `multiplayer_event_type` 枚举。

每个返回的 `multiplayer_event` 都包括一个 `event_args`，你必须将其转换为适用于事件类型的相应 event_args 类。 例如，如果 `event_type` 为 `member_joined`，则你要将 `event_args` 引用转换为 `member_joined_event_args` 类。

调用 `do_work()` 后，你的游戏应根据需要处理每个事件。

## <a name="multiplayer-member-class-a-namemultiplayer-member-class"></a>多人游戏成员类 <a name="multiplayer-member-class">

| 语言 | 类 |
| --- | --- |
| C++/CX | MultiplayerMember |
| C++ | multiplayer_member |

此类表示大厅或游戏会话中的玩家。 此类包含有关成员的属性，包括玩家的 XboxUserID、玩家的网络连接地址、每个玩家的自定义属性等。

## <a name="multiplayer-lobby-session-class-a-namemultiplayer-lobby-session-class"></a>多人游戏大厅会话类 <a name="multiplayer-lobby-session-class">

| 语言 | 类 |
| --- | --- |
| C++/CX | MultiplayerLobbySession |
| C++ | multiplayer_lobby_session |

用于管理设备本地用户和你希望一起玩的受邀好友的持续性对话。 大厅会话必须至少包含一个成员，以便多人游戏管理器可以执行任意多人游戏操作。 你可以通过调用 `add_local_user()` 方法初步创建新大厅会话。

下表介绍了一些（但不是全部）适用于此类的较常用的方法和属性。 有关类成员的完整描述列表，请参阅参考。

| C++/CX | C++ | 描述 |
| --- | --- | --- |
| **方法** | | |
| AddLocalUser() | add_local_user() | 将本地用户（已在本地设备上登录的玩家）添加到大厅会话。 如果这是添加到大厅会话的第一个成员，则它将创建新大厅会话。 |
| RemoveLocalUser() | remove_local_user() | 从大厅和游戏会话删除指定成员。 |
| InviteFriends() | invite_friends() | 打开允许玩家从好友列表选择用户的标准 Xbox Live UI，然后邀请这些玩家加入游戏。 |
| InviteUsers() | invite_users() | 邀请指定 Xbox Live 用户加入游戏。 |
| SetLocalMemberConnectionAddress() | set_local_member_connection_address() | 设置本地成员的网络地址。 游戏可以使用此网络地址在成员之间建立网络通信。 |
| SetLocalMemberProperties() | set_local_member_properties() | 设置本地成员的自定义属性。 此属性存储在 JSON 字符串中。 |
| DeleteLocalMemberProperties() | delete_local_member_properties() | 删除本地成员的自定义属性。 |
| SetProperties() / SetSynchronizedProperties() | set_properties() / set_synchronized_properties() | 设置大厅会话的自定义属性。 此属性存储在 JSON 字符串中。 如果属性在设备之间共享，并在同一时间由多个设备更新，请使用此方法的同步版本。 |
| IsHost() | is_host() | 指示当前设备是否可以充当大厅主机。 |
| SetSynchronizedHost() | set_synchronized_host() | 设置大厅的主机。 |
| **属性** | | |
| LocalMembers | local_members() | 在本地设备上登录的成员集合。 |
| 成员 | members() | 大厅会话中的成员集合。 |
| 属性 | properties() | 表示大厅会话的属性集合的 JSON 对象。 |
| 主机 | host() | 大厅的主持人。 |


## <a name="multiplayer-game-session-class-a-namemultiplayer-game-session-class"></a>多人游戏会话类 <a name="multiplayer-game-session-class">

| 语言 | 类 |
| --- | --- |
| C++/CX | MultiplayerGameSession |
| C++ | multiplayer_game_session |

游戏会话表示正在参加实际游戏实例的 Xbox Live 成员组。 这可能会包括已通过匹配服务匹配的玩家。

若要开始包括来自 `lobby_session` 的成员的新游戏会话，你可以调用 `multiplayer_manager::join_game_from_lobby()`。 如果要使用 Xbox Live 匹配，你可以调用 `multiplayer_manager::find_match()`。 如果要使用第三方匹配服务，你可以调用 `multiplayer_manager::join_game()`。

下表介绍了一些（但不是全部）适用于此类的较常用的方法和属性。 有关类成员的完整描述列表，请参阅参考。

| C++/CX | C++ | 描述 |
| --- | --- | --- |
| **方法** | | |
| SetProperties() / SetSynchronizedProperties() | set_properties() / set_synchronized_properties() | 设置游戏会话的自定义属性。 此属性存储在 JSON 字符串中。 如果属性在设备之间共享，并在同一时间由多个设备更新，请使用此方法的同步版本。 |
| IsHost() | is_host() | 指示当前设备是否可以充当游戏主机。 |
| SetSynchronizedHost() | set_synchronized_host() | 设置游戏的主机。 |
| **属性** | | |
| 成员 | members() | 游戏会话中的成员集合。 |
| 属性 | properties() | 表示游戏会话的属性集合的 JSON 对象。 |
| 主机 | host() | 游戏的主持人。 |
