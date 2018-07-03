---
author: serenaz
Description: Control that provides top-level app navigation with an automatically adapting, collapsible left navigation menu
title: 导航视图
ms.assetid: ''
label: Navigation view
template: detail.hbs
ms.author: sezhen
ms.date: 05/19/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10，uwp
pm-contact: vasriram
design-contact: kimsea
dev-contact: mitra
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: c7817bf7ff60a52ea48c988bdebd6d4d2eeacdb7
ms.sourcegitcommit: 618741673a26bd718962d4b8f859e632879f9d61
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/08/2018
ms.locfileid: "1992146"
---
# <a name="navigation-view"></a>导航视图

导航视图控件通过提供可折叠导航菜单让你的应用拥有顶级导航。 此控件实现导航窗格或汉堡包菜单、模式，并自动使其窗格的显示模式适应不同窗口大小。

> **重要 API**：[NavigationView 类](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview)、[NavigationViewItem 类](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationviewitem)、[NavigationViewDisplayMode 枚举](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationviewdisplaymode)

![NavigationView 示例](images/navview_wireframe.png)

## <a name="video-summary"></a>视频摘要

> [!VIDEO https://channel9.msdn.com/Events/Windows/Windows-Developer-Day-Fall-Creators-Update/WinDev010/player]

## <a name="is-this-the-right-control"></a>这是正确的控件吗？

NavigationView 非常适用于：

-  许多相似类型的高级导航项目。 （例如，具有“橄榄球”、“棒球”、“篮球”、“足球”等类别的体育应用。）
-  中等数量到大量 (5-10) 的顶级导航类别。
-  提供一致的导航体验。 导航窗格应仅包括导航元素，而非操作。
-  保留较小窗口的屏幕空间。

NavigationView 只是你可以使用的几个导航元素之一。 要了解有关其他导航模式和元素的更多信息，请参阅[导航设计基础知识](../basics/navigation-basics.md)。

NavigationView 控件有很多实现简单导航窗格模式的内置行为。 如果你的导航需要 NavigationView 不支持的更复杂行为，可以考虑改用[大纲/细节](master-details.md)模式。

## <a name="examples"></a>示例
<table>
<th align="left">XAML 控件库<th>
<tr>
<td><img src="images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>如果已安装 <strong style="font-weight: semi-bold">XAML 控件库</strong>应用，请单击此处<a href="xamlcontrolsgallery:/item/NavigationView">打开此应用，了解 NavigationView 的实际应用</a>。</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">获取 XAML 控件库应用 (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics">获取源代码 (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="navigationview-sections"></a>NavigationView 部分

![NavigationView 部分](images/navview_sections.png)

### <a name="pane"></a>窗格

内置的导航（“汉堡包”）按钮使用户可以打开和关闭窗格。 打开窗格后在较大的应用窗口中，你可以选择使用 [IsPaneToggleButtonVisible](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview.IsPaneToggleButtonVisible) 属性隐藏此按钮。 汉堡包旁的文本标签是 [PaneTitle](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview.PaneTitle) 属性。

内置的后退按钮显示在窗格左上角。 NavigationView 控件不会自动向后退堆栈中添加内容，但会向启用向后导航中添加，请参阅[向后导航](#backwards-navigation)部分。

NavigationView 窗格还可以包含：

- [NavigationViewItem](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationviewitem) 形式的导航项，用于导航到特定页
- [NavigationViewItemSeparator](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationviewitemseparator) 形式的分隔符，用于对导航项分组
- [NavigationViewItemHeader](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationviewitemheader) 形式的标题，用于标记项组
- 允许应用级别搜索的可选 [AutoSuggestBox](auto-suggest-box.md)
- [应用设置](../app-settings/app-settings-and-data.md)的可选入口点。 若要隐藏设置项，请使用 [IsSettingsVisible](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview.IsSettingsVisible) 属性
- 窗格页脚中的自由格式内容（添加到 [PaneFooter](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview.PaneFooter) 属性后）

#### <a name="visual-style"></a>视觉样式

NavigationView 项目支持选定、禁用、指针悬停、按下和聚焦这些视觉状态。

![NavigationView 项目状态：禁用、指针悬停、按下、聚焦](images/navview_item-states.png)

当满足硬件和软件要求时，NavigationView 会自动在其窗格中使用新的[亚克力材料](../style/acrylic.md)和[突出显示](../style/reveal.md)。

### <a name="header"></a>标题

标题区域与导航按钮纵向对齐，具有固定高度 52 px。 它的用途是保存所选导航类别的页面标题。 标题停靠在页面顶部，并作为内容区域的滚动剪辑点。

当 NavigationView 处于“最小化”模式时，标题必须可见。 你可以选择在其他模式下隐藏标题，这样可以增大窗口宽度。 若要执行此操作，请将 [AlwaysShowHeader](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview.AlwaysShowHeader) 属性设置为 **False**。

### <a name="content"></a>内容

内容区域是显示所选导航类别的大部分信息的位置。 

建议在 NavigationView 处于“最小”模式时对内容区域使用 12px 边距，其他情况使用 24px 边距。

## <a name="navigationview-display-modes"></a>NavigationView 显示模式
NavigationView 窗格可以打开或关闭，并且有三个显示模式选项：
-  **最小** 在根据需要显示和隐藏窗格时，只有汉堡包按钮保持固定。
-  **精简** 窗格始终显示为可以打开至全宽的细窄长条。
-  **展开** 窗格在内容旁边处于打开状态。 通过激活汉堡包按钮关闭后，窗格的宽度变为细窄长条。

默认情况下，系统将自动根据可用于控件的屏幕空间量选择最佳显示模式。 （你可以[覆盖](#overriding-the-default-adaptive-behavior)此设置。）

### <a name="minimal"></a>最小

![NavigationView 处于“最小”模式，显示已关闭和打开的窗格](images/navview_minimal.png)

-  关闭时，默认情况下该窗格将被隐藏，只有导航按钮可见。
-  提供可节省屏幕空间的按需导航。 非常适合用于手机和平板手机。
-  按下导航按钮可打开和关闭该窗格，将绘制为标题和内容之上的覆盖层。 内容不会重新排列。
-  打开时，该窗格是暂时的，可通过轻型消除手势（如进行选择、按下后退按钮或在窗格外点击）来关闭。
-  当窗格的覆盖层打开时，所选项目将可见。
-  满足要求时，打开窗格的背景为[应用中亚克力](../style/acrylic.md#acrylic-blend-types)。
-  默认情况下，当总宽度小于或等于 640px 时，NavigationView 处于“最小”模式。

### <a name="compact"></a>精简

![NavigationView 处于“精简”模式，显示已关闭和打开的窗格](images/navview_compact.png)

-  关闭时，仅显示图标和导航按钮的窗格的垂直条可见。
-  使用少量屏幕空间提供所选位置的一些指示。
-  此模式较适合中等屏幕，如平板电脑和 [10 英尺体验](../devices/designing-for-tv.md)。
-  按下导航按钮可打开和关闭该窗格，将绘制为标题和内容之上的覆盖层。 内容不会重新排列。
-  “标题”不是必需的，可以隐藏以便为内容提供更多垂直空间。
-  所选项目会显示一个可视指示器，以突出显示用户在导航树中的位置。
-  满足要求时，窗格的背景为[应用中亚克力](../style/acrylic.md#acrylic-blend-types)。
-  默认情况下，当总宽度在 641px 与 1007px 之间时，NavigationView 处于“精简”模式。

### <a name="expanded"></a>展开

![NavigationView 处于“展开”模式，显示打开的窗格](images/navview_expanded.png)

-  默认情况下，该窗格保持打开状态。 此模式更适合较大的屏幕。
-  窗格与标题和内容并排绘制，在可用空间内重新排列。
-  使用导航按钮关闭窗格后，窗格会显示为与标题和内容并排的细窄长条。
-  “标题”不是必需的，可以隐藏以便为内容提供更多垂直空间。
-  所选项目会显示一个可视指示器，以突出显示用户在导航树中的位置。
-  满足要求时，使用[背景亚克力](../style/acrylic.md#acrylic-blend-types)绘制窗格的背景。
-  默认情况下，当总宽度大于 1007px 时，NavigationView 处于“展开”模式。

### <a name="overriding-the-default-adaptive-behavior"></a>覆盖默认自适应行为

NavigationView 会根据可用屏幕空间大小自动更改其显示模式。

> [!NOTE]
> NavigationView 应该用作应用的根容器，因为此控件可跨越应用窗口的整个宽度和高度。
你可以使用 [CompactModeThresholdWidth](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview.CompactModeThresholdWidth) 和 [ExpandedModeThresholdWidth](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview.ExpandedModeThresholdWidth) 属性覆盖导航视图更改显示模式所依据的宽度。

请考虑以下情形，其中阐明了你可能要自定义显示模式行为的情况。

- **频繁导航** 如果你预计用户会在应用区域之间频繁导航，请考虑使窗格保留在狭窄窗口宽度的视图中。 包含“歌曲”/“相册”/“艺术家”导航区域的音乐应用可能会选择 280px 窗格宽度，当应用窗口宽于 560px 时，该应用将保持“展开”模式。
```xaml
<NavigationView OpenPaneLength="280" CompactModeThresholdWidth="560" ExpandedModeThresholdWidth="560"/>
```

- **少数导航** 如果你预计用户很少在应用区域之间导航，请考虑使窗格在较宽的窗口宽度隐藏。 即使在 1080p 显示器上最大化应用，具有多个布局的计算器应用也可能会选择保留“最小”模式。
```xaml
<NavigationView CompactModeThresholdWidth="1920" ExpandedModeThresholdWidth="1920"/>
```

- **图标消除歧义** 如果应用的导航区域不适用于有意义的图标，请勿使用“精简”模式。 具有“集合”/“相册”/“文件夹”导航区域的图像查看应用可以选择在狭窄和中等宽度以“最小”模式显示以及在较宽宽度以“展开”模式显示 NavigationView。
```xaml
<NavigationView CompactModeThresholdWidth="1008"/>
```

## <a name="interaction"></a>交互

当用户点击窗格中的导航项目时，NavigationView 会将该项目显示为选定项目，并引发 [ItemInvoked](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview.ItemInvoked) 事件。 如果点击导致新项被选中，则 NavigationView 还将引发 [SelectionChanged](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview.SelectionChanged) 事件。 

你的应用负责用相应信息更新“标题”和“内容”，以响应此用户交互。 此外，建议以编程方式将[焦点](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.controls.control.FocusState)从导航项目移动到内容。 通过在加载时设置初始焦点，可以简化用户流并最大程度地减少预期的键盘焦点移动数。

## <a name="backwards-navigation"></a>向后导航
NavigationView 具有内置的后退按钮，可以通过以下属性启用：
- [**IsBackButtonVisible**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview.IsBackButtonVisible) 是 NavigationViewBackButtonVisible 枚举，默认为“Auto”。 它用于显示/隐藏后退按钮。 当此按钮不可见时，绘制后退按钮的空间会折叠起来。
- [**IsBackEnabled**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview.IsBackEnabled) 默认为 false，可用于切换后退按钮的状态。
- [**BackRequested**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview.BackRequested) 在用户单击后退按钮时触发。
    - 在“最小”/“精简”模式下，NavigationView.Pane 以浮出控件的形式打开，单击后退按钮会关闭面板并引发 **PaneClosing** 事件。
    - 如果 IsBackEnabled 为 false，则不会引发。

![NavigationView 后退按钮](../basics/images/back-nav/NavView.png)

## <a name="code-example"></a>代码示例

下面是演示如何将 NavigationView 合并到应用中的简单示例。 

我们将演示如何使用 NavigationView 的后退按钮实现向后导航。 请注意，要使用 NavigationView 的向后导航属性，你需要 [Windows 10 Insider Preview（v10.0.17110.0 引入）](https://www.microsoft.com/en-us/software-download/windowsinsiderpreviewSDK)。

我们还将演示使用 `x:Uid` 的导航项目内容字符串本地化。 有关本地化的更多信息，请参阅[本地化 UI 中的字符串](../../app-resources/localize-strings-ui-manifest.md)。

```xaml
<Page
    x:Class="NavigationViewSample.MainPage"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:local="using:NavigationViewSample"
    xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
    xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
    mc:Ignorable="d">

    <NavigationView x:Name="NavView"
                    ItemInvoked="NavView_ItemInvoked"
                    Loaded="NavView_Loaded"
                    BackRequested="NavView_BackRequested">

        <NavigationView.MenuItems>
            <NavigationViewItem x:Uid="HomeNavItem" Content="Home" Tag="home">
                <NavigationViewItem.Icon>
                    <FontIcon Glyph="&#xE10F;"/>
                </NavigationViewItem.Icon>
            </NavigationViewItem>
            <NavigationViewItemSeparator/>
            <NavigationViewItemHeader Content="Main pages"/>
            <NavigationViewItem x:Uid="AppsNavItem" Icon="AllApps" Content="Apps" Tag="apps"/>
            <NavigationViewItem x:Uid="GamesNavItem" Icon="Video" Content="Games" Tag="games"/>
            <NavigationViewItem x:Uid="MusicNavItem" Icon="Audio" Content="Music" Tag="music"/>
        </NavigationView.MenuItems>

        <NavigationView.AutoSuggestBox>
            <AutoSuggestBox x:Name="ASB" QueryIcon="Find"/>
        </NavigationView.AutoSuggestBox>

        <NavigationView.HeaderTemplate>
            <DataTemplate>
                <Grid Margin="24,10,0,0">
                    <Grid.ColumnDefinitions>
                        <ColumnDefinition Width="Auto"/>
                        <ColumnDefinition/>
                    </Grid.ColumnDefinitions>
                    <TextBlock Style="{StaticResource TitleTextBlockStyle}"
                           FontSize="28"
                           VerticalAlignment="Center"
                           Text="Welcome"/>
                    <CommandBar Grid.Column="1"
                            HorizontalAlignment="Right"
                            VerticalAlignment="Top"
                            DefaultLabelPosition="Right"
                            Background="{ThemeResource SystemControlBackgroundAltHighBrush}">
                        <AppBarButton Label="Refresh" Icon="Refresh"/>
                        <AppBarButton Label="Import" Icon="Import"/>
                    </CommandBar>
                </Grid>
            </DataTemplate>
        </NavigationView.HeaderTemplate>

        <NavigationView.PaneFooter>
            <HyperlinkButton x:Name="MoreInfoBtn"
                             Content="More info"
                             Click="More_Click"
                             Margin="12,0"/>
        </NavigationView.PaneFooter>

        <Frame x:Name="ContentFrame" Margin="24">
            <Frame.ContentTransitions>
                <TransitionCollection>
                    <NavigationThemeTransition/>
                </TransitionCollection>
            </Frame.ContentTransitions>
        </Frame>

    </NavigationView>
</Page>
```

```csharp
private void NavView_Loaded(object sender, RoutedEventArgs e)
{
    // you can also add items in code behind
    NavView.MenuItems.Add(new NavigationViewItemSeparator());
    NavView.MenuItems.Add(new NavigationViewItem()
    { Content = "My content", Icon = new SymbolIcon(Symbol.Folder), Tag = "content" });

    // set the initial SelectedItem 
    foreach (NavigationViewItemBase item in NavView.MenuItems)
    {
        if (item is NavigationViewItem && item.Tag.ToString() == "home")
        {
            NavView.SelectedItem = item;
            break;
        }
    }
            
    ContentFrame.Navigated += On_Navigated;

    // add keyboard accelerators for backwards navigation
    KeyboardAccelerator GoBack = new KeyboardAccelerator();
    GoBack.Key = VirtualKey.GoBack;
    GoBack.Invoked += BackInvoked;
    KeyboardAccelerator AltLeft = new KeyboardAccelerator();
    AltLeft.Key = VirtualKey.Left;
    AltLeft.Invoked += BackInvoked;
    this.KeyboardAccelerators.Add(GoBack);
    this.KeyboardAccelerators.Add(AltLeft);
    // ALT routes here
    AltLeft.Modifiers = VirtualKeyModifiers.Menu;
    
}

private void NavView_ItemInvoked(NavigationView sender, NavigationViewItemInvokedEventArgs args)
{  
    if (args.IsSettingsInvoked)
    {
        ContentFrame.Navigate(typeof(SettingsPage));
    }
    else
    {
        // find NavigationViewItem with Content that equals InvokedItem
        var item = sender.MenuItems.OfType<NavigationViewItem>().First(x => (string)x.Content == (string)args.InvokedItem);
        NavView_Navigate(item as NavigationViewItem);
    }
}

private void NavView_Navigate(NavigationViewItem item)
{
    switch (item.Tag)
    {
        case "home":
            ContentFrame.Navigate(typeof(HomePage));
            break;

        case "apps":
            ContentFrame.Navigate(typeof(AppsPage));
            break;

        case "games":
            ContentFrame.Navigate(typeof(GamesPage));
            break;

        case "music":
            ContentFrame.Navigate(typeof(MusicPage));
            break;

        case "content":
            ContentFrame.Navigate(typeof(MyContentPage));
            break;
    }           
}

private void NavView_BackRequested(NavigationView sender, NavigationViewBackRequestedEventArgs args)
{
    On_BackRequested();
}

private void BackInvoked(KeyboardAccelerator sender, KeyboardAcceleratorInvokedEventArgs args)
{
    On_BackRequested();
    args.Handled = true;
}

private bool On_BackRequested()
{
    bool navigated = false;

    // don't go back if the nav pane is overlayed
    if (NavView.IsPaneOpen && (NavView.DisplayMode == NavigationViewDisplayMode.Compact || NavView.DisplayMode == NavigationViewDisplayMode.Minimal))
    {
        return false;
    }
    else
    {
        if (ContentFrame.CanGoBack)
        {
            ContentFrame.GoBack();
            navigated = true;
        }
    }
    return navigated;
}

private void On_Navigated(object sender, NavigationEventArgs e)
{
    NavView.IsBackEnabled = ContentFrame.CanGoBack;

    if (ContentFrame.SourcePageType == typeof(SettingsPage))
    {
        NavView.SelectedItem = NavView.SettingsItem as NavigationViewItem;
    }
    else 
    {
        Dictionary<Type, string> lookup = new Dictionary<Type, string>()
        {
            {typeof(HomePage), "home"},
            {typeof(AppsPage), "apps"},
            {typeof(GamesPage), "games"},
            {typeof(MusicPage), "music"},
            {typeof(MyContentPage), "content"}    
        };

        String stringTag = lookup[ContentFrame.SourcePageType];

        // set the new SelectedItem  
        foreach (NavigationViewItemBase item in NavView.MenuItems)
        {
            if (item is NavigationViewItem && item.Tag.Equals(stringTag))
            {
                item.IsSelected = true;
                break;
            }
        }        
    }
}
```

## <a name="customizing-backgrounds"></a>自定义背景

要更改 NavigationView 主区域的背景，请将其 `Background` 属性设置为你的首选画笔。

当 NavigationView 处于最小或精简模式且背景亚克力处于扩展模式时，窗格的背景将显示应用内亚克力。 要更新此行为或自定义窗格的亚克力外观，请修改两个主题资源，方法是在 App.xaml 中覆盖它们。

```xaml
<Application.Resources>
    <ResourceDictionary>
        <AcrylicBrush x:Key="NavigationViewDefaultPaneBackground"
        BackgroundSource="Backdrop" TintColor="Yellow" TintOpacity=".6"/>
        <AcrylicBrush x:Key="NavigationViewExpandedPaneBackground"
        BackgroundSource="HostBackdrop" TintColor="Orange" TintOpacity=".8"/>
    </ResourceDictionary>
</Application.Resources>
```

## <a name="extending-your-app-into-the-title-bar"></a>将应用扩展到标题栏

为使应用窗口外观完整流畅，建议将 NavigationView 及其亚克力窗格向上扩展到应用的标题栏区域中。 这可以避免标题栏、纯色的 NavigationView 内容以及 NavigationView 窗格的亚克力外观组成毫无视觉吸引力的形状。

为此，请将以下代码添加到 App.xaml.cs 中。

```csharp
//draw into the title bar
var coreTitleBar = CoreApplication.GetCurrentView().TitleBar;
coreTitleBar.ExtendViewIntoTitleBar = true;

//remove the solid-colored backgrounds behind the caption controls and system back button
var viewTitleBar = ApplicationView.GetForCurrentView().TitleBar;
viewTitleBar.ButtonBackgroundColor = Colors.Transparent;
viewTitleBar.ButtonInactiveBackgroundColor = Colors.Transparent;
viewTitleBar.ButtonForegroundColor = (Color)Resources["SystemBaseHighColor"];
```

绘制到标题栏中会有隐藏应用标题的副作用。 为帮助用户，可通过添加自己的 TextBlock 还原标题。 向包含你的 NavigationView 的根页面中添加以下标注。

```xaml
<Grid>
    <TextBlock x:Name="AppTitle"
        xmlns:appmodel="using:Windows.ApplicationModel"
        Text="{x:Bind appmodel:Package.Current.DisplayName}"
        Style="{StaticResource CaptionTextBlockStyle}"
        IsHitTestVisible="False"
        Canvas.ZIndex="1"/>
    

    <NavigationView Canvas.ZIndex="0" ... />

</Grid>
```

你还需要根据后退按钮是否可见来调整 AppTitle 边距。 当应用处于 FullScreenMode 时，你需要删除后退箭头的空间，即使 TitleBar 为它保留了空间。

```csharp
var coreTitleBar = CoreApplication.GetCurrentView().TitleBar;
Window.Current.SetTitleBar(AppTitle);
coreTitleBar.ExtendViewIntoTitleBar = true;

void UpdateAppTitle()
{
    var full = (ApplicationView.GetForCurrentView().IsFullScreenMode);
    var left = 12 + (full ? 0 : CoreApplication.GetCurrentView().TitleBar.SystemOverlayLeftInset);
    AppTitle.Margin = new Thickness(left, 8, 0, 0);
}

Window.Current.CoreWindow.SizeChanged += (s, e) => UpdateAppTitle();
coreTitleBar.LayoutMetricsChanged += (s, e) => UpdateAppTitle();
```

有关自定义标题栏的更多信息，请参阅[标题栏自定义](../shell/title-bar.md)。

## <a name="related-topics"></a>相关主题

- [NavigationView 类](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview)
- [大纲/细节](master-details.md)
- [Pivot 控件](tabs-pivot.md)
- [导航基础知识](../basics/navigation-basics.md)
- [UWP 的 Fluent Design 概述](../fluent-design-system/index.md)

