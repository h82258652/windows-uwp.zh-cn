---
author: Jwmsft
Description: Hyperlinks navigate the user to another part of the app, to another app, or launch a specific uniform resource identifier (URI) using a separate browser app.
title: 超链接
ms.assetid: 74302FF0-65FC-4820-B59A-718A765EF7F0
label: Hyperlinks
template: detail.hbs
ms.author: jimwalk
ms.date: 05/19/2017
ms.topic: article
keywords: windows 10, uwp
pm-contact: kisai
design-contact: kimsea
dev-contact: stpete
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: e6679f789f2530a34a2bf527556c144e7a8e03c3
ms.sourcegitcommit: e2fca6c79f31e521ba76f7ecf343cf8f278e6a15
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/16/2018
ms.locfileid: "6990402"
---
# <a name="hyperlinks"></a>超链接

 

超链接会将用户导航到应用的另一部分、导航到另一个应用，或使用单独的浏览器应用启动特定的统一资源标识符 (URI)。 可使用两种方法向 XAML 应用添加超链接：**Hyperlink** 文本元素和 **HyperlinkButton** 控件。

> **重要 API**：[Hyperlink 文本元素](https://msdn.microsoft.com/library/windows/apps/dn279356)，[HyperlinkButton 控件](https://msdn.microsoft.com/library/windows/apps/br242739)

![“超链接”按钮](images/controls/hyperlink-button.png)


## <a name="is-this-the-right-control"></a>这是正确的控件吗？

当你需要文本，并且该文本可在选中后做出响应并将用户导航至与所选文本有关的详细信息时，请使用超链接。

根据你的需求选择正确类型的超链接：

-   在文本控件内使用内联 **Hyperlink** 文本元素。 Hyperlink 元素随其他文本元素流动，你可以在任何 InlineCollection 中使用它。 如果你希望自动文本换行但不一定需要较大的命中目标，请使用文本超链接。 超链接文本可能较小且难以命中，对于触摸尤其如此。
-   将 **HyperlinkButton** 用于独立超链接。 HyperlinkButton 是一种专用按钮控件，可在需要使用按钮的任何位置使用。
-   使用带有[图像](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.image.aspx)的 **HyperlinkButton** 作为其内容，以创建可单击的图像。

## <a name="examples"></a>示例

<table>
<th align="left">XAML 控件库<th>
<tr>
<td><img src="images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>如果已安装 <strong style="font-weight: semi-bold">XAML 控件库</strong>应用，请单击此处<a href="xamlcontrolsgallery:/item/HyperlinkButton">打开此应用，了解 HyperlinkButton 的实际应用</a>。</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">获取 XAML 控件库应用 (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics">获取源代码 (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="create-a-hyperlink-text-element"></a>创建 Hyperlink 文本元素

此示例将演示如何在 [TextBlock](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.textblock.aspx) 内使用 Hyperlink 文本元素。

```xml
<StackPanel Width="200">
    <TextBlock Text="Privacy" Style="{StaticResource SubheaderTextBlockStyle}"/>
    <TextBlock TextWrapping="WrapWholeWords">
        <Span xml:space="preserve"><Run>Lorem ipsum dolor sit amet, consectetur adipiscing elit. Read the </Run><Hyperlink NavigateUri="http://www.contoso.com">Contoso Privacy Statement</Hyperlink><Run> in your browser.</Run> Donec pharetra, enim sit amet mattis tincidunt, felis nisi semper lectus, vel porta diam nisi in augue.</Span>
    </TextBlock>
</StackPanel>

```
超链接将内联显示，并且随周围的文本流动：

![作为文本元素的超链接示例](images/controls_hyperlink-element.png) 

> **提示**&nbsp;&nbsp;当你在文本控件中使用超链接而其他文本元素采用 XAML 时，请将内容放置在 [Span](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.documents.span.aspx) 容器中，并将 `xml:space="preserve"` 属性应用到 Span 以在超链接和其他元素之间保留空白区域。

## <a name="create-a-hyperlinkbutton"></a>创建 HyperlinkButton

下面介绍了如何使用带有文本和图像的 HyperlinkButton。

```xml
<StackPanel>
    <TextBlock Text="About" Style="{StaticResource TitleTextBlockStyle}"/>
    <HyperlinkButton NavigateUri="http://www.contoso.com">
        <Image Source="Assets/ContosoLogo.png"/>
    </HyperlinkButton>
    <TextBlock Text="Version: 1.0.0001" Style="{StaticResource CaptionTextBlockStyle}"/>
    <HyperlinkButton Content="Contoso.com" NavigateUri="http://www.contoso.com"/>
    <HyperlinkButton Content="Acknowledgments" NavigateUri="http://www.contoso.com"/>
    <HyperlinkButton Content="Help" NavigateUri="http://www.contoso.com"/>
</StackPanel>

```

文本内容显示为标记文本的超链接按钮。 Contoso 徽标图像也是可单击的超链接：

![作为按钮控件的超链接示例](images/controls_hyperlink-button-image.png)

本示例显示如何通过代码创建 HyperlinkButton。

```csharp
var helpLinkButton = new HyperlinkButton();
helpLinkButton.Content = "Help";
helpLinkButton.NavigateUri = new Uri("http://www.contoso.com");
```

## <a name="handle-navigation"></a>处理导航

对于这两种类型的超链接，你可以采用相同的方式处理导航；可设置 **NavigateUri** 属性，或处理 **Click** 事件。

**导航到 URI**

若要使用超链接导航到 URI，请设置 NavigateUri 属性。 当用户单击或点击超链接时，指定的 URI 将在默认浏览器中打开。 默认浏览器将在独立于应用的进程中运行。

> [!NOTE]
> URI 由 [Windows.Foundation.Uri](/uwp/api/windows.foundation.uri) 类表示。 使用 .NET 编程时，此类将隐藏，你应该使用 [System.Uri](https://docs.microsoft.com/dotnet/api/system.uri) 类。 有关详细信息，请参阅这些类的参考页面。

无需使用 **http:** 或 **https:**  方案。 你可以使用诸如 **ms-appx:**、**ms-appdata:** 或 **ms-resources:** 等方案，前提是这些位置中存在适合在浏览器中加载的资源内容。 但是，明确禁止 **file:** 方案。 有关详细信息，请参阅 [URI 方案](https://msdn.microsoft.com/library/windows/apps/jj655406.aspx)。

当用户单击超链接时，NavigateUri 属性的值传递到 URI 类型和方案的系统处理程序。 然后，系统启动针对为 NavigateUri 提供的 URI 的方案注册的应用。

如果你不希望超链接在默认的 Web 浏览器中加载内容（并且不希望浏览器显示），则不要为 NavigateUri 设置值。 改为处理 Click 事件，并编写执行所需操作的代码。


**处理 Click 事件**

对于除在浏览器中启动 URI 以外的操作（例如应用内导航），请使用 Click 事件。 例如，如果你希望加载新应用页面而不是打开浏览器，请在 Click 事件内调用 [Frame.Navigate](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.frame.navigate.aspx) 方法来导航到新应用页面。 如果你希望外部的绝对 URI 在 [WebView](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.webview.aspx) 控件（同样存在于应用中）内加载，请调用 [WebView.Navigate](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.webview.navigate.aspx) 作为 Click 处理程序逻辑的一部分。

通常不同时处理 Click 事件和指定 NavigateUri 值，因为它们表示使用超链接元素的两种不同方式。 如果你的意图是在默认浏览器中打开 URI，并且你已为 NavigateUri 指定了值，请不要处理 Click 事件。 相反，如果你处理 Click 事件，请不要指定 NavigateUri。

在 Click 事件处理程序内，你没有任何方法来阻止默认浏览器加载为 NavigateUri 指定的任何有效目标；当超链接已激活并且无法从 Click 事件处理程序内取消时，将自动（异步）发生该操作。 

## <a name="hyperlink-underlines"></a>超链接下划线
默认情况下，超链接带下划线。 此下划线很重要，因为它有助于满足辅助功能要求。 色盲用户使用下划线来区分超链接和其他文本。 如果你禁用下划线，应考虑添加某些其他类型的格式差异来将超链接与其他文本区分开来，如 FontWeight 或 FontStyle。

**超链接文本元素**

你可以设置 [UnderlineStyle](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.documents.hyperlink.underlinestyle.aspx) 属性来禁用下划线。 如果执行此操作，请考虑使用 [FontWeight](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.documents.textelement.fontweight.aspx) 或 [FontStyle](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.documents.textelement.fontstyle.aspx) 来区分链接文本。

**HyperlinkButton** 

默认情况下，当你将一个字符串设置为 [Content](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.contentcontrol.content.aspx) 属性的值时，HyperlinkButton 显示为带下划线的文本。

在以下情况下，文本不会显示为带下划线：
- 将 [TextBlock](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.textblock.aspx) 设置为 Content 属性的值，并在 TextBlock 上设置 [Text](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.textblock.text.aspx) 属性。
- 重新设置 HyperlinkButton 的模板，并更改 [ContentPresenter](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.contentpresenter.aspx) 模板部分的名称。

如果你需要一个显示为不带下划线的文本的按钮，请考虑使用标准按钮控件，并对其 Style 属性应用内置 `TextBlockButtonStyle` 系统资源。

## <a name="notes-for-hyperlink-text-element"></a>Hyperlink 文本元素的说明

本部分仅适用于超链接文本元素，不适用于 HyperlinkButton 控件。

**输入事件**

由于超链接不是 [UIElement](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.uielement.aspx)，因此它没有 UI 元素输入事件集，如 Tapped、PointerPressed 等。 不过，超链接具有其自己的 Click 事件以及系统加载任何指定为 NavigateUri 的 URI 的隐式行为。 系统会处理所有应调用超链接操作并引发 Click 事件作为回应的输入操作。

**内容**

超链接对可能存在于其 [Inlines](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.documents.span.inlines.aspx) 集合中的内容具有限制。 具体来说，超链接仅允许 [Run](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.documents.run.aspx) 和非另一个超链接的其他 [Span]() 类型。 [InlineUIContainer](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.documents.inlineuicontainer.aspx) 不可在超链接的内联集合中。 尝试添加受限制的内容会引发无效参数异常或 XAML 分析异常。

**超链接和主题/样式行为**

超链接不从 [Control](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.control.aspx) 继承，因此它没有 [Style](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.frameworkelement.style.aspx) 属性或 [Template](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.control.template.aspx)。 你可以编辑从 [TextElement](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.documents.textelement.aspx) 继承的属性（如 Foreground 或 FontFamily）来更改超链接的外观，但你无法使用常见样式或模板来应用更改。 请考虑为超链接属性的值使用常见资源（而不是使用模板）以保持一致性。 超链接的某些属性使用系统提供的 {ThemeResource} 标记扩展值的默认值。 这使超链接外观可以在用户在运行时更改系统主题时以相应的方式进行切换。

超链接的默认颜色为系统的主题色。 你可以设置 [Foreground](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.documents.textelement.foreground.aspx) 属性来覆盖此颜色。

## <a name="recommendations"></a>建议

-   仅使用超链接进行导航；不要使用它们进行其他操作。
-   将字形渐变中的正文样式用于基于文本的超链接。 阅读有关[fonts and the Windows 10 type ramp](../style/typography.md)的内容。
-   使离散型超链接具有足够的间隔，以便用户可以区分它们，并且可以轻松地选择每个超链接。
-   将工具提示添加到可指示用户将被定向到的位置的超链接中。 如果用户将被定向到外部站点，则将顶级域名包含在工具提示中，并将文本样式设置为辅助字体颜色。

## <a name="get-the-sample-code"></a>获取示例代码

- [XAML 控件库示例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics) - 以交互式格式查看所有 XAML 控件。

## <a name="related-articles"></a>相关文章

- [文本控件](text-controls.md)
- [工具提示指南](tooltips.md)

**对于开发人员 (XAML)**
- [Windows.UI.Xaml.Documents.Hyperlink 类](https://msdn.microsoft.com/library/windows/apps/dn279356)
- [Windows.UI.Xaml.Controls.HyperlinkButton 类](https://msdn.microsoft.com/library/windows/apps/br242739)
