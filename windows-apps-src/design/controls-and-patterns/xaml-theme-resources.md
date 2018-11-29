---
Description: Theme resources in XAML are a set of resources that apply different values depending on which system theme is active.
MS-HAID: dev\_ctrl\_layout\_txt.xaml\_theme\_resources
MSHAttr: PreferredLib:/library/windows/apps
Search.Product: eADQiWindows 10XVcnh
title: XAML 主题资源
ms.assetid: 41B87DBF-E7A2-44E9-BEBA-AF6EEBABB81B
label: XAML theme resources
template: detail.hbs
ms.date: 05/19/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 770896f467ff3a2c24fff65fdf16f1e13c83b688
ms.sourcegitcommit: b5c9c18e70625ab770946b8243f3465ee1013184
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/28/2018
ms.locfileid: "7976677"
---
# <a name="xaml-theme-resources"></a>XAML 主题资源

XAML 中的主题资源是一组可应用不同值的资源，具体取决于哪个系统主题处于活动状态。 XAML 框架支持三种主题：“浅色”、“深色”和“高对比度”。

**先决条件**：本主题假设你已阅读 [ResourceDictionary 和 XAML 资源引用](resourcedictionary-and-xaml-resource-references.md)。

## <a name="theme-resources-v-static-resources"></a>主题资源与 静态资源

有两个 XAML 标记扩展，可从现有 XAML 资源字典引用 XAML 资源：[{StaticResource} 标记扩展](../../xaml-platform/staticresource-markup-extension.md)和 [{ThemeResource} 标记扩展](../../xaml-platform/themeresource-markup-extension.md)。

将在应用加载时和随后每次在运行时更改主题时，发生 [{ThemeResource} 标记扩展](../../xaml-platform/themeresource-markup-extension.md)评估。 这通常是用户更改其设备设置或从更改其当前主题的应用内进行编程更改所引起的。

相比之下，仅在应用首次加载 XAML 时评估 [{StaticResource} 标记扩展](../../xaml-platform/staticresource-markup-extension.md)。 它未更新。 这类似于在应用启动时使用实际运行时值在 XAML 中进行查找和替换。

## <a name="theme-resources-in-the-resource-dictionary-structure"></a>资源字典结构中的主题资源

每个主题资源都是 XAML 文件 themeresources.xaml 的一部分。 出于设计目的，themeresources.xaml 在 Windows 软件开发工具包 (SDK) 安装中的 \\(Program Files)\\Windows Kits\\10\\DesignTime\\CommonConfiguration\\Neutral\\UAP\\&lt;SDK version&gt;\\Generic 文件夹中提供。 themeresources.xaml 中的资源字典还在同一目录中的 generic.xaml 中重现。

Windows 运行时不使用这些物理文件进行运行时查找。 这就是它们专门在 DesignTime 文件夹中，并且在默认情况下不复制到应用中的原因。 这些资源字典在内存中作为 Windows 运行时本身的一部分存在，并且你的应用的对主题资源（或系统资源）的 XAML 资源引用将在运行时在该处进行解析。

## <a name="guidelines-for-custom-theme-resources"></a>自定义主题资源指南

当定义和使用你自己的自定义主题资源时，请遵循以下指南：

- 除“高对比度”字典以外，还需指定“浅色”和“深色”的主题字典。 尽管你可以在将“默认”作为键的情况下创建 [ResourceDictionary](https://msdn.microsoft.com/library/windows/apps/br208794)，但建议以明确的方式改用“浅色”、“深色”和“高对比度”。

- 在以下位置使用 [{ThemeResource} 标记扩展](../../xaml-platform/themeresource-markup-extension.md)：Styles、Setters、Control 模板、Property 资源库和 Animations。

- 请勿在你的 [ThemeDictionaries](https://msdn.microsoft.com/library/windows/apps/br208807) 内的资源定义中使用 [{ThemeResource} 标记扩展](../../xaml-platform/themeresource-markup-extension.md)。 改用 [{StaticResource} 标记扩展](../../xaml-platform/staticresource-markup-extension.md)。

    例外：可以使用 [{ThemeResource} 标记扩展](../../xaml-platform/themeresource-markup-extension.md)，以引用对于 [ThemeDictionaries](https://msdn.microsoft.com/library/windows/apps/br208807) 中的应用主题不可知的资源。 这些资源的示例是主题色资源（例如 `SystemAccentColor`）或通常带有“SystemColor”前缀的系统颜色资源（例如 `SystemColorButtonFaceColor`）。

> [!CAUTION]
> 如果不遵循这些指南，可能会出现与应用中的主题相关的意外行为。 有关详细信息，请参阅[主题资源疑难解答](#troubleshooting-theme-resources)部分。

## <a name="the-xaml-color-ramp-and-theme-dependent-brushes"></a>XAML 颜色渐变和依赖于主题的画笔

由“浅色”、“深色”和“高对比度”主题组合的颜色集构成了 XAML 中的 *Windows 颜色渐变*。 无论是想要修改系统主题，还是将系统主题应用到自己的 XAML 元素，了解如何构建颜色资源都非常重要。

有关如何在 UWP 应用中应用颜色的更多信息，请参阅 [UWP 应用中的颜色](../style/color.md)。

### <a name="light-and-dark-theme-colors"></a>浅色和深色主题颜色

XAML 框架提供了一个已命名的 [Color](/uwp/api/Windows.UI.Color) 资源集，其中包含的值专为“浅色”和“深色”主题而定制。 用于引用这些内容的键遵循以下命名格式：`System[Simple Light/Dark Name]Color`。

针对 XAML 框架提供的“浅色”和“深色”资源，此表列出了该颜色的键、简单名称和字符串表示形式（使用 \#aarrggbb 格式）。 该键用于引用应用中的资源。 将“简单浅色/深色名称”用作我们后面介绍的画笔命名约定的一部分。

| 键                             | 简单浅色/深色名称 | 浅色      | 深色       |
|---------------------------------|------------------------|------------|------------|
| SystemAltHighColor              | AltHigh                | \#FFFFFFFF | \#FF000000 |
| SystemAltLowColor               | AltLow                 | \#33FFFFFF | \#33000000 |
| SystemAltMediumColor            | AltMedium              | \#99FFFFFF | \#99000000 |
| SystemAltMediumHighColor        | AltMediumHigh          | \#CCFFFFFF | \#CC000000 |
| SystemAltMediumLowColor         | AltMediumLow           | \#66FFFFFF | \#66000000 |
| SystemBaseHighColor             | BaseHigh               | \#FF000000 | \#FFFFFFFF |
| SystemBaseLowColor              | BaseLow                | \#33000000 | \#33FFFFFF |
| SystemBaseMediumColor           | BaseMedium             | \#99000000 | \#99FFFFFF |
| SystemBaseMediumHighColor       | BaseMediumHigh         | \#CC000000 | \#CCFFFFFF |
| SystemBaseMediumLowColor        | BaseMediumLow          | \#66000000 | \#66FFFFFF |
| SystemChromeAltLowColor         | ChromeAltLow           | \#FF171717 | \#FFF2F2F2 |
| SystemChromeBlackHighColor      | ChromeBlackHigh        | \#FF000000 | \#FF000000 |
| SystemChromeBlackLowColor       | ChromeBlackLow         | \#33000000 | \#33000000 |
| SystemChromeBlackMediumLowColor | ChromeBlackMediumLow   | \#66000000 | \#66000000 |
| SystemChromeBlackMediumColor    | ChromeBlackMedium      | \#CC000000 | \#CC000000 |
| SystemChromeDisabledHighColor   | ChromeDisabledHigh     | \#FFCCCCCC | \#FF333333 |
| SystemChromeDisabledLowColor    | ChromeDisabledLow      | \#FF7A7A7A | \#FF858585 |
| SystemChromeHighColor           | ChromeHigh             | \#FFCCCCCC | \#FF767676 |
| SystemChromeLowColor            | ChromeLow              | \#FFF2F2F2 | \#FF171717 |
| SystemChromeMediumColor         | ChromeMedium           | \#FFE6E6E6 | \#FF1F1F1F |
| SystemChromeMediumLowColor      | ChromeMediumLow        | \#FFF2F2F2 | \#FF2B2B2B |
| SystemChromeWhiteColor          | ChromeWhite            | \#FFFFFFFF | \#FFFFFFFF |
| SystemListLowColor              | ListLow                | \#19000000 | \#19FFFFFF |
| SystemListMediumColor           | ListMedium             | \#33000000 | \#33FFFFFF |

:::row:::
    :::column:::
        #### Light theme
    :::column-end:::
    :::column:::
        #### Dark theme
    :::column-end:::
:::row-end:::

#### <a name="base"></a>Base

:::row:::
    :::column:::
        ![The base light theme](images/themes/light-base.png)
    :::column-end:::
    :::column:::
        ![The base dark theme](images/themes/dark-base.png)
    :::column-end:::
:::row-end:::

#### <a name="alt"></a>备用

:::row:::
    :::column:::
        ![The alt light theme](images/themes/light-alt.png)
    :::column-end:::
    :::column:::
        ![The alt dark theme](images/themes/dark-alt.png)
    :::column-end:::
:::row-end:::

#### <a name="list"></a>列表

:::row:::
    :::column:::
        ![The list light theme](images/themes/light-list.png)
    :::column-end:::
    :::column:::
        ![The list dark theme](images/themes/dark-list.png)
    :::column-end:::
:::row-end:::

#### <a name="chrome"></a>镶边

:::row:::
    :::column:::
        ![The chrome light theme](images/themes/light-chrome.png)
    :::column-end:::
    :::column:::
        ![The chrome dark theme](images/themes/dark-chrome.png)
    :::column-end:::
:::row-end:::

### <a name="windows-system-high-contrast-colors"></a>Windows 系统高对比度颜色

除了 XAML 框架提供的资源集，还存在派生自 Windows 系统调色板的颜色值集。 这些颜色并不特定于 Windows 运行时或通用 Windows 平台(UWP) 应用。 然而，当使用“高度对比”主题运行系统（并且应用正在运行）时，许多 XAML [Brush](/uwp/api/Windows.UI.Xaml.Media.Brush) 资源都将使用这些颜色。 XAML 框架提供这些系统范围的颜色作为键控资源。 这些键遵循以下命名格式：`SystemColor[name]Color`。

此表列出了 XAML 提供的系统范围的颜色，可作为派生自 Windows 系统调色板的资源对象。 “轻松使用名称”列显示了如何在 Windows 设置 UI 中向颜色添加标签。 “简单的高对比度名称”列中使用一个词描述如何在 XAML 常用控件中应用该颜色。 它将用作我们后面介绍的画笔命名约定的一部分。 如果系统未以高对比度运行，则“初始默认设置”列会显示你已得到的值。

| 键                           | 轻松使用名称            | 简单的高对比度名称 | 初始默认设置 |
|-------------------------------|--------------------------------|--------------------------|-----------------|
| SystemColorButtonFaceColor    | **按钮文本**（背景）   | 后台               | \#FFF0F0F0      |
| SystemColorButtonTextColor    | **按钮文本**（前景）   | Foreground               | \#FF000000      |
| SystemColorGrayTextColor      | **失效文本**              | Disabled                 | \#FF6D6D6D      |
| SystemColorHighlightColor     | **选定文本**（背景） | Highlight                | \#FF3399FF      |
| SystemColorHighlightTextColor | **选定文本**（前景） | HighlightAlt             | \#FFFFFFFF      |
| SystemColorHotlightColor      | **超链接**                 | Hyperlink                | \#FF0066CC      |
| SystemColorWindowColor        | **后台**                 | PageBackground           | \#FFFFFFFF      |
| SystemColorWindowTextColor    | **文本**                       | PageText                 | \#FF000000      |

通过“轻松使用设置中心”，Windows 提供不同的高对比度主题，并且支持用户设置特定颜色以用于其高对比度设置，如此处所示。 因此，无法提供一个明确的高对比颜色值列表。

![Windows 高对比度设置 UI](images/high-contrast-settings.png)

有关支持高对比度主题的详细信息，请参阅[高对比度主题](https://msdn.microsoft.com/library/windows/apps/mt244346)。

### <a name="system-accent-color"></a>系统主题色

除了系统高对比度主题颜色外，通过使用键 `SystemAccentColor`，系统主题色还会作为特定的颜色资源提供。 在运行时，该资源获取用户已在 Windows 个性化设置中指定为主题色的颜色。

> [!NOTE]
> 虽然可能会覆盖系统颜色资源，但这是遵循用户的颜色选择（特别是对于高对比度设置而言）的最佳做法。

### <a name="theme-dependent-brushes"></a>依赖于主题的画笔

使用前面部分中所示的颜色资源来设置系统主题资源字典中的 [SolidColorBrush](/uwp/api/Windows.UI.Xaml.Media.SolidColorBrush.Color) 资源的 [Color](/uwp/api/Windows.UI.Xaml.Media.SolidColorBrush) 属性。 你可以使用画笔资源将颜色应用到 XAML 元素中。 画笔资源的键遵循以下命名格式：`SystemControl[Simple HighContrast name][Simple light/dark name]Brush`。 例如，`SystemControlBackroundAltHighBrush`。

让我们看一下如何在运行时确定此画笔的颜色值。 在“浅色”和“深色”资源字典中，定义此画笔，如下所示：

`<SolidColorBrush x:Key="SystemControlBackgroundAltHighBrush" Color="{StaticResource SystemAltHighColor}"/>`

在“高对比度”资源字典中，定义此画笔，如下所示：

`<SolidColorBrush x:Key="SystemControlBackgroundAltHighBrush" Color="{ThemeResource SystemColorButtonFaceColor}"/>`

当将此画笔应用到 XAML 元素时，其颜色在运行时由当前主题决定，如此表中所示。

| 主题        | 颜色简单名称 | 颜色资源             | 运行时值                                              |
|--------------|-------------------|----------------------------|------------------------------------------------------------|
| 浅色        | AltHigh           | SystemAltHighColor         | \#FFFFFFFF                                                 |
| 深色         | AltHigh           | SystemAltHighColor         | \#FF000000                                                 |
| 高对比度 | 后台        | SystemColorButtonFaceColor | 在设置中指定的按钮背景的颜色。 |

你可以使用 `SystemControl[Simple HighContrast name][Simple light/dark name]Brush` 命名方案确定要应用到你自己的 XAML 元素的画笔。

<!--
For many examples of how the brushes are used in the XAML control templates, see the [Default control styles and templates](default-control-styles-and-templates.md).
-->

> [!NOTE]
> 并非每个 \[*Simple HighContrast name*\]\[*Simple light/dark name*\] 组合都作为画笔资源提供。

## <a name="the-xaml-type-ramp"></a>XAML 类型渐变

themeresources.xaml 文件将定义若干个资源，这些资源定义可应用到 UI 中的文本容器（特别是 [TextBlock](https://msdn.microsoft.com/library/windows/apps/br208849) 或 [RichTextBlock](https://msdn.microsoft.com/library/windows/apps/br209652)）的 [Style](https://msdn.microsoft.com/library/windows/apps/br227565)。 它们不是默认的隐式样式。 通过它们，你可以更轻松地创建匹配[字体指南](../style/typography.md)中记录的 *Windows 类型渐变*的 XAML UI 定义。

这些样式用于你要应用到整个文本容器的文本属性。 如果你仅想要将样式应用到该文本部分，请在容器中的文本元素上（例如 [TextBlock.Inlines](https://msdn.microsoft.com/library/windows/apps/br209959) 中的 [Run](https://msdn.microsoft.com/library/windows/apps/br209668) 上或 [RichTextBlock.Blocks](https://msdn.microsoft.com/library/windows/apps/br244503) 中的 [Paragraph](https://msdn.microsoft.com/library/windows/apps/br244347) 上）设置属性。

当应用于 [TextBlock](https://msdn.microsoft.com/library/windows/apps/br209652) 时，这些样式如下所示：

![文本块样式](../style/images/type/text-block-type-ramp.svg)

```XAML
<TextBlock Text="Header" Style="{StaticResource HeaderTextBlockStyle}"/>
<TextBlock Text="SubHeader" Style="{StaticResource SubheaderTextBlockStyle}"/>
<TextBlock Text="Title" Style="{StaticResource TitleTextBlockStyle}"/>
<TextBlock Text="SubTitle" Style="{StaticResource SubtitleTextBlockStyle}"/>
<TextBlock Text="Base" Style="{StaticResource BaseTextBlockStyle}"/>
<TextBlock Text="Body" Style="{StaticResource BodyTextBlockStyle}"/>
<TextBlock Text="Caption" Style="{StaticResource CaptionTextBlockStyle}"/>
```

有关如何在应用中使用 UWP 类型渐变的指南，请参阅 [UWP 应用中的版式](../style/typography.md)。

### <a name="basetextblockstyle"></a>BaseTextBlockStyle

**TargetType**：[TextBlock](https://msdn.microsoft.com/library/windows/apps/br209652)

为所有其他 [TextBlock](https://msdn.microsoft.com/library/windows/apps/br209652) 容器样式提供常用属性。

```XAML
<!-- Usage -->
<TextBlock Text="Base" Style="{StaticResource BaseTextBlockStyle}"/>

<!-- Style definition -->
<Style x:Key="BaseTextBlockStyle" TargetType="TextBlock">
    <Setter Property="FontFamily" Value="Segoe UI"/>
    <Setter Property="FontWeight" Value="SemiBold"/>
    <Setter Property="FontSize" Value="15"/>
    <Setter Property="TextTrimming" Value="None"/>
    <Setter Property="TextWrapping" Value="Wrap"/>
    <Setter Property="LineStackingStrategy" Value="MaxHeight"/>
    <Setter Property="TextLineBounds" Value="Full"/>
</Style>
```

### <a name="headertextblockstyle"></a>HeaderTextBlockStyle

```XAML
<!-- Usage -->
<TextBlock Text="Header" Style="{StaticResource HeaderTextBlockStyle}"/>

<!-- Style definition -->
<Style x:Key="HeaderTextBlockStyle" TargetType="TextBlock"
       BasedOn="{StaticResource BaseTextBlockStyle}">
    <Setter Property="FontSize" Value="46"/>
    <Setter Property="FontWeight" Value="Light"/>
    <Setter Property="OpticalMarginAlignment" Value="TrimSideBearings"/>
</Style>
```

### <a name="subheadertextblockstyle"></a>SubheaderTextBlockStyle

```XAML
<!-- Usage -->
<TextBlock Text="SubHeader" Style="{StaticResource SubheaderTextBlockStyle}"/>

<!-- Style definition -->
<Style x:Key="SubheaderTextBlockStyle" TargetType="TextBlock" 
       BasedOn="{StaticResource BaseTextBlockStyle}">
    <Setter Property="FontSize" Value="34"/>
    <Setter Property="FontWeight" Value="Light"/>
    <Setter Property="OpticalMarginAlignment" Value="TrimSideBearings"/>
</Style>
```

### <a name="titletextblockstyle"></a>TitleTextBlockStyle

```XAML
<!-- Usage -->
<TextBlock Text="Title" Style="{StaticResource TitleTextBlockStyle}"/>

<!-- Style definition -->
<Style x:Key="TitleTextBlockStyle" TargetType="TextBlock" 
       BasedOn="{StaticResource BaseTextBlockStyle}">
    <Setter Property="FontWeight" Value="SemiLight"/>
    <Setter Property="FontSize" Value="24"/>
    <Setter Property="OpticalMarginAlignment" Value="TrimSideBearings"/>
</Style>
```

### <a name="subtitletextblockstyle"></a>SubtitleTextBlockStyle

```XAML
<!-- Usage -->
<TextBlock Text="SubTitle" Style="{StaticResource SubtitleTextBlockStyle}"/>

<!-- Style definition -->
<Style x:Key="SubtitleTextBlockStyle" TargetType="TextBlock" 
       BasedOn="{StaticResource BaseTextBlockStyle}">
    <Setter Property="FontWeight" Value="Normal"/>
    <Setter Property="FontSize" Value="20"/>
    <Setter Property="OpticalMarginAlignment" Value="TrimSideBearings"/>
</Style>
```

### <a name="bodytextblockstyle"></a>BodyTextBlockStyle

```XAML
<!-- Usage -->
<TextBlock Text="Body" Style="{StaticResource BodyTextBlockStyle}"/>

<!-- Style definition -->
<Style x:Key="BodyTextBlockStyle" TargetType="TextBlock" 
       BasedOn="{StaticResource BaseTextBlockStyle}">
    <Setter Property="FontWeight" Value="Normal"/>
    <Setter Property="FontSize" Value="15"/>
</Style>
```

### <a name="captiontextblockstyle"></a>CaptionTextBlockStyle

```XAML
<!-- Usage -->
<TextBlock Text="Caption" Style="{StaticResource CaptionTextBlockStyle}"/>

<!-- Style definition -->
<Style x:Key="CaptionTextBlockStyle" TargetType="TextBlock" 
       BasedOn="{StaticResource BaseTextBlockStyle}">
    <Setter Property="FontSize" Value="12"/>
    <Setter Property="FontWeight" Value="Normal"/>
</Style>
```

### <a name="baserichtextblockstyle"></a>BaseRichTextBlockStyle

**TargetType**：[RichTextBlock](https://msdn.microsoft.com/library/windows/apps/br227565)

为所有其他 [RichTextBlock](https://msdn.microsoft.com/library/windows/apps/br227565) 容器样式提供常用属性。

```XAML
<!-- Usage -->
<RichTextBlock Style="{StaticResource BaseRichTextBlockStyle}">
    <Paragraph>Rich text.</Paragraph>
</RichTextBlock>

<!-- Style definition -->
<Style x:Key="BaseRichTextBlockStyle" TargetType="RichTextBlock">
    <Setter Property="FontFamily" Value="Segoe UI"/>
    <Setter Property="FontWeight" Value="SemiBold"/>
    <Setter Property="FontSize" Value="15"/>
    <Setter Property="TextTrimming" Value="None"/>
    <Setter Property="TextWrapping" Value="Wrap"/>
    <Setter Property="LineStackingStrategy" Value="MaxHeight"/>
    <Setter Property="TextLineBounds" Value="Full"/>
    <Setter Property="OpticalMarginAlignment" Value="TrimSideBearings"/>
</Style>
```

### <a name="bodyrichtextblockstyle"></a>BodyRichTextBlockStyle

```XAML
<!-- Usage -->
<RichTextBlock Style="{StaticResource BodyRichTextBlockStyle}">
    <Paragraph>Rich text.</Paragraph>
</RichTextBlock>

<!-- Style definition -->
<Style x:Key="BodyRichTextBlockStyle" TargetType="RichTextBlock" BasedOn="{StaticResource BaseRichTextBlockStyle}">
    <Setter Property="FontWeight" Value="Normal"/>
</Style>
```

**注意**： [RichTextBlock](https://msdn.microsoft.com/library/windows/apps/br227565)样式不具有[TextBlock](https://msdn.microsoft.com/library/windows/apps/br209652)支持的所有文本渐变样式，主要是因为**RichTextBlock**的基于块的文档对象模型使在个别文本上设置属性元素。 同样，使用 XAML 内容属性设置 [TextBlock.Text](https://msdn.microsoft.com/library/windows/apps/br209676) 将出现以下情况：没有要设置样式的文本元素，因此你必须设置容器样式。 对于 **RichTextBlock**，这不是问题，因为其文本内容始终位于特定的文本元素（例如 [Paragraph](https://msdn.microsoft.com/library/windows/apps/br244503)）中，你可能在该元素中为页面标头、页面子标头和类似文本渐变定义应用 XAML 样式。

## <a name="miscellaneous-named-styles"></a>其他命名样式

还存在一组额外的键控 [Style](https://msdn.microsoft.com/library/windows/apps/br208849) 定义，可供你向 [Button](https://msdn.microsoft.com/library/windows/apps/br209265) 应用不同于其默认隐式样式的样式。

### <a name="textblockbuttonstyle"></a>TextBlockButtonStyle

**TargetType**：[ButtonBase](https://msdn.microsoft.com/library/windows/apps/br227736)

当你需要显示用户可以点击以进行操作的文本时，请将此样式应用到 [Button](https://msdn.microsoft.com/library/windows/apps/br209265)。 使用当前主题色设置该文本的样式以在交互时进行区分，并且该文本具有非常适用于文本的焦点矩形。 与 [HyperlinkButton](https://msdn.microsoft.com/library/windows/apps/br242739) 的隐式样式不同，**TextBlockButtonStyle** 不会为文本添加下划线。

该模板还设置显示文本的样式以使用 **SystemControlHyperlinkBaseMediumBrush**（适用于“PointerOver”状态）、**SystemControlHighlightBaseMediumLowBrush**（适用于“Pressed”状态）和 **SystemControlDisabledBaseLowBrush**（适用于“Disabled”状态）。

下面介绍向其应用了 **TextBlockButtonStyle** 资源的 [Button](https://msdn.microsoft.com/library/windows/apps/br209265)。

```XAML
<Button Content="Clickable text" Style="{StaticResource TextBlockButtonStyle}"
        Click="Button_Click"/>
```

它如下所示：

![按钮的样式看起来像文本](images/styles-textblock-button-style.png)

### <a name="navigationbackbuttonnormalstyle"></a>NavigationBackButtonNormalStyle

**TargetType**：[Button](https://msdn.microsoft.com/library/windows/apps/br209265)

此 [Style](https://msdn.microsoft.com/library/windows/apps/br208849) 提供的适用于 [Button](https://msdn.microsoft.com/library/windows/apps/br209265) 的完整模板可能是适用于导航应用的导航后退按钮。 默认尺寸是 40 x 40 像素。 若要定制样式，你可以明确设置 [Height](/uwp/api/Windows.UI.Xaml.FrameworkElement.Height)、[Width](/uwp/api/Windows.UI.Xaml.FrameworkElement.Width)、[FontSize](https://msdn.microsoft.com/library/windows/apps/br209406) 和 **Button** 上的其他属性，也可以使用 [BasedOn](https://msdn.microsoft.com/library/windows/apps/br208852) 创建一个派生的样式。

下面介绍向其应用了 **NavigationBackButtonNormalStyle** 资源的 [Button](https://msdn.microsoft.com/library/windows/apps/br209265)。

```XAML
<Button Style="{StaticResource NavigationBackButtonNormalStyle}" />
```

它如下所示：

![看起来像后退按钮样式的按钮](images/styles-back-button-normal.png)

### <a name="navigationbackbuttonsmallstyle"></a>NavigationBackButtonSmallStyle

**TargetType**：[Button](https://msdn.microsoft.com/library/windows/apps/br209265)

此 [Style](https://msdn.microsoft.com/library/windows/apps/br208849) 提供的适用于 [Button](https://msdn.microsoft.com/library/windows/apps/br209265) 的完整模板可能是适用于导航应用的导航后退按钮。 它与 **NavigationBackButtonNormalStyle** 类似，但尺寸为 30 x 30 像素。

下面是一个应用了 **NavigationBackButtonSmallStyle** 资源的 [Button](https://msdn.microsoft.com/library/windows/apps/br209265)。

```XAML
<Button Style="{StaticResource NavigationBackButtonSmallStyle}" />
```

## <a name="troubleshooting-theme-resources"></a>主题资源疑难解答

如果你不遵循[使用主题资源指南](#guidelines-for-using-theme-resources)，你可能会看到与你的应用中的主题相关的意外行为。主题

例如，当你打开浅色主题的浮出控件时，深色主题的应用的某些部分也会更改，就好像在浅色主题中一样。 或者如果你导航至浅色主题的页面，然后再导航回来，此时原始的深色主题页面（或部分页面）看起来仍像在浅色主题中一样。

通常，在提供“默认”主题和“高对比度”主题以支持高对比度方案，然后在你的应用的不同部分中使用“浅色”和“深色”主题时，会发生这些类型的问题。

例如，考虑此主题字典定义：

```XAML
<!-- DO NOT USE. THIS XAML DEMONSTRATES AN ERROR. -->
<ResourceDictionary>
  <ResourceDictionary.ThemeDictionaries>
    <ResourceDictionary x:Key="Default">
      <SolidColorBrush x:Key="myBrush" Color="{ThemeResource SystemBaseHighColor}"/>
    </ResourceDictionary>
    <ResourceDictionary x:Key="HighContrast">
      <SolidColorBrush x:Key="myBrush" Color="{ThemeResource SystemColorButtonFaceColor}"/>
    </ResourceDictionary>
  </ResourceDictionary.ThemeDictionaries>
</ResourceDictionary>
```

从直观上来说这看起来准确无误。 你想要在处于高对比度下时更改 `myBrush` 所指向的颜色，但在不处于高对比度下时，你依赖于 [{ThemeResource} 标记扩展](../../xaml-platform/themeresource-markup-extension.md)，以确保 `myBrush` 针对你的主题指向正确的颜色。 如果你的应用从未在可视化树内的元素上设置 [FrameworkElement.RequestedTheme](https://msdn.microsoft.com/library/windows/apps/dn298515)，这通常会按预期工作。 但是，在开始设置可视化树的不同部分的主题时，你的应用会立即出现问题。

与大多数其他 XAML 类型不同，产生该问题的原因是画笔是共享的资源。 如果你在 XAML 子树（具有可引用相同画笔资源的不同主题）中具有 2 个元素，则当框架遍历每个子树以更新其 [{ThemeResource} 标记扩展](../../xaml-platform/themeresource-markup-extension.md)表达式时，将在其他子树中反映对已共享画笔资源的更改，这不是你预期的结果。

若要解决此问题，除了“高对比度”以外，还使用“浅色”和“深色”主题的单独主题字典替换“默认”字典：

```XAML
<!-- DO NOT USE. THIS XAML DEMONSTRATES AN ERROR. -->
<ResourceDictionary>
  <ResourceDictionary.ThemeDictionaries>
    <ResourceDictionary x:Key="Light">
      <SolidColorBrush x:Key="myBrush" Color="{ThemeResource SystemBaseHighColor}"/>
    </ResourceDictionary>
    <ResourceDictionary x:Key="Dark">
      <SolidColorBrush x:Key="myBrush" Color="{ThemeResource SystemBaseHighColor}"/>
    </ResourceDictionary>
    <ResourceDictionary x:Key="HighContrast">
      <SolidColorBrush x:Key="myBrush" Color="{ThemeResource SystemColorButtonFaceColor}"/>
    </ResourceDictionary>
  </ResourceDictionary.ThemeDictionaries>
</ResourceDictionary>
```

但是，如果其中任何资源在继承属性（如 [Foreground](https://msdn.microsoft.com/library/windows/apps/br209414)）中引用，则仍会发生这些问题。 你的自定义控件模板可能使用 [{ThemeResource} 标记扩展](../../xaml-platform/themeresource-markup-extension.md)指定元素的前景色，但当框架将继承的值传播到子元素时，它可以直接引用由 {ThemeResource} 标记扩展表达式解决的资源。 这将在框架处理主题更改时导致问题，因为它遍历控件的可视化树。 它将重新计算 {ThemeResource} 标记扩展表达式以获取新的画笔资源，但尚未将该引用传播到你的控件的子元素；这种情况将在以后发生（例如在下一个测量阶段期间）。

因此，在遍历控件可视化树以响应主题更改后，该框架将遍历子项并更新对其设置的任何 [{ThemeResource} 标记扩展](../../xaml-platform/themeresource-markup-extension.md)表达式，或在其属性上设置的对象。 在此情况下会发生问题；框架将遍历画笔资源，并且因为它使用 {ThemeResource} 标记扩展指定其颜色，因此它将重新计算。

此时，框架看起来已被你的主题字典污染，因为它现在具有的资源来自从另一个字典设置其颜色的一个字典。

若要解决此问题，请使用 [{StaticResource} 标记扩展](../../xaml-platform/staticresource-markup-extension.md)而不是 [{ThemeResource} 标记扩展](../../xaml-platform/themeresource-markup-extension.md)。 通过应用指南，主题字典如下所示：

```XAML
<ResourceDictionary>
  <ResourceDictionary.ThemeDictionaries>
    <ResourceDictionary x:Key="Light">
      <SolidColorBrush x:Key="myBrush" Color="{StaticResource SystemBaseHighColor}"/>
    </ResourceDictionary>
    <ResourceDictionary x:Key="Dark">
      <SolidColorBrush x:Key="myBrush" Color="{StaticResource SystemBaseHighColor}"/>
    </ResourceDictionary>
    <ResourceDictionary x:Key="HighContrast">
      <SolidColorBrush x:Key="myBrush" Color="{ThemeResource SystemColorButtonFaceColor}"/>
    </ResourceDictionary>
  </ResourceDictionary.ThemeDictionaries>
</ResourceDictionary>
```

请注意，[{ThemeResource} 标记扩展](../../xaml-platform/themeresource-markup-extension.md)仍在“HighContrast”字典而不是 [{StaticResource} 标记扩展](../../xaml-platform/staticresource-markup-extension.md)中使用。 此情况属于在指南中的前面部分所给定的异常情况。 用于“高对比度”主题的大多数画笔值使用由系统全局控制的颜色选择，但会作为特别命名的资源暴露给 XAML（名称中带有“SystemColor”前缀）。 系统使用户可以通过“轻松访问中心”设置应该用于其高对比度设置的特定颜色。 这些颜色选择将应用于特殊命名的资源中。 当 XAML 框架检测到画笔在系统级别上发生更改时，它也将使用相同的主题更改事件更新这些画笔。 这就是在此处使用 {ThemeResource} 标记扩展的原因。