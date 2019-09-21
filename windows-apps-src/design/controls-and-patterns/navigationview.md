---
Description: NavigationView 是为应用实现顶级导航模式的自适应控件。
title: 导航视图
template: detail.hbs
ms.date: 10/02/2018
ms.topic: article
keywords: windows 10, uwp
pm-contact: yulikl
design-contact: kimsea
dev-contact: ''
doc-status: Published
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: c0d12b3b043546cd908fb474fa8ca9656d8dc56e
ms.sourcegitcommit: bac5574a1f47a5b38c984a5482272c9e49a9c91e
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/18/2019
ms.locfileid: "71100853"
---
# <a name="navigation-view"></a>导航视图

NavigationView 控件可为应用提供顶级导航。 它适应各种屏幕大小，支持顶部  和左侧  导航样式。

![顶部导航](images/nav-view-header.png)<br/>
导航视图支持顶部和左侧导航窗格或菜单 

> 平台 API  ：[Windows.UI.Xaml.Controls.NavigationView 类](/uwp/api/windows.ui.xaml.controls.navigationview)
>
> Windows UI 库 API  ：[Microsoft.UI.Xaml.Controls.NavigationView 类](/uwp/api/microsoft.ui.xaml.controls.navigationview)
>
> NavigationView 的某些功能（如顶部  导航）需要 Windows 10 版本 1809 ([SDK 17763](https://developer.microsoft.com/windows/downloads/windows-10-sdk)) 或更高版本，或是 [Windows UI 库](https://docs.microsoft.com/uwp/toolkits/winui/)。

## <a name="is-this-the-right-control"></a>这是正确的控件吗？

NavigationView 是非常适合于以下功能的自适应导航控件：

- 在整个应用中提供一致的导航体验。
- 在较小窗口上保留屏幕空间。
- 组织对许多导航类别的访问。

有关其他导航模式，请参阅[导航设计基础知识](../basics/navigation-basics.md)。

## <a name="examples"></a>示例

<table>
<th align="left">XAML 控件库<th>
<tr>
<td><img src="images/XAML-controls-gallery-app-icon.png" alt="XAML controls gallery" width="168"></img></td>
<td>
    <p>如果已安装 <strong style="font-weight: semi-bold">XAML 控件库</strong>应用，请单击此处<a href="xamlcontrolsgallery:/item/NavigationView">打开此应用，了解 NavigationView 的实际应用</a>。</p>
    <ul>
    <li><a href="https://www.microsoft.com/p/xaml-controls-gallery/9msvh128x2zt">获取 XAML 控件库应用 (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">获取源代码 (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="display-modes"></a>显示模式

> PaneDisplayMode 属性需要 Windows 10 版本 1809 ([SDK 17763](https://developer.microsoft.com/windows/downloads/windows-10-sdk)) 或更高版本，或是 [Windows UI 库](https://docs.microsoft.com/uwp/toolkits/winui/)。

可以使用 PaneDisplayMode 属性配置不同的导航样式或显示模式（对于 NavigationView）。

:::row:::
    :::column:::
    ### <a name="top"></a>顶部
    窗格位于内容上方。</br>
    `PaneDisplayMode="Top"`
    :::column-end:::
    :::column span="2":::
    ![顶部导航的示例](images/displaymode-top.png)
    :::column-end:::
:::row-end:::

在以下情况下建议使用顶部  导航：

- 具有 5 个或更少的同等重要顶级导航类别，最终采用下拉溢出菜单中的任何其他顶级导航类别都被视为不太重要。
- 需要在屏幕上显示所有导航选项。
- 要为应用内容提供更多空间。
- 图标无法清楚地描述应用的导航类别。

:::row:::
    :::column:::
    ### <a name="left"></a>向左
    窗格会展开并位于内容左侧。</br>
    `PaneDisplayMode="Left"`
    :::column-end:::
    :::column span="2":::
    ![展开的左侧导航窗格示例](images/displaymode-left.png)
    :::column-end:::
:::row-end:::

在以下情况下建议使用左侧  导航：

- 具有 5-10 个同等重要的顶级导航类别。
- 希望导航类别非常突出，而其他应用内容的空间较少。

:::row:::
    :::column:::
    ### <a name="leftcompact"></a>LeftCompact
    窗格在打开之前仅显示图标，位于内容左侧。</br>
    `PaneDisplayMode="LeftCompact"`
    :::column-end:::
    :::column span="2":::
    ![精简左侧导航窗格示例](images/displaymode-leftcompact.png)
    :::column-end:::
:::row-end:::

:::row:::
    :::column:::
    ### <a name="leftminimal"></a>LeftMinimal
    窗格打开之前仅显示菜单按钮。 打开时，它位于内容左侧。</br>
    `PaneDisplayMode="LeftMinimal"`
    :::column-end:::
    :::column span="2":::
    ![最小左侧导航窗格示例](images/displaymode-leftminimal.png)
    :::column-end:::
:::row-end:::

### <a name="auto"></a>自动

默认情况下，PaneDisplayMode 设置为 Auto。在 Auto 模式下，导航视图会进行自适应，在窗口狭窄时为 LeftMinimal，接下来为 LeftCompact，随后在窗口变宽时为 Left。 有关更多信息，请参阅[自适应行为](#adaptive-behavior)部分。

![左侧导航默认自适应行为](images/displaymode-auto.png)<br/>
导航视图默认自适应行为 

## <a name="anatomy"></a>结构

这些图像显示为顶部  或左侧  导航进行配置时，控件窗格、标题和内容区域的布局。

![顶部导航视图布局](images/topnav-anatomy.png)<br/>
顶部导航布局 

![左侧导航视图布局](images/leftnav-anatomy.png)<br/>
左侧导航布局 

### <a name="pane"></a>窗格

可以使用 PaneDisplayMode 属性将窗格置于内容上方或内容左侧。

NavigationView 窗格可以包含：

- [NavigationViewItem](/uwp/api/windows.ui.xaml.controls.navigationviewitem) 对象。 用于导航到特定页面的导航项。
- [NavigationViewItemSeparator](/uwp/api/windows.ui.xaml.controls.navigationviewitemseparator) 对象。 用于对导航项进行分组的分隔器。 将 [Opacity](/uwp/api/windows.ui.xaml.uielement.opacity) 属性设置为 0 可将分隔器呈现为空间。
- [NavigationViewItemHeader](/uwp/api/windows.ui.xaml.controls.navigationviewitemheader) 对象。 用于标记项组的标题。
- 允许应用级别搜索的可选 [AutoSuggestBox](auto-suggest-box.md) 控件。 可将该控件分配给 [NavigationView.AutoSuggestBox](/uwp/api/windows.ui.xaml.controls.navigationview.autosuggestbox) 属性。
- [应用设置](../app-settings/app-settings-and-data.md)的可选入口点。 若要隐藏设置项，请将 [IsSettingsVisible](/uwp/api/windows.ui.xaml.controls.navigationview.IsSettingsVisible) 属性设置为 false  。

左窗格还包含：

- 用于切换窗格打开和关闭状态的菜单按钮。 打开窗格后在较大的应用窗口中，你可以选择使用 [IsPaneToggleButtonVisible](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview.IsPaneToggleButtonVisible) 属性隐藏此按钮。

导航视图具有置于窗格左上角的后退按钮。 但是，它不会自动处理向后导航以及将内容添加到 Backstack。 若要启用向后导航，请参阅[向后导航](#backwards-navigation)部分。

下面是顶部和左侧窗格位置的详细窗格结构。

#### <a name="top-navigation-pane"></a>顶部导航窗格

![导航视图顶部窗格结构](images/navview-pane-anatomy-horizontal.png)

1. 标头
1. 导航项
1. 分隔器
1. AutoSuggestBox（可选）
1. 设置按钮（可选）

#### <a name="left-navigation-pane"></a>左侧导航窗格

![导航视图左侧窗格结构](images/navview-pane-anatomy-vertical.png)

1. “菜单”按钮
1. 导航项
1. 分隔器
1. 标头
1. AutoSuggestBox（可选）
1. 设置按钮（可选）

#### <a name="pane-footer"></a>窗格页脚

可以通过将自由格式内容添加到 [PaneFooter](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview.PaneFooter) 属性，将该内容置于窗格页脚中。

:::row:::
    :::column:::
    ![窗格页脚顶部导航](images/navview-freeform-footer-top.png)<br>
     顶部窗格页脚 <br>
    :::column-end:::
    :::column:::
    ![窗格页脚左侧导航](images/navview-freeform-footer-left.png)<br>
    左侧窗格页脚 <br>
    :::column-end:::
:::row-end:::

#### <a name="pane-title-and-header"></a>窗格标题和页眉

可以通过设置 [PaneTitle](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview.PaneTitle) 属性，在窗格标题区域中放置文本内容。 它采用一个字符串，在菜单按钮旁显示文本。

若要添加非文本内容（如图像或徽标），可以通过将任何元素添加到 [PaneHeader](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview.PaneHeader) 属性来置于窗格标题中。

如果同时设置了 PaneTitle 和 PaneHeader，则内容会在菜单按钮旁水平堆叠（PaneTitle 最接近菜单按钮）。

:::row:::
    :::column:::
    ![窗格标题顶部导航](images/navview-freeform-header-top.png)<br>
     顶部窗格标题 <br>
    :::column-end:::
    :::column:::
    ![窗格标题左侧导航](images/navview-freeform-header-left.png)<br>
    左侧窗格标题 <br>
    :::column-end:::
:::row-end:::

#### <a name="pane-content"></a>窗格内容

可以通过将自由格式内容添加到 [PaneCustomContent](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview.PaneCustomContent) 属性，将该内容置于窗格中。

:::row:::
    :::column:::
    ![窗格自定义内容顶部导航](images/navview-freeform-pane-top.png)<br>
     顶部窗格自定义内容 <br>
    :::column-end:::
    :::column:::
    ![窗格自定义内容左侧导航](images/navview-freeform-pane-left.png)<br>
    左侧窗格自定义内容 <br>
    :::column-end:::
:::row-end:::

### <a name="header"></a>标头

可以通过设置 [Header](/uwp/api/windows.ui.xaml.controls.navigationview.header) 属性来添加页面标题。

![导航视图标题区域示例](images/nav-header.png)<br/>
导航视图标题 

标题区域在左侧窗格位置中与导航按钮垂直对齐，在顶部窗格位置中位于窗格下方。 它具有固定高度 52 px。 它的用途是保存所选导航类别的页面标题。 标题停靠在页面顶部，并作为内容区域的滚动剪辑点。

只要 NavigationView 处于最小显示模式下，标题便可见。 你可以选择在其他模式下隐藏标题，这样可以增大窗口宽度。 若要隐藏标题，请将 [AlwaysShowHeader](/uwp/api/windows.ui.xaml.controls.navigationview.AlwaysShowHeader) 属性设置为 False  。

### <a name="content"></a>内容

![导航视图内容区域示例](images/nav-content.png)<br/>
导航视图内容 

内容区域是显示所选导航类别的大部分信息的位置。

建议在 NavigationView 处于最小  模式时对内容区域使用 12px 边距，其他情况使用 24px 边距。

## <a name="adaptive-behavior"></a>自适应行为

默认情况下，导航视图会根据可用的屏幕空间自动更改显示模式。 [CompactModeThresholdWidth](/uwp/api/windows.ui.xaml.controls.navigationview.compactmodethresholdwidth) 和 [ExpandedModeThresholdWidth](/uwp/api/windows.ui.xaml.controls.navigationview.expandedmodethresholdwidth) 属性指定显示模式更改的断点。 可以修改这些值以自定义自适应显示模式行为。

### <a name="default"></a>默认

当 PaneDisplayMode 设置为其默认值 Auto  时，自适应行为是：

- 在较大窗口宽度（1008px 或更大）上显示展开的左窗格。
- 在中等窗格宽度（641px 到 1007px）上显示仅限图标的左侧导航窗格 (LeftCompact)。
- 在较小窗口宽度（640px 或更小）上仅显示菜单按钮 (LeftMinimal)。

有关自适应行为的窗口大小的更多信息，请参阅[屏幕大小和断点](../layout/screen-sizes-and-breakpoints-for-responsive-design.md)。

![左侧导航默认自适应行为](images/displaymode-auto.png)<br/>
导航视图默认自适应行为 

### <a name="minimal"></a>最小

第二种常见自适应模式是在较大窗口宽度上使用展开的左侧窗格，在中等和较小窗口宽度上仅使用菜单按钮。

在以下情况下建议使用此模式：

- 在较小窗口宽度上需要将更多空间用于应用内容。
- 无法使用图标清楚地表示导航类别。

![左侧导航最小自适应行为](images/adaptive-behavior-minimal.png)<br/>
导航视图“最小”自适应行为 

若要配置此行为，请将 CompactModeThresholdWidth 设为要折叠窗格时的宽度。 在这里，它从默认值 640 更改为 1007。 还应设置 ExpandedModeThresholdWidth 以确保值不会发生冲突。

```xaml
<NavigationView CompactModeThresholdWidth="1007" ExpandedModeThresholdWidth="1007"/>
```

### <a name="compact"></a>精简

第三种常见自适应模式是在较大窗口宽度上使用展开的左侧窗格，在中等和较小窗口宽度上使用仅限图标的 LeftCompact 导航窗格。

在以下情况下建议使用此模式：

- 始终在屏幕上显示所有导航选项非常重要。
- 可以使用图标清楚地表示导航类别。

![左侧导航精简自适应行为](images/adaptive-behavior-compact.png)<br/>
导航视图“精简”自适应行为 

若要配置此行为，请将 CompactModeThresholdWidth 设置为 0。

```xaml
<NavigationView CompactModeThresholdWidth="0"/>
```

### <a name="no-adaptive-behavior"></a>无自适应行为

若要禁用自动自适应行为，请将 PaneDisplayMode 设置为 Auto 以外的值。在这里，它设置为 LeftMinimal，因此仅显示菜单按钮，无论窗口宽度如何。

![左侧导航无自适应行为](images/adaptive-behavior-none.png)<br/>
PaneDisplayMode 设置为 LeftMinimal 的导航视图 

```xaml
<NavigationView PaneDisplayMode="LeftMinimal" />
```

如前面在“显示模式”  部分中所述，可以将窗格设置为始终位于顶部、始终展开、始终精简或始终最小。 还可以在应用代码中自己管理显示模式。 下一部分中演示了这一示例。

### <a name="top-to-left-navigation"></a>顶部到左侧导航

在应用中使用顶部导航时，导航项会随着窗口宽度减少而折叠到溢出菜单中。 当应用窗口较窄时，将 PaneDisplayMode 从 Top 切换为 LeftMinimal 导航可以提供更好的用户体验，而不是让所有项都折叠到溢出菜单中。

在以下情况下，建议在较大窗口大小上使用顶部导航，在较小窗口大小上使用左侧导航：

- 要一起显示一组同等重要的顶级导航类别，以便在此组中的一个类别不适合屏幕时，会折叠为左侧导航栏以便使它们具有同等重要性。
- 希望在较小窗口大小中保留尽可能多的内容空间。

此示例演示如何使用 [VisualStateManager](/uwp/api/Windows.UI.Xaml.VisualStateManager) 和 [AdaptiveTrigger.MinWindowWidth](/uwp/api/windows.ui.xaml.adaptivetrigger.minwindowwidth) 属性在 Top 与 LeftMinimal 导航之间进行切换。

![顶部或左侧自适应行为 1 的示例](images/navigation-top-to-left.png)

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
> 如果使用 AdaptiveTrigger.MinWindowWidth，则当窗口宽于指定最小宽度时，会触发可视状态。 这意味着默认 XAML 定义狭窄窗口，而 VisualState 定义在窗口变得更宽时应用的修改。 导航视图的默认 PaneDisplayMode 为 Auto，所以当窗口宽度小于或等于 CompactModeThresholdWidth 时，会使用 LeftMinimal 导航。 当窗口变得更宽时，VisualState 会替代默认值，并使用 Top 导航。

## <a name="navigation"></a>导航

导航视图不自动执行任何导航任务。 当用户点击某个导航项时，导航视图会将该项显示为已选中，并引发 [ItemInvoked](/uwp/api/windows.ui.xaml.controls.navigationview.ItemInvoked) 事件。 如果点击导致新项被选中，则还会引发 [SelectionChanged](/uwp/api/windows.ui.xaml.controls.navigationview.SelectionChanged) 事件。

可以处理任一事件来执行与请求的导航相关的任务。 应处理哪个事件取决于希望应用具有的行为。 通常会导航到请求的页面并更新导航视图标题以响应这些事件。

每当用户点击导航项时都会引发 ItemInvoked  ，即使该项已被选中。 （还可以使用鼠标、键盘或其他输入通过等效操作调用该项。 有关更多信息，请参阅[输入和交互](../input/index.md)。）如果在 ItemInvoked 处理程序中导航，则默认情况下，会重新加载页面，并将重复条目添加到导航堆栈。 如果在调用某个项时进行导航，则应禁止重新加载页面，或确保在重新加载页面时不会在导航 Backstack 中创建重复条目。 （请参阅代码示例。）

用户可以通过调用当前未选中的项，或是以编程方式更改所选项来引发 SelectionChanged  。 如果因为用户调用某个项而发生选择更改，则首先发生 ItemInvoked 事件。 如果以编程方式进行选择更改，则不会引发 ItemInvoked。

### <a name="backwards-navigation"></a>向后导航

NavigationView 具有内置后退按钮；但是与向前导航一样，它不会自动执行向后导航。 用户点击后退按钮时，会引发 [BackRequested](/uwp/api/windows.ui.xaml.controls.navigationview.BackRequested) 事件。 可处理此事件以执行向后导航。 有关更多信息和代码示例，请参阅[导航历史记录和向后导航](../basics/navigation-history-and-backwards-navigation.md)。

在最小或精简模式下，导航视图窗格会作为浮出控件打开。 在这种情况下，单击后退按钮会关闭窗格并改为引发 PaneClosing  事件。

可以通过设置以下这些属性来隐藏或禁用后退按钮：

- [IsBackButtonVisible](/uwp/api/windows.ui.xaml.controls.navigationview.IsBackButtonVisible)：用于显示和隐藏后退按钮。 此属性采用 [NavigationViewBackButtonVisible](/uwp/api/windows.ui.xaml.controls.navigationviewbackbuttonvisible) 枚举值，默认情况下设置为 Auto  。 当按钮处于折叠状态时，不会在布局中为它保留空间。
- [IsBackEnabled](/uwp/api/windows.ui.xaml.controls.navigationview.IsBackEnabled)：用于启用或禁用后退按钮。 可以将此属性数据绑定到导航框架的 [CanGoBack](/uwp/api/windows.ui.xaml.controls.frame.cangoback) 属性。 如果 IsBackEnabled  为 false  ，则不会引发 BackRequested  。

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

此示例演示如何将 NavigationView 用于较大窗口大小上的顶部导航窗格和较小窗口大小上的左侧导航窗格。 它可以通过在 VisualStateManager 中删除顶部  导航设置，来适应仅限左侧导航。

该示例演示一种设置导航数据的推荐方法，它适用于许多常见方案。 还演示如何使用 NavigationView 的后退按钮和键盘导航实现向后导航。

此代码假设应用包含的要导航到的页面具有以下名称：HomePage  、AppsPage  、GamesPage  、MusicPage  、MyContentPage  和 SettingsPage  。 未显示这些页面的代码。

> [!IMPORTANT]
> 有关应用页面的信息存储在 [ValueTuple](https://docs.microsoft.com/dotnet/api/system.valuetuple) 中。 此结构要求应用项目的最低版本必须是 SDK 17763 或更高版本。 如果使用 WinUI 版本的 NavigationView 针对早期版本的 Windows 10，则可以改为使用 [System.ValueTuple NuGet 包](https://www.nuget.org/packages/System.ValueTuple/)。

> [!IMPORTANT]
> 此代码演示如何使用 [Windows UI 库](https://docs.microsoft.com/uwp/toolkits/winui/)版本的 NavigationView。 如果改为使用平台版本的 NavigationView，则应用项目的最低版本必须是 SDK 17763 或更高版本。 若要使用平台版本，请删除对 `muxc:` 的所有引用。

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
> 此代码演示如何使用 [Windows UI 库](https://docs.microsoft.com/uwp/toolkits/winui/)版本的 NavigationView。 如果改为使用平台版本的 NavigationView，则应用项目的最低版本必须是 SDK 17763 或更高版本。 若要使用平台版本，请删除对 `muxc` 的所有引用。

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

// NavView_SelectionChanged is not used in this example, but is shown for completeness.
// You will typically handle either ItemInvoked or SelectionChanged to perform navigation,
// but not both.
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

下面是上面 C# 代码示例中的 NavView_ItemInvoked  处理程序的 [C++/WinRT](/windows/uwp/cpp-and-winrt-apis/index) 版本。 C++/WinRT 处理程序中的方法涉及首先存储（在 [NavigationViewItem  ](/uwp/api/windows.ui.xaml.controls.navigationviewitem) 的标记中）要导航到的页面的完整类型名称。 在处理程序中，对值取消装箱，将它转换 [Windows::UI::Xaml::Interop::TypeName  ](/uwp/api/windows.ui.xaml.interop.typename) 对象，并使用该对象导航到目标页。 不需要在 C# 示例中看到的名为 `_pages` 的映射变量；能够创建单元测试，从而确认标记中的值是否属于有效类型。 另请参阅[通过 C++/WinRT 将标量值装箱到 IInspectable 和从 IInspectable 取消标量值装箱](/windows/uwp/cpp-and-winrt-apis/boxing)。

```cppwinrt
void MainPage::NavView_ItemInvoked(Windows::Foundation::IInspectable const & /* sender */, Windows::UI::Xaml::Controls::NavigationViewItemInvokedEventArgs const & args)
{
    if (args.IsSettingsInvoked())
    {
        // Navigate to Settings.
    }
    else if (args.InvokedItemContainer())
    {
        Windows::UI::Xaml::Interop::TypeName pageTypeName;
        pageTypeName.Name = unbox_value<hstring>(args.InvokedItemContainer().Tag());
        pageTypeName.Kind = Windows::UI::Xaml::Interop::TypeKind::Primitive;
        ContentFrame().Navigate(pageTypeName, nullptr);
    }
}
```

## <a name="navigation-view-customization"></a>导航视图自定义

### <a name="pane-backgrounds"></a>窗格背景

默认情况下，NavigationView 窗格根据显示模式使用不同的背景：

- 在左侧展开，与内容并排时（左侧模式），窗格是纯灰色。
- 在内容顶部以覆盖形式打开时（顶部、最小或精简模式），窗格使用应用内 Acrylic。

若要修改窗格背景，可以在每种模式中替代用于呈现背景的 XAML 主题资源。 （使用此方法而不是单个 PaneBackground 属性是为了针对不同显示模式支持不同背景。）

此表显示每种显示模式下使用的主题资源。

| 显示模式 | 主题资源 |
| ------------ | -------------- |
| 向左 | NavigationViewExpandedPaneBackground |
| LeftCompact<br/>LeftMinimal | NavigationViewDefaultPaneBackground |
| 顶部 | NavigationViewTopPaneBackground |

此示例演示如何在 App.xaml 中替代主题资源。 替代主题资源时，应始终至少提供“Default”和“HighContrast”资源字典，并根据需要提供用于“Light”或“Dark”资源的字典。 有关更多信息，请参阅 [ResourceDictionary.ThemeDictionaries](/uwp/api/windows.ui.xaml.resourcedictionary.themedictionaries)。

> [!IMPORTANT]
> 此代码演示如何使用 [Windows UI 库](https://docs.microsoft.com/uwp/toolkits/winui/)版本的 AcrylicBrush。 如果改为使用平台版本的 AcrylicBrush，则应用项目的最低版本必须是 SDK 16299 或更高版本。 若要使用平台版本，请删除对 `muxm:` 的所有引用。

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

### <a name="top-whitespace"></a>顶部空白
某些应用选择[自定义其窗口的标题栏](https://docs.microsoft.com/windows/uwp/design/shell/title-bar)，可能会将其应用内容扩展到标题栏区域中。 当 NavigationView 是**使用 [ExtendViewIntoTitleBar](https://docs.microsoft.com/uwp/api/windows.applicationmodel.core.coreapplicationviewtitlebar.extendviewintotitlebar) API** 扩展到标题栏的应用中的根元素时，该控件会自动调整其交互式元素的位置，以防止与[可拖动区域](https://docs.microsoft.com/windows/uwp/design/shell/title-bar#draggable-regions)重叠。 
![扩展到标题栏中的应用](images/navigation-view-with-titlebar-padding.png)

如果应用通过调用 [Window.SetTitleBar](https://docs.microsoft.com/uwp/api/windows.ui.xaml.window.settitlebar) 方法来指定可拖动区域，并且你希望使“后退”和菜单按钮拖动到距离应用窗口顶部更近的位置，请将 `IsTitleBarAutoPaddingEnabled` 设置为 False。

![应用扩展到标题栏而无需额外填充](images/navigation-view-no-titlebar-padding.png)

```Xaml
<muxc:NavigationView x:Name="NavView" IsTitleBarAutoPaddingEnabled="False">
```

#### <a name="remarks"></a>备注
若要进一步调整 NavigationView 的标题区域位置，请重写 *NavigationViewHeaderMargin XAML* 主题资源，例如，在“页”资源中。

```Xaml
<Page.Resources>
    <Thickness x:Key="NavigationViewHeaderMargin">12,0</Thickness>
</Page.Resources>
```

此主题资源修改 [NavigationView.Header](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview.header) 周围的边距。

## <a name="related-topics"></a>相关主题

- [NavigationView 类](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview)
- [大纲/细节](master-details.md)
- [导航基础知识](../basics/navigation-basics.md)
- [UWP 的 Fluent Design 概述](/windows/apps/fluent-design-system)
