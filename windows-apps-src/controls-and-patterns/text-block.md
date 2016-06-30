---
author: Jwmsft
ms.assetid: DA562509-D893-425A-AAE6-B2AE9E9F8A19
label: Text block
template: detail.hbs
translationtype: Human Translation
ms.sourcegitcommit: a4e9a90edd2aae9d2fd5d7bead948422d43dad59
ms.openlocfilehash: 7666c7a82b2959aa1329bbfe2532dccdc7fb38a5

---
# 文本块
 文本块是应用中用于显示只读文本的主要控件。 你可以使用它来显示单行或多行文本、内联超链接以及粗体、斜体或带下划线格式的文本。

<span class="sidebar_heading" style="font-weight: bold;">重要的 API</span>

-   [**TextBlock 类**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.textblock.aspx)
-   [**Text 属性**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.textblock.text.aspx)
-   [**Inlines 属性**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.textblock.inlines.aspx)

## 这是正确的控件吗？

文本块相比格式文本块通常更易于使用，并且提供更好的文本呈现性能，因此它优先用于大部分应用 UI 文本。 你可以通过获取 [Text](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.textblock.text.aspx) 属性轻松地访问和使用应用的文本块中的文本。 它还提供许多用于自定义文本呈现方式的相同格式设置选项。 

虽然你可以在文本中放入换行符，但文本块设计为显示一个段落且不支持文本缩进。 如果你需要支持多段落、多列文本或其他复杂文本布局或者内联 UI 元素（例如图像），请使用 **RichTextBlock**。

有关选择正确文本控件的详细信息，请参阅[文本控件](text-controls.md)文章。

## 示例


## 创建文本块

下面是定义简单 TextBlock 控件并将其 Text 属性设置为字符串的方法。

```xaml
<TextBlock Text="Hello, world!" />
```

```csharp
TextBlock textBlock1 = new TextBlock();
textBlock1.Text = "Hello, world!";
```

    <TextBlock Text="Hello, world!" />

    TextBlock textBlock1 = new TextBlock();
    textBlock1.Text = "Hello, world!";

### 内容模型

你可以使用两种属性将内容添加到 TextBlock：[Text](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.textblock.text.aspx) 和 [Inlines](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.textblock.inlines.aspx)。

显示文本的最常用方法是将 Text 属性设置为字符串值，如在之前示例中所示。

你还可以通过将内联流内容元素放入 TextBox.Inlines 属性来添加内容，如下所示。
```xaml
<TextBlock><Run>Text can be <Bold>bold</Bold>, <Italic>italic</Italic>, or <Bold><Italic>both</Italic></Bold>.</Run></TextBlock>
```

派生自 Inline 类的元素（例如 Bold、Italic、Run、Span 和 LineBreak）支持用于不同文本部分的不同格式。 有关详细信息，请参阅[设置文本格式]()部分。 你可以使用内联 Hyperlink 元素向文本添加超链接。 但是，使用 Inlines 还会禁用快速路径文本呈现，该主题将在下一节中讨论。


## 性能注意事项

XAML 应尽可能地使用更有效的布局文本的代码路径。 此快速路径既能降低总体内存使用率，又能大幅减少执行文本测量和排列的 CPU 时间。 此快速路径仅适用于 TextBlock，因此应仅在基于 RichTextBlock 时首选它。

某些条件要求 TextBlock 回退到具有更多功能的 CPU 密集型代码路径，以便进行文本呈现。 若要使文本呈现在快速路径上，请确保在设置此处列出的属性时遵循以下指南。
- [
            **Text**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.textblock.text.aspx)：最重要的条件是，快速路径仅在你通过在 XAML 或代码中显式设置 Text 属性来设置文本（如前面的示例所示）时使用。 由于多种格式的潜在复杂性，通过 TextBlock 的 Inlines 集合（例如 `<TextBlock>Inline text</TextBlock>`）设置文本将禁用快速路径。
- [
            **CharacterSpacing**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.textblock.characterspacing.aspx)：仅默认值 0 是快速路径。
- [
            **Typography**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.documents.typography.aspx)：仅各个 Typography 属性的默认值是快速路径。
- [
            **TextTrimming**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.textblock.texttrimming.aspx)：仅 **None**、**CharacterEllipsis** 和 **WordEllipsis** 值是快速路径。 **Clip** 值将禁用快速路径。
- [
            **LineStackingStrategy**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.textblock.linestackingstrategy.aspx)：如果 [LineHeight](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.textblock.lineheight.aspx) 不为 0，**BaselineToBaseline** 或 **MaxHeight** 值将禁用快速路径。
- [
            **IsTextSelectionEnabled**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.textblock.istextselectionenabled.aspx)：仅 **false** 是快速路径。 通过将此属性设置为 **true**，禁用快速路径。

你可以在调试期间将 [DebugSettings.IsTextPerformanceVisualizationEnabled](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.debugsettings.istextperformancevisualizationenabled.aspx) 属性设置为 **true** 来确定文本是否正在使用快速路径呈现。 当此属性设置为 true 时，快速路径上的文本将显示为亮绿色。 

>**提示** &nbsp;&nbsp;此功能将在 Build 2015 的这一会话- [XAML 性能：关于最大程度地提升使用 XAML 生成的通用 Windows 应用体验的技术](https://channel9.msdn.com/Events/Build/2015/3-698)中作深入介绍。

 

通常，通过在 App.xaml 的代码隐藏页中使用 [OnLaunched](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.application.onlaunched.aspx) 方法重写来设置调试设置，如下所示。
```csharp
protected override void OnLaunched(LaunchActivatedEventArgs e)
{
#if DEBUG
    if (System.Diagnostics.Debugger.IsAttached)
    {
        this.DebugSettings.IsTextPerformanceVisualizationEnabled = true;
    }
#endif

// ...

}
```

在此示例中，第一个 TextBlock 使用快速路径进行呈现，而第二个不是。
```xaml
<StackPanel>
    <TextBlock Text="This text is on the fast path."/>
    <TextBlock>This text is NOT on the fast path.</TextBlock>
<StackPanel/>
```

当通过将 IsTextPerformanceVisualizationEnabled 设置为 true 在调试模式下运行此 XAML 时，结果如下所示。

![在调试模式下呈现的文本](images/text-block-rendering-performance.png)

>**注意** &nbsp;&nbsp;快速路径之外的文本颜色不会更改。 如果应用中有颜色指定为亮绿色的文本，则它在较慢的呈现路径上时仍然显示为亮绿色。 请注意，勿因为调试设置而将应用中设置为绿色的文本与在快速路径上设置为绿色的文本混淆。

## 设置文本格式

虽然 Text 属性存储纯文本，但你可以将各种格式设置选项应用到 TextBlock 控件以自定义文本在应用中的呈现方式。 你可以设置标准控件属性（如 FontFamily、FontSize、FontStyle、Foreground 和 CharacterSpacing）来更改文本外观。 你还可以使用内联文本元素和 Typography 附加属性来设置文本格式。 这些选项仅影响 TextBlock 在本地显示文本的方式，所以举例来说，如果你将文本复制并粘贴到 RTF 控件，则不会应用任何格式。

>**注意** &nbsp;&nbsp;请记住，如在上一节中所述，内联文本元素和非默认版式值不会呈现在快速路径上。
 

### 内联元素

[Windows.UI.Xaml.Documents](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.documents.aspx) 命名空间提供可用于设置文本格式的各种内联文本元素，如 Bold、Italic、Run、Span 和 LineBreak。 

你可以在 TextBlock 中显示一系列字符串，其中每个字符串都具有不同的格式。 为此，你可以使用 Run 元素来显示具有各自格式的每个字符串，并使用 LineBreak 元素来分隔每个 Run 元素。

下面介绍如何通过使用 LineBreak 分隔的 Run 对象在 TextBlock 中定义多个具有不同格式的文本字符串。
```xaml
<TextBlock FontFamily="Arial" Width="400" Text="Sample text formatting runs">
    <LineBreak/>
    <Run Foreground="Gray" FontFamily="Courier New" FontSize="24"> 
        Courier New 24 
    </Run>
    <LineBreak/>
    <Run Foreground="Teal" FontFamily="Times New Roman" FontSize="18" FontStyle="Italic"> 
        Times New Roman Italic 18 
    </Run>
    <LineBreak/>
    <Run Foreground="SteelBlue" FontFamily="Verdana" FontSize="14" FontWeight="Bold"> 
        Verdana Bold 14 
    </Run>
</TextBlock>
```

结果如下。

![使用 run 元素设置格式的文本](images/text-block-run-examples.png)

### 版式

[Typography](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.documents.typography.aspx) 类的附加属性提供针对 Microsoft OpenType 版式属性集的访问权限。 你可以在 TextBlock 或个别内联文本元素上设置这些附加属性。 这些示例展示了这两种操作。
```xaml
<TextBlock Text="Hello, world!"
           Typography.Capitals="SmallCaps"
           Typography.StylisticSet4="True"/>
```

```csharp
TextBlock textBlock1 = new TextBlock();
textBlock1.Text = "Hello, world!";
Windows.UI.Xaml.Documents.Typography.SetCapitals(textBlock1, FontCapitals.SmallCaps);
Windows.UI.Xaml.Documents.Typography.SetStylisticSet4(textBlock1, true);
```

```xaml
<TextBlock>12 x <Run Typography.Fraction="Slashed">1/3</Run> = 4.</TextBlock>
```


## 相关文章

[文本控件](text-controls.md)

**对于设计人员**
- [拼写检查指南](spell-checking-and-prediction.md)
- [添加搜索](https://msdn.microsoft.com/library/windows/apps/hh465231)
- [文本输入指南](text-controls.md)

**面向开发人员 (XAML)**
- [**TextBox 类**](https://msdn.microsoft.com/library/windows/apps/br209683)
- [**Windows.UI.Xaml.Controls PasswordBox 类**](https://msdn.microsoft.com/library/windows/apps/br227519)


**对于开发人员（其他）**
- [字符串长度属性](https://msdn.microsoft.com/library/system.string.length(v=vs.110).aspx)



<!--HONumber=Jun16_HO4-->


