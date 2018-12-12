---
title: 使用 XIM (C++)
description: 了解如何将 Xbox 集成多人游戏 (XIM) 与 C++ 结合使用。
ms.date: 04/24/2018
ms.topic: article
keywords: Xbox live, xbox, 游戏, xbox one, xbox 集成多人游戏
ms.localizationpriority: medium
ms.openlocfilehash: eb01ac741a40b54e00efb5a602ec39c24f2990e0
ms.sourcegitcommit: 49d58bc66c1c9f2a4f81473bcb25af79e2b1088d
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/11/2018
ms.locfileid: "8946658"
---
# <a name="using-xim-c"></a>使用 XIM (C++)

> [!div class="op_single_selector" title1="Language"]
> - [C++](using-xim.md)
> - [C#](using-xim-cs.md)

这是关于使用 XIM 的 C++ API 的简短演练。 需要通过 C# 访问 XIM 的游戏开发人员应查看[使用 XIM (C#)](using-xim-cs.md)。

- [使用 XIM (C++)](#using-xim-c)
    - [先决条件](#prerequisites)
    - [初始化和启动](#initialization-and-startup)
    - [异步操作和处理状态更改](#asynchronous-operations-and-processing-state-changes)
    - [处理 XIM 玩家对象](#handling-xim-player-objects)
    - [允许好友加入并邀请他们](#enabling-friends-to-join-and-inviting-them)
    - [基本匹配和随他人移动到其他 XIM 网络](#basic-matchmaking-and-moving-to-another-xim-network-with-others)
    - [禁用匹配 NAT 规则以进行调试](#disabling-matchmaking-nat-rule-for-debugging-purposes)
    - [离开 XIM 网络并清理](#leaving-a-xim-network-and-cleaning-up)
    - [发送和接收消息](#sending-and-receiving-messages)
    - [评估数据路径质量](#assessing-data-pathway-quality)
    - [使用玩家自定义属性共享数据](#sharing-data-using-player-custom-properties)
    - [使用网络自定义属性共享数据](#sharing-data-using-network-custom-properties)
    - [使用按玩家技能匹配](#matchmaking-using-per-player-skill)
    - [使用按玩家角色匹配](#matchmaking-using-per-player-role)
    - [XIM 如何与玩家团队配合工作](#how-xim-works-with-player-teams)
    - [使用聊天](#working-with-chat)
    - [将玩家静音](#muting-players)
    - [使用玩家团队配置聊天目标](#configuring-chat-targets-using-player-teams)
    - [玩家空位的自动后台填充（“回填”匹配）](#automatic-background-filling-of-player-slots-backfill-matchmaking)
    - [查询可加入的网络](#querying-joinable-networks)

## <a name="prerequisites"></a>先决条件

开始使用 XIM 编码之前，有两个先决条件。

1. 必须通过标准多人游戏网络功能配置应用的 AppXManifest，并且必须配置网络清单部分，以声明 XIM 使用的必要流量模式模板。

    AppXManifest 功能和网络清单在平台文档中的对应部分中进行了详细介绍；[XIM 项目配置](xim-manifest.md) 中提供了要粘贴的特定于 XIM 的典型 XML。

1. 你需要使两条应用程序标识信息可用：

    * 已分配的 Xbox Live 标题 ID。
    * 作为预配访问 Xbox Live 服务的应用程序的一部分提供的服务配置 ID。

    有关获取这些信息的详细信息，请询问 Microsoft 代表。 将在初始化期间使用这些信息。

编译 XIM 需要包括 `XboxIntegratedMultiplayer.h` 主标头。 为了进行正确地链接，项目还必须在至少一个编译单元中包含 `XboxIntegratedMultiplayerImpl.h`（建议使用通用的预编译标头，因为这些存根功能实现很小，编译器很容易将其生成为“内联”）。

如 [XIM 概述](../xbox-integrated-multiplayer.md)中所述，在使用 XIM 接口时，项目无需在使用 C++/CX 或传统 C++ 进行编译之间作出选择；该接口可与以上两者一起使用。 此外，该实现也不会将引发异常作为报告非致命错误的手段，因此可以根据需要在无异常项目中轻松使用它。

## <a name="initialization-and-startup"></a>初始化和启动

通过 Xbox Live 服务配置 ID 字符串和游戏 ID 号，你可以初始化 XIM 对象单一实例并开始与库交互。 如果还要使用 Xbox 服务 API，你可以方便地从 `xbox::services::xbox_live_app_config` 中检索。 以下示例假定值已经分别位于“myServiceConfigurationId”和“myTitleId”变量中：

```cpp
xim::singleton_instance().initialize(myServiceConfigurationId, myTitleId);
```

初始化后，应用应该为当前位于将要参与的本地设备上的所有用户检索 Xbox 用户 ID 字符串，并将其传递给 `xim::set_intended_local_xbox_user_ids()` 调用。 以下示例代码假定单个用户已经按下表示意图运行的控制器按钮，且与用户关联的 Xbox 用户 ID 字符串已经被检索到“myXuid”变量中：

```cpp
xim::singleton_instance().set_intended_local_xbox_user_ids(1, &myXuid);
```

调用 `xim::set_intended_local_xbox_user_ids()` 会立即设置与应添加到 XIM 网络中的本地用户关联的 Xbox 用户 ID。 以后的所有网络操作都将使用此 Xbox 用户 ID 列表，直到通过其他对 `xim::set_intended_local_xbox_user_ids()` 的调用更改列表。

在不存在任何 XIM 网络的情况下，第一步是移动到 XIM 网络以便开始该过程。 如果用户没有记住具体的 XIM 网络，最佳做法是，只需移动到新的空网络。 这样，用户的好友便可以加入此网络并将其作为某种“大厅”，在“大厅”中，他们可以进行协作，选择下多人游戏活动（例如，一起进入匹配）。

下面的示例显示了如何将先前使用 `xim::set_intended_local_xbox_user_ids()` 添加的本地用户移动到空 XIM 网络（拥有最多能容纳共 8 位玩家的空间）：

```cpp
xim::singleton_instance().move_to_new_network(8, xim_players_to_move::bring_only_local_players);
```

对 `xim::move_to_new_network()` 的调用将会启动异步移动操作，最后以状态更改事件结束，开发人员应定期处理此类事件。

## <a name="asynchronous-operations-and-processing-state-changes"></a>异步操作和处理状态更改

XIM 的核心是应用对方法的 `xim::start_processing_state_changes()` 和 `xim::finish_processing_state_changes()` 对的定期且频繁的调用。 这些方法是，如何通知 XIM 应用已准备好处理多人游戏状态的更新，以及 XIM 如何提供这些更新。 它们被设计为可快速操作，以便可以在 UI 呈现循环中的每个图形帧调用它们。 这提供了方便的位置来检索所有已排队更改，而不必担心网络计时的不可预测性或多线程回调的复杂性。 XIM API 确实针对此单线程模式进行了优化。 它保证其状态在这两个函数之外将保持不变，因此，你可以直接、高效地使用它。

出于相同的原因，*不*应认为 XIM API 返回的所有对象都是线程安全的。 库拥有内部多线程保护，但是，如果你需要任意线程访问任意 XIM 值，则仍需要执行自身锁定。 例如，一个线程遍历 `xim::players()` 列表，而另一线程可能调用 `xim::start_processing_state_changes()` 或 `xim::finish_processing_state_changes()` 并更改与该玩家列表关联的内存。

调用 `xim::start_processing_state_changes()` 时，会在 `xim_state_change` 结构指针数组中报告所有已排队更新。

应用应该：

1. 迭代该数组。
1. 检查基本结构的更具体类型。
1. 将基本结构类型转换为相应更详细的类型。
1. 根据需要处理更新。

完成所有当前可用的 `xim_state_change` 结构后，应通过调用 `xim::finish_processing_state_changes()` 将该数组传递回 XIM 以释放资源。 例如：

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
           MyHandlePlayerJoined(static_cast<const xim_player_joined_state_change*>(stateChange));
           break;
        }

       case xim_state_change_type::player_left:
       {
           MyHandlePlayerLeft(static_cast<const xim_player_left_state_change*>(stateChange));
           break;
       }

       ...
    }
 }
 xim::singleton_instance().finish_processing_state_changes(stateChanges);
```

既然你有了基本处理循环，便可以处理与初始 `xim::move_to_new_network()` 操作关联的状态更改。 每个 XIM 网络移动操作都会从 `xim_move_to_network_starting_state_change` 开始。 如果出于任何原因，移动失败，则会向应用提供 `xim_network_exited_state_change`，对于阻止你移动到 XIM 网络或将你与当前 XIM 网络断开连接的任何异步错误而言，这是常见的故障处理机制。 否则，将在完成所有状态，并将所有玩家成功添加到 XIM 网络后，使用 `xim_move_to_network_succeeded_state_change` 完成移动。

当用户没有在多人游戏会话中与其他人一起玩游戏的平台特权时，XIM 会向试图创建或加入 XIM 网络的设备发送 `xim_network_exited_state_change`，并显示原因 `xim_network_exit_reason::user_multiplayer_restricted`。 当使用 `xim::set_intended_local_xbox_user_ids()` 设置的任何本地用户具有 Xbox Live 多人游戏限制时，将会发生这种情况。 使用 XIM 的 Xbox ERA 应用应该先对本地玩家调用 `Store::Product::CheckPrivilegeAsync`，然后再尝试将其移动到 XIM 网络中，并将 attemptResolution 设置为 true，也可以执行该操作，以响应接收 `xim_network_exit_reason::user_multiplayer_restricted` 作为返回 `xim_network_exited_state_change` 的原因。

## <a name="handling-xim-player-objects"></a>处理 XIM 玩家对象

假定将单个本地用户移动到新的 XIM 网络的示例成功完成，则会为应用提供本地 `xim_player` 对象的 `xim_player_joined_state_change`。 只要玩家实例本身有效，此对象指针就将保持有效。 如果该玩家实例对应的 `xim_player_left_state_change` 已通过调用 `xim::finish_processing_state_changes()` 返回，玩家实例将变为无效。 始终会为应用提供每个 `xim_player_joined_state_change` 的 `xim_player_left_state_change`。

你还可以使用 `xim::get_players()` 随时检索 XIM 网络中所有 `xim_player` 对象的数组。

`xim_player` 对象有很多有用的方法，例如，`xim_player::gamertag()` 可检索与玩家关联的当前 Xbox Live 玩家代号字符串，以用于显示。 如果 `xim_player` 位于设备本地，则它将报告 `xim_player::local()` 中的非 null `xim_player::xim_local` 对象指针，其中有其他仅适用于本地玩家的方法。

当然，对玩家而言，最重要的状态不是 XIM 知道的通用信息，而是特定应用要跟踪的信息。由于你对该跟踪信息可能有自己的构造，因此，可能需要将 `xim_player` 对象链接到自己的玩家构造对象，这样，每当 XIM 报告 `xim_player` 时，你都可以快速检索状态，而无需填充和查找自定义的玩家上下文指针。 以下示例假定你的隐私状态指针已经在变量“myPlayerStateObject”中，且新添加的 `xim_player` 对象已经在变量“newXimPlayer”中：

```cpp
newXimPlayer->set_custom_player_context(myPlayerStateObject);
```

这会将带玩家对象的指定指针值保存到本地（它绝不会通过网络传输到远程设备，远程设备中的内存无效）。 通过检索自定义上下文并将其重新转换为对象，你将能够始终返回你的对象，如以下示例所示：

```cpp
myPlayerStateObject = reinterpret_cast<MyPlayerState *>(newXimPlayer->custom_player_context());
```

你可以随时更改分配给 `xim_player` 的自定义玩家上下文指针。

## <a name="enabling-friends-to-join-and-inviting-them"></a>允许好友加入并邀请他们

出于隐私和安全性目的，默认情况下，所有新 XIM 网络都会自动配置为仅可由本地玩家加入，就绪后，由应用负责明确允许其他玩家加入。 下面的示例显示如何使用 `xim::network_configuration()` 检索当前网络配置，并使用 `xim::set_network_configuration()` 更新网络的可加入性，以开始允许新的本地用户作为玩家加入，以及已经获得邀请或正被 XIM 网络中现有玩家“关注”（一种 Xbox Live 社交关系）的其他用户作为玩家加入：

```cpp
xim_network_configuration networkConfiguration = *xim::singleton_instance.network_configuration();
networkConfiguration.allowed_player_joins = xim_allowed_player_joins::local | xim_allowed_player_joins::invited | xim_allowed_player_joins::followed;
xim::singleton_instance().set_network_configuration(&networkConfiguration);
```

`xim::set_network_configuration()` 异步执行。 完成前一代码示例调用后，将会提供 `xim_network_configuration_changed_state_change` 以通知应用，可加入性值已更改，不再是默认值 `xim_allowed_player_joins::none`。 然后，你可以通过检查 `xim::network_configuration()` 返回 `xim_network_configuration` 的 `allowed_player_joins` 属性来查询新值。 

当设备在 XIM 网络中时，可以通过检查 `allowed_player_joins` 确定该网络的可加入性。

如果其中一个本地玩家向远程用户发送加入此 XIM 网络的邀请，则应用可以调用 `xim_player::xim_local::show_invite_ui()` 以启动系统邀请 UI。 此时，本地用户可以选择他们希望邀请的人并发送邀请。 以下示例演示了此操作，并假定变量“ximPlayer”指向有效的本地 `xim_player`：

```cpp
ximPlayer->local()->show_invite_ui();
```

上述语句执行后，将会显示系统邀请 UI。 当用户发送邀请（或取消 UI）后，在下次调用 `xim::start_processing_state_changes()` 时会提供 `xim_show_invite_ui_completed_state_change`。 或者，应用可以使用 `xim_player::xim_local::invite_users()` 直接发送邀请而不调出系统邀请 UI。

无论使用哪种方式，远程用户都会在登录时收到 Xbox Live 邀请消息，并可以选择接受或拒绝。 如果他们接受邀请，协议激活将启用这些设备上尚未运行的应用，并且协议激活将通过可用于移动到相应的 XIM 网络的事件参数传递到应用。

有关协议激活本身的详细信息，请参阅平台文档。

以下示例展示了对 `xim::extract_protocol_activation_information()` 的调用如何确定事件参数是否适用于 XIM。 此示例假定你已经将原始 URI 字符串从 `Windows::ApplicationModel::Activation::ProtocolActivatedEventArgs` 检索到变量“uriString”：

```cpp
xim_protocol_activation_information activationInfo;
bool isXimActivation;
isXimActivation = xim::singleton_instance().extract_protocol_activation_information(uriString, &activationInfo);
```

如果是 XIM 激活，则需要确保填充的 `xim_protocol_activation_information` 结构的“local_xbox_user_id”字段中标识的本地用户已登录，并在指定给 `xim::set_intended_local_xbox_user_ids()` 的用户中。 然后，你可以使用相同的 URI 字符串，通过调用 `xim::move_to_network_using_protocol_activated_event_args()` 开始移动到指定的 XIM 网络中。 例如：

```cpp
xim::singleton_instance().move_to_network_using_protocol_activated_event_args(uriString);
```

另请注意，受到“关注”的远程用户可以导航到系统 UI 中的本地用户玩家卡片并自行启动加入尝试，无需邀请（假定你已经允许此类玩家加入，如上所述）。 此操作也会协议激活你的应用（和邀请一样），并且以相同的方式处理。

使用协议激活移动到 XIM 网络和[初始化和启动](#initialization-and-startup)中所示的移动到新 XIM 网络相同。 唯一的区别是，移动成功后，会向移动的设备提供表示所有适用玩家的本地和远程玩家 `xim_player_joined_state_change` 结构。 当然，XIM 网络中已经存在的原始设备不会移动，但会看到通过其他 `xim_player_joined_state_change` 结构将新设备的用户添加为玩家。

跨不同设备的玩家在 XIM 网络中集合后，可以启动多人游戏场景、自动通过语音和文字进行通信以及发送特定于应用的消息。

## <a name="basic-matchmaking-and-moving-to-another-xim-network-with-others"></a>基本匹配和随他人移动到其他 XIM 网络

通过将玩家移动到有陌生人的 XIM 网络，你可以进一步拓展一组好友的网络体验。 陌生人是基于同样的兴趣，通过 Xbox Live 匹配服务汇聚到一起的来自全世界的对手。

使用 XIM 启动基本匹配的一种简单方法是在其中一台填充了 `xim_matchmaking_configuration` 结构的设备上调用 `xim::move_to_network_using_matchmaking()`，从随之带上当前 XIM 网络中的玩家。

下面的示例通过使用一种匹配配置启动移动，该配置设置为寻找共 8 个玩家参与无团队自由竞赛。 如果找不到共 8 个玩家，系统可能仍会将 2-7 个玩家匹配在一起。 此示例使用应用定义的 uint64_t 类型的常量，名为 MYGAMEMODE_DEATHMATCH，表示要筛除的游戏模式。 XIM 的匹配只会将此网络与指定相同值的其他玩家匹配，当第二个参数 `xim_players_to_move::bring_existing_social_players` 指定为 `xim::move_to_network_using_matchmaking()` 时，会从当前 XIM 网络中带走以社交方式加入的所有玩家。

```cpp
xim_matchmaking_configuration matchmakingConfiguration = { 0 };
matchmakingConfiguration.team_configuration.team_count = 1;
matchmakingConfiguration.team_configuration.min_player_count_per_team = 2;
matchmakingConfiguration.team_configuration.max_player_count_per_team = 8;
matchmakingConfiguration.custom_game_mode = MYGAMEMODE_DEATHMATCH;

xim::singleton_instance().move_to_network_using_matchmaking(matchmakingConfiguration, xim_players_to_move::bring_existing_social_players);
```

和之前的移动一样，这一匹配过程会在所有设备上提供初始 `xim_move_to_network_starting_state_change`，并在成功完成移动后提供 `xim_move_to_network_succeeded_state_change`。 由于这是从一个 XIM 网络到另一个 XIM 网络的移动，因此，其中一个区别是，已经存在为本地和远程用户添加的现有 `xim_player` 对象，且会为所有一起移动到新 XIM 网络的玩家保留这些对象。 在进行匹配过程中，玩家之间的聊天和数据通信将继续工作，不会中断。

匹配可能是漫长的过程，具体取决于匹配池中也调用了 `xim::move_to_network_using_matchmaking()` 的潜在玩家数量。

整个操作过程中将定期提供 `xim_matchmaking_progress_updated_state_change`，使你和你的用户了解当前状态。 发现匹配时，会将其他玩家添加到具有典型 `xim_player_joined_state_change` 的 XIM 网络中，然后，移动完成。

完成与这组“匹配”玩家的多人游戏体验后，你可以通过另一轮匹配重复移动到其他 XIM 网络的过程。 你会看到通过之前的 `xim::move_to_network_using_matchmaking()` 操作加入的每个玩家提供 `xim_player_left_state_change`，以指示其 `xim_player` 对象不再处于同一 XIM 网络中。

当你选择通过使用 `xim::move_to_network_using_matchmaking()` 再次启动匹配时，如果指定 `xim_players_to_move::bring_existing_social_players`，则在进行新匹配时，通过社交入口点（`xim::move_to_network_using_protocol_activated_event_args()` 或 `xim::move_to_network_using_joinable_xbox_user_id()`）加入的玩家将会保留。 或者，指定 `xim_players_to_move::bring_only_local_players` 将会使以社交方式连接的远程玩家断开连接，只保留本地玩家。 在两种情况下，第二次移动操作完成后，将添加另一组陌生人。

你也可以在决定下匹配配置/多人游戏活动时，选择将非匹配玩家（或仅本地玩家）移动到全新的 XIM 网络。 下面的示例演示了设备如何对拥有最多 8 个玩家的 XIM 网络调用 `xim::move_to_new_network()`，但是，这次还包含以社交方式加入的现有玩家：

```cpp
xim::singleton_instance().move_to_new_network(8, xim_players_to_move::bring_existing_social_players);
```

将会为所有参与设备提供 `xim_move_to_network_starting_state_change` 和 `xim_move_to_network_succeeded_state_change`，并为留下的匹配玩家提供 `xim_player_left_state_change`。 对于移出网络的每个玩家，这些设备上会以同样的方式显示 `xim_player_left_state_change`。

你可以根据需要，多次以上述方式持续从 XIM 网络移动到 XIM 网络。

出于性能原因，Xbox Live 服务不会尝试匹配不太可能建立任何直接对等连接的设备上的玩家组。 如果你在未正确配置为支持标准 Xbox Live 多人游戏的网络环境中开发，则使用匹配移动到新网络的操作可能会无限期继续，而不进行成功匹配，即使你确定有足够满足匹配条件的玩家，这些玩家都在移动，且都在使用同一本地环境中的设备。 请务必在网络设置区域/Xbox 应用程序中运行多人游戏连接测试，并在其报告问题（尤其是关于“严格 NAT”）时遵循它的建议。

## <a name="disabling-matchmaking-nat-rule-for-debugging-purposes"></a>禁用匹配 NAT 规则以进行调试

如果网络管理员无法进行必要的环境更改以支持 XIM 的 NAT 规则，通过将 XIM 配置为允许在没有至少“开放 NAT”设备的情况下匹配“严格 NAT”设备，你可以取消阻止 Xbox One 开发工具包上的匹配测试。 可通过在所有 Xbox One 主机上的“游戏草稿”的根目录下放置名为“xim_disable_matchmaking_nat_rule”（内容不重要）的文件完成此操作。 示例操作方法是，在启动应用前，从 XDK 命令提示符执行以下内容，根据需要，替换每个控制台的“{console_name_or_ip_address}”占位符：

```bat
echo.>%TEMP%\emptyfile.txt
copy %TEMP%\emptyfile.txt \\{console_name_or_ip_address}\TitleScratch\xim_disable_matchmaking_nat_rule
del %TEMP%\emptyfile.txt
```

> [!NOTE]
> 此开发解决方法当前仅适用于 Xbox One 独占资源应用程序，不支持用于通用 Windows 应用程序。 另请注意，无论网络环境如何，使用此设置的控制台都不会与不存在此文件的设备匹配，因此，请务必随时随地添加或删除此文件。

## <a name="leaving-a-xim-network-and-cleaning-up"></a>离开 XIM 网络并清理

本地用户参与 XIM 网络后，他们通常只会重新移动到新 XIM 网络中，此网络允许本地用户、邀请用户和受到“关注”的用户加入，使他们能继续与其好友协作，查找下活动。 但是，如果用户完全完成了所有多人游戏体验，则你的应用可能要开始完全离开 XIM 网络，然后返回就像仅调用了 `xim::initialize()` 和 `xim::set_intended_local_xbox_user_ids()` 的状态。 此操作使用 `xim::leave_network()` 方法完成：

```cpp
xim::singleton_instance().leave_network();
```

此方法会开始顺利地异步断开与其他参与者的连接的过程。 这会导致为远程设备提供已断开连接的每个本地玩家的 `xim_player_left_state_change`。 也会为本地设备提供已断开连接的每个本地和远程玩家的 `xim_player_left_state_change`。 完成所有断开连接操作后，将提供最终 `xim_network_exited_state_change`。 然后，该应用可调用 `xim::cleanup()``，以释放所有资源并返回到未初始化状态：

```cpp
xim::singleton_instance().cleanup();
```

当尚未提供 `xim_network_exited_state_change` 时，始终强烈建议调用 `xim::leave_network()` 并等待 `xim_network_exited_state_change` 以便顺利退出 XIM 网络。

如果直接调用 `xim::cleanup()` 而没有调用 `xim::leave_network()`，其余参与者会发生通信性能问题，因为系统会强制他们向因调用 `xim::cleanup()` 而“消失的”设备发送无法交付的消息。

## <a name="sending-and-receiving-messages"></a>发送和接收消息

XIM 及其基础组件会执行建立安全的 Internet 通信渠道的所有繁琐操作，因此你无需担心连接问题或只能访问部分玩家。 如果有任何基本对等连接问题，将无法成功移动到 XIM 网络。

在组成并通过 `xim_move_to_network_succeeded_state_change` 确认 XIM 网络后，已加入的所有设备上的所有应用实例保证获得有关已连接到 XIM 网络的每个 `xim_player` 的通知，并可以向其中的任何发送消息。

下面我们演示如何跨 XIM 网络发送消息。 下面的示例假定“sendingPlayer”变量是有效本地玩家对象的指针。 “sendingPlayer”可调用 `local()` 和 `send_data_to_other_players()` 以通过有保证的连续传递向所有玩家（本地和远程）发送“msgData”消息结构。 请注意，由于未向该方法传递特定玩家的列表，因此会发送给所有玩家：

```cpp
sendingPlayer->local()->send_data_to_other_players(sizeof(msgData), &msgData, 0, nullptr, xim_send_type::guaranteed_and_sequential);
```

使用 `send_data_to_other_players()` 可以向所有玩家传递消息，无需向该方法传递特定玩家的数组。

会向所有消息收件人提供包括指向数据副本的指针的 `xim_player_to_player_data_received_state_change`，以及指向发送和本地接收消息的 xim_player 对象的指针。

当然，有保证的连续传递很方便，但它也有可能成为低效率的发送类型，因为，如果 Internet 丢失了数据包或打乱了数据包的顺序，则 XIM 需要重新传输或延迟消息。 请务必考虑使用应用可容忍丢失或乱序到达的其他消息发送类型。 由于消息数据来自远程计算机，因此，最佳做法是明确定义数据格式，例如按特定字节顺序（“字节顺序”）打包多字节值。 对数据执行操作之前，应用还应验证数据。

XIM 提供网络级别的安全，因此不需要使用额外的加密或签名方案。 不过，我们建议始终使用“深度防护”以避免受到意外应用程序错误的影响并能够处理正常共存的不同版本的应用程序协议（在开发、内容更新等期间）。 用户的 Internet 连接可能是受限且不断变化的资源。 我们强烈建议使用高效的消息数据格式，并避免采用会发送每个 UI 框架的设计。

## <a name="assessing-data-pathway-quality"></a>评估数据路径质量

若要了解有关两个玩家之间的当前数据路径质量的信息，请调用 `xim_player::network_path_information()` 方法。 以下示例会将指针检索到“remotePlayer”变量中包含的 `xim_player` 指针的 `xim_network_path_information` 结构：

```cpp
 const xim_network_path_information * networkPathInfo = remotePlayer->network_path_information();
```

如果连接在该时刻不支持传输更多数据，则返回的结构包含估计的往返行程延迟以及有多少消息仍在本地排队等待传输。

如果发现队列正在对某一特定“xim_player”进行备份，你应该降低发送数据的速率。

`xim_network_path_information::round_trip_latency_in_milliseconds` 字段表示在不排队情况下的基础网络延迟和 XIM 预计延迟。 有效延迟将随着 `xim_network_path_information::send_queue_size_in_messages` 增加以及 XIM 队列推进而增加。

请根据游戏使用情况和要求选择开始限制 `send_data_to_other_players` 调用的合理点。 发送队列中的每个消息都代表着有效网络延迟的增加。

接近 XIM 最大限制（当前为 3500 条消息）的值对于大部分游戏而言都太高了，而且可能意味着需要等待数秒才能发送数据，具体等待时长取决于调用 `send_data_to_other_players` 的速率以及每个有效数据负载的大小。 相反，我们选择的数值应该考虑到游戏的延迟要求以及游戏 `send_data_to_other_players` 调用模式的不稳定程度。

## <a name="sharing-data-using-player-custom-properties"></a>使用玩家自定义属性共享数据

大多数应用数据交易都会通过 `xim_player::xim_local::send_data_to_other_players()` 方法发生，因为它允许对接收数据的用户和时间、系统如何处理数据包丢失问题进行最大程度的控制。 但有时候，能让玩家很容易地将关于自身的基本、很少更改的状态与他人分享是不错的选择。 例如，在进入所有玩家用来呈现其游戏内表示形式的多人游戏之前，每个玩家可能都有表示所选的人物模型的固定字符串。

对于与玩家有关的不太频繁更改的数据，XIM 提供玩家自定义属性。 这些属性由应用定义的名称和值构成，它们是以 null 结尾的字符串对，可以应用于本地玩家，并可以在设备更改后自动传播到所有设备。

与 `xim_player::xim_local::send_data_to_other_players()` 方法相比，自定义玩家属性的内部同步开销更多，因此，对于快速变化的数据（即玩家位置），你应该仍然使用直接发送。

当新参与的设备加入 XIM 网络并查看添加的玩家时，会将玩家自定义属性键值对的当前值自动提供给这些设备。 可以通过将 `xim_player::xim_local::set_player_custom_property()` 与名称和值字符串一起调用来设置值。 在下面的示例中，我们将设置名为“型号”的属性，使变量“localPlayer”指向的本地 `xim_player` 对象上的“brute”值。

```cpp
localPlayer->local()->set_player_custom_property(L"model", L"brute");
```

更改玩家属性会导致为所有设备提供 `xim_player_custom_properties_changed_state_change`，以警告它们属性的名称已发生更改。 可使用 `xim_player::get_player_custom_property()` 在任意本地或远程玩家上检索给定名称的值。 以下示例检索了变量“ximPlayer”指向的 `xim_player` 中名为“模型”的属性的值：

```cpp
PCWSTR modelName = ximPlayer->get_player_custom_property(L"model");
```

为给定的属性名称设置新值会替换任何现有值。 如果将属性名称设置为某个 null 值字符串指针的值，则会将该属性与空值字符串同等对待，这相当于尚未指定该属性。 XIM 不会解释名称和值，因此，将由应用根据需要验证字符串内容。

从一个 XIM 网络移动到另一个 XIM 网络时，始终会重置自定义玩家属性。 但是，加入 XIM 网络的新玩家将会收到已设置某些自定义玩家属性的所有玩家的当前自定义玩家属性。

## <a name="sharing-data-using-network-custom-properties"></a>使用网络自定义属性共享数据

网络自定义属性旨在方便同步特定于不频繁更改的 XIM 网络的数据。 网络自定义属性的工作方式与[玩家自定义属性](#sharing-data-using-player-custom-properties)完全相同，只不过它们是使用 `xim::set_network_custom_property()` 在 XIM 单一实例对象上设置的。 以下示例设置了“地图”属性，使其拥有“要塞”一值:

```cpp
xim::singleton_instance().set_network_custom_property(L"map", L"stronghold");
```

更改网络属性会导致为所有设备提供 `xim_network_custom_properties_changed_state_change`，以警告它们属性的名称已发生更改。 可使用 `xim::get_network_custom_property()` 检索给定名称的值，例如，以下示例检索了名为“地图”的属性的值：

```cpp
PCWSTR mapName = xim::singleton_instance().get_network_custom_property(L"map");
```

正如[玩家自定义属性](#sharing-data-using-player-custom-properties)一样，为给定的属性名称设置新值会替换任何现有值。 如果将属性名称设置为某个 null 值字符串指针的值，则会将该属性与空值字符串同等对待，这相当于尚未指定该属性。 XIM 不会解释名称和值，因此，将由应用根据需要验证字符串内容。

新创建的 XIM 网络最开始时始终没有设置网络自定义属性。 但加入现有 XIM 网络的新玩家将会收到为该 XIM 网络设置的网络自定义属性的当前值。

## <a name="matchmaking-using-per-player-skill"></a>使用按玩家技能匹配

在某个应用指定的游戏模式下，按共同兴趣匹配玩家是不错的基本策略。 当可用玩家池扩大时，你还应该考虑基于玩家的个人技能或游戏体验匹配玩家，以便资深玩家可以享受与其他资深玩家的良性竞争带来的挑战，而新玩家可以通过与能力相似的玩家进行竞争来获得成长。

要执行此操作，在使用匹配开始移动到 XIM 网络前，请先在对 `xim_player::xim_local::set_roles_and_skill_configuration()` 的调用中指定的按玩家角色和技能配置结构中提供所有本地玩家的技能等级。 技术等级是特定于应用的概念，且 XIM 不会解释数字，不过，匹配会首先尝试查找具有相同技能值的玩家，然后定期以 +/- 10 的增量扩大搜索范围，以尝试查找其他在此技能附近范围内声明技能值的玩家。 以下示例假定本地 `xim_player` 对象（其指针为“localPlayer”）有一个关联的特定于应用的 uint32_t 技能值，该值从本地或 Xbox Live 存储检索到名为“playerSkillValue”的变量中：

```cpp
xim_player_roles_and_skill_configuration playerRolesAndSkillConfiguration = { 0 };
playerRolesAndSkillConfiguration.skill = playerSkillValue;

localPlayer->local()->set_roles_and_skill_configuration(&playerRolesAndSkillConfiguration);
```

此操作完成时，会向所有参与者提供 `xim_player_roles_and_skill_configuration_changed_state_change`，以指示此 `xim_player` 已更改其按玩家角色和技能配置。 可通过调用 `xim_player::roles_and_skill_configuration()` 检索新值。 当所有玩家都应用非 null 角色和技能配置时，你可以使用匹配将其移动到 XIM 网络，并为 `xim::move_to_network_using_matchmaking()` 指定 `xim_matchmaking_configuration` 结构的 `require_player_roles_and_skill_configuration` 字段中的 true 值。 以下示例使用由 MYGAMEMODE_DEATHMATCH 值（该值仅与指定相同值、或需要按玩家角色和技能配置的其他玩家匹配）定义的特定于应用的游戏模式常量 uint64_t 填充匹配配置，此配置将为无团队自由竞赛寻找共 2-8 个玩家：

```cpp
xim_matchmaking_configuration matchmakingConfiguration = { 0 };
matchmakingConfiguration.team_configuration.team_count = 1;
matchmakingConfiguration.team_configuration.min_player_count_per_team = 2;
matchmakingConfiguration.team_configuration.max_player_count_per_team = 8;
matchmakingConfiguration.custom_game_mode = MYGAMEMODE_DEATHMATCH;
matchmakingConfiguration.require_player_roles_and_skill_configuration = true;
```

为 `xim::move_to_network_using_matchmaking()` 提供此结构时，只要移动的玩家已通过非 null `xim_player_roles_and_skill_configuration` 指针调用 `xim_player::xim_local::set_roles_and_skill_configuration()`，移动操作将正常开始。 如果任意玩家未执行此操作，则会暂停匹配过程，并为所有参与者提供值为 `xim_matchmaking_status::waiting_for_player_roles_and_skill_configuration` 的 `xim_matchmaking_progress_updated_state_change`。 这包括在匹配完成前，通过以前发送的邀请或通过其他方式（例如，调用 `xim::move_to_network_using_joinable_xbox_user_id()`）后续加入 XIM 网络的玩家。 所有玩家都提供其 `xim_player_roles_and_skill_configuration` 结构后，匹配将会继续。

如下一节中所述，使用按玩家技能匹配也可以结合匹配用户按玩家角色使用。 如果只需要其中一个，你可以将另一个的值指定为 0。 这是因为声明 `xim_player_roles_and_skill_configuration` 技能值为 0 的所有玩家始终会彼此匹配。

`xim::move_to_network_using_matchmaking()` 或其他 XIM 网络移动操作完成后，将把所有玩家的 `xim_player_roles_and_skill_configuration` 结构自动清理为 null 指针（附带 `xim_player_roles_and_skill_configuration_changed_state_change` 通知）。 如果你计划使用需要按玩家配置的匹配移动到另一个 XIM 网络，则你需要通过一个包含最新信息的新结构指针再次调用 `xim_player::xim_local::set_roles_and_skill_configuration()`。

## <a name="matchmaking-using-per-player-role"></a>使用按玩家角色匹配

使用按玩家角色和技能配置改进用户匹配体验的另一种方法是使用所需的玩家角色。 这最适合于提供可选择的人物类型的游戏，这种类型可鼓励不同的合作游戏风格；即，这种类型不是仅更改游戏内的图形表示，而是控制互补、有影响力的特性（例如，防御性“医师”与近距离“格斗”与远“距离”攻击支持）。 用户的个性意味着他们可能想要以某个特长进行游戏。 但是，如果游戏设计为当没有至少一个履行每个角色的用户时，则在功能上不可能完成目标，有时，最好先将这些玩家匹配到一起，再将任意玩家匹配到一起，并在集合后要求其协商他们之间的游戏风格。 你可以通过先定义表示要在给定玩家的 `xim_player_roles_and_skill_configuration` 结构中指定的每个角色的唯一位标志来执行此操作。

以下示例为本地 xim_player 对象（其指针为 'localPlayer'）设置了特定于应用的 MYROLEBITFLAG_HEALER uint8_t 角色值：

```cpp
xim_player_roles_and_skill_configuration playerRolesAndSkillConfiguration = { 0 };
playerRolesAndSkillConfiguration.roles = MYROLEBITFLAG_HEALER;

localPlayer->local()->set_roles_and_skill_configuration(&playerRolesAndSkillConfiguration);
```

此操作完成时，会向所有参与者提供 `xim_player_roles_and_skill_configuration_changed_state_change`，以指示此 `xim_player` 已更改其按玩家角色配置。 可通过调用 `xim_player::roles_and_skill_configuration()` 检索新值。

指定给 `xim::move_to_network_using_matchmaking()` 的全局 `xim_matchmaking_configuration` 结构应使用按位 OR 和 require_player_matchmaking_configuration 字段的 true 值合并所有需要的角色标记。

下面的示例填充匹配配置，以便寻找共 3 个玩家参与无团队自由竞赛。 此外，此示例使用应用定义的常量，该常量的类型为 uint64_t，名为 MYGAMEMODE_COOPERATIVE，表示要筛除的游戏模式。 此外，将配置设置为需要按玩家角色和技能配置，其中至少一个玩家满足每个应用特定的 uint8_t 角色，这些角色均执行按位 OR 运算并被放置于配置（MYROLEBITFLAG_HEALER、MYROLEBITFLAG_MELEE 和 MYROLEBITFLAG_RANGE）中：

```cpp
xim_matchmaking_configuration matchmakingConfiguration = { 0 };
matchmakingConfiguration.team_configuration.team_count = 1;
matchmakingConfiguration.team_configuration.min_player_count_per_team = 3;
matchmakingConfiguration.team_configuration.max_player_count_per_team = 3;
matchmakingConfiguration.custom_game_mode = MYGAMEMODE_COOPERATIVE;
matchmakingConfiguration.required_roles = MYROLEBITFLAG_HEALER | MYROLEBITFLAG_MELEE | MYROLEBITFLAG_RANGE;
matchmakingConfiguration.require_player_roles_and_skill_configuration = true;
```

向 `xim::move_to_network_using_matchmaking()` 提供此结构时，将开始移动操作，如上所述。

使用按玩家角色匹配也可以与按玩家技能匹配用户结合使用。 如果只需要其中一个，可将另一个的值指定为 0。 这是因为，所有声明其 `xim_player_roles_and_skill_configuration` 技能值为 0 的玩家始终互相匹配，如果 `xim_matchmaking_configuration` required_roles 字段中均为零位，则不需要角色位就能匹配。

`xim::move_to_network_using_matchmaking()` 或其他 XIM 网络移动操作完成后，将把所有玩家的 `xim_player_roles_and_skill_configuration` 结构自动清理为 null 指针（附带 `xim_player_roles_and_skill_configuration_changed_state_change` 通知）。 如果你计划使用需要按玩家配置的匹配移动到另一个 XIM 网络，则你需要通过一个包含最新信息的新结构指针再次调用 `xim_player::xim_local::set_roles_and_skill_configuration()`。

## <a name="how-xim-works-with-player-teams"></a>XIM 如何与玩家团队配合工作

多人游戏通常涉及到组织到对方团队的玩家。 XIM 可通过设置 `xim_team_configuration` 简化匹配时的团队分配。 以下示例通过使用经过配置以寻找共 8 个玩家来编入两个同样的 4 人团队（如果找不到 4 个玩家，1-3 个玩家也可接受）的匹配发起移动，方法是，使用特定于应用的游戏模式常量 uint64_t（该常量由仅与指定相同值的其他玩家匹配的值 MYGAMEMODE_CAPTURETHEFLAG 定义），并将当前 XIM 网络中的所有以社交方式加入的玩家聚到一起：

```cpp
xim_matchmaking_configuration matchmakingConfiguration = { 0 };
matchmakingConfiguration.team_configuration.team_count = 2;
matchmakingConfiguration.team_configuration.min_player_count_per_team = 1;
matchmakingConfiguration.team_configuration.max_player_count_per_team = 4;
matchmakingConfiguration.custom_game_mode = MYGAMEMODE_CAPTURETHEFLAG;

xim::singleton_instance().move_to_network_using_matchmaking(matchmakingConfiguration, xim_players_to_move::bring_existing_social_players);
```

完成此 XIM 网络移动操作后，将通过 {n}（与请求的 {n} 个团队对应）为玩家分配团队索引值 1。 任何特定团队索引值的真实意义都取决于应用。 通过 `xim_player::team_index()` 检索玩家的团队索引值。 将 `xim_team_configuration` 与两个或以上团队一起使用时，绝不会通过调用 `xim::move_to_network_using_matchmaking()` 来为玩家分配团队索引值 0。 这与通过任意其他移动操作配置或类型（例如通过接受邀请导致的协议激活）添加到 XIM 网络的玩家相反，他们将始终拥有 0 团队索引。 它可能有助于将团队索引 0 视为特殊的“未分配”团队。

以下示例检索 xim_player 对象（其指针位于“ximPlayer”变量中）的团队索引：

```cpp
uint8_t playerTeamIndex = ximPlayer->team_index();
```

对于首选的用户体验（更不用说负面玩家行为的机会减少），Xbox Live 匹配服务绝不会将一起移动到 XIM 网络的玩家拆分到不同的团队中。

最初通过匹配分配的团队索引值只是建议，应用可以使用 `xim_player::xim_local::set_team_index()` 随时为本地玩家进行更改。 还可以在完全不使用匹配的 XIM 网络中调用此值。 以下示例配置了一个玩家指针“localPlayer”，以获得新团队索引值 1：

```cpp
localPlayer->local()->set_team_index(1);
```

为所有设备提供此玩家的 `xim_player_team_index_changed_state_change` 时，会告知其玩家有有效的新团队索引值。 你可以随时调用 `xim_player::xim_local::set_team_index()`。

匹配会独立于团队评估所需的按玩家角色。 因此，不建议将团队和所需的角色用作同时匹配配置标准，因为团队是通过玩家计数（而不是履行的玩家角色）进行平衡的。

## <a name="working-with-chat"></a>使用聊天

会为 XIM 网络中的玩家自动启用语音和文字聊天通信。 XIM 会为你处理与所有语音耳机和麦克风硬件的交互。 你的应用不需要为聊天执行过多操作，但有一个有关文字聊天的要求：支持输入和显示。 需要文本输入是因为，即使在过去没有广泛使用过物理键盘的平台和游戏类型中，玩家也可以配置系统以使用文本到语音转换辅助技术。 同样，需要文本显示是因为，玩家可以配置系统以使用语音到文本转换。

通过调用 `xim_player::xim_local::chat_text_to_speech_conversion_preference_enabled()` 和 `xim_player::xim_local::chat_speech_to_text_conversion_preference_enabled()` 方法可以在本地玩家中检测到这些首选项，并且，你可能想要有条件地启用文本机制。 但是，建议考虑进行文本输入，并显示始终可用的选项。

 `Windows::Xbox::UI::Accessability`  是专门为提供游戏内文字聊天的简单呈现而设计的 Xbox One 类，专注于语音到文本转换辅助技术。

获得由真实或虚拟键盘提供的文本输入后，请将字符串传递给 `xim_player::xim_local::send_chat_text()` 方法。 以下代码显示了从变量“localPlayer”指向的本地 `xim_player` 对象发送示例硬编码字符串：

```cpp
localPlayer->local()->send_chat_text(L"Example chat text");
```

此聊天文本会传递给能够从原始本地玩家接收聊天通信的 XIM 网络中的所有玩家。 它可能会合成为语音音频，并且可能会以 `xim_chat_text_received_state_change` 的形式提供。

你的应用应该对收到的任意文本字符串进行复制，并将它与原始玩家的某些标识一起显示适当时间（或在可滚动窗口中显示）。

还有一些有关聊天的最佳做法。 无论玩家在什么地方显示（尤其是显示在记分牌等玩家代号列表中），都建议你也为用户显示静音/说话图标作为反馈。 通过调用 `xim_player::chat_indicator()` 来检索 `xim_player_chat_indicator`（表示此玩家聊天的当前、即时状态）可完成此操作。 以下示例演示了检索变量“ximPlayer”指向的 `xim_player` 对象的指示器值，以确定要分配给“iconToShow”变量的特定图标常量值：

```cpp
switch (ximPlayer->chat_indicator())
{
   case xim_player_chat_indicator::silent:
   {
       iconToShow = Icon_InactiveSpeaker;
       break;
   }

   case xim_player_chat_indicator::talking:
   {
       iconToShow = Icon_ActiveSpeaker;
       break;
   }

   case xim_player_chat_indicator::muted:
   {
       iconToShow = Icon_MutedSpeaker;
       break;
   }
   ...
}
```

例如，由 `xim_player::chat_indicator()` 报告的值预计会在玩家开始和停止说话时频繁改变。 它旨在支持应用在每个 UI 框架将其作为结果进行轮询。

> [!NOTE]
> 如果本地用户因其设备设置而无足够的通信权限，`xim_player::chat_indicator()` 将返回 `xim_player_chat_indicator::platform_restricted`。 满足平台要求的期望是指应用可显示图标，为语音聊天或发送消息指示平台限制，以及显示发送给用户的消息，以指示问题。 我们建议的示例消息为：“抱歉，当前不允许聊天。”

## <a name="muting-players"></a>将玩家静音

另一个最佳做法是支持静音玩家。 XIM 会通过玩家卡自动处理由用户启动的系统静音，但应用应该支持可在游戏 UI 内通过 `xim_player::set_chat_muted()` 方法执行的特定于游戏的短暂性静音。 以下示例开始静音变量“remotePlayer”指向的远程 `xim_player` 对象，以便不会从中听到语音聊天或收到文字聊天：

```cpp
remotePlayer->set_chat_muted(true);
```

静音立即生效，且没有与之关联的 `xim_state_change`。 通过使用 false 值再次调用 `xim_player::set_chat_muted()` 可以将其撤销。 以下示例取消了变量“remotePlayer”指向的 `xim_player` 对象的静音：

```cpp
remotePlayer->set_chat_muted(false);
```

只要 `xim_player` 存在（包括随玩家移动到新 XIM 网络时），静音就保持有效。 如果玩家离开，然后同一个用户重新加入（作为新的 `xim_player` 实例），则不保留。

玩家通常会在取消静音状态下开始。 如果你的应用出于玩游戏的原因而想在静音状态下启动玩家，它可以在处理完关联的 `xim_player_joined_state_change` 之前，在 `xim_player` 对象上调用 `xim_player::set_chat_muted()`，且 XIM 将保证不会在任何时间段听到来自玩家的语音音频。

当远程玩家加入 XIM 网络时，会基于玩家信誉自动进行静音检查。 如果玩家有信誉不佳的标志，则玩家会被自动静音。 静音仅影响本地状态，因此，如果玩家在网络中移动，静音仍然存在。 只要 `xim_player` 保持有效，基于信誉的自动静音检查就只会执行一次，且不再重新评估。

## <a name="configuring-chat-targets-using-player-teams"></a>使用玩家团队配置聊天目标

如本文档 [XIM 如何与玩家团队配合工作](#how-xim-works-with-player-teams)部分中所述，任何特定团队索引值的真正含义都取决于应用。 XIM 不会解释索引值，与聊天目标配置有关的相等比较除外。 如果由 `xim::chat_targets()` 报告的聊天目标配置当前 `xim_chat_targets::same_team_index_only`，则任意给定玩家将仅与另一个玩家交换聊天通信，前提是两个玩家有相同的由 `xim_player::team_index()` 报告的值（且隐私/策略也允许）。

为保持谨慎并支持竞争方案，会将新建的 XIM 网络配置为默认为 `xim_chat_targets::same_team_index_only`。 但是，可能需要与其他团队中的战败对手聊天，例如，在游戏后的“大厅”中。 通过调用 `xim::set_chat_targets()`，你可以指示 XIM 允许所有人在隐私和策略允许的地方与其他任何人交谈。 以下示例开始配置 XIM 网络中的所有参与者，以使用 `xim_chat_targets::all_players` 值：

```cpp
xim::singleton_instance().set_chat_targets(xim_chat_targets::all_players);
```

向所有参与者提供 `xim_chat_targets_changed_state_change` 时，会告知他们有一个有效的新目标设置。

如之前所述，大多数 XIM 网络移动类型最初都会为所有玩家分配团队索引值 0。 这意味着，默认情况下，`xim_chat_targets::same_team_index_only` 的配置可能与 `xim_chat_targets::all_players` 无法区分。 但是，如果匹配配置的 `xim_team_matchmaking_mode` 值声明两个或以上团队，则使用匹配移动到 XIM 网络的玩家可能有不同的团队索引值。 你还可以随时调用 `xim_player::xim_local::set_team_index()`，如上所示。 如果你的应用通过以下任一方法使用非零团队索引值，请不要忘记相应地管理当前聊天目标设置。

## <a name="automatic-background-filling-of-player-slots-backfill-matchmaking"></a>玩家空位的自动后台填充（“回填”匹配）

同时调用 `xim::move_to_network_using_matchmaking()` 的不同玩家组会为 Xbox Live 匹配服务提供最大的灵活性，以便将玩家快速整理到最优的新 XIM 网络。 但是，某些游戏场景想要将某个 XIM 网络保持原样，只匹配其他玩家，仅仅是为了填充空白玩家空位。 XIM 通过以 `xim_network_configuration` （其 `xim_network_configuration::allowed_player_joins` 属性上设置了 `xim_allowed_player_joins::matchmade` 标志）调用 `xim::set_network_configuration()`，支持配置匹配以在自动背景填充（“回填”）模式下操作。

以下示例使用特定于应用的游戏模式常量 uint64_t 配置了回填匹配，以尝试为无团队自由竞赛寻找共 8 个玩家（如果找不到 8 个玩家，2-7 个玩家也可接受），该常量由仅与指定相同值的其他玩家匹配的值 MYGAMEMODE_DEATHMATCH 定义：

```cpp
xim_network_configuration networkConfiguration = *xim::singleton_instance().network_configuration();
networkConfiguration.allowed_player_joins |= xim_allowed_player_joins::matchmade;
networkConfiguration.team_configuration.team_count = 1;
networkConfiguration.team_configuration.min_player_count_per_team = 2;
networkConfiguration.team_configuration.max_player_count_per_team = 8;
networkConfiguration.custom_game_mode = MYGAMEMODE_DEATHMATCH;

xim::singleton_instance().set_network_configuration(&networkConfiguration);
```

这会使现有 XIM 网络适用于以正常方式调用 `xim::move_to_network_using_matchmaking()` 的设备。 这些设备不会看到任何行为更改。 回填 XIM 网络中的参与者不会移动，但会向其提供表明回填打开的 `xim_network_configuration_changed_state_change`，以及多个 `xim_matchmaking_progress_updated_state_change` 通知（如果适用）。 将使用正常的 `xim_player_joined_state_change` 将所有匹配的玩家添加到 XIM 网络。

默认情况下，回填匹配会保持无限期进行，但它不会在 XIM 网络已经有由 xim_team_configuration 设置指定的最大玩家数时尝试添加玩家。 可以通过设置 xim_allowed_player_joins 不允许匹配来禁用回填。 下面的示例通过以下方式禁用回填：清除 xim_allowed_player_joins::matchmade 标记，同时保留所有其他现有标记和网络配置设置。

```cpp
xim_network_configuration networkConfiguration = *xim::singleton_instance().network_configuration();
networkConfiguration.allowed_player_joins &= ~xim_allowed_player_joins::matchmade;
xim::singleton_instance().set_network_configuration(&networkConfiguration);
```

将向所有设备提供相应的 `xim_network_configuration_changed_state_change`，并且，完成此异步过程后，将提供最终 `xim_matchmaking_progress_updated_state_change` 和 `xim_matchmaking_status::none`，表明不会再向 XIM 网络提供任何匹配的玩家。

当通过声明两个或更多团队的 `xim_team_configuration` 设置启用回填匹配时，所有现有玩家的有效团队索引必须在 1 到团队数之间。 这包括调用 `xim_player::xim_local::set_team_index()` 来指定自定义值，或使用邀请或通过其他社交方式（例如，调用 `xim::move_to_network_using_joinable_xbox_user_id`）加入，并通过默认团队索引值 0 添加的玩家。 如果任意玩家没有有效的团队索引，则会暂停匹配过程，并为所有参与者提供值为 `xim_matchmaking_status::waiting_for_player_team_index` 的 `xim_matchmaking_progress_updated_state_change`。 在所有玩家通过 `xim_player::xim_local::set_team_index()` 提供或更正其团队索引值后，回填匹配将继续。 有关详细信息，请参阅本文档 [XIM 如何与玩家团队配合工作](#how-xim-works-with-player-teams)部分。

同样，当通过 `xim_network_configuration` 结构启用回填匹配，并将角色或技能的 `require_player_roles_and_skill_configuration` 字段设置为 true 时，所有玩家必须指定非 null 按玩家匹配配置。 如果任意玩家未执行此操作，则会暂停匹配过程，并为所有参与者提供值为 `xim_matchmaking_status::waiting_for_player_roles_and_skill_configuration` 的 `xim_matchmaking_progress_updated_state_change`。 所有玩家都提供其 `xim_player_roles_and_skill_configuration` 结构后，回填将会继续。 有关详细信息，请参阅本文档[使用按玩家技能匹配](#matchmaking-using-per-player-skill)和[使用按玩家角色匹配](#matchmaking-using-per-player-role)部分。

## <a name="querying-joinable-networks"></a>查询可加入的网络

匹配是将玩家快速连接在一起的一种很好的方法，有时最好允许玩家通过使用自定义搜索条件发现可加入的网络，并选择他们希望加入的网络。 这在游戏会话可能有大量可配置游戏规则和玩家首选项时尤具优势。 为此，必须先将现有网络设置为可查询，方法是启用 `xim_allowed_player_joins::queried` 可加入性，并通过调用 `xim::set_network_configuration()` 配置可供网络外部的其他人访问的网络信息。

下面的示例启用 `xim_allowed_player_joins::queried` 可加入性，设置以下网络配置：一个允许总共 1-8 位玩家加入 1 个团队的团队配置；一个特定于应用的游戏模式常量 uint64_t（由 GAME_MODE_BRAWL 值定义）；一个描述 "cat and sheep's boxing match"；一个特定于应用的地图索引常量 uint32_t（由 MAP_KITCHEN 值定义）；并包含三个标记 "chatrequired"、"easy"、"spectatorallowed"：

```cpp
PCWSTR tags[] = { L"chatrequired", L"easy", L"spectatorallowed" };
xim_network_configuration networkConfiguration = *xim::singleton_instance().network_configuration();
networkConfiguration.allowed_player_joins |= xim_allowed_player_joins::queried;
networkConfiguration.team_configuration.team_count = 1;
networkConfiguration.team_configuration.min_player_count_per_team = 1;
networkConfiguration.team_configuration.max_player_count_per_team = 8;
networkConfiguration.custom_game_mode = GAME_MODE_BRAWL;
networkConfiguration.description = L"cat and sheep's boxing match";
networkConfiguration.map_index = MAP_KITCHEN;
networkConfiguration.tag_count = _countof(tags);
networkConfiguration.tags = tags;

xim::set_network_configuration(&networkConfiguration);
```

然后，网络外部的其他玩家可以通过一组与前面 `xim::set_network_configuration()` 调用中的网络信息匹配的筛选器来调用 `xim::start_joinable_network_query()`，从而找到该网络。 下面的示例启动一个可加入网络查询，其中的游戏模式筛选器选项将只查询使用由 GAME_MODE_BRAWL 值定义的应用特定游戏模式的网络：

```cpp
xim_joinable_network_query_filters queryFilters = { 0 };
queryFilters.custom_game_mode_filter = GAME_MODE_BRAWL;

xim::start_joinable_network_query(queryFilters);
```

下面是另一个示例，它使用标记筛选器选项查询在公共可查询配置中包含 "easy" 和 "spectatorallowed" 标记的网络：

```cpp
PCWSTR tagFilters[] = { L"easy", L"spectatorallowed" };
xim_joinable_network_query_filters queryFilters = { 0 };
queryFilters.tag_filter_count = _countof(tagFilters);
queryFilters.tag_filters = tagFilters;

xim::start_joinable_network_query(queryFilters);
```

可以结合使用不同的筛选器选项。 下面的示例同时使用游戏模式筛选器选项和标记筛选器选项启动一个查询，以查询既有应用特定游戏模式常量 GAME_MODE_BRAWL 又包含 "easy" 标记的网络：

```cpp
PCWSTR tagFilters[] = { L"easy" };
xim_joinable_network_query_filters queryFilters = { 0 };
queryFilters.custom_game_mode_filter = GAME_MODE_BRAWL;
queryFilters.tag_filter_count = _countof(tagFilters);
queryFilters.tag_filters = tagFilters;

xim::start_joinable_network_query(queryFilters);
```

如果查询操作成功，应用将收到一个 `xim_start_joinable_network_query_completed_state_change`，该应用可以从中检索可加入网络的列表。 该应用将会继续接收其他可加入网络的 `xim_joinable_network_query_updated_state_change` 或是返回的可加入网络列表发生的任何更改，直到手动或自动停止查询。 可以通过调用 `xim::stop_joinable_network_query()` 手动停止进行中的查询。 当调用 `xim::start_joinable_network_query()` 以启动新查询时，查询将自动停止。

应用可以通过调用 `xim::move_to_network_using_joinable_network_information()` 尝试加入可加入网络列表中的网络。 下面的示例假定你想要加入 'selectedNetwork' 指针所指向的 `xim_joinable_network_information`（无密码保护，因此我们向第二个参数传入 nullptr）：

```cpp
xim::move_to_network_using_joinable_network_information(selectedNetwork, nullptr);
```

当通过 xim_team_configuration（声明了两个或更多团队）启用网络查询时，通过调用 `xim::move_to_network_using_joinable_network_information()` 加入的玩家将拥有默认团队索引值 0。