---
title: "使用 XIM"
author: KevinAsgari
description: "了解如何在游戏中实现 Xbox 集成多人游戏 (XIM)。"
ms.assetid: f5a2c68b-b1f9-4533-9282-41c31eab2487
ms.author: kevinasg
ms.date: 04-04-2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "Xbox live, xbox, 游戏, xbox one, xbox 集成多人游戏"
ms.openlocfilehash: 09c33d8b7c92ee8c17631de340ab0c2cb4e1c9da
ms.sourcegitcommit: 90fbdc0e25e0dff40c571d6687143dd7e16ab8a8
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/06/2017
---
# <a name="using-xim"></a>使用 XIM

这是有关 XIM 使用方面的简要演练，其中包含以下主题：

1. [先决条件](#prereq)
2. [初始化和启动](#init)
3. [异步操作和处理状态更改](#async)
4. [基本 xim_player 处理](#player)
5. [允许好友加入并邀请他们](#invites)
6. [发送和接收消息](#send)
7. [基本匹配和随他人移动到其他 XIM 网络](#basicmatch)
8. [离开 XIM 网络并清理](#leave)
9. [使用聊天](#chat)
10. [配置自定义玩家和网络属性](#properties)
11. [使用按玩家技能或角色匹配](#roles)
12. [玩家团队和配置聊天目标](#teams)
13. [玩家空位的自动后台填充（“回填”匹配）](#backfill)
14. [服务器角色（“主机迁移”）和 XIM 机构](#authority)

## <a name="prerequisites-a-nameprereq"></a>先决条件 <a name="prereq">

开始使用 XIM 编码之前，有两个先决条件。 首先，必须通过标准多人游戏网络功能配置应用的 AppXManifest，然后，必须配置其“网络清单”，以声明 XIM 使用的必要流量模式模板。

> AppXManifest 功能和网络清单在平台文档中的对应部分中进行了详细介绍；[XIM 项目配置](xim-manifest.md) 中提供了要粘贴的特定于 XIM 的典型 XML。

其次，你需要使两个应用程序标识信息可用：Xbox Live 游戏 ID 和服务配置 ID，它们会作为预配访问 Xbox Live 服务的应用程序的一部分提供。 有关获取这些信息的详细信息，请询问 Microsoft 代表。 将在初始化期间使用这些信息。

编译 XIM 需要包括 XboxIntegratedMultiplayer.h 主标头。 为了进行正确地链接，你的项目还必须在至少一个编译单元中包含 XboxIntegratedMultiplayerImpl.h（建议使用通用的预编译标头，因为这些存根功能实现很小，编译器很容易将其生成为“内联”）。

XIM 接口不需要项目在使用 C++/CX 还是传统 C++ 进行编译之间作出选择；该接口可与以上两者一起使用。 此外，该实现也不会将引发异常作为报告非致命错误的手段，因此你可以根据需要在无异常项目中轻松使用它。

## <a name="initialization-and-startup-a-nameinit"></a>初始化和启动<a name="init">

通过 Xbox Live 服务配置 ID 字符串和游戏 ID 号，你可以初始化 XIM 对象单一实例并开始与库交互。 如果还要使用 Xbox 服务 API，你可以方便地从 `xbox::services::xbox_live_app_config` 中检索。 以下示例假定值已经分别位于“myServiceConfigurationId”和“myTitleId”变量中：

```cpp
xim::singleton_instance().initialize(myServiceConfigurationId, myTitleId);
```

初始化后，应用应该为当前位于将要参与的本地设备上的所有用户检索 Xbox 用户 ID 字符串，并将其传递给 `xim::set_intended_local_xbox_user_ids()` 调用。 以下示例代码假定单个用户已经按下表示意图运行的控制器按钮，且与用户关联的 Xbox 用户 ID 字符串已经被检索到“myXuid”变量中：

```cpp
xim::singleton_instance().set_intended_local_xbox_user_ids(1, &myXuid);
```

调用 `xim::set_intended_local_xbox_user_ids()` 会立即设置与应添加到 XIM 网络中的本地用户关联的 Xbox 用户 ID。 以后的所有网络操作都将使用此 Xbox 用户 ID 列表，直到通过其他对 `xim::set_intended_local_xbox_user_ids()` 的调用更改列表。

在这种情况下，不会存在任何 XIM 网络，因此你必须先移动到 XIM 网络才能开始该过程。 如果用户没有记住具体的 XIM 网络，最佳做法是，只需移动到一个新的空网络，使用户的好友可以加入此网络并将其作为某种“大厅”，在“大厅”中，他们可以进行协作，选择下一个多人游戏活动（例如，一起进入匹配）。 开始仅移动之前添加到这种空 XIM 网络（拥有最多能容纳共 8 位玩家的空间）中的本地用户的示例是：

```cpp
xim::singleton_instance().move_to_new_network(8, xim_players_to_move::bring_only_local_players);
```
现在会开始异步移动操作，你可以通过定期处理状态更改了解其最终结果。

## <a name="asynchronous-operations-and-processing-state-changes-a-nameasync"></a>异步操作和处理状态更改 <a name="async">

XIM 的核心是应用对方法的 `xim::start_processing_state_changes()` 和 `xim::finish_processing_state_changes()` 对的定期且频繁的调用。 这些方法是，如何通知 XIM 应用已准备好处理多人游戏状态的更新，以及 XIM 如何提供这些更新。 它们被设计为可快速操作，以便可以在 UI 呈现循环中的每个图形帧调用它们。 这提供了一个方便的位置来检索所有已排队更改，而不必担心网络计时的不可预测性或多线程回调的复杂性。 XIM API 确实针对此单线程模式进行了优化。 它保证其状态在这两个函数之外将保持不变，因此，你可以直接、高效地使用它。

出于相同的原因，*不*应认为 XIM API 返回的所有对象都是线程安全的。 库有内部多线程保护，但是，如果你需要一个线程访问任意值（例如，浏览 `xim::players()` 列表），而另一线程可能在调用 `xim::start_processing_state_changes()` 或 `xim::finish_processing_state_changes()`，且正在更改与玩家列表关联的内存，则你仍然需要实现自行锁定。

调用 `xim::start_processing_state_changes()` 时，会在 `xim_state_change` 结构指针数组中报告所有已排队更新。 应用应迭代该数组，检查基本结构的更具体的类型，将基本结构类型转换为相应的更详细的类型，然后根据需要处理更新。 完成所有当前可用的 `xim_state_change` 结构后，应通过调用 `xim::finish_processing_state_changes()` 将该数组传递回 XIM 以释放资源。 例如：

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

既然你有了基本处理循环，便可以处理与初始 `xim::move_to_new_network()` 操作关联的状态更改。 每个 XIM 网络移动操作都会从 `xim_move_to_network_starting_state_change` 开始。 如果出于任何原因，移动失败，则会向应用提供 `xim_network_exited_state_change`，对于阻止你移动到 XIM 网络或将你与当前 XIM 网络断开连接的任何异步致命错误而言，这是常见的故障处理机制。 否则，将在完成所有状态，并将所有玩家成功添加到 XIM 网络后，使用 `xim_move_to_network_succeeded_state_change` 完成移动。

## <a name="basic-ximplayer-handling-a-nameplayer"></a>基本 xim_player 处理 <a name="player">

假定将单个本地用户移动到新的 XIM 网络的示例成功了，还为应用提供了本地 `xim_player` 对象的 `xim_player_joined_state_change`。 只要玩家实例本身有效（直到通过 `xim::finish_processing_state_changes()` 提供和返回其相应的 `xim_player_left_state_change` 为止），此对象指针就会保持有效。 始终会为应用提供每个 `xim_player_joined_state_change` 的 `xim_player_left_state_change`。 你还可以使用 `xim::get_players()` 随时检索 XIM 网络中所有 `xim_player` 对象的数组。

`xim_player` 对象有很多有用的方法，例如，`xim_player::gamertag()` 可检索与玩家关联的当前 Xbox Live 玩家代号字符串，以用于显示。 如果 `xim_player` 位于设备本地，则它将报告 `xim_player::local()` 中的非 null `xim_player::xim_local` 对象指针，其中有其他仅适用于本地玩家的方法。

当然，对玩家而言，最重要的状态不是 XIM 知道的通用信息，而是你的特定应用要跟踪的信息，而且，由于你可能有自己要跟踪的信息，因此，你可能想将 `xim_player` 对象链接到你要跟踪的信息，这样，每当 XIM 报告 `xim_player` 时，你都可以快速获取你的状态，而无需通过设置自定义玩家上下文指针执行查找。 以下示例假定你的隐私状态指针已经在变量“myPlayerStateObject”中，且新添加的 `xim_player` 对象已经在变量“newXimPlayer”中：

```cpp
newXimPlayer->set_custom_player_context(myPlayerStateObject);
```

这会将带玩家对象的指定指针值保存到本地（它绝不会通过网络传输到远程设备，远程设备中的内存无效）。 通过检索自定义上下文并将其重新转换为对象，你将能够始终返回你的对象，如以下示例所示：

```cpp
myPlayerStateObject = reinterpret_cast<MyPlayerState *>(newXimPlayer->custom_player_context());
```

你可以随时更改此自定义玩家上下文指针。

通过此基本玩家处理，你现在已经准备好允许远程用户通过与本地用户的现有社交关系加入此 XIM 网络。

## <a name="enabling-friends-to-join-and-inviting-thema-nameinvites"></a>允许好友加入并邀请他们<a name="invites">

出于隐私和安全性目的，默认情况下，所有新 XIM 网络都会自动配置为任何其他玩家都不可加入，就绪后，由应用决定是否明确允许其他玩家加入。 以下示例演示如何使用 xim::set_allowed_player_joins() 允许新的本地用户，以及其他已经收到邀请或受到“关注”（一种 Xbox Live 社交关系）的用户以玩家身份加入：

```cpp
xim::singleton_instance().set_allowed_player_joins(xim_allowed_player_joins::local_invited_or_followed);
```

这会以异步形式发生。 完成后会提供 `xim_allowed_player_joins_changed_state_change`，以通知你其值已发生更改，不再是默认值 `xim_allowed_player_joins::none`。 你可以使用 `xim::allowed_player_joins()`，在稍后或任意时间查询新值。

现在，本地玩家可能想要向远程用户发送加入此 XIM 网络的邀请。 通常情况下，会通过调用 `xim_player::xim_local::show_invite_ui()` 以启动本地用户可以选择用户并发送邀请的系统邀请 UI 完成此操作。 以下示例演示了此操作，假定变量“ximPlayer”指向有效的本地 `xim_player`：

```cpp
ximPlayer->local()->show_invite_ui();
```

现在将显示系统邀请 UI，当用户发送邀请（或取消 UI）后，会提供 `xim_show_invite_ui_completed_state_change`。 或者，应用可以使用 `xim_player::xim_local::invite_users()` 直接发送邀请。 无论使用哪种方式，远程用户都会在登录时收到 Xbox Live 邀请消息，并可以选择接受。 如果应用尚未运行，你可以在这些设备上启动应用，并使用可用于移动到同一个 XIM 网络的事件参数将该应用“协议激活”。 请参阅平台文档，获取有关激活本身的详细信息。 以下示例显示，假定你已经将 `Windows::ApplicationModel::Activation::ProtocolActivatedEventArgs` 中的原始 URI 字符串检索到变量“uriString”中，则应如何获取事件参数并调用 `xim::extract_protocol_activation_information()`，以确定其是否适用于 XIM：

```cpp
xim_protocol_activation_information activationInfo;
bool isXimActivation;
isXimActivation = xim::singleton_instance().extract_protocol_activation_information(uriString, &activationInfo);

```

如果是 XIM 激活，则需要确保填充的 `xim_protocol_activation_information` 结构的“local_xbox_user_id”字段中标识的本地用户已登录，并在指定给 `xim::set_intended_local_xbox_user_ids()` 的用户中。 然后，你可以使用相同的 URI 字符串，通过调用 `xim::move_to_network_using_protocol_activated_event_args()` 来开始移动到指定的 XIM 网络中。 例如：

```cpp
xim::singleton_instance().move_to_network_using_protocol_activated_event_args(uriString);
```

另请注意，受到“关注”的远程用户可以导航到系统 UI 中的本地用户玩家卡片并自行启动加入尝试，无需邀请（假定你已经允许此类玩家加入，如上所述）。 这会协议激活你的应用（和邀请一样），且无需用不同的方式处理。

使用协议激活移动到 XIM 网络和之前完成的移动到新 XIM 网络相同。 唯一的区别是，移动成功后，会向移动的设备提供表示适用玩家的本地和远程玩家 `xim_player_joined_state_change` 结构。 当然，XIM 网络中已经存在的设备不会移动，但会看到新设备的用户被添加为具有其他 `xim_player_joined_state_change` 结构的玩家。


此时，会自动为此 XIM 网络中的这些不同设备上的玩家启用语音和文字聊天通信。 你现在已经完全准备好进行多人游戏和发送想要发送的任何特定于应用的消息。

## <a name="sending-and-receiving-messages-a-namesend"></a>发送和接收消息 <a name="send">

XIM 及其基础组件会执行建立安全的 Internet 通信渠道的所有繁琐操作，因此你无需担心连接问题或只能访问部分玩家。 如果有任何基本对等连接问题，将无法成功移动到 XIM 网络。 否则，你可以确定，会向应用的所有实例和所有设备告知每个 `xim_player`，并可以向每个玩家发送消息。 以下示例将“sendingPlayer”变量假定为指向有效的本地玩家对象的指针，并通过有保证的连续传递向 XIM 网络中的所有本地或远程玩家发送消息结构“msgData”（通过不传递特定玩家的数组）：

```cpp
sendingPlayer->local()->send_data_to_other_players(sizeof(msgData), &msgData, 0, nullptr, xim_send_type::guaranteed_and_sequential);
```

会向所有消息收件人提供包括指向数据副本的指针的 xim_player_to_player_data_received_state_change，以及指向发送和本地接收消息的 xim_player 对象的指针。

当然，有保证的连续传递很方便，但它也有可能成为低效率的发送类型，因为，如果 Internet 丢失了数据包/打乱了数据包的顺序，则 XIM 需要将其重新传递或延迟。 请务必考虑使用应用可容忍丢失或乱序到达的其他消息发送类型。

由于消息数据来自远程计算机，因此，最佳做法是，明确定义数据格式（例如按特定字节顺序（“字节顺序”）打包多字节值），并在操作前进行验证。 XIM 提供网络级别的安全性，因此你不应实现任何其他加密或签名方案，但是，始终明智的做法是坚持“深度防护”，以便防止意外的应用程序 bug，或处理正常共存的不同版本的应用程序协议（在开发、内容更新等期间）。

用户的 Internet 连接还是一个受限且不断变化的资源。 请务必使用高效的消息数据格式，并避免采用会发送每个 UI 框架的设计。 你可以通过调用 `xim_player::network_path_information()` 方法，了解有关两个玩家之间的当前路径质量的详细信息。 以下示例会将指针检索到“remotePlayer”变量中包含的 `xim_player` 指针的 `xim_network_path_information` 结构：

```cpp
 const xim_network_path_information * networkPathInfo = remotePlayer->network_path_information();
```

由于连接此时无法支持传输更多数据，因此，返回的结构包括预计的往返行程延迟和仍在本地排队的消息数量。 如果发现队列正在备份，你应该降低发送数据的速率。

## <a name="basic-matchmaking-and-moving-to-another-xim-network-with-others-a-namebasicmatch"></a>基本匹配和随他人移动到其他 XIM 网络 <a name="basicmatch">

通过将玩家移动到有陌生人（基于同样的兴趣，通过 Xbox Live 匹配服务汇聚到一起的来自全世界的对手）的 XIM 网络，你可以进一步展开好友组的体验。 最基本的形式是在一台具有填充的 `xim_matchmaking_configuration` 结构的设备上调用 `xim::move_to_network_using_matchmaking()`，并随之带上当前 XIM 网络中的玩家。 以下示例通过使用配置为无团队自由竞赛寻找共 8 个玩家（如果找不到 8 个玩家，2-7 个玩家也可接受）的匹配启动移动，方法是，使用特定于应用的游戏模式常量 uint64_t（该常量由仅与指定相同值的其他玩家匹配的值 MYGAMEMODE_DEATHMATCH 定义），并将当前 XIM 网络中的所有以社交方式加入的玩家提出:

```cpp
xim_matchmaking_configuration matchmakingConfiguration = { 0 };
matchmakingConfiguration.team_matchmaking_mode = xim_team_matchmaking_mode::no_teams_8_players_minimum_2;
matchmakingConfiguration.custom_game_mode = MYGAMEMODE_DEATHMATCH;

xim::singleton_instance().move_to_network_using_matchmaking(matchmakingConfiguration, xim_players_to_move::bring_existing_social_players);
```

和之前的移动一样，这会在所有设备上提供初始 `xim_move_to_network_starting_state_change`，并在成功完成移动后提供 `xim_move_to_network_succeeded_state_change`。 由于这是从一个 XIM 网络到另一个 XIM 网络的移动，因此，其中一个区别是，已经存在为本地和远程用户添加的现有 `xim_player` 对象，且会为所有一起移动到新 XIM 网络的玩家保留这些对象。 进行匹配时（这可能是一个漫长的过程，具体取决于匹配池中调用了 `xim::move_to_network_using_matchmaking()` 的潜在玩家数量），这些玩家之间的聊天和数据通信将继续工作，不会中断。 整个操作过程中将定期提供 `xim_matchmaking_progress_updated_state_change`，使你和你的用户了解当前状态。 发现匹配时，会将其他玩家添加到具有典型 `xim_player_joined_state_change` 的 XIM 网络中，然后，移动完成。

完成与这组“匹配”玩家的多人游戏体验后，你可以通过另一轮匹配重复移动到其他 XIM 网络的过程。 你会看到通过之前的 `xim::move_to_network_using_matchmaking()` 操作加入的用户提供 `xim_player_left_state_change`，以指示其 `xim_player` 对象不再处于同一 XIM 网络中，发生新匹配时（假设你再次指定 `xim_players_to_move::bring_existing_social_players`；指定 `xim_players_to_move::bring_only_local_players` 甚至会断开与远程玩家的连接，只保留本地玩家），只会保留通过社交方式、`xim::move_to_network_using_protocol_activated_event_args()` 或 `xim::move_to_network_using_joinable_xbox_user_id()` 加入的玩家。 第二次移动操作完成后，将添加另一组陌生人。

或者，在确定下一个匹配配置/多人游戏活动之前，你可以移动到只有非匹配玩家（或只有本地玩家）的全新 XIM 网络。 以下示例演示了让设备再次为拥有最多 8 个玩家的 XIM 网络调用 `xim::move_to_new_network()`，但是，这次也要包含以社交方式加入的现有玩家：

```cpp
xim::singleton_instance().move_to_new_network(8, xim_players_to_move::bring_existing_social_players);
```

将为所有参与的设备提供 `xim_move_to_network_starting_state_change` 和 `xim_move_to_network_succeeded_state_change`，并为后台运行的匹配玩家（即同样看到每个正在移动的玩家的 `xim_player_left_state_change` 的设备）提供 `xim_player_left_state_change`。

你可以根据需要，多次以这种方式持续从 XIM 网络移动到使用（或不使用）匹配的 XIM 网络。

出于性能原因，Xbox Live 服务不会尝试匹配不太可能建立任何直接对等连接的设备上的玩家组。 如果你在未正确配置为支持标准 Xbox Live 多人游戏的网络环境中开发，则 `xim::move_to_network_using_matchmaking()` 操作可能会无限期继续，而不进行匹配，即使你确定有足够满足匹配条件的玩家，这些玩家都在移动，且都在使用同一本地环境中的设备。 请务必在网络设置区域/Xbox 应用程序中运行多人游戏连接测试，并在其报告问题（尤其是关于“严格 NAT”）时遵循它的建议。 但是，如果网络管理员无法进行必要的环境更改，通过将 XIM 配置为允许在没有“开放 NAT”设备的情况下匹配“严格 NAT”设备，你可以取消阻止 Xbox One 开发工具包上的匹配测试。 可通过在所有 Xbox One 主机上的“游戏草稿”的根目录下放置名为“xim_disable_matchmaking_nat_rule”（内容不重要）的文件完成此操作。 示例操作方法是，在启动应用前，从 XDK 命令提示符执行以下内容，根据需要，替换每个控制台的“{console_name_or_ip_address}”占位符：

```bat

echo.>%TEMP%\emptyfile.txt
copy %TEMP%\emptyfile.txt \\{console_name_or_ip_address}\TitleScratch\xim_disable_matchmaking_nat_rule
del %TEMP%\emptyfile.txt

```

此开发解决方法当前仅适用于 Xbox One 独占资源应用程序，不适用于通用 Windows 应用程序。 另请注意，无论网络环境如何，使用此设置的控制台都不会与不存在此文件的设备匹配，因此，请务必随时随地添加或删除此文件。

## <a name="leaving-a-xim-network-and-cleaning-up-a-nameleave"></a>离开 XIM 网络并清理 <a name="leave">

本地用户参与 XIM 网络后，他们通常只会重新移动到新 XIM 网络中，此网络允许本地用户、邀请用户和受到“关注”的用户加入，使他们能继续与其好友协作，查找下一个活动。 但是，如果用户完全完成了所有多人游戏体验，则你的应用可能要开始完全离开 XIM 网络，然后返回就像仅调用了 `xim::initialize()` 和 `xim::set_intended_local_xbox_user_ids()` 的状态。 此操作使用 `xim::leave_network()` 方法完成：

```cpp
xim::singleton_instance().leave_network();
```

此方法会开始顺利地异步断开与其他参与者的连接的过程。 这将导致向远程设备提供本地玩家的 `xim_player_left_state_change`，并将向本地设备提供各个玩家的 `xim_player_left_state_change`（本地或远程）。 完成所有断开连接操作后，将提供最终 `xim_network_exited_state_change`。 然后，该应用可调用 `xim::cleanup()``，以释放所有资源并返回到未初始化状态：

```cpp
xim::singleton_instance().cleanup();
```

当尚未提供 `xim_network_exited_state_change` 时，始终强烈建议调用 `xim::leave_network()` 并等待 `xim_network_exited_state_change` 以便顺利退出 XIM 网络。 当强制剩余参与者向只是“消失”的设备超时发送消息时，直接调用 `xim::cleanup()` 可能会导致剩余参与者的通信性能问题。

## <a name="working-with-chat-a-namechat"></a>使用聊天 <a name="chat">

会为 XIM 网络中的玩家自动启用语音和文字聊天通信。 XIM 会为你处理与所有语音耳机和麦克风硬件的交互。 你的应用不需要为聊天执行过多操作，但有一个有关文字聊天的要求：支持输入和显示。 需要文本输入是因为，即使在过去没有广泛使用过物理键盘的平台和游戏类型中，玩家也可以配置系统以使用文本到语音转换辅助技术。 同样，需要文本显示是因为，玩家可以配置系统以使用语音到文本转换。 通过分别调用 `xim_player::xim_local::chat_text_to_speech_conversion_preference_enabled()` 和 `xim_player::xim_local::chat_speech_to_text_conversion_preference_enabled()` 方法可以在本地玩家中检测到这些首选项，并且，你可能想要有条件地启用文本机制。 但是，请考虑进行文本输入，并显示始终可用的选项。


> `Windows::Xbox::UI::Accessability`  是专门为提供游戏内文字聊天的简单呈现而设计的 Xbox One 类，专注于语音到文本转换辅助技术。

获得由真实或虚拟键盘提供的文本输入后，请将字符串传递给 `xim_player::xim_local::send_chat_text()` 方法。 以下代码显示了从变量“localPlayer”指向的本地 `xim_player` 对象发送示例硬编码字符串：

```cpp
localPlayer->local()->send_chat_text(L"Example chat text");
```

此聊天文本会传递给能够从原始本地玩家接收聊天通信的 XIM 网络中的所有玩家。 它可能会合成为语音音频，并且可能会以 `xim_chat_text_received_state_change` 的形式提供。 你的应用应该对收到的任意文本字符串进行复制，并将它与原始玩家的某些标识一起显示适当时间（或在可滚动窗口中显示）。

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

例如，由 `xim_player::chat_indicator()` 报告的值预计会在玩家开始和停止说话时频繁改变。 它旨在支持因此而在每个 UI 框架对其轮询的应用。

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


## <a name="configuring-custom-player-and-network-properties-a-nameproperties"></a>配置自定义玩家和网络属性<a name="properties">

大多数应用数据交易都会通过 `xim_player::xim_local::send_data_to_other_players()` 方法发生，因为它允许对接收交换的用户和时间、如何处理数据包丢失等问题进行最大程度的控制。 但是，有时，能让玩家很容易地将关于自身的基本、很少更改的状态与他人分享是一个不错的选择。 例如，在进入所有玩家用来呈现其游戏内表示形式的多人游戏之前，每个玩家可能都有表示所选的人物模型的固定字符串。 XIM 会提供针对应用定义的名称和 null 值终止的字符串对的“自定义玩家属性”便利功能，此功能可适用于本地玩家，并在其更改时随时自动传播给所有设备。 当它们加入 XIM 网络并查看添加的玩家时，还会自动向新参与的设备提供它们的当前值。 可以通过调用具有名称和值字符串的 `xim_player::xim_local::set_player_custom_property()` 来配置这些内容，例如，以下示例设置了名为“模型”的属性，以使“暴力”值位于变量“localPlayer”指向的本地 `xim_player` 对象上：

```cpp
localPlayer->local()->set_player_custom_property(L"model", L"brute");
```

更改玩家属性会导致为所有设备提供 `xim_player_custom_properties_changed_state_change`，以警告它们属性的名称已发生更改。 可使用 `xim_player::get_player_custom_property()` 在任意本地或远程玩家上检索给定名称的值。 以下示例检索了变量“ximPlayer”指向的 `xim_player` 中名为“模型”的属性的值：

```cpp
PCWSTR modelName = ximPlayer->get_player_custom_property(L"model");
```

为给定的属性名称设置新值将替换任意现有值，并将 null 值字符串指针与空值字符串同等对待，即与尚未受指定的属性相同的对待。 否则，XIM 不会解释名称和值；由应用根据需要决定是否验证字符串内容。

还会通过“自定义属性”将此便利功能作为一个整体提供给 XIM 网络。 这些对自定义玩家属性同样起作用，只不过它们是使用 `xim::set_network_custom_property()` 在 XIM 单一实例对象上设置的。 以下示例设置了“地图”属性，使其拥有“要塞”一值:

```cpp
xim::singleton_instance().set_network_custom_property(L"map", L"stronghold");
```

更改网络属性会导致为所有设备提供 `xim_network_custom_properties_changed_state_change`，以警告它们属性的名称已发生更改。 可使用 `xim::get_network_custom_property()` 检索给定名称的值，例如，以下示例检索了名为“地图”的属性的值：

```cpp
PCWSTR mapName = xim::singleton_instance().get_network_custom_property(L"map");
```
和自定义玩家属性一样，设置给定的自定义网络属性名称的值会替换现有值，且 null 值、未设置值或已清除值会始终受到同等对待：即与非 null 字符串相同的对待。

从一个 XIM 网络移动到另一个时，会始终重设自定义玩家属性，且新建 XIM 网络会始终从未设置属性开始。 但是，加入现有 XIM 网络的新玩家将看到对现有玩家和 XIM 网络本身设置的自定义属性。

自定义玩家和网络属性旨在为不会频繁更改的状态提供便利。 与 `xim_player::xim_local::send_data_to_other_players()` 方法相比，它们的内部同步开销更多，因此，对于玩家位置等会快速替换的状态而言，你应该仍然改用直接发送。

## <a name="matchmaking-using-per-player-skill-or-role-a-nameroles"></a>使用按玩家技能或角色匹配<a name="roles">

在某个应用指定的游戏模式下，按共同兴趣匹配玩家是一个不错的基本策略。 当可用玩家池扩大时，你还应该考虑基于玩家的个人技能或游戏体验匹配玩家，以便资深玩家可以享受与其他资深玩家的良性竞争带来的挑战，而新玩家可以通过与能力相似的玩家进行竞争来获得成长。 要执行此操作，在使用匹配开始移动到 XIM 网络前，请先在对 `xim_player::xim_local::set_matchmaking_configuration()` 的调用中指定的按玩家匹配配置结构中提供所有本地玩家的技能等级。 技术等级是特定于应用的概念，且 XIM 不会解释数字，不过，匹配会首先尝试查找具有相同技能值的玩家，然后定期以 +/- 10 的增量扩大搜索范围，以尝试查找其他在此技能附近范围内声明技能值的玩家。 以下示例假定本地 `xim_player` 对象（其指针为“localPlayer”）有一个关联的特定于应用的 uint32_t 技能值，该值从本地或 Xbox Live 存储检索到名为“playerSkillValue”的变量中：

```cpp

 xim_player_matchmaking_configuration playerMatchmakingConfiguration = { 0 };
 playerMatchmakingConfiguration.skill = playerSkillValue;

 localPlayer->local()->set_matchmaking_configuration(&playerMatchmakingConfiguration);
```

此操作完成时，会向所有参与者提供 `xim_player_matchmaking_configuration_changed_state_change`，以指示此 `xim_player` 已更改其按玩家匹配配置。 可通过调用 `xim_player::matchmaking_configuration()` 检索新值。

当所有玩家都应用非 null 匹配配置时，你可以使用匹配将其移动到 XIM 网络，并为 `xim::move_to_network_using_matchmaking()` 指定 `xim_matchmaking_configuration` 结构的 require_player_matchmaking_configuration 字段中的 true 值。 以下示例使用由 MYGAMEMODE_DEATHMATCH 值（该值仅与指定相同值、或需要按玩家匹配配置的其他玩家匹配）定义的特定于应用的游戏模式常量 uint64_t 填充匹配配置，此配置将为无团队自由竞赛寻找共 2-8 个玩家：

```cpp
xim_matchmaking_configuration matchmakingConfiguration = { 0 };
matchmakingConfiguration.team_matchmaking_mode = xim_team_matchmaking_mode::no_teams_8_players_minimum_2;
matchmakingConfiguration.custom_game_mode = MYGAMEMODE_DEATHMATCH;
matchmakingConfiguration.require_player_matchmaking_configuration = true;
```

为 `xim::move_to_network_using_matchmaking()` 提供此结构时，只要移动的玩家已通过非 null `xim_player_matchmaking_configuration` 指针调用 `xim_player::xim_local::set_matchmaking_configuration()`，移动操作将正常开始。 如果任意玩家未执行此操作，则会暂停匹配过程，并为所有参与者提供值为 `xim_matchmaking_status::waiting_for_player_matchmaking_configuration` 的 `xim_matchmaking_progress_updated_state_change`。 这包括在匹配完成后，通过以前发送的邀请或通过其他社交方式（例如，调用 `xim::move_to_network_using_joinable_xbox_user_id`）加入 XIM 网络的玩家。 所有玩家都提供其 `xim_player_matchmaking_configuration` 结构后，匹配将会继续。

另一个使用按玩家匹配配置改进用户的匹配体验的方法是，使用所需的玩家角色。 这最适合于提供可选择的人物类型的游戏，这种类型可鼓励不同的合作游戏风格；即，这种类型不是仅更改游戏内的图形表示，而是控制互补、有影响力的特性（例如，防御性“医师”与近距离“格斗”与远“距离”攻击支持）。 用户的个性意味着他们可能想要以某个特长进行游戏。 但是，如果游戏设计为当没有至少一个履行每个角色的用户时，则在功能上不可能完成目标，有时，最好先将这些玩家匹配到一起，再将任意玩家匹配到一起，并在集合后要求其协商他们之间的游戏风格。 你可以通过先定义表示要在给定玩家的 `xim_player_matchmaking_configuration` 结构中指定的每个角色的唯一位标志来执行此操作。 以下示例为本地 `xim_player` 对象（其指针为“localPlayer”）设置了特定于应用的 MYROLEBITFLAG_HEALER uint8_t 角色值：

```cpp

xim_player_matchmaking_configuration playerMatchmakingConfiguration = { 0 };
playerMatchmakingConfiguration.roles = MYROLEBITFLAG_HEALER;

localPlayer->local()->set_matchmaking_configuration(&playerMatchmakingConfiguration);

```

将为所有参与者提供此玩家对上述技能的 `xim_player_matchmaking_configuration_changed_state_change`，如前所述。 然后，指定给 `xim::move_to_network_using_matchmaking()` 的全局 `xim_matchmaking_configuration` 结构应使用按位 OR 和 require_player_matchmaking_configuration 字段的 true 值合并所有需要的角色标记。 以下示例使用由值 MYGAMEMODE_COOPERATIVE 定义的特定于应用的游戏模式常量 uint64_t 填充将为无团队自由竞赛寻找共 3 名玩家的匹配配置，该值仅与指定相同值的其他玩家匹配，并需要按游戏匹配配置，这种配置需要至少一个履行全部三个特定于应用的 uint8_t 角色位标志（MYROLEBITFLAG_HEALER、MYROLEBITFLAG_MELEE 和 MYROLEBITFLAG_RANGE）的玩家：

```cpp
xim_matchmaking_configuration matchmakingConfiguration = { 0 };
matchmakingConfiguration.team_matchmaking_mode = xim_team_matchmaking_mode::no_teams_3_players_minimum_3;
matchmakingConfiguration.custom_game_mode = MYGAMEMODE_COOPERATIVE;
matchmakingConfiguration.required_roles = MYROLEBITFLAG_HEALER | MYROLEBITFLAG_MELEE | MYROLEBITFLAG_RANGE;
matchmakingConfiguration.require_player_matchmaking_configuration = true;
```

向 `xim::move_to_network_using_matchmaking()` 提供此结构时，将开始移动操作，如上所述。

技能和角色可以一起使用。 如果只需要其中一个，可将另一个的值指定为 0。 这是因为，所有声明其 `xim_player_matchmaking_configuration` 技能值为 0 的玩家始终互相匹配，如果 `xim_matchmaking_configuration` required_roles 字段中没有非零位，则不需要角色位就能匹配。

`xim::move_to_network_using_matchmaking()` 或其他 XIM 网络移动操作完成后，将把所有玩家的 `xim_player_matchmaking_configuration` 结构自动清理为 null 指针（附带 `xim_player_matchmaking_configuration_changed_state_change` 通知）。 如果你计划使用需要按玩家配置的匹配移动到另一个 XIM 网络，则你需要通过一个包含最新信息的新结构指针再次调用 `xim_player::xim_local::set_matchmaking_configuration()`。


## <a name="player-teams-and-configuring-chat-targets-a-nameteams"></a>玩家团队和配置聊天目标<a name="teams">

多人游戏通常涉及到组织到对方团队的玩家。 通过使用在指定配置中请求两个或以上团队的 `xim_team_matchmaking_mode` 值，XIM 可轻松地在匹配时分配团队。 以下示例通过使用经过配置以寻找共 8 个玩家来编入两个 4 人团队（如果找不到 4 个玩家，1-3 个玩家也可接受）的匹配发起移动，方法是，使用特定于应用的游戏模式常量 uint64_t（该常量由仅与指定相同值的其他玩家匹配的值 MYGAMEMODE_CAPTURETHEFLAG 定义），并将当前 XIM 网络中的所有以社交方式加入的玩家聚到一起：

```cpp
xim_matchmaking_configuration matchmakingConfiguration = { 0 };
matchmakingConfiguration.team_matchmaking_mode = two_teams_4v4_minimum_1_per_team;
matchmakingConfiguration.custom_game_mode = MYGAMEMODE_CAPTURETHEFLAG;

xim::singleton_instance().move_to_network_using_matchmaking(matchmakingConfiguration, xim_players_to_move::bring_existing_social_players);
```

完成此 XIM 网络移动操作后，将通过 {n}（与请求的 {n} 个团队对应）为玩家分配团队索引值 1。 通过 `xim_player::team_index()` 检索玩家的团队索引值。 以下示例检索 xim_player 对象（其指针位于“ximPlayer”变量中）的团队索引：

```cpp
uint8_t playerTeamIndex = ximPlayer->team_index();
```

对于首选的用户体验（更不用说负面玩家行为的机会减少），Xbox Live 匹配服务绝不会将一起移动到 XIM 网络的玩家拆分到不同的团队中。

最初通过匹配分配的团队索引值只是建议，应用可以使用 `xim_player::xim_local::set_team_index()` 随时为本地玩家进行更改。 还可以在完全不使用匹配的 XIM 网络中调用此值。 以下示例配置了一个玩家指针“localPlayer”，以获得新团队索引值 1：

```cpp
localPlayer->local()->set_team_index(1);
```

为所有设备提供此玩家的 `xim_player_team_index_changed_state_change` 时，会告知其玩家有一个有效的新团队索引值。

将 `xim_team_matchmaking_mode` 与两个或以上团队一起使用时，绝不会通过调用 `xim::move_to_network_using_matchmaking()` 来为玩家分配团队索引值 0。 这与通过任意其他移动操作配置或类型（例如通过接受邀请导致的协议激活）添加到 XIM 网络的玩家相反，他们将始终拥有 0 团队索引。 它可能有助于将团队索引 0 视为特殊的“未分配”团队。

任何特定团队索引值的真实意义都取决于应用。 XIM 不会解释索引值，与聊天目标配置有关的相等比较除外。 如果由 `xim::chat_targets()` 报告的聊天目标配置当前 `xim_chat_targets::same_team_index_only`，则任意给定玩家将仅与另一个玩家交换聊天通信，前提是两个玩家有相同的由 `xim_player::team_index()` 报告的值（且隐私/策略也允许）。

为保持谨慎并支持竞争方案，会将新建的 XIM 网络配置为默认为 `xim_chat_targets::same_team_index_only`。 但是，可能需要与其他团队中的战败对手聊天，例如，在游戏后的“大厅”中。 通过调用 `xim::set_chat_targets()`，你可以指示 XIM 允许所有人在隐私和策略允许的地方与其他任何人交谈。 以下示例开始配置 XIM 网络中的所有参与者，以使用 `xim_chat_targets::all_players` 值：

```cpp
xim::singleton_instance().set_chat_targets(xim_chat_targets::all_players);
```

向所有参与者提供 `xim_chat_targets_changed_state_change` 时，会告知他们有一个有效的新目标设置。

如之前所述，大多数 XIM 网络移动类型最初都会为所有玩家分配团队索引值 0。 这意味着，默认情况下，`xim_chat_targets::same_team_index_only` 的配置可能与 `xim_chat_targets::all_players` 无法区分。 但是，如果匹配配置的 `xim_team_matchmaking_mode` 值声明两个或以上团队，则使用匹配移动到 XIM 网络的玩家可能有不同的团队索引值。 你还可以随时调用 `xim_player::xim_local::set_team_index()`，如上所示。 如果你的应用通过以下任一方法使用非零团队索引值，请不要忘记相应地管理当前聊天目标设置。

匹配会独立于团队评估所需的按玩家角色。 因此，不建议将团队和所需的角色用作同时匹配配置标准，因为团队是通过玩家计数（而不是履行的玩家角色）进行平衡的。

## <a name="automatic-background-filling-of-player-slots-backfill-matchmaking-a-namebackfill"></a>玩家空位的自动后台填充（“回填”匹配）<a name="backfill">

同时调用 `xim::move_to_network_using_matchmaking()` 的不同玩家组会为 Xbox Live 匹配服务提供最大的灵活性，以便将玩家快速整理到最优的新 XIM 网络。 但是，某些游戏场景想要将某个 XIM 网络保持原样，只匹配其他玩家，仅仅是为了填充空白玩家空位。 XIM 支持使用 `xim::set_backfill_matchmaking_configuration()` 方法配置要在自动后台填充模式（或“回填”）下操作的匹配。 以下示例使用特定于应用的游戏模式常量 uint64_t 配置了回填匹配，以尝试为无团队自由竞赛寻找共 8 个玩家（如果找不到 8 个玩家，2-7 个玩家也可接受），该常量由仅与指定相同值的其他玩家匹配的值 MYGAMEMODE_DEATHMATCH 定义：

```cpp
 xim_matchmaking_configuration matchmakingConfiguration = { 0 };
 matchmakingConfiguration.team_matchmaking_mode = xim_team_matchmaking_mode::no_teams_8_players_minimum_2;
 matchmakingConfiguration.custom_game_mode = MYGAMEMODE_DEATHMATCH;

 xim::singleton_instance().set_backfill_matchmaking_configuration(&matchmakingConfiguration);
```

这会使现有 XIM 网络适用于以正常方式调用 `xim::move_to_network_using_matchmaking()` 的设备。 这些设备不会看到任何行为更改。 回填 XIM 网络中的参与者不会移动，但会向其提供表明回填打开的 `xim_backfill_matchmaking_configuration_changed_state_change`，以及多个 `xim_matchmaking_progress_updated_state_change` 通知（如果适用）。 将使用正常的 `xim_player_joined_state_change` 将所有匹配的玩家添加到 XIM 网络。

回填匹配会保持无限期进行，虽然它不会在 XIM 网络已经有由 `xim_team_matchmaking_mode` 值指定的最大玩家数时尝试添加玩家。 通过使用 null 指针再次调用 `xim::set_backfill_matchmaking_configuration()` 可禁用回填：

```cpp
 xim::singleton_instance().set_backfill_matchmaking_configuration(nullptr);
```

将向所有设备提供相应的 `xim_backfill_matchmaking_configuration_changed_state_change`，并且，完成此异步过程后，将提供最终 `xim_matchmaking_progress_updated_state_change` 和 `xim_matchmaking_status::none`，表明不会再向 XIM 网络提供任何匹配的玩家。

当通过声明两个或以上团队的 `xim_team_matchmaking_mode` 值启用回填匹配时，所有现有玩家的有效团队索引必须在 1 到团队数之间。 这包括调用 `xim_player::xim_local::set_team_index()` 来指定自定义值，或使用邀请或通过其他社交方式（例如，调用 `xim::move_to_network_using_joinable_xbox_user_id`）加入，并通过默认团队索引值 0 添加的玩家。 如果任意玩家没有有效的团队索引，则会暂停匹配过程，并为所有参与者提供值为 `xim_matchmaking_status::waiting_for_player_team_index` 的 `xim_matchmaking_progress_updated_state_change`。 在所有玩家通过 `xim_player::xim_local::set_team_index()` 提供或更正其团队索引值后，回填匹配将继续。

同样，当通过 `xim_matchmaking_configuration` 结构启用回填匹配，并将角色或技能的 require_player_matchmaking_configuration 字段设置为 true 时，所有玩家必须指定非 null 按玩家匹配配置。 如果任意玩家未执行此操作，则会暂停匹配过程，并为所有参与者提供值为 `xim_matchmaking_status::waiting_for_player_matchmaking_configuration` 的 `xim_matchmaking_progress_updated_state_change`。 所有玩家都提供其 `xim_player_matchmaking_configuration` 结构后，回填将会继续。

## <a name="the-role-of-servers-host-migration-and-xim-authorities-a-nameauthority"></a>服务器角色（“主机迁移”）和 XIM 机构 <a name="authority">

XIM 网络在逻辑上是一个完全连接的对等设备网格--与客户端/服务器模型等相反。 任何玩家都可以通过 API 直接发送给任意其他玩家，并能通过任意参与的设备调用所有影响 XIM 网络状态的方法（例如 xim::set_network_custom_property()、xim::set_allowed_player_joins() 和 xim::set_chat_targets()）。 如果应用不阻止多个参与者同时有效地修改同一个 XIM 网络状态，XIM 会使用简单的最后写入优先冲突解决方案。 这意味着 XIM 不会实施任何“服务器”或“主机”角色概念。 而且它不会限制仍然想要定义自身概念（以及在离开 XIM 网络时，将此角色迁移到其他参与者中的附带过程，也称为“主机迁移”）的应用。

对于某些应用而言，游戏的主机纯粹是一个简单的社交构造。 用户决定为要加入的好友启动多人游戏体验，且此用户将代表整体小组来管理当前的游戏规则、地图和其他设置。 对于其他应用而言，这是一个更基本的通信角色。 它可以简化游戏状态中由单独玩家生成的仲裁冲突，例如，某个玩家的实时输入占先，因此该玩家打败了另一个玩家。 保持不间断的用户体验，并可靠地同意在单个设备上管理此类授权游戏状态（尽管玩家在仲裁时加入和离开（可能不顺利））可能是一种挑战，因此 XIM 的设计中包含了可协助应用的可选 `xim_authority` 对象。

XIM 机构表示 XIM 网络中的单个设备，此设备是基于最佳网络质量、稳定性、玩家信誉和其他动态因素自动选定的。 通过查询 `xim_authority::is_local()`，XIM 网络中的所有参与者都可以确定当前是否向其本地设备分配此责任，如以下示例所示：

```cpp
 bool authorityIsLocal = xim::singleton_instance().authority()->is_local();
```

只有一个 XIM 网络中的设备会向任意给定的玩家组报告 xim_authority 位于本地。 对于正在或将要鼓励其不要向用户明确标识此类设备，以减少对应用中特定于机构的行为进行不正当利用（例如，低延迟，或在来自其他设备的移动数据包被恶意阻止时始终遵循本地模拟）的可能性的设备，应用不应该进行假设。

在将来的软件版本中，应用将能够使用 `xim_player::xim_local::send_data_to_authority()` 方法发送直接指向 `xim_authority` 的消息，还能接收直接来自其中的消息。 XIM 还会在迁移过程中提供 `xim_state_change` 通知和数据缓冲区交换。 但是，此软件版本中不提供这些功能。 `xim_authority::is_local()` 以外的所有 `xim_authority` 方法和 `xim_player::xim_local::send_data_to_authority()` 方法均未实现，如果调用，将引发异常。 如果对 xim_authority 有疑问，请联系 Microsoft 代表。`
