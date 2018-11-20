---
author: Xansky
Description: If your app does not provide good keyboard access, users who are blind or have mobility issues can have difficulty using your app or may not be able to use it at all.
ms.assetid: DDAE8C4B-7907-49FE-9645-F105F8DFAD8B
title: 键盘辅助功能
label: Keyboard accessibility
template: detail.hbs
ms.author: mhopkins
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 41c5e018ee56b6a0d26bf2159f62801aa4ab5c3c
ms.sourcegitcommit: ed0304b8a214c03b8aab74b8ef12c9f82b8e3c5f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/19/2018
ms.locfileid: "7288625"
---
# <a name="keyboard-accessibility"></a>键盘辅助功能  



如果应用未提供良好的键盘访问，则盲人用户或行动不便的用户在使用该应用时会存在困难，或者可能根本无法使用该应用。

<span id="keyboard_navigation_among_UI_elements"/>
<span id="keyboard_navigation_among_ui_elements"/>
<span id="KEYBOARD_NAVIGATION_AMONG_UI_ELEMENTS"/>

## <a name="keyboard-navigation-among-ui-elements"></a>在 UI 元素之间的键盘导航  
若要将键盘用于控件，则控件必须具有焦点，若要接收焦点（不使用指针），则必须通过 Tab 键导航在 UI 设计中访问该控件。 默认情况下，控件的 Tab 键顺序与以下顺序相同：将其添加到设计图面的顺序、在 XAML 中列出的顺序或以编程方式添加到容器的顺序。

大多数情况下，默认顺序（取决于你在 XAML 中定义控件的方式）即为最佳顺序，因为该顺序就是屏幕阅读器读取控件的顺序。 但是，默认顺序并非一定与视觉顺序对应。 实际的显示位置可能取决于父布局容器以及可以在子元素上进行设置以影响布局的某些属性。 为了确保你的应用具有良好的 Tab 键顺序，请亲自测试此行为。 尤其是在你为你的布局设置了网格隐喻或表格隐喻时，用户可能读取的顺序与 Tab 键顺序将以不同方式结束。 这在其中或对其本身而言并不总是一个问题。 但是只需作为可触摸 UI 和键盘辅助功能 UI 测试你的应用的功能，即可验证你的 UI 通过以上哪种方式起作用。

你可以通过调整 XAML 使 Tab 键顺序与可见顺序相符。 也可以通过设置 [**TabIndex**](https://msdn.microsoft.com/library/windows/apps/BR209461) 属性来替代默认的 Tab 键顺序，如下面的 [**Grid**](https://msdn.microsoft.com/library/windows/apps/BR242704) 布局示例所示，该布局使用先列后行的 Tab 键导航顺序。

XAML
```xml
<!--Custom tab order.-->
<Grid>
  <Grid.RowDefinitions>...</Grid.RowDefinitions>
  <Grid.ColumnDefinitions>...</Grid.ColumnDefinitions>

  <TextBlock Grid.Column="1" HorizontalAlignment="Center">Groom</TextBlock>
  <TextBlock Grid.Column="2" HorizontalAlignment="Center">Bride</TextBlock>

  <TextBlock Grid.Row="1">First name</TextBlock>
  <TextBox x:Name="GroomFirstName" Grid.Row="1" Grid.Column="1" TabIndex="1"/>
  <TextBox x:Name="BrideFirstName" Grid.Row="1" Grid.Column="2" TabIndex="3"/>

  <TextBlock Grid.Row="2">Last name</TextBlock>
  <TextBox x:Name="GroomLastName" Grid.Row="2" Grid.Column="1" TabIndex="2"/>
  <TextBox x:Name="BrideLastName" Grid.Row="2" Grid.Column="2" TabIndex="4"/>
</Grid>
```

你可能希望从 Tab 键顺序中排除某个控件。 这通常只能通过使控件成为非交互控件（例如，将控件的 [**IsEnabled**](https://msdn.microsoft.com/library/windows/apps/BR209419) 属性设置为 **false**）来实现。 已禁用的控件从 Tab 键顺序中自动排除。 但有时，如果某个控件未禁用，你可能希望从 Tab 键顺序中排除它。 在这种情况下，你可以将 [**IsTabStop**](https://msdn.microsoft.com/library/windows/apps/BR209422) 属性设置为 **false**。

默认情况下，任何具有焦点的元素通常都位于 Tab 键顺序中。 例外情况是某些文本显示类型（如 [**RichTextBlock**](https://msdn.microsoft.com/library/windows/apps/BR227565)）可以具有焦点，以便它们可以通过剪贴板访问，从而进行文本选择；但是，这些类型不在 Tab 键顺序中，因为静态文本元素不应在 Tab 键顺序中。 它们通常不交互（它们不能被调用且不需要文本输入，但要支持[文本控件模式](https://msdn.microsoft.com/library/windows/desktop/Ee671194)，该模式支持在文本中查找和调整选择点）。 文本不应该暗示向其设置焦点可能会启用某些操作。 文本元素仍由辅助技术检测并在屏幕阅读器中大声读出，但这依赖于按照实际 Tab 键顺序查找这些元素之外的其他技术。

无论你是调整 [**TabIndex**](https://msdn.microsoft.com/library/windows/apps/BR209461) 值还是使用默认顺序，下列规则都适用：

* [**TabIndex**](https://msdn.microsoft.com/library/windows/apps/BR209461) 等于 0 的 UI 元素基于 XAML 或子集合中的声明顺序添加到 Tab 键顺序。
* [**TabIndex**](https://msdn.microsoft.com/library/windows/apps/BR209461) 大于 0 的 UI 元素基于 **tabIndex** 值添加到 Tab 键顺序。
* [**TabIndex**](https://msdn.microsoft.com/library/windows/apps/BR209461) 小于 0 的 UI 元素添加到 Tab 键顺序并显示在任何零值前面。 这可能与 HTML 处理其 **tabindex** 属性的操作不同（且负值 **tabindex** 在顺序 HTML 规范中不受支持）。

<span id="keyboard_navigation_within_a_UI_element"/>
<span id="keyboard_navigation_within_a_ui_element"/>
<span id="KEYBOARD_NAVIGATION_WITHIN_A_UI_ELEMENT"/>

## <a name="keyboard-navigation-within-a-ui-element"></a>在某个 UI 元素内的键盘导航  
对于复合元素，确保在所包含的元素之间的内部导航正确，这一点很重要。 复合元素可以管理其当前活动子元素以降低使所有子元素可聚焦的开销。 采用 Tab 键顺序将这样的复合元素包括在内，且该元素本身会处理键盘导航事件。 许多复合控件的事件处理中已经内置了某种内部导航逻辑。 例如，默认情况下允许使用箭头键遍历 [**ListView**](https://msdn.microsoft.com/library/windows/apps/BR242878)、[**GridView**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.gridview)、[**ListBox**](https://msdn.microsoft.com/library/windows/apps/BR242868) 和 [**FlipView**](https://msdn.microsoft.com/library/windows/apps/BR242678) 控件上的项。

<span id="keyboard_activation"/>
<span id="KEYBOARD_ACTIVATION"/>

## <a name="keyboard-alternatives-to-pointer-actions-and-events-for-specific-control-elements"></a>特定控件元素的指针操作和事件的键盘替换选项  
确保可以单击的 UI 元素也可以使用键盘调用。 若要将键盘用于某个 UI 元素，该元素必须具有焦点。 只有从 [**Control**](https://msdn.microsoft.com/library/windows/apps/BR209390) 派生的类才支持焦点和 Tab 键导航。

对于可以调用的 UI 元素，实现空格键和 Enter 键的键盘事件处理程序。 这样可使基本键盘辅助功能支持完整，且允许用户仅使用键盘来完成基本应用方案；即，用户可以使用所有交互 UI 元素并激活默认功能。

如果要在 UI 中使用的元素不能有焦点，你可以创建自己的自定义控件。 你必须将 [**IsTabStop**](https://msdn.microsoft.com/library/windows/apps/BR209422) 属性设置为 **true** 以启用焦点，而且必须通过创建一个使用焦点指示器来装饰 UI 的视觉状态，以直观形式指示已设定焦点的元素的状态。 不过，使用控件组合会更容易些，此时，对以下项的支持可以由选择在其中撰写内容的控件处理：制表位、焦点以及 Microsoft UI 自动化对等和模式。

例如，可以在 [**Button**](https://msdn.microsoft.com/library/windows/apps/BR209265) 中的 [**Image**](https://msdn.microsoft.com/library/windows/apps/BR242752) 元素上换行以获取指针、键盘和焦点支持，而不是处理该元素上的按下指针事件。

XAML
```xml
<!--Don't do this.-->
<Image Source="sample.jpg" PointerPressed="Image_PointerPressed"/>

<!--Do this instead.-->
<Button Click="Button_Click"><Image Source="sample.jpg"/></Button>
```

<span id="keyboard_shortcuts"/>
<span id="KEYBOARD_SHORTCUTS"/>

## <a name="keyboard-shortcuts"></a>键盘快捷方式  
除了实现应用的键盘导航和激活外，建议实现应用功能的快捷方式。 Tab 导航提供了较好的基本级别的键盘支持，但对于复杂的表单，你可能还希望添加快捷键支持。 即便对于既使用键盘又使用指针设备的用户而言，这样也会提高应用程序的使用效率。

*快捷方式*是一套键盘组合，通过为用户提供一种可访问应用功能的高效方法来提高效率。 有两种类型的快捷方式：

* *访问键*是应用中一个 UI 项的快捷方式。 访问键由 Alt 键和一个字母键组成。
* *加速键*是应用命令的快捷方式。 你的应用可以拥有与命令完全对应的 UI，也可以没有。 加速键由 Ctrl 键和一个字母键组成。

为依靠屏幕阅读器和其他辅助技术的用户提供简单方法来发现应用的快捷键，这一点很有必要。 通过使用工具提示、辅助名称、辅助说明和某种其他形式的屏幕通信来传达快捷键。 至少需要将快捷键详细记录在应用的帮助内容中。

可以通过屏幕阅读器记录访问键，方法是将 [**AutomationProperties.AccessKey**](https://msdn.microsoft.com/library/windows/apps/Hh759763) 附加属性设置为用于说明快捷键的字符串。 还有一个 [**AutomationProperties.AcceleratorKey**](https://msdn.microsoft.com/library/windows/apps/Hh759762) 附加属性，用于记录非助记快捷键，尽管屏幕阅读器通常对这两个属性的处理方式相同。 尝试采用多种方式记录快捷键：使用工具提示、自动化属性以及书面帮助文档。

以下示例说明了如何记录用于媒体播放、暂停以及停止按钮的快捷键。

XAML
```xml
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

> [!IMPORTANT]
> 设置 [**AutomationProperties.AcceleratorKey**](https://msdn.microsoft.com/library/windows/apps/Hh759762) 或 [**AutomationProperties.AccessKey**](https://msdn.microsoft.com/library/windows/apps/Hh759763) 不会启用键盘功能。 而只是向 UI 自动化框架报告应当使用哪些键，以便此类信息可以通过辅助技术传递给用户。 对于键处理的实现仍需要在代码（而非 XAML）中完成。 你将仍需要在相关控件上附加 [**KeyDown**](https://msdn.microsoft.com/library/windows/apps/BR208941) 或 [**KeyUp**](https://msdn.microsoft.com/library/windows/apps/BR208942) 事件的处理程序，以便在你的应用中真正实现键盘快捷键行为。 此外，不会自动为访问键提供带下划线的文本效果。 如果你希望在 UI 中显示带下划线的文本，则必须明确对助记键中特定键的文本标注下划线，作为嵌入式 [**Underline**](https://msdn.microsoft.com/library/windows/apps/BR209982) 格式。

为简单起见，上一个示例对于“Ctrl+A”之类的字符串没有使用资源。 但是，本地化过程中也必须考虑快捷键。 对快捷键进行本地化是相关的，因为选择将哪个键用作快捷键通常取决于元素的可见文本标签。

有关实现快捷键的更多指南，请参阅 Windows 用户体验交互指南中的[快捷键](http://go.microsoft.com/fwlink/p/?linkid=221825)。

<span id="Implementing_a_key_event_handler"/>
<span id="implementing_a_key_event_handler"/>
<span id="IMPLEMENTING_A_KEY_EVENT_HANDLER"/>

### <a name="implementing-a-key-event-handler"></a>实现键盘事件处理程序  
输入事件（如键事件）使用名为*路由事件*的事件概念。 路由事件可以通过复合控件的子元素弹出，以便公用控件的父元素可以处理多个子元素的事件。 对于包含多个在设计上不能具有焦点或位于 Tab 键顺序中的复合部分的控件，此事件模型便于为其定义快捷键操作。

有关说明如何编写键事件处理程序以包括对于修饰字母（如 Ctrl 键）的检查的示例代码，请参阅[键盘交互](https://msdn.microsoft.com/library/windows/apps/Mt185607)。

<span id="Keyboard_navigation_for_custom_controls"/>
<span id="keyboard_navigation_for_custom_controls"/>
<span id="KEYBOARD_NAVIGATION_FOR_CUSTOM_CONTROLS"/>

## <a name="keyboard-navigation-for-custom-controls"></a>自定义控件的键盘导航  
如果子元素之间相互具有空间关系，我们建议使用箭头键作为键盘快捷方式来在子元素之间导航。 如果树状视图节点有单独的子元素，用于处理展开折叠和节点激活，则应使用左右箭头键提供键盘展开折叠功能。 对于支持在控件内容中沿着一定的方向进行遍历的方向控件，请使用相应的箭头键。

通常，可通过在类逻辑中包括 [**OnKeyDown**](https://msdn.microsoft.com/library/windows/apps/hh967982.aspx) 和 [**OnKeyUp**](https://msdn.microsoft.com/library/windows/apps/hh967983.aspx) 方法的替代方法来为自定义控件实现自定义的键处理。

<span id="An_example_of_a_visual_state_for_a_focus_indicator"/>
<span id="an_example_of_a_visual_state_for_a_focus_indicator"/>
<span id="AN_EXAMPLE_OF_A_VISUAL_STATE_FOR_A_FOCUS_INDICATOR"/>

## <a name="an-example-of-a-visual-state-for-a-focus-indicator"></a>焦点指示符的视觉状态示例  
我们之前曾提到，任何允许用户为其设置焦点的自定义控件都应当具有一个视觉焦点指示器。 通常，该焦点指示器可以简单到直接围绕控件的普通边界矩形周围绘制一个矩形形状。 视觉焦点的 [**Rectangle**](/uwp/api/Windows.UI.Xaml.Shapes.Rectangle) 是该控件在控件模板中的其余组合的对等元素，但它最初是用 **Collapsed** 的 [**Visibility**](https://msdn.microsoft.com/library/windows/apps/BR208992) 值设置的，因为该控件尚未获取焦点。 然后，当控件获取焦点时，会调用一个专门为对于 **Visible** 可见的焦点设置 **Visibility** 的视觉状态。 在焦点移到其他位置之后，会调用另一个视觉状态，**Visibility** 会变成 **Collapsed**。

所有默认 XAML 控件在获得焦点时（如果它们可以获得焦点），都将显示相应的视觉焦点指示器。 还可能存在不同的外观，具体取决于用户所选的主题（尤其是用户使用高对比度模式。）如果你在 UI 中使用的是 XAML 控件且未更换控件模板，则无需执行任何额外操作，即可在正确运行和显示的控件上获取视觉焦点指示器。 但是如果你想要重新创建控件模板，或者如果你对 XAML 控件如何提供其视觉焦点指示器感兴趣，则本部分的剩余部分将介绍如何在 XAML 中和控件逻辑中完成此操作。

下面是来自 [**Button**](https://msdn.microsoft.com/library/windows/apps/BR209265) 的默认 XAML 模板的示例 XAML。

XAML
```xml
<ControlTemplate TargetType="Button">
...
    <Rectangle
      x:Name="FocusVisualWhite"
      IsHitTestVisible="False"
      Stroke="{ThemeResource FocusVisualWhiteStrokeThemeBrush}"
      StrokeEndLineCap="Square"
      StrokeDashArray="1,1"
      Opacity="0"
      StrokeDashOffset="1.5"/>
    <Rectangle
      x:Name="FocusVisualBlack"
      IsHitTestVisible="False"
      Stroke="{ThemeResource FocusVisualBlackStrokeThemeBrush}"
      StrokeEndLineCap="Square"
      StrokeDashArray="1,1"
      Opacity="0"
      StrokeDashOffset="0.5"/>
...
</ControlTemplate>
```

到目前为止，这只是一个组合。 若要控制焦点指示器的可见性，需要定义可切换 [**Visibility**](https://msdn.microsoft.com/library/windows/apps/BR208992) 属性的视觉状态。 使用 [**VisualStateManager.VisualStateGroups**](https://msdn.microsoft.com/library/windows/apps/Hh738505) 附加属性可完成此操作，该附加属性应用于用来定义此组合的根元素。

XAML
```xml
<ControlTemplate TargetType="Button">
  <Grid>
    <VisualStateManager.VisualStateGroups>
       <!--other visual state groups here-->
       <VisualStateGroup x:Name="FocusStates">
         <VisualState x:Name="Focused">
           <Storyboard>
             <DoubleAnimation
               Storyboard.TargetName="FocusVisualWhite"
               Storyboard.TargetProperty="Opacity"
               To="1" Duration="0"/>
             <DoubleAnimation
               Storyboard.TargetName="FocusVisualBlack"
               Storyboard.TargetProperty="Opacity"
               To="1" Duration="0"/>
         </VisualState>
         <VisualState x:Name="Unfocused" />
         <VisualState x:Name="PointerFocused" />
       </VisualStateGroup>
     <VisualStateManager.VisualStateGroups>
<!--composition is here-->
   </Grid>
</ControlTemplate>
```

请注意，仅一个命名状态直接调整 [**Visibility**](https://msdn.microsoft.com/library/windows/apps/BR208992)，而其他状态似乎仍未空白。 视觉状态的工作方式如下：一旦控件使用来自同一个 [**VisualStateGroup**](https://msdn.microsoft.com/library/windows/apps/BR209014) 的另一个状态，由上一个状态应用的任何动画都将立即取消。 由于来自该组合的默认 **Visibility** 是 **Collapsed**，因此这意味着矩形将不出现。 控制逻辑通过侦听焦点事件（如 [**GotFocus**](https://msdn.microsoft.com/library/windows/apps/BR208927)）并使用 [**GoToState**](https://msdn.microsoft.com/library/windows/apps/BR209025) 更改状态来达到控制目的。 如果你使用默认控件或基于已具有该行为的控件进行自定义，则通常已经为你进行处理。

<span id="Keyboard_accessibility_and_Windows_Phone"/>
<span id="keyboard_accessibility_and_windows_phone"/>
<span id="KEYBOARD_ACCESSIBILITY_AND_WINDOWS_PHONE"/>

## <a name="keyboard-accessibility-and-windows-phone"></a>键盘辅助功能和 Windows Phone
Windows Phone 设备通常不具有专用的硬件键盘。 但是，软输入面板 (SIP) 可以支持若干键盘辅助功能方案。 屏幕阅读器可以读出“文本”**** SIP 的文本输入，包括读出删除的内容。 用户可以发现其手指所在的位置，因为屏幕阅读器可以检测到用户正在浏览各键，并且它可以大声读出浏览到的键名称。 同样，可以将某些面向键盘的辅助功能概念映射到完全不使用键盘的相关辅助技术行为。 例如，即使 SIP 不包括 Tab 键，讲述人也支持等效于按下 Tab 键的触摸手势，因此在 UI 中各个控件具有有用的 Tab 键顺序仍然是一项重要的辅助功能原则。 用于导航复杂控件中的各个部件的箭头键也可通过讲述人触摸手势受到支持。 在焦点到达不用于文本输入的控件后，讲述人支持可调用该控件的操作的手势。

键盘快捷方式通常不与 Windows Phone 应用相关，因为 SIP 不包含 Ctrl 或 Alt 键。

<span id="related_topics"/>

## <a name="related-topics"></a>相关主题  
* [辅助功能](accessibility.md)
* [键盘交互](https://msdn.microsoft.com/library/windows/apps/Mt185607)
* [触摸键盘示例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/TouchKeyboard)
* [XAML 辅助功能示例](http://go.microsoft.com/fwlink/p/?linkid=238570)

