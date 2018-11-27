---
Description: Build UWP apps and custom/templated controls that support platform text scaling.
title: 文本缩放
label: Text scaling
template: detail.hbs
keywords: UWP 中，文本、 缩放、 辅助功能、"轻松使用"，显示"进行更大的 text"，用户交互，输入
ms.date: 08/02/2018
ms.topic: article
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: f81c435690c7bf17066be5f49de4994f146fc5c9
ms.sourcegitcommit: b11f305dbf7649c4b68550b666487c77ea30d98f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/27/2018
ms.locfileid: "7854225"
---
# <a name="text-scaling"></a>文本缩放

![文本缩放 225%到 100%的示例](images/coretext/text-scaling-news-hero-small.png)  
*在 Windows 10 （225%到 100%) 缩放的文本示例*

## <a name="overview"></a>概述

读取 （从到桌面的显示器连接到 Surface Hub 的巨大屏幕的便携式计算机的移动设备） 的计算机屏幕上的文本会很困难，许多人。 相反，某些用户查找用于在应用和网站可比必要更大的字体大小。

若要确保文本是为尽可能广泛的用户清晰可见，Windows 提供的能力，用户在操作系统和个别应用程序更改相对字体大小。 而不是使用放大镜应用 （这通常只需放大屏幕区域内的所有内容，并介绍了其自己的可用性问题）、 更改屏幕分辨率或依赖于 DPI 缩放 （这调整大小，具体取决于显示和典型观看的所有内容距离），用户可以快速地访问用于调整大小只包含文本，范围从 100%（默认大小） 的设置 225%。

## <a name="support"></a>支持

通用 Windows 应用程序 (这两个标准和 PWA)，支持默认缩放的文本。

如果你的 UWP 应用程序包含自定义控件、 自定义文本的图面，硬编码的控件的高度、 早期的框架或第三方框架，你可能需要进行一些更新，以确保为你的用户体验一致且有用。  

DirectWrite、 GDI，以及 XAML SwapChainPanels 本质上不支持文本缩放，而 Win32 支持仅限于菜单、 图标和工具栏。  

<!-- If you want to support text scaling in your application with these frameworks, you’ll need to support the text scaling change event outlined below and provide alternative sizes for your UI and content.   -->

## <a name="user-experience"></a>用户体验

用户可以调整文本缩放与进行文本大滑块上设置-> 轻松访问-> 视觉/显示屏幕。

![文本缩放 225%到 100%的示例](images/coretext/text-scaling-settings-100-small.png)  
*文本缩放设置设置-> 轻松访问-> 视觉/显示屏幕*

## <a name="ux-guidance"></a>UX 指南

文本调整大小时，必须也调整控件和容器并将其重新排列，从而使其适应文本和其新的布局。 如前所述，具体取决于应用、 框架和平台，大部分操作完成。 下面的 UX 指南介绍这些情况下，不是。

### <a name="use-the-platform-controls"></a>使用平台控件

没有我们说这已经？ 值得： 如果可能，始终使用与各种 Windows 应用框架提供的内置控件以获得最全面的用户体验可能的最大程度地工作。

例如，所有 UWP 文本控件都支持全文缩放而无需任何自定义或模板化的体验。

下面是从基本的 UWP 应用包含几个标准文本控件的代码段：

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

![缩放 225%到 100%的动画的文本](images/coretext/text-scaling.gif)  
*动画的文本缩放*

### <a name="use-auto-sizing"></a>使用自动调整大小

请不要指定绝对大小为你的控件。 只要有可能，允许控件根据用户和设备设置自动调整大小的平台。  

在此代码段中前面的示例中，我们使用`Auto`和`*`网格列，让该平台的一组的宽度值调整应用布局，具体取决于包含网格中的元素的大小。

``` xaml
<Grid.ColumnDefinitions>
    <ColumnDefinition Width="Auto"/>
    <ColumnDefinition Width="*"/>
    <ColumnDefinition Width="Auto"/>
</Grid.ColumnDefinitions>
```

### <a name="use-text-wrapping"></a>使用文本换行

若要确保你的应用的布局是为高的灵活性和适应性尽可能，启用文本换行中包含的文本 （许多控件不支持文本换行默认情况下） 的任何控件。

如果未指定文本换行，该平台是使用其他方法来调整的布局，其中包括剪裁 （请参阅上一示例）。

在这里，我们使用`AcceptsReturn`和`TextWrapping`TextBox 属性，以确保我们的布局是尽可能地灵活。

``` xaml
<TextBox PlaceholderText="Type something here" 
          AcceptsReturn="True" TextWrapping="Wrap" />
```

![带有环绕文本缩放 225%到 100%的文本的动画](images/coretext/text-scaling-textwrap.gif)  
*带有环绕文本缩放的动画的文本*

### <a name="specify-text-trimming-behavior"></a>指定文本剪裁行为

如果文本换行不是首选的行为，大多数文本控件让剪裁文本，或指定的文本剪裁行为的省略号。 剪裁省略号占用空间本身是优先于省略号。

> [!NOTE]
> 如果你需要剪裁文本，剪裁的字符串，而不是开头结束。

在此示例中，我们显示了如何在 TextBlock 中剪裁文本使用[TextTrimming](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.textblock.texttrimming)属性。

``` xaml
<TextBlock TextTrimming="Clip">
    The quick brown fox jumped over the lazy dog.
</TextBlock>
```

![缩放 100%到 225%剪裁文字的文本](images/coretext/text-scaling-clipping-small.png)  
*缩放剪裁文字的文本*

### <a name="use-a-tooltip"></a>使用工具提示

如果你剪裁文本，使用工具提示来向用户提供完整的文本。

在这里，我们将工具提示添加到 TextBlock 不支持文本换行：

``` xaml
<TextBlock TextTrimming="Clip">
    <ToolTipService.ToolTip>
        <ToolTip Content="The quick brown fox jumped over the lazy dog."/>
    </ToolTipService.ToolTip>
    The quick brown fox jumped over the lazy dog.
</TextBlock>
```

### <a name="dont-scale-font-based-icons-or-symbols"></a>不会随比例基于字体的图标或符号

强调或修饰使用基于字体的图标时, 禁用这些字符上缩放。

将[IsTextScaleFactorEnabled](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.control.istextscalefactorenabled)属性设置为`false`用于大多数 XAML 控件。

### <a name="support-text-scaling-natively"></a>缩放本机支持文本

在你的自定义框架和控件将[TextScaleFactorChanged](https://docs.microsoft.com/uwp/api/windows.ui.viewmanagement.uisettings.textscalefactorchanged) UISettings 系统事件处理。 每当用户在其系统设置文本缩放比例，会引发此事件。

## <a name="summary"></a>摘要

本主题概要介绍的缩放支持 Windows 中的文本，并包括有关如何自定义用户体验的 UX 和开发人员指南。

## <a name="related-articles"></a>相关文章

### <a name="api-reference"></a>API 参考

- [IsTextScaleFactorEnabled](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.control.istextscalefactorenabled)
- [TextScaleFactorChanged](https://docs.microsoft.com/uwp/api/windows.ui.viewmanagement.uisettings.textscalefactorchanged)
