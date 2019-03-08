---
title: Xbox 集成的多人游戏
description: 了解 Xbox 集成多人游戏 (XIM)，这是一个针对 Xbox Live 游戏的一体化多人游戏/网络/聊天解决方案。
ms.assetid: edbb28e6-1b6c-4f12-a9c6-fa8961de99a8
ms.date: 01/25/2018
ms.topic: article
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one, xbox 集成多人游戏
localizationpriority: medium
ms.openlocfilehash: aa82b1042f710d81d83b98767802c7538fd53fba
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2019
ms.locfileid: "57641772"
---
# <a name="xbox-integrated-multiplayer-xim"></a>Xbox 集成多人游戏 (XIM)

- [概述](#overview)
- [概念](#concepts)
- [功能](#features)
- [与其他模块的关系](#relationship-to-other-modules)

## <a name="overview"></a>概述

Xbox 集成多人游戏 (XIM) 是一个独立式界面，可通过 Xbox Live 服务的功能向游戏轻松添加多人游戏实时网络和聊天通信。 XIM 接口不需要项目在使用 C++/CX 还是传统 C++ 进行编译之间作出选择；该接口可与以上两者一起使用。 此外，该实现也不会将引发异常作为报告非致命错误的手段，因此，你可以根据需要在无异常项目中轻松使用它。

要开始，请参阅[使用 XIM](xbox-integrated-multiplayer/using-xim.md)。 如果使用的C#，请参阅[使用 XIM C# ](xbox-integrated-multiplayer/using-xim-cs.md)。

## <a name="concepts"></a>概念

XIM 面向的是几个关键概念：

- XIM 网络-一组相互关联的用户参与特定的多玩家体验，以及描述该集合的基本状态的逻辑表示形式。 参与者一次只能在一个 XIM 网络中，但可从一个概念 XIM 网络无缝移动到另一个概念 XIM 网络。
- 匹配的可选发现其他具有类似兴趣或技能水平而无需现有社交关系加入 XIM 网络的远程播放机的过程。
- 查询-而无需现有社交关系参与者之间网络发现 XIM 的可选过程。
- `xim_player` -表示单个人类用户本地或远程设备上登录并参与 XIM 网络的对象。 加入、离开然后重新加入同一个 XIM 网络的单个物理用户被视为是两个单独的玩家实例。
- `xim_state_change` -A 结构，它表示 XIM 网络到本地设备有关的某些方面的异步更改的通知。
- `xim::start_processing_state_changes` 并`xim::finish_processing_state_changes`-对方法调用的应用程序执行异步操作以检索结果的形式处理每个 UI 帧`xim_state_change`结构，然后释放完成后，关联的资源。

在非常高的级别，游戏应用程序使用 XIM 库来配置一组用户登录的本地设备上要作为新的播放机移入 XIM 网络中。 应用程序调用`xim::start_processing_state_changes`和`xim::finish_processing_state_changes`每个 UI 框架。 由于在远程设备上的应用程序实例将用户添加到 XIM 网络，提供每个参与实例`xim_state_change`描述本地和远程更新`xim_player`s 加入该 XIM 网络。 当玩家停止参与 XIM 网络（正常停止或由于网络连接问题停止）时，会向指示 `xim_player` 已离开的所有应用实例提供 `xim_state_change` 更新。

应用可以通过多种方法确定要参与的 XIM 网络。 通常，应用会通过将本地用户自动移动到可用于用户好友的新网络中启动，在新网络中，本地用户可以发送邀请，或以可加入活动形式（例如，通过玩家卡）发现其 XIM 网络。 当这些通过社交方式发现的用户就绪后，应用可以启动 Xbox Live“匹配”进程，并将所有玩家移动到新的 XIM 网络中，此网络还包括其他“匹配的”远程玩家，可根据需要填写团队/对手列表。 然后，当此多人游戏体验完成后，应用实例可以将其本地玩家（也可以选择原始的预匹配远程玩家）重新移动到新的专用 XIM 网络中，或移动到通过匹配发现的其他随机 XIM 网络中。 语音和文本聊天在整个过程中保持可用。 这种将玩家在 XIM 网络间移动的便利性是 API 的核心，反映了对优质、高度社交游戏体验的现代期望。

而不是客户端-服务器模型，XIM 网络在逻辑上就完全连接的对等设备网格。 本文档的一部分中所述，可以直接向任何其他通过 API 发送任何播放机。 可以通过任何参与设备调用会影响 XIM 网络作为一个整体的状态的所有方法。

如果应用不阻止多个参与者同时有效地修改同一个 XIM 网络状态，XIM 会使用简单的最后写入优先冲突解决方案。 这意味着 XIM 不会施加"host"或"服务器"的任何角色概念。 XIM 也不会限制使用其自己的概念，例如应用程序定义角色迁移到另一个参与者时播放机离开 XIM 网络支持的应用。

## <a name="features"></a>功能

- 为游戏提供了语音和短观察到的并且遵循 player 隐私设置的聊天通信

    在隐私设置和应用配置允许的所有玩家中，还会自动提供语音和文本聊天通信。 对于已启用语音到文本或文本到语音转换的玩家，XIM 将以透明方式执行此转换，以分别传递表示传入语音音频的聊天文本消息，并为传出聊天文本消息播放合成语音文本。

- 允许游戏来发送其自己的游戏特定于数据消息

    在 XIM 网络中，应用可以发送其各自特定于游戏的数据消息，例如，虚拟形象移动更新。 所有接收的消息都会作为指示预期源和本地目标的 `xim_state_change` 传递给应用。

- 通过带外保留的专用的聊天解决方案的功能

    有关通过带外保留使用 XIM 的详细文档，请参阅 [XIM 保留](xbox-integrated-multiplayer/xim-reservations.md)。

- 无异常的和可用于 C + + /CX 或传统 c + +

## <a name="relationship-to-other-modules"></a>与其他模块的关系

XIM 旨在为具有多人游戏基本需求的游戏提供便利、一体化的界面。 它将多个模块（特别是 Xbox 服务 API (XSAPI) `multiplayer_manager` 模块、`xbox::services::game_chat_2` 库和 `Windows::Networking::XboxLive` 安全多人游戏网络）的功能封装为一个单一简化的 API。 当生成不需要绝对的最大灵活性或控制的多人游戏时，这会减少典型的所涉及代码、任务和概念的数量。 但是，要求与 XIM 的简化假定不一致的应用可能想要改为直接使用这些组件。

虽然 XIM 旨在通过基础组件消除管理多人游戏会话目录 (MPSD) 会话文档或网络传输等内容的需要，但它不会阻止将同时/并行使用两者作为单独玩家名单或通信网格的一部分。 在这种情况下，应用要负责确保 XIM 与其自身的机制之间的协作式网络资源使用。 XIM 目前支持“带外保留”，可轻松地用作专门的聊天解决方案，这种解决方案只由外部输入驱动用户列表。

Xbox Live 提供的很多其他功能，对于多人游戏而言非常有价值，但不太直接涉及设置多人游戏聊天和网络通信，因此，此模块不会包装这些功能。 建议应用到 Xbox 服务 API (XSAPI) 查找的播放机成就、 排行榜、 存储和更多。
