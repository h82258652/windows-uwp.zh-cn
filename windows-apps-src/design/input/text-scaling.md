---
Description: 构建 UWP 应用和支持平台文本缩放的自定义/模板化控件。
title: 文本缩放
label: Text scaling
template: detail.hbs
keywords: UWP，文本、 缩放、 可访问性、"轻松访问"，显示"使文本更大"、 用户交互、 输入
ms.date: 08/02/2018
ms.topic: article
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 22ad7a1ac6160fd8b1cfb70c69f299c5d89192d3
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2019
ms.locfileid: "57600812"
---
# <a name="text-scaling"></a>文本缩放

![缩放 225%到 100%的文本的示例](images/coretext/text-scaling-news-hero-small.png)  
*在 Windows 10 （225%到 100%) 中缩放的文本的示例*

## <a name="overview"></a>概述

对许多人读取文本 （从与桌面监视到 Surface Hub 的巨大屏幕的便携式计算机的移动设备） 的计算机屏幕上会很困难。 相反，某些用户认为应用和网站中使用变得过大的字体大小。

若要确保文本一样清晰尽可能最广泛范围的用户，Windows 提供用户跨操作系统和单个应用程序更改相对字体大小的功能。 而不是使用放大镜应用程序 （这通常只需将放大屏幕的区域内的所有内容，并引入了其自己的可用性问题）、 更改显示分辨率或依赖于 DPI 缩放 （用以调整大小基于显示和典型查看的所有内容距离），用户可以快速访问设置，以调整只是文本，范围为 100%（默认大小） 的大小高达 225%。

## <a name="support"></a>支持

通用 Windows 应用程序 (这两个标准和 PWA)，支持默认情况下缩放的文本。

如果在 UWP 应用程序包含自定义控件、 自定义文本图面、 硬编码的控件的高度，较旧框架或第三方框架，您可能必须进行一些更新以确保为你的用户获得一致且有用的体验。  

DirectWrite、 GDI 和 XAML SwapChainPanels 本质上不支持文本缩放，而 Win32 支持限于菜单、 图标和工具栏。  

<!-- If you want to support text scaling in your application with these frameworks, you’ll need to support the text scaling change event outlined below and provide alternative sizes for your UI and content.   -->

## <a name="user-experience"></a>用户体验

用户可以调整文本规模更大滑块上的设置-> 使文本与轻松访问-> 视觉/显示屏幕。

![缩放 225%到 100%的文本的示例](images/coretext/text-scaling-settings-100-small.png)  
*文本缩放设置中设置-> 轻松访问-> 视觉/显示屏幕*

## <a name="ux-guidance"></a>UX 指南

文本调整大小时，必须也重设大小控件和容器并将其重新排列以适应其新的布局和的文本。 如前所述，具体取决于应用程序、 框架和平台，其中的大部分工作是为您完成。 以下的用户体验指南介绍了这种情况下，不是。

### <a name="use-the-platform-controls"></a>使用平台控件

我们说过这已？ 仍有必要重复：如果可能，请始终使用具有不同的 Windows 应用程序框架提供的内置控件来获得最少的精力可能最全面的用户体验。

例如，所有 UWP 文本控件都支持缩放体验，而无任何自定义或模板化的完整文本。

下面是包含几个标准文本控件的基本 UWP 应用的代码段：

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
*经过动画处理的文本扩展*

### <a name="use-auto-sizing"></a>使用自动调整大小

未指定控件的绝对的大小。 只要有可能，使控件基于用户和设备设置自动调整大小的平台。  

在此代码段中上一示例中，我们将使用`Auto`和`*`网格列，并让该平台的一组的宽度值调整应用布局基于在网格内包含的元素的大小。

``` xaml
<Grid.ColumnDefinitions>
    <ColumnDefinition Width="Auto"/>
    <ColumnDefinition Width="*"/>
    <ColumnDefinition Width="Auto"/>
</Grid.ColumnDefinitions>
```

### <a name="use-text-wrapping"></a>使用文本换行

若要确保您的应用程序的布局，灵活性和适应性尽可能，使文本换行中包含的文本 （许多控件不支持文本换行默认情况下） 任何控件。

如果未指定文本换行，该平台使用其他方法来调整布局，包括剪辑 （请参阅前面的示例）。

在这里，我们使用`AcceptsReturn`和`TextWrapping`文本框属性以确保我们的布局是尽可能灵活。

``` xaml
<TextBox PlaceholderText="Type something here" 
          AcceptsReturn="True" TextWrapping="Wrap" />
```

![经过动画处理的文本与文本换行缩放 225%到 100%](images/coretext/text-scaling-textwrap.gif)  
*使用文本换行进行缩放的动画的文本*

### <a name="specify-text-trimming-behavior"></a>指定文本修整行为

如果文本换行不是首选的行为，大多数文本控件将让您剪切文本或指定的文本剪裁行为的省略号。 剪辑优于省略号如省略号将会占用空间本身。

> [!NOTE]
> 如果你需要剪辑文本，剪辑字符串，不是开头的末尾。

在此示例中，我们演示如何在 TextBlock 中使用的文本剪裁[TextTrimming](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.textblock.texttrimming)属性。

``` xaml
<TextBlock TextTrimming="Clip">
    The quick brown fox jumped over the lazy dog.
</TextBlock>
```

![缩放 100%至 225%，文本剪切的文本](images/coretext/text-scaling-clipping-small.png)  
*使用文本进行缩放的文本*

### <a name="use-a-tooltip"></a>使用工具提示

如果剪辑文本，使用工具提示为用户提供的完整文本。

在这里，我们将工具提示添加到不支持文本换行的 TextBlock:

``` xaml
<TextBlock TextTrimming="Clip">
    <ToolTipService.ToolTip>
        <ToolTip Content="The quick brown fox jumped over the lazy dog."/>
    </ToolTipService.ToolTip>
    The quick brown fox jumped over the lazy dog.
</TextBlock>
```

### <a name="dont-scale-font-based-icons-or-symbols"></a>不能扩展基于字体的图标或符号

当使用基于字体的图标强调或修饰，禁用对这些字符缩放功能。

设置[IsTextScaleFactorEnabled](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.control.istextscalefactorenabled)属性设置为`false`对于大多数 XAML 控制。

### <a name="support-text-scaling-natively"></a>支持文本以本机方式缩放

处理[TextScaleFactorChanged](https://docs.microsoft.com/uwp/api/windows.ui.viewmanagement.uisettings.textscalefactorchanged) UISettings 在自定义框架和控件中的系统事件。 用户将文本缩放系数设置他们的系统每次都会引发此事件。

## <a name="summary"></a>摘要

本主题概述了缩放支持在 Windows 中的文本，并包括有关如何自定义用户体验的用户体验和开发人员指南。

## <a name="related-articles"></a>相关文章

### <a name="api-reference"></a>API 参考

- [IsTextScaleFactorEnabled](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.control.istextscalefactorenabled)
- [TextScaleFactorChanged](https://docs.microsoft.com/uwp/api/windows.ui.viewmanagement.uisettings.textscalefactorchanged)
