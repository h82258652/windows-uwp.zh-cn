---
author: Jwmsft
Description: Use a RichTextBlock with RichTextBlockOverflow elements to create advanced text layouts.
title: RichTextBlock
ms.assetid: E4BE4B1B-418E-4075-88F1-22C09DDF8E45
label: Rich text block
template: detail.hbs
ms.author: jimwalk
ms.date: 05/19/2017
ms.topic: article
keywords: Windows 10, uwp
pm-contact: miguelrb
design-contact: ksulliv
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: 16ebc375a72984af8bbc40823121d2d94689fcf2
ms.sourcegitcommit: ca96031debe1e76d4501621a7680079244ef1c60
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/31/2018
ms.locfileid: "5840252"
---
# <a name="rich-text-block"></a>富文本块

 

RTF 块提供了多种适用于高级文本布局的功能，你可以在需要支持段落、内联 UI 元素或复杂文本布局时使用。

> **重要 API**：[RichTextBlock 类](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.richtextblock.aspx)、[RichTextBlockOverflow 类](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.richtextblockoverflow.aspx)、[Paragraph 类](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.documents.paragraph.aspx)、[Typography 类](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.documents.typography.aspx)

## <a name="is-this-the-right-control"></a>这是正确的控件吗？

如果你需要支持多段落、多列或其他复杂文本布局或者内联 UI 元素（例如图像），请使用 **RichTextBlock**。

使用 **TextBlock** 显示应用中大部分只读文本。 你可以使用它来显示单行或多行文本、内联超链接以及粗体、斜体或带下划线格式的文本。 TextBlock 提供较简单的内容模型，因此通常也更易于使用，并且它比 RichTextBlock 提供文本呈现性能更好。 它优先用于大部分应用 UI 文本。 虽然你可以在文本中放入换行符，但 TextBlock 旨在显示一个段落且不支持文本缩进。

有关选择正确文本控件的详细信息，请参阅[文本控件](text-controls.md)文章。

## <a name="examples"></a>示例

<table>
<th align="left">XAML 控件库<th>
<tr>
<td><img src="images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>如果已安装 <strong style="font-weight: semi-bold">XAML 控件库</strong>应用，请单击此处<a href="xamlcontrolsgallery:/item/RichTextBlock">打开此应用，了解 RichTextBlock 的实际应用</a>。</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">获取 XAML 控件库应用 (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics">获取源代码 (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="create-a-rich-text-block"></a>创建富文本块

RichTextBlock 的内容属性是 [Blocks](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.richtextblock.blocks.aspx) 属性，它通过 [Paragraph](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.documents.paragraph.aspx) 元素支持基于段落的文本。 它没有可以用来轻松访问应用中控件的文本内容的 **Text** 属性。 但是，RichTextBlock 提供了多个 TextBlock 没有提供的独特功能。 

RichTextBlock 支持：
- 多个段落。 通过设置 [TextIndent](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.richtextblock.textindent.aspx) 属性设置段落缩进。
- 内联 UI 元素。 使用 [InlineUIContainer](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.documents.inlineuicontainer.aspx) 显示文本的内联 UI 元素，如图像。
- 溢出容器。 使用 [RichTextBlockOverflow](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.richtextblockoverflow.aspx) 元素创建多列文本布局。

### <a name="paragraphs"></a>Paragraphs

使用 [Paragraph](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.documents.paragraph.aspx) 元素定义要在 RichTextBlock 控件中显示的文本块。 每个 RichTextBlock 应至少包括一个 Paragraph。 

通过设置 [RichTextBlock.TextIndent](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.richtextblock.textindent.aspx) 属性，你可以在 RichTextBlock 中设置所有段落的缩进量。 通过将 [Paragraph.TextIndent](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.documents.paragraph.textindent.aspx) 属性设置为不同值，你可以在 RichTextBlock 中为特定段落重写此设置。

```xaml
<RichTextBlock TextIndent="12">
  <Paragraph TextIndent="24">First paragraph.</Paragraph>
  <Paragraph>Second paragraph.</Paragraph>
  <Paragraph>Third paragraph. <Bold>With an inline.</Bold></Paragraph>
</RichTextBlock>
```

### <a name="inline-ui-elements"></a>内联 UI 元素

[InlineUIContainer](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.documents.inlineuicontainer.aspx) 类允许你将任何 UIElement 内联嵌入文本中。 常用方案是将 Image 内联置于文本中，但你也可以使用交互式元素，例如 Button 或 CheckBox。

如果你想要在相同位置嵌入多个元素内联，请考虑将面板用作单个 InlineUIContainer 子元素，然后将多个元素放入该面板中。

本示例显示如何使用 InlineUIContainer 将图像插入 RichTextBlock 中。 

```xaml
<RichTextBlock>
    <Paragraph>
        <Italic>This is an inline image.</Italic>
        <InlineUIContainer>
            <Image Source="Assets/Square44x44Logo.png" Height="30" Width="30"/>
        </InlineUIContainer>
        Mauris auctor tincidunt auctor.
    </Paragraph>
</RichTextBlock>
```

## <a name="overflow-containers"></a>溢出容器

你可以将 RichTextBlock 与 [RichTextBlockOverflow](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.richtextblockoverflow.aspx) 元素结合使用，以创建多列或其他高级页面布局。 RichTextBlockOverflow 元素的内容始终来自 RichTextBlock 元素。 链接 RichTextBlockOverflow 元素的方法是将其设置为 RichTextBlock 的 OverflowContentTarget 或另一个 RichTextBlockOverflow。

以下是创建两列布局的简单示例。 查看示例部分获取更复杂的示例。

```xaml
<Grid>
    <Grid.ColumnDefinitions>
        <ColumnDefinition/>
        <ColumnDefinition/>
    </Grid.ColumnDefinitions>
    <RichTextBlock Grid.Column="0" 
                   OverflowContentTarget="{Binding ElementName=overflowContainer}" >
        <Paragraph>
            Proin ac metus at quam luctus ultricies.
        </Paragraph>
    </RichTextBlock>
    <RichTextBlockOverflow x:Name="overflowContainer" Grid.Column="1"/>
</Grid>
```

## <a name="formatting-text"></a>设置文本格式

尽管 RichTextBlock 存储纯文本，但你可以应用各种格式选项自定义文本在应用中的呈现方式。 你可以设置标准控件属性（例如 FontFamily、FontSize、FontStyle、Foreground 和 CharacterSpacing）以更改文本外观。 你还可以使用内联文本元素和 Typography 附加属性设置文本格式。 这些选项仅影响 RichTextBlock 在本地显示文本的方式，因此如果你将文本复制并粘贴到富文本控件，将不会应用任何格式。

### <a name="inline-elements"></a>内联元素

[Windows.UI.Xaml.Documents](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.documents.aspx) 命名空间提供可用于设置文本格式的各种内联文本元素，如 Bold、Italic、Run、Span 和 LineBreak。 将格式应用到文本的各个文本部分的典型方法是将文本放入 Run 或 Span 元素中，然后在该元素上设置属性。

以下是 Paragraph，第一个词组显示为粗体、蓝色、16pt 文本。

```xaml
<Paragraph>
    <Bold><Span Foreground="DarkSlateBlue" FontSize="16">Lorem ipsum dolor sit amet</Span></Bold>
    , consectetur adipiscing elit.
</Paragraph>
```

### <a name="typography"></a>Typography

[Typography](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.documents.typography.aspx) 类的附加属性提供针对 Microsoft OpenType 版式属性集的访问权限。 你可以在 RichTextBlock 或个别内联文本元素上设置这些附加属性，如下所示。

```xaml
<RichTextBlock Typography.StylisticSet4="True">
    <Paragraph>
        <Span Typography.Capitals="SmallCaps">Lorem ipsum dolor sit amet</Span>
        , consectetur adipiscing elit.
    </Paragraph>
</RichTextBlock>
```

## <a name="recommendations"></a>建议

请参阅版式和字体指南。

## <a name="get-the-sample-code"></a>获取示例代码

- [XAML 控件库示例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics) - 以交互式格式查看所有 XAML 控件。

## <a name="related-articles"></a>相关文章

[文本控件](text-controls.md)

**对于设计人员**
- [拼写检查指南](text-controls.md)
- [添加搜索](https://msdn.microsoft.com/library/windows/apps/hh465231)
- [文本输入指南](text-controls.md)

**面向开发人员 (XAML)**
- [TextBox 类](https://msdn.microsoft.com/library/windows/apps/br209683)
- [Windows.UI.Xaml.Controls PasswordBox 类](https://msdn.microsoft.com/library/windows/apps/br227519)


**对于开发人员（其他）**
- [字符串长度属性](https://msdn.microsoft.com/library/system.string.length(v=vs.110).aspx)
