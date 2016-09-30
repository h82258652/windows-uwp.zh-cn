---
author: Karl-Bridge-Microsoft
Description: "通过语音命令扩展 Cortana 的基本功能，这些命令用于在外部应用程序中启动并执行一个单独操作。"
title: "Cortana 交互"
ms.assetid: 4C11A7CF-DA26-4CA1-A9B9-FE52670101F5
label: Cortana
template: detail.hbs
translationtype: Human Translation
ms.sourcegitcommit: a2ec5e64b91c9d0e401c48902a18e5496fc987ab
ms.openlocfilehash: d23416ad3344a39c09078b6ba3acc38fa3ba65a0

---

# UWP 应用中的 Cortana 交互




通过语音命令扩展 **Cortana** 的基本功能，这些命令用于在外部应用程序中启动并执行一个单独操作。 


**其他语音组件**

-   如果将语音识别和文本到语音转换（也称为 TTS 或语音合成）直接集成到你的应用中，请参阅[语音设计指南](speech-interactions.md)。

> **注意**  
> 语音命令是在语音命令定义 (VCD) 文件中定义的具有特定意图的单个发音，通过 **Cortana** 指向一个已安装的应用。

> VCD 文件定义一个或多个语音命令，每个语音命令都具有一种特殊意图。

> 语音命令定义的复杂程度可能有所不同。 它可以支持任何语音命令，从单个受限发音到一组更灵活的自然语言发音，所有发音都表示相同的意图。


目标应用可以在前台启动（应用获取焦点，而 **Cortana** 会被关闭）或在后台激活（**Cortana** 保留焦点，但提供来自应用的结果），具体取决于交互的复杂程度。 例如，需要额外上下文或用户输入的语音命令最好通过前台应用处理，而基本命令可通过后台应用使用 **Cortana** 进行处理。

 

通过集成应用的基本功能，并为用户提供中心入口点以便在不直接打开应用的情况下即可完成大多数任务，**Cortana** 成为了应用和用户之间的“联络人”。 通过为应用功能提供这一快捷方式并减少对应用进行切换的需求，从而为用户节省了大量的时间和精力。


<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">文章</th>
<th align="left">描述</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p>[设计指南](cortana-design-guidelines.md)</p></td>
<td align="left"><p>这些指南和建议描述了你的应用可以如何充分利用 **Cortana** 与用户交互、帮助他们完成任务，以及清楚地表明一切是如何发生的。</p></td>
</tr>
<tr class="even">
<td align="left"><p>[使用语音命令启动前台应用](launch-a-foreground-app-with-voice-commands-in-cortana.md)</p></td>
<td align="left"><p>除了在 <strong>Cortana</strong> 内使用语音命令访问系统功能以外，你还可以通过 <strong>Cortana</strong> 使用语音命令启动前台应用并在应用内指定某个要执行的操作或命令。</p></td>
</tr>
<tr class="odd">
<td align="left"><p>[动态修改 VCD 短语列表](dynamically-modify-voice-command-definition--vcd--phrase-lists.md)</p></td>
<td align="left"><p>了解如何在运行时使用语音识别结果的 VCD 文件中访问和更新受支持短语（<strong>PhraseList</strong> 元素）的列表。</p></td>
</tr>
<tr class="even">
<td align="left"><p>[使用语音命令启动后台应用](launch-a-background-app-with-voice-commands-in-cortana.md)</p></td>
<td align="left"><p>除了在 <strong>Cortana</strong> 内使用语音命令访问系统功能，你还可以使用可指定要在应用内执行的操作或命令的语音命令，通过后台应用中的特性和功能扩展 <strong>Cortana</strong>。</p></td>
</tr>
<tr class="odd">
<td align="left"><p>[与后台应用交互](interact-with-a-background-app-in-cortana.md)</p></td>
<td align="left"><p>了解用户如何在执行语音命令期间通过 <strong>Cortana</strong> 语音和 Canvas 与后台应用交互。</p></td>
</tr>
<tr class="even">
<td align="left"><p>[到后台应用的深层链接](deep-link-into-your-app-from-cortana.md)</p></td>
<td align="left"><p>从 <strong>Cortana</strong> 中的后台应用服务提供深层链接，以便在特定状态或上下文中将应用启动到前台。</p></td>
</tr>
<tr class="odd">
<td align="left"><p>[支持自然语言形式的语音命令](support-natural-language-voice-commands-in-cortana.md)</p></td>
<td align="left"><p>了解如何使用更灵活、更自然的语音命令扩展 <strong>Cortana</strong>，以便用户可以在命令中的任何位置说出应用名称。</p></td>
</tr>
</tbody>
</table>

 

## 相关文章


* [**VCD 元素和属性 v1.2**](https://msdn.microsoft.com/library/windows/apps/dn706593)

**设计人员**
* [语音设计指南](https://msdn.microsoft.com/library/windows/apps/dn596121)
* [Cortana 设计指南](https://msdn.microsoft.com/library/windows/apps/dn974233)

**示例**
* [Cortana 语音命令示例](http://go.microsoft.com/fwlink/p/?LinkID=619899)
 

 







<!--HONumber=Jun16_HO5-->


