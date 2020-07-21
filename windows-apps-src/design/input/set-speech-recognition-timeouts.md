---
Description: 设置语音识别器忽略静音或无法识别的声音（干扰）并继续侦听语音输入的时长。
title: 设置语音识别超时
ms.assetid: 58F446AC-4A56-454D-8125-62A2C4DBFCC8
label: Speech recognition timeouts
template: detail.hbs
keywords: 语音，语音，语音识别，自然语言，听写，输入，用户交互
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 0df5f6c2e12b3b2e761ce45f95930dc179ef367f
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/20/2019
ms.locfileid: "74258254"
---
# <a name="set-speech-recognition-timeouts"></a>设置语音识别超时


设置语音识别器忽略静音或无法识别的声音（干扰）并继续侦听语音输入的时长。

> **重要 API**：[**Timeouts**](https://docs.microsoft.com/uwp/api/windows.media.speechrecognition.speechrecognizer.timeouts)、[**SpeechRecognizerTimeouts**](https://docs.microsoft.com/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognizerTimeouts)

## <a name="set-a-timeout"></a>设置超时


我们在此处指定各种 [**Timeouts**](https://docs.microsoft.com/uwp/api/windows.media.speechrecognition.speechrecognizer.timeouts) 值：

-   InitialSilenceTimeout - SpeechRecognizer 检测静默（在生成任何识别结果之前），并假定语音输入尚未进行的时长。
-   BabbleTimeout - SpeechRecognizer 先继续侦听无法识别的声音（干扰），之后再假定语音输入已结束并结束识别操作的时长。
-   EndSilenceTimeout - SpeechRecognizer 检测静默（在生成任何识别结果之后），并假定语音输入已结束的时长。

**请注意**，可以根据每个识别器来设置  超时。

 

```CSharp
// Set timeout settings.
recognizer.Timeouts.InitialSilenceTimeout = TimeSpan.FromSeconds(6.0);
recognizer.Timeouts.BabbleTimeout = TimeSpan.FromSeconds(4.0);
recognizer.Timeouts.EndSilenceTimeout = TimeSpan.FromSeconds(1.2);
```

## <a name="related-articles"></a>相关文章


* [语音交互](speech-interactions.md)
**示例**
* [语音识别和语音合成示例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/SpeechRecognitionAndSynthesis)
 

 




