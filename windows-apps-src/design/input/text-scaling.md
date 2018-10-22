---
author: Karl-Bridge-Microsoft
Description: Build UWP apps and custom/templated controls that support platform text scaling.
title: 文本缩放
label: Text scaling
template: detail.hbs
keywords: UWP，文本、 缩放、 辅助功能、"轻松使用"，显示"请更大的 text"，用户交互，输入
ms.author: kbridge
ms.date: 08/02/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
ms.localizationpriority: medium
ms.openlocfilehash: 885ccc89fcbd4315eeed40c3546ef485c515294e
ms.sourcegitcommit: c4d3115348c8b54fcc92aae8e18fdabc3deb301d
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/22/2018
ms.locfileid: "5407447"
---
# <a name="text-scaling"></a>文本缩放

![文本缩放 225%到 100%的示例](images/coretext/text-scaling-news-hero-small.png)  
*文本缩放 （225%到 100%) 的 Windows 10 中的示例*

## <a name="overview"></a>概述

读取 （从到桌面到 Surface Hub 的巨大屏幕监视器的便携式计算机的移动设备） 的计算机屏幕上的文本会很困难，许多人。 相反，某些用户找到在应用和网站中用于为大于必要的字体大小。

若要确保文本是为尽可能广泛的用户清晰可见，Windows 提供的能力，用户在操作系统和个别应用程序更改相对字体大小。 而不是使用放大镜应用 （这通常只需放大屏幕区域内的所有内容，并引入了自己的可用性问题）、 更改显示分辨率或依赖于 DPI 缩放 （用以调整大小具体取决于显示和典型观看的所有内容距离），用户可以快速地访问用于调整大小只是文本，范围从 100%（默认大小） 的设置 225%。

## <a name="support"></a>支持

通用 Windows 应用程序 (这两个标准和 PWA)，支持文本缩放默认情况下。

如果你的 UWP 应用程序包含自定义控件、 自定义文本的图面、 硬编码控件高度、 早期的框架或第三方框架，你可能需要进行一些更新，以确保一致且有用为你的用户体验。  

DirectWrite、 GDI 和 XAML SwapChainPanels 本质上不支持文本缩放，而 Win32 支持仅限于菜单、 图标和工具栏。  

<!-- If you want to support text scaling in your application with these frameworks, you’ll need to support the text scaling change event outlined below and provide alternative sizes for your UI and content.   -->

## <a name="user-experience"></a>用户体验

用户可以调整文本比例变大滑块上设置-> 将文本与轻松访问-> 视觉/显示屏幕。

![文本缩放 225%到 100%的示例](images/coretext/text-scaling-settings-100-small.png)  
*文本缩放设置设置-> 轻松访问-> 视觉/显示屏幕*

## <a name="ux-guidance"></a>UX 指南

文本是调整大小时，必须还调整大小控件和容器并将其重新排列，从而使其适应文本和其新布局。 如前所述，具体取决于应用程序、 框架和平台，是为你完成此工作的大部分。 下面的 UX 指南介绍这些情况下，不是。

### <a name="use-the-platform-controls"></a>使用平台控件

没有我们说这已经？ 值得： 如果可能，始终使用与各种 Windows 应用框架提供的内置控件以获得最全面的用户体验可能的最小的努力。

例如，所有 UWP 文本控件都支持的完整文本缩放而无需任何自定义或模板化的体验。

下面是从基本 UWP 应用包含几个标准文本控件的代码段：

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

![动画的 225%到 100%缩放的文本](images/coretext/text-scaling.gif)  
*动画的文本缩放*

### <a name="use-auto-sizing"></a>使用自动调整大小

请不要指定绝对大小为你的控件。 只要有可能，让你的控件基于用户和设备设置自动调整大小的平台。  

在此代码段中前面的示例中，我们使用`Auto`和`*`的一组网格列和让平台宽度值调整应用布局，具体取决于包含网格中的元素的大小。

``` xaml
<Grid.ColumnDefinitions>
    <ColumnDefinition Width="Auto"/>
    <ColumnDefinition Width="*"/>
    <ColumnDefinition Width="Auto"/>
</Grid.ColumnDefinitions>
```

### <a name="use-text-wrapping"></a>使用文本换行

若要确保你的应用的布局是为高的灵活性和适应性尽可能，启用文本换行包含的文本 （许多控件不支持文本换行默认情况下） 的任何控件中。

如果你未指定文本换行，该平台是使用其他方法来调整的布局，包括剪裁 （请参阅上一示例）。

在这里，我们使用`AcceptsReturn`和`TextWrapping`TextBox 属性，以确保我们的布局是尽可能地灵活。

``` xaml
<TextBox PlaceholderText="Type something here" 
          AcceptsReturn="True" TextWrapping="Wrap" />
```

![动画 225%到 100%缩放带有环绕文本的文本](images/coretext/text-scaling-textwrap.gif)  
*带有环绕文本缩放的动画的文本*

### <a name="specify-text-trimming-behavior"></a>指定文本剪裁行为

如果文本换行不是首选的行为，大多数文本控件让剪裁文本或指定省略号文本剪裁行为。 剪裁省略号占用空间本身是首选到省略号。

> [!NOTE]
> 如果你需要剪裁文本，剪裁的字符串，而不是开头的末尾。

在此示例中，我们显示了如何在 TextBlock 中剪裁文本使用[TextTrimming](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.textblock.texttrimming)属性。

``` xaml
<TextBlock TextTrimming="Clip">
    The quick brown fox jumped over the lazy dog.
</TextBlock>
```

![缩放 100%到 225%剪裁文字的文本](images/coretext/text-scaling-clipping-small.png)  
*缩放剪裁文字的文本*

### <a name="use-a-tooltip"></a>使用工具提示

如果你剪裁文本，使用工具提示向用户提供的完整文本。

此处，我们将工具提示添加到 TextBlock 不支持文本换行：

``` xaml
<TextBlock TextTrimming="Clip">
    <ToolTipService.ToolTip>
        <ToolTip Content="The quick brown fox jumped over the lazy dog."/>
    </ToolTipService.ToolTip>
    The quick brown fox jumped over the lazy dog.
</TextBlock>
```

### <a name="dont-scale-font-based-icons-or-symbols"></a>不会随比例基于字体的图标或符号

当使用用于强调或修饰基于字体的图标，禁用这些字符上缩放。

将[IsTextScaleFactorEnabled](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.control.istextscalefactorenabled)属性设置为`false`大多数 xaml 控件。

### <a name="support-text-scaling-natively"></a>缩放本机支持文本

处理[TextScaleFactorChanged](https://docs.microsoft.com/uwp/api/windows.ui.viewmanagement.uisettings.textscalefactorchanged) UISettings 系统事件在你的自定义框架和控件。 用户在其系统设置文本比例因子每次引发此事件。

## <a name="summary"></a>摘要

本主题概要介绍的缩放支持 Windows 中的文本，并包括有关如何自定义用户体验的 UX 和开发人员指南。

## <a name="related-articles"></a>相关文章

### <a name="api-reference"></a>API 参考

- [IsTextScaleFactorEnabled](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.control.istextscalefactorenabled)
- [TextScaleFactorChanged](https://docs.microsoft.com/uwp/api/windows.ui.viewmanagement.uisettings.textscalefactorchanged)
