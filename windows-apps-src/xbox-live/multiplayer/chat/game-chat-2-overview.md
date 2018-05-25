---
title: 游戏聊天 2 概述
author: KevinAsgari
description: 了解如何使用 Xbox Live 游戏聊天 2（游戏聊天的更新版本）将语音通信添加到游戏中。
ms.author: tomco
ms.date: 10/20/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one, 游戏聊天, 游戏聊天 2, 语音通信
ms.localizationpriority: low
ms.openlocfilehash: 7b475a5a34c00ee9bd7a847584a32e9be75c7ba6
ms.sourcegitcommit: ab92c3e0dd294a36e7f65cf82522ec621699db87
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/03/2018
---
# <a name="game-chat-2-overview"></a>游戏聊天 2 概述

游戏聊天 2 (GC2) 让你可以将语音和文本聊天通信轻松地添加到应用，同时尊重玩家的隐私设置，并满足 Xbox One 游戏和与语音和文本聊天有关的中心应用的 Xbox 要求。 对于已通过“轻松使用 - 游戏聊天转录”设置启用语音到文本或文本到语音转换的玩家，GC2 将以透明方式执行转换，以分别创建表示传入语音音频的聊天文本消息，并播放传出聊天文本消息的合成语音文本。

- **通信关系** - GC2 可以让你对玩家相互通信的方式进行细化控制。 GC2 需要定义每对用户之间的明确关系，而不是指定团队或频道。 GC2 通信关系支持任意一对玩家之间的单向和双向通信。 可以相互独立地配置语音和文本通信关系。

- **辅助功能** - GC2 支持语音到文本和文本到语音转换。 启用后，GC2 将尊重玩家的“游戏聊天脚本”首选项，并将以透明方式执行转换，以分别创建表示传入语音音频的聊天文本消息，并播放传出聊天文本消息的合成语音文本。

- **Xbox Live 集成** - GC2 使用 Xbox Live 服务，确保尊重每个玩家的首选项和特权。

- **语音活动检测** - GC2 执行语音活动检测，以确定语音数据何时包含语音活动。

- **自动增益控制** - GC2 执行自动增益控制，以最小化用户麦克风输出中的差异。

- **编码解码器** - GC2 会将必须传递到应用的远程实例的音频数据编码。 在 Xbox One 上，此编码（和接收端上的编码）采用硬件加速。

- `chat_manager::start/finish_processing_state_changes` - 在每个 UI 框架由应用调用，以便执行异步操作、检索采用 `game_chat_state_change` 结构格式处理的结果，然后在完成时释放关联的资源的方法对。

- `chat_manager::start/finish_processing_data_frames` - 用于将 GC2 插入应用的传输层的方法对。 这些方法在每个网络框架由应用调用，以便检索 `game_chat_data_frame` 对象并将其分发到远程设备上的应用的实例，然后在完成时释放关联的资源。

- `chat_manager::process_incoming_data` - 用于向 GC2 提供数据的方法，该数据是从 GC2 的远程实例通过应用的传输层传递过来的。

- **实时音频操作** - GC2 允许应用在聊天音频管道中插入它，以便检查或操作聊天音频数据。 有关详细信息，请参阅[实时音频操作](real-time-audio-manipulation.md)。

应用会通知有望一起聊天的本地设备和远程设备上的用户的库。 然后，应用会配置每个用户之间的关系。

配置后，GC2 会轮询本地用户的麦克风中的音频、执行自动增益控制和语音活动检测、进行数据编码，然后公开要通过 `chat_manager::start/finish_processing_data_frames` 传递给 GC2 的远程实例的音频数据。 应用必须将 `game_chat_state_change` 对象中包含的数据传递给相同对象中指定的 GC2 的远程实例。 接收数据后，应用的远程实例必须通过 `chat_manager::process_incoming_data` 将数据发布到自身的 GC2 实例。 GC2 会解码传入的数据，然后将其呈现给本地用户的音频设备。

GC2 会强制 Xbox Live 特权和隐私要求。 音频数据不会由没有通信特权的用户生成，并且数据音频不会从通过隐私设置阻止的用户呈现。

要开始使用，请参阅[使用游戏聊天 2](using-game-chat-2.md)。 如果你使用的是 C#，请参阅[使用游戏聊天 2 WinRT 投影](using-game-chat-2-winrt.md)。
