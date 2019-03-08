---
title: 游戏聊天 2 概述
description: 了解如何使用 Xbox Live 游戏聊天 2（游戏聊天的更新版本）将语音通信添加到游戏中。
ms.date: 10/20/2017
ms.topic: article
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one, 游戏聊天, 游戏聊天 2, 语音通信
ms.localizationpriority: medium
ms.openlocfilehash: 9f013f8b80cc7bca367c3ef5cd2c0d1da86cc98c
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2019
ms.locfileid: "57615822"
---
# <a name="game-chat-2-overview"></a>游戏聊天 2 概述

游戏聊天 2 可以轻松地遵循玩家的隐私设置和一个 Xbox 游戏和 Hub 应用程序与语音和短聊天履行 Xbox 要求时向您的应用程序添加语音和短聊天通信。 已启用通过轻松访问的游戏聊天脚本设置的语音到文本或文本到语音转换的玩家的游戏聊天 2 将以透明方式执行转换，可创建表示传入的语音音频和播放的短信的聊天分别合成语音音频传出聊天文本消息。

> [!NOTE]
> 如果您正在寻找特定的 API 的引用，您可以找到它在可下载的 Xbox Live API 编译的 HTML 帮助 (.chm) 文件中[此处](https://aka.ms/xboxliveuwpdocs)。

- **通信关系**-游戏聊天 2 提供了对玩家与其他每个可以通信的方式进行精细控制。 而不是指定的团队或频道时，游戏聊天 2 需要定义每个用户对之间的显式关系。 游戏聊天 2 通信关系支持 uni 和播放机任何对之间的双向通信。 可以相互独立地配置语音和文本通信关系。

- **可访问性**-游戏聊天 2 支持语音转文本和文本到语音转换。 启用后，游戏聊天 2 将考虑玩家的"游戏聊天听录"首选项，并将以透明方式执行转换，可创建表示传入的语音音频和传出聊天文本 play 合成语音音频的短信的聊天消息，分别。

- **Xbox Live 集成**-游戏聊天 2 使用 Xbox Live 服务可确保遵循限制每个玩家的首选项和权限。

- **语音活动检测**-游戏聊天 2 执行语音活动检测以确定当音频数据包括语音活动。

- **自动获得控制**-游戏聊天 2 执行自动获得控制，以最大程度减少用户的麦克风输出中的变体。

- **编解码器**-游戏聊天 2 对必须传递到应用的远程实例的音频数据进行编码。 在 Xbox One 上，此编码（和接收端上的编码）采用硬件加速。

- `chat_manager::start/finish_processing_state_changes` -对方法调用的应用程序执行异步操作以检索结果的形式处理每个 UI 帧`game_chat_state_change`结构，然后释放完成后，关联的资源。

- `chat_manager::start/finish_processing_data_frames` -方法用于插入到应用的传输层的游戏聊天 2 对。 这些方法在每个网络框架由应用调用，以便检索 `game_chat_data_frame` 对象并将其分发到远程设备上的应用的实例，然后在完成时释放关联的资源。

- `chat_manager::process_incoming_data` -用于为游戏聊天 2 从游戏聊天 2 的远程实例传递通过应用程序的传输层提供数据的方法。

- **实时音频操作**-游戏聊天 2 允许应用将自身插入在聊天音频管道中，以便它可以检查或操作聊天音频数据。 有关详细信息，请参阅[实时音频操作](real-time-audio-manipulation.md)。

应用会通知有望一起聊天的本地设备和远程设备上的用户的库。 然后，应用会配置每个用户之间的关系。

配置后，游戏聊天 2 轮询从本地用户的 microphone(s) 音频，执行自动增益控制和语音活动检测，将编码数据，然后公开要传递到通过游戏聊天 2 的远程实例的音频数据`chat_manager::start/finish_processing_data_frames`。 应用程序中包含的数据必须传送`game_chat_state_change`游戏聊天 2 相同的对象中指定的远程实例的对象。 在收到数据后，应用程序的远程实例必须将数据提交到其自己的游戏聊天 2 通过实例`chat_manager::process_incoming_data`。 游戏聊天 2 对传入的数据进行解码，然后将其呈现到本地用户的音频设备。

游戏聊天 2 强制 Xbox Live 的权限和隐私要求。 音频数据不会由没有通信特权的用户生成，并且数据音频不会从通过隐私设置阻止的用户呈现。

要开始使用，请参阅[使用游戏聊天 2](using-game-chat-2.md)。 如果使用的C#，请参阅[使用游戏聊天 2 WinRT 投影](using-game-chat-2-winrt.md)。 如果已有的游戏聊天实现，使用[游戏聊天 2 迁移指南](game-chat-2-migration.md)游戏聊天概念和调用模式映射到游戏聊天 2 类似物。

有关游戏聊天 2 API 的信息，请下载 Xbox API 参考：[XboxAPIs.zip](https://aka.ms/xboxliveuwpdocs)