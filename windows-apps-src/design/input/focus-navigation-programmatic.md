---
Description: 了解如何使用 Windows 应用程序中的键盘、游戏板和辅助工具以编程方式管理焦点导航。
title: 使用键盘、手柄和辅助功能工具的编程焦点导航
label: Programmatic focus navigation
keywords: 键盘, 游戏控制器, 遥控器, 导航, 导航策略, 输入, 用户交互, 辅助功能, 可用性
ms.date: 03/19/2018
ms.topic: article
pm-contact: miguelrb
design-contact: kimsea
dev-contact: niallm
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: 6b66363588ddad01b05ccc9cc6b3b7912fa21594
ms.sourcegitcommit: 0dee502484df798a0595ac1fe7fb7d0f5a982821
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/08/2020
ms.locfileid: "82970142"
---
# <a name="programmatic-focus-navigation"></a>编程焦点导航

![键盘、遥控器和方向键](images/dpad-remote/dpad-remote-keyboard.png)

若要在 Windows 应用程序中以编程方式移动焦点，可以使用[system.windows.input.focusmanager>. TryMoveFocus](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.focusmanager#Windows_UI_Xaml_Input_FocusManager_TryMoveFocus_Windows_UI_Xaml_Input_FocusNavigationDirection_)方法或[FindNextElement](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.focusmanager#Windows_UI_Xaml_Input_FocusManager_FindNextElement_Windows_UI_Xaml_Input_FocusNavigationDirection_)方法。

[TryMoveFocus](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.focusmanager#Windows_UI_Xaml_Input_FocusManager_TryMoveFocus_Windows_UI_Xaml_Input_FocusNavigationDirection_) 将会尝试将焦点从具有焦点的元素更改到指定方向的下一个可聚焦元素，而 [FindNextElement](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.focusmanager#Windows_UI_Xaml_Input_FocusManager_FindNextElement_Windows_UI_Xaml_Input_FocusNavigationDirection_) 将会根据指定的导航方向检索将获得焦点的元素（作为 [DependencyObject](https://docs.microsoft.com/uwp/api/windows.ui.xaml.dependencyobject)）（仅限定向导航，不能用于模仿 Tab 键导航）。

> [!NOTE]
> 我们建议使用 [FindNextElement](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.focusmanager#Windows_UI_Xaml_Input_FocusManager_FindNextElement_Windows_UI_Xaml_Input_FocusNavigationDirection_) 方法而不是 [FindNextFocusableElement](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.focusmanager#Windows_UI_Xaml_Input_FocusManager_FindNextFocusableElement_Windows_UI_Xaml_Input_FocusNavigationDirection_)，因为 FindNextFocusableElement 将会检索 UIElement，如果下一个可聚焦元素不是 UIElement（例如超链接对象），该方法将返回 null。 

## <a name="find-a-focus-candidate-within-a-scope"></a>查找范围内的候选焦点

你可以自定义 [TryMoveFocus](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.focusmanager#Windows_UI_Xaml_Input_FocusManager_TryMoveFocus_Windows_UI_Xaml_Input_FocusNavigationDirection_) 和 [FindNextElement](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.focusmanager#Windows_UI_Xaml_Input_FocusManager_FindNextElement_Windows_UI_Xaml_Input_FocusNavigationDirection_) 的焦点导航行为，包括在特定 UI 树中搜索下一个焦点或排除特定元素不予考虑。

此示例使用井字游戏演示 [TryMoveFocus](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.focusmanager#Windows_UI_Xaml_Input_FocusManager_TryMoveFocus_Windows_UI_Xaml_Input_FocusNavigationDirection_) 和 [FindNextElement](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.focusmanager#Windows_UI_Xaml_Input_FocusManager_FindNextElement_Windows_UI_Xaml_Input_FocusNavigationDirection_) 方法。

```xaml
<StackPanel Orientation="Horizontal"
                VerticalAlignment="Center"
                HorizontalAlignment="Center" >
    <Button Content="Start Game" />
    <Button Content="Undo Movement" />
    <Grid x:Name="TicTacToeGrid" KeyDown="OnKeyDown">
        <Grid.ColumnDefinitions>
            <ColumnDefinition Width="50" />
            <ColumnDefinition Width="50" />
            <ColumnDefinition Width="50" />
        </Grid.ColumnDefinitions>
        <Grid.RowDefinitions>
            <RowDefinition Height="50" />
            <RowDefinition Height="50" />
            <RowDefinition Height="50" />
        </Grid.RowDefinitions>
        <myControls:TicTacToeCell 
            Grid.Column="0" Grid.Row="0" 
            x:Name="Cell00" />
        <myControls:TicTacToeCell 
            Grid.Column="1" Grid.Row="0" 
            x:Name="Cell10"/>
        <myControls:TicTacToeCell 
            Grid.Column="2" Grid.Row="0" 
            x:Name="Cell20"/>
        <myControls:TicTacToeCell 
            Grid.Column="0" Grid.Row="1" 
            x:Name="Cell01"/>
        <myControls:TicTacToeCell 
            Grid.Column="1" Grid.Row="1" 
            x:Name="Cell11"/>
        <myControls:TicTacToeCell 
            Grid.Column="2" Grid.Row="1" 
            x:Name="Cell21"/>
        <myControls:TicTacToeCell 
            Grid.Column="0" Grid.Row="2" 
            x:Name="Cell02"/>
        <myControls:TicTacToeCell 
            Grid.Column="1" Grid.Row="2" 
            x:Name="Cell22"/>
        <myControls:TicTacToeCell 
            Grid.Column="2" Grid.Row="2" 
            x:Name="Cell32"/>
    </Grid>
</StackPanel>
```

```csharp
private void OnKeyDown(object sender, KeyRoutedEventArgs e)
{
    DependencyObject candidate = null;

    var options = new FindNextElementOptions ()
    {
        SearchRoot = TicTacToeGrid,
        XYFocusNavigationStrategyOverride = XYFocusNavigationStrategyOverride.Projection
    };

    switch (e.Key)
    {
        case Windows.System.VirtualKey.Up:
            candidate = 
                FocusManager.FindNextElement(
                    FocusNavigationDirection.Up, options);
            break;
        case Windows.System.VirtualKey.Down:
            candidate = 
                FocusManager.FindNextElement(
                    FocusNavigationDirection.Down, options);
            break;
        case Windows.System.VirtualKey.Left:
            candidate = FocusManager.FindNextElement(
                FocusNavigationDirection.Left, options);
            break;
        case Windows.System.VirtualKey.Right:
            candidate = 
                FocusManager.FindNextElement(
                    FocusNavigationDirection.Right, options);
            break;
    }
    // Also consider whether candidate is a Hyperlink, WebView, or TextBlock.
    if (candidate != null && candidate is Control)
    {
        (candidate as Control).Focus(FocusState.Keyboard);
    }
}
```

使用 [FindNextElementOptions](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.findnextelementoptions) 可以进一步自定义候选焦点的识别方式。 此对象提供以下属性：

- [SearchRoot](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.findnextelementoptions#Windows_UI_Xaml_Input_FindNextElementOptions_SearchRoot) - 将焦点导航候选项的搜索范围确定为此 DependencyObject 的子级。 Null 指示从可视化树的根级开始搜索。

> [!Important] 
> 如果对 **SearchRoot** 的后代应用了一种或多种转换，因而将其放置在了定向区域之外，这些元素仍被视为候选项。

- [ExclusionRect](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.findnextelementoptions#Windows_UI_Xaml_Input_FindNextElementOptions_ExclusionRect) - 使用“虚构的”边界矩形标识焦点导航候选项，从导航焦点中排除所有重叠的对象。 此矩形仅用于计算，不会添加到可视化树中。
- [HintRect](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.findnextelementoptions#Windows_UI_Xaml_Input_FindNextElementOptions_HintRect) - 使用“虚构的”边界矩形标识焦点导航候选项，标识最可能获得焦点的元素。 此矩形仅用于计算，不会添加到可视化树中。
- [XYFocusNavigationStrategyOverride](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.findnextelementoptions#Windows_UI_Xaml_Input_FindNextElementOptions_XYFocusNavigationStrategyOverride) - 使用焦点导航策略标识将获得焦点的最佳候选元素。

下图展示了其中的一些概念。 

如果元素 B 具有焦点，则在向右导航时，FindNextElement 会将 I 标识为候选焦点。 这样做的原因是：
- 由于 A 上的 [HintRect](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.findnextelementoptions#Windows_UI_Xaml_Input_FindNextElementOptions_HintRect)，因此起始参考点是 A，不是 B
- C 不是候选项，因为已将 MyPanel 指定为 [SearchRoot](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.findnextelementoptions#Windows_UI_Xaml_Input_FindNextElementOptions_SearchRoot)
- F 不是候选项，因为 [ExclusionRect](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.findnextelementoptions#Windows_UI_Xaml_Input_FindNextElementOptions_ExclusionRect) 与其重叠

![使用导航提示的自定义焦点导航行为](images/keyboard/navigation-hints.png)

*使用导航提示的自定义焦点导航行为*

## <a name="navigation-focus-events"></a>导航焦点事件

### <a name="nofocuscandidatefound-event"></a>NoFocusCandidateFound 事件

在按下 Tab 键或箭头键而指定方向没有候选焦点时，将会触发 [UIElement.NoFocusCandidateFound](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement#Windows_UI_Xaml_UIElement_NoFocusCandidateFound) 事件。 不会对 [TryMoveFocus](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.focusmanager#Windows_UI_Xaml_Input_FocusManager_TryMoveFocus_Windows_UI_Xaml_Input_FocusNavigationDirection_) 触发此事件。

这是一个路由事件，因此，它将从连续父对象中的聚焦元素向上浮升到对象树的根。 这可让你在适当的位置处理该事件。

<a name="split-view-code-sample"></a>

此处，我们显示了在用户尝试向最左侧可聚焦控件的左侧移动焦点时，Grid 如何打开 [SplitView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.splitview)（请参阅[针对 Xbox 和电视进行设计](../devices/designing-for-tv.md#navigation-pane)）。

```xaml
<Grid NoFocusCandidateFound="OnNoFocusCandidateFound">
...
</Grid>
```

```csharp
private void OnNoFocusCandidateFound (
    UIElement sender, NoFocusCandidateFoundEventArgs args)
{
    if(args.NavigationDirection == FocusNavigationDirection.Left)
    {
        if(args.InputDevice == FocusInputDeviceKind.Keyboard ||
        args.InputDevice == FocusInputDeviceKind.GameController )
            {
                OpenSplitPaneView();
            }
        args.Handled = true;
    }
}
```

### <a name="gotfocus-and-lostfocus-events"></a>GotFocus 和 LostFocus 事件
元素在获得焦点或失去焦点时，将会分别触发 [UIElement.GotFocus](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.UIElement#Windows_UI_Xaml_UIElement_GotFocus) 和 [UIElement.LostFocus](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.UIElement#Windows_UI_Xaml_UIElement_LostFocus) 事件。 不会对 [TryMoveFocus](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.focusmanager#Windows_UI_Xaml_Input_FocusManager_TryMoveFocus_Windows_UI_Xaml_Input_FocusNavigationDirection_) 触发此事件。

这些是路由事件，因此，它们将从连续父对象中的聚焦元素向上浮升到对象树的根。 这可让你在适当的位置处理该事件。

### <a name="gettingfocus-and-losingfocus-events"></a>GettingFocus 和 LosingFocus 事件

[UIElement.GettingFocus](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.UIElement#Windows_UI_Xaml_UIElement_GettingFocus) 和 [UIElement.LosingFocus](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.UIElement#Windows_UI_Xaml_UIElement_LosingFocus) 事件在相应的 [UIElement.GotFocus](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.UIElement#Windows_UI_Xaml_UIElement_GotFocus) 和 [UIElement.LostFocus](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.UIElement#Windows_UI_Xaml_UIElement_LostFocus) 事件之前触发。 

这些是路由事件，因此，它们将从连续父对象中的聚焦元素向上浮升到对象树的根。 由于该事件在发生焦点更改之前发生，因此可以重定向或取消焦点更改。

[GettingFocus](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.UIElement#Windows_UI_Xaml_UIElement_GettingFocus) 和 [LosingFocus](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.UIElement#Windows_UI_Xaml_UIElement_LosingFocus) 是同步事件，因此，在这些事件浮升时，焦点不会移动。 但是，[GotFocus](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.UIElement#Windows_UI_Xaml_UIElement_GotFocus) 和 [LostFocus](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.UIElement#Windows_UI_Xaml_UIElement_LostFocus) 为异步事件，这意味着无法确保在执行处理程序之前焦点不会再次移动。

如果通过调用 [Control.Focus](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.control#Windows_UI_Xaml_Controls_Control_Focus_Windows_UI_Xaml_FocusState_) 移动了焦点，[GettingFocus](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.UIElement#Windows_UI_Xaml_UIElement_GettingFocus) 将在调用过程中提升，而 [GotFocus](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.UIElement#Windows_UI_Xaml_UIElement_GotFocus) 将在调用之后提升。

在 [GettingFocus](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.UIElement#Windows_UI_Xaml_UIElement_GettingFocus) 和 [LosingFocus](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.UIElement#Windows_UI_Xaml_UIElement_LosingFocus) 事件过程中（焦点移动之前），可以通过 [GettingFocusEventArgs.NewFocusedElement](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.gettingfocuseventargs#Windows_UI_Xaml_Input_GettingFocusEventArgs_NewFocusedElement) 属性更改焦点导航目标。 即便目标已更改，事件仍会浮升并且可以再次更改目标。

为了避免出现重新进入问题，如果你尝试在这些事件浮升时移动焦点（使用 [TryMoveFocus](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.focusmanager#Windows_UI_Xaml_Input_FocusManager_TryMoveFocus_Windows_UI_Xaml_Input_FocusNavigationDirection_) 或 [Control.Focus](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.control#Windows_UI_Xaml_Controls_Control_Focus_Windows_UI_Xaml_FocusState_)），则会引发异常。

无论焦点移动的原因为何（包括 Tab 键导航、定向导航和编程导航），均会触发这些事件。

下面是焦点事件的执行顺序：

1.  [LosingFocus](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.UIElement#Windows_UI_Xaml_UIElement_LosingFocus) 如果焦点重置回失去焦点的元素或 [TryCancel](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.losingfocuseventargs#Windows_UI_Xaml_Input_LosingFocusEventArgs_TryCancel) 成功，不触发其他事件。
2.  [GettingFocus](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.UIElement#Windows_UI_Xaml_UIElement_GettingFocus) 如果焦点重置回失去焦点的元素或 [TryCancel](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.gettingfocuseventargs#Windows_UI_Xaml_Input_GettingFocusEventArgs_TryCancel) 成功，不触发其他事件。
3.  [LostFocus](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.UIElement#Windows_UI_Xaml_UIElement_LostFocus)
4.  [GotFocus](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.UIElement#Windows_UI_Xaml_UIElement_GotFocus)

下图显示了在从 A 向右侧移动时，XYFocus 如何将 B4 选为候选焦点。 B4 随后会触发 GettingFocus 事件，其中 ListView 有机会为 B3 重新分配焦点。

![更改 GettingFocus 事件的焦点导航目标](images/keyboard/focus-events.png)

*更改 GettingFocus 事件的焦点导航目标*

此处，我们显示了如何处理 [GettingFocus](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.UIElement#Windows_UI_Xaml_UIElement_GettingFocus) 事件并重定向焦点。

```XAML
<StackPanel Orientation="Horizontal">
    <Button Content="A" />
    <ListView x:Name="MyListView" SelectedIndex="2" GettingFocus="OnGettingFocus">
        <ListViewItem>LV1</ListViewItem>
        <ListViewItem>LV2</ListViewItem>
        <ListViewItem>LV3</ListViewItem>
        <ListViewItem>LV4</ListViewItem>
        <ListViewItem>LV5</ListViewItem>
    </ListView>
</StackPanel>
```

```csharp
private void OnGettingFocus(UIElement sender, GettingFocusEventArgs args)
{
    //Redirect the focus only when the focus comes from outside of the ListView.
    // move the focus to the selected item
    if (MyListView.SelectedIndex != -1 && 
        IsNotAChildOf(MyListView, args.OldFocusedElement))
    {
        var selectedContainer = 
            MyListView.ContainerFromItem(MyListView.SelectedItem);
        if (args.FocusState == 
            FocusState.Keyboard && 
            args.NewFocusedElement != selectedContainer)
        {
            args.TryRedirect(
                MyListView.ContainerFromItem(MyListView.SelectedItem));
            args.Handled = true;
        }
    }
}
```

此处，我们显示了菜单关闭时如何处理 [CommandBar](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.commandbar) 的 [LosingFocus](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.UIElement#Windows_UI_Xaml_UIElement_LosingFocus) 事件并设置焦点。

```XAML
<CommandBar x:Name="MyCommandBar" LosingFocus="OnLosingFocus">
     <AppBarButton Icon="Back" Label="Back" />
     <AppBarButton Icon="Stop" Label="Stop" />
     <AppBarButton Icon="Play" Label="Play" />
     <AppBarButton Icon="Forward" Label="Forward" />

     <CommandBar.SecondaryCommands>
         <AppBarButton Icon="Like" Label="Like" />
         <AppBarButton Icon="Share" Label="Share" />
     </CommandBar.SecondaryCommands>
 </CommandBar>
```

```csharp
private void OnLosingFocus(UIElement sender, LosingFocusEventArgs args)
{
    if (MyCommandBar.IsOpen == true && 
        IsNotAChildOf(MyCommandBar, args.NewFocusedElement))
    {
        if (args.TryCancel())
        {
            args.Handled = true;
        }
    }
}
```

## <a name="find-the-first-and-last-focusable-element"></a>查找第一个和最后一个可聚焦的元素

[FocusManager.FindFirstFocusableElement](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.focusmanager#Windows_UI_Xaml_Input_FocusManager_FindFirstFocusableElement_Windows_UI_Xaml_DependencyObject_) 和 [FocusManager.FindLastFocusableElement](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.focusmanager#Windows_UI_Xaml_Input_FocusManager_FindLastFocusableElement_Windows_UI_Xaml_DependencyObject_) 方法将焦点移至对象范围内（[UIElement](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement) 的元素树或 [TextElement](https://docs.microsoft.com/uwp/api/windows.ui.xaml.documents.textelement) 的文本树）的第一个或最后一个可聚焦元素。 范围在调用中指定（如果参数为 null，则范围为当前窗口）。

如果在范围内无法标识候选焦点，则返回 null。

此处，我们显示了如何指定 CommandBar 的按钮具有环绕式定向行为（请参阅[键盘交互](keyboard-interactions.md#popup-ui)）。

```XAML
<CommandBar x:Name="MyCommandBar" LosingFocus="OnLosingFocus">
    <AppBarButton Icon="Back" Label="Back" />
    <AppBarButton Icon="Stop" Label="Stop" />
    <AppBarButton Icon="Play" Label="Play" />
    <AppBarButton Icon="Forward" Label="Forward" />

    <CommandBar.SecondaryCommands>
        <AppBarButton Icon="Like" Label="Like" />
        <AppBarButton Icon="ReShare" Label="Share" />
    </CommandBar.SecondaryCommands>
</CommandBar>
```

```csharp
private void OnLosingFocus(UIElement sender, LosingFocusEventArgs args)
{
    if (IsNotAChildOf(MyCommandBar, args.NewFocussedElement))
    {
        DependencyObject candidate = null;
        if (args.Direction == FocusNavigationDirection.Left)
        {
            candidate = FocusManager.FindLastFocusableElement(MyCommandBar);
        }
        else if (args.Direction == FocusNavigationDirection.Right)
        {
            candidate = FocusManager.FindFirstFocusableElement(MyCommandBar);
        }
        if (candidate != null)
        {
            args.NewFocusedElement = candidate;
            args.Handled = true;
        }
    }
}
```

## <a name="related-articles"></a>相关文章

- [适用于键盘、手柄、遥控器和辅助功能工具的焦点导航](focus-navigation.md)
- [键盘交互](keyboard-interactions.md)
- [键盘辅助功能](../accessibility/keyboard-accessibility.md)
