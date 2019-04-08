---
Description: NavigationView 是实现您的应用程序的顶级导航模式自适应控件。
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
ms.openlocfilehash: 4ba3a45701d82ad0b43591469bf390190ec18db0
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2019
ms.locfileid: "57642222"
---
# <a name="navigation-view"></a>导航视图

NavigationView 控件提供了有关您的应用程序的顶级导航。 它适应各种屏幕尺寸，并支持这两_顶部_并_左_导航样式。

![顶部导航栏](images/nav-view-header.png)<br/>
_导航视图支持顶部和左侧的导航窗格中或菜单_

> **平台 Api**:[Windows.UI.Xaml.Controls.NavigationView 类](/uwp/api/windows.ui.xaml.controls.navigationview)
>
> **Windows UI 库 Api**:[Microsoft.UI.Xaml.Controls.NavigationView 类](/uwp/api/microsoft.ui.xaml.controls.navigationview)
>
> NavigationView，某些功能等_顶部_导航窗格中，需要 Windows 10，版本 1809年 ([SDK 17763](https://developer.microsoft.com/windows/downloads/windows-10-sdk)) 或更高版本，或[Windows UI 库](https://docs.microsoft.com/uwp/toolkits/winui/)。

## <a name="is-this-the-right-control"></a>这是正确的控件吗？

NavigationView 是非常适合于在自适应导航控件：

- 提供在整个应用一致的导航体验。
- 保留的屏幕空间更小的窗口上。
- 组织访问许多导航类别。

其他导航模式，请参阅[导航设计基础知识](../basics/navigation-basics.md)。

## <a name="examples"></a>示例

<table>
<th align="left">XAML 控件库<th>
<tr>
<td><img src="images/XAML-controls-gallery-app-icon.png" alt="XAML controls gallery" width="168"></img></td>
<td>
    <p>如果已安装 <strong style="font-weight: semi-bold">XAML 控件库</strong>应用，请单击此处<a href="xamlcontrolsgallery:/item/NavigationView">打开此应用，了解 NavigationView 的实际应用</a>。</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">获取 XAML 控件库应用 (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">获取源代码 (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="display-modes"></a>显示模式

> PaneDisplayMode 属性需要 Windows 10，版本 1809年 ([SDK 17763](https://developer.microsoft.com/windows/downloads/windows-10-sdk)) 或更高版本，或[Windows 用户界面库](https://docs.microsoft.com/uwp/toolkits/winui/)。

可以使用 PaneDisplayMode 属性可以配置不同的导航样式或 NavigationView 的显示模式。

:::row:::
    :::column:::
    ### <a name="top"></a>顶部
    在窗格位于上述内容。</br>
    `PaneDisplayMode="Top"`
    :::column-end:::
    :::column span="2":::
    ![顶部导航栏的示例](images/displaymode-top.png)
    :::column-end:::
:::row-end:::

我们建议_顶部_导航时：

- 有 5 或更少都同等重要的顶级导航类别，最终会在下拉列表中溢出菜单中的类别被视为不太重要的任何其他的顶级导航。
- 您需要显示在屏幕上的所有导航选项。
- 为您的应用程序内容需要更多空间。
- 图标无法清楚地描述您的应用程序导航类别。

:::row:::
    :::column:::
    ### <a name="left"></a>向左
    窗格将展开并定位到左侧的内容。</br>
    `PaneDisplayMode="Left"`
    :::column-end:::
    :::column span="2":::
    ![展开左侧的导航窗格中的示例](images/displaymode-left.png)
    :::column-end:::
:::row-end:::

我们建议_左_导航时：

- 具有 5-10 同样重要的是顶级导航类别。
- 你想要非常醒目，使用较少的空间用于其他应用程序内容的导航类别。

:::row:::
    :::column:::
    ### <a name="leftcompact"></a>LeftCompact
    该窗格显示图标之前打开，并定位到左侧的内容。</br>
    `PaneDisplayMode="LeftCompact"`
    :::column-end:::
    :::column span="2":::
    ![Compact 左侧的导航窗格中的示例](images/displaymode-leftcompact.png)
    :::column-end:::
:::row-end:::

:::row:::
    :::column:::
    ### <a name="leftminimal"></a>LeftMinimal
    直到打开窗格，只有菜单按钮后显示。 打开时，它被定位到左侧的内容。</br>
    `PaneDisplayMode="LeftMinimal"`
    :::column-end:::
    :::column span="2":::
    ![最小的左侧的导航窗格中的示例](images/displaymode-leftminimal.png)
    :::column-end:::
:::row-end:::

### <a name="auto"></a>自动

默认情况下，PaneDisplayMode 设置为自动。在自动模式下，导航视图之间 LeftMinimal 时窗口较窄，到 LeftCompact，修改以适应，并再向左窗口变宽。 有关详细信息，请参阅[自适应行为](#adaptive-behavior)部分。

![左侧导航栏默认自适应行为](images/displaymode-auto.png)<br/>
_导航视图默认自适应行为_

## <a name="anatomy"></a>结构

这些图像显示窗格中，标头和控件时为配置的内容区域的布局_顶部_或_左_导航。

![顶部导航栏视图布局](images/topnav-anatomy.png)<br/>
_顶部导航栏布局_

![左侧的导航视图布局](images/leftnav-anatomy.png)<br/>
_左侧导航栏布局_

### <a name="pane"></a>窗格

PaneDisplayMode 属性可用于调整窗格内容的顶部或左侧的内容的位置。

NavigationView 窗格可能包含：

- [NavigationViewItem](/uwp/api/windows.ui.xaml.controls.navigationviewitem)对象。 导航到特定页面的导航项。
- [NavigationViewItemSeparator](/uwp/api/windows.ui.xaml.controls.navigationviewitemseparator)对象。 对导航项进行分组的分隔符。 设置[不透明度](/uwp/api/windows.ui.xaml.uielement.opacity)属性设为 0 来呈现的分隔符为空格。
- [NavigationViewItemHeader](/uwp/api/windows.ui.xaml.controls.navigationviewitemheader)对象。 标记项的组标头。
- 一个可选[AutoSuggestBox](auto-suggest-box.md)控件，以允许应用程序级搜索。 将分配到的控件[NavigationView.AutoSuggestBox](/uwp/api/windows.ui.xaml.controls.navigationview.autosuggestbox)属性。
- [应用设置](../app-settings/app-settings-and-data.md)的可选入口点。 若要隐藏设置项，设置[IsSettingsVisible](/uwp/api/windows.ui.xaml.controls.navigationview.IsSettingsVisible)属性设置为**false**。

左窗格中还包含：

- 若要切换窗格打开和关闭菜单按钮。 打开窗格后在较大的应用窗口中，你可以选择使用 [IsPaneToggleButtonVisible](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview.IsPaneToggleButtonVisible) 属性隐藏此按钮。

导航视图都有一个放置在窗格的左上角的后退按钮。 但是，它不会不会自动处理向后导航和将内容添加到 back 堆栈。 若要启用向后导航，请参阅[向后导航](#backwards-navigation)部分。

下面是顶部和左侧窗格位置详细的窗格剖析。

#### <a name="top-navigation-pane"></a>顶部导航窗格

![导航视图顶部窗格剖析](images/navview-pane-anatomy-horizontal.png)

1. 标头
1. 导航项
1. 分隔符
1. AutoSuggestBox （可选）
1. （可选） 设置按钮

#### <a name="left-navigation-pane"></a>左侧的导航窗格

![导航视图左窗格剖析](images/navview-pane-anatomy-vertical.png)

1. “菜单”按钮
1. 导航项
1. 分隔符
1. 标头
1. AutoSuggestBox （可选）
1. （可选） 设置按钮

#### <a name="pane-footer"></a>窗格页脚

然后就可以自由格式窗格的页脚中的内容添加到[PaneFooter](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview.PaneFooter)属性。

:::row:::
    :::column:::
    ![窗格页脚顶部导航栏](images/navview-freeform-footer-top.png)<br>
     _顶部窗格页脚_<br>
    :::column-end:::
    :::column:::
    ![窗格页脚左导航栏](images/navview-freeform-footer-left.png)<br>
    _左的窗格页脚_<br>
    :::column-end:::
:::row-end:::

#### <a name="pane-title-and-header"></a>窗格标题和页眉

通过设置，可以在窗格标题区域中放置文本内容[PaneTitle](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview.PaneTitle)属性。 它采用一个字符串，并显示菜单按钮旁边的文本。

要添加非文本内容，如图像或徽标，您可以以放置的任何元素窗格的标头中将其添加到[PaneHeader](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview.PaneHeader)属性。

如果 PaneTitle 和 PaneHeader 内容旁边的菜单按钮，使用最接近的菜单按钮 PaneTitle 水平堆叠。

:::row:::
    :::column:::
    ![窗格标头顶部导航栏](images/navview-freeform-header-top.png)<br>
     _顶部窗格标头_<br>
    :::column-end:::
    :::column:::
    ![窗格标头左侧导航栏](images/navview-freeform-header-left.png)<br>
    _左窗格中的标头_<br>
    :::column-end:::
:::row-end:::

#### <a name="pane-content"></a>窗格的内容

然后就可以自由的内容窗格中添加到[PaneCustomContent](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview.PaneCustomContent)属性。

:::row:::
    :::column:::
    ![窗格中自定义内容顶部导航](images/navview-freeform-pane-top.png)<br>
     _顶部窗格中自定义内容_<br>
    :::column-end:::
    :::column:::
    ![窗格中自定义内容向左导航栏](images/navview-freeform-pane-left.png)<br>
    _左窗格中自定义内容_<br>
    :::column-end:::
:::row-end:::

### <a name="header"></a>标头

可以通过设置添加页面标题[标头](/uwp/api/windows.ui.xaml.controls.navigationview.header)属性。

![导航视图标题区域的示例](images/nav-header.png)<br/>
_导航视图标头_

标头区域与在左的窗格中的位置中的导航按钮垂直对齐，并位于下方的窗格中的顶部窗格位置。 它具有固定的高度为 52 像素。 它的用途是保存所选导航类别的页面标题。 标题停靠在页面顶部，并作为内容区域的滚动剪辑点。

标头是可见的 NavigationView 处于最小显示模式下的任何时间。 你可以选择在其他模式下隐藏标题，这样可以增大窗口宽度。 若要隐藏该标头，设置[AlwaysShowHeader](/uwp/api/windows.ui.xaml.controls.navigationview.AlwaysShowHeader)属性设置为**false**。

### <a name="content"></a>内容

![导航视图内容区域的示例](images/nav-content.png)<br/>
_导航视图内容_

内容区域是显示所选导航类别的大部分信息的位置。

NavigationView 中时，我们建议内容区域的 12px 边距**最小**否则为模式和 24px 边距。

## <a name="adaptive-behavior"></a>自适应行为

默认情况下，导航视图会自动更改其显示模式，基于提供给它的屏幕空间量。 [CompactModeThresholdWidth](/uwp/api/windows.ui.xaml.controls.navigationview.compactmodethresholdwidth)并[ExpandedModeThresholdWidth](/uwp/api/windows.ui.xaml.controls.navigationview.expandedmodethresholdwidth)属性指定的显示模式更改的断点。 可以修改这些值以自定义的自适应显示模式行为。

### <a name="default"></a>默认

如果 PaneDisplayMode 设置为其默认值**自动**，自适应行为是说明：

- 较大的窗口宽度上展开左窗格中 (1008px 或更高版本)。
- 一个保留，仅图标中的窗口宽度上的导航窗格 (LeftCompact) (641px 到 1007px)。
- 仅菜单上的按钮 (LeftMinimal) 小窗口的宽度 (640 像素或更少)。

有关自适应行为的窗口大小的详细信息，请参阅[屏幕大小和断点](../layout/screen-sizes-and-breakpoints-for-responsive-design.md)。

![左侧导航栏默认自适应行为](images/displaymode-auto.png)<br/>
_导航视图默认自适应行为_

### <a name="minimal"></a>最小

第二个常见的自适应模式是使用较大的窗口宽度和菜单按钮将这两个中型和小型窗口宽度上展开的左窗格。

我们建议在以下情况：

- 对于较小的窗口宽度上的应用程序内容需要更多空间。
- 不能使用图标来清楚地表示导航类别。

![左侧导航栏的最小自适应行为](images/adaptive-behavior-minimal.png)<br/>
_导航视图的"最小"自适应行为_

若要配置此行为，请将 CompactModeThresholdWidth 设为折叠窗格的宽度。 在这里，它是从默认值为 640 到 1007年进行更改。 此外应设置 ExpandedModeThresholdWidth 以确保值不会发生冲突。

```xaml
<NavigationView CompactModeThresholdWidth="1007" ExpandedModeThresholdWidth="1007"/>
```

### <a name="compact"></a>精简

第三个常见的自适应模式是使用较大的窗口宽度和 LeftCompact，仅图标在这两个中型和小型窗口宽度的导航窗格上的展开的左窗格。

我们建议在以下情况：

- 请务必始终显示在屏幕上的所有导航选项。
- 可以使用图标来清楚地表示导航类别。

![左侧的导航 compact 自适应行为](images/adaptive-behavior-compact.png)<br/>
_导航视图"compact"自适应行为_

若要配置此行为，请将 CompactModeThresholdWidth 设为 0。

```xaml
<NavigationView CompactModeThresholdWidth="0"/>
```

### <a name="no-adaptive-behavior"></a>没有自适应行为

若要禁用自动自适应行为，请将 PaneDisplayMode 设为自动以外的值。此处，设置到 LeftMinimal，因此，只有菜单按钮显示而不考虑窗口宽度。

![左侧导航栏没有自适应行为](images/adaptive-behavior-none.png)<br/>
_使用设置为 LeftMinimal PaneDisplayMode 导航视图_

```xaml
<NavigationView PaneDisplayMode="LeftMinimal" />
```

如前面所述_显示模式_部分中，您可以设置窗格始终位于顶部，始终展开的、 始终 compact，还是始终最小。 您还可以管理的显示模式自己在应用代码中。 此示例是下一节中所示。

### <a name="top-to-left-navigation"></a>从上到左侧导航栏

当应用程序中使用顶部导航栏时，导航项折叠到溢出菜单作为窗口宽度减少。 当应用窗口较窄时，它可以提供更好的用户体验，若要切换到 LeftMinimal 导航栏中，而不是让所有项折叠到溢出菜单从上 PaneDisplayMode。

我们建议使用较大的窗口大小和较小的左侧的导航上的顶部导航窗口大小时：

- 可以一组同样重要的顶级导航类别显示在一起，以便您在此集中的一个类别不适合在屏幕上，如果折叠左侧导航栏以便用户同等的重要性。
- 你想要保留作为内容更多的空间，小窗口的大小。

此示例演示如何使用[VisualStateManager](/uwp/api/Windows.UI.Xaml.VisualStateManager)并[AdaptiveTrigger.MinWindowWidth](/uwp/api/windows.ui.xaml.adaptivetrigger.minwindowwidth)属性顶部和 LeftMinimal 导航之间进行切换。

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
> 当使用 AdaptiveTrigger.MinWindowWidth 时，当窗口大于指定最小宽度时，触发的可视状态。 这意味着默认 XAML 定义窄的窗口中，并且 VisualState 定义时窗口获取更广泛应用的修改。 默认 PaneDisplayMode 导航视图为自动，所以当窗口宽度小于或等于 CompactModeThresholdWidth，LeftMinimal 导航使用。 当窗口变宽时，VisualState 重写默认值，并使用顶部导航栏。

## <a name="navigation"></a>导航

导航视图不会自动执行任何导航任务。 在用户点击导航项，导航视图显示为已选中该项，并引发[ItemInvoked](/uwp/api/windows.ui.xaml.controls.navigationview.ItemInvoked)事件。 如果点击导致所选择的新项[SelectionChanged](/uwp/api/windows.ui.xaml.controls.navigationview.SelectionChanged)也会引发事件。

您可以处理这两个事件来执行请求的导航到相关的任务。 哪一个你应处理取决于您希望您的应用程序的行为。 通常情况下，导航到所请求的页面并更新以响应这些事件的导航视图标头。

**ItemInvoked**即使已选择它，的只要用户点击导航项，就会引发。 （该项目，也可以调用等效操作时，使用鼠标、 键盘或其他输入。 有关详细信息，请参阅[输入和交互](../input/index.md)。)如果导航 ItemInvoked 处理程序中，默认情况下，将重新加载页面，并重复项添加到导航堆栈。 如果导航调用某项时，应不允许重新加载该页面，或确保重复项创建的导航 backstack 中重新加载页面时。 （请参阅代码示例。）

**SelectionChanged**通过调用不当前选定的项的用户或以编程方式更改所选的项可以引发。 如果选择更改发生，因为用户调用某个项，第一次发生 ItemInvoked 事件。 如果以编程方式选择更改，则不会引发 ItemInvoked。

### <a name="backwards-navigation"></a>向后导航

NavigationView 具有内置后退按钮;但是，与前向导航栏中，它不会向后导航自动执行。 当用户点击后退按钮， [BackRequested](/uwp/api/windows.ui.xaml.controls.navigationview.BackRequested)引发事件。 处理此事件来执行向后导航。 有关更多的信息和代码示例，请参阅[导航历史记录和向后导航](../basics/navigation-history-and-backwards-navigation.md)。

在最小或 Compact 模式下，导航视图窗格已打开作为浮出控件。 在这种情况下，单击后退按钮将关闭窗格并引发**PaneClosing**事件相反。

可以隐藏或禁用通过设置这些属性的后退按钮：

- [IsBackButtonVisible](/uwp/api/windows.ui.xaml.controls.navigationview.IsBackButtonVisible)： 用于显示和隐藏后退按钮。 此属性采用的值[NavigationViewBackButtonVisible](/uwp/api/windows.ui.xaml.controls.navigationviewbackbuttonvisible)枚举，并将设置为**自动**默认情况下。 当按钮处于折叠状态时，没有空间为其保留在布局中。
- [IsBackEnabled](/uwp/api/windows.ui.xaml.controls.navigationview.IsBackEnabled)： 用于启用或禁用后退按钮。 您可以数据绑定到此属性[CanGoBack](/uwp/api/windows.ui.xaml.controls.frame.cangoback)导航框架的属性。 **BackRequested**如果，则不会引发**IsBackEnabled**是**false**。

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

此示例演示如何使用 NavigationView 的左侧的导航窗格中，在小型窗口大小和较大的窗口大小的顶部导航窗格。 它可适用于仅限左侧的导航通过删除_顶部_VisualStateManager 中的导航设置。

该示例演示设置可用于许多常见方案的导航数据的推荐的方法。 它还演示了如何实现向后导航与 NavigationView 的后退按钮和键盘导航。

此代码假定你的应用包含具有以下名称以导航到页面：_主页_， _AppsPage_， _GamesPage_， _MusicPage_， _MyContentPage_，和_SettingsPage_. 不显示为这些页面的代码。

> [!IMPORTANT]
> 有关应用的页面信息存储在[ValueTuple](https://docs.microsoft.com/dotnet/api/system.valuetuple)。 此结构要求的最低版本为您的应用程序项目必须 17763 或更高版本的 SDK。 如果您使用的 NavigationView WinUI 版本面向早期版本的 Windows 10，则可以使用[System.ValueTuple NuGet 包](https://www.nuget.org/packages/System.ValueTuple/)相反。

> [!IMPORTANT]
> 此代码演示如何使用[Windows 用户界面库](https://docs.microsoft.com/uwp/toolkits/winui/)NavigationView 的版本。 如果改为使用 NavigationView 的平台版本，应用程序项目的最低版本必须是 17763 或更高版本的 SDK。 若要使用的平台版本，请删除对所有引用`muxc:`。

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
> 此代码演示如何使用[Windows 用户界面库](https://docs.microsoft.com/uwp/toolkits/winui/)NavigationView 的版本。 如果改为使用 NavigationView 的平台版本，应用程序项目的最低版本必须是 17763 或更高版本的 SDK。 若要使用的平台版本，请删除对所有引用`muxc`。

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

下面是[C + + WinRT](/windows/uwp/cpp-and-winrt-apis/index)新版**NavView_ItemInvoked**处理程序从C#上面的代码示例。 方法在 C + + WinRT 处理程序涉及到您第一次存储 (中的标记[ **NavigationViewItem**](/uwp/api/windows.ui.xaml.controls.navigationviewitem)) 想要导航的页面的完整类型名称。 在处理程序中，取消装箱的值，将其转换[ **Windows::UI::Xaml::Interop::TypeName** ](/uwp/api/windows.ui.xaml.interop.typename)对象，并使用它来导航到目标页。 名为的映射变量无需`_pages`中看到C#的示例; 并且您将能够创建单元测试，证明你标记内的值的有效类型。 另请参阅[装箱和取消装箱的标量值到 IInspectable 使用 C + + WinRT](/windows/uwp/cpp-and-winrt-apis/boxing)。

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

## <a name="navigation-view-customization"></a>导航视图自定义项

### <a name="pane-backgrounds"></a>窗格背景

默认情况下，NavigationView 窗格使用不同的背景，具体取决于显示模式：

- 在窗格是纯灰颜色在左侧 （中左模式） 的内容同时展开。
- 窗格中使用时打开的应用程序内染料作为基础 （顶部、 最小，或 Compact 模式下的内容） 上一个覆盖区。

若要修改窗格背景，您可以覆盖用于呈现在每种模式中的背景的 XAML 主题资源。 （这种方法使用而不是单个 PaneBackground 属性以便为不同的显示模式支持不同的背景。）

此表显示了每个显示模式下使用哪些主题资源。

| 显示模式 | 主题资源 |
| ------------ | -------------- |
| 向左 | NavigationViewExpandedPaneBackground |
| LeftCompact<br/>LeftMinimal | NavigationViewDefaultPaneBackground |
| 顶部 | NavigationViewTopPaneBackground |

此示例演示如何重写 App.xaml 中的主题资源。 当重写主题资源时，应始终提供最低限度的"Default"和"高对比度"资源字典和字典"浅色"或"深色"资源根据需要。 有关详细信息，请参阅[ResourceDictionary.ThemeDictionaries](/uwp/api/windows.ui.xaml.resourcedictionary.themedictionaries)。

> [!IMPORTANT]
> 此代码演示如何使用[Windows 用户界面库](https://docs.microsoft.com/uwp/toolkits/winui/)AcrylicBrush 的版本。 如果改为使用 AcrylicBrush 的平台版本，应用程序项目的最低版本必须是 16299 或更高版本的 SDK。 若要使用的平台版本，请删除对所有引用`muxm:`。

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
- [Fluent 设计 UWP 概述](../fluent-design-system/index.md)
