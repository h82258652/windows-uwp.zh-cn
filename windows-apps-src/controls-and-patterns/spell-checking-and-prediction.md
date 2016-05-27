---
author: Jwmsft
Description: 在文本输入和编辑过程中，拼写检查通过使用红色波形曲线突出显示拼写错误的字词来通知用户，并提供一种供用户纠正拼写错误的方法。
title: 拼写检查和文本预测
ms.assetid: B867C956-5AB2-4207-A8DE-179CE7871180
label: Spell checking and text prediction
template: detail.hbs
---

# 拼写检查指南

在文本输入和编辑过程中，拼写检查通过使用红色波形曲线突出显示拼写错误的字词来通知用户，并提供一种供用户纠正拼写错误的方法。

**重要的 API**

-   [**IsSpellCheckEnabled 属性 (XAML)**](https://msdn.microsoft.com/library/windows/apps/br209688)


## <span id="checklist_section"></span><span id="CHECKLIST_SECTION"></span>建议


-   使用拼写检查可以在用户向文本输入控件中输入字词或句子时提供帮助。 拼写检查适用于触摸、鼠标以及键盘输入。
-   请勿在以下情况中使用拼写检查：字词可能不在词典中，或者用户不重视拼写检查。 例如，对于密码、电话号码或名称的输入框，不要打开拼写检查。 对于这些控件，拼写检查在默认情况下处于禁用状态。
-   请勿仅因为当前拼写检查引擎不支持你的应用语言就禁用拼写检查。 当拼写检查器不支持某种语言时，它不会进行任何操作，因此使该选项处于打开状态不会造成任何损害。 而且，某些用户可以使用输入法编辑器 (IME) 在你的应用中输入另一种语言，并且该语言可能受支持。 例如，在生成日语应用时，即使拼写检查引擎当前可能无法识别该语言，也不要关闭拼写检查。 用户可能会切换到英语 IME 并在应用中键入英语；如果拼写检查处于启用状态，则会进行英语拼写检查。

## <span id="Additional_usage_guidance"></span><span id="additional_usage_guidance"></span><span id="ADDITIONAL_USAGE_GUIDANCE"></span>其他使用指南


Windows 应用商店应用为多行和单行文本输入框，以及 **contentEditable** 属性设置为 **true** 的元素提供内置的拼写检查器。 下面是内置拼写检查器的示例：

![内置的拼写检查器](images/spellchecking.png)

有关详细信息，请参阅 [**TextBox 类**](https://msdn.microsoft.com/library/windows/apps/br209683)。

对于文本输入控件使用拼写检查可用于以下两个目的：

-   **自动更正拼写错误**

    拼写检查引擎会在有绝对把握的情况下自动更正拼错的词。 例如，该引擎会自动将“teh”更改为“the”。

-   **显示备选拼写**

    当拼写检查引擎对于更正不确信时，它会在拼错的词下面显示一条红线，并在你点击或右键单击该词时在上下文菜单中显示备选词。

在 JavaScript 控件中，对于多行文本输入控件，拼写检查在默认情况下处于打开状态；对于单行控件，拼写检查在默认情况下处于关闭状态。 对于单行控件，可以将控件的 **spellcheck** 属性设置为 **true** 来手动打开拼写检查。 你可以通过将控件的 **spellcheck** 属性设置为 **false** 来禁用拼写检查。

对于 XAML TextBox 控件，拼写检查在默认情况下处于关闭状态。 你可以通过将 **IsSpellCheckEnabled** 属性设置为 **true** 来启用它。



## <span id="related_topics"></span>相关文章

* [文本和文本控件](text-controls.md)
* [文本输入指南](https://msdn.microsoft.com/library/windows/apps/hh750315)
* [文本和版式指南](https://msdn.microsoft.com/library/windows/apps/hh700394) 
           **对于开发人员 (XAML)**
* [**TextBox.IsSpellCheckEnabled 属性**](https://msdn.microsoft.com/library/windows/apps/br209688)
* [**TextBox 类**](https://msdn.microsoft.com/library/windows/apps/br209683)

 






<!--HONumber=May16_HO2-->


