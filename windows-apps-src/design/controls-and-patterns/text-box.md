---
author: Jwmsft
ms.assetid: CC1BF51D-3DAC-4198-ADCB-1770B901C2FC
Description: The TextBox control lets a user enter text into an app.
title: 文本框
label: Text box
template: detail.hbs
ms.author: jimwalk
ms.date: 05/19/2017
ms.topic: article
keywords: Windows 10, uwp
pm-contact: miguelrb
design-contact: ksulliv
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: 43af446513ebb857858c21f0b2859af6febd82d0
ms.sourcegitcommit: ed0304b8a214c03b8aab74b8ef12c9f82b8e3c5f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/19/2018
ms.locfileid: "7300285"
---
# <a name="text-box"></a>文本框

 

TextBox 控件可使用户在应用中键入文本。 它通常用于捕获单行文本，但可配置为捕获多行文本。 文本以简单、统一、纯文本的格式显示在屏幕上。

TextBox 具有大量可简化文本输入的功能。 它附带熟悉的内置上下文菜单，并提供对复制和粘贴文本的支持。 “清除所有”按钮使用户可以快速删除所输入的所有文本。 它还内置了拼写检查功能，并且在默认情况下处于启用状态。

> **重要 API**：[TextBox 类](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.textbox.aspx)、[Text 属性](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.textbox.text.aspx)


## <a name="is-this-the-right-control"></a>这是正确的控件吗？

使用 **TextBox** 控件允许用户输入和编辑无格式文本（例如在表单中）。 你可以使用 [Text](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.textbox.text.aspx) 属性在 TextBox 中获取和设置文本。

你可以使 TextBox 只读，但只应是临时的、有条件的状态。 如果文本永远不可编辑，请考虑改用 [TextBlock](text-block.md)。

使用 [PasswordBox](password-box.md) 控件收集密码或其他隐私数据，如身份证号。 密码框看起来像文本输入框，区别在于它呈现项目符号来代替已输入的文本。

使用 [AutoSuggestBox](auto-suggest-box.md) 控件允许用户输入搜索词或向用户显示建议列表以供他们在键入时从其中选择。

使用 [RichEditBox](rich-edit-box.md) 显示和编辑 RTF 文件。

有关选择正确文本控件的详细信息，请参阅 [文本控件](text-controls.md) 文章。

## <a name="examples"></a>示例

<table>
<th align="left">XAML 控件库<th>
<tr>
<td><img src="images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>如果已安装 <strong style="font-weight: semi-bold">XAML 控件库</strong>应用，请单击此处<a href="xamlcontrolsgallery:/item/TextBox">打开此应用，了解 TextBox 的实际应用</a>。</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">获取 XAML 控件库应用 (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics">获取源代码 (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

![文本框](images/text-box.png)

## <a name="create-a-text-box"></a>创建文本块

下面是具有标题和占位符文本的简单文本框的 XAML。

```xaml
<TextBox Width="500" Header="Notes" PlaceholderText="Type your notes here"/>
```

```csharp
TextBox textBox = new TextBox();
textBox.Width = 500;
textBox.Header = "Notes";
textBox.PlaceholderText = "Type your notes here";
// Add the TextBox to the visual tree.
rootGrid.Children.Add(textBox);
```

下面是此 XAML 所产生的文本框。

![简单文本框](images/text-box-ex1.png)

### <a name="use-a-text-box-for-data-input-in-a-form"></a>为表单中的数据输入使用文本框

通常使用文本框接受表单上的数据输入，并使用 [Text](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.textbox.text.aspx) 属性获取来自文本框的完整文本字符串。 通常使用提交按钮单击之类的事件来访问 Text 属性，但如果你需要在文本发生更改时执行某些操作，可以处理 [TextChanged](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.textbox.textchanged.aspx) 或 [TextChanging](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.textbox.textchanging.aspx) 事件。

你可以将 [Header](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.textbox.header.aspx)（或标签）和 [PlaceholderText](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.textbox.placeholdertext.aspx)（或水位线）添加到文本框，以向用户指示文本框的用途。 若要自定义标题外观，可设置 [HeaderTemplate](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.textbox.headertemplate.aspx) 属性而非 Header。 *有关设计信息，请参阅标签指南*。

可以通过设置 [MaxLength](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.textbox.maxlength.aspx) 属性来限制用户可键入的字符数。 但是，MaxLength 不会限制粘贴文本的长度。 使用 [Paste](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.textbox.paste.aspx) 事件修改粘贴文本（如果这对你的应用很重要）。

文本框包含“清除所有”按钮（“X”），该按钮会在框中输入文本时显示。 当用户单击“X”时，将清除文本框中的文本。 它的外观如下所示。

![带有“清除所有”按钮的文本框](images/text-box-clear-all.png)

“清除所有”按钮仅为包含文本并具有焦点的可编辑单行文本框显示。

在以下任意情况下，不会显示“清除所有”按钮：
- **IsReadOnly** 为 **true**
- **AcceptsReturn** 为 **true**
- **TextWrap** 具有非 **NoWrap** 的值

### <a name="make-a-text-box-read-only"></a>使文本框变为只读

可以通过将 [IsReadOnly](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.textbox.isreadonly.aspx) 属性设置为 **true** 使文本框变为只读。 通常根据应用中的条件在应用代码中切换此属性。 如果需要始终为只读的文本，请考虑改用 TextBlock。

可以通过将 IsReadOnly 属性设置为 true 使 TextBox 变为只读。 例如，你可以提供一个供用户输入评论的 TextBox，该文本框仅在特定条件下启用。 你可以使 TextBox 在不满足特定条件时变为只读。 如果你只需要显示文本，仅考虑改用 TextBlock 或 RichTextBlock。

只读文本框外观与读/写文本框相同，因此它可能使用户产生困惑。
用户可以选择并复制文本。
IsEnabled


### <a name="enable-multi-line-input"></a>启用多行输入

有两个可用于控制文本框是否在多行上显示文本的属性。 通常同时设置这两个属性来创建多行文本框。
- 若要使文本框允许和显示新行或返回字符，请将 [AcceptsReturn](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.textbox.acceptsreturn.aspx) 属性设置为 **true**。
- 若要启用文本换行，请将 [TextWrapping](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.textbox.textwrapping.aspx) 属性设置为 **Wrap**。 这会导致文本在其到达文本框边缘时独立于行分隔符换行。

> **注意**&nbsp;&nbsp;TextBox 和 RichEditBox 不支持其 TextWrapping 属性的 **WrapWholeWords** 值。 如果你尝试使用 WrapWholeWords 作为 TextBox.TextWrapping 或 RichEditBox.TextWrapping 的值，将引发无效参数异常。

在输入文本时多行文本框将持续在垂直方向上增长，除非它受到其 [Height](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.frameworkelement.height.aspx) 或 [MaxHeight](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.frameworkelement.maxheight.aspx) 属性或父容器的约束。 你应测试多行文本框是否会增长到其可见区域之外，如果确实如此，则约束其增长。 我们建议你始终为多行文本框指定相应的高度，使其在用户键入时高度不会增长。

在需要时自动启用使用滚轮或触摸的滚动。 但是，垂直滚动条默认不可见。 你可以通过在嵌入的 ScrollViewer 上将 [ScrollViewer.VerticalScrollBarVisibility](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.scrollviewer.verticalscrollbarvisibility.aspx) 设置为 **Auto** 来显示垂直滚动条，如下所示。

```xaml
<TextBox AcceptsReturn="True" TextWrapping="Wrap"
         MaxHeight="172" Width="300" Header="Description"
         ScrollViewer.VerticalScrollBarVisibility="Auto"/>
```

```csharp
TextBox textBox = new TextBox();
textBox.AcceptsReturn = true;
textBox.TextWrapping = TextWrapping.Wrap;
textBox.MaxHeight = 172;
textBox.Width = 300;
textBox.Header = "Description";
ScrollViewer.SetVerticalScrollBarVisibility(textBox, ScrollBarVisibility.Auto);
```

下面是添加文本后的文本框外观。

![多行文本框](images/text-box-multi-line.png)

### <a name="format-the-text-display"></a>设置文本显示的格式

使用 [TextAlignment]() 属性使文本在文本框内对齐。 若要使文本框在页面布局内对齐，请使用 [HorizontalAlignment](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.frameworkelement.horizontalalignment.aspx) 和 [VerticalAlignment](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.frameworkelement.verticalalignment.aspx) 属性。

尽管文本框仅支持无格式文本，但你可以自定义文本在文本框中的显示方式来匹配你的品牌。 你可以设置标准 [Control](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.control.aspx) 属性（如 [FontFamily](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.control.fontfamily.aspx)、[FontSize](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.control.fontsize.aspx)、[FontStyle](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.control.fontstyle.aspx)、[Background](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.control.background.aspx)、[Foreground](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.control.foreground.aspx) 和 [CharacterSpacing](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.control.characterspacing.aspx)）以更改文本外观。 这些属性仅影响文本框在本地显示文本的方式，所以举例来说，如果你要将文本复制并粘贴到 RTF 控件，将不会应用任何格式。

此示例显示一个设置了多个属性以自定义文本外观的只读文本框。

```xaml
<TextBox Text="Sample Text" IsReadOnly="True"
         FontFamily="Verdana" FontSize="24"
         FontWeight="Bold" FontStyle="Italic"
         CharacterSpacing="200" Width="300"
         Foreground="Blue" Background="Beige"/>
```

```csharp
TextBox textBox = new TextBox();
textBox.Text = "Sample Text";
textBox.IsReadOnly = true;
textBox.FontFamily = new FontFamily("Verdana");
textBox.FontSize = 24;
textBox.FontWeight = Windows.UI.Text.FontWeights.Bold;
textBox.FontStyle = Windows.UI.Text.FontStyle.Italic;
textBox.CharacterSpacing = 200;
textBox.Width = 300;
textBox.Background = new SolidColorBrush(Windows.UI.Colors.Beige);
textBox.Foreground = new SolidColorBrush(Windows.UI.Colors.Blue);
// Add the TextBox to the visual tree.
rootGrid.Children.Add(textBox);
```

生成的文本框如下所示。

![格式化的文本](images/text-box-formatted.png)

### <a name="modify-the-context-menu"></a>修改上下文菜单

默认情况下，文本框上下文菜单中显示的命令取决于文本框的状态。 例如，以下命令可以在文本框可编辑时显示。

命令 | 在以下情况下显示...
------- | -------------
复制 | 文本处于选中状态。
剪切 | 文本处于选中状态。
粘贴 | 剪贴板包含文本。
全选 | TextBox 包含文本。
撤销 | 文本已更改。

若要修改上下文菜单中显示的命令，请处理 [ContextMenuOpening](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.textbox.contextmenuopening.aspx) 事件。 有关此内容的示例，请参阅 [ContextMenu 示例](http://go.microsoft.com/fwlink/p/?linkid=234891)的方案 2。 有关设计信息，请参阅上下文菜单指南。

### <a name="select-copy-and-paste"></a>选择、复制和粘贴

你可以使用 [SelectedText](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.textbox.selectedtext.aspx) 属性在文本框中获取或设置所选文本。 使用 [SelectionStart](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.textbox.selectionstart.aspx) 和 [SelectionLength](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.textbox.selectionlength.aspx) 属性以及 [Select](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.textbox.select.aspx) 和 [SelectAll](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.textbox.selectall.aspx) 方法来操作文本选择。 当用户选择或取消选择文本时，处理 [SelectionChanged](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.textbox.selectionchanged.aspx) 事件以执行某些操作。 可以通过设置 [SelectionHighlightColor](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.textbox.selectionhighlightcolor.aspx) 属性更改用于突出显示所选文本的颜色。

默认情况下，TextBox 支持复制和粘贴。 你可以在应用中的可编辑文本控件上提供 [Paste](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.textbox.paste.aspx) 事件的自定义处理。 例如，在将多行地址粘贴到单行搜索框中时，你可以从该地址中删除换行符。 或者，你可以检查粘贴文本的长度，如果它超出可保存到数据库的最大长度，则警告用户。 有关详细信息和示例，请参阅 [Paste](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.textbox.paste.aspx) 事件。

下面是使用这些属性和方法的一个示例。 当你在第一个文本框中选择文本时，所选文本在第二个文本框中显示，该文本框为只读。 [SelectionLength](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.textbox.selectionlength.aspx) 和 [SelectionStart](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.textbox.selectionstart.aspx) 属性的值在两个文本块中显示。 此操作使用 [SelectionChanged](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.textbox.selectionchanged.aspx) 事件完成。

```xaml
<StackPanel>
   <TextBox x:Name="textBox1" Height="75" Width="300" Margin="10"
         Text="The text that is selected in this TextBox will show up in the read only TextBox below."
         TextWrapping="Wrap" AcceptsReturn="True"
         SelectionChanged="TextBox1_SelectionChanged" />
   <TextBox x:Name="textBox2" Height="75" Width="300" Margin="5"
         TextWrapping="Wrap" AcceptsReturn="True" IsReadOnly="True"/>
   <TextBlock x:Name="label1" HorizontalAlignment="Center"/>
   <TextBlock x:Name="label2" HorizontalAlignment="Center"/>
</StackPanel>
```

```csharp
private void TextBox1_SelectionChanged(object sender, RoutedEventArgs e)
{
    textBox2.Text = textBox1.SelectedText;
    label1.Text = "Selection length is " + textBox1.SelectionLength.ToString();
    label2.Text = "Selection starts at " + textBox1.SelectionStart.ToString();
}
```

下面是该代码的结果。

![文本框中的所选文本](images/text-box-selection.png)

## <a name="choose-the-right-keyboard-for-your-text-control"></a>为文本控件选择正确的键盘

若要帮助用户使用触摸键盘或软输入面板 (SIP) 输入数据，你可以将文本控件的输入范围设置为与期望用户输入的数据类型匹配。

当你的应用在具有触摸屏的设备上运行时，触摸键盘可用于文本输入。 当用户点击可编辑的输入字段（如 TextBox 或 RichEditBox）时，系统会调用触摸键盘。 通过将文本控件的输入范围设置为与你期望用户输入的数据类型匹配，可以让用户在应用中更快捷地输入数据。 输入范围会针对控件所预期的文本输入类型向系统提供提示，以便系统可以为该输入类型提供专用的触摸键盘布局。

例如，如果文本框中仅用于输入一个 4 位数的 PIN，请将 [InputScope](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.textbox.inputscope.aspx) 属性设置为 **Number**。 这将通知系统显示数字键盘布局，以便于用户输入 PIN。

> **重要提示**&nbsp;&nbsp;输入范围不会导致任何输入验证的执行，并且不会阻止用户通过硬件键盘或其他输入设备提供任何输入。 你仍然负责按需在代码中验证输入。

影响触摸键盘的其他属性是 [IsSpellCheckEnabled](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.textbox.isspellcheckenabled.aspx)、[IsTextPredictionEnabled](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.textbox.istextpredictionenabled.aspx) 和 [PreventKeyboardDisplayOnProgrammaticFocus](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.textbox.preventkeyboarddisplayonprogrammaticfocus.aspx)。 （IsSpellCheckEnabled 还会在使用硬件键盘时影响 TextBox。）

有关详细信息和示例，请参阅[使用输入范围更改触摸键盘](https://msdn.microsoft.com/library/windows/apps/mt280229)和属性文档。

## <a name="recommendations"></a>建议

-   如果文本框的用途不甚清楚，请使用标签或占位符文本。 无论文本输入框是否具有值，标签都可见。 占位符文本显示在文本输入框内，并在输入值后立即消失。
-   针对可输入值的范围，为文本框提供适当的宽度。 字词长度因语言而异，因此如果你希望应用在全世界通用，请将本地化考虑在内。
-   文本输入框通常为单行 (`TextWrap = "NoWrap"`)。 当用户需要输入或编辑长字符串时，将文本输入框设置为多行 (`TextWrap = "Wrap"`)。
-   一般而言，文本输入框用于可编辑文本。 但你可以使文本输入框变为只读，以使其内容可供阅读、选择和复制，但不能编辑。
-   如果你需要减少视图中的混乱感，请考虑让一组文本输入框仅在选中控制复选框时显示。 你还可以将文本输入框的启用状态绑定到诸如复选框等控件上。
-   请考虑你希望文本输入框在包含值并且用户点击时表现怎样的行为。 默认行为适用于编辑值，而不是替代该值；插入点将位于字词之间，并且没有选择任何内容。 如果替代是给定文本输入框最常用的情况，你可以在控件接收焦点时选择字段中的所有文本，并且键入将替代选择的内容。

**单行输入框**

-   使用多个单行文本框来捕获许多文本信息的较小部分。 如果文本框在本质上相关联，请将它们分在一组。

-   使单行文本框的大小比最长的预期输入稍微宽一些。 如果执行此操作后导致该控件变得过宽，请将其分成两个控件。 例如，可以将一个地址输入拆分为“地址行 1”和“地址行 2”。
-   设置可输入字符的最大长度。 如果备份数据源不允许输入长字符串，请限制输入并使用验证弹出窗口来让用户知道何时达到限制。
-   使用单行文本输入控件从用户那里收集小文本块。

    下面的示例显示一个用来捕获安全问题答案的单行文本框。 答案应当简短，因此此处适合使用单行文本框。

    ![基本数据输入](images/guidelines_and_checklist_for_singleline_text_input_type_text.png)

-   使用一组简短的、大小固定的单行输入文本控件可输入具有特定格式的数据。

    ![格式化数据输出](images/textinput_example_productkey.png)

-   将不受限制的单行文本输入控件与一个命令按钮结合使用可输入或编辑字符串。

    ![辅助数据输入](images/textinput_example_assisted.png)


**多行文本输入控件**

-   创建富文本框时，提供样式按钮并实现它们的操作。
-   使用与应用样式一致的字体。
-   使文本控件的高度足够大，以便容纳典型输入。
-   如果要捕获具有最大字符或字词计数的长文本，请使用纯文本框并提供实时运行计数器，以便向用户显示他们在达到上限前还能输入多少字符或字词。 你需要自行创建计数器、将它放在文本框下面，并在用户输入每个字符或字词时动态更新它。

    ![长文本](images/guidelines_and_checklist_for_multiline_text_input_text_limits.png)

-   不要让文本输入控件在用户键入时增加高度。
-   当用户仅需要一行时，不要使用多行文本框。
-   如果纯文本控件足够使用，不要使用富文本控件。

## <a name="get-the-sample-code"></a>获取示例代码

- [XAML 控件库示例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics) - 以交互式格式查看所有 XAML 控件。

## <a name="related-articles"></a>相关文章

- [文本控件](text-controls.md)
- [拼写检查指南](text-controls.md)
- [添加搜索](https://msdn.microsoft.com/library/windows/apps/hh465231)
- [文本输入指南](text-controls.md)
- [TextBox 类](https://msdn.microsoft.com/library/windows/apps/br209683)
- [PasswordBox 类](https://msdn.microsoft.com/library/windows/apps/br227519)
- [String.Length 属性](https://msdn.microsoft.com/library/system.string.length(v=vs.110).aspx)
