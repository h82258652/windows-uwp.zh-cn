---
title: 游戏聊天 2 概述
author: KevinAsgari
description: 了解如何使用 Xbox Live 游戏聊天 2（游戏聊天的更新版本）将语音通信添加到游戏中。
ms.author: tomco
ms.date: 10/20/2017
ms.topic: article
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one, 游戏聊天, 游戏聊天 2, 语音通信
ms.localizationpriority: medium
ms.openlocfilehash: 211769593fb2801aee31cfcd8b20869fab27314e
ms.sourcegitcommit: ca96031debe1e76d4501621a7680079244ef1c60
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/31/2018
ms.locfileid: "5834480"
---
# <a name="game-chat-2-overview"></a>游戏聊天 2 概述

游戏聊天 2 允许你轻松地将语音和文本聊天通信添加到你的应用，同时尊重玩家的隐私设置并满足 Xbox One 游戏和与语音和文字聊天的中心应用的 Xbox 要求。 已启用语音到文本或文本到语音转换通过轻松使用-游戏聊天脚本设置的玩家游戏聊天 2 将以透明方式执行转换以创建表示传入语音音频和播放的文本消息的聊天分别合成传出聊天文本消息的语音音频。

- **通信关系**-游戏聊天 2 为你提供了你的玩家可以如何与相互通信的细化控制。 而不是指定团队或频道，游戏聊天 2 需要定义的每个对用户之间的明确关系。 游戏聊天 2 通信关系支持单向和双向任意一对玩家之间的通信。 可以相互独立地配置语音和文本通信关系。

- **辅助功能**的游戏聊天 2 支持语音到文本和文本到语音转换。 启用后，游戏聊天 2 将尊重玩家的"游戏聊天脚本"首选项，并将以透明方式执行转换，以创建表示传入语音音频，并为传出聊天文本播放合成语音音频的文本消息的聊天消息，分别。

- **Xbox Live 集成**的游戏聊天 2 使用 Xbox Live 服务，确保尊重每个玩家的首选项和特权。

- **语音活动检测**-游戏聊天 2 执行语音活动检测，以确定语音数据何时包含语音活动。

- **自动增益控制**-游戏聊天 2 执行自动增益控制，以最小化用户麦克风输出中。

- **编解码器**-游戏聊天 2 会将必须传递到应用的远程实例的音频数据编码。 在 Xbox One 上，此编码（和接收端上的编码）采用硬件加速。

- `chat_manager::start/finish_processing_state_changes` - 在每个 UI 框架由应用调用，以便执行异步操作、检索采用 `game_chat_state_change` 结构格式处理的结果，然后在完成时释放关联的资源的方法对。

- `chat_manager::start/finish_processing_data_frames` -用于游戏聊天 2 插入应用的传输层的方法对。 这些方法在每个网络框架由应用调用，以便检索 `game_chat_data_frame` 对象并将其分发到远程设备上的应用的实例，然后在完成时释放关联的资源。

- `chat_manager::process_incoming_data` -用于向游戏聊天 2 从游戏聊天 2 的远程实例提供通过应用的传输层的数据的方法。

- **实时音频操作**-游戏聊天 2 允许应用在聊天音频管道中插入本身，以便它可能会检查或操作聊天音频数据。 有关详细信息，请参阅[实时音频操作](real-time-audio-manipulation.md)。

应用会通知有望一起聊天的本地设备和远程设备上的用户的库。 然后，应用会配置每个用户之间的关系。

配置后，游戏聊天 2 轮询本地用户的麦克风的音频、 执行自动增益控制和语音活动检测、 编码数据，然后公开要传递给通过游戏聊天 2 的远程实例的音频数据`chat_manager::start/finish_processing_data_frames`。 应用必须提供中包含的数据`game_chat_state_change`给相同对象中指定的游戏聊天 2 的远程实例的对象。 在收到数据后，应用的远程实例必须提交到游戏聊天 2 通过自身实例的数据`chat_manager::process_incoming_data`。 游戏聊天 2 会解码传入的数据，然后将其呈现给本地用户的音频设备。

游戏聊天 2 强制执行 Xbox Live 特权和隐私要求。 音频数据不会由没有通信特权的用户生成，并且数据音频不会从通过隐私设置阻止的用户呈现。

要开始使用，请参阅[使用游戏聊天 2](using-game-chat-2.md)。 如果你使用的 C#，请参阅[使用游戏聊天 2 WinRT 投影](using-game-chat-2-winrt.md)。 如果你已经有了游戏聊天实现，使用[游戏聊天 2 迁移指南](game-chat-2-migration.md)游戏聊天概念和调用模式映射到游戏聊天 2 类似。
