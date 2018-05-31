---
author: Karl-Bridge-Microsoft
Description: Set how long a speech recognizer ignores silence or unrecognizable sounds (babble) and continues listening for speech input.
title: 设置语音识别超时
ms.assetid: 58F446AC-4A56-454D-8125-62A2C4DBFCC8
label: Speech recognition timeouts
template: detail.hbs
keywords: 语音，语音，语音识别，自然语言，听写，输入，用户交互
ms.author: kbridge
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
ms.localizationpriority: medium
ms.openlocfilehash: c82e0d77804e293176c53a9d8079e32054f9cca6
ms.sourcegitcommit: 0ab8f6fac53a6811f977ddc24de039c46c9db0ad
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/15/2018
ms.locfileid: "1652746"
---
# <a name="set-speech-recognition-timeouts"></a>设置语音识别超时


设置语音识别器忽略静音或无法识别的声音（干扰）并继续侦听语音输入的时长。

> **重要 API**：[**Timeouts**](https://msdn.microsoft.com/library/windows/apps/dn653253)、[**SpeechRecognizerTimeouts**](https://msdn.microsoft.com/library/windows/apps/dn653230)

## <a name="set-a-timeout"></a>设置超时


我们在此处指定各种 [**Timeouts**](https://msdn.microsoft.com/library/windows/apps/dn653253) 值：

-   InitialSilenceTimeout - SpeechRecognizer 检测静默（在生成任何识别结果之前），并假定语音输入尚未进行的时长。
-   BabbleTimeout - SpeechRecognizer 先继续侦听无法识别的声音（干扰），之后再假定语音输入已结束并结束识别操作的时长。
-   EndSilenceTimeout - SpeechRecognizer 检测静默（在生成任何识别结果之后），并假定语音输入已结束的时长。

**注意** 可以基于每个识别器设置超时。

 

```CSharp
// Set timeout settings.
recognizer.Timeouts.InitialSilenceTimeout = TimeSpan.FromSeconds(6.0);
recognizer.Timeouts.BabbleTimeout = TimeSpan.FromSeconds(4.0);
recognizer.Timeouts.EndSilenceTimeout = TimeSpan.FromSeconds(1.2);
```

## <a name="related-articles"></a>相关文章


* [语音交互](speech-interactions.md)
**示例**
* [语音识别和语音合成示例](http://go.microsoft.com/fwlink/p/?LinkID=619897)
 

 




