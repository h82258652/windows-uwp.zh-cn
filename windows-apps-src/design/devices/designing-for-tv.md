---
Description: 设计你的应用，以使其在电视上外观良好且运行正常。
title: 针对 Xbox 和电视进行设计
ms.assetid: 780209cb-3e8a-4cf7-8f80-8b8f449580bf
label: Designing for Xbox and TV
template: detail.hbs
isNew: true
keywords: Xbox, TV, 10 英尺体验, 游戏板, 遥控器, 输入, 交互
ms.date: 11/13/2018
ms.topic: article
pm-contact: chigy
design-contact: jeffarn
dev-contact: niallm
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: 431b8912e43647bc2678aaab7efc9ec68b866d10
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2019
ms.locfileid: "57616652"
---
# <a name="designing-for-xbox-and-tv"></a>针对 Xbox 和电视进行设计

设计你的通用 Windows 平台 (UWP) 应用，以便它在 Xbox One 和电视屏幕上外观良好且运行正常。

请参阅[游戏手柄和远程控制交互](../input/gamepad-and-remote-interactions.md)交互有关的指南中的体验中的 UWP 应用程序*10 英尺*体验。

## <a name="overview"></a>概述

通用 Windows 平台可使你在多台 Windows 10 设备上创建令人愉快的体验。
UWP 框架提供的大部分功能使应用能够在这些设备上使用相同的用户界面 (UI)，而无需附加工作。
但是，定制和优化你的应用以使其在 Xbox One 和电视屏幕上良好工作需要考虑特殊的注意事项。

坐在房间中的沙发上使用游戏板或遥控器与电视交互的体验称为 **10 英尺体验**。
如此命名是因为用户通常坐在离屏幕大约 10 英尺的位置。
这提出了 *2 英尺*体验（假设）中或与电脑交互时所不存在的独特挑战。
如果你要为 Xbox One 或任何输出到电视屏幕和使用控制器进行输入的其他设备开发应用，应始终牢记这一点。

若要使你的应用非常适合 10 英尺体验，并不需要执行本文中的所有步骤，但了解这些步骤并为应用进行相应的决策可使 10 英尺体验更符合应用的特定需求。
在将应用实际应用到 10 英尺环境中时，请考虑以下设计原则。

### <a name="simple"></a>Simple

针对 10 英尺环境进行设计提出了一组独特的挑战。 分辨率和观看距离可能使人难以处理太多信息。
尝试使设计保持干净，尽可能减少到最简单的组件。 电视上所显示的信息量应与你在移动电话上（而不是桌面上）看到的内容相当。

![Xbox One 主屏幕](images/designing-for-tv/xbox-home-screen.png)

### <a name="coherent"></a>一致

10 英尺环境中的 UWP 应用应当直观且易于使用。 是重点清晰明显。
安排内容，以使跨空间移动一致且可预测。 为用户提供指向所需操作的最短路径。

![Xbox One 电影应用](images/designing-for-tv/xbox-movies-app.png)

_**Microsoft 电影和电视上提供了所有电影的屏幕截图中所示。**_  

### <a name="captivating"></a>引人入胜

在大屏幕上可享受最沉浸式的电影体验。 边对边场景、优美的动作以及颜色和版式的灵活使用可将你的应用提升到下一层次。 兼具大胆和美观。

![Xbox One 虚拟形象应用](images/designing-for-tv/xbox-avatar-app.png)

### <a name="optimizations-for-the-10-foot-experience"></a>针对 10 英尺体验的优化

现在你已了解了适用于 10 英尺体验的良好 UWP 应用设计的原则，请阅读以下关于优化应用和实现出色用户体验的特定方法的概述。

| 功能        | 描述           |
| -------------------------------------------------------------- |--------------------------------|
| [UI 元素大小调整](#ui-element-sizing)  | 通用 Windows 平台使用[缩放和有效像素](../basics/design-and-ui-intro.md#effective-pixels-and-scaling)来根据观看距离缩放 UI。 了解大小调整并在 UI 上应用它有助于针对 10 英尺环境优化你的应用。  |
|  [电视安全区域](#tv-safe-area) | 默认情况下，UWP 将自动避免在电视不安全区域（接近屏幕边缘的区域）显示任何 UI。 但是，这将导致“箱中”效果，即 UI 看起来以宽屏显示。 为了使你的应用在电视上提供真正的沉浸式体验，你将要对其进行修改，以使其延伸到电视上的屏幕边缘（如果电视支持此功能）。 |
| [颜色](#colors)  |  UWP 支持颜色主题，并且遵循系统主题的应用在 Xbox One 上将默认为**深色**。 如果你的应用具有特定的颜色主题，你应考虑到某些颜色不适用于电视，应避免使用它们。 |
| [声音](../style/sound.md)    | 声音在 10 英尺体验中起到关键作用，它有助于使用户沉浸和向用户提供反馈。 当应用在 Xbox One 上运行时，UWP 提供为常用控件自动启用声音的功能。 了解有关内置于 UWP 的声音支持的详细信息，并了解如何充分利用它。    |
| [UI 控件的指导原则](#guidelines-for-ui-controls)  |  有多个 UI 控件可跨多台设备运行良好，但在电视上使用时有些特定的注意事项。 请阅读在针对 10 英尺体验进行设计时使用这些控件的一些最佳做法。 |
| [Xbox 的自定义视觉状态触发器](#custom-visual-state-trigger-for-xbox) | 若要针对 3 米体验定制 UWP 应用，我们建议你使用自定义*视觉状态触发器*，以在应用检测到它已在 Xbox 主机上启动时更改布局。 |

除了前面的设计和布局注意事项，有大量[游戏板和远程控制交互](../input/gamepad-and-remote-interactions.md)生成您的应用程序时应考虑的优化。

| 功能        | 描述           |
| -------------------------------------------------------------- |--------------------------------|
| [XY 焦点导航和交互](../input/gamepad-and-remote-interactions.md#xy-focus-navigation-and-interaction) | **XY 焦点导航**使用户能够浏览应用程序的 UI。 但是，这会限制用户只能向上、向下、向左和向右导航。 本部分概述了处理此情况的建议和其他注意事项。 |
| [鼠标模式](../input/gamepad-and-remote-interactions.md#mouse-mode)|XY 焦点导航并不可行，或甚至可能，对于某些类型的映射或绘图和绘画应用等应用程序。 在这些情况下，**鼠标模式**可让用户自由导航与游戏板或远程控制，就像鼠标在 PC 上的。|
| [焦点视觉对象](../input/gamepad-and-remote-interactions.md#focus-visual)  | 焦点视觉对象是突出显示了当前聚焦的 UI 元素的边框。 这有助于用户快速确定它们是在导航或与之交互的 UI。  |
| [焦点 engagement](../input/gamepad-and-remote-interactions.md#focus-engagement) | 焦点 engagement 要求用户按**A/选择**游戏板或远程控制时的 UI 元素与之交互以便具有焦点的按钮。 |
| [硬件按钮](../input/gamepad-and-remote-interactions.md#hardware-buttons) | 游戏板和远程控制提供非常不同的按钮和配置。 |

> [!NOTE]
> 本主题中的大多数代码段都是以 XAML /C# 编写的；但其原则和概念适用于所有 UWP 应用。 如果你在开发适用于 Xbox 的 HTML/JavaScript UWP 应用，请查看 GitHub 上的出色 [TVHelpers](https://github.com/Microsoft/TVHelpers/wiki) 库。

## <a name="ui-element-sizing"></a>UI 元素大小调整

由于 10 英尺环境中的应用的用户使用遥控器或游戏板，并且坐在离屏幕数英尺远的位置，因此需要将一些 UI 注意事项纳入设计中。
确保 UI 具有相应的内容密度并且不过于凌乱，以便用户可以轻松地导航和选择元素。 记住：关键在于简洁性。

### <a name="scale-factor-and-adaptive-layout"></a>比例系数和自适应布局

**比例系数**有助于确保 UI 元素以适合正在运行应用的设备的大小显示。
在桌面上，此设置在**设置&gt;系统&gt;显示**中以滑动值的形式提供。
如果设备支持，此相同设置也存在于手机上。

![更改文本、应用和其他项的大小](images/designing-for-tv/ui-scaling.png)

在 Xbox One 上，没有此类系统设置；但是，对于要针对电视设置相应大小的 UWP UI 元素，将以 **200%**（对于 XAML 应用）和 **150%**（对于 HTML 应用）的默认值缩放它们。
只要 UI 元素针对其他设备设置相应的大小，也将针对电视设置相应的大小。
Xbox One 以 1080p（1920 x 1080 像素）呈现你的应用。 因此，当显示来自其他设备（如电脑）的应用时，请确保利用[自适应技术](../layout/screen-sizes-and-breakpoints-for-responsive-design.md)使 UI 在 100% 缩放的 960 x 540 px（对于HTML 应用，则为 100% 缩放的 1280 x 720 px）外观出色。

针对 Xbox 进行设计与针对电脑进行设计稍有不同，因为你只需要考虑一个分辨率，即 1920 x 1080。
用户的电视是否具有较高的分辨率不重要&mdash;UWP 应用将始终缩放为 1080p。

无论电视分辨率如何，当在 Xbox One 上运行时，还将为你的应用从 200%（对于 HTML 应用，则为 150%）集中提取正确的资源大小。

### <a name="content-density"></a>内容密度

在设计应用时，请记住用户将从一段距离外观看 UI 并使用遥控器或游戏控制器与其交互，这花费的导航时间比使用鼠标或触摸输入要多。

#### <a name="sizes-of-ui-controls"></a>UI 控件的大小

交互式 UI 元素的大小应设置为最低高度 32 epx（有效像素）。 这是常用 UWP 控件的默认值，并且当以 200% 缩放使用时，它可确保 UI 元素在一段距离外可见，并且有助于降低内容密度。

![100% 和 200% 缩放的 UWP 按钮](images/designing-for-tv/button-100-200.png)

#### <a name="number-of-clicks"></a>单击数

当用户从电视屏幕的一侧导航到另一侧时，此操作不应超过**六次单击**，以便简化你的 UI。 同样，**简洁性**原则也适用于此处。 

![跨 6 个图标](images/designing-for-tv/six-clicks.png)

### <a name="text-sizes"></a>文本大小

若要使你的 UI 在一段距离外可见，请使用以下经验法则：

* 主要文本和读取内容：15 epx 最小值
* 非关键的文本和补充内容：12 epx 最小值

当在 UI 中使用较大的文本时，请选取一个没有过分限制屏幕空间的大小，以免占用其他内容可能填充的空间。

### <a name="opting-out-of-scale-factor"></a>选择退出比例系数

我们建议你的应用充分利用比例系数支持，这将有助于它通过针对每个设备类型进行缩放在所有设备上以相应方式运行。
但是，可以选择退出此行为并以 100% 缩放设计你的所有 UI。 请注意，你无法将比例系数更改为任何 100% 以外的值。

对于 XAML 应用，你可以通过使用以下代码段选择退出比例系数：

```csharp
bool result =
    Windows.UI.ViewManagement.ApplicationViewScaling.TrySetDisableLayoutScaling(true);
```

`result` 将通知你是否您已成功选择退出。

有关详细信息，包括 HTML/JavaScript 示例代码，请参阅[如何关闭缩放](../../xbox-apps/disable-scaling.md)。

请务必计算 UI 元素的相应大小，方法是将本主题中提到的*有效*像素值乘以 2，即为*实际*像素值（对于 HTML 应用，则乘以 1.5）。

## <a name="tv-safe-area"></a>电视安全区域

由于历史和技术原因，并非所有电视都将内容一直显示到屏幕边缘。 默认情况下，UWP 将避免在电视不安全区域中显示任何 UI 内容，并将改为仅绘制页面背景。

电视不安全区域在下图中由蓝色区域表示。

![电视不安全区域](images/designing-for-tv/tv-unsafe-area.png)

你可以将背景设置为静态或主题色，或设置为图像，如以下代码段所示。

### <a name="theme-color"></a>主题色

```xml
<Page x:Class="Sample.MainPage"
      Background="{ThemeResource ApplicationPageBackgroundThemeBrush}"/>
```

### <a name="image"></a>图像

```xml
<Page x:Class="Sample.MainPage"
      Background="\Assets\Background.png"/>
```

这是你的应用在未进行附加工作时的外观。

![电视安全区域](images/designing-for-tv/tv-safe-area.png)

这不是最佳的外观，因为它使应用具有“箱中”效果，其中部分 UI（如导航窗格和网格）看似被切断。 但是，你可以进行优化以将部分 UI 延伸到屏幕边缘，以为应用提供更加电影化的效果。

### <a name="drawing-ui-to-the-edge"></a>将 UI 绘制到边缘

我们建议你使用特定 UI 元素来延伸到屏幕边缘，以向用户提供更多的沉浸式体验。 这些元素包括 [ScrollViewers](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ScrollViewer)、[nav panes](../controls-and-patterns/navigationview.md) 和 [CommandBars](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.CommandBar)。

另一方面，交互式元素和文本始终避开屏幕边缘以确保它们在某些电视上不会被切断，这一点也很重要。 我们建议你在屏幕边缘的 5% 内仅绘制非必要的视觉对象。 如 [UI 元素大小调整](#ui-element-sizing)中所述，遵循 Xbox One 主机的默认比例系数 200% 的 UWP 应用将利用 960 x 540 epx 的区域，因此在应用的 UI 中，你应避免在以下区域中放置必需 UI：

- 距离顶部和底部 27 epx
- 距离左侧和右侧 48 epx

以下部分介绍了如何使你的 UI 延伸到屏幕边缘。

#### <a name="core-window-bounds"></a>核心窗口边界

对于仅面向 10 英尺体验的 UWP 应用，使用核心窗口边界是更简单的选项。

在 `App.xaml.cs` 的 `OnLaunched` 方法中，添加以下代码：

```csharp
Windows.UI.ViewManagement.ApplicationView.GetForCurrentView().SetDesiredBoundsMode
    (Windows.UI.ViewManagement.ApplicationViewBoundsMode.UseCoreWindow);
```

通过此代码行，应用窗口将延伸到屏幕边缘，因此你需要将所有交互式和必需 UI 移动到之前所述的电视安全区域中。 临时 UI（如上下文菜单和打开的 [ComboBoxes](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ComboBox)）将自动保留在电视安全区域内。

![核心窗口边界](images/designing-for-tv/core-window-bounds.png)

#### <a name="pane-backgrounds"></a>窗格背景

导航窗格通常绘制在屏幕边缘附近，因此背景应延伸到电视不安全区域中，以免引入不可取的间距。 只需将导航窗格的背景颜色更改为应用的背景颜色即可达成目标。

按上述方法使用核心窗口边界会允许你将 UI 绘制到屏幕边缘，但之后对于 [SplitView](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.SplitView) 内容应使用正边距以确保内容位于电视安全区域内。

![延伸到屏幕边缘的导航窗格](images/designing-for-tv/tv-safe-areas-2.png)

在此处，导航窗格的背景已延伸到屏幕边缘，而其导航项会保留在电视安全区域内。
`SplitView` 的内容（在本例中为项的网格）已延伸到屏幕底部，以便它看起来会持续显示且不会被切断，而网格顶部仍然位于电视安全区域内。 （了解有关如何在[滚动列表和网格的末端](#scrolling-ends-of-lists-and-grids)中执行此操作的详细信息）。

以下代码段可实现此效果：

```xml
<SplitView x:Name="RootSplitView"
           Margin="48,0,48,0">
    <SplitView.Pane>
        <ListView x:Name="NavMenuList"
                  ContainerContentChanging="NavMenuItemContainerContentChanging"
                  ItemContainerStyle="{StaticResource NavMenuItemContainerStyle}"
                  ItemTemplate="{StaticResource NavMenuItemTemplate}"
                  ItemInvoked="NavMenuList_ItemInvoked"
                  ItemsSource="{Binding NavMenuListItems}"/>
    </SplitView.Pane>
    <Frame x:Name="frame"
           Navigating="OnNavigatingToPage"
           Navigated="OnNavigatedToPage"/>
</SplitView>
```

[CommandBar](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.CommandBar) 是另一个通常定位在应用的一个或多个边缘附近的窗格示例，因此在电视上，它的背景应延伸到屏幕边缘。 它通常还包含一个**更多**按钮，由右侧的“...”表示，该按钮应保留在电视安全区域中。 以下是实现所需交互和视觉效果的一些不同策略。

**选项 1**:更改`CommandBar`背景色为透明或与页面背景颜色相同：

```xml
<CommandBar x:Name="topbar"
            Background="{ThemeResource SystemControlBackgroundAltHighBrush}">
            ...
</CommandBar>
```

执行此操作将使 `CommandBar` 看起来像与页面其余部分位于同一个背景顶部，因此背景将无缝流动到屏幕边缘。

**选项 2**:添加背景和矩形的填充是相同的颜色作为`CommandBar`后台，并使其位于如下`CommandBar`和跨页的其余部分：

```xml
<Rectangle VerticalAlignment="Top"
            HorizontalAlignment="Stretch"      
            Fill="{ThemeResource SystemControlBackgroundChromeMediumBrush}"/>
<CommandBar x:Name="topbar"
            VerticalAlignment="Top"
            HorizontalContentAlignment="Stretch">
            ...
</CommandBar>
```

> [!NOTE]
> 如果使用此方法，请注意**更多**按钮将更改打开的 `CommandBar` 的高度（如有必要），以便在其图标下方显示 `AppBarButton` 的标签。 我们建议你将标签移动到其图标*右侧*以避免此大小调整。 有关详细信息，请参阅 [CommandBar 标签](#commandbar-labels)。

这两种方法还适用于本部分中列出的其他类型的控件。

#### <a name="scrolling-ends-of-lists-and-grids"></a>滚动列表和网格的末端

列表和网格所包含的项通常多于屏幕上同时可以容纳的数量。 在这种情况下，我建议你将列表或网格延伸到屏幕边缘。 水平滚动列表和网格应延伸到右边缘，而垂直滚动应延伸到底部。

![电视安全区域网格切断](images/designing-for-tv/tv-safe-area-grid-cutoff.png)

当列表或网格进行此类延伸时，将焦点视觉对象及其关联项保留在电视安全区域内很重要。

![滚动网格焦点应保留在电视安全区域中。](images/designing-for-tv/scrolling-grid-focus.png)

UWP 具有将焦点视觉对象保留在 [VisibleBounds](https://docs.microsoft.com/uwp/api/windows.ui.viewmanagement.applicationview.visiblebounds) 内部的功能，但你需要添加填充以确保列表/网格项可以滚动到安全区域的视图中。 具体来说，将正边距添加到 [ListView](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ListView) 或 [GridView](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.GridView) 的 [ItemsPresenter](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ItemsPresenter)，如以下代码片段所示：

```xml
<Style x:Key="TitleSafeListViewStyle"
       TargetType="ListView">
    <Setter Property="Template">
        <Setter.Value>
            <ControlTemplate TargetType="ListView">
                <Border BorderBrush="{TemplateBinding BorderBrush}"
                        Background="{TemplateBinding Background}"
                        BorderThickness="{TemplateBinding BorderThickness}">
                    <ScrollViewer x:Name="ScrollViewer"
                                  TabNavigation="{TemplateBinding TabNavigation}"
                                  HorizontalScrollMode="{TemplateBinding ScrollViewer.HorizontalScrollMode}"
                                  HorizontalScrollBarVisibility="{TemplateBinding ScrollViewer.HorizontalScrollBarVisibility}"
                                  IsHorizontalScrollChainingEnabled="{TemplateBinding ScrollViewer.IsHorizontalScrollChainingEnabled}"
                                  VerticalScrollMode="{TemplateBinding ScrollViewer.VerticalScrollMode}"
                                  VerticalScrollBarVisibility="{TemplateBinding ScrollViewer.VerticalScrollBarVisibility}"
                                  IsVerticalScrollChainingEnabled="{TemplateBinding ScrollViewer.IsVerticalScrollChainingEnabled}"
                                  IsHorizontalRailEnabled="{TemplateBinding ScrollViewer.IsHorizontalRailEnabled}"
                                  IsVerticalRailEnabled="{TemplateBinding ScrollViewer.IsVerticalRailEnabled}"
                                  ZoomMode="{TemplateBinding ScrollViewer.ZoomMode}"
                                  IsDeferredScrollingEnabled="{TemplateBinding ScrollViewer.IsDeferredScrollingEnabled}"
                                  BringIntoViewOnFocusChange="{TemplateBinding ScrollViewer.BringIntoViewOnFocusChange}"
                                  AutomationProperties.AccessibilityView="Raw">
                        <ItemsPresenter Header="{TemplateBinding Header}"
                                        HeaderTemplate="{TemplateBinding HeaderTemplate}"
                                        HeaderTransitions="{TemplateBinding HeaderTransitions}"
                                        Footer="{TemplateBinding Footer}"
                                        FooterTemplate="{TemplateBinding FooterTemplate}"
                                        FooterTransitions="{TemplateBinding FooterTransitions}"
                                        Padding="{TemplateBinding Padding}"
                                        Margin="0,27,0,27"/>
                    </ScrollViewer>
                </Border>
            </ControlTemplate>
        </Setter.Value>
    </Setter>
</Style>
```

你将之前的代码片段放置在页面或应用资源中，然后通过以下方式访问它：

```xml
<Page>
    <Grid>
        <ListView Style="{StaticResource TitleSafeListViewStyle}"
                  ... />
```

> [!NOTE]
> 此代码片段专门用于 `ListView`；对于 `GridView` 样式，请将 [ControlTemplate](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.controltemplate.targettype) 和 [Style](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ControlTemplate) 的 [TargetType](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Style) 属性设置为 `GridView`。

有关如何项放入视图，如果应用程序面向 1803年版或更高版本，可以使用更细粒度控制[UIElement.BringIntoViewRequested 事件](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.bringintoviewrequested)。 可以将其置于[ItemsPanel](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.itemscontrol.itemspanel)有关**ListView**/**GridView**捕获它之前的内部**ScrollViewer**执行，如以下代码片段中所示：

```xaml
<GridView x:Name="gridView">
    <GridView.ItemsPanel>
        <ItemsPanelTemplate>
            <ItemsWrapGrid Orientation="Horizontal"
                           BringIntoViewRequested="ItemsWrapGrid_BringIntoViewRequested"/>
        </ItemsPanelTemplate>
    </GridView.ItemsPanel>
</GridView>
```

```cs
// The BringIntoViewRequested event is raised by the framework when items receive keyboard (or Narrator) focus or 
// someone triggers it with a call to UIElement.StartBringIntoView.
private void ItemsWrapGrid_BringIntoViewRequested(UIElement sender, BringIntoViewRequestedEventArgs args)
{
    if (args.VerticalAlignmentRatio != 0.5)  // Guard against our own request
    {
        args.Handled = true;
        // Swallow this request and restart it with a request to center the item.  We could instead have chosen
        // to adjust the TargetRect’s Y and Height values to add a specific amount of padding as it bubbles up, 
        // but if we just want to center it then this is easier.

        // (Optional) Account for sticky headers if they exist
        var headerOffset = 0.0;
        var itemsWrapGrid = sender as ItemsWrapGrid;
        if (gridView.IsGrouping && itemsWrapGrid.AreStickyGroupHeadersEnabled)
        {
            var header = gridView.GroupHeaderContainerFromItemContainer(args.TargetElement as GridViewItem);
            if (header != null)
            {
                headerOffset = ((FrameworkElement)header).ActualHeight;
            }
        }

        // Issue a new request
        args.TargetElement.StartBringIntoView(new BringIntoViewOptions()
        {
            AnimationDesired = true,
            VerticalAlignmentRatio = 0.5, // a normalized alignment position (0 for the top, 1 for the bottom)
            VerticalOffset = headerOffset, // applied after meeting the alignment ratio request
        });
    }
}
```

## <a name="colors"></a>颜色

默认情况下，通用 Windows 平台会将应用的颜色缩放到电视安全范围（有关更多信息，请参阅[电视安全颜色](#tv-safe-colors)），以便应用在任何电视上都显示正常。 此外，你可以对应用使用的颜色集进行一些改进以改进电视上的视觉体验。

### <a name="application-theme"></a>应用程序主题

你可以根据应用的需求选择**应用程序主题**（深色或浅色），也可以选择退出主题。 阅读有关[颜色主题](../style/color.md)的主题中一般建议的详细信息。

UWP 还允许应用基于运行这些应用的设备所提供的系统设置动态设置主题。
尽管 UWP 始终遵循用户指定的主题设置，但每台设备还提供相应的默认主题。
由于 Xbox One 的性质，即预期其*媒体*体验多于*办公*体验，因此它默认为深色系统主题。
如果你的应用主题基于系统设置，则预期它在 Xbox One 上默认为深色。

### <a name="accent-color"></a>主题色

UWP 提供一种便捷方式来公开用户从其系统设置中选择的**主题色**。

在 Xbox One 上，用户能够选择用户颜色，就像他们可以在电脑上选择主题色一样。
只要你的应用通过画笔或颜色资源调用这些主题色，就会使用用户在系统设置中选择的颜色。 请注意，Xbox One 上的主题色按用户而不是按系统设置。

另请注意，Xbox One 上的用户颜色集与电脑、手机和其他设备上的不同。

只要应用使用画笔资源（如 **SystemControlForegroundAccentBrush**）或颜色资源 (**SystemAccentColor**)，或者直接通过 [UIColorType.Accent*](https://docs.microsoft.com/uwp/api/Windows.UI.ViewManagement.UIColorType) API 调用主题色，这些颜色就将替换为适用于 Xbox One 的主题色。 还通过与电脑和手机上相同的方式从系统中提取高对比度画笔颜色。

若要了解有关主题色的一般详细信息，请参阅[主题色](../style/color.md#accent-color)。

### <a name="color-variance-among-tvs"></a>电视之间的颜色变化

在针对电视进行设计时，请注意，根据呈现颜色的电视，颜色会以非常不同的方式显示。 不要假设颜色看起来会与在监视器上完全相同。 如果你的应用依赖细微的颜色差异来区分 UI 的各个部分，则颜色可能会混合在一起，并且用户可能会混淆。 无论用户使用何种电视，尝试使用差异足够大的颜色，以便用户能够清楚地区分它们。

### <a name="tv-safe-colors"></a>电视安全颜色

颜色的 RGB 值表示红色、绿色和蓝色的浓度。 电视无法很好地处理极强的浓度&mdash;它们会产生奇怪的色带效果，或在某些电视上显示为褪色。 此外，高浓度颜色可能导致高度曝光（附近的像素开始绘制相同的颜色）。 尽管对于哪些颜色应视为电视安全颜色存在不同的学说，但 RGB 值为 16-235（或十六进制的 10-EB）内的颜色一般可安全用于电视。

![电视安全颜色范围](images/designing-for-tv/tv-safe-colors-2.png)

过去，Xbox 上的应用必须定制其颜色才能位于这种“电视安全”颜色范围内；但是，从 Fall Creators Update 开始，Xbox One 可自动将全范围内容缩放为电视安全范围。 这意味着大多数应用开发人员将无需担心电视安全颜色问题。

> [!IMPORTANT]
> 已位于电视安全颜色范围中的视频内容在使用[媒体基础](https://docs.microsoft.com/windows/desktop/medfound/microsoft-media-foundation-sdk)播放时不会应用这种颜色缩放效果。

如果你要使用 DirectX 11 或 DirectX 12 开发应用并创建自己的交换链来呈现 UI 或视频，你可以通过调用 [IDXGISwapChain3::SetColorSpace1](https://docs.microsoft.com/windows/desktop/api/dxgi1_4/nf-dxgi1_4-idxgiswapchain3-setcolorspace1) 来指定要使用的颜色空间，这将让系统了解其是否需要缩放颜色。

## <a name="guidelines-for-ui-controls"></a>UI 控件指南

有多个 UI 控件可跨多台设备运行良好，但在电视上使用时有些特定的注意事项。 请阅读在针对 10 英尺体验进行设计时使用这些控件的一些最佳做法。

### <a name="pivot-control"></a>Pivot 控件

[Pivot](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Pivot) 通过选择不同标题或选项卡来提供应用内视图的快速导航。 该控件会对具有焦点的标题加下划线，从而在使用游戏板/遥控器时使当前选中标题的显示较为明显。

![Pivot 下划线](images/designing-for-tv/pivot-underline.png)

你可以将 [Pivot.IsHeaderItemsCarouselEnabled](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.pivot.isheaderitemscarouselenabledproperty) 属性设置为 `true`，以便透视表始终保持在相同的位置，而不是使选定的透视表标题始终移动到第一个位置。 这对大屏幕显示（如电视）来说是更佳的体验，因为标题换行可能会干扰用户。 如果所有透视表标题不能全部适合屏幕，有一个滚动条可以让客户看到其他标题；但是，你应确保它们全部适合屏幕以提供最佳体验。 有关详细信息，请参阅[表和透视表](/windows/uwp/design/controls-and-patterns/pivot)。

### <a name="navigation-pane-a-namenavigation-pane-"></a>导航窗格 <a name="navigation-pane" />

导航窗格（也称为*汉堡菜单*）是 UWP 应用中常用的导航控件。 通常，该窗格内含多个从列表样式菜单中选择的选项，用于将用户转到其他页面。 此窗格通常一开始以折叠方式显示以节省空间，用户可以通过单击某个按钮来打开它。

用户很容易通过鼠标和触摸操作访问导航窗格，但游戏板/遥控器将使这些窗格不易访问，因为用户必须导航到某个按钮才能打开窗格。 因此，好的做法是让“视图”按钮打开导航窗格，因此，好的做法是让**视图**按钮打开导航窗格，以及允许用户通过一直向左导航页面来打开该窗格。以及允许用户通过一直向左导航页面来打开该窗格。 可以在[编程焦点导航](../input/focus-navigation-programmatic.md#split-view-code-sample)文档中找到有关如何实现此设计模式的代码示例。 这样可以使用户轻松访问窗格内容。 有关导航窗格在不同的屏幕大小中的行为以及游戏板/遥控器导航的最佳做法的详细信息，请参阅[导航窗格](../controls-and-patterns/navigationview.md)。

### <a name="commandbar-labels"></a>CommandBar 标签

最好是将标签放置于 [CommandBar](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.CommandBar) 上的图标右侧，以便最大程度降低标签高度并保持一致性。 为此，你可以将 [CommandBar.DefaultLabelPosition](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.commandbar.defaultlabelpositionproperty) 属性设置为 `CommandBarDefaultLabelPosition.Right`。

![图标右侧带有标签的 CommandBar](images/designing-for-tv/commandbar.png)

设置此属性还会导致标签始终显示，这适用于 3 米体验，因为它最大程度减少了用户的单击次数。 这也是可供其他设备类型遵循的绝佳模型。

### <a name="tooltip"></a>工具提示

引入了 [Tooltip](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ToolTip) 控件，可作为一种在用户将鼠标悬停在元素上或点击并按住元素上的图片时在 UI 中提供更多信息的方法。 对于游戏板和遥控器，`Tooltip` 将在元素得到焦点的短暂时间后显示、在屏幕上保留一小段时间，然后消失。 如果使用了太多 `Tooltip`，此行为可能令人分心。 在针对电视进行设计时，请尝试避免使用 `Tooltip`。

### <a name="button-styles"></a>按钮样式

当标准 UWP 按钮适用于电视时，某些按钮视觉样式能更好地将注意力吸引到 UI，你可能希望针对所有平台考虑这些样式，这对于 10 英尺体验尤其有利，因为可以受益于清晰传达焦点所在的位置。 若要阅读有关这些样式的详细信息，请参阅[按钮](../controls-and-patterns/buttons.md)。

### <a name="nested-ui-elements"></a>嵌套 UI 元素

嵌套 UI 公开包含在容器 UI 元素内的可操作项，其中嵌套项和容器项可彼此独立捕获焦点。

嵌套 UI 较适合某些输入类型，但对于依赖 XY 导航的游戏板和遥控器不一定适用。 请务必遵循本主题中的指南操作，确保你的 UI 已针对 10 英尺环境进行了优化，并且用户可以轻松地访问所有可交互元素。 一个常见的解决方案是将嵌套的 UI 元素中放置`ContextFlyout`。

有关嵌套 UI 的详细信息，请参阅[列表项中的嵌套 UI](../controls-and-patterns/nested-ui.md)。

### <a name="mediatransportcontrols"></a>MediaTransportControls

[MediaTransportControls](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.MediaTransportControls) 元素通过提供默认的播放体验（即允许用户播放、暂停、打开隐藏式字幕等），允许用户与其媒体交互。 此控件是 [MediaPlayerElement](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.MediaPlayerElement) 的一个属性，支持以下两个布局选项：*单行*和*双行*。 在单行布局中，滑块和播放按钮位于同一行，而播放/暂停按钮位于滑块左侧。 在双行布局中，滑块单独占用一行，而播放按钮位于单独的下一行。 当针对 10 英尺体验进行设计时，应使用双行布局，因为它能为游戏板提供更好的导航。 若要启用双行布局，请在 `MediaPlayerElement` 的 [TransportControls](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.mediaplayerelement.transportcontrols) 属性中的 `MediaTransportControls` 元素上设置 `IsCompact="False"`。

```xml
<MediaPlayerElement x:Name="mediaPlayerElement1"  
                    Source="Assets/video.mp4"
                    AreTransportControlsEnabled="True">
    <MediaPlayerElement.TransportControls>
        <MediaTransportControls IsCompact="False"/>
    </MediaPlayerElement.TransportControls>
</MediaPlayerElement>
```  

请访问[媒体播放](../controls-and-patterns/media-playback.md)，了解有关将媒体添加到应用的详细信息。

> ![注意] `MediaPlayerElement` 仅在 Windows 10 版本 1607 及更高版本中可用。 如果要针对早期版本的 Windows 10 开发应用，你将需要改用 [MediaElement](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.MediaElement)。 上述建议也适用于 `MediaElement`，并且可通过相同方式访问 `TransportControls` 属性。

### <a name="search-experience"></a>搜索体验

搜索内容是 10 英尺体验中最常执行的功能。 如果你的应用提供搜索体验，这对于用户通过将游戏板上的 **Y** 按钮用做快捷键快速访问它非常有用。

大多数客户应该已经熟悉此快捷键，但你可以将 **Y** 字形添加到 UI（如果可以）来指示客户可使用该按钮访问搜索功能。 如果你确实添加此提示，请务必使用 **Segoe Xbox MDL2 Symbol** 字体中的符号（对于 XAML 应用为 `&#xE3CC;`，对于 HTML 应用则为 `\E426`）以提供与 Xbox shell 和其他应用的一致性。

> [!NOTE]
> 因为 **Segoe Xbox MDL2 Symbol** 字体仅在 Xbox 上可用，因此符号不会在你的电脑上正确显示。 但是，在部署到 Xbox 后，它将显示在电视上。

由于 **Y** 按钮仅在游戏板上可用，请确保提供其他访问搜索的方法（如 UI 中的按钮）。 否则，一些客户可能无法访问该功能。

在 10 英尺体验中，客户通常可以更轻松地使用全屏搜索，因为屏幕上的空间受限。 无论你是进行全屏还是部分屏幕“就地”搜索，我们建议在用户打开搜索体验时，屏幕上键盘显示为已打开，随时供客户输入搜索词。

## <a name="custom-visual-state-trigger-for-xbox"></a>Xbox 的自定义视觉状态触发器

若要针对 3 米体验定制 UWP 应用，我们建议你在应用检测到它已在 Xbox 主机上启动时更改布局。 达到此目的一个方法是使用自定义*视觉状态触发器*。 当你想要在 **Blend for Visual Studio** 中进行编辑时，视觉状态触发器最有用。 以下代码片段显示如何创建适用于 Xbox 的视觉状态触发器：

```xml
<VisualStateManager.VisualStateGroups>
    <VisualStateGroup>
        <VisualState>
            <VisualState.StateTriggers>
                <triggers:DeviceFamilyTrigger DeviceFamily="Windows.Xbox"/>
            </VisualState.StateTriggers>
            <VisualState.Setters>
                <Setter Target="RootSplitView.OpenPaneLength"
                        Value="368"/>
                <Setter Target="RootSplitView.CompactPaneLength"
                        Value="96"/>
                <Setter Target="NavMenuList.Margin"
                        Value="0,75,0,27"/>
                <Setter Target="Frame.Margin"
                        Value="0,27,48,27"/>
                <Setter Target="NavMenuList.ItemContainerStyle"
                        Value="{StaticResource NavMenuItemContainerXboxStyle}"/>
            </VisualState.Setters>
        </VisualState>
    </VisualStateGroup>
</VisualStateManager.VisualStateGroups>
```

若要创建触发器，请将以下类添加到应用。 这是由上述 XAML 代码引用的类：

```csharp
class DeviceFamilyTrigger : StateTriggerBase
{
    private string _currentDeviceFamily, _queriedDeviceFamily;

    public string DeviceFamily
    {
        get
        {
            return _queriedDeviceFamily;
        }

        set
        {
            _queriedDeviceFamily = value;
            _currentDeviceFamily = AnalyticsInfo.VersionInfo.DeviceFamily;
            SetActive(_queriedDeviceFamily == _currentDeviceFamily);
        }
    }
}
```

在你添加了自定义触发器后，每当你的应用检测到它正在 Xbox One 主机上运行时，你的应用将自动进行你在 XAML 代码中指定的布局修改。

检查你的应用是否在 Xbox 上运行，然后进行相应调整的另一种方法是通过代码。 可以使用以下简单变量来检查你的应用是否在 Xbox 上运行：

```csharp
bool IsTenFoot = (Windows.System.Profile.AnalyticsInfo.VersionInfo.DeviceFamily ==
                    "Windows.Xbox");
```

然后，你可以遵循此检查对代码块中的 UI 进行相应调整。 

## <a name="summary"></a>摘要

针对 10 英尺体验进行设计需要考虑特殊的注意事项，这些注意事项有别于针对任何其他平台进行设计。 当然直接将 UWP 应用移植到 Xbox One 也能使其工作，但它不一定已针对 10 英尺体验进行优化，并且可能导致用户沮丧。 按照本文中的指南进行操作可确保你的应用在电视上达到最佳状态。

## <a name="related-articles"></a>相关文章

- [通用 Windows 平台 (UWP) 应用的设备入门](index.md)
- [游戏手柄和遥控器之间的交互](../input/gamepad-and-remote-interactions.md)
- [在 UWP 应用中的声音](../style/sound.md)
