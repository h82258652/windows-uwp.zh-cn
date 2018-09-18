---
author: QuinnRadich
Description: NavigationView is an adaptive control that implements top-level navigation patterns for your app.
title: 导航视图
template: detail.hbs
ms.author: quradic
ms.date: 06/25/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10，uwp
pm-contact: yulikl
design-contact: kimsea
dev-contact: ''
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: 6c75169f118e2c8ef575fa251a7badc8cfe44247
ms.sourcegitcommit: f5321b525034e2b3af202709e9b942ad5557e193
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/18/2018
ms.locfileid: "4022215"
---
# <a name="navigation-view-preview-version"></a>导航视图 （预览版本）

> **这是预览版本**： 本指南介绍了仍处于开发阶段 NavigationView 控件的新版本。 若要使用它现在，你需要的[最新的 Windows 预览体验成员版本和 SDK](https://insider.windows.com/for-developers/)或[Windows UI 库](https://docs.microsoft.com/uwp/toolkits/winui/)。

NavigationView 控件提供了你的应用的顶级导航。 它适应各种屏幕大小支持多个导航样式。

> **Windows UI 库 Api**: [Microsoft.UI.Xaml.Controls.NavigationView 类](/uwp/api/microsoft.ui.xaml.controls.navigationview)

> **平台 Api**: [Windows.UI.Xaml.Controls.NavigationView 类](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview)

## <a name="get-the-windows-ui-library"></a>获取 Windows UI 库

此控件是 Windows UI 库，包含新控件和适用于 UWP 应用的 UI 功能的 NuGet 包的一部分。 有关详细信息，包括安装说明，请参阅[Windows UI 库概述](https://docs.microsoft.com/uwp/toolkits/winui/)。 

## <a name="navigation-styles"></a>导航样式

NavigationView 支持：

**在左侧的导航窗格或菜单**

![展开的导航窗格](images/displaymode-left.png)

**顶部导航窗格或菜单**

![顶部导航](images/displaymode-top.png)

## <a name="is-this-the-right-control"></a>这是正确的控件吗？

NavigationView 是自适应的导航控件，非常适用于：

- 提供一致的导航体验整个应用。
- 保留较小的 windows 上的屏幕空间。
- 组织访问很多导航类别。

有关其他导航控件，请参阅[导航设计基础知识](../basics/navigation-basics.md)。

如果你的导航需要 NavigationView 不支持的更复杂行为，可以考虑改用[大纲/细节](master-details.md)模式。

:::row:::
    :::column:::
        ![某些图像](images/XAML-controls-gallery-app-icon.png)
    :::column-end:::
    ::: 列范围 ="2"::: **XAML 控件库**<br>
        如果你安装了该 XAML 控件库应用，请单击<a href="xamlcontrolsgallery:/item/NavigationView">此处</a>以打开此应用，了解 NavigationView 操作。

        <a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Get the XAML Controls Gallery app (Microsoft Store)</a><br>
        <a href="https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics">Get the source code (GitHub)</a>
    :::column-end:::
:::row-end:::

## <a name="display-modes"></a>显示模式

NavigationView 可以将设置为不同的显示模式，通过`PaneDisplayMode`属性：

:::row:::
    :::column:::
    ### Left
    Displays an expanded left positioned pane.
    :::column-end:::
    :::column span="2":::
    ![left nav pane expanded](images/displaymode-left.png)
    :::column-end:::
:::row-end:::

我们建议左侧的导航时：

- 你可以中高大量 (5-10) 同样重要的顶级导航类别。
- 你希望与其他应用内容所需的空间更小的非常重要的导航类别。

:::row:::
    :::column:::
    ### Top
    Displays a top positioned pane.
    :::column-end:::
    :::column span="2":::
    ![top navigation](images/displaymode-top.png)
    :::column-end:::
:::row-end:::

我们建议顶部导航时：

- 你有 5 或更少同样重要的顶级导航类别，以便最终下拉列表中任何额外的顶级导航类别溢出菜单被视为不太重要。
- 你需要显示在屏幕上的所有导航选项。
- 为应用内容需要更多空间。
- 图标不能清楚地描述你的应用的导航类别。

:::row:::
    :::column:::
    ### LeftCompact
    Displays a thin sliver with icons on the left.
    :::column-end:::
    :::column span="2":::
    ![nav pane compact](images/displaymode-leftcompact.png)
    :::column-end:::
:::row-end:::

:::row:::
    :::column:::
    ### LeftMinimal
    Displays only the menu button.
    :::column-end:::
    :::column span="2":::
    ![nav pane minimal](images/displaymode-leftminimal.png)
    :::column-end:::
:::row-end:::

### <a name="auto"></a>Auto

![gif leftnav 默认自适应行为](images/displaymode-auto.png)

可在较小的屏幕上的 LeftMinimal、 LeftCompact 之间中等屏幕，并在较大的屏幕上的左侧。 请参阅有关详细信息的[自适应行为](#adaptive-behavior)部分。

## <a name="anatomy"></a>结构

<b>左侧的导航</b><br>

![左侧的导航视图部分](images/leftnav-anatomy.png)

<b>顶级导航</b><br>

![顶级导航视图部分](images/topnav-anatomy.png)

## <a name="pane"></a>窗格

窗格可以放置的顺序或左侧，通过`PanePosition`属性。

下面是向左和顶级窗格职位详细的窗格结构：

<b>左侧的导航</b><br>

![NavigationView 解剖学](images/navview-pane-anatomy-vertical.png)

1. “菜单”按钮
1. 导航项目
1. 分隔符
1. 标题
1. AutoSuggestBox （可选）
1. （可选） 的设置按钮

<b>顶级导航</b><br>

![NavigationView 解剖学](images/navview-pane-anatomy-horizontal.png)

1. 标题
1. 导航项目
1. 分隔符
1. AutoSuggestBox （可选）
1. （可选） 的设置按钮

后退按钮显示在窗格的左上角，但 NavigationView 不会自动向后退堆栈添加内容。 若要启用向后导航，请参阅[向后导航](#backwards-navigation)部分。

NavigationView 窗格还可以包含：

1. 导航项，形式的[NavigationViewItem](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationviewitem)，用于导航到特定页面。
2. 分隔符，形式的[NavigationViewItemSeparator](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationviewitemseparator)，用于对导航项分组。 设置为 0 来呈现空间作为分隔符的[Opacity](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationviewitemseparator.opacity)属性。
3. 标头，形式的[NavigationViewItemHeader](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationviewitemheader)，用于标记项组。
4. 可选[AutoSuggestBox](auto-suggest-box.md)允许应用级别搜索。
5. [应用设置](../app-settings/app-settings-and-data.md)的可选入口点。 若要隐藏设置项，请使用[IsSettingsVisible](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview.IsSettingsVisible)属性。

左窗格中包含：

6. 若要切换的窗格中打开和关闭的菜单按钮。 打开窗格后在较大的应用窗口中，你可以选择使用 [IsPaneToggleButtonVisible](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview.IsPaneToggleButtonVisible) 属性隐藏此按钮。

### <a name="pane-footer"></a>窗格页脚

窗格页脚中的自由格式内容（添加到 [PaneFooter](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview.PaneFooter) 属性后）

:::row:::
    :::column:::
    <b>左侧的导航</b><br>
    ![窗格页脚左侧的导航](images/navview-freeform-footer-left.png)<br>
    :::column-end:::
    :::column:::
     <b>顶级导航</b><br>
    ![窗格标头顶部导航](images/navview-freeform-footer-top.png)<br>
    :::column-end:::
:::row-end:::

### <a name="pane-header"></a>窗格标头

窗格的标头，当添加到[PaneHeader](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview.PaneHeader)属性中的自由格式内容

:::row:::
    :::column:::
    <b>左侧的导航</b><br>
    ![窗格标头左侧的导航](images/navview-freeform-header-left.png)<br>
    :::column-end:::
    :::column:::
     <b>顶级导航</b><br>
    ![窗格标头顶部导航](images/navview-freeform-header-top.png)<br>
    :::column-end:::
:::row-end:::

### <a name="pane-content"></a>窗格内容

在窗格中，添加到[PaneCustomContent](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview.PaneCustomContent)属性时的自由格式内容

:::row:::
    :::column:::
    <b>左侧的导航</b><br>
    ![自定义 contentleft 导航窗格](images/navview-freeform-pane-left.png)<br>
    :::column-end:::
    :::column:::
     <b>顶级导航</b><br>
    ![窗格自定义内容顶部导航](images/navview-freeform-pane-top.png)<br>
    :::column-end:::
:::row-end:::

### <a name="visual-style"></a>视觉样式

满足硬件和软件要求时，NavigationView 会自动使用[亚克力材料](../style/acrylic.md)在其窗格中，并且仅在其左侧窗格中[显示突出显示](../style/reveal.md)。

## <a name="header"></a>标题

![标题区域的 navview 一般图像](images/nav-header.png)

标题区域与在左窗格中的位置中，导航按钮纵向对齐，位于下方的顶部窗格位置的窗格。 它具有固定的高度 52 px。 它的用途是保存所选导航类别的页面标题。 标题停靠在页面顶部，并作为内容区域的滚动剪辑点。

当 NavigationView 处于最小的显示模式时，标题必须可见。 你可以选择在其他模式下隐藏标题，这样可以增大窗口宽度。 若要执行此操作，请将 [AlwaysShowHeader](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview.AlwaysShowHeader) 属性设置为 **False**。

## <a name="content"></a>内容

![内容区域的 navview 一般图像](images/nav-content.png)

内容区域是显示所选导航类别的大部分信息的位置。

建议在 NavigationView 处于“最小”模式时对内容区域使用 12px 边距，其他情况使用 24px 边距。

## <a name="adaptive-behavior"></a>自适应行为

NavigationView 会根据可用屏幕空间大小自动更改其显示模式。 但是，你可能想要自定义的自适应显示模式行为。

### <a name="default"></a>默认值

NavigationView 的默认自适应行为是较小的窗口宽度上显示较大的窗口宽度展开左侧窗格中、 中等大小的窗口宽度上向左仅限图标的导航窗格和汉堡菜单按钮。 有关自适应行为的窗口大小的详细信息，请参阅[屏幕大小和断点](../layout/screen-sizes-and-breakpoints-for-responsive-design.md)。

![gif leftnav 默认自适应行为](images/displaymode-auto.png)

```xaml
<NavigationView />
```

### <a name="minimal"></a>最小

第二个常见的自适应模式是在较大的窗口宽度，并且这两个中等和小窗口宽度汉堡包菜单上使用展开的左侧窗格。

![gif leftnav 自适应行为 2](images/adaptive-behavior-minimal.png)

```xaml
<NavigationView CompactModeThresholdWidth="1008" ExpandedModeThresholdWidth="1007" />
```

我们建议此时：

- 所需较小的窗口宽度上的应用内容的更多的空间。
- 导航类别不清楚地表示带图标。

### <a name="compact"></a>精简

第三个常见的自适应模式是使用较大的窗口宽度，，这两个中等和小窗口宽度为仅图标的左侧的导航窗格的展开的左侧窗格。 良好示例是邮件应用。

![gif leftnav 自适应行为 3](images/adaptive-behavior-compact.png)

```xaml
<NavigationView CompactModeThresholdWidth="0" ExpandedModeThresholdWidth="1007" />
```

我们建议此时：

- 请务必始终显示在屏幕上的所有导航选项。
- 导航类别可以清楚地表示带图标。

### <a name="no-adaptive-behavior"></a>任何自适应行为

有时你在所有可能不需要任何自适应行为。 你可以设置为始终扩展、 始终紧凑，或始终最少的窗格。

![gif leftnav 自适应行为 4](images/adaptive-behavior-none.png)

```xaml
<NavigationView PaneDisplayMode="LeftMinimal" />
```

### <a name="top-to-left-navigation"></a>上向左导航

我们建议使用较大的窗口大小和在小的左侧的导航的顶部导航窗口大小时：

- 你有同等重要的顶级导航类别显示在一起，一组，以便如果此设置中的一个类别不适合在屏幕上，你折叠到左侧的导航，以便为他们提供同样重要。
- 你想要保留为更内容的空间，较小的窗口大小。

下面是一个示例：

![gif 顶部或左侧导航自适应行为 1](images/navigation-top-to-left.png)

```xaml
<Grid >
    <VisualStateManager.VisualStateGroups>
        <VisualStateGroup>
            <VisualState>
                <VisualState.StateTriggers>
                    <AdaptiveTrigger MinWindowWidth="{x:Bind NavigationViewControl.CompactModeThresholdWidth}" />
                </VisualState.StateTriggers>

                <VisualState.Setters>
                    <Setter Target="NavigationViewControl.PaneDisplayMode" Value="Top"/>
                </VisualState.Setters>
            </VisualState>
        </VisualStateGroup>
    </VisualStateManager.VisualStateGroups>

    <NavigationView x:Name="NavigationViewControl" >
        <NavigationView.MenuItems>
            <NavigationViewItem Content="A" x:Name="A" />
            <NavigationViewItem Content="B" x:Name="B" />
            <NavigationViewItem Content="C" x:Name="C" />
        </NavigationView.MenuItems>
    </NavigationView>
</Grid>

```

有时应用需要将不同的数据绑定到的顶部窗格和左窗格。 通常左侧的窗格中包括更多的导航元素。

下面是一个示例：

![gif 顶部或左侧导航自适应行为 2](images/navigation-top-to-left2.png)

```xaml
<Page >
    <Page.Resources>
        <DataTemplate x:name="navItem_top_temp" x:DataType="models:Item">
            <NavigationViewItem Background= Icon={x:Bind TopIcon}, Content={x:Bind TopContent}, Visibility={x:Bind TopVisibility} />
        </DataTemplate>

        <DataTemplate x:name="navItem_temp" x:DataType="models:Item">
            <NavigationViewItem Icon={x:Bind Icon}, Content={x:Bind Content}, Visibility={x:Bind Visibility} />
        </DataTemplate>
        
        <services:NavViewDataTemplateSelector x:Key="navview_selector" 
              NavItemTemplate="{StaticResource navItem_temp}" 
              NavItemTopTemplate="{StaticResource navItem_top_temp}" 
              NavPaneDisplayMode="{x:Bind NavigationViewControl.PaneDisplayMode}">
        </services:NavViewDataTemplateSelector>
    </Page.Resources>

    <Grid >
        <VisualStateManager.VisualStateGroups>
            <VisualStateGroup>
                <VisualState>
                    <VisualState.StateTriggers>
                        <AdaptiveTrigger MinWindowWidth="{x:Bind NavigationViewControl.CompactModeThresholdWidth}" />
                    </VisualState.StateTriggers>

                    <VisualState.Setters>
                        <Setter Target="NavigationViewControl.PaneDisplayMode" Value="Top"/>
                    </VisualState.Setters>
                </VisualState>
            </VisualStateGroup>
        </VisualStateManager.VisualStateGroups>

        <NavView x:Name='NavigationViewControl' MenuItemsSource={x:Bind items}   
                 PanePosition = "Top" MenuItemTemplateSelector="navview_selector" />
    </Grid>
</Page>

```

```csharp
ObservableCollection<Item> items = new ObservableCollection<Item>();
items.Add(new Item() {
    Content = "Aa",
    TopContent ="A",
    Icon = new BitmapIcon() { UriSource = new Uri("ms-appx:///testimage.jpg") },
    TopIcon = new BitmapIcon(),
    ItemVisibility = Visibility.Visible,
    TopItemVisiblity = Visibility.Visible 
});
items.Add(new Item() {
    Content = "Bb",
    TopContent = "B",
    Icon = new BitmapIcon() { UriSource = new Uri("ms-appx:///testimage.jpg") },
    TopIcon = new BitmapIcon(),
    ItemVisibility = Visibility.Visible,
    TopItemVisiblity = Visibility.Visible 
});
items.Add(new Item() {
    Content = "Cc",
    TopContent = "C",
    Icon = new BitmapIcon() { UriSource = new Uri("ms-appx:///testimage.jpg") },
    TopIcon = new BitmapIcon(),
    ItemVisibility = Visibility.Visible,
    TopItemVisiblity = Visibility.Visible 
});

public class NavViewDataTemplateSelector : DataTemplateSelector
    {
        public DataTemplate NavItemTemplate { get; set; }

        public DataTemplate NavItemTopTemplate { get; set; }    

     public NavigationViewPaneDisplayMode NavPaneDisplayMode { get; set; }

        protected override DataTemplate SelectTemplateCore(object item)
        {
            Item currItem = item as Item;
            if (NavPaneDisplayMode == NavigationViewPanePosition.Top)
                return NavItemTopTemplate;
            else 
                return NavItemTemplate;
        }   

    }

```

## <a name="interaction"></a>交互

当用户点击窗格中的导航项目时，NavigationView 会将该项目显示为选定项目，并引发 [ItemInvoked](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview.ItemInvoked) 事件。 如果点击导致新项被选中，则 NavigationView 还将引发 [SelectionChanged](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview.SelectionChanged) 事件。

你的应用负责用相应信息更新“标题”和“内容”，以响应此用户交互。 此外，建议以编程方式将[焦点](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.control.FocusState)从导航项目移动到内容。 通过在加载时设置初始焦点，可以简化用户流并最大程度地减少预期的键盘焦点移动数。

### <a name="tabs"></a>选项卡

在选项卡模型中，选择和焦点相关联。 通常将焦点移将还转移选择操作。 在以下示例中，右箭头会选择指示器从移动显示到放大镜。 你可以通过将[SelectionFollowsFocus](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview.selectionfollowsfocus)属性设置为启用达到此目的。

![纯文本的顶部 navview 的屏幕截图](images/nav-tabs.png)

下面是示例 XAML 为该：

```xaml
<NavigationView PanePosition="Top" SelectionFollowsFocus="Enabled" >
   <NavigationView.MenuItems>
        <NavigationViewItem Content="Display" />
        <NavigationViewItem Content="Magnifier"  />
        <NavigationViewItem Content="Keyboard" />
    </NavigationView.MenuItems>
</NavigationView>

```

若要更改选项卡选择时，则换出内容，可以使用帧的[NavigateWithOptions](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.frame.NavigateToType)方法 FrameNavigationOptions.IsNavigationStackEnabled 设置为 False，并 NavigateOptions.TransitionInfoOverride 将设置为相应到并行幻灯片动画。 有关示例，请参阅下面的[代码示例](#code-example)。

如果你想要更改默认样式，你可以重写 NavigationView 的[MenuItemContainerStyle](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview.menuitemcontainerstyle)属性。 你还可以设置[MenuItemTemplate](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview.menuitemtemplate)属性指定不同的数据模板。

## <a name="backwards-navigation"></a>向后导航

NavigationView 具有内置的后退按钮，可以通过以下属性启用：

- [**IsBackButtonVisible**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview.IsBackButtonVisible) 是 NavigationViewBackButtonVisible 枚举，默认为“Auto”。 它用于显示/隐藏后退按钮。 当此按钮不可见时，绘制后退按钮的空间会折叠起来。
- [**IsBackEnabled**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview.IsBackEnabled) 默认为 false，可用于切换后退按钮的状态。
- [**BackRequested**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview.BackRequested) 在用户单击后退按钮时触发。
    - 在“最小”/“精简”模式下，NavigationView.Pane 以浮出控件的形式打开，单击后退按钮会关闭面板并引发 **PaneClosing** 事件。
    - 如果 IsBackEnabled 为 false，则不会引发。

:::row:::
    :::column:::
    <b>左侧的导航</b><br>
    ![NavigationView 后退按钮上的左导航](images/leftnav-back.png)
    :::column-end:::
    :::column:::
     <b>顶级导航</b><br>
    ![在顶部导航 NavigationView 后退按钮](images/topnav-back.png)
    :::column-end:::
:::row-end:::

## <a name="code-example"></a>代码示例

> [!NOTE]
> NavigationView 应该用作应用的根容器，因为此控件可跨越应用窗口的整个宽度和高度。
你可以使用 [CompactModeThresholdWidth](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview.CompactModeThresholdWidth) 和 [ExpandedModeThresholdWidth](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview.ExpandedModeThresholdWidth) 属性覆盖导航视图更改显示模式所依据的宽度。

下面是如何，你可以将 NavigationView 合并与较大的窗口大小的顶部导航窗格和较小的窗口大小的左侧的导航窗格的端到端示例。

在此示例中，我们希望最终用户经常选择新的导航类别，因此我们：

- 将[SelectionFollowsFocus](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview.PanePostion)属性设置为已启用
- 使用不将添加到导航堆栈的框架导航。
- [ShoulderNavigationEnabled](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview.PanePostion)属性，用于指示游戏板上的左/右缓冲键是否导航你的应用的顶级导航类别上保留默认值。 默认值为"WhenSelectionFollowsFocus"。 和"从不"，其他可能的值为"始终"。

我们还演示了如何实现向后导航使用 NavigationView 的后退按钮。

下面是录制的示例演示的内容：

![NavigationView 端到端示例](images/nav-code-example.gif)

下面是示例代码：

> [!NOTE]
> 如果你使用[Windows UI 库](https://docs.microsoft.com/uwp/toolkits/winui/)，则你将需要添加对此工具包的引用： `xmlns:controls="using:Microsoft.UI.Xaml.Controls"`。

```xaml
<Page
    x:Class="NavigationViewSample.MainPage"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:local="using:NavigationViewSample"
    xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
    xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
    mc:Ignorable="d">

    <Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
        <VisualStateManager.VisualStateGroups>
            <VisualStateGroup>
                <VisualState>
                    <VisualState.StateTriggers>
                        <AdaptiveTrigger MinWindowWidth="{x:Bind NavView.CompactModeThresholdWidth}" />
                    </VisualState.StateTriggers>

                    <VisualState.Setters>
                        <Setter Target="NavView.PaneDisplayMode" Value="Top"/>
                    </VisualState.Setters>
                </VisualState>
            </VisualStateGroup>
        </VisualStateManager.VisualStateGroups>

        <NavigationView x:Name="NavView"
                    SelectionFollowsFocus="Enabled"
                    ItemInvoked="NavView_ItemInvoked"
                    IsSettingsVisible="True"
                    Loaded="NavView_Loaded"
                    BackRequested="NavView_BackRequested"
                    Header="Welcome">

            <NavigationView.MenuItems>
                <NavigationViewItem Content="Home" x:Name="home" Tag="home">
                    <NavigationViewItem.Icon>
                        <FontIcon Glyph="&#xE10F;"/>
                    </NavigationViewItem.Icon>
                </NavigationViewItem>
                <NavigationViewItemSeparator/>
                <NavigationViewItemHeader Content="Main pages"/>
                <NavigationViewItem Icon="AllApps" Content="Apps" x:Name="apps" Tag="apps"/>
                <NavigationViewItem Icon="Video" Content="Games" x:Name="games" Tag="games"/>
                <NavigationViewItem Icon="Audio" Content="Music" x:Name="music" Tag="music"/>
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

            <Frame x:Name="ContentFrame" Margin="24"/>

        </NavigationView>
    </Grid>
</Page>
```

> [!NOTE]
> 如果你使用[Windows UI 库](https://docs.microsoft.com/uwp/toolkits/winui/)，则你将需要添加对此工具包的引用： `using MUXC = Microsoft.UI.Xaml.Controls;`。

```csharp
// List of ValueTuple holding the Navigation Tag and the relative Navigation Page 
private readonly IList<(string Tag, Type Page)> _pages = new List<(string Tag, Type Page)>
{
    ("home", typeof(HomePage)),
    ("apps", typeof(AppsPage)),
    ("games", typeof(GamesPage)),
    ("music", typeof(MusicPage)),
};

private void NavView_Loaded(object sender, RoutedEventArgs e)
{
    // You can also add items in code behind
    NavView.MenuItems.Add(new NavigationViewItemSeparator());
    NavView.MenuItems.Add(new NavigationViewItem
    {
        Content = "My content",
        Icon = new SymbolIcon(Symbol.Folder),
        Tag = "content"
    });
    _pages.Add(("content", typeof(MyContentPage)));

    ContentFrame.Navigated += On_Navigated;

    // NavView doesn't load any page by default: you need to specify it
    NavView_Navigate("home");

    // Add keyboard accelerators for backwards navigation
    var goBack = new KeyboardAccelerator { Key = VirtualKey.GoBack };
    goBack.Invoked += BackInvoked;
    this.KeyboardAccelerators.Add(goBack);

    // ALT routes here
    var altLeft = new KeyboardAccelerator
    {
        Key = VirtualKey.Left,
        Modifiers = VirtualKeyModifiers.Menu
    };
    altLeft.Invoked += BackInvoked;
    this.KeyboardAccelerators.Add(altLeft);
}

private void NavView_ItemInvoked(NavigationView sender, NavigationViewItemInvokedEventArgs args)
{

    if (args.IsSettingsInvoked)
        ContentFrame.Navigate(typeof(SettingsPage));
    else
    {
        // Getting the Tag from Content (args.InvokedItem is the content of NavigationViewItem)
        var navItemTag = NavView.MenuItems
            .OfType<NavigationViewItem>()
            .First(i => args.InvokedItem.Equals(i.Content))
            .Tag.ToString();

        NavView_Navigate(navItemTag);
    }
}

private void NavView_Navigate(string navItemTag)
{
    var item = _pages.First(p => p.Tag.Equals(navItemTag));
    ContentFrame.Navigate(item.Page);
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
    if (!ContentFrame.CanGoBack)
        return false;

    // Don't go back if the nav pane is overlayed
    if (NavView.IsPaneOpen &&
        (NavView.DisplayMode == NavigationViewDisplayMode.Compact ||
        NavView.DisplayMode == NavigationViewDisplayMode.Minimal))
        return false;

    ContentFrame.GoBack();
    return true;
}

private void On_Navigated(object sender, NavigationEventArgs e)
{
    NavView.IsBackEnabled = ContentFrame.CanGoBack;

    if (ContentFrame.SourcePageType == typeof(SettingsPage))
    {
        // SettingsItem is not part of NavView.MenuItems, and doesn't have a Tag
        NavView.SelectedItem = (NavigationViewItem)NavView.SettingsItem;
    }
    else
    {
        var item = _pages.First(p => p.Page == e.SourcePageType);

        NavView.SelectedItem = NavView.MenuItems
            .OfType<NavigationViewItem>()
            .First(n => n.Tag.Equals(item.Tag));
    }
}
```

## <a name="customizing-backgrounds"></a>自定义背景

要更改 NavigationView 主区域的背景，请将其 `Background` 属性设置为你的首选画笔。

当 NavigationView 处于顶部，最小或精简模式时，窗格的背景将显示应用内亚克力。 要更新此行为或自定义窗格的亚克力外观，请修改两个主题资源，方法是在 App.xaml 中覆盖它们。

```xaml
<Application.Resources>
    <ResourceDictionary>
        <AcrylicBrush x:Key="NavigationViewDefaultPaneBackground"
        BackgroundSource="Backdrop" TintColor="Yellow" TintOpacity=".6"/>
        <AcrylicBrush x:Key="NavigationViewTopPaneBackground"
        BackgroundSource="Backdrop" TintColor="Yellow" TintOpacity=".6"/>
        <AcrylicBrush x:Key="NavigationViewExpandedPaneBackground"
        BackgroundSource="HostBackdrop" TintColor="Orange" TintOpacity=".8"/>
    </ResourceDictionary>
</Application.Resources>
```

## <a name="scroll-content-under-top-pane"></a>在顶部窗格下的滚动内容

对于无缝的外观和感觉，如果你的应用具有使用 ScrollViewer 的页面，并且在导航窗格顶部放置，我们建议在顶部导航窗格下方的内容滚动。

这可以通过将[CanContentRenderOutsideBounds](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.scrollviewer.cancontentrenderoutsidebounds)属性设置为 true 相关的 ScrollViewer 上实现。

![navview 滚动导航窗格](images/nav-scroll-content.png)

如果你的应用具有非常长滚动内容，你可能想要考虑融入粘滞标头将附加到顶部导航窗格和形成平滑图面。 

![navview 滚动粘滞标头](images/nav-scroll-stickyheader.png)

你可以通过设置[ContentOverlay](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview.ContentOverlay)上 NavigationView 达到此目的。 

有时，如果用户向下滚动，你可能想要隐藏导航窗格中，通过在 NavigationView 的[IsPaneVisible](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview.ContentOverlay)属性设置为 false 来实现。

![navview 滚动隐藏导航](images/nav-scroll-hidepane.png)

## <a name="related-topics"></a>相关主题

- [NavigationView 类](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview)
- [大纲/细节](master-details.md)
- [Pivot 控件](tabs-pivot.md)
- [导航基础知识](../basics/navigation-basics.md)
- [UWP 的 Fluent Design 概述](../fluent-design-system/index.md)