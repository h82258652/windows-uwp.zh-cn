---
author: KevinAsgari
title: 可访问的游戏内聊天概述
description: 了解 Xbox Live 游戏聊天辅助功能。
ms.assetid: ecd84fa1-9b28-414b-b5e1-daba68de9472
ms.author: kevinasg
ms.date: 04/04/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one, 游戏聊天, 辅助功能, 文本到语音转换, 语音到文本转换
ms.localizationpriority: low
ms.openlocfilehash: 7f2236931add4a0a812ff10e06431f4b4cdb8c5b
ms.sourcegitcommit: 01760b73fa8cdb423a9aa1f63e72e70647d8f6ab
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/24/2018
---
#  <a name="accessible-in-game-chat-overview"></a>可访问的游戏内聊天概述

以下是对**游戏聊天**组件提供的可访问聊天功能的概述。

为了帮助你改善游戏内交流这一辅助功能，我们提供了两个新功能&mdash;文本到语音转换 (TTS) 和语音到文本转换 (STT)。

你应考虑同时启用这两个功能，以便有听力或语言障碍的玩家能够更容易地访问你的主题作品。

## <a name="text-to-speech-tts"></a>文本到语音转换 (TTS)

TTS 描述了从文本转换到语音的操作；使用合成语音读出文本消息。 在**游戏聊天**中使用 TTS，你的主题作品可以自动合成音频流，读出用户发送的文本流。 可以向其他活跃聊天用户发送文本及合成的音频流。 此功能将使有语言障碍的玩家能够积极地参与游戏聊天，即便是你的主题作品没有基于文本的聊天系统。

由于**游戏聊天**组件不会提供任何其他 API 来捕获玩家输入的文本，所以你的主题作品仍要负责提供自定义文本输入字段或启用 Xbox 平台提供的虚拟键盘。

**HasRequestedSynthesizedAudio** 是 **ChatUser** 类中的一个新属性，**Microsoft.Xbox.GameChat** 命名空间的一部分。 用户可以通过 Xbox 的**轻松使用**设置设定此值。

当用户加入当前聊天会话时，你的主题作品需要侦听 **ChatUser** 类中的 **OnAccessibilitySettingsChanged** 事件。 聊天辅助功能设置一经更改，便会引发此事件。 引发 **OnAccessibilitySettingsChanged** 事件时，请检查 **HasRequestedSynthesizedAudio** 的值。

如果 **HasRequestedSynthesizedAudio** 被设置为 **TRUE**，则这意味着 TTS 已启用，用户已请求向所有活跃用户发送文本消息，以将读出文本消息的语音数据包包括在内。

下表展示了用户的 TTS 设置（由 **HasRequestedSynthesizedAudio** 的属性值指示）如何影响聊天中文本消息的处理方式。

|HasRequestedSynthesizedAudio  |发送给所有远程用户的数据类型                                                 |
|------------------------------|--------------------------------------------------------------------------------------|
|**FALSE**                     |仅文本                                                                             |
|**TRUE**                      |文本和合成语音                                                       |

**allowTextToSpeechConversion** 是 **GenerateTextMessage** API 的一个输入参数。 此 API 还是 **Microsoft.Xbox.GameChat** 命名空间中 **ChatUser** 类的一部分。 此参数由你的主题作品设置，并决定是否希望利用将文本转换为语音的内置功能。

如果 **allowTextToSpeechConversion** 为 **TRUE**，则**游戏聊天**将自动处理该文本消息，生成一个合成的语音数据包。 使用用户在其设置（Zira、Marcus 或 David）中选择的语音配置文件，可创建合成的语音数据。
若设置为 **FALSE**，则这意味着你的主题作品计划让你自行处理转换，或者不希望**游戏聊天**代表你自动生成这些合成的语音数据包。

由于 **allowTextToSpeechConversion** 的属性值决定了**游戏聊天**是否将文本自动转换为语音，下表展示了该属性值是如何影响发送给所有远程用户的最终数据类型的。

|allowTextToSpeechConversion   |发送给所有远程用户的数据类型                                                                                                               |
|------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------|
|**FALSE**                     |无论 TTS 启用与否，仅发送文本                                                                                               |
|**TRUE**                      |只有 **HasRequestedSynthesizedAudio** 为 **TRUE** 时，才同时发送文本和合成的语音                                                       |

### <a name="example-flow"></a>示例流程

当用户加入聊天会话时，你的主题作品会检查 **ChatUser** 类中的 **HasRequestedSynthesizedAudio** 属性。 该值表明用户是否希望将文本转换为语音发送。

当 `HasRequestedSynthesizedAudio = True` 时：

* 提供一种方式，以便用户输入文本以进行 TTS 转换。

* 调用 **GenerateTextMessage** 函数，从用户传入文本消息，决定是否需要作为输入参数的文本消息的语音合成。 在大多数情况下，第二个参数将始终为 true。

* 响应现将包含原始文本和合成的语音数据的 **OnOutgoingChatPacketReady** 事件。

接收包含合成语音数据的聊天包时：

* 接收数据时，调用 **ProcessIncomingChatMessage**。

* 对于每个已处理的聊天包，**游戏聊天**将引发包含 **ChatTextMessageType** 参数的 **OnTextMessageReceived** 事件。 此参数提供关于收到的消息类型的信息。

* 如果消息类型被标识为 **ChatSynthesizedVoiceDataMessage**，则将以处理录制的语音数据包的方式处理合成的语音消息。

## <a name="speech-to-text-stt"></a>语音到文本转换 (STT)

STT 描述了提取语音并将其转录为文本的操作。 在**游戏聊天**中，用户可以从其他在聊天中使用语音交流的用户接收文本转录。 此功能不仅可以让有听力障碍的玩家进行语音聊天，还可以让那些关掉游戏音量的用户参与语音聊天。

**HasRequestedTranscription** 是 **ChatUser** 类中的一个新属性，**Microsoft.Xbox.GameChat** 命名空间的一部分。 用户可以通过 Xbox 的**轻松使用**设置设定此值。

当用户加入当前聊天会话时，你的主题作品需要侦听 **ChatUser** 类中的 **OnAccessibilitySettingsChanged** 事件。 聊天辅助功能设置一经更改，便会引发此事件。 引发 **OnAccessibilitySettingsChanged** 事件时，请检查 **HasRequestedTranscription** 的值。

如果 **HasRequestedTranscription** 被设置为 **TRUE**，则这意味着 STT 已启用，所有远程用户会将其语音数据发送到 Microsoft 语音服务进行转录，然后再将语音和文本响应发送给本地用户。

计划使用自定义 UI 呈现文本聊天的主题作品可能需要检查此值，以便提前准备其屏幕，但是这不是必需的。

下表展示了用户的 STT 设置（由 **HasRequestedTranscription** 的属性值指示）如何影响从其他活跃聊天用户发来的语音消息。

|HasRequestedTranscription     |用户收到的数据类型                                                         |
|------------------------------|--------------------------------------------------------------------------------------|
|**FALSE**                     |仅语音                                                                            |
|**TRUE**                      |转录的文本和语音                                                       |


### <a name="to-render-a-transcribed-text-message"></a>呈现转录的文本消息

启用 STT 后，用户将收到转录的文本消息，该消息的类型为 **TranscribedText**，你的主题作品必需使用以下两种方式之一将文本呈现到屏幕上：

* 主题作品可调用 UI (TCUI)

  你可以使用 TCUI 将转录的文本呈现在主题作品的屏幕上，而 TCUI 由 shell 和平台提供。 此 UI 是一种具有固定高度和宽度的叠加。 其可置于屏幕上八个可能的位置之一，如下所示。

  ![八个可能的 TCUI 呈现位置](../../images/multiplayer/tcui-render-positions.png)

  你的主题作品将需要向 API 发送要显示的三条信息：说话者的姓名、转录的文本以及指示文本消息的创建方式（即通过实际的文本输入或 STT 创建）的标识符。 对于通过实际的文本输入创建的文本消息，将显示键盘标识符；对于转录的文本消息，将显示耳机标识符。 因为这是一种基于 shell 的叠加，因此它继承了所有的 shell 行为，并且用户无法与其交互。

  对于在通用 Windows 平台 (UWP) 上开发的主题作品，此叠加预期在下一个主要更新中推出。 在这之前，你可以使用游戏内 UI（下文会有所介绍）来呈现转录的文本消息。

* 游戏内 UI

  如果你希望主题作品可以更好地控制不同设备（Xbox、PC 或任何 Windows 10 设备）上的 UI 体验，那么你可以创建自定义游戏内 UI 来显示聊天文本。

### <a name="example-flow"></a>示例流程

启动后，便可以查看 **ChatUser** 类中的 **HasRequestedTranscription** 属性。 你的主题作品可以根据此设置得知用户是否启用了语音到文本转换功能，从而决定如何处理聊天会话期间发送给用户的语音和文本数据。

当启用了 STT 的用户收到含转录的文本字符串的聊天包时：
* 发送给用户的**聊天数据包**包含语音数据和处理了语音数据后收到的转录字符串文本。
* 对于每个已处理的聊天包，将引发 **OnTextMessageReceived** 事件。 此事件包含 **ChatTextMessageType** 参数，该参数提供关于收到的消息类型的信息。 如果消息类型被标识为 **TranscribedText**，则使用之前列出的两种方式&mdash;TCUI 或等效的游戏内 UI 之一将转录的文本消息呈现在屏幕上。

## <a name="related-links"></a>相关链接

* [游戏聊天概述](gamechat-overview.md)
