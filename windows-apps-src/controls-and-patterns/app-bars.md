---
author: Jwmsft
Description: "命令栏使用户能够轻松地访问应用的最常见任务。"
title: "应用栏和命令栏"
label: App bars/command bars
template: detail.hbs
op-migration-status: ready
ms.author: jimwalk
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, uwp
ms.assetid: 868b4145-319b-4a97-82bd-c98d966144db
ms.openlocfilehash: 6dc3c9d15ebbda67dd055adb4b9d5548b6ac81e3
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="app-bar-and-command-bar"></a>应用栏和命令栏

<link rel="stylesheet" href="https://az835927.vo.msecnd.net/sites/uwp/Resources/css/custom.css"> 

命令栏（也称为“应用栏”）使用户能够轻松访问你的应用的最常用操作，还可用于显示特定于用户上下文的命令或选项，例如照片选择或绘图模式。 它们还可用于在应用页面或应用的各个部分之间导航。 命令栏可以与任何导航模式一起使用。

![带有图标的命令栏示例](images/controls_appbar_icons.png)

<div class="important-apis" >
<b>重要的 API</b><br/>
<ul>
<li>[**CommandBar**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.commandbar.aspx)</li>
<li>[**AppBarButton**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.appbarbutton.aspx) </li>
<li> [**AppBarToggleButton**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.appbartogglebutton.aspx)</li>
<li>[**AppBarSeparator**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.appbarseparator.aspx) </li>
</ul>
</div>


## <a name="is-this-the-right-control"></a>这是正确的控件吗？

CommandBar 控件是一款通用、灵活、轻型的控件，可显示复杂内容（如图像或文本块）以及简单的命令（如 [AppBarButton](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.appbarbutton.aspx)、[AppBarToggleButton](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.appbartogglebutton.aspx) 和 [AppBarSeparator](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.appbarseparator.aspx) 控件）。

XAML 提供 AppBar 控件和 CommandBar 控件。 应仅在升级使用 AppBar 的通用 Windows 8 应用时使用 AppBar，并且需要最大程序减少更改。 对于 Windows 10 中的新应用，我们建议改用 CommandBar 控件。 此文档假定你将要使用 CommandBar 控件。

## <a name="examples"></a>示例
Microsoft 照片应用中扩展的命令栏。

![Microsoft 照片应用中的命令栏](images/control-examples/command-bar-photos.png)

Windows Phone 的 Outlook 日历中的命令栏。

![Outlook 日历应用中的命令栏](images/control-examples/command-bar-calendar-phone.png)

## <a name="anatomy"></a>结构

默认情况下，命令栏显示一行图标按钮和一个可选的“查看更多”按钮，该按钮由省略号 \[•••\] 表示。 下面是由稍后所示的示例代码创建的命令栏。 它在其关闭、精简状态下显示。

![关闭的命令栏](images/command-bar-compact.png)

命令栏可在如下所示的关闭、最小状态下显示。 有关详细信息，请参阅[打开和关闭状态](#open-and-closed-states)部分。

![关闭的命令栏](images/command-bar-minimal.png)

下面是处于打开状态的相同命令栏。 标签标识控件的主要部分。

![关闭的命令栏](images/commandbar_anatomy_open.png)

命令栏分为 4 个主要区域：
- “查看更多”\[•••\] 按钮显示在该栏的右侧。 按“查看更多”\[•••\] 按钮具有两种效果：显示主要命令按钮上的标签，以及在存在任何辅助命令的情况下打开溢出菜单。 在最新的 SDK 中，当不存在任何辅助命令和隐藏标签时，该按钮将不可见。 [**OverflowButtonVisibility**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.commandbar.overflowbuttonvisibility.aspx) 属性允许应用更改此默认自动隐藏行为。
- 内容区域与栏的左侧对齐。 如果填充了 [**Content**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.contentcontrol.content.aspx) 属性，将显示该区域。
- 主要命令区域与栏的右侧对齐，位于“查看更多”\[•••\] 按钮旁边。 如果填充了 [**PrimaryCommands**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.commandbar.primarycommands.aspx) 属性，将显示该区域。  
- 仅在命令栏处于打开状态并且填充了 [**SecondaryCommands**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.commandbar.secondarycommands.aspx) 属性时才显示溢出菜单。 当空间有限时，新的动态溢出行为会自动将主要命令移到 SecondaryCommands 区域中。

当 [FlowDirection](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.frameworkelement.flowdirection.aspx) 为 **RightToLeft** 时，布局会反转。

## <a name="create-a-command-bar"></a>创建命令栏
此示例创建以前显示的命令栏。

```xaml
<CommandBar>
    <AppBarToggleButton Icon="Shuffle" Label="Shuffle" Click="AppBarButton_Click" />
    <AppBarToggleButton Icon="RepeatAll" Label="Repeat" Click="AppBarButton_Click"/>
    <AppBarSeparator/>
    <AppBarButton Icon="Back" Label="Back" Click="AppBarButton_Click"/>
    <AppBarButton Icon="Stop" Label="Stop" Click="AppBarButton_Click"/>
    <AppBarButton Icon="Play" Label="Play" Click="AppBarButton_Click"/>
    <AppBarButton Icon="Forward" Label="Forward" Click="AppBarButton_Click"/>

    <CommandBar.SecondaryCommands>
        <AppBarButton Icon="Like" Label="Like" Click="AppBarButton_Click"/>
        <AppBarButton Icon="Dislike" Label="Dislike" Click="AppBarButton_Click"/>
    </CommandBar.SecondaryCommands>

    <CommandBar.Content>
        <TextBlock Text="Now playing..." Margin="12,14"/>
    </CommandBar.Content>
</CommandBar>
```

## <a name="commands-and-content"></a>命令和内容
CommandBar 控件具有三个可用于添加命令和内容的属性：[**PrimaryCommands**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.commandbar.primarycommands.aspx)、[**SecondaryCommands**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.commandbar.secondarycommands.aspx) 和 [**Content**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.contentcontrol.content.aspx)。


### <a name="primary-actions-and-overflow"></a>主要操作和溢出

默认情况下，你添加到命令栏中的项目也会添加到 **PrimaryCommands** 集合中。 这些命令显示在“查看更多”\[•••\] 按钮的左侧，我们将该区域称为操作空间。 将最重要的命令、你希望在栏中保持可见的命令放置在操作空间中。 在最小的屏幕（宽度为 320 epx）上，命令栏的操作空间中可放入最多 4 个项目。

你可以将命令添加到 **SecondaryCommands** 集合，这些项目将显示在溢出区域中。 将不太重要的命令放置在溢出区域内。

默认溢出区域的样式设置为与栏不同。 你可以通过将 [**CommandBarOverflowPresenterStyle**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.commandbar.commandbaroverflowpresenterstyle.aspx) 属性设置为面向 [**CommandBarOverflowPresenter**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.style.aspx) 的[样式](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.commandbaroverflowpresenter.aspx)来调整样式设置。

你可以根据需要以编程方式在 PrimaryCommands 和 SecondaryCommands 之间移动命令。 

<div class="microsoft-internal-note">
命令还可以在命令栏宽度发生变化（例如，用户调整应用窗口大小）时自动移入或移出溢出。 动态溢出默认处于打开状态，但应用可以通过更改 `IsDynamicOverflowEnabled` 属性的值关闭此行为。
</div>

### <a name="app-bar-buttons"></a>应用栏按钮

PrimaryCommands 和 SecondaryCommands 都只能使用[**AppBarButton**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.appbarbutton.aspx)、[**AppBarToggleButton**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.appbartogglebutton.aspx) 和 [**AppBarSeparator**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.appbarseparator.aspx) 命令元素填充。 优化这些控件以供在命令栏中使用，并且根据是在操作空间中还是在溢出区域中使用控件来更改其外观。

应用栏按钮控件以一个图标和关联标签为特征。 它们有两种大小；正常和精简。 默认情况下，显示文本标签。 当 [**IsCompact**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.appbarbutton.iscompact.aspx) 属性设置为 **true** 时，将隐藏文本标签。 在 CommandBar 控件中使用时，命令栏随着命令栏打开和关闭而自动覆盖按钮的 IsCompact 属性。

若要将应用栏按钮标签放到其图标的右侧，应用可以使用 CommandBar 的新 [**DefaultLabelPosition**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.commandbar.defaultlabelposition.aspx) 属性。

```xaml
<CommandBar DefaultLabelPosition="Right">
    <AppBarToggleButton Icon="Shuffle" Label="Shuffle"/>
    <AppBarToggleButton Icon="RepeatAll" Label="Repeat"/>
</CommandBar>
```

下面是应用绘制的上述代码片段的外观。

![标签位于右侧的命令栏](images/app-bar-labels-on-right.png)

单独的应用栏按钮无法移动其标签位置，此操作必须作为一个整体在命令栏上完成。 应用栏按钮可以通过将新的 [**LabelPosition**](https://msdn.microsoft.com/library/windows/apps/mt710920.aspx) 属性设置为 **Collapsed** 来指定其标签从不显示。 我们建议将此设置的使用限制为可全局识别的图标，如“+”。

在溢出菜单中放置应用栏按钮 (SecondaryCommands) 时，它仅显示为文本。 将忽略溢出中的应用栏按钮的 **LabelPosition**。 下面是作为主要命令（顶部）在操作空间中显示以及作为辅助命令（底部）在溢出区域中显示的相同应用栏切换按钮。

![作为主要和辅助命令的应用栏按钮](images/app-bar-toggle-button-two-modes.png)

- *如果某个命令会一直显示在各个页面上，则最好将该命令放置在一个一致的位置上。*
- *我们建议将“接受”、“是”和“确定”命令放置在“拒绝”、“否”和“取消”的左侧。 一致性可以增加用户浏览系统的信心，并帮助他们在应用之间传递应用导航知识。*

### <a name="button-labels"></a>按钮标签

我们建议应用栏按钮标签要保持简短，最好是一个词。 较长的标签放在应用栏按钮图标下会换行为多行，从而增加了打开的命令栏的总体高度。 你可以在文本中包含软连字符 (0x00AD)，以使标签提示应发生换行的字符边界。 在 XAML 中，你使用转义序列表示它，如下所示：

```xaml
<AppBarButton Icon="Back" Label="Areally&#x00AD;longlabel"/>
```

当标签在提示的位置换行时，它如下所示。

![带有换行标签的应用栏按钮](images/app-bar-button-label-wrap.png)

### <a name="other-content"></a>其他内容

你可以通过设置 **Content** 属性来将任何 XAML 元素添加到内容区域。 如果要添加多个元素，需要将它们放置在面板容器中，并使该面板成为 Content 属性的单个子对象。

当同时存在主要命令和内容时，主要命令优先，并且可能导致内容进行裁剪。 
<div class="microsoft-internal-note">
启用动态溢出时不会裁剪内容，这是因为主要命令将移动到溢出菜单中，从而为内容释放空间。
</div>

当 [**ClosedDisplayMode**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.appbar.closeddisplaymode.aspx) 为 **Compact** 时，如果内容大于命令栏的精简大小，则可能会剪裁内容。 你应处理 [**Opening**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.appbar.opening.aspx) 和 [**Closed**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.appbar.closed.aspx) 事件以在内容区域中显示或隐藏部分 UI，以使它们不会进行剪裁。 有关详细信息，请参阅[打开和关闭状态](#open-and-closed-states)部分。

## <a name="open-and-closed-states"></a>打开和关闭状态

可以打开或关闭命令栏。 用户可通过按“查看更多”\[•••\] 按钮在这些状态之间切换。 你可以通过设置 [**IsOpen**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.appbar.isopen.aspx) 属性来以编程方式在它们之间切换。 处于打开状态时，将显示主要命令按钮，并带有文本标签，并且如果存在辅助命令，溢出菜单也将处于打开状态，如之前所示。

你可以使用 [**Opening**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.appbar.opening.aspx)、[**Opened**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.appbar.opened.aspx)、[**Closing**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.appbar.closing.aspx) 和 [**Closed**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.appbar.closed.aspx) 事件来响应要打开或关闭的命令栏。  
- Opening 和 Closing 事件在过渡动画开始前发生。
- Opened 和 Closed 事件在过渡完成后发生。

在此示例中，Opening 和 Closing 事件用于更改命令栏的不透明度。 命令栏处于关闭状态时，它是半透明的，以使应用背景隐约显示出来。 命令栏处于打开状态时，命令栏变为不透明，以使用户专注于命令。

```xaml
<CommandBar Opening="CommandBar_Opening"
            Closing="CommandBar_Closing">
    <AppBarButton Icon="Accept" Label="Accept"/>
    <AppBarButton Icon="Edit" Label="Edit"/>
    <AppBarButton Icon="Save" Label="Save"/>
    <AppBarButton Icon="Cancel" Label="Cancel"/>
</CommandBar>
```

```csharp
private void CommandBar_Opening(object sender, object e)
{
    CommandBar cb = sender as CommandBar;
    if (cb != null) cb.Background.Opacity = 1.0;
}

private void CommandBar_Closing(object sender, object e)
{
    CommandBar cb = sender as CommandBar;
    if (cb != null) cb.Background.Opacity = 0.5;
}

```

### <a name="closeddisplaymode"></a>ClosedDisplayMode

你可以通过设置 [**ClosedDisplayMode**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.appbar.closeddisplaymode.aspx) 属性来控制命令栏在关闭状态下的显示方式。 有三种可供选择的关闭显示模式：
- **精简**：默认模式。 显示内容、不带有标签的主要命令图标和“查看更多”\[•••\] 按钮。
- **最小**：仅显示充当“查看更多”\[•••\] 按钮的细栏。 用户可以按栏上的任意位置来将其打开。
- **隐藏**：命令栏在关闭时不显示。 这对于显示带有内联命令栏的上下文命令非常有用。 在此情况下，你必须通过设置 **IsOpen** 属性或将 ClosedDisplayMode 更改为**最小**或**精简**来以编程方式打开命令栏。

此处，命令栏用于为 [RichEditBox](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.richeditbox.aspx) 承载简单格式化命令。 当编辑框没有焦点时，格式化命令可能令人分心，因此隐藏它们。 当使用编辑框时，命令栏的 ClosedDisplayMode 更改为精简，以便使格式化命令可见。

```xaml
<StackPanel Width="300"
            GotFocus="EditStackPanel_GotFocus"
            LostFocus="EditStackPanel_LostFocus">
    <CommandBar x:Name="FormattingCommandBar" ClosedDisplayMode="Hidden">
        <AppBarButton Icon="Bold" Label="Bold" ToolTipService.ToolTip="Bold"/>
        <AppBarButton Icon="Italic" Label="Italic" ToolTipService.ToolTip="Italic"/>
        <AppBarButton Icon="Underline" Label="Underline" ToolTipService.ToolTip="Underline"/>
    </CommandBar>
    <RichEditBox Height="200"/>
</StackPanel>
```

```csharp
private void EditStackPanel_GotFocus(object sender, RoutedEventArgs e)
{
    FormattingCommandBar.ClosedDisplayMode = AppBarClosedDisplayMode.Compact;
}

private void EditStackPanel_LostFocus(object sender, RoutedEventArgs e)
{
    FormattingCommandBar.ClosedDisplayMode = AppBarClosedDisplayMode.Hidden;
}
```

>**注意**&nbsp;&nbsp;编辑命令的实现不在本示例范围内。 有关详细信息，请参阅 [RichEditBox](rich-edit-box.md) 文章。

尽管最小和隐藏模式在某些情况下很有用，但请记住，隐藏所有操作可能会使用户困惑。

更改 ClosedDisplayMode 以向用户提供或多或少的提示会影响周围元素的布局。 相比之下，当 CommandBar 在关闭和打开之间转换时，它不会影响其他元素的布局。

### <a name="issticky"></a>IsSticky

打开命令栏后，如果用户与控制之外的任意位置的应用进行交互，则默认情况下将消除溢出菜单并隐藏标签。 通过这种方式关闭它称为*轻型消除*。 你可以通过设置 [**IsSticky**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.appbar.issticky.aspx) 属性来控制如何消除该栏。 当栏处于粘滞状态 (`IsSticky="true"`) 时，无法通过轻型消除手势关闭它。 在用户按“查看更多”\[•••\] 按钮或从溢出菜单中选择项目之前，该栏保持打开状态。 我们建议避免使用粘滞命令栏，因为它们不符合用户对轻型消除的预期。

## <a name="dos-and-donts"></a>应做事项和禁止事项

### <a name="placement"></a>放置

命令栏可以放置在应用窗口顶部、应用窗口底部和内联放置。

![应用栏放置示例 1](images/AppbarGuidelines_Placement1.png)

-   对于较小的手持设备，我们建议将命令栏定位在屏幕底部以易于操作。
-   对于拥有较大屏幕的设备，如果你只放置一个命令栏，我们建议将其靠近窗口顶部放置。
使用 [**DiagonalSizeInInches**](https://msdn.microsoft.com/library/windows/apps/windows.graphics.display.displayinformation.diagonalsizeininches.aspx) API 确定物理屏幕大小。

命令栏可以放置在单视图屏幕（左边的示例）和多视图屏幕（右边的示例）上的以下屏幕区域中。 内联命令栏可以放置在操作空间中的任意位置。

![应用栏放置示例 2](images/AppbarGuidelines_Placement2.png)

>**触摸设备**：如果命令栏必须在触摸键盘或软输入面板 (SIP) 出现时对用户保持可见，则可以将命令栏分配到某个页面的 [BottomAppBar](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.page.bottomappbar.aspx) 属性，它将转换为在 SIP 出现时保持可见。 否则，你应内联放置命令栏，并根据应用内容定位。

### <a name="actions"></a>操作

根据其可见性设置命令栏中的操作的优先级。

-   将最重要的命令、你希望在栏中保持可见的命令放置在操作空间中的前几个插槽中。 在最小的屏幕（宽度为 320 epx）上，命令栏的操作空间中将放入 2 到 4个项目，具体取决于其他屏幕 UI。
-   稍后将不太重要的命令放置在应用的操作空间中或溢出区域的前几个插槽内。 这些命令将在应用具有足够的屏幕空间时可见，但是将在空间不足时进入溢出区域的下拉菜单中。
-   将最不重要的命令放置在溢出区域内。 这些命令将始终显示在下拉菜单中。

如果某个命令会一直显示在各个页面上，则最好将该命令放置在一个一致的位置上。 我们建议将“接受”、“是”和“确定”命令放置在“拒绝”、“否”和“取消”的左侧。 一致性可以增加用户浏览系统的信心，并帮助他们在应用之间传递应用导航知识。

尽管你可以将所有操作都放置在溢出区域内以使命令栏上只有“查看更多”\[•••\] 按钮可见，但请记住，隐藏所有操作可能会使用户产生困惑。

### <a name="command-bar-flyouts"></a>命令栏浮出控件

考虑对这些命令进行逻辑分组，例如将“答复”、“全部答复”和“转发”置于“响应”菜单中。 虽然一个应用栏按钮通常激活单个按钮，但应用栏按钮可用于显示有自定义内容的 [**MenuFlyout**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.menuflyout.aspx) 或 [**Flyout**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.flyout.aspx)。

![命令栏浮出控件示例](images/AppbarGuidelines_Flyouts.png)

### <a name="overflow-menu"></a>溢出菜单

![带有“更多”区域的命令栏示例](images/AppbarGuidelines_Illustration.png)

-   溢出菜单由“查看更多”\[•••\] 按钮（菜单的可见入口点）表示。 它位于工具栏的最右边，与主要操作相邻。
-   溢出区域用于放置不太常用的操作。
-   在断点处，操作可以在主要操作空间和溢出菜单之间往返。 你还可以指定操作始终保留在主要操作空间中，而不必考虑屏幕或应用窗口大小。
-   即使应用栏在较大屏幕上展开，不常用操作也可以保留在溢出菜单中。

## <a name="adaptability"></a>适应性

-   在纵向和横向方向中，应用栏中应该显示相同数量的操作，这可以降低用户的认知负荷。 所提供的操作数应由处于纵向的设备宽度确定。
-   在可能一只手就可操作的小屏幕上，应用栏应靠近屏幕底部放置。
-   对于较大的屏幕，将应用栏靠近窗口顶部放置会使它们更为醒目，并且更易于发现。
-   通过定位断点，你可以随着窗口大小的更改将操作移入和移出菜单。
-   通过定位屏幕对角线，你可以根据设备屏幕大小来修改应用栏位置。
-   请考虑将标签移动到应用栏按钮图标的右侧，以改善可读性。 底部的标签需要用户打开命令栏才会显示，而右侧的标签在命令栏关闭的情况下也可见。 此优化在较大的窗口上运行良好。

## <a name="get-the-sample-code"></a>获取示例代码
* [命令示例](http://go.microsoft.com/fwlink/p/?LinkId=620019)
* [XAML UI 基本示例](https://github.com/Microsoft/Windows-universal-samples/blob/master/Samples/XamlUIBasics)

## <a name="related-articles"></a>相关文章

* [UWP 应用的命令设计基础知识](../layout/commanding-basics.md)
* [**CommandBar 类**](https://msdn.microsoft.com/library/windows/apps/dn279427)
