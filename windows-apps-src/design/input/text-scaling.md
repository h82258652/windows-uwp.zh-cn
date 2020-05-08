---
Description: 构建支持平台文本缩放的 Windows 应用和自定义/模板化控件。
title: 文本缩放
label: Text scaling
template: detail.hbs
keywords: UWP，文本，缩放，辅助功能，"轻松访问"，显示，"放大文本"，用户交互，输入
ms.date: 08/02/2018
ms.topic: article
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 4db3af0d2ec0ce1dbd0866f569ad9bf9b0392aa8
ms.sourcegitcommit: 0dee502484df798a0595ac1fe7fb7d0f5a982821
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/08/2020
ms.locfileid: "82970562"
---
# <a name="text-scaling"></a>文本缩放

![文本缩放100% 到225% 的示例](images/coretext/text-scaling-news-hero-small.png)  
*Windows 10 中的文本缩放示例（100% 至225%）*

## <a name="overview"></a>概述

将计算机屏幕上的文本（从移动设备移动到台式计算机监视器到桌面监视器）读取 Surface Hub 到很多人都非常困难。 相反，某些用户会发现在应用和网站中使用的字体大小要大于所需的大小。

为了确保最广泛的用户能够尽可能使文本更清晰，Windows 提供了一种功能，使用户能够更改操作系统和单个应用程序的相对字体大小。 用户不必使用放大镜应用（通常只放大屏幕某个区域内的全部内容，并带来其自身的可用性问题）、更改显示分辨率或依赖 DPI 缩放（根据显示器和典型观看距离调整所有内容的大小），而是可以快速访问设置，只调整文本大小，调整范围为 100%（默认大小）至最高 225%。

## <a name="support"></a>支持

默认情况下，通用 Windows 应用程序（标准和 PWA）支持文本缩放。

如果您的 Windows 应用程序包括自定义控件、自定义文本表面、硬编码控件高度、较早的框架或第三方框架，则您可能需要进行一些更新，以确保用户的一致且实用的体验。  

DirectWrite、GDI 和 XAML SwapChainPanels 不能以本机方式支持文本缩放，而 Win32 支持仅限于菜单、图标和工具栏。  

<!-- If you want to support text scaling in your application with these frameworks, you’ll need to support the text scaling change event outlined below and provide alternative sizes for your UI and content.   -->

## <a name="user-experience"></a>用户体验

用户可以通过 "设置-> 轻松访问->" 视觉/显示屏幕上的 "放大文本" 滑块调整文本比例。

![文本缩放100% 到225% 的示例](images/coretext/text-scaling-settings-100-small.png)  
*设置中的文本缩放设置-> 轻松访问-> 视觉/显示屏幕*

## <a name="ux-guidance"></a>UX 指南

调整文本的大小时，控件和容器还必须调整大小和重排以容纳文本及其新布局。 如前文所述，根据应用、框架和平台，将完成大部分工作。 以下 UX 指导涵盖了不是的情况。

### <a name="use-the-platform-controls"></a>使用平台控件

我们已经说过了吗？ 值得一提的是：如果可能，请始终使用随各种 Windows 应用框架一起提供的内置控件，以最大程度地获得最全面的用户体验。

例如，所有 UWP 文本控件都支持完全文本缩放体验，无需任何自定义或模板化。

下面是基本 UWP 应用中的一个代码片段，其中包含几个标准文本控件：

``` xaml
<Grid>
    <Grid.RowDefinitions>
        <RowDefinition Height="Auto"/>
        <RowDefinition Height="Auto" />
        <RowDefinition Height="Auto"/>
    </Grid.RowDefinitions>
    <TextBlock Grid.Row="0" 
                Style="{ThemeResource TitleTextBlockStyle}"
                Text="Text scaling test" 
                HorizontalTextAlignment="Center" />
    <Grid Grid.Row="1">
        <Grid.ColumnDefinitions>
            <ColumnDefinition Width="Auto"/>
            <ColumnDefinition Width="*"/>
            <ColumnDefinition Width="Auto"/>
        </Grid.ColumnDefinitions>
        <Image Grid.Column="0" 
                Source="Assets/StoreLogo.png" 
                Width="450" Height="450"/>
        <StackPanel Grid.Column="1" 
                    HorizontalAlignment="Center">
            <TextBlock TextWrapping="WrapWholeWords">
                The quick brown fox jumped over the lazy dog.
            </TextBlock>
            <TextBox PlaceholderText="Type something here" />
        </StackPanel>
        <Image Grid.Column="2" 
                Source="Assets/StoreLogo.png" 
                Width="450" Height="450"/>
    </Grid>
    <TextBlock Grid.Row="2" 
                Style="{ThemeResource TitleTextBlockStyle}"
                Text="Text scaling test footer" 
                HorizontalTextAlignment="Center" />
</Grid>
```

![动画文本缩放100% 至225%](images/coretext/text-scaling.gif)  
*动画文本缩放*

### <a name="use-auto-sizing"></a>使用自动调整大小

不要为控件指定绝对大小。 尽可能让平台根据用户和设备设置自动调整控件的大小。  

在上面的示例中，我们对一组网格`Auto`列`*`使用和宽度值，并让平台根据网格中包含的元素的大小调整应用程序布局。

``` xaml
<Grid.ColumnDefinitions>
    <ColumnDefinition Width="Auto"/>
    <ColumnDefinition Width="*"/>
    <ColumnDefinition Width="Auto"/>
</Grid.ColumnDefinitions>
```

### <a name="use-text-wrapping"></a>使用文字环绕

若要确保应用程序的布局尽可能灵活和可调整，请在包含文本的任何控件中启用文本换行（许多控件默认情况下不支持文本换行）。

如果未指定文本换行，则平台将使用其他方法调整布局，包括剪辑（请参阅上一个示例）。

此处，我们使用`AcceptsReturn`和`TextWrapping` TextBox 属性来确保布局尽可能灵活。

``` xaml
<TextBox PlaceholderText="Type something here" 
          AcceptsReturn="True" TextWrapping="Wrap" />
```

![带有文本换行的动画文本缩放100% 至225%](images/coretext/text-scaling-textwrap.gif)  
*带有文本换行的动画文本缩放*

### <a name="specify-text-trimming-behavior"></a>指定文本修整行为

如果文本换行不是首选行为，则大多数文本控件允许您剪切文本或指定文本修整行为的省略号。 剪切是椭圆的首选椭圆，因为椭圆占据空间本身。

> [!NOTE]
> 如果需要剪裁文本，请剪辑字符串末尾，而不是开始。

在此示例中，我们将演示如何使用[system.windows.controls.textblock.texttrimming](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.textblock.texttrimming)属性在 TextBlock 中剪辑文本。

``` xaml
<TextBlock TextTrimming="Clip">
    The quick brown fox jumped over the lazy dog.
</TextBlock>
```

![文本剪辑的文本缩放100% 至225%](images/coretext/text-scaling-clipping-small.png)  
*带有文本剪辑的文本缩放*

### <a name="use-a-tooltip"></a>使用工具提示

如果剪辑文本，请使用工具提示向用户提供完整文本。

在这里，我们将向不支持文本换行的 TextBlock 添加工具提示：

``` xaml
<TextBlock TextTrimming="Clip">
    <ToolTipService.ToolTip>
        <ToolTip Content="The quick brown fox jumped over the lazy dog."/>
    </ToolTipService.ToolTip>
    The quick brown fox jumped over the lazy dog.
</TextBlock>
```

### <a name="dont-scale-font-based-icons-or-symbols"></a>不缩放基于字体的图标或符号

使用基于字体的图标进行强调或装饰时，请禁用对这些字符进行缩放。

对于大多数[IsTextScaleFactorEnabled](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.control.istextscalefactorenabled) XAML 控件， `false`请将 IsTextScaleFactorEnabled 属性设置为。

### <a name="support-text-scaling-natively"></a>支持本机文本缩放

处理自定义框架和控件中的[TextScaleFactorChanged](https://docs.microsoft.com/uwp/api/windows.ui.viewmanagement.uisettings.textscalefactorchanged) UISettings 系统事件。 每次用户在其系统上设置文本缩放因子时，都会引发此事件。

## <a name="summary"></a>总结

本主题概述了 Windows 中的文本缩放支持，并包括有关如何自定义用户体验的 UX 和开发人员指南。

## <a name="related-articles"></a>相关文章

### <a name="api-reference"></a>API 参考

- [IsTextScaleFactorEnabled](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.control.istextscalefactorenabled)
- [TextScaleFactorChanged](https://docs.microsoft.com/uwp/api/windows.ui.viewmanagement.uisettings.textscalefactorchanged)
