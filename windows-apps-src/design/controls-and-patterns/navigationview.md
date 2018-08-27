---
author: serenaz
Description: NavigationView is an adaptive control that implements top-level navigation patterns for your app.
title: 导航视图
template: detail.hbs
ms.author: sezhen
ms.date: 08/06/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10，uwp
pm-contact: yulikl
design-contact: kimsea
dev-contact: ''
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: 4c0857005d584b1fde0eb52a6ab0ef5ec29eaf44
ms.sourcegitcommit: 753dfcd0f9fdfc963579dd0b217b445c4b110a18
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/27/2018
ms.locfileid: "2860554"
---
# <a name="navigation-view-preview-version"></a>导航视图 （预览版本）

> **这是预览版本**： 本文介绍仍处于开发 NavigationView 控件的新版本。 要使用它现在，您需要的[最新的 Windows 内幕生成和 SDK](https://insider.windows.com/for-developers/)或[Windows 用户界面库](https://docs.microsoft.com/uwp/toolkits/winui/)。

NavigationView 控件提供您的应用程序的顶级导航。 它适应多种屏幕大小支持多个导航样式。

> **Windows UI 库 Api**: [Microsoft.UI.Xaml.Controls.NavigationView 类](/uwp/api/microsoft.ui.xaml.controls.navigationview)

> **平台 Api**: [Windows.UI.Xaml.Controls.NavigationView 类](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview)

## <a name="get-the-windows-ui-library"></a>获取 Windows UI 库

此控件是作为 Windows UI 库，NuGet 程序包包含新控件和 UWP 应用程序的 UI 功能的一部分包含。 有关详细信息，包括安装说明，请参阅[Windows UI 库概述](https://docs.microsoft.com/uwp/toolkits/winui/)。 

## <a name="navigation-styles"></a>导航样式

NavigationView 支持：

**左侧的导航窗格或菜单**

![扩展的导航窗格](images/displaymode-left.png)

**顶部导航窗格或菜单**

![顶部导航](images/displaymode-top.png)

## <a name="is-this-the-right-control"></a>这是正确的控件吗？

NavigationView 是非常适合于在一个自适应导航控件：

- 提供一致的导航体验整个您的应用程序。
- 保留屏幕不动产上更小的窗口。
- 组织访问多个导航类别。

有关其他导航控件，请参阅[导航设计基础知识](../basics/navigation-basics.md)。

如果你的导航需要 NavigationView 不支持的更复杂行为，可以考虑改用[大纲/细节](master-details.md)模式。

:::row:::
    :::column:::
        ![某个图像](images/XAML-controls-gallery-app-icon.png)
    :::column-end:::
    ::: 列跨度 ="2"::: **XAML 控件库**<br>
        如果您已安装的 XAML 控件库应用程序，请单击<a href="xamlcontrolsgallery:/item/NavigationView">此处</a>以打开应用程序并查看 NavigationView 在操作。

        <a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Get the XAML Controls Gallery app (Microsoft Store)</a><br>
        <a href="https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics">Get the source code (GitHub)</a>
    :::column-end:::
:::row-end:::

## <a name="display-modes"></a>显示模式

NavigationView 可通过设置为不同的显示模式，`PaneDisplayMode`属性：

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

- 您必须中到高 (5-10) 也相当重要的顶级导航类别数。
- 所需非常主要导航类别与其他应用程序内容较少的空间。

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

- 具有 5 或更少的也相当重要的顶级导航类别，以便最终下拉列表中的任何其他的顶级导航类别溢出菜单视为重要性较低。
- 您需要显示在屏幕上的所有导航选项。
- 为应用程序内容需要更多空间。
- 图标不能清楚地描述您的应用程序导航类别。

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

小型屏幕上的 LeftMinimal、 LeftCompact 之间多变中型屏幕和大型屏幕上的左边。 请参阅详细信息[自适应行为](#adaptive-behavior)的部分。

## <a name="anatomy"></a>结构

<b>左侧导航窗格</b><br>

![左的 NavigationView 部分](images/leftnav-anatomy.png)

<b>顶部导航栏中</b><br>

![顶部 NavigationView 部分](images/topnav-anatomy.png)

## <a name="pane"></a>窗格

窗格中可以位于顶部或在左侧，通过`PanePosition`属性。

下面是详细的窗格解析的左边缘和上窗格位置：

<b>左侧导航窗格</b><br>

![NavigationView 解析](images/navview-pane-anatomy-vertical.png)

1. “菜单”按钮
1. 导航项目
1. 分隔符
1. 标题
1. AutoSuggestBox （可选）
1. （可选） 的设置按钮

<b>顶部导航栏中</b><br>

![NavigationView 解析](images/navview-pane-anatomy-horizontal.png)

1. 标题
1. 导航项目
1. 分隔符
1. AutoSuggestBox （可选）
1. （可选） 的设置按钮

后退按钮显示在左上角的窗格中，但 NavigationView 不会自动向后堆栈添加内容。 若要启用向后导航，请参阅[backwards 导航](#backwards-navigation)部分。

此外可以包含 NavigationView 窗格：

1. 表单[NavigationViewItem](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationviewitem)，导航到特定页面中的导航项目。
2. 分隔符， [NavigationViewItemSeparator](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationviewitemseparator)，形式进行分组的导航项目。 将[不透明度](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationviewitemseparator.opacity)属性设置为 0 呈现空间作为分隔符。
3. [NavigationViewItemHeader](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationviewitemheader)，用于标记的项目的组的窗体中的标题。
4. 可选[AutoSuggestBox](auto-suggest-box.md)以允许应用程序级别的搜索。
5. [应用设置](../app-settings/app-settings-and-data.md)的可选入口点。 要隐藏设置项，请使用[IsSettingsVisible](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview.IsSettingsVisible)属性。

左侧的窗格中包含：

6. 切换的窗格中打开和关闭的菜单按钮。 打开窗格后在较大的应用窗口中，你可以选择使用 [IsPaneToggleButtonVisible](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview.IsPaneToggleButtonVisible) 属性隐藏此按钮。

### <a name="pane-footer"></a>窗格页脚

窗格页脚中的自由格式内容（添加到 [PaneFooter](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview.PaneFooter) 属性后）

:::row:::
    :::column:::
    <b>左侧导航窗格</b><br>
    ![窗格页脚左侧导航窗格](images/navview-freeform-footer-left.png)<br>
    :::column-end:::
    :::column:::
     <b>顶部导航栏中</b><br>
    ![窗格标头顶部导航栏](images/navview-freeform-footer-top.png)<br>
    :::column-end:::
:::row-end:::

### <a name="pane-header"></a>窗格标头

自由格式窗格的标头，当添加到[PaneHeader](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview.PaneHeader)属性中的内容

:::row:::
    :::column:::
    <b>左侧导航窗格</b><br>
    ![窗格标头左侧导航窗格](images/navview-freeform-header-left.png)<br>
    :::column-end:::
    :::column:::
     <b>顶部导航栏中</b><br>
    ![窗格标头顶部导航栏](images/navview-freeform-header-top.png)<br>
    :::column-end:::
:::row-end:::

### <a name="pane-content"></a>窗格的内容

自由格式内容在窗格中，当添加到[PaneCustomContent](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview.PaneCustomContent)属性

:::row:::
    :::column:::
    <b>左侧导航窗格</b><br>
    ![自定义 contentleft 导航窗格](images/navview-freeform-pane-left.png)<br>
    :::column-end:::
    :::column:::
     <b>顶部导航栏中</b><br>
    ![窗格自定义内容顶部导航栏中](images/navview-freeform-pane-top.png)<br>
    :::column-end:::
:::row-end:::

### <a name="visual-style"></a>视觉样式

当满足硬件和软件要求时，NavigationView 会自动使用[丙烯酸纤维材料](../style/acrylic.md)中其窗格，并仅在其左侧窗格中[显示突出显示](../style/reveal.md)。

## <a name="header"></a>标题

![页眉区域的 navview 一般图像](images/nav-header.png)

页眉区域与的左窗格中位置，在导航按钮垂直对齐，并位于下方的窗格中的顶部窗格位置。 它具有 52 固定的高度像素。 它的用途是保存所选导航类别的页面标题。 标题停靠在页面顶部，并作为内容区域的滚动剪辑点。

NavigationView 处于最小的显示模式时，必须可见标头。 你可以选择在其他模式下隐藏标题，这样可以增大窗口宽度。 若要执行此操作，请将 [AlwaysShowHeader](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview.AlwaysShowHeader) 属性设置为 **False**。

## <a name="content"></a>内容

![内容区域的 navview 一般图像](images/nav-content.png)

内容区域是显示所选导航类别的大部分信息的位置。

建议在 NavigationView 处于“最小”模式时对内容区域使用 12px 边距，其他情况使用 24px 边距。

## <a name="adaptive-behavior"></a>自适应行为

NavigationView 会根据可用屏幕空间大小自动更改其显示模式。 但是，您可能想要自定义的自适应显示模式行为。

### <a name="default"></a>默认值

NavigationView 的默认自适应行为是小窗口宽度上显示大窗口宽度上的展开左侧窗格中、 中型窗口宽度上的仅图标的左侧导航窗格窗格和汉堡菜单按钮。 有关为自适应行为的窗口大小的详细信息，请参阅[屏幕大小和断点](../layout/screen-sizes-and-breakpoints-for-responsive-design.md)。

![gif leftnav 默认自适应行为](images/displaymode-auto.png)

```xaml
<NavigationView />
```

### <a name="minimal"></a>最小

第二个常见的自适应模式是使用大型窗口宽度和在这两个中小型窗口宽度汉堡菜单上的扩展的左窗格。

![gif leftnav 自适应行为 2](images/adaptive-behavior-minimal.png)

```xaml
<NavigationView CompactModeThresholdWidth="1008" ExpandedModeThresholdWidth="1007" />
```

我们建议此时：

- 您希望应用程序上的内容较小窗口宽度的更多空间。
- 导航类别不能清楚地表示与图标。

### <a name="compact"></a>精简

第三个常见的自适应模式是大型窗口宽度和两个中小型窗口宽度上的仅图标的左侧导航窗格窗格中使用扩展的左窗格。 一个很好的示例是邮件应用程序。

![gif leftnav 自适应行为 3](images/adaptive-behavior-compact.png)

```xaml
<NavigationView CompactModeThresholdWidth="0" ExpandedModeThresholdWidth="1007" />
```

我们建议此时：

- 请务必始终显示在屏幕上的所有导航选项。
- 可以用图标来明确表示导航类别。

### <a name="no-adaptive-behavior"></a>无自适应行为

有时您根本可能不需要任何自适应行为。 您可以设置为始终展开、 始终压缩，还是始终最少的窗格。

![gif leftnav 自适应行为 4](images/adaptive-behavior-none.png)

```xaml
<NavigationView PaneDisplayMode="LeftMinimal" />
```

### <a name="top-to-left-navigation"></a>从上到左侧导航

我们建议使用大型窗口大小和在小型的左侧导航栏上的顶部导航窗口调整大小时时：

- 您具有一组也相当重要的顶级导航类别显示在一起，例如，如果此设置中的一个类别不适合在屏幕上，则折叠到要为其提供同等重要的左侧导航。
- 您希望保留为得多内容空间尽可能的小窗口大小。

下面是一个示例：

![gif 顶部或左侧导航窗格自适应行为 1](images/navigation-top-to-left.png)

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

有时应用程序需要将不同的数据绑定到的顶部窗格和左窗格。 通常左窗格中包含多个导航元素。

下面是一个示例：

![gif 顶部或左侧导航窗格自适应行为 2](images/navigation-top-to-left2.png)

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

在选项卡模型中，选择和焦点关联。 操作，通常将焦点移将还移位所选内容。 在下面的示例中，右 arrowing 会移动所选内容标记从显示到放大镜。 您可以通过[SelectionFollowsFocus](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview.selectionfollowsfocus)属性设置为已启用达到此目的。

![纯文本顶部 navview 的屏幕截图](images/nav-tabs.png)

下面是示例 XAML，：

```xaml
<NavigationView PanePosition="Top" SelectionFollowsFocus="Enabled" >
   <NavigationView.MenuItems>
        <NavigationViewItem Content="Display" />
        <NavigationViewItem Content="Magnifier"  />
        <NavigationViewItem Content="Keyboard" />
    </NavigationView.MenuItems>
</NavigationView>

```

更换内容更改选项卡选择时，可以将框架的[NavigateWithOptions](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.frame.NavigateToType)方法 FrameNavigationOptions.IsNavigationStackEnabled 设置为 False，并且 NavigateOptions.TransitionInfoOverride 设置为适当侧边到侧幻灯片动画。 有关示例，请参阅下面的[代码示例](#code-example)。

如果您想要更改默认的样式，您可以重写 NavigationView 的[MenuItemContainerStyle](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview.menuitemcontainerstyle)属性。 您还可以设置[MenuItemTemplate](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview.menuitemtemplate)属性指定不同的数据模板。

## <a name="backwards-navigation"></a>向后导航

NavigationView 具有内置的后退按钮，可以通过以下属性启用：

- [**IsBackButtonVisible**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview.IsBackButtonVisible) 是 NavigationViewBackButtonVisible 枚举，默认为“Auto”。 它用于显示/隐藏后退按钮。 当此按钮不可见时，绘制后退按钮的空间会折叠起来。
- [**IsBackEnabled**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview.IsBackEnabled) 默认为 false，可用于切换后退按钮的状态。
- [**BackRequested**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview.BackRequested) 在用户单击后退按钮时触发。
    - 在“最小”/“精简”模式下，NavigationView.Pane 以浮出控件的形式打开，单击后退按钮会关闭面板并引发 **PaneClosing** 事件。
    - 如果 IsBackEnabled 为 false，则不会引发。

:::row:::
    :::column:::
    <b>左侧导航窗格</b><br>
    ![NavigationView 后退按钮在左侧导航窗格](images/leftnav-back.png)
    :::column-end:::
    :::column:::
     <b>顶部导航栏中</b><br>
    ![在顶部导航栏上的 NavigationView 后退按钮](images/topnav-back.png)
    :::column-end:::
:::row-end:::

## <a name="code-example"></a>代码示例

> [!NOTE]
> NavigationView 应该用作应用的根容器，因为此控件可跨越应用窗口的整个宽度和高度。
你可以使用 [CompactModeThresholdWidth](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview.CompactModeThresholdWidth) 和 [ExpandedModeThresholdWidth](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview.ExpandedModeThresholdWidth) 属性覆盖导航视图更改显示模式所依据的宽度。

下面是与大型窗口大小的顶部导航窗格和小窗口大小在左侧的导航窗格中，您就可以合并 NavigationView 的端到端示例。

在此示例中，我们期望频率选择新导航类别，最终用户，因此我们：

- 将[SelectionFollowsFocus](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview.PanePostion)属性设置为已启用
- 使用不将添加到导航堆栈的框架导航。
- [ShoulderNavigationEnabled](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview.PanePostion)属性，用于指示是否游戏板上的左/右缓冲器导航您的应用程序的顶级导航类别上保留默认值。 默认值为"WhenSelectionFollowsFocus"。 其他可能的值是"始终"和"从不"。

我们还演示了如何实现向后使用 NavigationView 的返回按钮的导航。

下面是录制的示例演示的内容：

![NavigationView 端到端示例](images/nav-code-example.gif)

下面是示例代码：

> [!NOTE]
> 如果您使用的[Windows 用户界面库](https://docs.microsoft.com/uwp/toolkits/winui/)中，则您需要添加对该工具包： `xmlns:controls="using:Microsoft.UI.Xaml.Controls"`。

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
> 如果您使用的[Windows 用户界面库](https://docs.microsoft.com/uwp/toolkits/winui/)中，则您需要添加对该工具包： `using MUXC = Microsoft.UI.Xaml.Controls;`。

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

窗格的背景显示应用程序内 acrylic NavigationView 位于顶部，最少或紧凑模式时。 要更新此行为或自定义窗格的亚克力外观，请修改两个主题资源，方法是在 App.xaml 中覆盖它们。

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

## <a name="scroll-content-under-top-pane"></a>在顶部窗格下滚动内容

无缝外观 + 外观，如果您的应用程序已使用 ScrollViewer 的页面和导航窗格是顶部定位，我们建议具有嵌套在顶部导航栏窗格的下方内容滚动。

这可以通过将[CanContentRenderOutsideBounds](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.scrollviewer.cancontentrenderoutsidebounds)属性设置为 true 时，相关 ScrollViewer 上实现。

![navview 滚动导航窗格](images/nav-scroll-content.png)

如果您的应用程序有很长滚动内容，您可能需要考虑引入的附加到顶部导航栏窗格和表单顺利面粘滞标头。 

![navview 滚动粘滞标头](images/nav-scroll-stickyheader.png)

您可以通过在 NavigationView 上设置[ContentOverlay](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview.ContentOverlay)属性来完成此任务。 

有时，如果用户向下滚动，您可能希望隐藏导航窗格中，通过在 NavigationView [IsPaneVisible](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview.ContentOverlay)属性设置为 false。

![navview 滚动隐藏导航窗格](images/nav-scroll-hidepane.png)

## <a name="related-topics"></a>相关主题

- [NavigationView 类](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview)
- [大纲/细节](master-details.md)
- [Pivot 控件](tabs-pivot.md)
- [导航基础知识](../basics/navigation-basics.md)
- [UWP 的 Fluent Design 概述](../fluent-design-system/index.md)