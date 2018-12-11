---
Description: Respond to keystroke actions from hardware or software keyboards in your apps using both keyboard and class event handlers.
title: 键盘事件
ms.assetid: ac500772-d6ed-4a3a-825b-210a9c3c8f59
label: Keyboard events
template: detail.hbs
keywords: 键盘, 游戏板, 远程, 辅助功能, 导航, 焦点, 文本, 输入, 用户交互, 键放开, 键按下
ms.date: 03/29/2017
ms.topic: article
pm-contact: chigy
design-contact: kimsea
dev-contact: niallm
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: 1e7453d3973cef31ae8143f3ecff31fffeb763a3
ms.sourcegitcommit: 8921a9cc0dd3e5665345ae8eca7ab7aeb83ccc6f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/11/2018
ms.locfileid: "8886452"
---
# <a name="keyboard-events"></a>键盘事件

## <a name="keyboard-events-and-focus"></a>键盘事件和焦点

以下键盘事件仅针对硬件和触摸键盘发生。

| 事件                                      | 描述                    |
|--------------------------------------------|--------------------------------|
| [**KeyDown**](https://msdn.microsoft.com/library/windows/apps/br208941) | 按下某个键时发生。  |
| [**KeyUp**](https://msdn.microsoft.com/library/windows/apps/br208942)     | 释放某个键时发生。 |

> [!IMPORTANT]
> 某些 Windows 运行时控件在内部处理输入事件。 在这些情况下，输入事件好像没有发生，因为你的事件侦听器没有调用相关联的处理程序。 通常，类处理程序将处理这些键的子集以提供对基本键盘辅助功能的内置支持。 例如，[**Button**](https://msdn.microsoft.com/library/windows/apps/br209265) 类将替代空格键和 Enter 键的 [**OnKeyDown**](https://msdn.microsoft.com/library/windows/apps/hh967982) 事件（[**OnPointerPressed**](https://msdn.microsoft.com/library/windows/apps/hh967989) 也是如此），并将其路由到控件的 [**Click**](https://msdn.microsoft.com/library/windows/apps/br227737) 事件。 当控件类处理按键时，不会引发 [**KeyDown**](https://msdn.microsoft.com/library/windows/apps/br208941) 和 [**KeyUp**](https://msdn.microsoft.com/library/windows/apps/br208942) 事件。  
> 这提供了与调用按钮等效的内置键盘，效果与使用手指点击或使用鼠标单击相似。 空格键或 Enter 键之外的键仍会引发 [**KeyDown**](https://msdn.microsoft.com/library/windows/apps/br208941) 和 [**KeyUp**](https://msdn.microsoft.com/library/windows/apps/br208942) 事件。 有关基于类处理事件的工作原理的详细信息（特别是“控件中的输入事件处理程序”部分），请参阅[事件和路由事件概述](https://msdn.microsoft.com/library/windows/apps/mt185584)。


UI 中的控件仅在具有输入焦点时才会生成键盘事件。 当用户直接单击或点击布局中的一个控件时该控件获得焦点，或者使用 Tab 键进入内容区域内的 Tab 序列。

也可以调用控件的 [**Focus**](https://msdn.microsoft.com/library/windows/apps/hh702161) 方法来强制使用焦点。 当实现快捷键时需要此操作，因为默认情况下在 UI 加载时不设置键盘焦点。 有关详细信息，请参阅本主题后面部分的**快捷键示例**。

若要使某个控件接收输入焦点，则必须启用该控件，该控件必须可见并且 [**IsTabStop**](https://msdn.microsoft.com/library/windows/apps/br209422) 和 [**HitTestVisible**](https://msdn.microsoft.com/library/windows/apps/br208933) 的属性值必须为 **true**。 这是大多数控件的默认状态。 当某个控件具有输入焦点时，该控件可能引发并响应键盘输入事件，如本主题后面部分所述。 你也可以通过处理 [**GotFocus**](https://msdn.microsoft.com/library/windows/apps/br208927) 和 [**LostFocus**](https://msdn.microsoft.com/library/windows/apps/br208943) 事件来响应接收或丢失焦点的控件。

默认情况下，控件的 Tab 键顺序即为它们在 Extensible Application Markup Language (XAML) 中的显示顺序。 但是，可以使用 [**TabIndex**](https://msdn.microsoft.com/library/windows/apps/br209461) 属性修改此顺序。 有关详细信息，请参阅[实现键盘辅助功能](https://msdn.microsoft.com/library/windows/apps/hh868161)。

## <a name="keyboard-event-handlers"></a>键盘事件处理程序


输入事件处理程序实现提供以下信息的委派：

-   事件的发送者。 发送者报告附加事件处理程序的对象。
-   事件数据。 对于键盘事件，该数据将是 [**KeyRoutedEventArgs**](https://msdn.microsoft.com/library/windows/apps/hh943072) 的一个实例。 处理程序的委托为 [**KeyEventHandler**](https://msdn.microsoft.com/library/windows/apps/br227904)。 对于大多数处理程序方案而言，**KeyRoutedEventArgs** 的最为相关的属性是 [**Key**](https://msdn.microsoft.com/library/windows/apps/hh943074)，并且可能为 [**KeyStatus**](https://msdn.microsoft.com/library/windows/apps/hh943075)。
-   [**OriginalSource**](https://msdn.microsoft.com/library/windows/apps/br208810)。 由于键盘事件是路由事件，因此事件数据提供 **OriginalSource**。 如果有意允许事件通过对象树向上浮生，则 **OriginalSource** 有时是所涉及的对象而不是发送者。 但是，这取决于你的设计。 有关如何使用 **OriginalSource** 而不是发送者的详细信息，请参阅本主题中的“键盘路由事件”部分或[事件和路由事件概述](https://msdn.microsoft.com/library/windows/apps/mt185584)。

### <a name="attaching-a-keyboard-event-handler"></a>附加键盘事件处理程序

可以为将事件作为成员包含的任何对象附加键盘事件处理程序功能。 这包括任何 [**UIElement**](https://msdn.microsoft.com/library/windows/apps/br208911) 派生类。 以下 XAML 示例显示了如何为 [**Grid**](https://msdn.microsoft.com/library/windows/apps/br208942) 的 [**KeyUp**](https://msdn.microsoft.com/library/windows/apps/br242704) 事件附加处理程序。

```xaml
<Grid KeyUp="Grid_KeyUp">
  ...
</Grid>
```

也可以在代码中附加事件处理程序。 有关详细信息，请参阅[事件和路由事件概述](https://msdn.microsoft.com/library/windows/apps/mt185584)。

### <a name="defining-a-keyboard-event-handler"></a>定义键盘事件处理程序

以下示例显示了在上一示例中附加的 [**KeyUp**](https://msdn.microsoft.com/library/windows/apps/br208942) 事件处理程序的不完整事件处理程序定义。

```csharp
void Grid_KeyUp(object sender, KeyRoutedEventArgs e)
{
    //handling code here
}
```

```vb
Private Sub Grid_KeyUp(ByVal sender As Object, ByVal e As KeyRoutedEventArgs)
    ' handling code here
End Sub
```

```c++
void MyProject::MainPage::Grid_KeyUp(
  Platform::Object^ sender,
  Windows::UI::Xaml::Input::KeyRoutedEventArgs^ e)
  {
      //handling code here
  }
```

### <a name="using-keyroutedeventargs"></a>使用 KeyRoutedEventArgs

所有键盘事件对事件数据均使用 [**KeyRoutedEventArgs**](https://msdn.microsoft.com/library/windows/apps/hh943072)，而且 **KeyRoutedEventArgs** 包含以下属性：

-   [**Key**](https://msdn.microsoft.com/library/windows/apps/hh943074)
-   [**KeyStatus**](https://msdn.microsoft.com/library/windows/apps/hh943075)
-   [**Handled**](https://msdn.microsoft.com/library/windows/apps/hh943073)
-   [**OriginalSource**](https://msdn.microsoft.com/library/windows/apps/br208810)（继承自 [**RoutedEventArgs**](https://msdn.microsoft.com/library/windows/apps/br208809)）

### <a name="key"></a>键

如果按下某个键，则引发 [**KeyDown**](https://msdn.microsoft.com/library/windows/apps/br208941) 事件。 同样，如果释放某个键，则引发 [**KeyUp**](https://msdn.microsoft.com/library/windows/apps/br208942)。 通常会侦听这些事件以处理特定键值。 若要确定按下或释放了哪个键，请检查事件数据中的 [**Key**](https://msdn.microsoft.com/library/windows/apps/hh943074) 值。 **Key** 返回 [**VirtualKey**](https://msdn.microsoft.com/library/windows/apps/br241812) 值。 
            **VirtualKey** 枚举包括所有受支持的键。

### <a name="modifier-keys"></a>修改键

修改键是用户通常与其他键组合而按下的键，如 Ctrl 或 Shift。 你的应用可以使用这些组合作为键盘快捷方式来调用应用命令。

使用 [**KeyDown**](https://msdn.microsoft.com/library/windows/apps/br208941) 和 [**KeyUp**](https://msdn.microsoft.com/library/windows/apps/br208942) 事件处理程序中的代码检测快捷组合键。 然后，你可以跟踪感兴趣的修改键的按下状态。 当非修改键发生键盘事件时，可以同时检查修改键是否处于按下状态。

> [!NOTE]
> Alt 键由 **VirtualKey.Menu** 值表示。

 

### <a name="shortcut-keys-example"></a>快捷键示例


以下示例演示如何实现快捷键。 在此示例中，用户可以使用 Play、Pause 和 Stop 按钮或 Ctrl+P、Ctrl+A 和 Ctrl+S 键盘快捷键来控制媒体播放。 按钮 XAML 通过使用工具提示和按钮标签中的 [**AutomationProperties**](https://msdn.microsoft.com/library/windows/apps/br209081) 属性来显示快捷键。 此自述文档对于增加应用的实用功能和辅助功能非常重要。 有关详细信息，请参阅[键盘辅助功能](https://msdn.microsoft.com/library/windows/apps/mt244347)。

另请注意，加载页面时页面将输入焦点设置为其自身。 如果没有此步骤，则不会有控件获得初始输入焦点，并且在用户手动设置输入焦点之前，应用不会引发输入事件（例如，通过 Tab 浏览或单击某个控件）。

```xaml
<Grid KeyDown="Grid_KeyDown">

  <Grid.RowDefinitions>
    <RowDefinition Height="Auto" />
    <RowDefinition Height="Auto" />
  </Grid.RowDefinitions>

  <MediaElement x:Name="DemoMovie" Source="xbox.wmv"
    Width="500" Height="500" Margin="20" HorizontalAlignment="Center" />

  <StackPanel Grid.Row="1" Margin="10"
    Orientation="Horizontal" HorizontalAlignment="Center">

    <Button x:Name="PlayButton" Click="MediaButton_Click"
      ToolTipService.ToolTip="Shortcut key: Ctrl+P"
      AutomationProperties.AcceleratorKey="Control P">
      <TextBlock>Play</TextBlock>
    </Button>

    <Button x:Name="PauseButton" Click="MediaButton_Click"
      ToolTipService.ToolTip="Shortcut key: Ctrl+A"
      AutomationProperties.AcceleratorKey="Control A">
      <TextBlock>Pause</TextBlock>
    </Button>

    <Button x:Name="StopButton" Click="MediaButton_Click"
      ToolTipService.ToolTip="Shortcut key: Ctrl+S"
      AutomationProperties.AcceleratorKey="Control S">
      <TextBlock>Stop</TextBlock>
    </Button>

  </StackPanel>

</Grid>
```

```c++
//showing implementations but not header definitions
void MainPage::OnNavigatedTo(NavigationEventArgs^ e)
{
    (void) e;    // Unused parameter
    this->Loaded+=ref new RoutedEventHandler(this,&amp;MainPage::ProgrammaticFocus);
}
void MainPage::ProgrammaticFocus(Object^ sender, RoutedEventArgs^ e) {
    this->Focus(Windows::UI::Xaml::FocusState::Programmatic);
}

void KeyboardSupport::MainPage::MediaButton_Click(Platform::Object^ sender, Windows::UI::Xaml::RoutedEventArgs^ e)
{
    FrameworkElement^ fe = safe_cast<FrameworkElement^>(sender);
    if (fe->Name == "PlayButton") {DemoMovie->Play();}
    if (fe->Name == "PauseButton") {DemoMovie->Pause();}
    if (fe->Name == "StopButton") {DemoMovie->Stop();}
}


bool KeyboardSupport::MainPage::IsCtrlKeyPressed()
{
    auto ctrlState = CoreWindow::GetForCurrentThread()->GetKeyState(VirtualKey::Control);
    return (ctrlState & CoreVirtualKeyStates::Down) == CoreVirtualKeyStates::Down;
}

void KeyboardSupport::MainPage::Grid_KeyDown(Platform::Object^ sender, Windows::UI::Xaml::Input::KeyRoutedEventArgs^ e)
{
    if (e->Key == VirtualKey::Control) isCtrlKeyPressed = true;
}


void KeyboardSupport::MainPage::Grid_KeyUp(Platform::Object^ sender, Windows::UI::Xaml::Input::KeyRoutedEventArgs^ e)
{
    if (IsCtrlKeyPressed()) {
        if (e->Key==VirtualKey::P) { DemoMovie->Play(); }
        if (e->Key==VirtualKey::A) { DemoMovie->Pause(); }
        if (e->Key==VirtualKey::S) { DemoMovie->Stop(); }
    }
}
```

```csharp
protected override void OnNavigatedTo(NavigationEventArgs e)
{
    // Set the input focus to ensure that keyboard events are raised.
    this.Loaded += delegate { this.Focus(FocusState.Programmatic); };
}

private void MediaButton_Click(object sender, RoutedEventArgs e)
{
    switch ((sender as Button).Name)
    {
        case "PlayButton": DemoMovie.Play(); break;
        case "PauseButton": DemoMovie.Pause(); break;
        case "StopButton": DemoMovie.Stop(); break;
    }
}

private static bool IsCtrlKeyPressed()
{
    var ctrlState = CoreWindow.GetForCurrentThread().GetKeyState(VirtualKey.Control);
    return (ctrlState & CoreVirtualKeyStates.Down) == CoreVirtualKeyStates.Down;
}

private void Grid_KeyDown(object sender, KeyRoutedEventArgs e)
{
    if (IsCtrlKeyPressed())
    {
        switch (e.Key)
        {
            case VirtualKey.P: DemoMovie.Play(); break;
            case VirtualKey.A: DemoMovie.Pause(); break;
            case VirtualKey.S: DemoMovie.Stop(); break;
        }
    }
}
```

```VisualBasic
Private isCtrlKeyPressed As Boolean
Protected Overrides Sub OnNavigatedTo(e As Navigation.NavigationEventArgs)

End Sub

Private Function IsCtrlKeyPressed As Boolean
    Dim ctrlState As CoreVirtualKeyStates = CoreWindow.GetForCurrentThread().GetKeyState(VirtualKey.Control);
    Return (ctrlState & CoreVirtualKeyStates.Down) == CoreVirtualKeyStates.Down;
End Function

Private Sub Grid_KeyDown(sender As Object, e As KeyRoutedEventArgs)
    If IsCtrlKeyPressed() Then
        Select Case e.Key
            Case Windows.System.VirtualKey.P
                DemoMovie.Play()
            Case Windows.System.VirtualKey.A
                DemoMovie.Pause()
            Case Windows.System.VirtualKey.S
                DemoMovie.Stop()
        End Select
    End If
End Sub

Private Sub MediaButton_Click(sender As Object, e As RoutedEventArgs)
    Dim fe As FrameworkElement = CType(sender, FrameworkElement)
    Select Case fe.Name
        Case "PlayButton"
            DemoMovie.Play()
        Case "PauseButton"
            DemoMovie.Pause()
        Case "StopButton"
            DemoMovie.Stop()
    End Select
End Sub
```

> [!NOTE]
> 在 XAML 中设置 [**AutomationProperties.AcceleratorKey**](https://msdn.microsoft.com/library/windows/apps/hh759762) 或 [**AutomationProperties.AccessKey**](https://msdn.microsoft.com/library/windows/apps/hh759763) 会提供字符串信息，这可记录用于调用该特定操作的快捷键。 该信息由 Microsoft UI 自动化客户端（如讲述人）捕获，并且通常直接提供给用户。
>
> 设置 **AutomationProperties.AcceleratorKey** 或 **AutomationProperties.AccessKey** 不会自行执行任何操作。 你仍需要附加 [**KeyDown**](https://msdn.microsoft.com/library/windows/apps/br208941) 或 [**KeyUp**](https://msdn.microsoft.com/library/windows/apps/br208942) 事件的处理程序，才能在你的应用中真正实现键盘快捷方式行为。 此外，不会自动为访问键提供带下划线的文本效果。 如果你希望在 UI 中显示带下划线的文本，则必须明确对助记键中特定键的文本标注下划线，作为嵌入式 [**Underline**](https://msdn.microsoft.com/library/windows/apps/br209982) 格式。

 

## <a name="keyboard-routed-events"></a>键盘路由事件


有些事件为路由事件，这些事件包括 [**KeyDown**](https://msdn.microsoft.com/library/windows/apps/br208941) 和 [**KeyUp**](https://msdn.microsoft.com/library/windows/apps/br208942)。 路由事件使用浮升路由策略。 浮升路由策略意味着某个事件从子对象开始，然后向上路由到对象树中的连续父对象。 这样便提供了处理相同事件以及与相同数据数据交互的另一个机会。

请考虑以下 XAML 示例，该示例处理一个 [**Canvas**](https://msdn.microsoft.com/library/windows/apps/br208942) 和两个 [**Button**](https://msdn.microsoft.com/library/windows/apps/br209267) 对象的 [**KeyUp**](https://msdn.microsoft.com/library/windows/apps/br209265) 事件。 在这种情况下，如果在任一 **Button** 对象具有焦点的同时释放键，则会引发 **KeyUp** 事件。 然后，该事件向上浮升到父 **Canvas**。

```xaml
<StackPanel KeyUp="StackPanel_KeyUp">
  <Button Name="ButtonA" Content="Button A"/>
  <Button Name="ButtonB" Content="Button B"/>
  <TextBlock Name="statusTextBlock"/>
</StackPanel>
```

以下示例显示了如何为前面示例中对应的 XAML 内容实现 [**KeyUp**](https://msdn.microsoft.com/library/windows/apps/br208942) 事件处理程序。

```csharp
void StackPanel_KeyUp(object sender, KeyRoutedEventArgs e)
{
    statusTextBlock.Text = String.Format(
        "The key {0} was pressed while focus was on {1}",
        e.Key.ToString(), (e.OriginalSource as FrameworkElement).Name);
}
```

请注意前面的处理程序中的 [**OriginalSource**](https://msdn.microsoft.com/library/windows/apps/br208810) 属性的用法。 在此处，**OriginalSource** 报告引发此事件的对象。 该对象不可能是 [**StackPanel**](https://msdn.microsoft.com/library/windows/apps/br209635)，因为 **StackPanel** 不是控件，无法具有焦点。 只有 **StackPanel** 中的两个按钮之一可能引发此事件，但是是哪一个按钮呢？ 如果在父对象上处理此事件，可使用 **OriginalSource** 来辨别实际的事件源对象。

### <a name="the-handled-property-in-event-data"></a>事件数据中的 Handled 属性

根据你的事件处理策略，你可能希望只有一个事件处理程序对浮升事件进行响应。 例如，如果已将特定的 [**KeyUp**](https://msdn.microsoft.com/library/windows/apps/br208942) 处理程序附加到其中一个 [**Button**](https://msdn.microsoft.com/library/windows/apps/br209265) 控件，则这是处理该事件的第一次机会。 在这种情况下，你可能不希望父面板也处理该事件。 对于此方案，可以使用事件数据中的 [**Handled**](https://msdn.microsoft.com/library/windows/apps/hh943073) 属性。

路由事件数据类中的 [**Handled**](https://msdn.microsoft.com/library/windows/apps/hh943073) 属性用于报告之前在事件路由上注册的另一个处理程序已进行操作。 这会影响路由事件系统的行为。 在事件处理程序中将 **Handled** 设置为 **true** 时，该事件停止路由，但不会发送到连续的父元素。

### <a name="addhandler-and-already-handled-keyboard-events"></a>AddHandler 和 already-handled 键盘事件

可以使用特殊技术来附加处理程序，该技术对已标记为已处理的事件进行操作。 此技术使用 [**AddHandler**](https://msdn.microsoft.com/library/windows/apps/hh702399) 方法注册处理程序，而不是使用 XAML 属性或特定于语言的用于添加处理程序的语法，如 C# 中的“+=”。

此技术的一般限制是 **AddHandler** API 带有一个类型为 [**RoutedEvent**](https://msdn.microsoft.com/library/windows/apps/br208808) 的参数，该参数标识相关的路由事件。 并非所有路由事件都提供 **RoutedEvent** 标识符，因此此注意事项会影响在 [**Handled**](https://msdn.microsoft.com/library/windows/apps/hh943073) 情况下仍然可以处理的路由事件。 
            [
              **KeyDown**
            ](https://msdn.microsoft.com/library/windows/apps/br208941) 和 [**KeyUp**](https://msdn.microsoft.com/library/windows/apps/br208942) 事件在 [**UIElement**](https://msdn.microsoft.com/library/windows/apps/hh702416) 上具有路由事件标识符（[**KeyDownEvent**](https://msdn.microsoft.com/library/windows/apps/hh702418) 和 [**KeyUpEvent**](https://msdn.microsoft.com/library/windows/apps/br208911)）。 但是，其他事件（如 [**TextBox.TextChanged**](https://msdn.microsoft.com/library/windows/apps/br209706)）没有路由事件标识符，因此不能使用 **AddHandler** 技术。

### <a name="overriding-keyboard-events-and-behavior"></a>替代键盘事件和行为

你可以替代特定控件（如 [**GridView**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Controls.GridView)）的键事件，以针对各种输入设备（包括键盘和游戏板）提供一致的焦点导航。

在以下示例中，我们按下任意箭头键时，控件子类并替代 KeyDown 行为，以将焦点移动到 GridView 内容。

```csharp
public class CustomGridView : GridView
  {
    protected override void OnKeyDown(KeyRoutedEventArgs e)
    {
      // Override arrow key behaviors.
      if (e.Key != Windows.System.VirtualKey.Left && e.Key !=
        Windows.System.VirtualKey.Right && e.Key !=
          Windows.System.VirtualKey.Down && e.Key !=
            Windows.System.VirtualKey.Up)
              base.OnKeyDown(e);
      else
        FocusManager.TryMoveFocus(FocusNavigationDirection.Down);
    }
  }
```

> [!NOTE]
> 如果仅将 GridView 用于布局，请考虑使用其他控件（如 [**ItemsControl**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Controls.ItemsControl) 与 [**ItemsWrapGrid**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Controls.ItemsWrapGrid)）。

## <a name="commanding"></a>命令

少量 UI 元素提供对命令的内置支持。 命令在其基础实现中使用与输入相关的路由事件。 它能够通过调用一个命令处理程序来处理相关的 UI 输入，如某个指针操作或特定加速键。

如果命令处理可用于 UI 元素，可以考虑使用它的命令处理 API 代替任何具体的输入事件。 有关详细信息，请参阅 [**ButtonBase.Command**](https://msdn.microsoft.com/library/windows/apps/br227740)。

还可以实现 [**ICommand**](https://msdn.microsoft.com/library/windows/apps/br227885) 来封装从普通事件处理程序中调用的命令功能。 这使你甚至能够在没有任何可用 **Command** 属性的情况下使用命令。

## <a name="text-input-and-controls"></a>文本输入和控件

某些控件通过自身的处理来对键盘事件作出响应。 例如，[**TextBox**](https://msdn.microsoft.com/library/windows/apps/br209683) 是一个控件，设计用于捕获然后在视觉上显示使用键盘输入的文本。 它以自己的逻辑使用 [**KeyUp**](https://msdn.microsoft.com/library/windows/apps/br208942) 和 [**KeyDown**](https://msdn.microsoft.com/library/windows/apps/br208941) 来捕获击键，然后，即使在文本实际上已更改的情况下，也还是会引发自己的 [**TextChanged**](https://msdn.microsoft.com/library/windows/apps/br209706) 事件。

通常，仍然可以将 [**KeyUp**](https://msdn.microsoft.com/library/windows/apps/br208942) 和 [**KeyDown**](https://msdn.microsoft.com/library/windows/apps/br208941) 的处理程序添加到 [**TextBox**](https://msdn.microsoft.com/library/windows/apps/br209683) 或计划处理文本输入的任何相关控件中。 但是，作为其预期设计，一个控件可能并不会对通过键事件指向它的所有键值作出响应。 行为特定于每个控件。

例如，[**ButtonBase**](https://msdn.microsoft.com/library/windows/apps/br227736)（[**Button**](https://msdn.microsoft.com/library/windows/apps/br209265) 的基类）处理 [**KeyUp**](https://msdn.microsoft.com/library/windows/apps/br208942)，以便可以检查空格键或 Enter 键。 **ButtonBase** 认为 **KeyUp** 等同于按下鼠标左键以引发 [**Click**](https://msdn.microsoft.com/library/windows/apps/br227737) 事件。 在 **ButtonBase** 覆盖虚拟方法 [**OnKeyUp**](https://msdn.microsoft.com/library/windows/apps/hh967983) 时完成此事件处理操作。 在其实现过程中，会将 [**Handled**](https://msdn.microsoft.com/library/windows/apps/hh943073) 设置为 **true**。 如果空格键未接收到自己处理程序的 already-handled 事件，则结果为某个按钮的任意父按钮侦听键事件。

另一个示例是 [**TextBox**](https://msdn.microsoft.com/library/windows/apps/br209683)。 
            **TextBox** 不会将某些键（如箭头键）视为文本，而是视为特定于控件 UI 的行为。 
            **TextBox** 将这些事件案例标记为已处理。

自定义控件可以通过重写 [**OnKeyDown**](https://msdn.microsoft.com/library/windows/apps/hh967982) / [**OnKeyUp**](https://msdn.microsoft.com/library/windows/apps/hh967983) 来为键事件实现自己的类似重写行为。 如果你的自定义控件处理特定加速键或者具有类似于为 [**TextBox**](https://msdn.microsoft.com/library/windows/apps/br209683) 描述的方案的控件或焦点行为，则应该将该逻辑放置在自己的 **OnKeyDown** / **OnKeyUp** 重写中。

## <a name="the-touch-keyboard"></a>触摸键盘

文本输入控件提供对触摸键盘的自动支持。 当用户将输入焦点设置为使用触摸输入的文本控件时，触摸键盘会自动出现。 当输入焦点不在文本控件上时，隐藏触摸键盘。

当出现触摸键盘时，会自动重新定位你的 UI 以确保可以看见具有焦点的元素。 这可能会导致 UI 的其他重要区域移出屏幕。 但是，你可以禁用默认行为并在出现触摸键盘时对自己的 UI 进行调整。 有关详细信息，请参阅[触摸键盘示例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/TouchKeyboard)。

如果创建一个需文本输入但并非从标准文本输入控件派生的自定义控件，则可以通过实现正确的 UI 自动化控件模式来添加触摸键盘支持。 有关详细信息，请参阅[触摸键盘示例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/TouchKeyboard)。

按下触摸键盘上的键引发 [**KeyDown**](https://msdn.microsoft.com/library/windows/apps/br208941) 和 [**KeyUp**](https://msdn.microsoft.com/library/windows/apps/br208942) 事件，就像在硬件键盘上按下键一样。 但是，触摸键盘不会引发 Ctrl+A、Ctrl+Z、Ctrl+X、Ctrl+C 和 Ctrl+V 的输入事件，这些是输入控件中保留的文本操作。

通过将文本控件的输入范围设置为与你期望用户输入的数据类型匹配，可以让用户在应用中更快捷地输入数据。 输入范围会针对控件所预期的文本输入类型提供提示，以便系统可以为该输入类型提供专用的触摸键盘布局。 例如，如果文本框中仅用于输入一个 4 位数的 PIN，请将 [**InputScope**](https://msdn.microsoft.com/library/windows/apps/hh702632) 属性设置为 [**Number**](https://msdn.microsoft.com/library/windows/apps/hh702028)。 这将通知系统显示数字键盘布局，以便于用户输入 PIN。 有关详细信息，请参阅[使用输入范围更改触摸键盘](https://msdn.microsoft.com/library/windows/apps/mt280229)。

## <a name="related-articles"></a>相关文章

**开发人员**
* [键盘交互](keyboard-interactions.md)
* [标识输入设备](identify-input-devices.md)
* [响应触摸键盘的存在](respond-to-the-presence-of-the-touch-keyboard.md)

**设计人员**
* [键盘设计指南](https://msdn.microsoft.com/library/windows/apps/hh972345)

**示例**
* [触摸键盘示例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/TouchKeyboard)
* [基本输入示例](http://go.microsoft.com/fwlink/p/?LinkID=620302)
* [低延迟输入示例](http://go.microsoft.com/fwlink/p/?LinkID=620304)
* [焦点视觉示例](http://go.microsoft.com/fwlink/p/?LinkID=619895)

**存档示例**
* [输入示例](http://go.microsoft.com/fwlink/p/?linkid=226855)
* [输入：设备功能示例](http://go.microsoft.com/fwlink/p/?linkid=231530)
* [输入：触摸键盘示例](http://go.microsoft.com/fwlink/p/?linkid=246019)
* [响应屏幕键盘外观示例](http://go.microsoft.com/fwlink/p/?linkid=231633)
* [XAML 文本编辑示例](http://go.microsoft.com/fwlink/p/?LinkID=251417)
 

 
