---
author: Karl-Bridge-Microsoft
Description: "了解如何选择要用于语音识别的安装语言。"
title: "指定语音识别器语言"
ms.assetid: 4C463A1B-AF6A-46FD-A839-5D6724955B38
label: Specify the speech recognizer language
template: detail.hbs
ms.sourcegitcommit: a4e9a90edd2aae9d2fd5d7bead948422d43dad59
ms.openlocfilehash: 24393ef52d72aa08f9aab2d541e65ccb5f2aceed

---

# 指定语音识别器语言


了解如何选择要用于语音识别的安装语言。




**重要的 API**

-   [**SupportedTopicLanguages**](https://msdn.microsoft.com/library/windows/apps/dn653251)
-   [**SupportedGrammarLanguages**](https://msdn.microsoft.com/library/windows/apps/dn653250)
-   [**语言**](https://msdn.microsoft.com/library/windows/apps/br206804)


此处，我们枚举了已安装在系统上的语言、标识了默认语言，并选择了不同的语言以供识别。

**先决条件：  **

本主题围绕[语音识别](speech-recognition.md)展开。

你应该已经大致了解了语音识别和识别约束。

如果你还不熟悉通用 Windows 平台 (UWP) 应用开发，请查看这些主题来熟悉此处讨论的技术。

-   [创建你的第一个应用](https://msdn.microsoft.com/library/windows/apps/bg124288)
-   借助[事件和路由事件概述](https://msdn.microsoft.com/library/windows/apps/mt185584)了解事件

**用户体验指南：  **

有关设计出既实用又有吸引力且支持语音的应用的有用提示，请参阅[语音设计指南](https://msdn.microsoft.com/library/windows/apps/dn596121)。

## <span id="Identify_the_default_language"></span><span id="identify_the_default_language"></span><span id="IDENTIFY_THE_DEFAULT_LANGUAGE"></span>识别默认语言


语音识别器使用系统语音语言作为其默认识别语言。 用户可在设备的“设置”&gt;“系统”&gt;“语音”&gt;“语音语言”屏幕上设置此语言。

我们通过查看 [**SystemSpeechLanguage**](https://msdn.microsoft.com/library/windows/apps/dn653252) 静态属性来识别默认语言。

```CSharp
var language = SpeechRecognizer.SystemSpeechLanguage; </code></pre></td>
</tr>
</tbody>
</table>
```

## <span id="Confirm_an_installed_language"></span><span id="confirm_an_installed_language"></span><span id="CONFIRM_AN_INSTALLED_LANGUAGE"></span>确认已安装的语言


已安装的语言在不同的设备之间可能会不同。 如果对于特定的约束你依赖于某种语言，你应验证是否存在该语言。

**注意** 在安装新的语言包后，必须重新启动。 如果指定的语言不受支持或未完成安装过程，将引发异常，其错误代码为 SPERR\_NOT\_FOUND (0x8004503a)。

 

通过检查 [**SpeechRecognizer**](https://msdn.microsoft.com/library/windows/apps/dn653226) 类的下列两种静态属性之一，来确定设备上受支持的语言：

-   [
            **SupportedTopicLanguages**](https://msdn.microsoft.com/library/windows/apps/dn653251) - 可与预定义的听写语法和 Web 搜索语法一起使用的 [**Language**](https://msdn.microsoft.com/library/windows/apps/br206804) 对象的集合。

-   [
            **SupportedGrammarLanguages**](https://msdn.microsoft.com/library/windows/apps/dn653250) - 可与列表约束或语音识别语法规范 (SRGS) 文件一起使用的 [**Language**](https://msdn.microsoft.com/library/windows/apps/br206804) 对象的集合。

## <span id="Specify_a_language"></span><span id="specify_a_language"></span><span id="SPECIFY_A_LANGUAGE"></span>指定语言


要指定一种语言，请将 [**Language**](https://msdn.microsoft.com/library/windows/apps/br206804) 对象传入 [**SpeechRecognizer**](https://msdn.microsoft.com/library/windows/apps/dn653226) 构造函数。

此处，我们将“en-US”指定为识别语言。

<span codelanguage="CSharp"></span>
```CSharp
<colgroup>
<col width="100%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">C#</th>
</tr>
</thead>
<tbody>
<tr class="odd">
var language = new Windows.Globalization.Language(“en-US”); 
var recognizer = new SpeechRecognizer(language); 
```

## <span id="Remarks"></span><span id="remarks"></span><span id="REMARKS"></span>备注


可对主题约束进行配置，方法是将 [**SpeechRecognitionTopicConstraint**](https://msdn.microsoft.com/library/windows/apps/dn631446) 添加到 [**SpeechRecognizer**](https://msdn.microsoft.com/library/windows/apps/dn653226) 的 [**Constraints**](https://msdn.microsoft.com/library/windows/apps/dn653241) 集合，然后调用 [**CompileConstraintsAsync**](https://msdn.microsoft.com/library/windows/apps/dn653240)。 如果识别器未使用支持的主题语言进行初始化，则将返回 **TopicLanguageNotSupported** 的 [**SpeechRecognitionResultStatus**](https://msdn.microsoft.com/library/windows/apps/dn631433)。

可对列表约束进行配置，方法是将 [**SpeechRecognitionListConstraint**](https://msdn.microsoft.com/library/windows/apps/dn631421) 添加到 [**SpeechRecognizer**](https://msdn.microsoft.com/library/windows/apps/dn653226) 的 [**Constraints**](https://msdn.microsoft.com/library/windows/apps/dn653241) 集合，然后调用 [**CompileConstraintsAsync**](https://msdn.microsoft.com/library/windows/apps/dn653240)。 你无法直接指定自定义列表的语言。 该列表将改为使用识别器的语言进行处理。

SRGS 语法是由 [**SpeechRecognitionGrammarFileConstraint**](https://msdn.microsoft.com/library/windows/apps/dn631412) 类表示的开放式标准 XML 格式。 与自定义列表不同，你可以在 SRGS 标记中指定语法语言。 如果识别器未使用与 SRGS 标记相同的语言进行初始化，[**CompileConstraintsAsync**](https://msdn.microsoft.com/library/windows/apps/dn653240) 将失败并返回 **TopicLanguageNotSupported** 的 [**SpeechRecognitionResultStatus**](https://msdn.microsoft.com/library/windows/apps/dn631433)。

## <span id="related_topics"></span>相关文章


**开发人员**
* [语音交互](speech-interactions.md) 
           **设计人员**
* [语音设计指南](https://msdn.microsoft.com/library/windows/apps/dn596121) 
           **示例**
* [语音识别和语音合成示例](http://go.microsoft.com/fwlink/p/?LinkID=619897)
 

 







<!--HONumber=Jun16_HO3-->


