---
Description: 优化你的应用程序，以便从 Xbox 游戏板和远程控制输入。
title: 游戏板和遥控器交互
ms.assetid: 784a08dc-2736-4bd3-bea0-08da16b1bd47
label: Gamepad and remote interactions
template: detail.hbs
isNew: true
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 207ad9cb3008f1a36402e413b7e246aa2135ae26
ms.sourcegitcommit: 0dee502484df798a0595ac1fe7fb7d0f5a982821
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/08/2020
ms.locfileid: "82970162"
---
# <a name="gamepad-and-remote-control-interactions"></a>游戏板和遥控器交互

![键盘和游戏板图像](images/keyboard/keyboard-gamepad.jpg)

***各种交互体验在游戏板、远程控制和键盘之间共享***

在 Windows 应用程序应用程序中构建交互体验，以确保你的应用程序可供使用，并可通过传统的 Pc、笔记本电脑和平板电脑（鼠标、键盘、触摸等）输入类型进行访问，同时还可实现电视和 Xbox *10 英尺*体验（例如游戏板和遥控器）的典型输入类型。

有关 Windows 应用程序的常规设计指南，请参阅在*10 英尺*体验中[设计 Xbox 和 TV](../devices/designing-for-tv.md) 。

## <a name="overview"></a>概述

在本主题中，我们将讨论你应该在交互设计中考虑的内容（或者，如果平台看起来不是这样，则可以提供指导、建议和建议）来构建使用愉快的 Windows 应用程序，而不考虑设备、输入类型或用户能力和首选项。

要点是，当应用程序在*10 英尺*环境中时，您的应用程序应该像在*2*英尺环境中一样直观且易于使用（反之亦然）。 支持用户的首选设备，使 UI 专注于清晰和不容置疑、排列内容以便导航一致且可预测，并为用户提供尽可能短的路径。

> [!NOTE]
> 本主题中的大多数代码片段都在 XAML/c # 中;但是，原则和概念适用于所有 Windows 应用。 如果要开发适用于 Xbox 的 HTML/JavaScript Windows 应用，请查看 GitHub 上的优秀[TVHelpers](https://github.com/Microsoft/TVHelpers/wiki)库。


## <a name="optimize-for-both-2-foot-and-10-foot-experiences"></a>优化2英尺和10英尺体验

我们至少建议你对应用程序进行测试，以确保它们能在2英尺和10英尺方案中正常运行，并且 Xbox[游戏板和远程控制](#gamepad-and-remote-control)功能可被发现并可访问。

下面是一些其他方法，你可以通过这种方式优化你的应用程序，以便在2英尺和10英尺体验以及所有输入设备（每个链接都链接到本主题中的相应部分）中使用。

> [!NOTE]
> 由于 Xbox gamepads 和远程控制支持许多 Windows 键盘行为和体验，因此，这些建议适用于这两种输入类型。 有关更详细的键盘信息，请参阅[键盘交互](keyboard-interactions.md)。

| 功能        | 说明           |
| -------------------------------------------------------------- |--------------------------------|
| [XY 焦点导航和交互](#xy-focus-navigation-and-interaction) | 通过**XY 焦点导航**，用户可以在应用的 UI 周围导航。 但是，这会限制用户只能向上、向下、向左和向右导航。 本部分概述了处理此情况的建议和其他注意事项。 |
| [鼠标模式](#mouse-mode)|对于某些类型的应用程序（例如地图、绘图和绘图应用程序），XY 焦点导航并不可行，甚至可能不可行。 在这些情况下，**鼠标模式**使用户能够在游戏板或遥控器上自由导航，就像 PC 上的鼠标一样。|
| [焦点视觉对象](#focus-visual)  | 焦点视觉对象是突出显示当前聚焦的 UI 元素的边框。 这可帮助用户快速识别他们要浏览或与之交互的 UI。  |
| [专注参与](#focus-engagement) | 当 UI 元素具有焦点以便与之进行交互时，用户需要在游戏板或遥控器上按下 **/选择**按钮。 |
| [硬件按钮](#hardware-buttons) | 游戏板和远程控制提供了非常不同的按钮和配置。 |

## <a name="gamepad-and-remote-control"></a>游戏板和遥控器

就像键盘和鼠标适用于电脑、而触摸适用于手机和平板电脑一样，游戏板和遥控器是 10 英尺体验的主要输入设备。
本部分介绍了硬件按钮的定义和作用。
在 [XY 焦点导航和交互](#xy-focus-navigation-and-interaction)和[鼠标模式](#mouse-mode)中，你将了解如何在使用这些输入设备时优化你的应用。

全新的游戏板和遥控器行为的质量取决于键盘在应用中受到多大的支持。 确保你的应用适用于游戏板/遥控器的一个好方法是，确保它在电脑上适用于键盘，然后使用游戏板/遥控器进行测试以查找 UI 中的缺陷。

### <a name="hardware-buttons"></a>硬件按钮

在整个文档中，将使用下图中给定的名称指代按钮。

![游戏板和遥控器按钮图](images/designing-for-tv/hardware-buttons-gamepad-remote.png)

如该图中所示，某些按钮在游戏板上受支持，但在遥控器上不受支持，反之亦然。 尽管你可以使用仅在一台输入设备上受支持的按钮来加快导航 UI 的速度，但请注意将这些按钮用于关键交互可能会产生用户无法与特定 UI 部分交互的情况。

下表列出了 Windows 应用支持的所有硬件按钮，以及哪些输入设备支持这些按钮。

| Button                    | 游戏板   | 远程控制    |
|---------------------------|-----------|-------------------|
| A/“选择”按钮           | 是       | 是               |
| B/“后退”按钮             | 是       | 是               |
| 方向键（方向键）   | 是       | 是               |
| “菜单”按钮               | 是       | 是               |
| “视图”按钮               | 是       | 是               |
| X 和 Y 按钮           | 是       | 否                |
| 左摇杆                | 是       | 否                |
| 右摇杆               | 是       | 否                |
| 左和右扳机键   | 是       | 否                |
| 左和右缓冲键    | 是       | 否                |
| OneGuide 按钮           | 否        | 是               |
| 音量按钮             | 否        | 是               |
| 频道按钮            | 否        | 是               |
| 媒体控制按钮     | 否        | 是               |
| “静音”按钮               | 否        | 是               |

### <a name="built-in-button-support"></a>内置按钮支持

UWP 会自动将现有键盘输入行为映射到游戏板和远程控制输入。 下表列出了这些内置映射。

| Keyboard              | 游戏板/遥控器                        |
|-----------------------|---------------------------------------|
| 箭头键            | 方向键（以及游戏板上的左摇杆）    |
| 空格键              | A/“选择”按钮                       |
| Enter                 | A/“选择”按钮                       |
| Escape                | B/“后退”按钮*                        |

\*当应用程序不处理 B 按钮的[KeyDown](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.keydown)事件和[KeyUp](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.keyup)事件时，将激发[SystemNavigationManager BackRequested](https://docs.microsoft.com/uwp/api/windows.ui.core.systemnavigationmanager.backrequested)事件，这将导致在应用内后退导航。 但是，你必须自行实现此操作，如以下代码片段中所示：

```csharp
// This code goes in the MainPage class

public MainPage()
{
    this.InitializeComponent();

    // Handling Page Back navigation behaviors
    SystemNavigationManager.GetForCurrentView().BackRequested +=
        SystemNavigationManager_BackRequested;
}

private void SystemNavigationManager_BackRequested(
    object sender,
    BackRequestedEventArgs e)
{
    if (!e.Handled)
    {
        e.Handled = this.BackRequested();
    }
}

public Frame AppFrame { get { return this.Frame; } }

private bool BackRequested()
{
    // Get a hold of the current frame so that we can inspect the app back stack
    if (this.AppFrame == null)
        return false;

    // Check to see if this is the top-most page on the app back stack
    if (this.AppFrame.CanGoBack)
    {
        // If not, set the event to handled and go back to the previous page in the
        // app.
        this.AppFrame.GoBack();
        return true;
    }
    return false;
}
```

> [!NOTE]
> 如果 B 按钮用于返回，则 UI 中不会显示“后退”按钮。 如果你使用的是[导航视图](../controls-and-patterns/navigationview.md)，则“后退”按钮将自动隐藏。 有关向后导航的详细信息，请参阅[Windows 应用的导航历史记录和向后导航](../basics/navigation-history-and-backwards-navigation.md)。

Xbox one 上的 Windows 应用程序还支持按**菜单**按钮打开上下文菜单。 有关详细信息，请参阅 [CommandBar 和 ContextFlyout](#commandbar-and-contextflyout)。

### <a name="accelerator-support"></a>加速器支持

加速器按钮是可用于加快在 UI 中导航的按钮。 但是，这些按钮可能是特定输入设备所独有的，因此请记住并非所有用户都能够使用这些功能。 事实上，游戏板是当前唯一支持 Xbox one 上 Windows 应用加速器功能的输入设备。

下表列出了内置于 UWP 以及可自行实现的加速器支持。 在自定义 UI 中利用这些行为提供一致且友好的用户体验。

| 交互   | 键盘/鼠标   | 游戏板      | 内置用于：  | 建议用于： |
|---------------|------------|--------------|----------------|------------------|
| 向上/向下翻页  | 向上/向下翻页 | 左/右扳机键 | [CalendarView](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.CalendarView)、[ListBox](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ListBox)、[ListViewBase](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ListViewBase)、[ListView](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ListView)、`ScrollViewer`、[Selector](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Primitives.Selector)、[LoopingSelector](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Primitives.LoopingSelector)、[ComboBox](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ComboBox)、[FlipView](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.FlipView) | 支持垂直滚动的视图
| 向左/向右翻页 | None | 左/右缓冲键 | [Pivot](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Pivot)、[ListBox](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ListBox)、[ListViewBase](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ListViewBase)、[ListView](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ListView)、`ScrollViewer`、[Selector](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Primitives.Selector)、[LoopingSelector](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Primitives.LoopingSelector)、[FlipView](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.FlipView) | 支持水平滚动的视图
| 放大/缩小        | Ctrl +/- | 左/右扳机键 | None | `ScrollViewer`支持放大和缩小的视图 |
| 打开/关闭导航窗格 | None | 查看 | None | 导航窗格​​ |
| 搜索 | None | Y 按钮 | None | 应用中主要搜索功能的快捷方式 |
| [打开上下文菜单](#commandbar-and-contextflyout) | 右键单击 | “菜单”按钮 | [ContextFlyout](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.UIElement.ContextFlyout) | 上下文菜单 |

## <a name="xy-focus-navigation-and-interaction"></a>XY 焦点导航和交互

如果应用支持键盘的适当焦点导航，这将很好地转换到游戏板和遥控器。
使用箭头键的导航映射到**方向键**（以及游戏板上的**左摇杆**），而与 UI 元素的交互映射到 **输入/选择**键（请参阅[游戏板和远程控制](#gamepad-and-remote-control)）。

键盘和游戏板会使用许多事件和属性&mdash;它们会触发 `KeyDown` 和 `KeyUp` 事件，并且两者仅导航到具有 `IsTabStop="True"` 和 `Visibility="Visible"` 属性的控件。 有关键盘设计指南，请参阅[键盘交互](../input/keyboard-interactions.md)。

如果已正确实现键盘支持，你的应用理应良好地工作；但是，若要支持每一种方案，可能需要一些额外的工作。 思考你的应用的具体需求以尽可能提供最佳的用户体验。

> [!IMPORTANT]
> 对于在 Xbox One 上运行的 Windows 应用，将默认启用鼠标模式。 若要禁用鼠标模式并启用 XY 焦点导航，请设置 `Application.RequiresPointerMode=WhenRequested`。

### <a name="debugging-focus-issues"></a>调试焦点问题

[FocusManager.GetFocusedElement](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.focusmanager.getfocusedelement) 方法会告知你当前具有焦点的元素。 这对于焦点视觉位置可能不明显的情形非常有用。 你可以将此信息记录到 Visual Studio 输出窗口，如下所示：

```csharp
page.GotFocus += (object sender, RoutedEventArgs e) =>
{
    FrameworkElement focus = FocusManager.GetFocusedElement() as FrameworkElement;
    if (focus != null)
    {
        Debug.WriteLine("got focus: " + focus.Name + " (" +
            focus.GetType().ToString() + ")");
    }
};
```

有三个常见原因可能会导致 XY 导航不能按你预期的方式工作：

* 错误设置 [IsTabStop](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.control.istabstop) 或 [Visibility](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.visibility) 属性。
* 获取焦点的控件实际大于你认为&mdash; XY 导航看到的该控件的总大小（[ActualWidth](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.actualwidth) 和 [ActualHeight](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.actualheight)），不仅限于呈现关注内容的控件部分。
* 一个可获得焦点的控件基于其他控件之上&mdash;XY 导航不支持层叠显示的控件。

如果在修复这些问题后，XY 导航仍未按预期的方式工作，你可以使用[替代默认导航](#overriding-the-default-navigation)中所述的方法手动指向你希望获取焦点的元素。

如果 XY 导航按预期方式工作，但不显示任何焦点视觉，以下问题之一可能是原因所在：

* 你对控件重新模板化，但并未包含焦点视觉。 设置 `UseSystemFocusVisuals="True"` 或手动添加焦点视觉。
* 通过调用 `Focus(FocusState.Pointer)` 移动焦点。 [FocusState](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.FocusState) 参数控制焦点视觉的变化。 通常应该将该参数设置为 `FocusState.Programmatic`，以使焦点视觉保持可见（如果之前为可见）或隐藏（如果之前为隐藏）。

本部分的其余内容会详细介绍使用 XY 导航时遇到的常规设计挑战，并提供解决这些挑战的多种方法。

### <a name="inaccessible-ui"></a>无法访问的 UI

由于 XY 焦点导航限制用户向上、向下、向左和向右移动，因此你最终可能得到部分 UI 无法访问的方案。
下图演示了 XY 焦点导航不支持的 UI 布局类型的示例。
请注意，中间的元素无法通过使用游戏板/遥控器来访问，因为垂直和水平导航将设置优先级，而中间元素永远不会是足以得到焦点的高优先级。

![四个角的元素，中间的元素无法访问](images/designing-for-tv/2d-navigation-best-practices-ui-layout-to-avoid.png)

如果由于某些原因无法重新排列 UI，请使用下一部分中所述的技术之一来重写默认焦点行为。

### <a name="overriding-the-default-navigation"></a>替代默认导航

尽管通用 Windows 平台尝试确保方向键/左摇杆导航对用户有意义，但无法保证行为已针对应用的意图进行优化。
确保导航已针对应用进行优化的最佳方式是使用游戏板测试它，然后确认用户可以通过对应用方案有意义的方式访问每个 UI 元素。 如果你的应用方案需要并非通过所提供的 XY 焦点导航实现的行为，请考虑以下部分中的以下建议和/或替代该行为以将焦点放置在逻辑项上。

以下代码段显示了你可以如何替代 XY 焦点导航行为：

```xml
<StackPanel>
    <Button x:Name="MyBtnLeft"
            Content="Search" />
    <Button x:Name="MyBtnRight"
            Content="Delete"/>
    <Button x:Name="MyBtnTop"
            Content="Update" />
    <Button x:Name="MyBtnDown"
            Content="Undo" />
    <Button Content="Home"  
            XYFocusLeft="{x:Bind MyBtnLeft}"
            XYFocusRight="{x:Bind MyBtnRight}"
            XYFocusDown="{x:Bind MyBtnDown}"
            XYFocusUp="{x:Bind MyBtnTop}" />
</StackPanel>
```

在此情况下，当焦点在 `Home` 按钮上且用户导航到左侧时，焦点将移动到 `MyBtnLeft` 按钮；如果用户导航到右侧，焦点将移动到 `MyBtnRight` 按钮；依此类推。

若要防止焦点从控件以特定方向移动，请使用 `XYFocus*` 属性将其指向相同的控件：

```xml
<Button Name="HomeButton"  
        Content="Home"  
        XYFocusLeft ="{x:Bind HomeButton}" />
```

如果使用这些 `XYFocus` 属性，当下一焦点候选项位于其可视化树之外时，除非具有焦点的子对象使用相同的 `XYFocus` 属性，否则控件父对象也可以强制其子对象的导航。

```xml
<StackPanel Orientation="Horizontal" Margin="300,300">
    <UserControl XYFocusRight="{x:Bind ButtonThree}">
        <StackPanel>
            <Button Content="One"/>
            <Button Content="Two"/>
        </StackPanel>
    </UserControl>
    <StackPanel>
        <Button x:Name="ButtonThree" Content="Three"/>
        <Button Content="Four"/>
    </StackPanel>
</StackPanel>
```

在上面的示例中，如果焦点在 `Button` 二上并且用户导航至右侧，则最佳焦点候选项为 `Button` 四；但是，在焦点位于其可视化树之外时，焦点将移动到 `Button` 三，因为父 `UserControl` 将强制它导航到此处。

### <a name="path-of-least-clicks"></a>最少单击路径

尝试允许用户以最少的单击数执行最常见的任务。 在下面的示例中， [TextBlock](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TextBlock)放置在 "**播放**" 按钮（最初获得焦点）和一个常用元素之间，因此在优先级任务之间放置了不必要的元素。

![导航最佳做法可提供最少单击路径。](images/designing-for-tv/2d-navigation-best-practices-provide-path-with-least-clicks.png)

在下面的示例中， [TextBlock](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TextBlock)放置在 "**播放**" 按钮的上方。
只需重新排列 UI 以使优先任务之间不放置不必要的元素即可大幅提高应用的可用性。

![TextBlock 已移到“播放”按钮上方，以使其不再位于优先任务之间。](images/designing-for-tv/2d-navigation-best-practices-provide-path-with-least-clicks-2.png)

### <a name="commandbar-and-contextflyout"></a>CommandBar 和 ContextFlyout

在使用 [CommandBar](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.CommandBar) 时，请记住[问题：位于长滚动列表/网格之后的 UI 元素](#problem-ui-elements-located-after-long-scrolling-list-grid)中所提到的滚动浏览列表的问题。 下图显示了列表/网格底部带有 `CommandBar` 的 UI 布局。 用户需要向下一直滚动浏览列表/网格才能到达 `CommandBar`。

![位于列表/网格底部的 CommandBar](images/designing-for-tv/2d-navigation-best-practices-commandbar-and-contextflyout.png)

如果你将  放置在列表/网格`CommandBar` *上方*会怎么样？ 尽管向下滚动列表/网格的用户必须重新向上滚动才能到达 `CommandBar`，但与之前的配置相比，导航数略少。 请注意，这假设你的应用的初始焦点放置在 `CommandBar` 旁边或上方；如果初始焦点在列表/网格下方，则此方法不起作用。 如果这些 `CommandBar` 项是无需经常访问的全局操作项（如 **“同步”** 按钮），则可以将它们放置在列表/网格上方。

尽管你无法垂直堆叠 `CommandBar` 的项，但相对于滚动方向放置它们（例如，垂直滚动列表的左侧或右侧，或者水平滚动列表的顶部或底部）是另一个需要考虑的选项（如果适用于你的 UI 布局）。

如果应用的 `CommandBar` 具有需要便于用户访问的项，你可能要考虑将这些项放置在 [ContextFlyout](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.contextflyout) 内部，并将它们从 `CommandBar` 中删除。 `ContextFlyout`是[UIElement](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.UIElement)的属性，是与该元素关联的[上下文菜单](../controls-and-patterns/dialogs-and-flyouts/index.md)。 在电脑上，右键单击带有 `ContextFlyout` 的元素时，会弹出上下文菜单。 在 Xbox one 上，当焦点位于此类元素上**时，将**发生这种情况。

### <a name="ui-layout-challenges"></a>UI 布局挑战

由于 XY 焦点导航的性质，某些 UI 布局更具挑战性，应根据具体情况进行评估。 尽管不存在单一的“正确”方法，并且选择哪个解决方案取决于应用的具体需求，但你可以利用某些技术来创造出色的电视体验。

为了更好地了解这一点，让我们查看一个虚构的应用，该应用演示了其中的一些问题以及克服它们的技术。

> [!NOTE]
> 此虚构应用旨在演示 UI 问题及其潜在解决方案，而非演示你的特定应用的最佳用户体验。

以下是一个虚构的房地产应用，该应用显示可供销售的房屋列表、地图、房产说明以及其他信息。 此应用提出了三项可通过使用以下技术克服的挑战：

- [UI 重新排列](#ui-rearrange)
- [专注参与](#engagement)
- [鼠标模式](#mouse-mode)

![虚构房地产应用](images/designing-for-tv/2d-focus-navigation-and-interaction-real-estate-app.png)

#### <a name="problem-ui-elements-located-after-long-scrolling-listgrid"></a>问题：位于长滚动列表/网格之后的 UI 元素 <a name="problem-ui-elements-located-after-long-scrolling-list-grid"></a>

下图所示的房产 [ListView](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ListView) 是一个非常长的滚动列表。 如果 `ListView` 上*不*要求[占用](#focus-engagement)，当用户导航到该列表时，焦点将放置在列表中的第一个项上。 若要使用户到达**上一步**或**下一步**按钮，他们必须浏览列表中的所有项。 在这种难以要求用户遍历整个列表&mdash;即，当列表太长，无法接受此体验时&mdash;的情况下，你可能希望考虑其他选项。

![房地产应用：带有 50 个项的列表，需要单击 51 次才能到达下方的按钮](images/designing-for-tv/2d-focus-navigation-and-interaction-real-estate-app-list.png)

#### <a name="solutions"></a>解决方案

**UI 重新排列<a name="ui-rearrange"></a>**

除非你的初始焦点放置在页面底部，否则放置在长滚动列表上方的 UI 元素通常比放置在下方更易于访问。
如果此新布局适用于其他设备，则针对所有设备系列更改布局而不仅仅针对 Xbox One 进行特殊 UI 更改，此方法的成本可能更低。
此外，针对滚动方向放置 UI元素（即对垂直滚动列表水平放置，或对水平滚动列表垂直放置）将更便于访问。

![房地产应用：将按钮放置在长滚动列表上方](images/designing-for-tv/2d-focus-navigation-and-interaction-ui-rearrange.png)

**专注参与<a name="engagement"></a>**

当要求*占用*时，整个 `ListView` 将变为一个焦点目标。 用户将能够绕过列表的内容到达下一个可聚焦元素。 在[焦点占用](#focus-engagement)中阅读有关哪些控件支持占用以及如何使用它们的详细信息。

![房地产应用：设置要求的占用，以便只需 1 次单击即可到达“上一步”/“下一步”按钮](images/designing-for-tv/2d-focus-navigation-and-interaction-engagement.png)

#### <a name="problem-scrollviewer-without-any-focusable-elements"></a>问题：不带有任何可聚焦元素的 ScrollViewer

由于 XY 焦点导航依赖一次导航到一个可聚焦 UI 元素，因此不包含任何可聚焦元素的 [ScrollViewer](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ScrollViewer)（例如本示例中的仅带有文本的一个元素）可能导致用户无法查看 `ScrollViewer` 中的所有内容的方案。
有关此问题的解决方案和其他相关方案，请参阅[焦点占用](#focus-engagement)。

![房地产应用：仅带有文本的 ScrollViewer](images/designing-for-tv/2d-focus-navigation-and-interaction-scrollviewer.png)

#### <a name="problem-free-scrolling-ui"></a>问题：自由滚动 UI

当你的应用需要自由滚动的 UI（例如绘图图面，或本示例中的地图）时，XY 焦点导航不起作用。
在这种情况下，你可以打开[鼠标模式](#mouse-mode)以允许用户在 UI 元素内部自由导航。

![使用鼠标模式的 UI 元素](images/designing-for-tv/map-mouse-mode.png)

## <a name="mouse-mode"></a>鼠标模式

如 [XY 焦点导航和交互](#xy-focus-navigation-and-interaction)中所述，在 Xbox One 上，焦点通过使用 XY 导航系统进行移动，从而允许用户通过向上、向下、向左和向右移动在控件之间转移焦点。
但是，某些控件（如 [WebView](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.WebView) 和 [MapControl](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Maps.MapControl)）需要类似于鼠标的交互，在该交互中，用户可以在控件的边界内自由移动指针。
另外，在某些应用中，用户能够将指针在整个页面上移动，并且此操作有意义，该游戏板/遥控器体验类似于用户可在带有鼠标的电脑上找到的体验。

对于这些方案，你应为整个页面或者在页面内部的控件上请求指针（鼠标模式）。
例如，你的应用可能具有一个包含 `WebView` 控件的页面，它仅在该控件内部使用鼠标模式，而在任何其他位置使用 XY 焦点导航。
若要请求一个指针，你可以指定**在控件或页面已占用时**还是**在页面具有焦点时**需要它。

> [!NOTE]
> 不支持在控件得到焦点时请求指针。

对于 Xbox One 上运行的 XAML 应用和托管的 Web 应用，为整个应用默认打开鼠标模式。 强烈建议你关闭此模式，并针对 XY 导航优化你的应用。 为此，请将 `Application.RequiresPointerMode` 属性设置为 `WhenRequested`，以便仅在控件或页面对它进行调用时，才启用鼠标模式。

若要在 XAML 应用中达成此目的，请在 `App` 类中使用以下代码：

```csharp
public App()
{
    this.InitializeComponent();
    this.RequiresPointerMode =
        Windows.UI.Xaml.ApplicationRequiresPointerMode.WhenRequested;
    this.Suspending += OnSuspending;
}
```

有关详细信息，包括 HTML/JavaScript 示例代码，请参阅[如何禁用鼠标模式](../../xbox-apps/how-to-disable-mouse-mode.md)。

下图显示游戏板/遥控器在鼠标模式下的按钮映射。

![游戏板/遥控器在鼠标模式下的按钮映射](images/designing-for-tv/10ft_infographics_mouse-mode.png)

> [!NOTE]
> 鼠标模式仅在带有游戏板/遥控器的 Xbox One 上受支持。 在其他设备系列和输入类型上以静默方式忽略它。

在控件或页面上使用 [RequiresPointer](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.requirespointer) 属性在其上激活鼠标模式。 此属性具有三个可能的值：`Never`（默认值）、`WhenEngaged` 和 `WhenFocused`。

### <a name="activating-mouse-mode-on-a-control"></a>在控件上激活鼠标模式

当用户使用 `RequiresPointer="WhenEngaged"` 占用控件时，将在该控件上激活鼠标模式，直到用户脱离它。 以下代码片段演示了在已占用时激活鼠标模式的简单 `MapControl`：

```xml
<Page>
    <Grid>
        <MapControl IsEngagementRequired="true"
                    RequiresPointer="WhenEngaged"/>
    </Grid>
</Page>
```

> [!NOTE]
> 如果控件在已占用时激活鼠标模式，则它还必须通过 `IsEngagementRequired="true"` 要求占用；否则，将永远不激活鼠标模式。

当控件处于鼠标模式时，其嵌套控件也将处于鼠标模式。 将忽略它的子元素的请求模式&mdash;当父元素处于鼠标模式下时，子元素也必须处于鼠标模式下。

此外，仅在控件得到焦点时检查控件的请求模式，因此模式不会在其具有焦点时动态更改。

### <a name="activating-mouse-mode-on-a-page"></a>在页面上激活鼠标模式

当页面具有属性 `RequiresPointer="WhenFocused"` 时，将在该页面得到焦点时为整个页面激活鼠标模式。 以下代码片段演示了如何为页面提供此属性：

```xml
<Page RequiresPointer="WhenFocused">
    ...
</Page>
```

> [!NOTE]
> `WhenFocused` 值仅在 [Page](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Page) 对象上受支持。 如果你尝试在控件上设置此值，将引发异常。

### <a name="disabling-mouse-mode-for-full-screen-content"></a>针对全屏内容禁用鼠标模式

通常，在全屏显示视频或其他类型的内容时，你将需要隐藏光标，因为它可能会分散用户的注意力。 如果应用的其余部分使用鼠标模式，但你想要在显示全屏内容时将其关闭，将出现此情形。 若要实现此目的，请将全屏内容放置在其自己的 `Page` 上，并按照下面的步骤操作。

1. 在 `App` 对象中，设置 `RequiresPointerMode="WhenRequested"`。
2. 在每个 `Page` 对象中（全屏 `Page`*除外*），设置 `RequiresPointer="WhenFocused"`。
3. 对于全屏 `Page`，设置 `RequiresPointer="Never"`。

这样一来，光标将永远不会在显示全屏内容时出现。

## <a name="focus-visual"></a>焦点视觉对象

焦点视觉对象是当前具有焦点的 UI 元素周围的边框。 这有助于使用户定位，以便他们可以轻松导航你的 UI而不会迷失。

借助添加到焦点视觉对象的视觉更新和大量自定义选项，开发人员可以相信，单个焦点视觉对象将适用于电脑和 Xbox One 以及支持键盘和/或游戏板/遥控器的任何其他 Windows 10 设备。

尽管可以在不同的平台上使用相同的焦点视觉对象，但对于 10 英尺体验，用户遇到此情况的上下文稍有不同。 你应假设用户不会对整个电视屏幕投入完全的注意力，因此当前具有焦点的元素始终对用户清晰可见以避免其在搜寻视觉对象时感到沮丧，这一点很重要。

请记住在使用游戏板或遥控器（而*不是*键盘）时，默认情况下将显示焦点视觉对象，这一点也很重要。 因此，即使你不实现它，当你在 Xbox One 上运行你的应用时，它仍会出现。

### <a name="initial-focus-visual-placement"></a>初始焦点视觉对象放置

在启动应用或导航到某个页面时，将焦点放置在作为用户会执行操作的第一个元素很合理的 UI 元素上。 例如，照片应用可能会将焦点放置在库中的第一个项上，而导航到歌曲详细视图的音乐应用可能将焦点放置在播放按钮上以便于播放音乐。

尝试将初始焦点放置在应用的左上区域（对于从右向左流，则为右上区域）。 大多数用户倾向于先将焦点放置在该角度，因为应用内容流通常在此位置开始。

### <a name="making-focus-clearly-visible"></a>使焦点清晰可见

一个焦点视觉对象应始终在屏幕上可见，以便用户可以从离开的位置继续，而无需搜索焦点。 同样，屏幕上应始终存在可聚焦的项&mdash;例如，不要使用仅带有文本而没有可聚焦元素的弹出窗口。

全屏体验（如观看视频或查看图像）是此规则的一个例外，在全屏情况下不适宜显示焦点视觉。

### <a name="reveal-focus"></a>显示焦点

显示焦点是一种灯光效果，当用户将游戏板或键盘焦点移向可聚焦元素时，这种灯光效果将这些元素的边框进行动画处理，如按钮。 通过在可聚焦元素边框周围的明亮辉光创建动画，显示焦点可帮助用户更好地了解焦点的位置以及焦点将前往的位置。

默认情况下，“显示焦点”处于关闭状态。 为了实现 10 英尺体验，你应选择通过在应用构造函数中设置 [Application.FocusVisualKind property](https://docs.microsoft.com/uwp/api/windows.ui.xaml.application.FocusVisualKind) 来显示焦点。

```csharp
    if(AnalyticsInfo.VersionInfo.DeviceFamily == "Windows.Xbox")
    {
        this.FocusVisualKind = FocusVisualKind.Reveal;
    }
```

有关更多信息，请参阅[显示焦点](/windows/uwp/design/style/reveal-focus)指南。

### <a name="customizing-the-focus-visual"></a>自定义焦点视觉

如果你想要自定义焦点视觉，可以通过以下方式执行此操作：为每一个控件修改与焦点视觉有关的属性。 可以充分利用多个此类属性个性化你的应用。

你甚至可以通过使用视觉状态绘制你自己的焦点视觉，来选择退出系统提供的焦点视觉。 若要了解详细信息，请参阅 [VisualState](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.VisualState)。

### <a name="light-dismiss-overlay"></a>轻型消除覆盖层

若要将用户注意到用户当前正在使用游戏控制器或远程控制操作的 UI 元素，则 UWP 会自动添加一个 "冒烟" 层，该图层涵盖了当应用在 Xbox One 上运行时，弹出式 UI 外的区域。 这无需任何额外工作，但却是在设计 UI 时需要牢记的内容。 可以设置任何 `FlyoutBase` 上的 `LightDismissOverlayMode` 属性以启用或禁用烟层；它默认为 `Auto`，这意味着它将在 Xbox 上处于启用状态，在别处处于禁用状态。 有关详细信息，请参阅[模式和轻型消除](../controls-and-patterns/menus.md)。

## <a name="focus-engagement"></a>焦点占用

焦点占用旨在更轻松地使用游戏板或遥控器与应用交互。

> [!NOTE]
> 设置焦点占用不会影响键盘或其他输入设备。

当 [FrameworkElement](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.FrameworkElement) 对象上的属性 `IsFocusEngagementEnabled` 设置为 `True` 时，它会将控件标记为需要焦点占用。 这意味着用户必须按**A/Select**按钮才能 "参与" 控件并与其进行交互。 完成后，他们可以按**B/后退**按钮来脱开控件并从中导航。

> [!NOTE]
> `IsFocusEngagementEnabled`是新的 API，但尚未记录。

### <a name="focus-trapping"></a>焦点捕获

焦点捕获是指用户尝试导航应用 UI 但被“捕获”在控件内，从而难以甚至无法移出该控件时发生的情况。

以下示例显示产生焦点捕获的 UI。

![水平滑块左侧和右侧的按钮](images/designing-for-tv/focus-engagement-focus-trapping.png)

如果用户要从左按钮导航到右按钮，则符合逻辑的假设是他们只需按方向键/左摇杆上的向右键两次。
但是，如果[滑块](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Slider)不需要占用，则将发生以下行为：当用户第一次按向右键时，焦点将转移到 `Slider`，并且当用户再次按向右键时，`Slider` 的手柄将移到右侧。 用户不断地将手柄移到右侧，从而无法到达按钮。

有多种方法解决此问题。 一个方法是设计一个不同的布局，类似于 [XY 焦点导航和交互](#xy-focus-navigation-and-interaction)中的房地产应用示例，在该示例中我们将 **“上一步”** 和 **“下一步”** 按钮重新放置在 `ListView` 上方。 如下图所示垂直堆叠而不是水平堆叠这些控件将解决该问题。

![水平滑块上方和下方的按钮](images/designing-for-tv/focus-engagement-focus-trapping-2.png)

现在用户可以通过按方向键/左摇杆上的向上和向下键来导航到每个控件，并且当 `Slider` 具有焦点时，他们可以按预期通过按向左和向右键来移动 `Slider` 手柄。

解决此问题的另一个方法是在 `Slider` 上要求占用。 如果你设置 `IsFocusEngagementEnabled="True"`，这将导致以下行为。

![在滑块上要求焦点占用，以便用户可以导航到右侧的按钮。](images/designing-for-tv/focus-engagement-slider.png)

当 `Slider` 要求焦点占用时，用户只需按方向键/左摇杆上的向右键两次即可到达右侧的按钮。 此解决方案很出色，因为它无需进行 UI 调整即可产生预期的行为。

### <a name="items-controls"></a>项控件

除了 [Slider](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Slider) 控件，还有其他可能要求占用的控件，例如：

- [ListBox](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ListBox)
- [ListView](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ListView)
- [GridView](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.GridView)
- [FlipView](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.FlipView)

与 `Slider` 控件不同，这些控件不会将焦点捕获在自身内部；但是，当它们包含大量数据，它们可能会导致可用性问题。 以下是一个包含大量数据的 `ListView` 的示例。

![带有大量数据的 ListView 以及上方和下方的按钮](images/designing-for-tv/focus-engagement-list-and-grid-controls.png)

与 `Slider` 示例类似，让我们尝试使用游戏板/遥控器从顶部的按钮导航到底部的按钮。
从顶部按钮的焦点开始，按方向键/摇杆上的向下键会将焦点放置在 `ListView` 中的第一个项上（“项 1”）。
当用户再次按向下键时，列表中的下一个项将得到焦点，而不是底部的按钮。
若要到达该按钮，用户必须先导航 `ListView` 中的每一项。
如果 `ListView` 包含大量数据，这可能造成不便且非最佳的用户体验。

若要解决此问题，请在 `ListView` 上设置属性 `IsFocusEngagementEnabled="True"` 以在该控件上要求占用。
这将允许用户只需按向下键即可跳过 `ListView`。 但是，他们将无法在列表中滚动或从列表中选择项目，除非他们通过在具有焦点的情况下按**A/Select**按钮，然后按**B/后退**按钮脱开。

![要求占用的 ListView](images/designing-for-tv/focus-engagement-list-and-grid-controls-2.png)

#### <a name="scrollviewer"></a>ScrollViewer

与这些控件稍有不同的是 [ScrollViewer](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ScrollViewer)，该控件具有其自己的特点要考虑。 如果你有包含可聚焦内容的 `ScrollViewer`，默认情况下导航到 `ScrollViewer` 将允许你移动其可聚焦元素。 和在 `ListView` 中相同，你必须滚动浏览每一项才能导航到 `ScrollViewer` 外部。

如果不`ScrollViewer`具有*no*可设定焦点&mdash;的内容（例如，如果它仅&mdash;包含可以设置`IsFocusEngagementEnabled="True"`的文本），则用户`ScrollViewer`可以通过使用 " **/选择**" 按钮来参与。 使用后，他们可以使用**D-pad/左摇杆**滚动文本，然后按**B/后退**按钮，在完成后松开。

另一个方法是在 `ScrollViewer` 上设置 `IsTabStop="True"`，以便当 `ScrollViewer` 内不存在可聚焦元素时用户无需占用控件&mdash;他们只需将焦点放置在该控件上，然后使用**方向键/左摇杆**进行滚动。

### <a name="focus-engagement-defaults"></a>焦点占用默认值

某些控件导致焦点捕获的频率足以保证其默认设置要求焦点占用，而其他控件默认关闭焦点占用，但可能因打开它而受益。 下表列出了这些控件及其默认焦点占用行为。

| 控制               | 焦点占用默认值  |
|-----------------------|---------------------------|
| CalendarDatePicker    | 启用                        |
| FlipView              | 关闭                       |
| GridView              | 关闭                       |
| ListBox               | 关闭                       |
| ListView              | 关闭                       |
| ScrollViewer          | 关闭                       |
| SemanticZoom          | 关闭                       |
| 滑块                | 启用                        |

所有其他 Windows 控件在时`IsFocusEngagementEnabled="True"`不会导致行为或视觉对象更改。

## <a name="summary"></a>总结

你可以构建针对特定设备或体验进行了优化的 Windows 应用程序，但通用 Windows 平台还允许你构建可跨设备成功使用的应用（在2英尺和10英尺体验中，无论输入设备或用户的功能如何）。 使用本文中的建议可以确保你的应用程序可以在电视和 PC 上正常工作。

## <a name="related-articles"></a>相关文章

- [针对 Xbox 和电视进行设计](../devices/designing-for-tv.md)
- [Windows 应用应用的设备入门](index.md)
