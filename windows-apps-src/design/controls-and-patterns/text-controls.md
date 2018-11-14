---
author: Jwmsft
Description: Consider how often we read text in our daily lives - in email, a book, a road sign, the prices on a menu, tire pressure markings, or posters on a street pole.
title: 文本控件
ms.assetid: 43DC68BF-FA86-43D2-8807-70A359453048
label: Text controls
template: detail.hbs
ms.author: jimwalk
ms.date: 10/01/2018
ms.topic: article
keywords: Windows 10, uwp
pm-contact: miguelrb
design-contact: ksulliv
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: fa990d7e4733c90b7ea46cec881ea502dd6a11b2
ms.sourcegitcommit: 4d88adfaf544a3dab05f4660e2f59bbe60311c00
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/13/2018
ms.locfileid: "6465726"
---
# <a name="text-controls"></a>文本控件

文本控件由文本输入框、密码框、自动建议框和文本块组成。 XAML 框架提供用于呈现、输入和编辑文本的多个控件，以及一组用于设置文本格式的属性。

- 用于显示只读文本的控件是 [TextBlock](text-block.md) 和 [RichTextBlock](rich-text-block.md)。
- 用于文本输入和编辑的控件是： [TextBox](text-box.md)、 [RichEditBox](rich-edit-box.md)、 [AutoSuggestBox](auto-suggest-box.md)，以及[PasswordBox](password-box.md)。

> **重要的 Api**: [TextBlock 类](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.textblock.aspx)、 [RichTextBlock 类](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.richtextblock.aspx)、 [TextBox 类](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.textbox.aspx)、 [RichEditBox 类](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.richeditbox.aspx)、 [AutoSuggestBox 类](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.autosuggestbox.aspx)、 [PasswordBox 类](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.passwordbox.aspx)

## <a name="is-this-the-right-control"></a>这是正确的控件吗？

你使用的文本控件取决于你的方案。 使用此信息选取要在应用中使用的正确文本控件。

### <a name="render-read-only-text"></a>呈现只读文本

使用 **TextBlock** 显示应用中大部分只读文本。 你可以使用它来显示单行或多行文本、内联超链接以及粗体、斜体或带下划线格式的文本。

TextBlock 相比 RichTextBlock 通常更易于使用，并且提供更好的文本呈现性能，因此它优先用于大部分应用 UI 文本。 你可以通过获取 [Text](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.textblock.text.aspx) 属性的值轻松地访问和使用应用的 TextBlock 中的文本。

它还提供许多用于自定义文本呈现方式的相同格式设置选项。 虽然你可以在文本中放入换行符，但 TextBlock 设计为显示单个段落且不支持文本缩进。

如果你需要支持多段落、多列文本或其他复杂文本布局或者内联 UI 元素（例如图像），请使用 **RichTextBlock**。 RichTextBlock 提供适用于高级文本布局的若干功能。

RichTextBlock 的内容属性是 [Blocks](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.richtextblock.blocks.aspx) 属性，它通过 [Paragraph](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.documents.paragraph.aspx) 元素支持基于段落的文本。 它没有可以用来轻松访问应用中控件的文本内容的 **Text** 属性。  

### <a name="text-input"></a>文本输入

使用 **TextBox** 控件允许用户输入和编辑无格式文本（例如在表单中）。 你可以使用 [Text](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.textbox.text.aspx) 属性在 TextBox 中获取和设置文本。

你可以使 TextBox 只读，但只应是临时的、有条件的状态。 如果文本永远不可编辑，请考虑改用 TextBlock。

使用 **PasswordBox** 控件收集密码或其他隐私数据，如身份证号。 密码框是指出于隐私目的隐藏所键入的字符的文本输入框。 密码框看起来像文本输入框，区别在于它显示项目符号来代替已输入的文本。 可自定义项目符号字符。

使用 **AutoSuggestBox** 控件向用户显示建议列表以供他们在键入时从其中选择。 自动建议框是可触发基本搜索建议列表的文本输入框。 建议的搜索字词可取自热门搜索字词和用户以前所输入字词的组合。

你还应使用 AutoSuggestBox 控件实现搜索框。

使用 **RichEditBox** 显示和编辑文本文件。 不要像使用其他标准文本输入框那样使用 RichEditBox 在应用中获取用户输入。 而应使用它来处理独立于应用的文本文件。 通常需要将输入到 RichEditBox 中的文本保存到 .rtf 文件。

**文本输入是否是最佳选项？**

有多种在应用中获取用户输入的方法。 这些问题将有助于回答最适合用于获取用户输入的是标准文本输入框之一还是其他控件。

-   **高效枚举所有有效值是否可行？** 如果是，请考虑使用选择控件之一，例如[复选框](checkbox.md)、[下拉列表](lists.md)、列表框、[单选按钮](radio-button.md)、[滑块](slider.md)、[切换开关](toggles.md)、[日期选取器](date-and-time.md)或时间选取器。
-   **有效值集是否非常小？** 如果是，请考虑使用[下拉列表](lists.md)或列表框，在值的长度超过几个字符的情况下尤其如此。
-   **有效数据是否完全不受限制？ 或者有效数据是否仅受格式限制（长度或字符类型受限制）？** 如果是，请使用文本输入控件。 你可以限制可输入的字符数，并且可以在应用代码中验证格式。
-   **该值是否表示具有常见专用控件的数据类型？** 如果是，请使用相应的控件，而不要使用文本输入控件。 例如，使用 [DatePicker](https://msdn.microsoft.com/library/windows/apps/br211681)（而非文本输入控件）接受日期输入。
-   如果数据完全为数值：
    -   **输入的值是否为近似值，并且/或者与同一页面的另一个数量相关？** 如果是，请使用[滑块](slider.md)。
    -   **用户是否会从设置更改效果的即时反馈中获益？** 如果是，请使用[滑块](slider.md)（可能带有随附控件）。
    -   **输入的值是否很可能在观察结果后得到调整（例如，调节音量或屏幕亮度）？** 如果是，请使用[滑块](slider.md)。

## <a name="examples"></a>示例

<table>
<th align="left">XAML 控件库<th>
<tr>
<td><img src="images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>如果已安装 <strong style="font-weight: semi-bold">XAML 控件库</strong>应用，请单击此处<a href="xamlcontrolsgallery:/category/Text">打开此应用，了解文本控件的实际应用</a>。</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">获取 XAML 控件库应用 (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics">获取源代码 (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

文本框

![文本框](images/text-box.png)

自动建议框

![展开的自动建议控件示例](images/controls_autosuggest_expanded01.png)

密码框

![正在键入文本的密码框焦点状态](images/passwordbox-focus-typing.png)

## <a name="create-a-text-control"></a>创建文本控件

有关特定于每个文本控件的信息和示例，请参阅以下文章。

-   [AutoSuggestBox](auto-suggest-box.md)
-   [PasswordBox](password-box.md)
-   [RichEditBox](rich-edit-box.md)
-   [RichTextBlock](rich-text-block.md)
-   [TextBlock](text-block.md)
-   [TextBox](text-box.md)

## <a name="font-and-style-guidelines"></a>字体和样式指南
有关字体指南，请参阅以下文章：

- [版式指南](../style/typography.md)
- [Segoe MDL2 图标列表和指南](../style/segoe-ui-symbol-font.md)

## <a name="pen-input"></a>笔输入

**适用于：** TextBox、RichEditBox、AutoSuggestBox

从 Windows 10 版本 1803 开始，XAML 文本输入框默认支持使用 [Windows Ink](../input/pen-and-stylus-interactions.md) 进行笔输入。 当用户使用 Windows 手写笔点击进入文本输入框时，文本框会进行转换，以允许用户直接用手写笔进行写入，而不是打开一个单独的输入面板。

![用手写笔点击时，文本框会扩展](images/handwritingview/handwritingview2.gif)

有关详细信息，请参阅[与手写视图的文本输入](text-handwriting-view.md)。

## <a name="choose-the-right-keyboard-for-your-text-control"></a>为文本控件选择正确的键盘

**适用于：** TextBox、PasswordBox、RichEditBox

若要帮助用户使用触摸键盘或软输入面板 (SIP) 输入数据，你可以将文本控件的输入范围设置为与期望用户输入的数据类型匹配。

>提示 此信息仅适用于 SIP。 它不适用于硬件键盘或 Windows“轻松使用”选项中提供的屏幕键盘。

当你的应用在具有触摸屏的设备上运行时，触摸键盘可用于文本输入。 当用户点击可编辑的输入字段（如 TextBox 或 RichEditBox）时，系统会调用触摸键盘。 通过将文本控件的输入范围设置为与你期望用户输入的数据类型匹配，可以让用户在应用中更快捷地输入数据。 输入范围会针对控件所预期的文本输入类型向系统提供提示，以便系统可以为该输入类型提供专用的触摸键盘布局。

例如，如果文本框中仅用于输入一个 4 位数的 PIN，请将 [InputScope](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.textbox.inputscope.aspx) 属性设置为 **Number**。 这将通知系统显示数字键盘布局，以便于用户输入 PIN。

>重要提示  
>输入范围不会导致任何输入验证的执行，并且不会阻止用户通过硬件键盘或其他输入设备提供任何输入。 你仍然负责按需在代码中验证输入。

有关详细信息，请参阅[使用输入范围更改触摸键盘](https://msdn.microsoft.com/library/windows/apps/mt280229)。

## <a name="color-fonts"></a>颜色字体

**适用于：** TextBlock、RichTextBlock、TextBox、RichEditBox

Windows 具有使字体为每个字形包含多个颜色层的功能。 例如，Segoe UI Emoji 字体定义表情和其他表情符号字符的颜色版本。

标准和格式文本控件支持显示颜色字体。 默认情况下，**IsColorFontEnabled** 属性为 **true**，并且带有这些附加层的字体使用颜色呈现。 系统上的默认颜色字体是 Segoe UI Emoji，并且控件将回退到此字体以使用颜色显示字形。

```xaml
<TextBlock FontSize="30">Hello ☺⛄☂♨⛅</TextBlock>
```

呈现的文本如下所示：

![带有颜色字体的文本块](images/text-block-color-fonts.png)

有关详细信息，请参阅 [IsColorFontEnabled](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.textblock.iscolorfontenabled.aspx) 属性。

## <a name="guidelines-for-line-and-paragraph-separators"></a>行和段落分隔符指南

**适用于：** TextBlock、RichTextBlock、multi-line TextBox、RichEditBox

使用行分隔符 (0x2028) 和段分隔符 (0x2029) 划分纯文本。 在每个行分隔符后开始新行。 在每个段分隔符后开始新段落。

没有必要使用这些字符开始第一行或第一段，也不必使用它们结束最后一行或最后一段；执行此操作会指示在该位置存在空行或段落。

你的应用可以使用行分隔符指示无条件的行末尾。 但是，行分隔符不对应于单独的回车键和换行字符或这些字符的组合。 行分隔符必须与回车键和换行字符分开处理。

你的应用可以在文本段落之间插入段分隔符。 使用此分隔符，可以创建可在不同操作系统上使用不同行宽度设置格式的纯文本文件。 目标系统可忽略任何行分隔符，并且仅在段分隔符处分段。

## <a name="guidelines-for-spell-checking"></a>拼写检查指南

**适用于：** TextBox、RichEditBox

在文本输入和编辑过程中，拼写检查通过使用红色波形曲线突出显示拼写错误的字词来通知用户，并提供一种供用户纠正拼写错误的方法。

下面是内置拼写检查器的示例：

![内置拼写检查器](images/spellchecking.png)

对文本输入控件使用拼写检查可用于以下两个目的：

-   **自动更正拼写错误**

    拼写检查引擎会在有绝对把握的情况下自动更正拼错的词。 例如，该引擎会自动将“teh”更改为“the”。

-   **显示备选拼写**

    当拼写检查引擎对于更正不确信时，它会在拼错的字词下面显示一条红线，并在你点击或右键单击该字词时在上下文菜单中显示备选词。

-   使用拼写检查可以在用户向文本输入控件中输入字词或句子时提供帮助。 拼写检查适用于触摸、鼠标以及键盘输入。
-   不要在以下情况中使用拼写检查：字词可能不在词典中，或者用户不重视拼写检查。 例如，如果文本框旨在捕获电话号码或姓名，请不要打开它。
-   不要仅因为当前拼写检查引擎不支持你的应用语言就禁用拼写检查。 当拼写检查器不支持某种语言时，它不会进行任何操作，因此使该选项处于打开状态不会造成任何损害。 而且，某些用户可以使用输入法编辑器 (IME) 在你的应用中输入另一种语言，并且该语言可能受支持。 例如，在生成日语应用时，即使拼写检查引擎当前可能无法识别该语言，也不要关闭拼写检查。 用户可能会切换到英语 IME 并在应用中键入英语；如果拼写检查处于启用状态，则会进行英语拼写检查。

对于 TextBox 和 RichEditBox 控件，拼写检查在默认情况下处于打开状态。 你可以通过将 **IsSpellCheckEnabled** 属性设置为 **false** 来关闭它。

## <a name="related-articles"></a>相关文章

**对于设计人员**
- [版式指南](../style/typography.md)
- [Segoe MDL2 图标列表和指南](../style/segoe-ui-symbol-font.md)
- [添加搜索](https://msdn.microsoft.com/library/windows/apps/hh465231)

**面向开发人员 (XAML)**
- [TextBox 类](https://msdn.microsoft.com/library/windows/apps/br209683)
- [Windows.UI.Xaml.Controls PasswordBox 类](https://msdn.microsoft.com/library/windows/apps/br227519)
- [String.Length 属性](https://msdn.microsoft.com/library/system.string.length.aspx)
