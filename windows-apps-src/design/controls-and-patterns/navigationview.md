---
author: jwmsft
Description: NavigationView is an adaptive control that implements top-level navigation patterns for your app.
title: 导航视图
template: detail.hbs
ms.author: jimwalk
ms.date: 10/02/2018
ms.topic: article
keywords: windows 10，uwp
pm-contact: yulikl
design-contact: kimsea
dev-contact: ''
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: 9eef1616625ca30b1887e7f317c59f7a75abfeea
ms.sourcegitcommit: 71e8eae5c077a7740e5606298951bb78fc42b22c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/13/2018
ms.locfileid: "6646511"
---
# <a name="navigation-view"></a>导航视图

NavigationView 控件提供了你的应用的顶级导航。 它适应各种屏幕大小，并支持_顶部_和_左侧_导航样式。

![顶部导航](images/nav-view-header.png)<br/>
_导航视图支持顶部和左侧的导航窗格或菜单_

> **平台 Api**: [Windows.UI.Xaml.Controls.NavigationView 类](/uwp/api/windows.ui.xaml.controls.navigationview)
>
> **Windows UI 库 Api**: [Microsoft.UI.Xaml.Controls.NavigationView 类](/uwp/api/microsoft.ui.xaml.controls.navigationview)
>
> NavigationView，例如_顶部_导航的某些功能需要 Windows 10 版本 1809 ([SDK 17763](https://developer.microsoft.com/windows/downloads/windows-10-sdk)) 或更高版本，或[Windows UI 库](https://docs.microsoft.com/uwp/toolkits/winui/)。

## <a name="is-this-the-right-control"></a>这是正确的控件吗？

NavigationView 处于非常适用于自适应的导航控件：

- 提供一致的导航体验整个应用。
- 保留较小的 windows 上的屏幕空间。
- 组织访问很多导航类别。

有关其他导航模式，请参阅[导航设计基础知识](../basics/navigation-basics.md)。

## <a name="examples"></a>示例

<table>
<th align="left">XAML 控件库<th>
<tr>
<td><img src="images/XAML-controls-gallery-app-icon.png" alt="XAML controls gallery" width="168"></img></td>
<td>
    <p>如果已安装 <strong style="font-weight: semi-bold">XAML 控件库</strong>应用，请单击此处<a href="xamlcontrolsgallery:/item/NavigationView">打开此应用，了解 NavigationView 的实际应用</a>。</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">获取 XAML 控件库应用 (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics">获取源代码 (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="display-modes"></a>显示模式

> PaneDisplayMode 属性需要 Windows 10 版本 1809 ([SDK 17763](https://developer.microsoft.com/windows/downloads/windows-10-sdk)) 或更高版本，或[Windows UI 库](https://docs.microsoft.com/uwp/toolkits/winui/)。

你可以使用 PaneDisplayMode 属性配置不同的导航样式，或为 NavigationView 显示模式。

:::row:::
    :::column:::
    ### <a name="top"></a>Top
    窗格位于上述内容。</br>
    `PaneDisplayMode="Top"`
    :::column-end:::
    :::column span="2":::
    ![顶部导航示例](images/displaymode-top.png)
    :::column-end:::
:::row-end:::

我们建议_顶部_导航时：

- 你有 5 或更少的顶级导航类别的同等重要，并且最终下拉列表中溢出菜单中的类别被视为不太重要的任何其他的顶级导航。
- 你需要显示在屏幕上的所有导航选项。
- 你为应用内容需要更多空间。
- 图标不能清楚地描述你的应用的导航类别。

:::row:::
    :::column:::
    ### <a name="left"></a>向左
    窗格扩展，并且位于左侧的内容。</br>
    `PaneDisplayMode="Left"`
    :::column-end:::
    :::column span="2":::
    ![展开左侧的导航窗格的示例](images/displaymode-left.png)
    :::column-end:::
:::row-end:::

我们建议_左侧_导航时：

- 你有 5-10 同样重要的顶级导航类别。
- 你想要非常重要，与其他应用内容所需的空间更小的导航类别。

:::row:::
    :::column:::
    ### <a name="leftcompact"></a>LeftCompact
    窗格会显示图标直到打开并位于左侧的内容。</br>
    `PaneDisplayMode="LeftCompact"`
    :::column-end:::
    :::column span="2":::
    ![精简的左侧的导航窗格的示例](images/displaymode-leftcompact.png)
    :::column-end:::
:::row-end:::

:::row:::
    :::column:::
    ### <a name="leftminimal"></a>LeftMinimal
    仅菜单按钮会显示直到打开窗格。 打开时，它位于左侧的内容。</br>
    `PaneDisplayMode="LeftMinimal"`
    :::column-end:::
    :::column span="2":::
    ![最小的左侧的导航窗格的示例](images/displaymode-leftminimal.png)
    :::column-end:::
:::row-end:::

### <a name="auto"></a>Auto

默认情况下，PaneDisplayMode 设置为自动。在自动模式中，导航视图适应之间 LeftMinimal 时窗口较窄时，对 LeftCompact，，并且然后保留宽窗口越高。 有关详细信息，请参阅[自适应行为](#adaptive-behavior)部分。

![左侧的导航默认自适应行为](images/displaymode-auto.png)<br/>
_导航视图默认自适应行为_

## <a name="anatomy"></a>结构

这些图像显示窗格中，标头和控件配置的_顶部_或_向左_导航时的内容区域的布局。

![顶部导航视图布局](images/topnav-anatomy.png)<br/>
_顶部导航布局_

![左侧的导航视图布局](images/leftnav-anatomy.png)<br/>
_左侧的导航布局_

### <a name="pane"></a>窗格

你可以使用 PaneDisplayMode 属性放置在内容上方或内容的左侧窗格。

NavigationView 窗格可以包含：

- [NavigationViewItem](/uwp/api/windows.ui.xaml.controls.navigationviewitem)对象。 用于导航到特定页面的导航项。
- [NavigationViewItemSeparator](/uwp/api/windows.ui.xaml.controls.navigationviewitemseparator)对象。 用于对导航项分组的分隔符。 设置为 0 来呈现空间作为分隔符的[Opacity](/uwp/api/windows.ui.xaml.controls.navigationviewitemseparator.opacity)属性。
- [NavigationViewItemHeader](/uwp/api/windows.ui.xaml.controls.navigationviewitemheader)对象。 用于标记项组标头。
- 若要允许应用级别搜索可选[AutoSuggestBox](auto-suggest-box.md)控件。 分配给[NavigationView.AutoSuggestBox](/uwp/api/windows.ui.xaml.controls.navigationview.autosuggestbox)属性的控件。
- [应用设置](../app-settings/app-settings-and-data.md)的可选入口点。 若要隐藏设置项，请将[IsSettingsVisible](/uwp/api/windows.ui.xaml.controls.navigationview.IsSettingsVisible)属性设置为**false**上。

在左的窗格还包含：

- 若要切换窗格的打开和关闭菜单按钮。 打开窗格后在较大的应用窗口中，你可以选择使用 [IsPaneToggleButtonVisible](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview.IsPaneToggleButtonVisible) 属性隐藏此按钮。

导航视图具有放置在窗格左上角的后退按钮。 但是，它不会自动处理向后导航和向后退堆栈中添加内容。 若要启用向后导航，请参阅[向后导航](#backwards-navigation)部分。

下面是详细的窗格结构的顶部和左侧窗格中的位置。

#### <a name="top-navigation-pane"></a>顶部导航窗格

![导航视图顶部窗格解剖学](images/navview-pane-anatomy-horizontal.png)

1. 标题
1. 导航项目
1. 分隔符
1. AutoSuggestBox （可选）
1. 设置按钮 （可选）

#### <a name="left-navigation-pane"></a>在左侧的导航窗格

![导航视图左窗格解剖学](images/navview-pane-anatomy-vertical.png)

1. “菜单”按钮
1. 导航项目
1. 分隔符
1. 标题
1. AutoSuggestBox （可选）
1. 设置按钮 （可选）

#### <a name="pane-footer"></a>窗格页脚

你可以自由格式内容放置在窗格页脚中，通过将其添加到[PaneFooter](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview.PaneFooter)属性。

:::row:::
    :::column:::
    ![窗格页脚顶部导航](images/navview-freeform-footer-top.png)<br>
     _顶部窗格页脚_<br>
    :::column-end:::
    :::column:::
    ![窗格页脚左侧的导航](images/navview-freeform-footer-left.png)<br>
    _左的窗格页脚_<br>
    :::column-end:::
:::row-end:::

#### <a name="pane-title-and-header"></a>窗格标题和标头

你可以通过设置[PaneTitle](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview.PaneTitle)窗格标头区域中放置文本内容。 它采用字符串，并显示菜单按钮旁边的文本。

若要添加非文本内容，如图像或徽标，你可以通过将其添加到[PaneHeader](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview.PaneHeader)属性窗格的标头中放置任何元素。

设置 PaneTitle 和 PaneHeader，如果内容是水平堆叠菜单按钮与 PaneTitle 接近菜单按钮旁边。

:::row:::
    :::column:::
    ![窗格标头顶部导航](images/navview-freeform-header-top.png)<br>
     _顶部窗格标头_<br>
    :::column-end:::
    :::column:::
    ![窗格标头左侧的导航](images/navview-freeform-header-left.png)<br>
    _左窗格中的标头_<br>
    :::column-end:::
:::row-end:::

#### <a name="pane-content"></a>窗格内容

你可以通过将其添加到[PaneCustomContent](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview.PaneCustomContent)属性自由格式内容放置在窗格中。

:::row:::
    :::column:::
    ![窗格自定义内容顶部导航](images/navview-freeform-pane-top.png)<br>
     _顶部窗格自定义内容_<br>
    :::column-end:::
    :::column:::
    ![窗格自定义内容向左导航](images/navview-freeform-pane-left.png)<br>
    _左窗格中自定义内容_<br>
    :::column-end:::
:::row-end:::

### <a name="header"></a>标题

你可以通过设置[Header](/uwp/api/windows.ui.xaml.controls.navigationview.header)属性添加页面标题。

![导航视图标题区域的示例](images/nav-header.png)<br/>
_导航视图标头_

标题区域与在左窗格中的位置中，导航按钮纵向对齐，位于下方的顶部窗格位置的窗格。 它具有固定的高度 52 px。 它的用途是保存所选导航类别的页面标题。 标题停靠在页面顶部，并作为内容区域的滚动剪辑点。

标头是可见随时 NavigationView 处于最小的显示模式。 你可以选择在其他模式下隐藏标题，这样可以增大窗口宽度。 若要隐藏标头，设置为**false** [AlwaysShowHeader](/uwp/api/windows.ui.xaml.controls.navigationview.AlwaysShowHeader)属性。

### <a name="content"></a>内容

![导航视图内容区域的示例](images/nav-content.png)<br/>
_导航视图内容_

内容区域是显示所选导航类别的大部分信息的位置。

否则我们建议在 NavigationView 处于**最小**模式时对内容区域 12px 边距，并使用 24px 边距。

## <a name="adaptive-behavior"></a>自适应行为

默认情况下，导航视图自动更改其显示模式，具体取决于其可用的屏幕空间量。 [CompactModeThresholdWidth](/uwp/api/windows.ui.xaml.controls.navigationview.compactmodethresholdwidth)和[ExpandedModeThresholdWidth](/uwp/api/windows.ui.xaml.controls.navigationview.expandedmodethresholdwidth)属性指定的显示模式更改的断点。 你可以修改这些值以自定义的自适应显示模式行为。

### <a name="default"></a>默认值

当 PaneDisplayMode 设置为**自动**其默认值时，自适应行为是显示：

- 展开的左侧窗格中较大的窗口宽度 (1008px 或更高版本)。
- 向左、 仅图标的导航窗格 (LeftCompact) 中等大小的窗口宽度 (641px 到 1007px)。
- 仅菜单上的按钮 (LeftMinimal) 较小的窗口宽度 (640px 或更少)。

有关自适应行为的窗口大小的详细信息，请参阅[屏幕大小和断点](../layout/screen-sizes-and-breakpoints-for-responsive-design.md)。

![左侧的导航默认自适应行为](images/displaymode-auto.png)<br/>
_导航视图默认自适应行为_

### <a name="minimal"></a>最小

第二个常见的自适应模式是在较大的窗口宽度，并仅在这两个中小型窗口宽度的菜单按钮使用展开的左侧窗格。

我们建议此时：

- 你为较小的窗口宽度的应用内容需要更多空间。
- 导航类别不清楚地表示带图标。

![左侧的导航最小自适应行为](images/adaptive-behavior-minimal.png)<br/>
_导航视图"最小"自适应行为_

若要配置此行为，将设置为折叠的窗格的宽度的 CompactModeThresholdWidth。 此处，它将更改默认值为 640 到 1007年。 你还应该设置 ExpandedModeThresholdWidth 以确保不会发生冲突的值。

```xaml
<NavigationView CompactModeThresholdWidth="1007" ExpandedModeThresholdWidth="1007"/>
```

### <a name="compact"></a>精简

第三个常见的自适应模式是在较大的窗口宽度和 LeftCompact，仅图标，这两个中小型窗口宽度的导航窗格上使用展开的左侧窗格。

我们建议此时：

- 请务必始终显示在屏幕上的所有导航选项。
- 导航类别可以清楚地表示带图标。

![左侧的导航压缩自适应行为](images/adaptive-behavior-compact.png)<br/>
_导航视图"压缩"自适应行为_

若要配置此行为，请将 CompactModeThresholdWidth 设置为 0。

```xaml
<NavigationView CompactModeThresholdWidth="0"/>
```

### <a name="no-adaptive-behavior"></a>任何自适应行为

若要禁用自动自适应行为，设置为自动以外的值 PaneDisplayMode。在这里，设置 LeftMinimal，这样只有菜单按钮将显示无论窗口宽度。

![不左侧导航栏任何自适应行为](images/adaptive-behavior-none.png)<br/>
_导航视图，内含 PaneDisplayMode 设置为 LeftMinimal_

```xaml
<NavigationView PaneDisplayMode="LeftMinimal" />
```

如之前所述的_显示模式_部分中，你可以设置为始终在顶部、 始终展开，始终紧凑，或始终最少的窗格。 你还可以管理的显示模式自己应用代码中。 此示例显示在下一节。

### <a name="top-to-left-navigation"></a>上向左导航

当你在应用中使用顶部导航时，导航项折叠到溢出菜单为窗口宽度降低。 你的应用的窗口较窄时，它可以提供更好的用户体验，若要切换到 LeftMinimal 导航，而不是让所有这些项目折叠到溢出菜单 PaneDisplayMode 从顶部。

我们建议在较大窗口尺寸和小型上的左侧的导航中使用顶部导航窗口大小时：

- 必须一套同等重要顶级导航类别显示在一起，以便在屏幕上不符合此设置中的一个类别，如果你折叠到左侧的导航，以便为他们提供同样重要。
- 你想要保留为更内容的空间，较小的窗口大小。

此示例显示了如何使用[VisualStateManager](/uwp/api/Windows.UI.Xaml.VisualStateManager)和[AdaptiveTrigger.MinWindowWidth](/uwp/api/windows.ui.xaml.adaptivetrigger.minwindowwidth)属性顶部和 LeftMinimal 导航之间进行切换。

![边缘或左边自适应行为 1 的示例](images/navigation-top-to-left.png)

```xaml
<Grid >
    <NavigationView x:Name="NavigationViewControl" >
        <NavigationView.MenuItems>
            <NavigationViewItem Content="A" x:Name="A" />
            <NavigationViewItem Content="B" x:Name="B" />
            <NavigationViewItem Content="C" x:Name="C" />
        </NavigationView.MenuItems>
    </NavigationView>

    <VisualStateManager.VisualStateGroups>
        <VisualStateGroup>
            <VisualState>
                <VisualState.StateTriggers>
                    <AdaptiveTrigger
                        MinWindowWidth="{x:Bind NavigationViewControl.CompactModeThresholdWidth}" />
                </VisualState.StateTriggers>

                <VisualState.Setters>
                    <Setter Target="NavigationViewControl.PaneDisplayMode" Value="Top"/>
                </VisualState.Setters>
            </VisualState>
        </VisualStateGroup>
    </VisualStateManager.VisualStateGroups>
</Grid>

```

> [!TIP]
> 当你使用 AdaptiveTrigger.MinWindowWidth 时，在窗口宽度超过指定的最小宽度时触发的视觉状态。 这意味着默认的 XAML 定义的较窄的窗口中，并且 VisualState 定义了哪些修改，当窗口变得更广泛的应用。 默认 PaneDisplayMode 导航视图是自动，因此当窗口宽度小于或等于 CompactModeThresholdWidth，LeftMinimal 导航时使用。 当窗口宽度时，VisualState 替代默认值，并使用顶部导航。

## <a name="navigation"></a>导航

导航视图不会自动执行任何导航任务。 当用户点击的导航项时，导航视图将显示为已选中该项目，并引发[ItemInvoked](/uwp/api/windows.ui.xaml.controls.navigationview.ItemInvoked)事件。 如果点击导致新项被选中，也会引发[SelectionChanged](/uwp/api/windows.ui.xaml.controls.navigationview.SelectionChanged)事件。

你可以处理这两个事件来执行与请求的导航相关的任务。 哪一个你应处理取决于你希望为你的应用的行为。 通常情况下，导航到请求的页面，并更新以响应这些事件的导航视图标头。

即使已选择的只要用户点击的导航项，就会引发**ItemInvoked** 。 （该项目，也可以调用与使用鼠标、 键盘或其他输入等效操作。 有关详细信息，请参阅[输入和交互](../input/index.md)。）如果你导航 ItemInvoked 处理程序中，默认情况下，将重新加载页面，并重复项添加到导航堆栈。 如果导航项目被调用时，你应该禁止重新加载该页面，或确保重复项不创建导航 backstack 中重新加载页面时。 （请参阅代码示例）。

通过调用项目不是当前所选用户或者以编程方式更改所选的项目时，会引发**SelectionChanged** 。 如果选择更改是由于用户调用某项发生，首先发生 ItemInvoked 事件。 如果以编程方式选择更改，则不会引发 ItemInvoked。

### <a name="backwards-navigation"></a>向后导航

NavigationView 具有内置的后退按钮;但是，与前进导航，它不会向后导航自动执行。 当用户点击后退按钮时，会引发的[BackRequested](/uwp/api/windows.ui.xaml.controls.navigationview.BackRequested)事件。 处理此事件以执行向后导航。 有关详细信息和代码示例，请参阅[导航历史记录和向后导航](../basics/navigation-history-and-backwards-navigation.md)。

在最小或精简模式中，导航视图窗格已打开作为浮出控件。 在此情况下，单击后退按钮将关闭窗格，将改为引发**PaneClosing**事件。

你可以隐藏或禁用后退按钮设置这些属性：

- [IsBackButtonVisible](/uwp/api/windows.ui.xaml.controls.navigationview.IsBackButtonVisible)： 用于显示和隐藏后退按钮。 此属性采用[NavigationViewBackButtonVisible](/uwp/api/windows.ui.xaml.controls.navigationviewbackbuttonvisible)枚举的值，默认设置为**自动**。 当折叠按钮时，没有空间为其保留在布局中。
- [IsBackEnabled](/uwp/api/windows.ui.xaml.controls.navigationview.IsBackEnabled)： 用于启用或禁用后退按钮。 你可以数据绑定到[CanGoBack](/uwp/api/windows.ui.xaml.controls.frame.cangoback)属性导航帧的此属性。 如果**IsBackEnabled**为**false**，则不会引发**BackRequested** 。

:::row:::
    :::column:::
        ![Navigation view back button in the left navigation pane](images/leftnav-back.png)<br/>
        _The back button in the left navigation pane_
    :::column-end:::
    :::column:::
        ![Navigation view back button in the top navigation pane](images/topnav-back.png)<br/>
        _The back button in the top navigation pane_
    :::column-end:::
:::row-end:::

## <a name="code-example"></a>代码示例

此示例显示了如何使用 NavigationView 具有较大的窗口大小的顶部导航窗格和较小的窗口大小的左侧的导航窗格。 它可以适应仅向左导航通过 VisualStateManager 中删除的_顶部_导航设置。

该示例演示了设置将适用于许多常见场景的导航数据的推荐的方法。 它还演示了如何实现向后导航使用 NavigationView 的后退按钮和键盘导航。

此代码假设你的应用包含具有以下名称以导航到页面：_主页_、 _AppsPage_、 _GamesPage_、 _MusicPage_、 _MyContentPage_，以及_SettingsPage_。 有关这些页面的代码不会显示。

> [!IMPORTANT]
> 有关应用的页面信息存储在[ValueTuple](https://docs.microsoft.com/dotnet/api/system.valuetuple)。 此结构要求你的应用项目的最低版本必须为 SDK 17763 或更高版本。 如果你使用 NavigationView 的 WinUI 版本面向较早版本的 Windows 10，你可以改为使用[System.ValueTuple NuGet 程序包](https://www.nuget.org/packages/System.ValueTuple/)。

> [!IMPORTANT]
> 此代码演示如何使用 NavigationView 的[Windows UI 库](https://docs.microsoft.com/uwp/toolkits/winui/)版本。 如果你改为使用 NavigationView 的平台版本，你的应用项目的最低版本必须 17763 或更高版本的 SDK。 若要使用的平台版本，删除对所有引用`muxc:`。

```xaml
<!-- xmlns:muxc="using:Microsoft.UI.Xaml.Controls" -->
<Grid>
    <muxc:NavigationView x:Name="NavView"
                         Loaded="NavView_Loaded"
                         ItemInvoked="NavView_ItemInvoked"
                         BackRequested="NavView_BackRequested">
        <muxc:NavigationView.MenuItems>
            <muxc:NavigationViewItem Tag="home" Icon="Home" Content="Home"/>
            <muxc:NavigationViewItemSeparator/>
            <muxc:NavigationViewItemHeader x:Name="MainPagesHeader"
                                           Content="Main pages"/>
            <muxc:NavigationViewItem Tag="apps" Content="Apps">
                <muxc:NavigationViewItem.Icon>
                    <FontIcon FontFamily="Segoe MDL2 Assets" Glyph="&#xEB3C;"/>
                </muxc:NavigationViewItem.Icon>
            </muxc:NavigationViewItem>
            <muxc:NavigationViewItem Tag="games" Content="Games">
                <muxc:NavigationViewItem.Icon>
                    <FontIcon FontFamily="Segoe MDL2 Assets" Glyph="&#xE7FC;"/>
                </muxc:NavigationViewItem.Icon>
            </muxc:NavigationViewItem>
            <muxc:NavigationViewItem Tag="music" Icon="Audio" Content="Music"/>
        </muxc:NavigationView.MenuItems>

        <muxc:NavigationView.AutoSuggestBox>
            <!-- See AutoSuggestBox documentation for
                 more info about how to implement search. -->
            <AutoSuggestBox x:Name="NavViewSearchBox" QueryIcon="Find"/>
        </muxc:NavigationView.AutoSuggestBox>

        <ScrollViewer>
            <Frame x:Name="ContentFrame" Padding="12,0,12,24" IsTabStop="True"
                   NavigationFailed="ContentFrame_NavigationFailed"/>
        </ScrollViewer>
    </muxc:NavigationView>

    <VisualStateManager.VisualStateGroups>
        <VisualStateGroup>
            <VisualState>
                <VisualState.StateTriggers>
                    <AdaptiveTrigger
                        MinWindowWidth="{x:Bind NavView.CompactModeThresholdWidth}"/>
                </VisualState.StateTriggers>
                <VisualState.Setters>
                    <!-- Remove the next 3 lines for left-only navigation. -->
                    <Setter Target="NavView.PaneDisplayMode" Value="Top"/>
                    <Setter Target="NavViewSearchBox.Width" Value="200"/>
                    <Setter Target="MainPagesHeader.Visibility" Value="Collapsed"/>
                    <!-- Leave the next line for left-only navigation. -->
                    <Setter Target="ContentFrame.Padding" Value="24,0,24,24"/>
                </VisualState.Setters>
            </VisualState>
        </VisualStateGroup>
    </VisualStateManager.VisualStateGroups>
</Grid>
```

> [!IMPORTANT]
> 此代码演示如何使用 NavigationView 的[Windows UI 库](https://docs.microsoft.com/uwp/toolkits/winui/)版本。 如果你改为使用 NavigationView 的平台版本，你的应用项目的最低版本必须 17763 或更高版本的 SDK。 若要使用的平台版本，删除对所有引用`muxc`。

```csharp
// Add "using" for WinUI controls.
// using muxc = Microsoft.UI.Xaml.Controls;

private void ContentFrame_NavigationFailed(object sender, NavigationFailedEventArgs e)
{
    throw new Exception("Failed to load Page " + e.SourcePageType.FullName);
}

// List of ValueTuple holding the Navigation Tag and the relative Navigation Page
private readonly List<(string Tag, Type Page)> _pages = new List<(string Tag, Type Page)>
{
    ("home", typeof(HomePage)),
    ("apps", typeof(AppsPage)),
    ("games", typeof(GamesPage)),
    ("music", typeof(MusicPage)),
};

private void NavView_Loaded(object sender, RoutedEventArgs e)
{
    // You can also add items in code.
    NavView.MenuItems.Add(new muxc.NavigationViewItemSeparator());
    NavView.MenuItems.Add(new muxc.NavigationViewItem
    {
        Content = "My content",
        Icon = new SymbolIcon((Symbol)0xF1AD),
        Tag = "content"
    });
    _pages.Add(("content", typeof(MyContentPage)));

    // Add handler for ContentFrame navigation.
    ContentFrame.Navigated += On_Navigated;

    // NavView doesn't load any page by default, so load home page.
    NavView.SelectedItem = NavView.MenuItems[0];
    // If navigation occurs on SelectionChanged, this isn't needed.
    // Because we use ItemInvoked to navigate, we need to call Navigate
    // here to load the home page.
    NavView_Navigate("home", new EntranceNavigationTransitionInfo());

    // Add keyboard accelerators for backwards navigation.
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

private void NavView_ItemInvoked(muxc.NavigationView sender,
                                 muxc.NavigationViewItemInvokedEventArgs args)
{
    if (args.IsSettingsInvoked == true)
    {
        NavView_Navigate("settings", args.RecommendedNavigationTransitionInfo);
    }
    else if (args.InvokedItemContainer != null)
    {
        var navItemTag = args.InvokedItemContainer.Tag.ToString();
        NavView_Navigate(navItemTag, args.RecommendedNavigationTransitionInfo);
    }
}

<!-- NavView_SelectionChanged is not used in this example, but is shown for completeness.
     You will typically handle either ItemInvoked or SelectionChanged to perform navigation,
     but not both. -->
private void NavView_SelectionChanged(muxc.NavigationView sender,
                                      muxc.NavigationViewSelectionChangedEventArgs args)
{
    if (args.IsSettingsSelected == true)
    {
        NavView_Navigate("settings", args.RecommendedNavigationTransitionInfo);
    }
    else if (args.SelectedItemContainer != null)
    {
        var navItemTag = args.SelectedItemContainer.Tag.ToString();
        NavView_Navigate(navItemTag, args.RecommendedNavigationTransitionInfo);
    }
}

private void NavView_Navigate(string navItemTag, NavigationTransitionInfo transitionInfo)
{
    Type _page = null;
    if (navItemTag == "settings")
    {
        _page = typeof(SettingsPage);
    }
    else
    {
        var item = _pages.FirstOrDefault(p => p.Tag.Equals(navItemTag));
        _page = item.Page;
    }
    // Get the page type before navigation so you can prevent duplicate
    // entries in the backstack.
    var preNavPageType = ContentFrame.CurrentSourcePageType;

    // Only navigate if the selected page isn't currently loaded.
    if (!(_page is null) && !Type.Equals(preNavPageType, _page))
    {
        ContentFrame.Navigate(_page, null, transitionInfo);
    }
}

private void NavView_BackRequested(muxc.NavigationView sender,
                                   muxc.NavigationViewBackRequestedEventArgs args)
{
    On_BackRequested();
}

private void BackInvoked(KeyboardAccelerator sender,
                         KeyboardAcceleratorInvokedEventArgs args)
{
    On_BackRequested();
    args.Handled = true;
}

private bool On_BackRequested()
{
    if (!ContentFrame.CanGoBack)
        return false;

    // Don't go back if the nav pane is overlayed.
    if (NavView.IsPaneOpen &&
        (NavView.DisplayMode == muxc.NavigationViewDisplayMode.Compact ||
         NavView.DisplayMode == muxc.NavigationViewDisplayMode.Minimal))
        return false;

    ContentFrame.GoBack();
    return true;
}

private void On_Navigated(object sender, NavigationEventArgs e)
{
    NavView.IsBackEnabled = ContentFrame.CanGoBack;

    if (ContentFrame.SourcePageType == typeof(SettingsPage))
    {
        // SettingsItem is not part of NavView.MenuItems, and doesn't have a Tag.
        NavView.SelectedItem = (muxc.NavigationViewItem)NavView.SettingsItem;
        NavView.Header = "Settings";
    }
    else if (ContentFrame.SourcePageType != null)
    {
        var item = _pages.FirstOrDefault(p => p.Page == e.SourcePageType);

        NavView.SelectedItem = NavView.MenuItems
            .OfType<muxc.NavigationViewItem>()
            .First(n => n.Tag.Equals(item.Tag));

        NavView.Header =
            ((muxc.NavigationViewItem)NavView.SelectedItem)?.Content?.ToString();
    }
}
```

## <a name="navigation-view-customization"></a>导航视图自定义

### <a name="pane-backgrounds"></a>窗格背景

默认情况下，NavigationView 窗格使用不同的背景，具体取决于显示模式：

- 该窗格是灰色纯色展开左侧，（在左侧的模式下） 的内容与并行时。
- 窗格以覆盖形式 （在顶部、 最小或精简模式下） 的内容顶部使用时打开的应用内亚克力。

若要修改窗格背景，你可以替代用于呈现每个模式中的背景的 XAML 主题资源。 （此技术使用而不是单个 PaneBackground 属性才可支持不同的背景为不同的显示模式。）

此表显示每个显示模式中使用哪个主题资源。

| 显示模式 | 主题资源 |
| ------------ | -------------- |
| 向左 | NavigationViewExpandedPaneBackground |
| LeftCompact<br/>LeftMinimal | NavigationViewDefaultPaneBackground |
| Top | NavigationViewTopPaneBackground |

此示例显示了如何替代在 App.xaml 中的主题资源。 覆盖主题资源后，你应该始终提供"Default"和"高对比度"资源字典中的至少和字典"浅色"或"深色"资源根据需要。 有关详细信息，请参阅[ResourceDictionary.ThemeDictionaries](/uwp/api/windows.ui.xaml.resourcedictionary.themedictionaries)。

> [!IMPORTANT]
> 此代码演示如何使用[Windows UI 库](https://docs.microsoft.com/uwp/toolkits/winui/)版本的 AcrylicBrush。 如果你使用的是 AcrylicBrush 的平台版本，你的应用项目的最低版本必须是 SDK 16299 或更高版本。 若要使用的平台版本，删除对所有引用`muxm:`。

```xaml
<Application
    <!-- ... -->
    xmlns:muxm="using:Microsoft.UI.Xaml.Media">
    <Application.Resources>
        <ResourceDictionary>
            <ResourceDictionary.MergedDictionaries>
                <XamlControlsResources xmlns="using:Microsoft.UI.Xaml.Controls"/>
                <ResourceDictionary>
                    <ResourceDictionary.ThemeDictionaries>
                        <ResourceDictionary x:Key="Default">
                            <!-- The "Default" theme dictionary is used unless a specific
                                 light, dark, or high contrast dictionary is provided. These
                                 resources should be tested with both the light and dark themes,
                                 and specific light or dark resources provided as needed. -->
                            <muxm:AcrylicBrush x:Key="NavigationViewDefaultPaneBackground"
                                   BackgroundSource="Backdrop"
                                   TintColor="LightSlateGray"
                                   TintOpacity=".6"/>
                            <muxm:AcrylicBrush x:Key="NavigationViewTopPaneBackground"
                                   BackgroundSource="Backdrop"
                                   TintColor="{ThemeResource SystemAccentColor}"
                                   TintOpacity=".6"/>
                            <LinearGradientBrush x:Key="NavigationViewExpandedPaneBackground"
                                     StartPoint="0.5,0" EndPoint="0.5,1">
                                <GradientStop Color="LightSlateGray" Offset="0.0" />
                                <GradientStop Color="White" Offset="1.0" />
                            </LinearGradientBrush>
                        </ResourceDictionary>
                        <ResourceDictionary x:Key="HighContrast">
                            <!-- Always include a "HighContrast" dictionary when you override
                                 theme resources. This empty dictionary ensures that the 
                                 default high contrast resources are used when the user
                                 turns on high contrast mode. -->
                        </ResourceDictionary>
                    </ResourceDictionary.ThemeDictionaries>
                </ResourceDictionary>
            </ResourceDictionary.MergedDictionaries>
        </ResourceDictionary>
    </Application.Resources>
</Application>
```

## <a name="related-topics"></a>相关主题

- [NavigationView 类](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview)
- [大纲/细节](master-details.md)
- [导航基础知识](../basics/navigation-basics.md)
- [UWP 的 Fluent Design 概述](../fluent-design-system/index.md)