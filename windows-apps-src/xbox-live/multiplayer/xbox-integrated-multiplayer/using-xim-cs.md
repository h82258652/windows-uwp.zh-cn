---
title: 使用 XIM (C#)
author: KevinAsgari
description: 了解如何将 Xbox 集成多人游戏 (XIM) 与 C# 结合使用。
ms.author: kevinasg
ms.date: 04/24/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Xbox live, xbox, 游戏, uwp, windows 10, xbox 集成多人游戏
ms.localizationpriority: medium
ms.openlocfilehash: 7cb15206469abf96d8c88490c35c86b8c5197e93
ms.sourcegitcommit: a160b91a554f8352de963d9fa37f7df89f8a0e23
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/22/2018
ms.locfileid: "4129049"
---
# <a name="using-xim-c"></a>使用 XIM (C#)

> [!div class="op_single_selector" title1="Language"]
> - [C++](using-xim.md)
> - [C#](using-xim-cs.md)

这是关于使用 XIM 的 C# API 的简短演练。 需要通过 C++ 访问 XIM 的游戏开发人员应查看[使用 XIM (C++)](using-xim.md)。
- [使用 XIM (C#)](#using-xim-c)
    - [先决条件](#prerequisites)
    - [初始化和启动](#initialization-and-startup)
    - [异步操作和处理状态更改](#asynchronous-operations-and-processing-state-changes)
    - [基本 IXimPlayer 处理](#basic-iximplayer-handling)
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

## <a name="initialization-and-startup"></a>初始化和启动

通过 Xbox Live 服务配置 ID 字符串和游戏 ID 号，你可以初始化 XIM 静态类属性并开始与库交互。 如果还要使用 Xbox 服务 API，你可以方便地从 `Microsoft.Xbox.Services.XboxLiveAppConfiguration` 中检索。 以下示例假定值已经分别位于“myServiceConfigurationId”和“myTitleId”变量中：

```cs
XboxIntegratedMultiplayer.ServiceConfigurationId = myServiceConfigurationId;
XboxIntegratedMultiplayer.TitleId = myTitleId;
```

初始化后，应用应该为当前位于将要参与的本地设备上的所有用户检索 Xbox 用户 ID 字符串，并将其传递给 `XboxIntegratedMultiplayer.SetIntendedLocalXboxUserIds()` 调用。 以下示例代码假定单个用户已经按下表示意图运行的控制器按钮，且与用户关联的 Xbox 用户 ID 字符串已经被检索到“myXuid”变量中：

```cs
XboxIntegratedMultiplayer.SetIntendedLocalXboxUserIds(new List<string>() { myXuid });
```

调用 `XboxIntegratedMultiplayer.SetIntendedLocalXboxUserIds()` 会立即设置与应添加到 XIM 网络中的本地用户关联的 Xbox 用户 ID。 以后的所有网络操作都将使用此 Xbox 用户 ID 列表，直到通过其他对 `XboxIntegratedMultiplayer.SetIntendedLocalXboxUserIds()` 的调用更改列表。

在不存在任何 XIM 网络的情况下，第一步是移动到 XIM 网络以便开始该过程。 如果用户没有记住具体的 XIM 网络，最佳做法是，只需移动到新的空网络。 这样，用户的好友便可以加入此网络并将其作为某种“大厅”，在“大厅”中，他们可以进行协作，选择下多人游戏活动（例如，一起进入匹配）。

下面的示例显示了如何将先前使用 `XboxIntegratedMultiplayer.SetIntendedLocalXboxUserIds()` 添加的本地用户移动到空 XIM 网络（拥有最多能容纳共 8 位玩家的空间）：

```cs
XboxIntegratedMultiplayer.MovetoNewNetwork(8, XimPlayersToMove.BringOnlyLocalPlayers);
```

对 `XboxIntegratedMultiplayer.MovetoNewNetwork()` 的调用将会启动异步移动操作，最后以状态更改事件结束，开发人员应定期处理此类事件。

## <a name="asynchronous-operations-and-processing-state-changes"></a>异步操作和处理状态更改

XIM 的核心是应用对 `XboxIntegratedMultiplayer.GetStateChanges()` 方法的定期且频繁的调用。 此方法是，如何通知 XIM 应用已准备好处理多人游戏状态的更新，以及 XIM 如何通过返回包含所有已排队更新的 `XimStateChangeCollection` 对象来提供这些更新。 `XboxIntegratedMultiplayer.GetStateChanges()` 旨在加快运行速度，以便在 UI 呈现循环中对每个图形帧执行调用。 这提供了方便的位置来检索所有已排队更改，而不必担心网络计时的不可预测性或多线程回调的复杂性。 XIM API 实际上已针对此单线程模式进行了优化。 它保证其状态在 `XimStateChangeCollection` 对象被处理且尚未释放之前将保持不变。

`XimStateChangeCollection` 是 `IXimStateChange` 对象的集合。
应用应该：

1. 迭代该数组。
1. 检查 `IXimStateChange` 的更具体类型。
1. 将 `IXimStateChange` 类型转换为相应的更详细的类型。
1. 根据需要处理更新。

完成所有当前可用的 `IXimStateChange` 对象的操作后，应通过调用 `XimStateChangeCollection.Dispose()` 来将该数组传递回 XIM 以释放资源。 建议使用 `using` 声明，以确保资源在处理后会释放。 例如：

```cs
using (var stateChanges = XboxIntegratedMultiplayer.GetStateChanges())
{
    foreach (var stateChange in stateChanges)
    {
        switch (stateChange.Type)
        {
            case XimStateChangeType.PlayerJoined:
                HandlePlayerJoined((XimPlayerJoinedStateChange)stateChange);
                break;

            case XimStateChangeType.PlayerLeft:
                HandlePlayerLeft((XimPlayerLeftStateChange)stateChange);
                break;

            ...
        }
    }
}
```

既然你有了基本处理循环，便可以处理与初始 `XboxIntegratedMultiplayer.MoveToNewNetwork()` 操作关联的状态更改。 每个 XIM 网络移动操作都会从 `XimMoveToNetworkStartingStateChange` 开始。 如果出于任何原因，移动失败，则会向应用提供 `XimNetworkExitedStateChange`，对于阻止你移动到 XIM 网络或将你与当前 XIM 网络断开连接的任何异步错误而言，这是常见的故障处理机制。 否则，将在完成所有状态，并将所有玩家成功添加到 XIM 网络后，使用 `XimMoveToNetworkSucceededStateChange` 完成移动。

当用户没有在多人游戏会话中与其他人一起玩游戏的平台特权时，XIM 会向试图创建或加入 XIM 网络的设备发送 `XimNetworkExitedStateChange`，并显示原因 `UserMultiplayerRestricted`。 当使用 `XboxIntegratedMultiplayer.SetIntendedLocalXboxUserIds()` 设置的任何本地用户具有 Xbox Live 多人游戏限制时，将会发生这种情况。 请通过将该问题告知用户，进行适当处理。

## <a name="basic-iximplayer-handling"></a>基本 IXimPlayer 处理

假定将单个本地用户移动到新的 XIM 网络的示例成功完成，则会为应用提供本地 `XimLocalPlayer` 对象的 `XimPlayerJoinedStateChange`。 在通过 `XimStateChangeCollection.Dispose()` 提供并返回对应的 `XimPlayerLeftStateChange` 之前，此对象一直保持有效。 始终为应用提供每个 `XimPlayerJoinedStateChange` 的 `XimPlayerLeftStateChange`。

你还可以使用 `XboxIntegratedMultiplayer.GetPlayers()` 随时检索 XIM 网络中所有 `IXimPlayer` 对象的列表。

`IXimPlayer` 对象有很多有用的属性和方法，例如，`IXimPlayer.Gamertag()` 可检索与玩家关联的当前 Xbox Live 玩家代号字符串，以用于显示。 如果 `IXimPlayer` 在设备本地，IXimPlayer.Local 将返回 true。 本地 `IXimPlayer` 可以转换为 `XimLocalPlayer`，它具有只有本地玩家可用的其他方法。

当然，对玩家而言，最重要的状态不是 XIM 知道的常见信息，而是特定应用要跟踪的信息。由于你对该跟踪信息可能有自己的构造，因此，你可能需要将 `IXimPlayer` 对象链接到你自己的玩家构造对象，这样，每当 XIM 报告 `IXimPlayer` 时，你都可以快速检索状态，而无需通过设置自定义玩家上下文对象执行查找。 以下示例假定包含你的隐私状态的对象已经在变量“myPlayerStateObject”中，且新添加的 `IXimPlayer` 对象已经在变量“newXimPlayer”中：

```cs
newXimPlayer.CustomPlayerContext = myPlayerStateObject;
```

这会将带玩家对象的指定对象保存到本地（它绝不会通过网络传输到远程设备，远程设备中的内存无效）。 通过检索自定义上下文并将其重新转换为对象，你将能够始终返回你的对象，如以下示例所示：

```cs
myPlayerStateObject = (MyPlayerState)(newXimPlayer.CustomPlayerContext);
```

你可以随时更改此自定义玩家上下文。

## <a name="enabling-friends-to-join-and-inviting-them"></a>允许好友加入并邀请他们

出于隐私和安全性目的，默认情况下，所有新 XIM 网络都会自动配置为仅可由本地玩家加入，就绪后，由应用负责明确允许其他玩家加入。 下面的示例显示如何使用 `XboxIntegratedMultiplayer.NetworkConfiguration` 检索当前网络配置，并使用 `XboxIntegratedMultiplayer.SetNetworkConfiguration()` 更新网络的可加入性，以开始允许新的本地用户作为玩家加入，以及已经获得邀请或正被 XIM 网络中现有玩家“关注”（一种 Xbox Live 社交关系）的其他用户作为玩家加入：

```cs
XimNetworkConfiguration currentConfiguration = new XimNetworkConfiguration(XboxIntegratedMultiplayer.NetworkConfiguration);
currentConfiguration.AllowedPlayerJoins = XimAllowedPlayerJoins.Local | XimAllowedPlayerJoins.Invited | XimAllowedPlayerJoins.Followed;
XboxIntegratedMultiplayer.SetNetworkConfiguration(currentConfiguration);
```

`XboxIntegratedMultiplayer.SetNetworkConfiguration()` 异步执行。 完成前一代码示例调用后，将会提供 `XimNetworkConfigurationChangedStateChange` 以通知应用，可加入性值已更改，不再是默认值 `XimAllowedPlayerJoins.None`。 然后，你可以通过检查 `XboxIntegratedMultiplayer.NetworkConfiguration.AllowedPlayerJoins` 的值来查询新值。

`XboxIntegratedMultiplayer.NetworkConfiguration.AllowedPlayerJoins` 当设备在 XIM 网络中时，可以通过检查该值确定该网络的可加入性。

如果其中一个本地玩家向远程用户发送加入此 XIM 网络的邀请，则应用可以调用 `XimLocalPlayer.ShowInviteUI()` 以启动系统邀请 UI。 此处，本地用户可以选择他们希望邀请的人并发送邀请。

```cs
XimLocalPlayer.ShowInviteUI();
```

上述语句执行后，将会显示系统邀请 UI。 当用户发送邀请（或取消 UI）后，会提供 `xim_show_invite_ui_completed_state_change`。 或者，应用可以使用 `XimLocalPlayer.InviteUsers()` 直接发送邀请而不调出系统邀请 UI。

无论使用哪种方式，远程用户都会在登录时收到 Xbox Live 邀请消息，并可以选择接受。 如果应用尚未运行，你可以在这些设备上启动应用，并使用可用于移动到同一个 XIM 网络的事件参数将该应用“协议激活”。

有关协议激活本身的详细信息，请参阅平台文档。

下面的示例显示了 `XboxIntegratedMultiplayer.ExtractProtocolActivationInformation()` 调用如何确定事件参数是否适用于 XIM。 此示例假定你已经将原始 URI 字符串从 `Windows.ApplicationModel.Activation.ProtocolActivatedEventArgs` 检索到变量“uriString”：

```cs
XimProtocolActivationInformation protocolActivationInformation;
XboxIntegratedMultiplayer.ExtractProtocolActivationInformation(uriString, out protocolActivationInformation);
if (protocolActivationInformation != null)
{
    // It is a XIM activation.
}
```

如果是 XIM 激活，则需要确保 `XimProtocolActivationInformation` 对象的“LocalXboxUserId”属性中标识的本地用户已登录，并在指定给 `XboxIntegratedMultiplayer.SetIntendedLocalXboxUserIds()` 的用户中。 然后，你可以使用相同的 URI 字符串，通过调用 `XboxIntegratedMultiplayer.MoveToNetworkUsingProtocolActivatedEventArgs()` 开始移动到指定的 XIM 网络中。 例如：

```cs
XboxIntegratedMultiplayer.MoveToNetworkUsingProtocolActivatedEventArgs(uriString);
```

另请注意，受到“关注”的远程用户可以导航到系统 UI 中的本地用户玩家卡片并自行启动加入尝试，无需邀请（假定你已经允许此类玩家加入，如上所述）。 此操作也会协议激活你的应用（和邀请一样），并且以相同的方式处理。

使用协议激活移动到 XIM 网络和[初始化和启动](#initialization-and-startup)中所示的移动到新 XIM 网络相同。 唯一的区别是，移动成功后，会向移动的设备提供表示适用玩家的本地和远程玩家 `XimPlayerJoinedStateChange` 对象。 当然，XIM 网络中已经存在的原始设备不会移动，但会看到通过其他 `XimPlayerJoinedStateChange` 对象将新设备的用户添加为玩家。

跨不同设备的玩家在 XIM 网络中连接到一起后，他们可以启动多人游戏场景、自动通过语音和文字进行通信以及发送特定于应用的消息。

## <a name="basic-matchmaking-and-moving-to-another-xim-network-with-others"></a>基本匹配和随他人移动到其他 XIM 网络

通过将玩家移动到有陌生人的 XIM 网络，你可以进一步拓展一组好友的网络体验。 陌生人是基于同样的兴趣，通过 Xbox Live 匹配服务汇聚到一起的来自全世界的对手。

使用 XIM 启动基本匹配的一种简单方法是在填充了 `MatchmakingConfiguration` 对象的一个设备上调用 `XboxIntegratedMultiplayer.MoveToNetworkUsingMatchmaking()`，从当前 XIM 网络中带走玩家。

下面的示例通过使用一种匹配配置启动移动，该配置设置为寻找共 8 个玩家参与无团队自由竞赛。 如果找不到共 8 个玩家，系统可能仍会将 2-7 个玩家匹配在一起。 此示例使用应用定义的 uint 类型的常量，名为 MYGAMEMODE_DEATHMATCH，表示要筛除的游戏模式。 XIM 的匹配只会将此网络与指定相同值的其他玩家匹配，当第二个参数 `XimPlayersToMove.BringExistingSocialPlayers` 指定为 `XboxIntegratedMultiplayer.MoveToNetworkUsingMatchmaking()` 时，会从当前 XIM 网络中带走以社交方式加入的所有玩家。

```cs
var matchmakingConfiguration = new XimMatchmakingConfiguration();
matchmakingConfiguration.TeamConfiguration.TeamCount = 1;
matchmakingConfiguration.TeamConfiguration.MinPlayerCountPerTeam = 2;
matchmakingConfiguration.TeamConfiguration.MaxPlayerCountPerTeam = 8;
matchmakingConfiguration.CustomGameMode = MYGAMEMODE_DEATHMATCH;
XboxIntegratedMultiplayer.MoveToNetworkUsingMatchmaking(matchmakingConfiguration, XimPlayersToMove.BringExistingSocialPlayers);
```

和之前的移动一样，这会在所有设备上提供初始 `MoveToNetworkStartingStateChange`，并在成功完成移动后提供 `MoveToNetworkSucceededStateChange`。 由于这是从一个 XIM 网络到另一个 XIM 网络的移动，因此，其中一个区别是，已经存在为本地和远程用户添加的现有 `IXimPlayer` 对象，且会为所有一起移动到新 XIM 网络的玩家保留这些对象。 在进行匹配过程中，玩家之间的聊天和数据通信将继续工作，不会中断。

匹配可能是一个漫长的过程，具体取决于匹配池中也调用了 `XboxIntegratedMultiplayer.MoveToNetworkUsingMatchmaking()` 的潜在玩家数量。

整个操作过程中将定期提供 `XimMatchmakingProgressUpdatedStateChange`，使你和你的用户了解当前状态。 发现匹配时，会将其他玩家添加到具有典型 `PlayerJoinedStateChange` 的 XIM 网络中，然后，移动完成。

完成与这组“匹配”玩家的多人游戏体验后，你可以通过另一轮匹配重复移动到其他 XIM 网络的过程。 你会看到通过之前的 `XboxIntegratedMultiplayer.MoveToNetworkUsingMatchmaking()` 操作加入的每个玩家提供 `PlayerLeftStateChange`，以指示其 `IXimPlayer` 对象不再处于同一 XIM 网络中。

当你选择通过使用 `XboxIntegratedMultiplayer.MoveToNetworkUsingMatchmaking()` 再次启动匹配时，如果指定 `XimPlayersToMove.BringExistingSocialPlayers`，则在进行新匹配时，通过社交入口点（`XboxIntegratedMultiplayer.MoveToNetworkUsingProtocolActivatedEventArgs()` 或 `xXboxIntegratedMultiplayer.MoveToNetworkUsingJoinableXboxUserId`）加入的玩家将会保留。 或者，指定 `XimPlayersToMove.BringOnlyLocalPlayers` 将会使以社交方式连接的远程玩家断开连接，只保留本地玩家。 在两种情况下，第二次移动操作完成后，将添加另一组陌生人。

你也可以在决定下匹配配置/多人游戏活动时，选择将非匹配玩家（或仅本地玩家）移动到全新的 XIM 网络。

下面的示例演示了设备如何对拥有最多 8 个玩家的 XIM 网络调用 `XboxIntegratedMultiplayer.MoveToNewNetwork()`，但是，这次还包含以社交方式加入的现有玩家：

```cs
XboxIntegratedMultiplayer.MovetoNewNetwork(8, XimPlayersToMove.BringExistingSocialPlayers);
```

将会为所有参与设备提供 `MoveToNetworkStartingStateChange` 和 `MoveToNetworkSucceededStateChange`，并为留下的匹配设备提供 `PlayerLeftStateChange`。 对于移出网络的每个玩家，这些设备上会以同样的方式显示 `PlayerLeftStateChange`。

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

本地用户参与 XIM 网络后，他们通常只会重新移动到新 XIM 网络中，此网络允许本地用户、邀请用户和受到“关注”的用户加入，使他们能继续与其好友协作，查找下一个活动。 但是，如果用户完全完成了所有多人游戏体验，则你的应用可能要开始完全离开 XIM 网络，然后返回就像仅调用了 `XboxIntegratedMultiplayer.SetIntendedLocalXboxUserIds` 的状态。 此操作使用 `XboxIntegratedMultiplayer.LeaveNetwork()` 方法完成：

```cs
 XboxIntegratedMultiplayer.LeaveNetwork();
```

此方法会开始顺利地异步断开与其他参与者的连接的过程。 这会导致为远程设备提供已断开连接的每个本地玩家的 `PlayerLeftStateChange`。 也会为本地设备提供已断开连接的每个本地和远程玩家的 `PlayerLeftStateChange`。 完成所有断开连接操作后，将提供最终 `NetworkExitedStateChange`。

当尚未提供 `NetworkExitedStateChange` 时，始终强烈建议调用 `XboxIntegratedMultiplayer.LeaveNetwork()` 并等待 `NetworkExitedStateChange` 以便顺利退出 XIM 网络。

## <a name="sending-and-receiving-messages"></a>发送和接收消息

XIM 及其基础组件会执行建立安全的 Internet 通信渠道的所有繁琐操作，因此你无需担心连接问题或只能访问部分玩家。 如果有任何基本对等连接问题，将无法成功移动到 XIM 网络。

在组成并通过 `XimMoveToNetworkSucceededStateChange` 确认 XIM 网络后，已加入的所有设备上的所有应用实例保证获得有关已连接到 XIM 网络的每个 `IXimPlayer` 的通知，并可以向其中的任何一个发送消息。

下面我们演示如何跨 XIM 网络发送消息。 下面的示例假定“sendingPlayer”变量是有效的本地玩家对象。 该示例使用“sendingPlayer”的方法 `SendDataToOtherPlayers()` 将已复制到“msgBuffer”中的消息结构发送给 XIM 网络中的所有玩家（本地或远程）。 请注意，由于未向该方法传递特定玩家的列表，因此会发送给所有玩家：

```cs
SendingPlayer.SendDataToOtherPlayers(msgBuffer, null, XimSendType.GuaranteedAndSequential);
```

将向该消息的所有收件人提供 `XimPlayertoPlayerDataReceivedStateChange`，其中包括数据副本、发送它的 `IXimPlayer` 对象以及本地接收它的 `IXimPlayer` 对象的列表。

当然，有保证的连续传递很方便，但它也有可能成为低效率的发送类型，这是因为，如果 Internet 丢失了数据包或打乱了数据包的顺序，则 XIM 需要重新传输或延迟消息。 请务必考虑使用应用可容忍丢失或乱序到达的其他消息发送类型。 由于消息数据来自远程计算机，因此，最佳做法是明确定义数据格式，例如按特定字节顺序（“字节顺序”）打包多字节值。 对数据执行操作之前，应用还应验证数据。

XIM 提供网络级别的安全，因此不需要使用额外的加密或签名方案。 不过，我们建议始终使用“深度防护”以避免受到意外应用程序错误的影响并能够处理正常共存的不同版本的应用程序协议（在开发、内容更新等期间）。 用户的 Internet 连接可能是受限且不断变化的资源。 我们强烈建议使用高效的消息数据格式，并避免采用会发送每个 UI 框架的设计。

## <a name="assessing-data-pathway-quality"></a>评估数据路径质量

可以通过检查 `XimPlayer.NetworkPathInformation` 属性，了解有关两个玩家之间的当前数据路径质量的信息。 你可以通过检查 `XimPlayer.NetworkPathInformation` 属性，了解有关两个玩家之间的当前路径质量的详细信息。 如果连接在该时刻不支持传输更多数据，则该属性包含估计的往返行程延迟以及有多少消息仍在本地排队等待传输。

如果发现队列正在对某一特定“IXimPlayer”进行备份，你应该降低发送数据的速率。

`NetworkPathInformation.RoundTripLatency` 字段表示在不排队情况下的基础网络延迟和 XIM 预计延迟。 有效延迟将随着 `XimNetworkPathInformation.SendQueueSizeInMessages` 增加以及 XIM 队列推进而增加。

请根据游戏使用情况和要求选择开始限制 `SendDataToOtherPlayers` 调用的合理点。 发送队列中的每个消息都代表着有效网络延迟的增加。

一个接近 XIM 最大限制（当前为 3500 条消息）的值对于大部分游戏而言都太高了，而且可能意味着需要等待数秒才能发送数据，具体等待时长取决于调用 `SendDataToOtherPlayers` 的速率以及每个有效数据负载的大小。 相反，我们选择的数值应该考虑到游戏的延迟要求以及游戏 `SendDataToOtherPlayers` 调用模式的不稳定程度。

## <a name="sharing-data-using-player-custom-properties"></a>使用玩家自定义属性共享数据

大多数应用数据交易都会通过 `XimLocalPlayer.SendDataToOtherPlayers()` 方法发生，因为它允许对接收交换的用户和时间、如何处理数据包丢失等问题进行最大程度的控制。 但有时候，能让玩家很容易地将关于自身的基本、很少更改的状态与他人分享是一个不错的选择。 例如，在进入所有玩家用来呈现其游戏内表示形式的多人游戏之前，每个玩家可能都有表示所选的人物模型的固定字符串。

对于与玩家有关的不太频繁更改的数据，XIM 提供玩家自定义属性。 这些属性由应用定义的名称和值构成，它们是以 null 结尾的字符串对，可以应用于本地玩家，并可以在设备更改后自动传播到所有设备。

与 `XimLocalPlayer.SendDataToOtherPlayers()` 方法相比，自定义玩家属性的内部同步开销更多，因此，对于快速变化的数据（即玩家位置），你应该仍然使用直接发送。

当新参与的设备加入 XIM 网络并查看添加的玩家时，会将玩家自定义属性键值对的当前值自动提供给这些设备。 通过使用名称和值字符串调用 `XimLocalPlayer.SetPlayerCustomProperty()`，可以设置这些值，例如，以下示例设置了名为“model”的属性，在由变量“localPlayer”指向的本地 `IXIMPlayer` 对象上，该属性的值为“brute”：

```cs
localPlayer.SetPlayerCustomProperty("model","brute");
```

更改玩家属性会导致为所有设备提供 `XimPlayerCustomPropertiesChangedStateChange`，以警告它们属性的名称已发生更改。 可使用 `IXIMPlayer.GetPlayerCustomProperty()` 在任意本地或远程玩家上检索给定名称的值。 以下示例检索了变量“ximPlayer”指向的 `IXIMPlayer` 中名为“模型”的属性的值：

```cs
string propertyValue = localPlayer.GetPlayerCustomProperty("model");
```

为给定的属性名称设置新值会替换任何现有值。 如果将属性名称设置为某个 null 值字符串指针的值，则会将该属性与空值字符串同等对待，这相当于尚未指定该属性。 XIM 不会解释名称和值，因此，将由应用根据需要验证字符串内容。

从一个 XIM 网络移动到另一个 XIM 网络时，始终会重置自定义玩家属性。 但是，加入 XIM 网络的新玩家将会收到已设置某些自定义玩家属性的所有玩家的当前自定义玩家属性。

## <a name="sharing-data-using-network-custom-properties"></a>使用网络自定义属性共享数据

网络自定义属性旨在方便同步特定于不频繁更改的 XIM 网络的数据。 网络自定义属性的工作方式与[玩家自定义属性](#sharing-data-using-player-custom-properties)完全相同，只不过它们是使用 `XboxIntegratedMultiplayer.SetNetworkCustomProperty()` 在 XIM 单一实例对象上设置的。 以下示例设置了“地图”属性，使其拥有“要塞”一值:

```cs
XboxIntegratedMultiplayer.SetNetworkCustomProperty("map", "stronghold");
```

更改网络属性会导致为所有设备提供 `NetworkCustomPropertiesChanged`，以警告它们属性的名称已发生更改。 可使用 `XboxIntegratedMultiplayer.GetNetworkCustomProperty()` 检索给定名称的值，例如，以下示例检索了名为“地图”的属性的值：

```cs
string property = XboxIntegratedMultiplayer.GetNetworkCustomProperty("map")
```

正如[玩家自定义属性](#sharing-data-using-player-custom-properties)一样，为给定的属性名称设置新值会替换任何现有值。 如果将属性名称设置为某个 null 值字符串指针的值，则会将该属性与空值字符串同等对待，这相当于尚未指定该属性。 XIM 不会解释名称和值，因此，将由应用根据需要验证字符串内容。

新创建的 XIM 网络最开始时始终没有设置网络自定义属性。 但加入现有 XIM 网络的新玩家将会收到为该 XIM 网络设置的网络自定义属性的当前值。

## <a name="matchmaking-using-per-player-skill"></a>使用按玩家技能匹配

在某个应用指定的游戏模式下，按共同兴趣匹配玩家是不错的基本策略。 当可用玩家池扩大时，你还应该考虑基于玩家的个人技能或游戏体验匹配玩家，以便资深玩家可以享受与其他资深玩家的良性竞争带来的挑战，而新玩家可以通过与能力相似的玩家进行竞争来获得成长。

要执行此操作，在使用匹配开始移动到 XIM 网络前，请先在对 `XimLocalPlayer.SetRolesAndSkillConfiguration()` 的调用中指定的按玩家角色和技能配置结构中提供所有本地玩家的技能等级。 技术等级是特定于应用的概念，且 XIM 不会解释数字，不过，匹配会首先尝试查找具有相同技能值的玩家，然后定期以 +/- 10 的增量扩大搜索范围，以尝试查找其他在此技能附近范围内声明技能值的玩家。 以下示例假定本地 `XimLocalPlayer` 对象（其变量为“localPlayer”）有一个关联的特定于应用的整数技能值，该值从本地或 Xbox Live 存储检索到名为“playerSkillValue”的变量中：

```cs
var config = new XimPlayerRolesAndSkillConfiguration();
config.Skill = playerSkillValue;
localPlayer.SetRolesAndSkillConfiguration(config);
```

此操作完成时，会向所有参与者提供 `XimPlayerRolesAndSkillConfigurationChangedStateChange`，以指示此 `IXIMPlayer` 已更改其按玩家角色和技能配置。 可通过调用 `IXIMPlayer.RolesAndSkillConfiguration` 检索新值。 当所有玩家都应用非 null 匹配配置时，你可以使用匹配将其移动到 XIM 网络，并为 `XboxIntegratedMultiplayer.MoveToNetworkUsingMatchmaking()` 指定 `XimMatchmakingConfiguration` 结构的 `RequirePlayerRolesAndSkillConfiguration` 字段中的 true 值。

下面的示例填充一个匹配配置，以便寻找共 2-8 个玩家参与无团队自由竞赛。 此外，此示例使用应用定义的常量，该常量的类型为 Uint64，名为 MYGAMEMODE_DEATHMATCH，表示要筛除的游戏模式。 这将配置匹配，将 XIM 网络的玩家与指定这些相同值的其他玩家相匹配，并且需要按玩家匹配配置。

```cs
XimMatchmakingConfiguration matchmakingConfiguration = new XimMatchmakingConfiguration();
matchmakingConfiguration.TeamConfiguration.TeamCount = 1;
matchmakingConfiguration.TeamConfiguration.MinPlayerCountPerTeam = 2;
matchmakingConfiguration.TeamConfiguration.MaxPlayerCountPerTeam = 8;
matchmakingConfiguration.CustomGameMode = MYGAMEMODE_DEATHMATCH;
matchmakingConfiguration.RequirePlayerRolesAndSkillConfiguration = true;
```

为 `XboxIntegratedMultiplayer.MoveToNetworkUsingMatchmaking()` 提供此结构时，只要移动的玩家已通过非 null `XimPlayerRolesAndSkillConfiguration` 对象调用 `XimLocalPlayer.SetRolesAndSkillConfiguration()`，移动操作将正常开始。 如果任意玩家未执行此操作，则会暂停匹配过程，并为所有参与者提供值为 `WaitingForPlayerRolesAndSkillConfiguration` 的 `XimMatchmakingProgressUpdatedStateChange`。 这包括在匹配完成后，通过以前发送的邀请或通过其他社交方式（例如，调用 `XboxIntegratedMultiplayer.MoveToNetworkUsingJoinableXboxUserId()`）加入 XIM 网络的玩家。 所有玩家都提供其 `XimPlayerRolesAndSkillConfiguration` 对象后，匹配将继续。

如下一节中所述，使用按玩家技能匹配也可以与按玩家角色匹配用户结合使用。 如果只需要其中一个，你可以将另一个的值指定为 0。 这是因为声明 `XimPlayerRolesAndSkillConfiguration` 技能值为 0 的所有玩家始终会彼此匹配。

`XboxIntegratedMultiplayer.MoveToNetworkUsingMatchmaking()` 或其他 XIM 网络移动操作完成后，将把所有玩家的 `XimPlayerRolesAndSkillConfiguration` 结构自动清理为 null 指针（附带 `XimPlayerRolesAndSkillConfigurationChangedStateChange` 通知）。 如果你计划使用需要按玩家配置的匹配移动到另一个 XIM 网络，则需要通过一个包含最新信息的对象再次调用 `XimLocalPlayer.SetRolesAndSkillConfiguration()`。

## <a name="matchmaking-using-per-player-role"></a>使用按玩家角色匹配

使用按玩家角色和技能配置改进用户匹配体验的另一种方法是使用所需的玩家角色。 这非常适合提供可选角色类型的游戏，这些角色可以鼓励各种合作游戏风格。 这些角色类型不是单纯地改变游戏内图形表示形式，而是改变玩家的游戏风格。 用户可能想要以某个特长进行游戏。 但是，如果游戏设计为每个角色必须不少于一个玩家才能实际完成目标，有时需要先将这些玩家而非任意玩家匹配到一起，并在集合后要求其协商他们之间的游戏风格。 你可以通过先定义表示要在给定玩家的 `XimPlayerRolesAndSkillConfiguration` 结构中指定的每个角色的唯一位标志来执行此操作。

下面的示例为本地 `XimLocalPlayer` 对象（其指针为“localPlayer”）设置特定于应用的角色值，其类型为字节，名为 MYROLEBITFLAG_HEALER：

```cs
var config = new XimPlayerRolesAndSkillConfiguration();
config.Roles = MYROLEBITFLAG_HEALER;
localPlayer.SetRolesAndSkillConfiguration(config);
```

此操作完成时，会向所有参与者提供 `XimPlayerRolesAndSkillConfigurationChangedStateChange`，以指示此 `IXimPlayer` 已更改其按玩家角色和技能配置。 可通过调用 `IXimPlayer.RolesAndSkillConfiguration` 检索新值。

然后，指定给 `XboxIntegratedMultiplayer.MoveToNetworkUsingMatchmaking()` 的全局 `XimMatchmakingConfiguration` 结构应使用按位 OR 和 `RequirePlayerRolesAndSkillConfiguration` 字段的 true 值合并所有需要的角色标记。

使用按玩家角色匹配也可以与按玩家技能匹配用户结合使用。 如果只需要其中一个，可将另一个的值指定为 0。 这是因为，所有声明其 `XimPlayerRolesAndSkillConfiguration` 技能值为 0 的玩家始终互相匹配，如果 `XimMatchmakingConfiguration.RequiredRoles` 字段中均为零位，则不需要角色位就能匹配。

`XboxIntegratedMultiplayer.MoveToNetworkUsingMatchmaking()` 或任何其他 XIM 网络移动操作完成后，会将所有玩家的 `XimPlayerRolesAndSkillConfiguration` 对象自动清理为 null（附带 `XimPlayerRolesAndSkillConfigurationChangedStateChange` 通知）。 如果你计划使用需要按玩家配置的匹配移动到另一个 XIM 网络，则需要通过一个包含最新信息的对象再次调用 `XimLocalPlayer.SetRolesAndSkillConfiguration()`。

## <a name="how-xim-works-with-player-teams"></a>XIM 如何与玩家团队配合工作

多人游戏通常涉及到组织到对方团队的玩家。 XIM 可通过设置 `XimMatchmakingConfiguration.TeamConfiguration` 简化匹配时的团队分配。 以下示例通过使用经过配置以寻找共 8 个玩家来编入两个 4 人团队（如果找不到 4 个玩家，1-3 个玩家也可接受）的匹配发起移动。 此外，此示例使用应用定义的常量，该常量的类型为 uint，名为 MYGAMEMODE_CAPTURETHEFLAG，表示要筛除的游戏模式。  此外，配置设置为从当前 XIM 网络中带走所有以社交方式加入的玩家：

```cs
var matchmakingConfiguration = new XimMatchmakingConfiguration();
matchmakingConfiguration.TeamConfiguration.TeamCount = 2;
matchmakingConfiguration.TeamConfiguration.MinPlayerCountPerTeam = 1;
matchmakingConfiguration.TeamConfiguration.MaxPlayerCountPerTeam = 4;
XboxIntegratedMultiplayer.MoveToNetworkUsingMatchmaking(matchmakingConfiguration, XimPlayersToMove.BringExistingSocialPlayers);
```

完成此 XIM 网络移动操作后，将通过 {n}（与请求的 {n} 个团队对应）为玩家分配团队索引值 1。 通过 `IXIMPlayer.TeamIndex` 检索玩家的团队索引值。 将 `XimMatchmakingConfiguration.TeamConfiguration` 与两个或以上团队一起使用时，绝不会通过调用 `XboxIntegratedMultiplayer.MoveToNetworkUsingMatchmaking()` 来为玩家分配团队索引值 0。 这与通过任意其他移动操作配置或类型（例如通过接受邀请导致的协议激活）添加到 XIM 网络的玩家相反，他们将始终拥有 0 团队索引。 它可能有助于将团队索引 0 视为特殊的“未分配”团队。

以下示例检索 XIM 玩家对象（存储在“ximPlayer”变量中）的团队索引：

```cs
byte playerIndex = ximPlayer.TeamIndex;
```

对于首选的用户体验（更不用说负面玩家行为的机会减少），Xbox Live 匹配服务绝不会将一起移动到 XIM 网络的玩家拆分到不同的团队中。

最初通过匹配分配的团队索引值只是建议，应用可以使用 `XimLocalPlayer.SetTeamIndex()` 随时为本地玩家进行更改。 还可以在完全不使用匹配的 XIM 网络中调用此值。 下面的示例配置了一个存储在“ximLocalPlayer”变量中的 XIM 本地玩家对象，以获得新的团队索引值 1 ：

```cs
ximLocalPlayer.SetTeamIndex(1);
```

为所有设备提供此玩家的 `XimPlayerTeamIndexChangedStateChange` 时，会告知所有设备该玩家有一个有效的新团队索引值。 你可以随时调用 `XimLocalPlayer.SetTeamIndex()`。

匹配会独立于团队评估所需的按玩家角色。 因此，不建议将团队和所需的角色用作同时匹配配置标准，因为团队是通过玩家计数（而不是履行的玩家角色）进行平衡的。

## <a name="working-with-chat"></a>使用聊天

会为 XIM 网络中的玩家自动启用语音和文字聊天通信。 XIM 会为你处理与所有语音耳机和麦克风硬件的交互。 你的应用不需要为聊天执行过多操作，但有一个有关文字聊天的要求：支持输入和显示。 需要文本输入是因为，即使在过去没有广泛使用过物理键盘的平台和游戏类型中，玩家也可以配置系统以使用文本到语音转换辅助技术。 同样，需要文本显示是因为，玩家可以配置系统以使用语音到文本转换。

如果希望有条件地启用文本机制，可以通过分别访问 `XimLocalPlayer.ChatTextToSpeechConversionPreferenceEnabled` 和 `XimLocalPlayer.ChatSpeechToTextConversionPreferenceEnabled` 字段在本地玩家中检测到这些首选项，并且，你可能希望有条件地启用文本机制。 但是，建议考虑进行文本输入，并显示始终可用的选项。

`Windows::Xbox::UI::Accessability`  是专门为提供游戏内文字聊天的简单呈现而设计的 Xbox One 类，专注于语音到文本转换辅助技术。

获得由真实或虚拟键盘提供的文本输入后，请将字符串传递给 `XimLocalPlayer.SendChatText()` 方法。 以下代码显示了从变量“localPlayer”指向的本地 `IXIMPlayer` 对象发送示例硬编码字符串：

```cs
localPlayer.SendChatText(text);
```

此聊天文本会传递给 XIM 网络中能够以 `XimChatTextReceivedStateChange` 形式从原始本地玩家接收聊天通信的所有玩家。 如果启用了 TTS，则它可以与语音音频同步。

你的应用应该对收到的任意文本字符串进行复制，并将它与原始玩家的某些标识一起显示适当时间（或在可滚动窗口中显示）。

还有一些有关聊天的最佳做法。 无论玩家在什么地方显示（尤其是显示在记分牌等玩家代号列表中），都建议你也为用户显示静音/说话图标作为反馈。 通过访问 `IXimPlayer.ChatIndicator` 以检索表示玩家当前的即时聊天状态的 `XboxIntegratedMultiplayer.XimPlayerChatIndicator` 来完成此操作。

例如，由 `IXimPlayer.ChatIndicator` 报告的值预计会在玩家开始和停止说话时频繁改变。 它旨在支持应用在每个 UI 框架将其作为结果进行轮询。

> [!NOTE]
> 如果本地用户因其设备设置而没有足够的通信权限，`XimLocalPlayer.ChatIndicator` 将返回一个值为 `XIM_PLAYER_CHAT_INDICATOR_PLATFORM_RESTRICTED` 的 `XboxIntegratedMultiplayer.XimPlayerChatIndicator`。 对平台满足要求的期望指的是：应用可显示指示语音聊天或发送消息的平台限制的图标，以及指示问题的用户消息。 我们建议的示例消息为：“抱歉，当前不允许聊天。”

## <a name="muting-players"></a>将玩家静音

另一个最佳做法是支持静音玩家。 XIM 通过玩家卡自动处理由用户启动的系统静音，但应用应该支持可在游戏 UI 内通过 `IXimPlayer.ChatMuted` 属性执行的特定于游戏的短暂性静音。 以下示例开始静音变量“remotePlayer”指向的远程 `IXIMPlayer` 对象，以便不会从中听到语音聊天或收到文字聊天：

```cs
remotePlayer.ChatMuted = true;
```

静音立即生效，且没有与之关联的 XIM 状态更改。 可以将 `IXimPlayer.ChatMuted` 属性的值更改为 false。 下面的示例取消变量“remotePlayer”指向的远程 `IXIMPlayer` 对象的静音：

```cs
remotePlayer.ChatMuted = false;
```

只要 `IXIMPlayer` 存在（包括随玩家移动到新 XIM 网络时），静音就保持有效。 如果玩家离开，然后同一个用户重新加入（作为新的 `IXIMPlayer` 实例），则不保留。

玩家通常会在取消静音状态下开始。 如果你的应用出于玩游戏的原因而需要在静音状态下启动玩家，它可以在处理完关联的 `PlayerJoinedStateChange` 之前，在 `IXIMPlayer` 对象上设置 `IXIMPlayer.ChatMuted`，且 XIM 保证不会在任何时间段听到来自玩家的语音音频。

当远程玩家加入 XIM 网络时，会基于玩家信誉自动进行静音检查。 如果玩家有信誉不佳的标志，则玩家会被自动静音。 静音仅影响本地状态，因此，如果玩家在网络中移动，静音仍然存在。 只要 `IXIMPlayer` 保持有效，基于信誉的自动静音检查就只会执行一次，且不再重新评估。

## <a name="configuring-chat-targets-using-player-teams"></a>使用玩家团队配置聊天目标

如本文档 [XIM 如何与玩家团队配合工作](#how-xim-works-with-player-teams)部分中所述，任何特定团队索引值的真正含义都取决于应用。 XIM 不会解释索引值，与聊天目标配置有关的相等比较除外。 如果由 `XboxIntegratedMultiplayer.ChatTargets` 报告的聊天目标配置当前 `XimChatTargets.SameTeamIndexOnly`，则任意给定玩家将仅与另一个玩家交换聊天通信，前提是两个玩家有相同的由 `IXIMPlayer.TeamIndex` 报告的值（且隐私/策略也允许）。

为保持谨慎并支持竞争方案，会将新建的 XIM 网络配置为默认为 `XimChatTargets.SameTeamIndexOnly`。 但是，可能需要与其他团队中的战败对手聊天，例如，在游戏后的“大厅”中。 通过调用 `XboxIntegratedMultiplayer.SetChatTargets()`，你可以指示 XIM 允许所有人在隐私和策略允许的情况下与其他任何人交谈。 以下示例开始配置 XIM 网络中的所有参与者，以使用 `XimChatTargets.AllPlayers` 值：

```cs
XboxIntegratedMultiplayer.SetChatTargets(XimChatTargets.AllPlayers)
```

向所有参与者提供 `XimChatTargetsChangedStateChange` 时，会告知他们有一个有效的新目标设置。

如之前所述，大多数 XIM 网络移动类型最初都会为所有玩家分配团队索引值 0。 这意味着，默认情况下，`XimChatTargets.SameTeamIndexOnly` 的配置可能与 `XimChatTargets.AllPlayers` 无法区分。 但是，如果匹配配置的 `TeamMatchmakingMode` 值声明两个或以上团队，则使用匹配移动到 XIM 网络的玩家可能有不同的团队索引值。 你还可以随时调用 `XimLocalPlayer.SetTeamIndex()`，如上所示。 如果你的应用通过以下任一方法使用非零团队索引值，请不要忘记相应地管理当前聊天目标设置。

## <a name="automatic-background-filling-of-player-slots-backfill-matchmaking"></a>玩家空位的自动后台填充（“回填”匹配）

同时调用 `XboxIntegratedMultiplayer.MoveToNetworkUsingMatchmaking()` 的不同玩家组会为 Xbox Live 匹配服务提供最大的灵活性，以便将玩家快速整理到最优的新 XIM 网络。 但是，某些游戏场景想要将某个 XIM 网络保持原样，只匹配其他玩家，仅仅是为了填充空白玩家空位。 XIM 通过以 `XimNetworkConfiguration` （其 `XimNetworkConfiguration.AllowedPlayerJoins` 属性上设置了 `XimAllowedPlayerJoins.Matchmade` 标志）调用 `XboxIntegratedMultiplayer.SetNetworkConfiguration()`，支持配置匹配以在自动背景填充（“回填”）模式下操作。

以下示例使用特定于应用的游戏模式常量 uint64_t 配置了回填匹配，以尝试为无团队自由竞赛寻找共 8 个玩家（如果找不到 8 个玩家，2-7 个玩家也可接受），该常量由仅与指定相同值的其他玩家匹配的值 MYGAMEMODE_DEATHMATCH 定义：

```cs
var networkConfiguration = new XimNetworkConfiguration(XboxIntegratedMultiplayer.NetworkConfiguration);
networkConfiguration.AllowedPlayerJoins |= XimAllowedPlayerJoins.Matchmade;
networkConfiguration.TeamConfiguration.TeamCount = 1;
networkConfiguration.TeamConfiguration.MinPlayerCountPerTeam = 2;
networkConfiguration.TeamConfiguration.MaxPlayerCountPerTeam = 8;
networkConfiguration.CustomGameMode = MYGAMEMODE_DEATHMATCH;
XboxIntegratedMultiplayer.SetNetworkConfiguration(networkConfiguration);
```

这会使现有 XIM 网络适用于以正常方式调用 `XboxIntegratedMultiplayer.MoveToNetworkUsingMatchmaking()` 的设备。 这些设备不会看到任何行为更改。 回填 XIM 网络中的参与者不会移动，但会向其提供表明回填打开的 `XimNetworkConfigurationChangedStateChange`，以及多个 `XimMatchmakingProgressUpdatedStateChange` 通知（如果适用）。 将使用正常的 `XimPlayerJoinedStateChange` 将所有匹配的玩家添加到 XIM 网络。

默认情况下，回填匹配会保持无限期启用，但它不会在 XIM 网络已经有由 `XimNetworkConfiguration.TeamConfiguration` 设置指定的最大玩家数时尝试添加玩家。 可以通过设置 `XimNetworkConfiguration.AllowedPlayerJoins`（但不带 `XimAllowedPlayerJoins.Matchmade`）来禁用回填：

```cs
var networkConfiguration = new XimNetworkConfiguration(XboxIntegratedMultiplayer.NetworkConfiguration);
networkConfiguration.AllowedPlayerJoins &= ~XimAllowedPlayerJoins.Matchmade;
XboxIntegratedMultiplayer.SetNetworkConfiguration(networkConfiguration);
```

将向所有设备提供相应的 `XimNetworkConfigurationChangedStateChange`，并且，完成此异步过程后，将提供最终 `XimMatchmakingProgressUpdatedStateChange` 和 `MatchmakingStatus.None`，表明不会再向 XIM 网络提供任何匹配的玩家。

当通过声明两个或更多团队的 `XimNetworkConfiguration.TeamConfiguration` 设置启用回填匹配时，所有现有玩家的有效团队索引必须在 1 到团队数之间。 这包括调用 `XimLocalPlayer.SetTeamIndex()` 来指定自定义值，或使用邀请或通过其他社交方式（例如，调用 `XboxIntegratedMultiplayer.MoveToNetworkUsingJoinableXboxUserId()`）加入，并通过默认团队索引值 0 添加的玩家。 如果任意玩家没有有效的团队索引，则会暂停匹配过程，并为所有参与者提供值为 `MatchmakingStatus.WaitingForPlayerTeamIndex` 的 `XimMatchmakingProgressUpdatedStateChange`。 在所有玩家通过 `XimLocalPlayer.SetTeamIndex()` 提供或更正其团队索引值后，回填匹配将继续。 有关详细信息，请参阅本文档 [XIM 如何与玩家团队配合工作](#how-xim-works-with-player-teams)部分。

同样，当通过 `XimNetworkConfiguration` 结构启用回填匹配，并将 `RequirePlayerRolesAndSkillConfiguration` 字段设置为 true 时，所有玩家必须已指定非 null 按玩家匹配配置。 如果任意玩家未执行此操作，则会暂停匹配过程，并为所有参与者提供值为 `XimMatchMakingStatus.WaitingForPlayerRolesAndSkillConfiguration` 的 `XimMatchmakingProgressUpdatedStateChange`。 所有玩家都提供其 `XimPlayerRolesAndSkillConfiguration` 对象后，回填匹配将继续。 有关详细信息，请参阅本文档[使用按玩家技能匹配](#matchmaking-using-per-player-skill)和[使用按玩家角色匹配](#matchmaking-using-per-player-role)部分。

## <a name="querying-joinable-networks"></a>查询可加入的网络

匹配是将玩家快速连接在一起的一种很好的方法，有时最好允许玩家通过使用自定义搜索条件发现可加入的网络，并选择他们希望加入的网络。 这在游戏会话可能有大量可配置游戏规则和玩家首选项时尤具优势。 为此，必须先将现有网络设置为可查询，方法是启用 `XimAllowedPlayerJoins.Queried` 可加入性，并通过调用 `XboxIntegratedMultiplayer.SetNetworkConfiguration()` 配置可供网络外部的其他人访问的网络信息。

下面的示例启用 `XimAllowedPlayerJoins.Queried` 可加入性，设置以下网络配置：一个允许总共 1-8 位玩家加入 1 个团队的团队配置；一个特定于应用的游戏模式常量 uint64_t（由 GAME_MODE_BRAWL 值定义）；一个描述 "cat and sheep's boxing match"；一个特定于应用的地图索引常量 uint32_t（由 MAP_KITCHEN 值定义）；并包含三个标记 "chatrequired"、"easy"、"spectatorallowed"：

```cs
string[] tags = { "chatrequired", "easy", "spectatorallowed" };
var networkConfiguration = new XimNetworkConfiguration(XboxIntegratedMultiplayer.NetworkConfiguration);
networkConfiguration.AllowedPlayerJoins |= XimAllowedPlayerJoins.Queried;
networkConfiguration.TeamConfiguration.TeamCount = 1;
networkConfiguration.TeamConfiguration.MinPlayerCountPerTeam = 1;
networkConfiguration.TeamConfiguration.MaxPlayerCountPerTeam = 8;
networkConfiguration.CustomGameMode = GAME_MODE_BRAWL;
networkConfiguration.Description = "Cat and sheep's boxing match";
networkConfiguration.MapIndex = MAP_KITCHEN;
networkConfiguration.SetTags(tags);
XboxIntegratedMultiplayer.SetNetworkConfiguration(networkConfiguration);
```

然后，网络外部的其他玩家可以通过一组与前面 xim::set_network_configuration() 调用中的网络信息匹配的筛选器来调用 xim::start_joinable_network_query()，从而找到该网络。 下面的示例启动一个可加入网络查询，其中的游戏模式筛选器选项将只查询使用由 GAME_MODE_BRAWL 值定义的应用特定游戏模式的网络：

```cs
XimJoinableNetworkQueryFilters queryFilters = new XimJoinableNetworkQueryFilters();
queryFilters.CustomGameModeFilter = GAME_MODE_BRAWL;
XboxIntegratedMultiplayer.StartJoinableNetworkQuery(queryFilters);
```

下面是另一个示例，它使用标记筛选器选项查询在公共可查询配置中包含 "easy" 和 "spectatorallowed" 标记的网络：

```cs
string[] tagFilters = { "easy", "spectatorallowed" };
XimJoinableNetworkQueryFilters queryFilters = new XimJoinableNetworkQueryFilters();
queryFilters.SetTagFilters(tagFilters);
XboxIntegratedMultiplayer.StartJoinableNetworkQuery(queryFilters);
```

可以结合使用不同的筛选器选项。 下面的示例同时使用游戏模式筛选器选项和标记筛选器选项启动一个查询，以查询既有应用特定游戏模式常量 GAME_MODE_BRAWL 又包含 "easy" 标记的网络：

```cs
string[] tagFilters = { "easy" };
XimJoinableNetworkQueryFilters queryFilters = new XimJoinableNetworkQueryFilters();
queryFilters.CustomGameModeFilter = GAME_MODE_BRAWL;
queryFilters.SetTagFilters(tagFilters);
XboxIntegratedMultiplayer.StartJoinableNetworkQuery(queryFilters);
```

如果查询操作成功，应用将收到一个 xim_start_joinable_network_query_completed_state_change，该应用可以从中检索可加入网络的列表。 该应用将会继续接收其他可加入网络的 `XimJoinableNetworkUpdatedStateChange` 或是返回的可加入网络列表发生的任何更改，直到手动或自动停止查询。 可以通过调用 `XboxIntegratedMultiplayer.StopJoinableNetworkQuery()` 手动停止进行中的查询。 当调用 `XboxIntegratedMultiplayer.StartJoinableNetworkQuery()` 以启动新查询时，查询将自动停止。

应用可以通过调用 `XboxIntegratedMultiplayer.MoveToNetworkUsingJoinableNetworkInformation()` 尝试加入可加入网络列表中的网络。 下面的示例假定你想要加入 'selectedNetwork' 所引用的网络（无密码保护，因此我们向第二个参数传入空字符串）：

```cs
XboxIntegratedMultiplayer.MoveToNetworkUsingJoinableNetworkInformation(selectedNetwork, "");
```

当通过 `XimNetworkConfiguration.TeamConfiguration`（声明了两个或更多团队）启用网络查询时，通过调用 XboxIntegratedMultiplayer.MoveToNetworkUsingJoinableNetworkInformation() 加入的玩家将拥有默认团队索引值 0。