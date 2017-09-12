---
author: Jwmsft
Description: "可布局顶级导航、同时节省屏幕空间的控件。"
title: "导航视图"
ms.assetid: 
label: Navigation view
template: detail.hbs
ms.author: jimwalk
ms.date: 05/19/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "windows 10，uwp"
pm-contact: yulikl
design-contact: kimsea
dev-contact: tpaine
doc-status: Published
ms.openlocfilehash: 98ab8a95288e5a0225a2185f7ce037e16209d173
ms.sourcegitcommit: ba0d20f6fad75ce98c25ceead78aab6661250571
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/24/2017
---
# <a name="navigation-view"></a>导航视图

<link rel="stylesheet" href="https://az835927.vo.msecnd.net/sites/uwp/Resources/css/custom.css">

> [!IMPORTANT]
> 本文介绍的功能尚未发布，在商业发行之前可能发生实质性修改。 Microsoft 不对此处提供的信息作任何明示或默示的担保。

导航视图控件通过可折叠导航菜单为应用的顶级区域提供常见的垂直布局。 此控件旨在实现导航窗格或汉堡包菜单、模式，并自动使其布局适应不同窗口大小。

> **重要 API**：[NavigationView 类](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview)、[NavigationViewItem 类](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationviewitem)、[NavigationViewDisplayMode 枚举](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationviewdisplaymode)

![NavigationView 示例](images/navview_wireframe.png)


## <a name="is-this-the-right-control"></a>这是正确的控件吗？

NavigationView 非常适用于：

-  具有许多相似类型的高级导航项目的应用。 例如具有“橄榄球”、“棒球”、“篮球”、“足球”等类别的体育应用。
-  跨应用提供一致的导航体验。 导航窗格应仅包括导航元素，而非操作。
-  中等数量到大量 (5-10) 的顶级导航类别。
-  保留较小窗口的屏幕空间。

导航视图只不过是你可以使用的几个导航元素之一；若要了解有关导航模式和其他导航元素的详细信息，请参阅[通用 Windows 平台 (UWP) 应用的导航设计基础知识](../layout/navigation-basics.md)。

有关如何使用 SplitView 和 ListView 构建导航窗格模式的代码示例，请从 GitHub 下载 [XAML 导航解决方案](https://github.com/Microsoft/Windows-universal-samples/tree/dev/Samples/XamlNavigation)。

## <a name="navigationview-parts"></a>NavigationView 部分
该控件大致细分为三个部分 - 左侧是用于导航的窗格，右侧是标题和内容区域。

![NavigationView 部分](images/navview_sections.png)

### <a name="pane"></a>窗格

导航窗格可以包含：

- [NavigationViewItem](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationviewitem) 形式的导航项，用于导航到特定页
- [NavigationViewItemSeparator](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationviewitemseparator) 形式的分隔符，用于对导航项分组
- [NavigationViewItemHeader](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationviewitemheader) 形式的标题，用于标记项组
- [应用设置](../app-settings/app-settings-and-data.md)的可选入口点。 若要隐藏设置项，请使用 [IsSettingsVisible](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview#Windows_UI_Xaml_Controls_NavigationView_IsSettingsVisible) 属性
- 窗格页脚中的自由格式内容（添加到 [PaneFooter](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview#Windows_UI_Xaml_Controls_NavigationView_PaneFooter) 属性后）

内置的导航（“汉堡包”）按钮使用户可以打开和关闭窗格。 打开窗格后在较大的应用窗口中，你可以选择使用 [IsPaneToggleButtonVisible](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview#Windows_UI_Xaml_Controls_NavigationView_IsPaneToggleButtonVisible) 属性隐藏此按钮。

### <a name="header"></a>标题

标题区域与导航按钮纵向对齐，有固定的高度。 它的用途是保存所选导航类别的页面标题。 标题停靠在页面顶部，并作为内容区域的滚动剪辑点。

当 NavigationView 处于“最小化”模式时，标题必须可见。 你可以选择在其他模式下隐藏标题，这样可以增大窗口宽度。 若要执行此操作，请将 [AlwaysShowHeader](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview#Windows_UI_Xaml_Controls_NavigationView_AlwaysShowHeader) 属性设置为 **False**。

### <a name="content"></a>内容

内容区域是显示所选导航类别的大部分信息的位置。 它可能包含一个或多个元素，是用于放置附加的子级别导航（如[透视表](tabs-pivot.md)）的合适区域。

建议在 NavigationView 处于“最小化”模式时使用 12px 边距，否则使用 24px 边距。

## <a name="visual-style"></a>视觉样式

<div class="microsoft-internal-note">
用于此控件的红线在 [UNI](http://uni/DesignDepot.FrontEnd/#/ProductNav?t=Fluent%20Design%20System%7CControls%20%26%20Patterns%7CNavigationView) 中提供。<br/><br/>
</div>

导航项目支持选定、禁用、指针悬停、按下和聚焦这些视觉状态。

![NavigationView 项目状态：禁用、指针悬停、按下、聚焦](images/navview_item-states.png)

当满足硬件和软件要求时，NavigationView 会自动在其窗格中使用新的[亚克力材料](../style/acrylic.md)和[突出显示](../style/reveal.md)。


## <a name="navigationview-modes"></a>NavigationView 模式
NavigationView 窗格可以打开或关闭，并且有三个显示模式选项：
-  **最小** 在根据需要显示和隐藏窗格时，只有汉堡包按钮保持固定。
-  **精简** 窗格始终显示为可以打开至全宽的细窄长条。
-  **展开** 窗格在内容旁边处于打开状态。 通过激活汉堡包按钮关闭后，窗格的宽度变为细窄长条。

默认情况下，系统将自动根据可用于控件的屏幕空间量选择最佳显示模式。 （你可以覆盖此设置 — 请参阅下一节了解详细信息。）

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
-  在使用少量屏幕空间时提供所选位置的一些指示。
-  此模式较适合中等屏幕，如平板电脑和 [10 英尺体验](../input-and-devices/designing-for-tv.md)。
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

## <a name="overriding-the-default-adaptive-behavior"></a>覆盖默认自适应行为

导航视图会根据可用的屏幕空间自动更改显示模式。
[!NOTE] NavigationView 应用作应用的根容器，此控件可跨越应用窗口的整个宽度和高度。
通过使用 [CompactModeThresholdWidth](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview#Windows_UI_Xaml_Controls_NavigationView_CompactModeThresholdWidth) 和 ExpandedModeThresholdWidth](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview#Windows_UI_Xaml_Controls_NavigationView_ExpandedModeThresholdWidth) 属性，你可以覆盖导航视图更改显示模式所在的宽度。 请考虑以下情形，其中阐明了你可能要自定义显示模式行为的情况。

-  **频繁导航** 如果你预计用户会在应用区域之间频繁导航，请考虑使窗格保留在狭窄窗口宽度的视图中。 包含“歌曲”/“相册”/“艺术家”导航区域的音乐应用可能会选择 280px 窗格宽度，当应用窗口宽于 560px 时，该应用将保持“展开”模式。
```xaml
<NavigationView OpenPaneLength=”280” CompactModeThresholdWidth="560" ExpandedModeThresholdWidth=”560”/>
```
-    **少数导航** 如果你预计用户很少在应用区域之间导航，请考虑使窗格在较宽的窗口宽度隐藏。 即使在 1080p 显示器上最大化应用，具有多个布局的计算器应用也可能会选择保留“最小”模式。
```xaml
<NavigationView CompactModeThresholdWidth=”1920” ExpandedModeThresholdWidth=”1920”/>
```
-    **图标消除歧义** 如果应用的导航区域不适用于有意义的图标，请勿使用“精简”模式。 具有“集合”/“相册”/“文件夹”导航区域的图像查看应用可以选择在狭窄和中等宽度以“最小”模式显示以及在较宽宽度以“展开”模式显示 NavigationView。
```xaml
<NavigationView CompactModeThresholdWidth=”1008”/>
```

## <a name="interaction"></a>交互

当用户点击窗格中的导航类别时，NavigationView 会将该项目显示为选定项目，并引发 [ItemInvoked](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview#Windows_UI_Xaml_Controls_NavigationView_ItemInvoked) 事件。 如果点击导致新项被选中，则 NavigationView 还将引发 [SelectionChanged](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview#Windows_UI_Xaml_Controls_NavigationView_SelectionChanged) 事件。 你的应用负责用相应信息更新“标题”和“内容”，以响应此用户交互。 此外，我们建议你以编程方式将焦点从导航项目移动到内容。 通过在加载时设置初始焦点，可以简化用户流并最大程度地减少预期的键盘焦点移动数。

## <a name="how-to-use-navigationview"></a>如何使用 NavigationView

下面是演示如何将 NavigationView 合并到应用中的简单示例。

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
                    Loaded="NavView_Loaded">
    <!-- Load NavigationViewItems in NavView_Loaded. -->

        <NavigationView.HeaderTemplate>
            <DataTemplate>
                <Grid>
                    <Grid.ColumnDefinitions>
                        <ColumnDefinition Width="Auto"/>
                        <ColumnDefinition/>
                    </Grid.ColumnDefinitions>
                    <TextBlock Style="{StaticResource TitleTextBlockStyle}"
                           FontSize="28"
                           VerticalAlignment="Center"
                           Margin="12,0"
                           Text="Welcome"/>
                    <CommandBar Grid.Column="1"
                            HorizontalAlignment="Right"
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

        <Frame x:Name="ContentFrame">
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
    NavView.MenuItems.Add(new NavigationViewItem()
        { Text = "Apps", Icon = new SymbolIcon(Symbol.AllApps), Tag = "apps" });
    NavView.MenuItems.Add(new NavigationViewItem()
        { Text = "Games", Icon = new SymbolIcon(Symbol.Video), Tag = "games" });
    NavView.MenuItems.Add(new NavigationViewItem()
        { Text = "Music", Icon = new SymbolIcon(Symbol.Audio), Tag = "music" });
    NavView.MenuItems.Add(new NavigationViewItemSeparator());
    NavView.MenuItems.Add(new NavigationViewItem()
        { Text = "My content", Icon = new SymbolIcon(Symbol.Folder), Tag = "content" });

    foreach (NavigationViewItem item in NavView.MenuItems)
    {
        if (item.Tag.ToString() == "play")
        {
            NavView.SelectedItem = item;
            break;
        }
    }
}

private void NavView_ItemInvoked(NavigationView sender, NavigationViewItemInvokedEventArgs args)
{
    if (args.IsSettingsInvoked == true)
    {
        ContentFrame.Navigate(typeof(SettingsPage));
    }
    else
    {
        switch ((args.InvokedItem as NavigationViewItem).Tag)
        {
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
}
```

## <a name="navigation"></a>导航

NavigationView 不会自动在应用的标题栏中显示后退按钮，也不会向后退堆栈中添加内容。 该控件不会自动响应软件或硬件的后退按钮按下操作。 有关本主题的详细信息以及如何向应用中添加对导航的支持，请参阅[历史记录和向后导航](../layout/navigation-history-and-backwards-navigation.md)。


## <a name="related-topics"></a>相关主题

* [NavigationView 类](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview)
* [大纲/细节](master-details.md)
* [Pivot 控件](tabs-pivot.md)
* [导航基础知识](../layout/navigation-basics.md)
 

 
