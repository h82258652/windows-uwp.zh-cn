---
author: Karl-Bridge-Microsoft
pm-contact: miguelrb
design-contact: kimsea
dev-contact: niallm
doc-status: Published
ms.openlocfilehash: 8389d1f089d507a087693879a793b95572d7860b
ms.sourcegitcommit: 5ece992c31870df4c089360ef47501bd4ce14fa9
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/22/2017
---
# <a name="managing-focus-navigation"></a>管理焦点导航

诸如键盘、辅助功能工具（如 Windows 讲述人）、游戏板和遥控器之类的许多输入使用共同的基础机制来在应用程序 UI 内移动焦点视觉。 阅读[键盘交互](keyboard-interactions.md)文档和[针对 Xbox 和电视进行设计](designing-for-tv.md#xy-focus-navigation-and-interaction)文档，了解与焦点视觉和导航相关的更多信息。

以下内容为应用提供了一种与输入无关的方式来在应用程序 UI 中移动焦点，使你能够写入可让应用程序适合于多种输入类型的单个代码。

## <a name="navigation-strategies-properties-to-fine-tune-focus-movements"></a>用于微调焦点移动的导航策略属性

使用 XYFocus 导航策略属性来指定哪些控件应根据按下的箭头键接收焦点。 这些属性包括：

-   XYFocusUpNavigationStrategy
-   XYFocusDownNavigationStrategy
-   XYFocusLeftNavigationStrategy
-   XYFocusRightNavigationStrategy

这些属性具有 **XYFocusNavigationStrategy** 类型值。 如果设为 **Auto**，则元素的行为基于元素的祖先。 如果所有元素均设为 **Auto**，则将使用 **Projection**。

这些导航策略适用于键盘、游戏板和遥控器。

### <a name="projection"></a>Projection

Projection 策略会将焦点移至在导航方向上投影当前聚焦元素的边缘时所遇到的第一个元素。

> [!NOTE]
> 诸如先前聚焦的元素以及靠近导航方向轴之类的其他因素可能会影响结果。

![projection](images/keyboard/projection.png)

*焦点根据 A 的底边投影在向下导航时从 A 移到 D*

### <a name="navigationdirectiondistance"></a>NavigationDirectionDistance

NavigationDirectionDistance 策略会将焦点移至最靠近导航方向坐标轴的元素。

与导航方向相对应的边界矩形的边缘会被扩展和投影以标识候选目标。 遇到的第一个元素被标识为目标。 在有多个候选项的情况下，最近的元素被标识为目标。 如果仍有多个候选项，则最顶层/最左端的元素被标识为候选项。

![导航方向距离](images/keyboard/navigation-direction-distance.png)

*焦点在向下导航时从 A 移到 C，然后从 C 移到 B*


### <a name="rectilineardistance"></a>RectilinearDistance

RectilinearDistance 策略会根据最短的 2D 距离（曼哈顿距离）将焦点移至最近的元素。 此距离通过为每个潜在候选焦点添加主距离和辅助距离来计算。 因此，如果方向是向上或向下，则会选择左侧的第一个元素；如果方向是向左或向右，则会选择顶部的第一个元素。

在下图中，我们展示了当 A 具有焦点时，焦点将移至 B，因为：

-   距离（A - B，向下）= 10 + 0 = 10
-   距离（A - C，向下）= 0 + 30 = 30
-   距离（A - D，向下）= 30 + 0 = 30

![直线距离](images/keyboard/rectilinear-distance.png)

*当 A 具有焦点时，如果使用 RectilinearDistance 策略，则焦点将移至 B*

下图展示了如何将导航策略应用到元素本身，而不是子元素（与 XYFocusKeyboardNavigation 不同）。

例如，当 E 具有焦点时，如果使用 RectilinearDistance 策略，则按向右箭头会将焦点移至 F，但是，如果使用的是 Projection 策略，则按向右箭头则会将焦点移至 H。

请注意，不会应用主容器策略（导航方向距离）。 仅当焦点位于 H、F 或 G 时才会应用它。

![不同的导航策略](images/keyboard/different-navigation-strategies.png)

*基于所应用的导航策略的不同焦点行为*

以下示例模拟 Windows 10“开始”菜单中“磁贴”窗格的行为。 在此案例中，按向上和向下箭头将使用 NavigationDirectionDistance 策略，按向左和向右箭头将使用 Projection 策略。

```XAML
<start:TileGridView
        XYFocusKeyboardNavigation ="Default"
        XYFocusUpNavigationStrategy=" NavigationDirectionDistance "
        XYFocusDownNavigationStrategy=" NavigationDirectionDistance "
        XYFocusLeftNavigationStrategy="Projection"
        XYFocusRightNavigationStrategy="Projection"
/>
```

## <a name="move-focus-programmatically"></a>以编程方式移动焦点

使用 [FocusManager.TryMoveFocus](http://msdn.microsoft.com/en-us/library/windows/apps/xaml/Windows.UI.Xaml.Input.FocusManager.TryMoveFocus) 方法或 [FocusManager.FindNextElement](http://msdn.microsoft.com/en-us/library/windows/apps/xaml/Windows.UI.Xaml.Input.FocusManager.FindNextFocusableElement) 方法以编程方式移动焦点。

TryMoveFocus 将像按下导航键一样设置焦点。
FindNextElement 返回 TryMoveFocus 将移至的元素（DependencyObject）。

**注意** 我们建议使用 **FindNextElement** 方法，而不是 **FindNextFocusableElement**。 FindNextFocusableElement 将尝试返回 UIElement，但是，如果下一个可聚焦元素为超链接，则它将会返回 null，因为超链接并非 UIElement。 相反，FindNextElement 方法将返回 DependencyObject。

### <a name="find-a-focus-candidate-in-a-scope"></a>查找范围内的候选焦点

FocusManager.FindNextElement 和 FocusManager.TryMoveFocus 均可让你自定义下一个可聚焦元素的搜索行为。 你可以在特定 UI 树内进行搜索，或者排除特定导航区域内的元素。

本示例取自 TicTacToe 游戏的实施，它展示了如何使用 FindNextElement 和 TryMoveFocus 方法：

```XAML
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
            <myControls:TicTacToeCell Grid.Column="0" Grid.Row="0" x:Name="Cell00" />
            <myControls:TicTacToeCell Grid.Column="1" Grid.Row="0" x:Name="Cell10"/>
            <myControls:TicTacToeCell Grid.Column="2" Grid.Row="0" x:Name="Cell20"/>
            <myControls:TicTacToeCell Grid.Column="0" Grid.Row="1" x:Name="Cell01"/>
            <myControls:TicTacToeCell Grid.Column="1" Grid.Row="1" x:Name="Cell11"/>
            <myControls:TicTacToeCell Grid.Column="2" Grid.Row="1" x:Name="Cell21"/>
            <myControls:TicTacToeCell Grid.Column="0" Grid.Row="2" x:Name="Cell02"/>
            <myControls:TicTacToeCell Grid.Column="1" Grid.Row="2" x:Name="Cell22"/>
            <myControls:TicTacToeCell Grid.Column="2" Grid.Row="2" x:Name="Cell32"/>
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
                NavigationStrategy = NavigationStrategyMode.Heuristic
            };

            switch (e.Key)
            {
                case Windows.System.VirtualKey.Up:
                    candidate = FocusManager.FindNextElement(FocusNavigationDirection.Up, options);
                    break;
                case Windows.System.VirtualKey.Down:
                    candidate = FocusManager.FindNextElement(FocusNavigationDirection.Down, options);
                    break;
                case Windows.System.VirtualKey.Left:
                    candidate = FocusManager.FindNextElement(FocusNavigationDirection.Left, options);
                    break;
                case Windows.System.VirtualKey.Right:
                    candidate = FocusManager.FindNextElement(FocusNavigationDirection.Right, options);
                    break;
            }
         //Note you should also consider whether is a Hyperlink, WebView, or TextBlock.
            if (candidate != null && candidate is Control)
            {
                (candidate as Control).Focus(FocusState.Keyboard);
            }
        }
```

**注意** FocusNavigationDirection.Left 和 FocusNavigationDirection.Right 对于支持 RTL 方案来说是合理（近和远平面）的。 （这与其他 Xaml 相同。）

可以使用 FindNextElementOptions 类调整对候选焦点的搜索。 此对象具有以下属性：

-   **SearchRoot** DependencyObject：用于将候选焦点的搜索限制为此 DependencyObject 的子项。 Null 值标识应用的整个视觉树。
-   **ExclusionRect** Rect：用于从搜索中排除呈现时至少重叠此矩形中的一个像素的项目。
-   **HintRect** Rect：可以将聚焦的项目用作参考来计算潜在的候选焦点。 此矩形使开发人员可以指定其他引用，而不是已聚焦的元素。 此矩形是一个仅可用于计算的“虚构”矩形。 它永远不会转换成实际的矩形并添加到虚拟树。
-   **XYFocusNavigationStrategyOverride** XYFocusNavigationStrategyOverride：用于移动焦点的导航策略。 类型为枚举值，包括：None（值为零）、Auto、RectilinearDistance、NavigationDirectionDistance 和 Projection。
    **重要提示** **SearchRoot** 不用于计算呈现（几何）区域或获取区域内的候选焦点。 因此，对方向区域以外的 DependencyObject 后代应用转换时，这些元素仍被视为候选焦点。

FindNextElement 的选项重载无法与 Tab 键导航一起使用，它只支持方向导航。

下图展示了这些选项。

例如，当 B 具有焦点时，调用 FindNextElement（通过设置不同的选项来指示焦点移至右侧）会将焦点设置在 I 上。原因如下：

1.  引用不是 B，是因为 HintRect 才导致 A
2.  C 被排除，因为潜在候选焦点应位于 MyPanel (SearchRoot) 上
3.  F 被排除，因为它与排除矩形重叠

![导航提示](images/keyboard/navigation-hints.png)

*基于导航提示的焦点行为*

### <a name="no-focus-candidate-available"></a>无可用候选焦点

如果在用户尝试通过 Tab 或箭头键移动焦点时，指定方向内没有潜在的候选焦点，那么，将会触发 UIElement.NoFocusCandidateFound 事件。 FocusManager.TryMoveFocus() 不会触发此事件。

这是一个路由事件，因此，它将从连续父对象中的聚焦元素向上浮升到对象树的根。 这样一来，你就可以在适当的位置处理事件。

<a name="split-view-code-sample"></a>以下示例中所示的网格将在用户尝试将焦点移至最左侧可聚焦控件左边时打开一个拆分视图，如[针对电视文档进行设计](designing-for-tv.md#navigation-pane)中所述。

```XAML
<Grid NoFocusCandidateFound="OnNoFocusCandidateFound">  ...</Grid>
```

```csharp
private void OnNoFocusCandidateFound (UIElement sender, NoFocusCandidateFoundEventArgs args)
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

## <a name="focus-events"></a>焦点事件

控件在获得焦点或失去焦点时，将会分别触发 [UIElement.GotFocus](http://msdn.microsoft.com/en-us/library/windows/apps/xaml/Windows.UI.Xaml.UIElement.GotFocus) 和 UIElement.LostFocus 事件。 FocusManager.TryMoveFocus() 不会触发此事件。

这是一个路由事件，因此，它将从连续父对象中的聚焦元素向上浮升到对象树的根。 这样一来，你就可以在适当的位置处理事件。

**UIElement.GettingFocus** 和 **UIElement.LosingFocus** 事件也会从控件浮升，但在这发生*之前*，焦点将会发生更改。 这使应用有机会重定向或取消焦点更改。

请注意，GettingFocus 和 LosingFocus 事件是同步的，但 GotFocus 和 LostFocus 事件则是异步的。 例如，如果焦点因应用调用 Control.Focus() 方法移动，则 GettingFocus 将在调用过程中提升，但 GotFocus 有时要在调用之后才会提升。

UIElements 可以在焦点移动之前关联 GettingFocus 和 LosingFocus 事件并更改目标（使用 NewFocusedElement 属性）。 即便目标已更改，事件仍会浮升并且其他父项仍可再次更改目标。

为了避免出现重新进入问题，如果你尝试在这些事件浮升时移动焦点（使用 FocusManager.TryMoveFocus 或 Control.Focus），则会引发异常。

无论焦点移动的原因为何（例如，Tab 键导航、XYFocus 导航、编程方式），均会触发这些事件。

下图展示了 XYFocus 如何将 LVI4 选为候选焦点（从 A 移至右侧）。 LVI4 随后会触发 GettingFocus 事件，其中 ListView 有机会为 LVI3 重新分配焦点。

![焦点事件](images/keyboard/focus-events.png)

*焦点事件*

此代码示例展示了如何处理 OnGettingFocus 事件并根据先前设置的焦点位置重定向焦点。

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
            if (MyListView.SelectedIndex != -1 && IsNotAChildOf(MyListView, args.OldFocusedElement))
            {
                var selectedContainer = MyListView.ContainerFromItem(MyListView.SelectedItem);
                if (args.FocusState == FocusState.Keyboard && args.NewFocusedElement != selectedContainer)
                {
                    args.NewFocusedElement = MyListView.ContainerFromItem(MyListView.SelectedItem);
                    args.Handled = true;
                }
            }        
  }
```

此代码示例展示了如何为 CommandBar 对象处理 OnLosingFocus 事件并等待设置焦点，直到下拉菜单关闭。

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
          if (MyCommandBar.IsOpen == true && IsNotAChildOf(MyCommandBar, args.NewFocusedElement))
          {
              args.Cancel = true;
              args.Handled = true;
          }
}
```

### <a name="losingfocus-and-gettingfocus-event-order"></a>LosingFocus 和 GettingFocus 事件顺序

LosingFocus 和 GettingFocus 是同步事件，因此，在这些事件浮升时，焦点不会移动。 但是，LostFocus 和 GotFocus 为异步事件，因此，无法确保在执行处理程序之前焦点不会再次移动。 唯一可保证的是会先执行 LostFocus 处理程序，然后再执行 GotFocus 处理程序。

以下是执行顺序：

1.  同步 LosingFocus：如果 LosingFocus 将焦点重置回失去焦点的元素或设置 Cancel=true，则无进一步事件
2.  同步 GettingFocus：如果 GettingFocus 将焦点重置回失去焦点的元素或设置 Cancel=true，则无进一步事件
3.  异步 LostFocus
4.  异步 GotFocus

## <a name="find-the-first-and-last-focusable-element-a-namefindfirstfocusableelement"></a>查找第一个和最后一个可聚焦的元素 <a name="findfirstfocusableelement">

**FocusManager.FindFirstFocusableElement** 和 **FocusManager.FindLastFocusableElement** 方法使你能够将焦点移至对象范围内（UIElement 的元素树或 TextElement 的文本树）的第一个或最后一个可聚焦元素。 在调用这些方法时，你可以指定范围。 如果参数为 null，则范围为当前的窗口。

**注意** 如果范围内没有可聚焦的项目，则这些方法可能会返回 null。

<a name="popup-ui-code-sample"></a>以下代码示例展示了如何指定 CommandBar 按钮具有[键盘交互](keyboard-interactions.md#popup-ui)文章中所述的环绕方向行为。

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
