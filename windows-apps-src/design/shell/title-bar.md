---
author: jwmsft
description: 自定义桌面应用的标题栏以匹配应用的个性。
title: 标题栏自定义
template: detail.hbs
ms.author: jimwalk
ms.date: 10/10/2017
ms.topic: article
keywords: windows 10, uwp, 标题栏
doc-status: Draft
ms.localizationpriority: medium
ms.openlocfilehash: 2ebe590f98afef031ab183589fc7dcfc29cd9493
ms.sourcegitcommit: e2fca6c79f31e521ba76f7ecf343cf8f278e6a15
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/15/2018
ms.locfileid: "6975028"
---
# <a name="title-bar-customization"></a>标题栏自定义



当你的应用在桌面窗口中运行时，你可以自定义标题栏以匹配应用的个性。 利用标题栏自定义 API，你可以为标题栏元素指定颜色，或者将应用内容扩展到标题栏区域并完全控制该内容。

> **重要 API**：[ApplicationView.TitleBar 属性](https://docs.microsoft.com/uwp/api/windows.ui.viewmanagement.applicationview)、[ApplicationViewTitleBar 类](https://docs.microsoft.com/uwp/api/windows.ui.viewmanagement.applicationviewtitlebar)、[CoreApplicationViewTitleBar 类](https://docs.microsoft.com/uwp/api/windows.applicationmodel.core.coreapplicationviewtitlebar)

## <a name="how-much-to-customize-the-title-bar"></a>标题栏的自定义程度

你可以对标题栏应用两个自定义级别。

对于简单的颜色自定义，你可以设置 [ApplicationViewTitleBar](https://docs.microsoft.com/uwp/api/windows.ui.viewmanagement.applicationviewtitlebar) 属性，以指定要用于标题栏元素的颜色。 在此情况下，系统仍对标题栏的所有其他方面负责，比如绘制应用标题和定义可拖动区域。

你的其他选项是隐藏默认标题栏并将其替换为自己的 XAML 内容。 例如，你可以在标题栏区域中放置文本、按钮或自定义的菜单。 你将还需要使用此选项将[亚克力](../style/acrylic.md)背景扩展到标题栏区域中。

当你选择完全自定义时，你负责将内容放在标题栏区域中，并且可以定义自己的可拖动区域。 系统的“后退”、“关闭”、“最小化”和“最大化”按钮仍然可用并由系统处理，但是应用标题等元素则不可用。 你将需要根据应用的需要自行创建这些元素。

> [!NOTE]
> 可以使用 XAML、DirectX 和 HTML 对 UWP 应用进行简单的颜色自定义。 只能使用 XAML 对 UWP 应用进行完全自定义。

## <a name="simple-color-customization"></a>简单的颜色自定义

如果只想自定义标题栏的颜色而不做任何太奇特的操作（比如将选项卡放在标题栏中），则可以为应用窗口设置 [ApplicationViewTitleBar](https://docs.microsoft.com/uwp/api/windows.ui.viewmanagement.applicationviewtitlebar) 的颜色属性。

此示例显示了如何获取 ApplicationViewTitleBar 的实例以及如何设置其颜色属性。

```csharp
// using Windows.UI.ViewManagement;

var titleBar = ApplicationView.GetForCurrentView().TitleBar;

// Set active window colors
titleBar.ForegroundColor = Windows.UI.Colors.White;
titleBar.BackgroundColor = Windows.UI.Colors.Green;
titleBar.ButtonForegroundColor = Windows.UI.Colors.White;
titleBar.ButtonBackgroundColor = Windows.UI.Colors.SeaGreen;
titleBar.ButtonHoverForegroundColor = Windows.UI.Colors.White;
titleBar.ButtonHoverBackgroundColor = Windows.UI.Colors.DarkSeaGreen;
titleBar.ButtonPressedForegroundColor = Windows.UI.Colors.Gray;
titleBar.ButtonPressedBackgroundColor = Windows.UI.Colors.LightGreen;

// Set inactive window colors
titleBar.InactiveForegroundColor = Windows.UI.Colors.Gray;
titleBar.InactiveBackgroundColor = Windows.UI.Colors.SeaGreen;
titleBar.ButtonInactiveForegroundColor = Windows.UI.Colors.Gray;
titleBar.ButtonInactiveBackgroundColor = Windows.UI.Colors.SeaGreen;
```

> [!NOTE]
> 此代码可放在应用的 [OnLaunched](https://docs.microsoft.com/uwp/api/windows.ui.xaml.application.onlaunched) 方法 (_App.xaml.cs_) 中、对 [Window.Activate](https://docs.microsoft.com/uwp/api/windows.ui.xaml.window.Activate) 的调用的后面，或应用的第一页中。

> [!TIP]
> Windows 社区工具包提供的扩展可用于在 XAML 中设置这些颜色属性。 有关详细信息，请参阅 [Windows 社区工具包文档](https://docs.microsoft.com/windows/uwpcommunitytoolkit/extensions/viewextensions)。

设置标题栏颜色时需要注意以下几点：

- 按钮背景颜色不会应用于关闭按钮的悬停和按下状态。 关闭按钮始终对这些状态使用系统定义的颜色。
- 使用系统后退按钮时，按钮颜色属性会应用于该按钮。 （[请参阅“导航历史记录和向后导航”](../basics/navigation-history-and-backwards-navigation.md)。）
- 将颜色属性设置为 **null** 会将其重置为默认的系统颜色。
- 你无法设置透明色。 颜色的 alpha 通道会被忽略。

Windows 为用户提供了将选定的[主题色](https://docs.microsoft.com/windows/uwp/style/color#accent-color)应用于标题栏的选项。 如果你要设置任何标题栏颜色，那么我们建议你显式设置所有颜色。 这可以确保不存在因用户定义的颜色设置而出现的意外颜色组合。

## <a name="full-customization"></a>完全自定义

当你选择进行标题栏完全自定义时，应用的客户端区域会进行扩展以覆盖整个窗口，包括标题栏区域。 你负责对整个窗口（在应用画布顶部重叠的标题按钮除外）进行绘制和输入处理。

若要隐藏默认标题栏并将你的内容扩展到标题栏区域中，请将 [CoreApplicationViewTitleBar.ExtendViewIntoTitleBar](https://docs.microsoft.com/uwp/api/windows.applicationmodel.core.coreapplicationviewtitlebar) 属性设置为 **true**。

此示例介绍如何获取 CoreApplicationViewTitleBar 以及如何将 ExtendViewIntoTitleBar 属性设置为 **true**。 此操作可以在应用的 [OnLaunched](https://docs.microsoft.com/uwp/api/windows.ui.xaml.application.onlaunched) 方法 (_App.xaml.cs_) 中或在应用的第一页中完成。

```csharp
// using Windows.ApplicationModel.Core;

// Hide default title bar.
var coreTitleBar = CoreApplication.GetCurrentView().TitleBar;
coreTitleBar.ExtendViewIntoTitleBar = true;
```

> [!TIP]
> 当应用关闭并重启后，系统会保留此设置。 在 Visual Studio 中，如果你将 ExtendViewIntoTitleBar 设置为 **true**，然后要还原为默认值，则应将其显式设置为 **false** 并运行应用以覆盖保留的设置。

### <a name="draggable-regions"></a>可拖动区域

标题栏的可拖动区域定义了用户可以在哪里通过单击和拖动来移动窗口（而不是只在应用画布中拖动内容）。 通过调用 [Window.SetTitleBar](https://docs.microsoft.com/uwp/api/windows.ui.xaml.window.settitlebar) 方法并传入一个定义可拖动区域的 UIElement，你可以指定可拖动区域。 （UIElement 通常是一个包含其他元素的面板。）

下面介绍如何将内容网格设置为可拖动的标题栏区域。 此代码会写入应用第一页的 XAML 和代码隐藏部分中。 请参阅[完全自定义示例](./title-bar.md#full-customization-example)部分以获取完整代码。

```xaml
<Grid x:Name="AppTitleBar" Background="Transparent">
    <!-- Width of the padding columns is set in LayoutMetricsChanged handler. -->
    <!-- Using padding columns instead of Margin ensures that the background
         paints the area under the caption control buttons (for transparent buttons). -->
    <Grid.ColumnDefinitions>
        <ColumnDefinition x:Name="LeftPaddingColumn" Width="0"/>
        <ColumnDefinition/>
        <ColumnDefinition x:Name="RightPaddingColumn" Width="0"/>
    </Grid.ColumnDefinitions>
    <Image Source="Assets/Square44x44Logo.png" 
           Grid.Column="1" HorizontalAlignment="Left" 
           Width="20" Height="20" Margin="12,0"/>
    <TextBlock Text="Custom Title Bar" 
               Grid.Column="1" 
               Style="{StaticResource CaptionTextBlockStyle}" 
               Margin="44,8,0,0"/>
</Grid>
```

```csharp
public MainPage()
{
    this.InitializeComponent();

    var coreTitleBar = CoreApplication.GetCurrentView().TitleBar;
    coreTitleBar.ExtendViewIntoTitleBar = true;

    // Set XAML element as a draggable region.
    AppTitleBar.Height = coreTitleBar.Height;
    Window.Current.SetTitleBar(AppTitleBar);
}
```

UIElement (`AppTitleBar`) 是应用的 XAML 的一部分。 你可以在不变的根页面中声明和设置标题栏，也可以在应用可以导航到的每个页面中声明和设置标题栏区域。 如果你在每个页面中对其进行设置，则应该确保当用户在应用中导航时可拖动区域保持一致。

你可以调用 SetTitleBar 以在应用运行时切换到新的标题栏元素。 你还可以将 **null** 作为参数传递给 SetTitleBar 以还原为默认的拖动行为。 （有关详细信息，请参阅“默认的可拖动区域”。）

> [!IMPORTANT]
> 你指定的可拖动区域必须可执行点击测试，这意味着对于某些元素，你可能需要设置透明背景画笔。 有关详细信息，请参阅关于 [VisualTreeHelper.FindElementsInHostCoordinates](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.visualtreehelper.findelementsinhostcoordinates) 的备注。
>
>例如，如果将网格定义为可拖动区域，请设置 `Background="Transparent"` 以使其可拖动。
>
>此网格不可拖动（但可拖动其中的可见元素）：`<Grid x:Name="AppTitleBar">`。
>
>此网格看上去相同，但是整个网格可以拖动：`<Grid x:Name="AppTitleBar" Background="Transparent">`。

#### <a name="default-draggable-region"></a>默认的可拖动区域

如果你未指定可拖动区域，则会将宽度为窗口宽度、高度为标题按钮高度且沿窗口顶边放置的矩形设置为默认的可拖动区域。

如果你定义了可拖动区域，则系统会将默认的可拖动区域缩小为标题按钮大小的小区域，此区域位于标题按钮的左侧（如果标题按钮在窗口左侧，则位于标题按钮的右侧）。 这可以确保始终存在一个用户可以拖动的一致区域。

### <a name="system-caption-buttons"></a>系统标题按钮

系统会为系统标题按钮（“后退”、“最小化”、“最大化”、“关闭”）保留应用窗口的左上角或右上角。 系统会保留对标题控制区域的控制，以保证提供最少功能来拖动、最小化、最大化和关闭窗口。 系统在右上方为从左到右的语言绘制“关闭”按钮，在左上方为从右到左的语言绘制“关闭”按钮。

标题控制区域的尺寸和位置由 CoreApplicationViewTitleBar 类进行传递，以便你可以在标题栏 UI 布局中将其考虑在内。 每一侧上的保留区域的宽度由 [SystemOverlayLeftInset](https://docs.microsoft.com/uwp/api/windows.applicationmodel.core.coreapplicationviewtitlebar.SystemOverlayLeftInset) 或 [SystemOverlayRightInset](https://docs.microsoft.com/uwp/api/windows.applicationmodel.core.coreapplicationviewtitlebar.SystemOverlayRightInset) 属性提供，其高度由 [Height](https://docs.microsoft.com/uwp/api/windows.applicationmodel.core.coreapplicationviewtitlebar.Height) 属性提供。

你可以在这些属性所定义的标题控制区域下方绘制内容，如应用背景，但不应放置你希望用户能够与之交互的任何 UI。 它不会接收任何输入，因为标题控件的输入由系统进行处理。

你可以处理 [LayoutMetricsChanged](https://docs.microsoft.com/uwp/api/windows.applicationmodel.core.coreapplicationviewtitlebar.LayoutMetricsChanged) 事件，以响应标题按钮的大小变化。 例如，显示或隐藏系统后退按钮时，可能会发生这种情况。 处理此事件可验证并更新取决于标题栏大小的 UI 元素的定位。

此示例介绍如何调整标题栏的布局，以考虑系统后退按钮的显示或隐藏等变化。 `AppTitleBar`、`LeftPaddingColumn` 和 `RightPaddingColumn` 在之前显示的 XAML 中已声明。

```csharp
private void CoreTitleBar_LayoutMetricsChanged(CoreApplicationViewTitleBar sender, object args)
{
    UpdateTitleBarLayout(sender);
}

private void UpdateTitleBarLayout(CoreApplicationViewTitleBar coreTitleBar)
{
    // Get the size of the caption controls area and back button 
    // (returned in logical pixels), and move your content around as necessary.
    LeftPaddingColumn.Width = new GridLength(coreTitleBar.SystemOverlayLeftInset);
    RightPaddingColumn.Width = new GridLength(coreTitleBar.SystemOverlayRightInset);
    TitleBarButton.Margin = new Thickness(0,0,coreTitleBar.SystemOverlayRightInset,0);

    // Update title bar control size as needed to account for system size changes.
    AppTitleBar.Height = coreTitleBar.Height;
}
```

### <a name="interactive-content"></a>交互式内容

你可以在应用顶部放置交互式控件，如按钮、菜单或搜索框，以使它们显示在标题栏中。 但是，有几条规则必须遵循，以确保交互式元素接收用户输入。
- 你必须调用 SetTitleBar 以将一个区域定义为可拖动的标题栏区域。 如果不这样做，系统会在页面顶部设置默认的可拖动区域。 然后，系统将处理此区域的所有用户输入，并阻止输入到达你的控件。
- 将交互式控件放在由 SetTitleBar（具有更高的 z 顺序）调用定义的可拖动区域的顶部。 不要将交互式控件设为传递到 SetTitleBar 的 UIElement 的子项。 将元素传递到 SetTitleBar 后，系统会将其视为系统标题栏，并处理该元素的所有指针输入。

在此，`TitleBarButton` 元素比 `AppTitleBar` 具有更高的 z 顺序，因此它将收到用户输入。

```xaml
<Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
    <Grid.RowDefinitions>
        <RowDefinition Height="Auto"/>
        <RowDefinition />
    </Grid.RowDefinitions>

    <Grid x:Name="AppTitleBar" Background="Transparent">
        <!-- Width of the padding columns is set in LayoutMetricsChanged handler. -->
        <!-- Using padding columns instead of Margin ensures that the background
             paints the area under the caption control buttons (for transparent buttons). -->
        <Grid.ColumnDefinitions>
            <ColumnDefinition x:Name="LeftPaddingColumn" Width="0"/>
            <ColumnDefinition/>
            <ColumnDefinition x:Name="RightPaddingColumn" Width="0"/>
        </Grid.ColumnDefinitions>
        <Image Source="Assets/Square44x44Logo.png"
               Grid.Column="1" HorizontalAlignment="Left"
               Width="20" Height="20" Margin="12,0"/>
        <TextBlock Text="Custom Title Bar"
                   Grid.Column="1"
                   Style="{StaticResource CaptionTextBlockStyle}"
                   Margin="44,8,0,0"/>
    </Grid>

    <!-- This Button has a higher z-order than AppTitleBar, 
         so it receives user input. -->
    <Button x:Name="TitleBarButton" Content="Button in the title bar"
        HorizontalAlignment="Right"/>
</Grid>
```

### <a name="transparency-in-caption-buttons"></a>标题按钮中的透明度

如果将 ExtendViewIntoTitleBar 设置为 **true**，则可以使标题按钮的背景变为透明，以使应用背景可隐约显示。 为了实现完全透明，通常可以将背景设置为 [Colors.Transparent](https://docs.microsoft.com/uwp/api/windows.ui.colors.Transparent)。 为了实现部分透明，请将属性设置为 [Color](https://docs.microsoft.com/uwp/api/windows.ui.color) 并为其设置 alpha 通道。

以下 ApplicationViewTitleBar 属性可以是透明的：

- ButtonBackgroundColor
- ButtonHoverBackgroundColor
- ButtonPressedBackgroundColor
- ButtonInactiveBackgroundColor

按钮背景颜色不会应用于关闭按钮的悬停和按下状态。 关闭按钮始终对这些状态使用系统定义的颜色。

所有其他颜色属性都将继续忽略 alpha 通道。 如果将 ExtendViewIntoTitleBar 设置为 **false**，则始终会对所有 ApplicationViewTitleBar 颜色属性忽略 alpha 通道。

### <a name="full-screen-and-tablet-mode"></a>全屏和平板模式

当应用在_全屏_或_平板模式_下运行时，系统将隐藏标题栏和标题控制按钮。 但是，用户可以调用标题栏，以使其以覆盖形式显示在应用的 UI 顶部。
你可以处理隐藏或调用标题栏时将通知的 [CoreApplicationViewTitleBar.IsVisibleChanged](https://docs.microsoft.com/uwp/api/windows.applicationmodel.core.coreapplicationviewtitlebar.IsVisibleChanged) 事件，并根据需要显示或隐藏你的自定义标题栏内容。

此示例介绍如何处理 IsVisibleChanged 以显示和隐藏之前显示的 `AppTitleBar` 元素。

```csharp
public MainPage()
{
    this.InitializeComponent();

    var coreTitleBar = CoreApplication.GetCurrentView().TitleBar;

    // Register a handler for when the title bar visibility changes.
    // For example, when the title bar is invoked in full screen mode.
    coreTitleBar.IsVisibleChanged += CoreTitleBar_IsVisibleChanged;
}

private void CoreTitleBar_IsVisibleChanged(CoreApplicationViewTitleBar sender, object args)
{
    if (sender.IsVisible)
    {
        AppTitleBar.Visibility = Visibility.Visible;
    }
    else
    {
        AppTitleBar.Visibility = Visibility.Collapsed;
    }
}
```

>[!NOTE]
>仅在应用支持时才能进入_全屏_模式。 有关详细信息，请参阅 [ApplicationView.IsFullScreenMode](https://docs.microsoft.com/uwp/api/windows.ui.viewmanagement.applicationview.IsFullScreenMode)。 [_平板模式_](https://support.microsoft.com/help/17210/windows-10-use-your-pc-like-a-tablet)是受支持的硬件上的用户选项，以便用户可以选择在平板模式下运行任何应用。

## <a name="full-customization-example"></a>完全自定义示例

```xaml
<Page
    x:Class="CustomTitleBar.MainPage"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:local="using:CustomTitleBar"
    xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
    xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
    mc:Ignorable="d">

    <Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
        <Grid.RowDefinitions>
            <RowDefinition Height="Auto"/>
            <RowDefinition />
        </Grid.RowDefinitions>

        <Grid x:Name="AppTitleBar" Background="Transparent">
            <!-- Width of the padding columns is set in LayoutMetricsChanged handler. -->
            <!-- Using padding columns instead of Margin ensures that the background
                 paints the area under the caption control buttons (for transparent buttons). -->
            <Grid.ColumnDefinitions>
                <ColumnDefinition x:Name="LeftPaddingColumn" Width="0"/>
                <ColumnDefinition/>
                <ColumnDefinition x:Name="RightPaddingColumn" Width="0"/>
            </Grid.ColumnDefinitions>
            <Image Source="Assets/Square44x44Logo.png" 
                   Grid.Column="1" HorizontalAlignment="Left" 
                   Width="20" Height="20" Margin="12,0"/>
            <TextBlock Text="Custom Title Bar" 
                       Grid.Column="1" 
                       Style="{StaticResource CaptionTextBlockStyle}" 
                       Margin="44,8,0,0"/>
        </Grid>

        <!-- This Button has a higher z-order than MyTitleBar, 
             so it receives user input. -->
        <Button x:Name="TitleBarButton" Content="Button in the title bar"
                HorizontalAlignment="Right"/>
    </Grid>
</Page>
```

```csharp
public MainPage()
{
    this.InitializeComponent();

    // Hide default title bar.
    var coreTitleBar = CoreApplication.GetCurrentView().TitleBar;
    coreTitleBar.ExtendViewIntoTitleBar = true;
    UpdateTitleBarLayout(coreTitleBar);

    // Set XAML element as a draggable region.
    Window.Current.SetTitleBar(AppTitleBar);

    // Register a handler for when the size of the overlaid caption control changes.
    // For example, when the app moves to a screen with a different DPI.
    coreTitleBar.LayoutMetricsChanged += CoreTitleBar_LayoutMetricsChanged;

    // Register a handler for when the title bar visibility changes.
    // For example, when the title bar is invoked in full screen mode.
    coreTitleBar.IsVisibleChanged += CoreTitleBar_IsVisibleChanged;
}

private void CoreTitleBar_LayoutMetricsChanged(CoreApplicationViewTitleBar sender, object args)
{
    UpdateTitleBarLayout(sender);
}

private void UpdateTitleBarLayout(CoreApplicationViewTitleBar coreTitleBar)
{
    // Get the size of the caption controls area and back button 
    // (returned in logical pixels), and move your content around as necessary.
    LeftPaddingColumn.Width = new GridLength(coreTitleBar.SystemOverlayLeftInset);
    RightPaddingColumn.Width = new GridLength(coreTitleBar.SystemOverlayRightInset);
    TitleBarButton.Margin = new Thickness(0,0,coreTitleBar.SystemOverlayRightInset,0);

    // Update title bar control size as needed to account for system size changes.
    AppTitleBar.Height = coreTitleBar.Height;
}

private void CoreTitleBar_IsVisibleChanged(CoreApplicationViewTitleBar sender, object args)
{
    if (sender.IsVisible)
    {
        AppTitleBar.Visibility = Visibility.Visible;
    }
    else
    {
        AppTitleBar.Visibility = Visibility.Collapsed;
    }
}
```

## <a name="dos-and-donts"></a>应做事项和禁止事项

- 当你的窗口处于活动或非活动状态时，应做事项显而易见。 至少要更改标题栏中文本、图标和按钮的颜色。
- 务必定义一个沿着应用画布上边缘布局的可拖动区域。 通过匹配系统标题栏的位置，用户可以更容易找到。
- 务必定义一个可拖动区域，以匹配应用画布上的可视标题栏（如果有）。

## <a name="related-articles"></a>相关文章

- [亚克力](../style/acrylic.md)
- [颜色](../style/color.md)
