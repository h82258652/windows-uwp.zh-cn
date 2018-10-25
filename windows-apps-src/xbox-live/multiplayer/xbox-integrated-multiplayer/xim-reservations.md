---
title: XIM 带外保留
author: KevinAsgari
description: 描述如何通过带外保留将 Xbox 集成多人游戏 (XIM) 用作专用聊天解决方案。
ms.assetid: 0ed26d19-defb-414d-a414-c4877bd0ed37
ms.author: kevinasg
ms.date: 01/28/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one, xbox 集成多人游戏, xim, 聊天
ms.localizationpriority: medium
ms.openlocfilehash: aeae8316aae29c548c1f2e0463ae1db916a259e3
ms.sourcegitcommit: 82c3fc0b06ad490c3456ad18180a6b23ecd9c1a7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/24/2018
ms.locfileid: "5468174"
---
# <a name="using-xim-as-a-dedicated-chat-solution-via-out-of-band-reservations"></a>通过带外保留将 XIM 用作专门的聊天解决方案

大多数应用会使用 XIM 来处理聚集玩家的每个方面。 毕竟，专注于组装支持端到端常见多人游戏场景所需的功能，这就是称其为“Xbox 集成多人游戏”的原因。 但是，一些使用专用 Internet 服务器实现其自身多人游戏解决方案的应用也希望拥有建立可靠、低延迟、低成本对等聊天通信的优势。 XIM 认识到了这一需要，并且支持扩展模式，此模式利用 XIM 的简化对等通信，以增强 XIM API 之外的外部玩家管理。 玩家不是通过社交方式或匹配移入 XIM 网络，而是使用“保留”进行移动，通过应用的外部玩家集合机制在“带外”交换适用于特定用户的保证占位符。

除了移动过程，使用带外保留托管的 XIM 网络和任何其他 XIM 网络一样有效。 所有通信功能作用相同。 但是，使用带外保留托管的 XIM 网络必须禁用匹配和社交发现 API 方法，因为这两种方法会与应用自身的外部实施发生冲突。 例如，你无法从此类 XIM 网络发送邀请。

>已优化 XIM 以提供简单的端到端解决方案。 因此，并非所有复杂的拓扑或场景会完全适合带外保留。 如果对是否需要或如何利用 XIM 的通信功能存有疑问，请联系你的 Microsoft 代表。

后续主题将描述如何在 XIM 中利用带外保留。 由于与之前部分中所述的“标准”XIM 使用大致相同，某些讨论已略过。 建议熟悉[使用 XIM](using-xim.md)。

主题：

1. [移至新的带外保留 XIM 网络](#moving)
1. [添加玩家至使用带外保留托管的 XIM 网络](#adding)
1. [在使用带外保留托管的 XIM 网络中配置聊天目标](#targets)
1. [将玩家从使用带外保留托管的 XIM 网络中移除](#remove)
1. [清理使用带外保留托管的 XIM 网络](#clean)

## <a name="moving-to-a-new-out-of-band-reservation-xim-network"></a>移至新的带外保留 XIM 网络 

若要开始使用带外保留，你聚集的某个参与者必须移至在此模式中创建的新 XIM 网络。 选择哪一个参与的对等设备由你决定。 你可能已经对游戏主机或服务器有了概念，这是启动进程的自然选择，但这不是必需的。 我们建议选择报告“开放式”网络访问类型的设备，以实现最快的连接设置时间。 有关详细信息，请参阅 `Windows::Networking::XboxLive` 平台文档。

通过初始化 XIM 并声明预期本地 Xbox 用户 ID（如标准 XIM 使用操作实例中所示，但不是调用类似 `xim::move_to_new_network()` 的方法，而是用空保留字符串调用 `xim::move_to_network_using_out_of_band_reservation()`），实现向通过带外保留托管的 XIM 网络的移动。 例如：

```cpp
 xim::singleton_instance().initialize(myServiceConfigurationId, myTitleId);
 xim::singleton_instance().set_intended_local_xbox_user_ids(1, &myXuid);
 xim::singleton_instance().move_to_network_using_out_of_band_reservation(nullptr);
```

然后，将在处理典型 `xim::start_processing_state_changes()` 和 `xim::finish_processing_state_changes()` 循环中的状态更改的时间段内提供标准 `xim_move_to_network_starting_state_change`、`xim_player_joined_state_change` 和 `xim_move_to_network_succeeded_state_change`。 例如：

```cpp
 uint32_t stateChangeCount;
 xim_state_change_array stateChanges;
 xim::singleton_instance().start_processing_state_changes(&stateChangeCount, &stateChanges);
 for (uint32_t stateChangeIndex = 0; stateChangeIndex < stateChangeCount; stateChangeIndex++)
 {
     const xim_state_change * stateChange = stateChanges[stateChangeIndex];
     switch (stateChange->state_change_type)
     {
         case xim_state_change_type::player_joined:
         {
             MyHandlePlayerJoined(static_cast<const xim_player_joined_state_change *>(stateChange));
             break;
         }

         case xim_state_change_type::player_left:
         {
             MyHandlePlayerLeft(static_cast<const xim_player_left_state_change *>(stateChange));*
             break;
         }

         ...
     }
 }
 xim::singleton_instance().finish_processing_state_changes(stateChanges);
```

在初始设备处理状态更改并将玩家成功移入 XIM 网络后，它必须为其他用户创建保留。

## <a name="adding-players-to-a-xim-network-managed-using-out-of-band-reservations"></a>添加玩家至使用带外保留托管的 XIM 网络 

使用带外保留托管的 XIM 网络会始终报告 `xim::allowed_player_joins()` 方法中的 `xim_allowed_player_joins::out_of_band_reservation` 值；除了通过调用 `xim::create_out_of_band_reservation()` 具有保留位置的 Xbox 用户 ID 以外的玩家，该网络将对所有其他玩家关闭。 `xim::create_out_of_band_reservation()` 获取了一组用户，因此你可以一次性或在一段时间后为在外部聚集的玩家创建此类保留。 此外，已忽略让玩家加入 XIM 网络的用户，因此，你还可以提供其他 Xbox 用户 ID 作为完整的替换集或增量更改（以操作更方便者为准）。 下面的示例假设你已经将完全收集的 Xbox 用户 ID 字符串指针集合赋予具有“xboxUserIdCount”数字元素的数组变量“xboxUserIds”：

```cpp
 xim::singleton_instance().create_out_of_band_reservation(xboxUserIdCount, xboxUserIds);
```

这会开始为指定的 Xbox 用户 ID 创建保留的异步过程。 操作完成后，XIM 将会提供一个用于报告成功或失败的 `xim_create_out_of_band_reservation_completed_state_change`。 如果成功，将会为你的系统创建一个保留字符串，以便提供给为操作提供的那些 Xbox 用户 ID。 成功创建的保留字符串仅在一定时间内有效。 该时间在 `xim_create_out_of_band_reservation_completed_state_change` 内返回。

有了有效的保留字符串后，可以使用用于聚集 XIM 外部玩家的“带外”外部机制将该字符串分配给枚举的玩家。 例如，如果你正在使用多人游戏会话目录 (MPSD) Xbox Live 服务，可将此字符串作为会话文件中的自定义属性写入（注意：该保留字符串将始终仅包含一组受限字符，此类字符可在 JSON 内安全使用，而无需转义或 Base64 编码）。

一旦其他用户都拥有各自的保留字符串，他们便可以将其用作 `xim::move_to_network_using_out_of_band_reservation()` 的参数，准备开始移动至 XIM 网络。 以下示例假设已将保留字符串提取到名为“reservationString”的变量。

```cpp
xim::singleton_instance().move_to_network_using_out_of_band_reservation(reservationString);

```

正如为保留字符串指定空指针的初始设备那样，移动操作异步执行。 状态更改 `xim_move_to_network_starting_state_change`、`xim_player_joined_state_change` 和 `xim_move_to_network_succeeded_state_change` 将会由移动生成。 移动成功后，将会添加本地和远程玩家。 将会为 XIM 网络上的现有设备提供这些新玩家的 `xim_player_joined_state_change`。 此时，在该 XIM 网络中（隐私和策略允许时），这些不同设备上的玩家之间将自动启用语音和文本聊天通信。

如果移动操作由于保留字符串无效而失败，XIM 将返回 `xim_network_exited_state_change`，并显示原因 `xim_network_exit_reason::invalid_reservation`。 发生这种情况可能有多种原因：

1. 在主机设备上返回 `xim_create_out_of_band_reservation_completed_state_change` 之前，作品尝试在远程设备上调用 `xim::move_to_network_using_out_of_band_reservation()`
1. 保留到期后，作品尝试在远程设备上调用 `xim::move_to_network_using_out_of_band_reservation()`。
1. 作品尝试在远程设备上调用 `xim::move_to_network_using_out_of_band_reservation()` 多次，而只调用 `xim::create_out_of_band_reservation()` 一次。

无论成功还是失败，使用保留字符串的移动操作都将消耗保留字符串。 因此，每次使用尝试后，主机应通过再次调用 `xim::create_out_of_band_reservation()` 生成一个新的保留字符串。

## <a name="configuring-chat-targets-in-a-xim-network-managed-using-out-of-band-reservations"></a>在使用带外保留托管的 XIM 网络中配置聊天目标

在聊天通信上，使用带外保留托管的 XIM 网络与传统的 XIM 网络行为一致。 为支持竞争场景，将会自动配置新建的 XIM 网络，仅支持与身为同一团队成员的其他玩家聊天；也就是说，`xim::chat_targets()` 报告的配置值默认为 `xim_chat_targets::same_team_index_only`。 通过调用 `xim::set_chat_targets()`，这可以更改为允许所有人在隐私和策略允许的地方与其他任何人交谈。 以下示例开始配置 XIM 网络中的每个人，以使用 `xim_chat_targets::all_players` 值：

```cpp
xim::singleton_instance().set_chat_targets(xim_chat_targets::all_players);
```

向所有参与者提供 `xim_chat_targets_changed_state_change` 时，会告知他们有一个有效的新目标设置。

使用相同团队索引值零，对使用带外保留托管的 XIM 网络中的每一个玩家进行初步配置。 这意味着，默认情况下，`xim_chat_targets::same_team_index_only` 的配置可能与 `xim_chat_targets::all_players` 无法区分。 但是，你可以通过调用 `xim_player::xim_local::set_team_index()` 随时更改本地玩家的团队索引。 以下示例配置了一个玩家指针“localPlayer”，以获得新团队索引值 1：

```cpp
 localPlayer->local()->set_team_index(1);
```

为所有设备提供此玩家的 xim_player_team_index_changed_state_change 时，会告知其玩家有一个有效的新团队索引值。 如果聊天目标配置当前为 xim_chat_targets::same_team_index_only，则具有相同新团队索引的其他玩家将开始从更改玩家收听语音和接收文本聊天（隐私和策略允许），反之亦然。 具有旧团队索引的玩家将停止交换此类聊天通信。 如果聊天目标配置当前为 xim_chat_targets::all_players，则团队索引将不会对谁跟谁聊天产生影响。

## <a name="removing-players-from-a-xim-network-managed-using-out-of-band-reservations"></a>将玩家从使用带外保留托管的 XIM 网络中移除 

使用带外保留时，是从外部管理玩家名单，因此自然可能需要将玩家从 XIM 网络中移除。 典型的方法是：应用利用最初用于创建 XIM 网络和后续保留的同一游戏主机来管理玩家移除，并通过调用 `xim::kick_player()` 执行此操作。 这会从 XIM 网络移除相同设备上的指定玩家和所有玩家。 以下示例假设应用已确定其想要移除由“playerToRemove”变量指向的 `xim_player` 对象：

```cpp
xim::singleton_instance().kick_player(playerToRemove);
```

向相应的玩家（或玩家们）提供每一位玩家所必须的任何 `xim_player_left_state_change`，以及指示他们已被踢出网络的 `xim_network_exited_state_change`。 或者，每位玩家可调用 `xim::leave_network()` 自行退出以达到相同效果。

请注意，当玩家由于环境困难离开 XIM 网络时，XIM 对等通信可随时自行决定。 应用应准备好处理“未经请求的”`xim_player_left_state_change`，并以适用于特定应用的方式协调 XIM 的状态和外部玩家管理架构之间的任何差异。

## <a name="cleaning-up-a-xim-network-managed-using-out-of-band-reservations"></a>清理使用带外保留托管的 XIM 网络 

任何尚未从 XIM 网络中踢出并想要返回就像已经调用了 xim::initialize() 和 `xim::set_intended_local_xbox_user_ids()` 状态的玩家，可以使用 `xim::leave_network()` 方法开始执行此操作：

```cpp
xim::singleton_instance().leave_network();
```

这会开始以异步方式断开与其他参与者的连接。 这将导致向远程设备提供本地玩家的 `xim_player_left_state_change`，并将向本地设备提供各个玩家的 `xim_player_left_state_change`（本地或远程）。 所有正常断开连接结束后，将提供最终的 `xim_network_exited_state_change`。 然后，该应用可调用 `xim::cleanup()`，以释放所有资源并返回到未初始化状态：

```cpp
 xim::singleton_instance().cleanup();
```

当尚未提供 `xim_network_exited_state_change` 时，始终强烈建议调用 `xim::leave_network()` 并等待 `xim_network_exited_state_change`，以便顺利退出 XIM 网络。 当强制剩余参与者向只是“消失”的设备超时发送消息时，直接调用 `xim::cleanup()` 可能会导致剩余参与者的通信性能问题。
