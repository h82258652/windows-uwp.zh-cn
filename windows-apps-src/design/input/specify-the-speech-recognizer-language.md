---
Description: 了解如何选择要用于语音识别的安装语言。
title: 指定语音识别器语言
ms.assetid: 4C463A1B-AF6A-46FD-A839-5D6724955B38
label: Specify the speech recognizer language
template: detail.hbs
keywords: 语音，语音，语音识别，自然语言，听写，输入，用户交互
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 778aa04861fa7704f4235763a429bb77f92a8b65
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/29/2019
ms.locfileid: "66365328"
---
# <a name="specify-the-speech-recognizer-language"></a>指定语音识别器语言


了解如何选择要用于语音识别的安装语言。

> **重要的 API**：[**SupportedTopicLanguages**](https://docs.microsoft.com/uwp/api/windows.media.speechrecognition.speechrecognizer.supportedtopiclanguages)， [ **SupportedGrammarLanguages**](https://docs.microsoft.com/uwp/api/windows.media.speechrecognition.speechrecognizer.supportedgrammarlanguages)， [**语言**](https://docs.microsoft.com/uwp/api/Windows.Globalization.Language)


此处，我们枚举了已安装在系统上的语言、标识了默认语言，并选择了不同的语言以供识别。

**先决条件：**

本主题围绕[语音识别](speech-recognition.md)展开。

你应该已经大致了解了语音识别和识别约束。

如果你还不熟悉通用 Windows 平台 (UWP) 应用开发，请仔细阅读这些主题来熟悉此处讨论的技术。

-   [创建你的第一个应用](https://docs.microsoft.com/windows/uwp/get-started/your-first-app)
-   借助[事件和路由事件概述](https://docs.microsoft.com/windows/uwp/xaml-platform/events-and-routed-events-overview)了解事件

**用户体验指南：**

有关设计出既实用又有吸引力且支持语音的应用的有用提示，请参阅[语音设计指南](https://docs.microsoft.com/windows/uwp/input-and-devices/speech-interactions)。

## <a name="identify-the-default-language"></a>识别默认语言


语音识别器使用系统语音语言作为其默认识别语言。 用户可在设备的“设置”&gt;“系统”&gt;“语音”&gt;“语音语言”屏幕上设置此语言。

我们通过查看 [**SystemSpeechLanguage**](https://docs.microsoft.com/uwp/api/windows.media.speechrecognition.speechrecognizer.systemspeechlanguage) 静态属性来识别默认语言。

```CSharp
var language = SpeechRecognizer.SystemSpeechLanguage; 
```

## <a name="confirm-an-installed-language"></a>确认已安装的语言


已安装的语言在不同的设备之间可能会不同。 如果对于特定的约束你依赖于某种语言，你应验证是否存在该语言。

**请注意**  安装新的语言包后需要重新启动。 异常，错误代码为 SPERR\_不\_如果指定的语言不受支持或尚未完成安装，则会引发找到 (0x8004503a)。

 

通过检查 [**SpeechRecognizer**](https://docs.microsoft.com/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognizer) 类的下列两种静态属性之一，来确定设备上受支持的语言：

-   [**SupportedTopicLanguages**](https://docs.microsoft.com/uwp/api/windows.media.speechrecognition.speechrecognizer.supportedtopiclanguages)— 的集合[**语言**](https://docs.microsoft.com/uwp/api/Windows.Globalization.Language)与预定义的听写和 web 搜索语法一起使用的对象。

-   [**SupportedGrammarLanguages**](https://docs.microsoft.com/uwp/api/windows.media.speechrecognition.speechrecognizer.supportedgrammarlanguages)— 的集合[**语言**](https://docs.microsoft.com/uwp/api/Windows.Globalization.Language)与列表约束或语音识别语法规范 (SRGS) 文件一起使用的对象。

## <a name="specify-a-language"></a>指定语言


要指定一种语言，请将 [**Language**](https://docs.microsoft.com/uwp/api/Windows.Globalization.Language) 对象传入 [**SpeechRecognizer**](https://docs.microsoft.com/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognizer) 构造函数。

此处，我们将“en-US”指定为识别语言。


```CSharp
var language = new Windows.Globalization.Language(“en-US”); 
var recognizer = new SpeechRecognizer(language); 
```

## <a name="remarks"></a>备注


可对主题约束进行配置，方法是将 [**SpeechRecognitionTopicConstraint**](https://docs.microsoft.com/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognitionTopicConstraint) 添加到 [**SpeechRecognizer**](https://docs.microsoft.com/uwp/api/windows.media.speechrecognition.speechrecognizer.constraints) 的 [**Constraints**](https://docs.microsoft.com/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognizer) 集合，然后调用 [**CompileConstraintsAsync**](https://docs.microsoft.com/uwp/api/windows.media.speechrecognition.speechrecognizer.compileconstraintsasync)。 如果识别器未使用支持的主题语言进行初始化，则将返回 **TopicLanguageNotSupported** 的 [**SpeechRecognitionResultStatus**](https://docs.microsoft.com/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognitionResultStatus)。

可对列表约束进行配置，方法是将 [**SpeechRecognitionListConstraint**](https://docs.microsoft.com/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognitionListConstraint) 添加到 [**SpeechRecognizer**](https://docs.microsoft.com/uwp/api/windows.media.speechrecognition.speechrecognizer.constraints) 的 [**Constraints**](https://docs.microsoft.com/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognizer) 集合，然后调用 [**CompileConstraintsAsync**](https://docs.microsoft.com/uwp/api/windows.media.speechrecognition.speechrecognizer.compileconstraintsasync)。 你无法直接指定自定义列表的语言。 该列表将改为使用识别器的语言进行处理。

SRGS 语法是由 [**SpeechRecognitionGrammarFileConstraint**](https://docs.microsoft.com/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognitionGrammarFileConstraint) 类表示的开放式标准 XML 格式。 与自定义列表不同，你可以在 SRGS 标记中指定语法语言。 [**CompileConstraintsAsync** ](https://docs.microsoft.com/uwp/api/windows.media.speechrecognition.speechrecognizer.compileconstraintsasync)失败，出现[ **SpeechRecognitionResultStatus** ](https://docs.microsoft.com/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognitionResultStatus)的**TopicLanguageNotSupported**如果识别器不会初始化为 SRGS 标记相同的语言。

## <a name="related-articles"></a>相关文章

**开发人员**

* [语音交互](speech-interactions.md)

**设计器**

* [语音设计指南](https://docs.microsoft.com/windows/uwp/input-and-devices/speech-interactions)

**示例**

* [语音识别和语音合成示例](https://go.microsoft.com/fwlink/p/?LinkID=619897)
 

 




