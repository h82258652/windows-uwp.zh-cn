---
author: eliotcowley
Description: "设计你的应用，以使其在电视上外观良好且运行正常。"
title: "针对 Xbox 和电视进行设计"
ms.assetid: 780209cb-3e8a-4cf7-8f80-8b8f449580bf
label: Designing for Xbox and TV
template: detail.hbs
isNew: true
translationtype: Human Translation
ms.sourcegitcommit: 96a35ded526b09dd1ce1cb8528bb4a99e3511b32
ms.openlocfilehash: 734a0f0574ac7698dd6bd963bf3e20225b26d401

---

# 针对 Xbox 和电视进行设计

设计你的通用 Windows 平台 (UWP) 应用，以便它在 Xbox One 和电视屏幕上外观良好且运行正常。

## 概述

通用 Windows 平台可使你在多台 Windows 10 设备上创建令人愉快的体验。 UWP 框架提供的大部分功能使应用能够在这些设备上使用相同的用户界面 (UI)，而无需附加工作。 但是，定制和优化你的应用以使其在 Xbox One 和电视屏幕上良好工作需要考虑特殊的注意事项。

坐在房间中的沙发上使用游戏板或遥控器与电视交互的体验称为 **10 英尺体验**。 如此命名是因为用户通常坐在离屏幕大约 10 英尺的位置。 这提出了 *2 英尺*体验（假设）中或与电脑交互时所不存在的独特挑战。 如果你要为 Xbox One 或任何输出到电视屏幕和使用控制器进行输入的其他设备开发应用，应始终牢记这一点。

若要使你的应用非常适合 10 英尺体验，并不需要执行本文中的所有步骤，但了解这些步骤并为应用进行相应的决策可使 10 英尺体验更符合应用的特定需求。 在将应用实际应用到 10 英尺环境中时，请考虑以下设计原则。

### 简单

针对 10 英尺环境进行设计提出了一组独特的挑战。 分辨率和观看距离可能使人难以处理太多信息。 尝试使设计保持干净，尽可能减少到最简单的组件。 电视上所显示的信息量应与你在移动电话上（而不是桌面上）看到的内容相当。

![Xbox One 主屏幕](images/designing-for-tv/xbox-home-screen.png)

### 一致

10 英尺环境中的 UWP 应用应当直观且易于使用。 是重点清晰明显。 安排内容，以使跨空间移动一致且可预测。 为用户提供指向所需操作的最短路径。

![Xbox One 电影应用](images/designing-for-tv/xbox-movies-app.png)

_**Microsoft 电影和电视中提供了屏幕截图中所示的所有电影。**_  

### 引人入胜

在大屏幕上可享受最沉浸式的电影体验。 边对边场景、优美的动作以及颜色和版式的灵活使用可将你的应用提升到下一层次。 兼具大胆和美观。

![Xbox One 虚拟形象应用](images/designing-for-tv/xbox-avatar-app.png)

### 针对 10 英尺体验的优化

现在你已了解了适用于 10 英尺体验的良好 UWP 应用设计的原则，请阅读以下关于优化应用和实现出色用户体验的特定方法的概述。

| 功能        | 描述           |
| -------------------------------------------------------------- |--------------------------------|
| [游戏板和遥控器](#gamepad-and-remote-control)      | 确保你的应用适用于游戏板和遥控器是针对 10 英尺体验进行优化中最重要的步骤。 可以进行多个特定于游戏板和遥控器的改进，以优化用户操作在某种程度上受限的设备上的用户交互体验。 |
| [XY 焦点导航和交互](#xy-focus-navigation-and-interaction) | UWP 提供 **XY 焦点导航**，该导航允许用户在应用的 UI 中四处导航。 但是，这会限制用户只能向上、向下、向左和向右导航。 本部分概述了处理此情况的建议和其他注意事项。 |
| [鼠标模式](#mouse-mode)|在某些用户界面（如地图和绘图图面）中，使用 XY 焦点导航不可行或不现实。 对于这些界面，UWP 提供了**鼠标模式**来使游戏板/遥控器自由导航，就像桌面计算机上的鼠标一样。|
| [焦点视觉对象](#focus-visual)  | 焦点视觉对象是当前具有焦点的 UI 元素周围的边框。 这有助于使用户定位，以便他们可以轻松导航你的 UI 而不会迷失。 如果焦点不清晰可见，用户可能在 UI 中迷失，并且无法得到出色的体验。  |
| [焦点占用](#focus-engagement) | 在 UI 元素上设置焦点占用需要用户按“A/选择”****按钮以便与其交互。 这有助于为用户在导航应用 UI 时创建更好的体验。
| [UI 元素大小调整](#ui-element-sizing)  | 通用 Windows 平台使用[缩放和有效像素](..\layout\design-and-ui-intro.md#effective-pixels-and-scaling)来根据观看距离缩放 UI。 了解大小调整并在 UI 上应用它有助于针对 10 英尺环境优化你的应用。  |
|  [电视安全区域](#tv-safe-area) | 默认情况下，UWP 将自动避免在电视不安全区域（接近屏幕边缘的区域）显示任何 UI。 但是，这将导致“箱中”效果，即 UI 看起来以宽屏显示。 为了使你的应用在电视上提供真正的沉浸式体验，你将要对其进行修改，以使其延伸到电视上的屏幕边缘（如果电视支持此功能）。 |
| [颜色](#colors)  |  UWP 支持颜色主题，并且遵循系统主题的应用在 Xbox One 上将默认为**深色**。 如果你的应用具有特定的颜色主题，你应考虑到某些颜色不适用于电视，应避免使用它们。 |
| [声音](../style/sound.md)    | 声音在 10 英尺体验中起到关键作用，它有助于使用户沉浸和向用户提供反馈。 当应用在 Xbox One 上运行时，UWP 提供为常用控件自动启用声音的功能。 了解有关内置于 UWP 的声音支持的详细信息，并了解如何充分利用它。    |
| [UI 控件指南](#guidelines-for-ui-controls)  |  有多个 UI 控件可跨多台设备运行良好，但在电视上使用时有些特定的注意事项。 请阅读在针对 3 米体验进行设计时使用这些控件的一些最佳做法。 |
| [Xbox 的自定义视觉状态触发器](#custom-visual-state-trigger-for-xbox) | 若要针对 3 米体验定制 UWP 应用，我们建议你使用自定义*视觉状态触发器*，以在应用检测到它已在 Xbox 主机上启动时更改布局。

## 游戏板和遥控器

就像键盘和鼠标适用于电脑、而触摸适用于手机和平板电脑一样，游戏板和遥控器是 10 英尺体验的主要输入设备。 本部分介绍了硬件按钮的定义和作用。 在 [XY 焦点导航和交互](#xy-focus-navigation-and-interaction)和[鼠标模式](#mouse-mode)中，你将了解如何在使用这些输入设备时优化你的应用。

全新的游戏板和遥控器行为的质量取决于键盘在应用中受到多大的支持。 确保你的应用适用于游戏板/遥控器的一个好方法是，确保它在电脑上适用于键盘，然后使用游戏板/遥控器进行测试以查找 UI 中的缺陷。

### 硬件按钮

在整个文档中，将使用下图中给定的名称指代按钮。

![游戏板和遥控器按钮图](images/designing-for-tv/hardware-buttons-gamepad-remote.png)

如该图中所示，某些按钮在游戏板上受支持，但在遥控器上不受支持，反之亦然。 尽管你可以使用仅在一台输入设备上受支持的按钮来加快导航 UI 的速度，但请注意将这些按钮用于关键交互可能会产生用户无法与特定 UI 部分交互的情况。

下表列出了所有受 UWP 应用支持的硬件按钮，以及哪些输入设备支持它们。

| 按钮                    | 游戏板   | 遥控器    |
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
| 静音按钮               | 否        | 是               |

### 内置按钮支持

UWP 将现有键盘输入行为自动映射到游戏板和遥控器输入。 下表列出了这些内置映射。

| 键盘              | 游戏板/遥控器                        |
|-----------------------|---------------------------------------|
| 箭头键            | 方向键（以及游戏板上的左摇杆）    |
| 空格键              | A/“选择”按钮                       |
| Enter                 | A/“选择”按钮                       |
| Escape                | B/“后退”按钮*                        |

\*当应用既不处理 B 按钮的 [KeyDown](https://msdn.microsoft.com/library/windows/apps/br208941) 事件也不处理 [KeyUp](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.uielement.keyup.aspx) 事件时，将引发 [SystemNavigationManager.BackRequested](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.core.systemnavigationmanager.backrequested.aspx) 事件，从而会导致应用内的向后导航。 但是，你必须自行实现此操作，如以下代码片段中所示：

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

Xbox One 上的 UWP 应用还支持按“菜单”****按钮来打开上下文菜单。 有关详细信息，请参阅 [CommandBar 和 ContextFlyout](#commandbar-and-contextflyout)。

### 加速器支持

加速器按钮是可用于加快在 UI 中导航的按钮。 但是，这些按钮可能是特定输入设备所独有的，因此请记住并非所有用户都能够使用这些功能。 事实上，游戏板当前是在 Xbox One 上针对 UWP 应用支持加速器功能的唯一输入设备。

下表列出了内置于 UWP 以及可自行实现的加速器支持。 在自定义 UI 中利用这些行为提供一致且友好的用户体验。

| 交互   | 键盘   | 游戏板      | 内置用于：  | 建议用于： |
|---------------|------------|--------------|----------------|------------------|
| 向上/向下翻页  | 向上/向下翻页 | 左/右扳机键 | [CalendarView](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.calendarview.aspx)、[ListBox](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.listbox.aspx)、[ListViewBase](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.listviewbase.aspx)、[ListView](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.listview.aspx)、`ScrollViewer`、[Selector](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.primitives.selector.aspx)、[LoopingSelector](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.primitives.loopingselector.aspx)、[ComboBox](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.combobox.aspx)、[FlipView](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.flipview.aspx) | 支持垂直滚动的视图
| 向左/向右翻页 | 无 | 左/右缓冲键 | [Pivot](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.pivot.aspx)、[ListBox](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.listbox.aspx)、[ListViewBase](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.listviewbase.aspx)、[ListView](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.listview.aspx)、`ScrollViewer`、[Selector](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.primitives.selector.aspx)、[LoopingSelector](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.primitives.loopingselector.aspx)、[FlipView](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.flipview.aspx) | 支持水平滚动的视图
| 放大/缩小        | Ctrl +/- | 左/右扳机键 | 无 | `ScrollViewer`支持放大和缩小的视图 |
| 打开/关闭导航窗格 | 无 | 查看 | 无 | 导航窗格​​

## XY 焦点导航和交互

如果应用支持键盘的适当焦点导航，这将很好地转换到游戏板和遥控器。 使用箭头键的导航映射到**方向键**（以及游戏板上的**左摇杆**），而与 UI 元素的交互映射到 **输入/选择**键（请参阅[游戏板和远程控制](#gamepad-and-remote-control)）。 

键盘和游戏板会使用许多事件和属性&mdash;它们会触发 `KeyDown` 和 `KeyUp` 事件，并且两者仅导航到具有 `IsTabStop="True"` 和 `Visibility="Visible"` 属性的控件。 有关键盘设计指南，请参阅[键盘交互](keyboard-interactions.md)。

如果已正确实现键盘支持，你的应用理应良好地工作；但是，若要支持每一种方案，可能需要一些额外的工作。 思考你的应用的具体需求以尽可能提供最佳的用户体验。

> [!IMPORTANT]
> 对于 Xbox One 上运行的 UWP 应用，默认会启用鼠标模式。 若要禁用鼠标模式并启用 XY 焦点导航，请设置 `Application.RequiresPointerMode=WhenRequested`。

### 调试焦点问题

[FocusManager.GetFocusedElement](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.input.focusmanager.getfocusedelement.aspx) 方法会告知你当前具有焦点的元素。 这对于焦点视觉位置可能不明显的情形非常有用。 你可以将此信息记录到 Visual Studio 输出窗口，如下所示：

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

* 错误设置 [IsTabStop](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.control.istabstop.aspx) 或 [Visibility](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.uielement.visibility.aspx) 属性。
* 获取焦点的控件实际大于你认为&mdash; XY 导航看到的该控件的总大小（[ActualWidth](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.frameworkelement.actualwidth.aspx) 和 [ActualHeight](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.frameworkelement.actualheight.aspx)），不仅限于呈现关注内容的控件部分。
* 一个可获得焦点的控件基于其他控件之上&mdash;XY 导航不支持层叠显示的控件。

如果在修复这些问题后，XY 导航仍未按预期的方式工作，你可以使用[替代默认导航](#overriding-the-default-navigation)中所述的方法手动指向你希望获取焦点的元素。

如果 XY 导航按预期方式工作，但不显示任何焦点视觉，以下问题之一可能是原因所在：

* 你对控件重新模板化，但并未包含焦点视觉。 设置 `UseSystemFocusVisuals="True"` 或手动添加焦点视觉。
* 通过调用 `Focus(FocusState.Pointer)` 移动焦点。 [FocusState](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.focusstate.aspx) 参数控制焦点视觉的变化。 通常应该将该参数设置为 `FocusState.Programmatic`，以使焦点视觉保持可见（如果之前为可见）或隐藏（如果之前为隐藏）。

本部分的其余内容会详细介绍使用 XY 导航时遇到的常规设计挑战，并提供解决这些挑战的多种方法。

### 无法访问的 UI

由于 XY 焦点导航限制用户向上、向下、向左和向右移动，因此你最终可能得到部分 UI 无法访问的方案。 下图演示了 XY 焦点导航不支持的 UI 布局类型的示例。 请注意，中间的元素无法通过使用游戏板/遥控器来访问，因为垂直和水平导航将设置优先级，而中间元素永远不会是足以得到焦点的高优先级。

![四个角的元素，中间的元素无法访问](images/designing-for-tv/2d-navigation-best-practices-ui-layout-to-avoid.png)

如果由于某些原因无法重新排列 UI，请使用下一部分中所述的技术之一来重写默认焦点行为。

### 替代默认导航

尽管通用 Windows 平台尝试确保方向键/左摇杆导航对用户有意义，但无法保证行为已针对应用的意图进行优化。 确保导航已针对应用进行优化的最佳方式是使用游戏板测试它，然后确认用户可以通过对应用方案有意义的方式访问每个 UI 元素。 如果你的应用方案需要并非通过所提供的 XY 焦点导航实现的行为，请考虑以下部分中的以下建议和/或替代该行为以将焦点放置在逻辑项上。

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

### 最少单击路径

尝试允许用户以最少的单击数执行最常见的任务。 在以下示例中，[TextBlock](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.textblock.aspx) 放置在“播放”****按钮（最先得到焦点）和常用元素之间，因此不必要的元素放置在优先任务之间。

![导航最佳做法可提供最少单击路径。](images/designing-for-tv/2d-navigation-best-practices-provide-path-with-least-clicks.png)

在以下示例中，[TextBlock](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.textblock.aspx) 改为放置在“播放”****按钮上方。 只需重新排列 UI 以使优先任务之间不放置不必要的元素即可大幅提高应用的可用性。

![TextBlock 已移到“播放”按钮上方，以使其不再位于优先任务之间。](images/designing-for-tv/2d-navigation-best-practices-provide-path-with-least-clicks-2.png)

### CommandBar 和 ContextFlyout

在使用 [CommandBar](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.commandbar.aspx) 时，请记住[问题：位于长滚动列表/网格之后的 UI 元素](#problem-ui-elements-located-after-long-scrolling-list-grid)中所提到的滚动浏览列表的问题。 下图显示了列表/网格底部带有 `CommandBar` 的 UI 布局。 用户需要向下一直滚动浏览列表/网格才能到达 `CommandBar`。

![位于列表/网格底部的 CommandBar](images/designing-for-tv/2d-navigation-best-practices-commandbar-and-contextflyout.png)

如果你将 `CommandBar` 放置在列表/网格*上方*会怎么样？ 尽管向下滚动列表/网格的用户必须重新向上滚动才能到达 `CommandBar`，但与之前的配置相比，导航数略少。 请注意，这假设你的应用的初始焦点放置在 `CommandBar` 旁边或上方；如果初始焦点在列表/网格下方，则此方法不起作用。 如果这些 `CommandBar` 项是无需经常访问的全局操作项（如**“同步”**按钮），则可以将它们放置在列表/网格上方。

尽管你无法垂直堆叠 `CommandBar` 的项，但相对于滚动方向放置它们（例如，垂直滚动列表的左侧或右侧，或者水平滚动列表的顶部或底部）是另一个需要考虑的选项（如果适用于你的 UI 布局）。

如果应用的 `CommandBar` 具有需要便于用户访问的项，你可能要考虑将这些项放置在 [ContextFlyout](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.uielement.contextflyout.aspx) 内部，并将它们从 `CommandBar` 中删除。 `ContextFlyout` 是 [UIElement](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.uielement.aspx) 的属性，并且是与该元素关联的[上下文菜单](../controls-and-patterns/dialogs-popups-menus.md#context-menus-and-flyouts)。 在电脑上，右键单击带有 `ContextFlyout` 的元素时，会弹出上下文菜单。 在 Xbox One 上，如果焦点在此类元素上，按“菜单”****按钮也会发生此行为。

<!--The following XAML code demonstrates a simple `ContextFlyout`:

```xml
<Button HorizontalAlignment="Center"
        Content="Context Flyout">
    <Button.ContextFlyout>
        <MenuFlyout>
            <MenuFlyoutItem Text="Item 1"/>
        </MenuFlyout>
    </Button.ContextFlyout>
</Button>
```

In the above example, when you press the **Menu** button while the [Button](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.button.aspx) has focus, the context menu appears with the menu item labeled **Item 1**.

`ContextFlyout` takes any element of type [FlyoutBase](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.primitives.flyoutbase.aspx); however, most of the time you will likely use [Flyout](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.flyout.aspx) or [MenuFlyout](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.menuflyout.aspx).

Alternatively, you can listen for the [ContextRequested](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.uielement.contextrequested.aspx) event, which occurs when the user has completed a context input gesture (pressing the **Menu** button). In this case you can, in the code-behind, create the context menu, attach it to the **UIElement**, and show the flyout when the event is raised.

The following C# code demonstrates a simple example of this:

```csharp
MenuFlyout myFlyout = new MenuFlyout();
MenuFlyoutItem item1 = new MenuFlyoutItem();
item1.Text = "Item 1";
myFlyout.Items.Add(item1);
MyButton.ContextFlyout = myFlyout;

private void MyButton_ContextRequested(UIElement sender, ContextRequestedEventArgs args)
{
    Point point = new Point(0, 0);
    if (args.TryGetPosition(sender, out point)
    {
        myFlyout.ShowAt(sender, point);
    }
    else
    {
        myFlyout.ShowAt(sender as FrameworkElement);
    }
}
```
> **Note** Don't use both of these options, as `ContextFlyout` already handles the `ContextRequested` event.-->

### UI 布局挑战

由于 XY 焦点导航的性质，某些 UI 布局更具挑战性，应根据具体情况进行评估。 尽管不存在单一的“正确”方法，并且选择哪个解决方案取决于应用的具体需求，但你可以利用某些技术来创造出色的电视体验。

为了更好地了解这一点，让我们查看一个虚构的应用，该应用演示了其中的一些问题以及克服它们的技术。

> [!NOTE]
> 此虚构应用旨在演示 UI 问题及其潜在解决方案，而非演示你的特定应用的最佳用户体验。

以下是一个虚构的房地产应用，该应用显示可供销售的房屋列表、地图、房产说明以及其他信息。 此应用提出了三项可通过使用以下技术克服的挑战：

- [UI 重新排列](#ui-rearrange)
- [焦点占用](#engagement)
- [鼠标模式](#mouse-mode)

![虚构房地产应用](images/designing-for-tv/2d-focus-navigation-and-interaction-real-estate-app.png)

#### 问题：位于长滚动列表/网格之后的 UI 元素 <a name="problem-ui-elements-located-after-long-scrolling-list-grid"></a>

下图所示的房产 [ListView](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.listview.aspx) 是一个非常长的滚动列表。 如果 `ListView` 上*不*要求[占用](#focus-engagement)，当用户导航到该列表时，焦点将放置在列表中的第一个项上。 若要使用户到达“上一步”****或“下一步”****按钮，他们必须浏览列表中的所有项。 在这种难以要求用户遍历整个列表&mdash;即，当列表太长，无法接受此体验时&mdash;的情况下，你可能希望考虑其他选项。

![房地产应用：带有 50 个项的列表，需要单击 51 次才能到达下方的按钮](images/designing-for-tv/2d-focus-navigation-and-interaction-real-estate-app-list.png)

#### 解决方案

**UI 重新排列 <a name="ui-rearrange"></a>**

除非你的初始焦点放置在页面底部，否则放置在长滚动列表上方的 UI 元素通常比放置在下方更易于访问。 如果此新布局适用于其他设备，则针对所有设备系列更改布局而不仅仅针对 Xbox One 进行特殊 UI 更改，此方法的成本可能更低。 此外，针对滚动方向放置 UI元素（即对垂直滚动列表水平放置，或对水平滚动列表垂直放置）将更便于访问。

![房地产应用：将按钮放置在长滚动列表上方](images/designing-for-tv/2d-focus-navigation-and-interaction-ui-rearrange.png)

**焦点占用 <a name="engagement"></a>**

当要求*占用*时，整个 `ListView` 将变为一个焦点目标。 用户将能够绕过列表的内容到达下一个可聚焦元素。 在[焦点占用](#focus-engagement)中阅读有关哪些控件支持占用以及如何使用它们的详细信息。

![房地产应用：设置要求的占用，以便只需 1 次单击即可到达“上一步”/“下一步”按钮](images/designing-for-tv/2d-focus-navigation-and-interaction-engagement.png)

#### 问题：不带有任何可聚焦元素的 ScrollViewer

由于 XY 焦点导航依赖一次导航到一个可聚焦 UI 元素，因此不包含任何可聚焦元素的 [ScrollViewer](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.scrollviewer.aspx)（例如本示例中的仅带有文本的一个元素）可能导致用户无法查看 `ScrollViewer` 中的所有内容的方案。 有关此问题的解决方案和其他相关方案，请参阅[焦点占用](#focus-engagement)。

![房地产应用：仅带有文本的 ScrollViewer](images/designing-for-tv/2d-focus-navigation-and-interaction-scrollviewer.png)

#### 问题：自由滚动 UI

当你的应用需要自由滚动的 UI（例如绘图图面，或本示例中的地图）时，XY 焦点导航不起作用。 在这种情况下，你可以打开[鼠标模式](#mouse-mode)以允许用户在 UI 元素内部自由导航。

![使用鼠标模式的 UI 元素](images/designing-for-tv/map-mouse-mode.png)

## 鼠标模式

如 [XY 焦点导航和交互](#xy-focus-navigation-and-interaction)中所述，在 Xbox One 上，焦点通过使用 XY 导航系统进行移动，从而允许用户通过向上、向下、向左和向右移动在控件之间转移焦点。 但是，某些控件（如 [WebView](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.webview.aspx) 和 [MapControl](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.maps.mapcontrol.aspx)）需要类似于鼠标的交互，在该交互中，用户可以在控件的边界内自由移动指针。 另外，在某些应用中，用户能够将指针在整个页面上移动，并且此操作有意义，该游戏板/遥控器体验类似于用户可在带有鼠标的电脑上找到的体验。

对于这些方案，你应为整个页面或者在页面内部的控件上请求指针（鼠标模式）。 例如，你的应用可能具有一个包含 `WebView` 控件的页面，它仅在该控件内部使用鼠标模式，而在任何其他位置使用 XY 焦点导航。 若要请求一个指针，你可以指定**在控件或页面已占用时**还是**在页面具有焦点时**需要它。

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

在 HTML 和 JavaScript 应用中，使用以下内容：

```javascript
navigator.gamepadInputEmulation = "keyboard";
```

下图显示游戏板/遥控器在鼠标模式下的按钮映射。

![游戏板/遥控器在鼠标模式下的按钮映射](images/designing-for-tv/mouse-mode.png)

> [!NOTE]
> 鼠标模式仅在带有游戏板/遥控器的 Xbox One 上受支持。 在其他设备系列和输入类型上以静默方式忽略它。

在控件或页面上使用 `RequiresPointer` 属性在其上激活鼠标模式。 `RequiresPointer` 具有三个可能的值：`Never`（默认值）、`WhenEngaged` 和 `WhenFocused`。

> [!NOTE]
> `RequiresPointer` 是新的 API，尚未介绍。 

<!--TODO: Link to doc-->

### 在控件上激活鼠标模式

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

### 在页面上激活鼠标模式

当页面具有属性 `RequiresPointer="WhenFocused"` 时，将在该页面得到焦点时为整个页面激活鼠标模式。 以下代码片段演示了如何为页面提供此属性：

```xml
<Page RequiresPointer="WhenFocused">
    ...
</Page> 
```

> [!NOTE]
> `WhenFocused` 值仅在 [Page](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.page.aspx) 对象上受支持。 如果你尝试在控件上设置此值，将引发异常。

### 针对全屏内容禁用鼠标模式

通常，在全屏显示视频或其他类型的内容时，你将需要隐藏光标，因为它可能会分散用户的注意力。 如果应用的其余部分使用鼠标模式，但你想要在显示全屏内容时将其关闭，将出现此情形。 若要实现此目的，请将全屏内容放置在其自己的 `Page` 上，并按照下面的步骤操作。

1. 在 `App` 对象中，设置 `RequiresPointerMode="WhenRequested"`。
2. 在每个 `Page` 对象中（全屏 `Page` *除外*），设置 `RequiresPointer="WhenFocused"`。
3. 对于全屏 `Page`，设置 `RequiresPointer="Never"`。

这样一来，光标将永远不会在显示全屏内容时出现。

## 焦点视觉对象

焦点视觉对象是当前具有焦点的 UI 元素周围的边框。 这有助于使用户定位，以便他们可以轻松导航你的 UI而不会迷失。

借助添加到焦点视觉对象的视觉更新和大量自定义选项，开发人员可以相信，单个焦点视觉对象将适用于电脑和 Xbox One 以及支持键盘和/或游戏板/遥控器的任何其他 Windows 10 设备。

尽管可以在不同的平台上使用相同的焦点视觉对象，但对于 10 英尺体验，用户遇到此情况的上下文稍有不同。 你应假设用户不会对整个电视屏幕投入完全的注意力，因此当前具有焦点的元素始终对用户清晰可见以避免其在搜寻视觉对象时感到沮丧，这一点很重要。

请记住在使用游戏板或遥控器（而*不是*键盘）时，默认情况下将显示焦点视觉对象，这一点也很重要。 因此，即使你不实现它，当你在 Xbox One 上运行你的应用时，它仍会出现。

### 初始焦点视觉对象放置

在启动应用或导航到某个页面时，将焦点放置在作为用户会执行操作的第一个元素很合理的 UI 元素上。 例如，照片应用可能会将焦点放置在库中的第一个项上，而导航到歌曲详细视图的音乐应用可能将焦点放置在播放按钮上以便于播放音乐。

尝试将初始焦点放置在应用的左上区域（对于从右向左流，则为右上区域）。 大多数用户倾向于先将焦点放置在该角度，因为应用内容流通常在此位置开始。

### 使焦点清晰可见

一个焦点视觉对象应始终在屏幕上可见，以便用户可以从离开的位置继续，而无需搜索焦点。 同样，屏幕上应始终存在可聚焦的项&mdash;例如，不要使用仅带有文本而没有可聚焦元素的弹出窗口。

全屏体验（如观看视频或查看图像）是此规则的一个例外，在全屏情况下不适宜显示焦点视觉。

### 自定义焦点视觉

如果你想要自定义焦点视觉，可以通过以下方式执行此操作：为每一个控件修改与焦点视觉有关的属性。 可以充分利用多个此类属性个性化你的应用。

你甚至可以通过使用视觉状态绘制你自己的焦点视觉，来选择退出系统提供的焦点视觉。 若要了解详细信息，请参阅 [VisualState](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.visualstate.Aspx)。

### 轻型消除覆盖层

为了将用户的注意力吸引到用户当前正在使用游戏控制器或遥控器操作的 UI 元素，当应用在 Xbox One 上运行时，UWP 自动添加“烟”层，该层覆盖弹出窗口 UI 之外的区域。 这无需任何额外工作，但却是在设计 UI 时需要牢记的内容。 可以设置任何 `FlyoutBase` 上的 `LightDismissOverlayMode` 属性以启用或禁用烟层；它默认为 `Auto`，这意味着它将在 Xbox 上处于启用状态，在别处处于禁用状态。 有关详细信息，请参阅[模式和轻型消除](../controls-and-patterns/dialogs-popups-menus.md#modal-vs-light-dismiss)。

## 焦点占用

焦点占用旨在更轻松地使用游戏板或遥控器与应用交互。 

> [!NOTE]
> 设置焦点占用不会影响键盘或其他输入设备。

当 [FrameworkElement](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.frameworkelement.aspx) 对象上的属性 `IsFocusEngagementEnabled` 设置为 `True` 时，它会将控件标记为需要焦点占用。 这意味着用户必须按“A/选择”****按钮来“占用”该控件并与其交互。 当用户完成操作时，他们可以按“B/后退”****按钮脱离该控件并导航以离开它。

> [!NOTE]
> `IsFocusEngagementEnabled` 是新的 API，尚未介绍。

### 焦点捕获

焦点捕获是指用户尝试导航应用 UI 但被“捕获”在控件内，从而难以甚至无法移出该控件时发生的情况。

以下示例显示产生焦点捕获的 UI。

![水平滑块左侧和右侧的按钮](images/designing-for-tv/focus-engagement-focus-trapping.png)

如果用户要从左按钮导航到右按钮，则符合逻辑的假设是他们只需按方向键/左摇杆上的向右键两次。 但是，如果[滑块](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.slider.aspx)不需要占用，则将发生以下行为：当用户第一次按向右键时，焦点将转移到 `Slider`，并且当用户再次按向右键时，`Slider` 的手柄将移到右侧。 用户不断地将手柄移到右侧，从而无法到达按钮。

有多种方法解决此问题。 一个方法是设计一个不同的布局，类似于 [XY 焦点导航和交互](#xy-focus-navigation-and-interaction)中的房地产应用示例，在该示例中我们将**“上一步”**和**“下一步”**按钮重新放置在 `ListView` 上方。 如下图所示垂直堆叠而不是水平堆叠这些控件将解决该问题。

![水平滑块上方和下方的按钮](images/designing-for-tv/focus-engagement-focus-trapping-2.png)

现在用户可以通过按方向键/左摇杆上的向上和向下键来导航到每个控件，并且当 `Slider` 具有焦点时，他们可以按预期通过按向左和向右键来移动 `Slider` 手柄。

解决此问题的另一个方法是在 `Slider` 上要求占用。 如果你设置 `IsFocusEngagementEnabled="True"`，这将导致以下行为。

![在滑块上要求焦点占用，以便用户可以导航到右侧的按钮。](images/designing-for-tv/focus-engagement-slider.png)

当 `Slider` 要求焦点占用时，用户只需按方向键/左摇杆上的向右键两次即可到达右侧的按钮。 此解决方案很出色，因为它无需进行 UI 调整即可产生预期的行为。

### 项控件

除了 [Slider](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.slider.aspx) 控件，还有其他可能要求占用的控件，例如：

- [ListBox](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.listbox.aspx)
- [ListView](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.listview.aspx)
- [GridView](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.gridview.aspx)
- [FlipView](https://msdn.microsoft.com/en-us/library/windows/apps/xaml/windows.ui.xaml.controls.flipview)

与 `Slider` 控件不同，这些控件不会将焦点捕获在自身内部；但是，当它们包含大量数据，它们可能会导致可用性问题。 以下是一个包含大量数据的 `ListView` 的示例。

![带有大量数据的 ListView 以及上方和下方的按钮](images/designing-for-tv/focus-engagement-list-and-grid-controls.png)

与 `Slider` 示例类似，让我们尝试使用游戏板/遥控器从顶部的按钮导航到底部的按钮。 从顶部按钮的焦点开始，按方向键/摇杆上的向下键会将焦点放置在 `ListView` 中的第一个项上（“项 1”）。 当用户再次按向下键时，列表中的下一个项将得到焦点，而不是底部的按钮。 若要到达该按钮，用户必须先导航 `ListView` 中的每一项。 如果 `ListView` 包含大量数据，这可能造成不便且非最佳的用户体验。

若要解决此问题，请在 `ListView` 上设置属性 `IsFocusEngagementEnabled="True"` 以在该控件上要求占用。 这将允许用户只需按向下键即可跳过 `ListView`。 但是，他们将无法滚动浏览列表或从中选择项，除非他们占用该列表，方法是在具有焦点时按“A/选择”****按钮，然后按“B/后退”****按钮来脱离。

![要求占用的 ListView](images/designing-for-tv/focus-engagement-list-and-grid-controls-2.png)

#### ScrollViewer

与这些控件稍有不同的是 [ScrollViewer](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.scrollviewer.aspx)，该控件具有其自己的特点要考虑。 如果你有包含可聚焦内容的 `ScrollViewer`，默认情况下导航到 `ScrollViewer` 将允许你移动其可聚焦元素。 和在 `ListView` 中相同，你必须滚动浏览每一项才能导航到 `ScrollViewer` 外部。 

如果 `ScrollViewer` *没有*可聚焦内容&mdash;例如，如果它仅包含文本&mdash;，则可以设置 `IsFocusEngagementEnabled="True"`，以便用户可以通过使用“A/选择”****按钮占用 `ScrollViewer`。 在用户已占用后，他们可以通过使用**方向键/左摇杆**滚动浏览文本，然后在他们完成后按“B/后退”****按钮来脱离。

另一个方法是在 `ScrollViewer` 上设置 `IsTabStop="True"`，以便当 `ScrollViewer` 内不存在可聚焦元素时用户无需占用控件&mdash;他们只需将焦点放置在该控件上，然后使用**方向键/左摇杆**进行滚动。

### 焦点占用默认值

某些控件导致焦点捕获的频率足以保证其默认设置要求焦点占用，而其他控件默认关闭焦点占用，但可能因打开它而受益。 下表列出了这些控件及其默认焦点占用行为。

| 控件               | 焦点占用默认值  |
|-----------------------|---------------------------|
| CalendarDatePicker    | 开                        |
| FlipView              | 关                       |
| GridView              | 关                       |
| ListBox               | 关                       |
| ListView              | 关                       |
| ScrollViewer          | 关                       |
| SemanticZoom          | 关                       |
| Slider                | 开                        |

当为 `IsFocusEngagementEnabled="True"` 时，所有其他 UWP 控件都不会导致任何行为或视觉更改。

## UI 元素大小调整

由于 10 英尺环境中的应用的用户使用遥控器或游戏板，并且坐在离屏幕数英尺远的位置，因此需要将一些 UI 注意事项纳入设计中。 确保 UI 具有相应的内容密度并且不过于凌乱，以便用户可以轻松地导航和选择元素。 记住：关键在于简洁性。

### 比例系数和自适应布局

**比例系数**有助于确保 UI 元素以适合正在运行应用的设备的大小显示。 在桌面上，此设置在“设置”&gt;“系统”&gt;“显示”****中以滑动值的形式提供。 如果设备支持，此相同设置也存在于手机上。

![更改文本、应用和其他项的大小](images/designing-for-tv/ui-scaling.png) 

在 Xbox One 上，没有此类系统设置；但是，对于要针对电视设置相应大小的 UWP UI 元素，将以 **200%** 的默认值缩放它们。 只要 UI 元素针对其他设备设置相应的大小，也将针对电视设置相应的大小。 Xbox One 以 1080p（1920 x 1080 像素）呈现你的应用。 因此，当显示来自其他设备（如电脑）的应用时，请确保利用[自适应技术](https://msdn.microsoft.com/en-us/windows/uwp/layout/screen-sizes-and-breakpoints-for-responsive-design)使 UI 在 100% 缩放的 960 x 540 px 下外观出色。

针对 Xbox 进行设计与针对电脑进行设计稍有不同，因为你只需要考虑一个分辨率，即 1920 x 1080。 用户的电视是否具有较高的分辨率不重要&mdash;UWP 应用将始终缩放为 1080p。

无论电视分辨率如何，当在 Xbox One 上运行时，还将为你的应用从 200% 集中提取正确的资源大小。

### 内容密度

在设计应用时，请记住用户将从一段距离外观看 UI 并使用遥控器或游戏控制器与其交互，这花费的导航时间比使用鼠标或触摸输入要多。

#### UI 控件的大小

交互式 UI 元素的大小应设置为最低高度 32 epx（有效像素）。 这是常用 UWP 控件的默认值，并且当以 200% 缩放使用时，它可确保 UI 元素在一段距离外可见，并且有助于降低内容密度。 

![100% 和 200% 缩放的 UWP 按钮](images/designing-for-tv/button-100-200.png)

#### 单击数

当用户从电视屏幕的一侧导航到另一侧时，此操作不应超过**六次单击**，以便简化你的 UI。 同样，**简洁性**原则也适用于此处。 有关更多详细信息，请参阅[最少单击路径](#path-of-least-clicks)。

![跨 6 个图标](images/designing-for-tv/six-clicks.png)

### 文本大小

若要使你的 UI 在一段距离外可见，请使用以下经验法则：

* 主要文本和阅读内容：最低 15 epx
* 非关键文本和补充文本：最低 12 epx

当在 UI 中使用较大的文本时，请选取一个没有过分限制屏幕空间的大小，以免占用其他内容可能填充的空间。

### 选择退出比例系数

我们建议你的应用充分利用比例系数支持，这将有助于它通过针对每个设备类型进行缩放在所有设备上以相应方式运行。 但是，可以选择退出此行为并以 100% 缩放设计你的所有 UI。 请注意，你无法将比例系数更改为任何 100% 以外的值。

你可以通过使用以下代码片段选择退出比例系数：

```csharp
bool result = Windows.UI.ViewManagement.ApplicationViewScaling.TrySetDisableLayoutScaling(true);
```

`result` 将通知你是否已成功选择退出。

请务必计算 UI 元素的相应大小，方法是将本主题中提到的*有效*像素值乘以 2，即为*实际*像素值。

## 电视安全区域

由于历史和技术原因，并非所有电视都将内容一直显示到屏幕边缘。 默认情况下，UWP 将避免在电视不安全区域中显示任何 UI 内容，并将改为仅绘制页面背景。

电视不安全区域在下图中由蓝色区域表示。

![电视不安全区域](images/designing-for-tv/tv-unsafe-area.png)

你可以将背景设置为静态或主题色，或设置为图像，如以下代码段所示。

### 主题色

```xml
<Page x:Class="Sample.MainPage"
      Background="{ThemeResource ApplicationPageBackgroundThemeBrush}"/>
```

### 图像

```xml
<Page x:Class="Sample.MainPage"
      Background="\Assets\Background.png"/>
```

这是你的应用在未进行附加工作时的外观。

![电视安全区域](images/designing-for-tv/tv-safe-area.png)

这不是最佳的外观，因为它使应用具有“箱中”效果，其中部分 UI（如导航窗格和网格）看似被切断。 但是，你可以进行优化以将部分 UI 延伸到屏幕边缘，以为应用提供更加电影化的效果。

### 将 UI 绘制到边缘

我们建议你使用特定 UI 元素来延伸到屏幕边缘，以向用户提供更多的沉浸式体验。 这些元素包括 [ScrollViewers](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.scrollviewer.aspx)、[nav panes](https://msdn.microsoft.com/en-us/windows/uwp/controls-and-patterns/nav-pane) 和 [CommandBars](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.commandbar.aspx)。

另一方面，交互式元素和文本始终避开屏幕边缘以确保它们在某些电视上不会被切断，这一点也很重要。 我们建议你在屏幕边缘的 5% 内仅绘制非必要的视觉对象。 如 [UI 元素大小调整](#ui-element-sizing)中所述，遵循 Xbox One 主机的默认比例系数 200% 的 UWP 应用将利用 960 x 540 epx 的区域，因此在应用的 UI 中，你应避免在以下区域中放置必需 UI：

- 距离顶部和底部 27 epx
- 距离左侧和右侧 48 epx

以下部分介绍了如何使你的 UI 延伸到屏幕边缘。

#### 核心窗口边界

对于仅面向 10 英尺体验的 UWP 应用，使用核心窗口边界是更简单的选项。

在 `App.xaml.cs` 的 `OnLaunched` 方法中，添加以下代码：

```csharp
Windows.UI.ViewManagement.ApplicationView.GetForCurrentView().SetDesiredBoundsMode
    (Windows.UI.ViewManagement.ApplicationViewBoundsMode.UseCoreWindow);
```

通过此代码行，应用窗口将延伸到屏幕边缘，因此你需要将所有交互式和必需 UI 移动到之前所述的电视安全区域中。 临时 UI（如上下文菜单和打开的 [ComboBoxes](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.combobox.aspx)）将自动保留在电视安全区域内。

![核心窗口边界](images/designing-for-tv/core-window-bounds.png)

#### 窗格背景 

导航窗格通常绘制在屏幕边缘附近，因此背景应延伸到电视不安全区域中，以免引入不可取的间距。 只需将导航窗格的背景颜色更改为应用的背景颜色即可达成目标。

按上述方法使用核心窗口边界会允许你将 UI 绘制到屏幕边缘，但之后对于 [SplitView](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.splitview.aspx) 内容应使用正边距以确保内容位于电视安全区域内。

![延伸到屏幕边缘的导航窗格](images/designing-for-tv/tv-safe-areas-2.png)

在此处，导航窗格的背景已延伸到屏幕边缘，而其导航项会保留在电视安全区域内。 `SplitView` 的内容（在本例中为项的网格）已延伸到屏幕底部，以便它看起来会持续显示且不会被切断，而网格顶部仍然位于电视安全区域内。 （了解有关如何在[滚动列表和网格的末端](#scrolling-ends-of-lists-and-grids)中执行此操作的详细信息）。

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

[CommandBar](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.commandbar.aspx) 是另一个通常定位在应用的一个或多个边缘附近的窗格示例，因此在电视上，它的背景应延伸到屏幕边缘。 它通常还包含一个“更多”****按钮，由右侧的“...”表示，该按钮应保留在电视安全区域中。 以下是实现所需交互和视觉效果的一些不同策略。

**选项 1**：将 `CommandBar` 背景色更改为透明或与页面背景相同的颜色：

```xml
<CommandBar x:Name="topbar" 
            Background="{ThemeResource SystemControlBackgroundAltHighBrush}">
            ...
</CommandBar>
```

执行此操作将使 `CommandBar` 看起来像与页面其余部分位于同一个背景顶部，因此背景将无缝流动到屏幕边缘。

**选项 2**：添加一个背景矩形，该矩形的填充与 `CommandBar` 背景颜色相同，然后将该矩形置于 `CommandBar` 的下方并填充页面的其余部分：

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
> 如果使用此方法，请注意“更多”****按钮将更改打开的 `CommandBar` 的高度（如有必要），以便在其图标下方显示 `AppBarButton` 的标签。 我们建议你将标签移动到其图标*右侧*以避免此大小调整。 有关详细信息，请参阅 [CommandBar 标签](#commandbar-labels)。

这两种方法还适用于本部分中列出的其他类型的控件。

#### 滚动列表和网格的末端

列表和网格所包含的项通常多于屏幕上同时可以容纳的数量。 在这种情况下，我建议你将列表或网格延伸到屏幕边缘。 水平滚动列表和网格应延伸到右边缘，而垂直滚动应延伸到底部。

![电视安全区域网格切断](images/designing-for-tv/tv-safe-area-grid-cutoff.png)

当列表或网格进行此类延伸时，将焦点视觉对象及其关联项保留在电视安全区域内很重要。

![滚动网格焦点应保留在电视安全区域中。](images/designing-for-tv/scrolling-grid-focus.png)

UWP 具有将焦点视觉对象保留在 [VisibleBounds](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.viewmanagement.applicationview.visiblebounds.aspx) 内部的功能，但你需要添加填充以确保列表/网格项可以滚动到安全区域的视图中。 具体来说，将正边距添加到 [ListView](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.listview.aspx) 或 [GridView](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.gridview.aspx) 的 [ItemsPresenter](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.itemspresenter.aspx)，如以下代码片段所示：

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
> 此代码片段专门用于 `ListView`；对于 `GridView` 样式，请将 [ControlTemplate](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.controltemplate.aspx) 和 [Style](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.style.aspx) 的 [TargetType](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.controltemplate.targettype.aspx) 属性设置为 `GridView`。

## 颜色

默认情况下，通用 Windows 平台不会执行任何操作来更改应用的颜色。 也就是说，你可以对你的应用使用的颜色集进行一些改进以改进电视上的视觉体验。

### 应用程序主题

你可以根据应用的需求选择**应用程序主题**（深色或浅色），也可以选择退出主题。 阅读有关[颜色主题](../style/color.md#color-themes)的主题中一般建议的详细信息。

UWP 还允许应用基于运行这些应用的设备所提供的系统设置动态设置主题。 尽管 UWP 始终遵循用户指定的主题设置，但每台设备还提供相应的默认主题。 由于 Xbox One 的性质，即预期其*媒体*体验多于*办公*体验，因此它默认为深色系统主题。 如果你的应用主题基于系统设置，则预期它在 Xbox One 上默认为深色。

### 主题色

UWP 提供一种便捷方式来公开用户从其系统设置中选择的**主题色**。

在 Xbox One 上，用户能够选择用户颜色，就像他们可以在电脑上选择主题色一样。 只要你的应用通过画笔或颜色资源调用这些主题色，就会使用用户在系统设置中选择的颜色。 请注意，Xbox One 上的主题色按用户而不是按系统设置。

另请注意，Xbox One 上的用户颜色集与电脑、手机和其他设备上的不同。 部分原因归咎于以下事实：遵循本文中所述的相同方法和策略，精选这些颜色以在 Xbox One 上实现最佳的 10 英尺体验。

只要你的应用使用画笔资源（如 **SystemControlForegroundAccentBrush**）或颜色资源 (**SystemAccentColor**)，或者直接通过 [UIColorType.Accent*](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.viewmanagement.uicolortype.aspx) API 调用主题色，这些颜色就将替换为适合电视的主题色。 还通过与电脑和手机上相同的方式从系统中提取高对比画笔颜色，但使用适合电视的颜色。

若要了解有关主题色的一般详细信息，请参阅[主题色](../style/color.md#accent-color)。

### 电视之间的颜色变化

在针对电视进行设计时，请注意，根据呈现颜色的电视，颜色会以非常不同的方式显示。 不要假设颜色看起来会与在监视器上完全相同。 如果你的应用依赖细微的颜色差异来区分 UI 的各个部分，则颜色可能会混合在一起，并且用户可能会混淆。 无论用户使用何种电视，尝试使用差异足够大的颜色，以便用户能够清楚地区分它们。

### 电视安全颜色

颜色的 RGB 值表示红色、绿色和蓝色的浓度。 电视无法很好地处理极强的浓度；因此，在针对 10 英尺体验进行设计时应避免使用这些颜色。 它们会产生奇怪的色带效果，或在某些电视上显示为褪色。 此外，高浓度颜色可能导致高度曝光（附近的像素开始绘制相同的颜色）。 

尽管对于哪些颜色应视为电视安全颜色存在不同的学说，但 RGB 值为 16-235（或十六进制的 10-EB）内的颜色一般可安全用于电视。

![电视安全颜色范围](images/designing-for-tv/tv-safe-colors.png)

### 修复电视不安全颜色

通过将 RGB 值调整到电视安全范围内来单独修复电视不安全颜色通常称为**颜色固定**。 此方法可能适用于不使用丰富调色板的应用。 但是，仅使用此方法修复颜色可能导致颜色互相冲突，这不利于实现最佳的 10 英尺体验。

若要针对电视优化调色板，建议你先通过某种方法（如颜色固定）确保你的颜色是电视安全颜色，然后使用称为**缩放**的方法。

这涉及到通过某个系数缩放所有颜色的 RGB 值以使它们位于电视安全范围内。 缩放应有的所有颜色有助于防止颜色冲突，并且有利于实现更好的 10 英尺体验。

![固定与缩放](images/designing-for-tv/clamping-vs-scaling.png)

### 资源

在对颜色进行更改时，请确保还相应地更新资源。 如果你的应用在 XAML 中使用某个颜色，并且该颜色应当看起来与资源颜色相同，但你仅更新 XAML 代码，则你的资源将看起来颜色不佳。

### UWP 颜色示例

[UWP 颜色主题](../style/color.md#color-themes)围绕应用的背景进行设计，该背景在深色主题中为**黑色**，在浅色主题中为**白色**。 由于黑色和白色都不是电视安全颜色，因此这些颜色需要使用*固定*修复。 修复这些颜色后，所有其他颜色都需要通过*缩放*进行调整，以保留必要的对比度。

<!--[v-lcap to eliot]why is the above paragraph in the past tense?-->
<!--[elcowle] Because this is something that Microsoft had to do to the UWP color themes to accommodate TV-safe colors for Xbox. These themes are then provided in the below code sample.-->

以下示例代码提供已针对电视用途进行优化的颜色主题：

```xml
<Application.Resources>
    <ResourceDictionary>
        <ResourceDictionary.ThemeDictionaries>
            <ResourceDictionary x:Key="Default">
                <SolidColorBrush x:Key="ApplicationPageBackgroundThemeBrush" 
                                 Color="#FF101010"/>
                <Color x:Key="SystemAltHighColor">#FF101010</Color>
                <Color x:Key="SystemAltLowColor">#33101010</Color>
                <Color x:Key="SystemAltMediumColor">#99101010</Color>
                <Color x:Key="SystemAltMediumHighColor">#CC101010</Color>
                <Color x:Key="SystemAltMediumLowColor">#66101010</Color>
                <Color x:Key="SystemBaseHighColor">#FFEBEBEB</Color>
                <Color x:Key="SystemBaseLowColor">#33EBEBEB</Color>
                <Color x:Key="SystemBaseMediumColor">#99EBEBEB</Color>
                <Color x:Key="SystemBaseMediumHighColor">#CCEBEBEB</Color>
                <Color x:Key="SystemBaseMediumLowColor">#66EBEBEB</Color>
                <Color x:Key="SystemChromeAltLowColor">#FFDDDDDD</Color>
                <Color x:Key="SystemChromeBlackHighColor">#FF101010</Color>
                <Color x:Key="SystemChromeBlackLowColor">#33101010</Color>
                <Color x:Key="SystemChromeBlackMediumLowColor">#66101010</Color>
                <Color x:Key="SystemChromeBlackMediumColor">#CC101010</Color>
                <Color x:Key="SystemChromeDisabledHighColor">#FF333333</Color>
                <Color x:Key="SystemChromeDisabledLowColor">#FF858585</Color>
                <Color x:Key="SystemChromeHighColor">#FF767676</Color>
                <Color x:Key="SystemChromeLowColor">#FF1F1F1F</Color>
                <Color x:Key="SystemChromeMediumColor">#FF262626</Color>
                <Color x:Key="SystemChromeMediumLowColor">#FF2B2B2B</Color>
                <Color x:Key="SystemChromeWhiteColor">#FFEBEBEB</Color>
                <Color x:Key="SystemListLowColor">#19EBEBEB</Color>
                <Color x:Key="SystemListMediumColor">#33EBEBEB</Color>
            </ResourceDictionary>
            <ResourceDictionary x:Key="Light">
                <SolidColorBrush x:Key="ApplicationPageBackgroundThemeBrush" 
                                 Color="#FFEBEBEB" /> 
                <Color x:Key="SystemAltHighColor">#FFEBEBEB</Color>
                <Color x:Key="SystemAltLowColor">#33EBEBEB</Color>
                <Color x:Key="SystemAltMediumColor">#99EBEBEB</Color>
                <Color x:Key="SystemAltMediumHighColor">#CCEBEBEB</Color>
                <Color x:Key="SystemAltMediumLowColor">#66EBEBEB</Color>
                <Color x:Key="SystemBaseHighColor">#FF101010</Color>
                <Color x:Key="SystemBaseLowColor">#33101010</Color>
                <Color x:Key="SystemBaseMediumColor">#99101010</Color>
                <Color x:Key="SystemBaseMediumHighColor">#CC101010</Color>
                <Color x:Key="SystemBaseMediumLowColor">#66101010</Color>
                <Color x:Key="SystemChromeAltLowColor">#FF1F1F1F</Color>
                <Color x:Key="SystemChromeBlackHighColor">#FF101010</Color>
                <Color x:Key="SystemChromeBlackLowColor">#33101010</Color>
                <Color x:Key="SystemChromeBlackMediumLowColor">#66101010</Color>
                <Color x:Key="SystemChromeBlackMediumColor">#CC101010</Color>
                <Color x:Key="SystemChromeDisabledHighColor">#FFCCCCCC</Color>
                <Color x:Key="SystemChromeDisabledLowColor">#FF7A7A7A</Color>
                <Color x:Key="SystemChromeHighColor">#FFB2B2B2</Color>
                <Color x:Key="SystemChromeLowColor">#FFDDDDDD</Color>
                <Color x:Key="SystemChromeMediumColor">#FFCCCCCC</Color>
                <Color x:Key="SystemChromeMediumLowColor">#FFDDDDDD</Color>
                <Color x:Key="SystemChromeWhiteColor">#FFEBEBEB</Color>
                <Color x:Key="SystemListLowColor">#19101010</Color>
                <Color x:Key="SystemListMediumColor">#33101010</Color>
            </ResourceDictionary> 
        </ResourceDictionary.ThemeDictionaries>
    </ResourceDictionary>
</Application.Resources>
```

> [!NOTE]
> 浅色主题 **SystemChromeMediumLowColor** 和 **SystemChromeMediumLowColor** 有意设为相同颜色，并且不是因固定而生成的。 

> [!NOTE]
> 十六进制颜色在 **ARGB**（Alpha 红绿蓝）中指定。

在不固定的情况下，我们不建议在能够显示完整范围的监视器上使用电视安全颜色，因为颜色将褪色。 当你的应用在 Xbox 而*不是*其他平台上运行时，改为加载资源字典（上一示例）。 在 `App.xaml.cs` 的 `OnLaunched` 方法中，添加以下检查：

```csharp
if (IsTenFoot)
{ 
    this.Resources.MergedDictionaries.Add(new ResourceDictionary 
    { 
        Source = new Uri("ms-appx:///TenFootStylesheet.xaml") 
    }); 
}
```

> [!NOTE] 
> `IsTenFoot` 变量在 [Xbox 的自定义视觉状态触发器](#custom-visual-state-trigger-for-xbox)中进行定义。

这将确保无论应用在哪台设备上运行都显示正确的颜色，从而为用户提供更好、在审美上更令人愉悦的体验。

## UI 控件指南

有多个 UI 控件可跨多台设备运行良好，但在电视上使用时有些特定的注意事项。 请阅读在针对 10 英尺体验进行设计时使用这些控件的一些最佳做法。

### Pivot 控件

[Pivot](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.pivot.aspx) 通过选择不同标题或选项卡来提供应用内视图的快速导航。 该控件会对具有焦点的标题加下划线，从而在使用游戏板/遥控器时使当前选中标题的显示较为明显。 

![Pivot 下划线](images/designing-for-tv/pivot-underline.png)

<!--By default, when you navigate to a `Pivot`, one of the [PivotItem](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.pivotitem.aspx)s will get focus. However, you can show focus around all the headers by setting `Pivot.HeaderFocusVisualPlacement="ItemHeaders"`.

![Pivot focus around headers](images/designing-for-tv/pivot-headers-focus.png)-->

可以将 [Pivot.HeaderOverflowMode](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.pivot.headeroverflowmode.aspx) 属性设置为 `PivotHeaderOverflowMode.NoWrap`，这样标题不会像在手机和平板电脑上那样在屏幕上换行。 这对大屏幕显示（如电视）来说是更佳的体验，因为标题换行可能会干扰用户。 有关详细信息，请参阅[表和透视表](https://msdn.microsoft.com/windows/uwp/controls-and-patterns/tabs-pivot)。

<!--If you find it necessary to wrap headers, you can set it so that it doesn't show the selected header in the left-most position, like it does by default. When you set `Pivot.IsHeaderItemsCarouselEnabled="False"`, the selected header will move left by the minimal amount required to become fully visible. This is the recommended approach for 10-foot design.

![Pivot headers carousel disabled](images/designing-for-tv/pivot-headers-carousel.png)-->

### 导航窗格

导航窗格（也称为*汉堡菜单*）是 UWP 应用中常用的导航控件。 通常，该窗格内含多个从列表样式菜单中选择的选项，用于将用户转到其他页面。 此窗格通常一开始以折叠方式显示以节省空间，用户可以通过单击某个按钮来打开它。 

用户很容易通过鼠标和触摸操作访问导航窗格，但游戏板/遥控器将使这些窗格不易访问，因为用户必须导航到某个按钮才能打开窗格。 因此，好的做法是让“视图”****按钮打开导航窗格，以及允许用户通过一直向左导航页面来打开该窗格。 这样可以使用户轻松访问窗格内容。 有关导航窗格在不同的屏幕大小中的行为以及游戏板/遥控器导航的最佳做法的详细信息，请参阅[导航窗格](https://msdn.microsoft.com/windows/uwp/controls-and-patterns/nav-pane)。

### CommandBar 标签

最好是将标签放置于 [CommandBar](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.commandbar.aspx) 上的图标右侧，以便最大程度降低标签高度并保持一致性。 为此，你可以将 [CommandBar.DefaultLabelPosition](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.commandbar.defaultlabelposition.aspx) 属性设置为 `CommandBarDefaultLabelPosition.Right`。

![图标右侧带有标签的 CommandBar](images/designing-for-tv/commandbar.png)

设置此属性还会导致标签始终显示，这适用于 3 米体验，因为它最大程度减少了用户的单击次数。 这也是可供其他设备类型遵循的绝佳模型。

<!--When there isn't enough space in the window to fit all of the `AppBarButton`s, buttons move into an overflow menu, which is accessed by selecting the "..." button. This happens dynamically as the screen resizes. This generally shouldn't be a problem for TV because the screen size is so large, but if you find that you have overflow buttons, you can specify which appear first using the `AppBarButton.DynamicOverflowOrder` property.

![CommandBar with overflow commands](images/designing-for-tv/commandbar-overflow.png)-->

### 工具提示

引入了 [Tooltip](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.tooltip.aspx) 控件，可作为一种在用户将鼠标悬停在元素上或点击并按住元素上的图片时在 UI 中提供更多信息的方法。 对于游戏板和遥控器，`Tooltip` 将在元素得到焦点的短暂时间后显示、在屏幕上保留一小段时间，然后消失。 如果使用了太多 `Tooltip`，此行为可能令人分心。 在针对电视进行设计时，请尝试避免使用 `Tooltip`。

### 按钮样式

当标准 UWP 按钮适用于电视时，某些按钮视觉样式能更好地将注意力吸引到 UI，你可能希望针对所有平台考虑这些样式，这对于 10 英尺体验尤其有利，因为可以受益于清晰传达焦点所在的位置。 若要阅读有关这些样式的详细信息，请参阅[按钮](https://msdn.microsoft.com/windows/uwp/controls-and-patterns/buttons)。

### 嵌套 UI 元素

嵌套 UI 公开包含在容器 UI 元素内的可操作项，其中嵌套项和容器项可彼此独立捕获焦点。

嵌套 UI 较适合某些输入类型，但对于依赖 XY 导航的游戏板和遥控器不一定适用。 请务必遵循本主题中的指南操作，确保你的 UI 已针对 10 英尺环境进行了优化，并且用户可以轻松地访问所有可交互元素。 一个常见解决方案是将嵌套 UI 元素置于 `ContextFlyout` 中（请参阅 [CommandBar 和 ContextFlyout](#commandbar-and-contextflyout)）。

有关嵌套 UI 的详细信息，请参阅[列表项中的嵌套 UI](../controls-and-patterns/nested-ui.md)。

### MediaTransportControls

[MediaTransportControls](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.mediatransportcontrols.aspx) 元素通过提供默认的播放体验（即允许用户播放、暂停、打开隐藏式字幕等），允许用户与其媒体交互。 此控件是 [MediaPlayerElement](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Controls.MediaPlayerElement.aspx) 的一个属性，支持以下两个布局选项：*单行*和*双行*。 在单行布局中，滑块和播放按钮位于同一行，而播放/暂停按钮位于滑块左侧。 在双行布局中，滑块单独占用一行，而播放按钮位于单独的下一行。 当针对 10 英尺体验进行设计时，应使用双行布局，因为它能为游戏板提供更好的导航。 若要启用双行布局，请在 `MediaPlayerElement` 的 [TransportControls](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.mediaplayerelement.transportcontrols.aspx) 属性中的 `MediaTransportControls` 元素上设置 `IsCompact="False"`。

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

> ![注意] `MediaPlayerElement` 仅在 Windows 10 版本 1607 及更高版本中可用。 如果要针对早期版本的 Windows 10 开发应用，你将需要改用 [MediaElement](https://msdn.microsoft.com/library/windows/apps/br242926)。 上述建议也适用于 `MediaElement`，并且可通过相同方式访问 `TransportControls` 属性。

## Xbox 的自定义视觉状态触发器

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
bool IsTenFoot = (Windows.System.Profile.AnaylticsInfo.VersionInfo.DeviceFamily == 
                    "Windows.Xbox");
```

然后，你可以遵循此检查对代码块中的 UI 进行相应调整。 [UWP 颜色示例](#uwp-color-sample)中显示了这一示例。

## 小结

针对 10 英尺体验进行设计需要考虑特殊的注意事项，这些注意事项有别于针对任何其他平台进行设计。 当然直接将 UWP 应用移植到 Xbox One 也能使其工作，但它不一定已针对 10 英尺体验进行优化，并且可能导致用户沮丧。 按照本文中的指南进行操作可确保你的应用在电视上达到最佳状态。

## 相关文章

- [通用 Windows 平台 (UWP) 应用的设备基础版](device-primer.md)
- [游戏板和遥控器交互](gamepad-and-remote-interactions.md)
- [UWP 应用中的声音](../style/sound.md)



<!--HONumber=Aug16_HO3-->


