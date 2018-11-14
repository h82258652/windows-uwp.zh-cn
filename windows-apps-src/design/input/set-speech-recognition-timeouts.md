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
ms.localizationpriority: medium
ms.openlocfilehash: 00923b4448d96943cf00eade46c39c42e87c4f96
ms.sourcegitcommit: 4d88adfaf544a3dab05f4660e2f59bbe60311c00
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/12/2018
ms.locfileid: "6471392"
---
# <a name="set-speech-recognition-timeouts"></a>设置语音识别超时


设置语音识别器忽略静音或无法识别的声音（干扰）并继续侦听语音输入的时长。

> **重要 API**：[**Timeouts**](https://msdn.microsoft.com/library/windows/apps/dn653253)、[**SpeechRecognizerTimeouts**](https://msdn.microsoft.com/library/windows/apps/dn653230)

## <a name="set-a-timeout"></a>设置超时


我们在此处指定各种 [**Timeouts**](https://msdn.microsoft.com/library/windows/apps/dn653253) 值：

-   InitialSilenceTimeout - SpeechRecognizer 检测静默（在生成任何识别结果之前），并假定语音输入尚未进行的时长。
-   BabbleTimeout - SpeechRecognizer 先继续侦听无法识别的声音（干扰），之后再假定语音输入已结束并结束识别操作的时长。
-   EndSilenceTimeout - SpeechRecognizer 检测静默（在生成任何识别结果之后），并假定语音输入已结束的时长。

**请注意**可以基于每个识别器设置超时。

 

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
 

 




