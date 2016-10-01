---
author: Karl-Bridge-Microsoft
Description: "支持使用 Tab 键导航和访问键的键盘访问，以便用户可以使用键盘在 UI 元素之间导航。"
title: "访问键"
ms.assetid: C2F3F3CE-737F-4652-98B7-5278A462F9D3
label: Access keys
template: detail.hbs
keyword: Access keys, keyboard, accessibility
translationtype: Human Translation
ms.sourcegitcommit: ac86012b63646e53dbde492eef504cb8230f2afd
ms.openlocfilehash: d96d507c6ce8537888619ce174e2ff0e5284dcce

---

# 访问键

使用鼠标有困难的用户（如行动有障碍的用户）通常依赖键盘在应用上导航并与应用交互。  XAML 框架使你可以实现通过 Tab 键导航和访问键来键盘访问 UI 元素。

- Tab 键导航是基本的键盘辅助功能提示（默认情况下已启用），允许用户使用键盘上的 Tab 键和箭头键在 UI 元素之间移动焦点。
- 访问键是补充的辅助功能提示（在应用中实现），以实现使用键盘修饰符（Alt 键）和一个或多个字母数字键（通常为与命令关联的字母）的组合来快速访问应用命令。 常用访问键包括用于打开“文件”菜单的 _Alt+F_ 以及用于执行左对齐的 _Alt+AL_。  

有关键盘导航和辅助功能的详细信息，请参阅[键盘交互](https://msdn.microsoft.com/windows/uwp/input-and-devices/keyboard-interactions)和[键盘辅助功能](https://msdn.microsoft.com/windows/uwp/accessibility/keyboard-accessibility)。 文本假定你了解这些文章中所讨论的概念。

## 访问键概述

借助访问键，用户使用键盘就可以直接调用按钮或设置焦点，无需他们反复按箭头键和 Tab 键。 访问键要易于发现，因此应该将它们直接公布在 UI 中；例如，基于访问键控制的浮动锁屏提醒。

![Microsoft Word 中的访问键以及关联的键提示的示例](images/keyboard/accesskeys-keytips.png)

_图 1：Microsoft Word 中的访问键以及关联的键提示的示例。_

访问键是与 UI 元素关联的一个或多个字母数字字符。 例如，Microsoft Word 将 _H_ 用于“主页”选项卡、将 _2_ 用于“撤销”按钮或将 _JI_ 用于“绘制”选项卡。

**访问键作用域**

一个访问键属于一个特定作用域。 例如，在图 1 中，_F_、_H_、_N_ 和 _JI_ 属于页面作用域。  当用户按下 _H_ 时，该作用域将变更为“主页”选项卡作用域，同时会显示其访问键，如图 2 所示。 访问键 _V_、_FP_、_FF_ 和 _FS_ 属于“主页”选项卡作用域。

![Microsoft Word 中“主页”选项卡作用域的访问键以及关联的键提示的示例](images/keyboard/accesskeys-keytips-hometab.png)

_图 2：Microsoft Word 中“主页”选项卡作用域的访问键以及关联的键提示的示例。_

如果两个元素属于不同作用域，这两个元素可以具有相同的访问键。 例如，_2_ 是页面作用域中“撤销”的访问键（图 1），也是“主页”选项卡作用域中“斜体”的访问键（图 2）。 除非另行指定作用域，否则所有访问键都属于默认作用域。

**访问键序列**

实现操作的访问键组合通常一次按下一个键，而不是同时按下多个键。 （我们会在下一节中介绍相关例外情况。）实现操作所需的击键序列即为_访问键序列_。 用户按下 Alt 键以启动访问键序列。 在用户按下访问键序列的最后一个键时，将调用该访问键。 例如，若要在 Word 中打开“视图”选项卡，用户将按 _Alt, W_ 访问键序列。

用户可以在一个访问键序列中调用多个访问键。 例如，若要在 Word 文档中打开“格式刷”，用户按下 Alt 以启动该序列，然后按下 _H_ 以导航到“主页”部分（同时更改访问键作用域），最后依次按下 _F_ 和 _P_。_H_ 和 _FP_ 分别为“主页”选项卡和“格式刷”按钮的访问键。

某些元素会在被调用后结束访问键序列（如“格式刷”按钮），而其他元素则不会（如“主页”选项卡）。 调用访问键可能会导致执行命令、移动焦点、更改访问键作用域或相关联的某些其他操作。

## 访问键用户交互

若要了解访问键 API，首先需要了解用户交互模型。 可以从下面找到访问键用户交互模型的摘要：

- 当用户按下 Alt 键时，会启动访问键序列，即使焦点位于输入控件上也是如此。 然后，用户可以按访问键来调用关联的操作。 此用户交互要求你在带有某些可视化提示的 UI（例如会在按下 Alt 键时显示的浮动锁屏提醒）内公布可用访问键
- 当用户同时按下 Alt 键以及访问键时，会立即调用该访问键。 这类似于由 Alt+_访问键_定义的键盘快捷方式。 在这种情况下，不会显示访问键可视化提示。 但是，调用访问键可能会导致更改访问键作用域。 在这种情况下，会启动访问键序列，针对新作用域显示可视化提示。
    > [!NOTE]
    > 仅具有一个字符的访问键可以充分利用此用户交互。 针对具有多个字符的访问键，不支持 Alt+_访问键_组合。    
- 当存在多个共享某些字符的多字符访问键时，如果用户按下共享的字符时，会筛选这些访问键。 例如，假定具有如下所示的三个访问键：_A1_、_A2_ 和 _C_。如果用户按下 _A_，将仅显示 _A1_ 和 _A2_ 访问键，隐藏 C 的可视化提示。
- Esc 键会删除筛选的一个级别。 例如，如果存在访问键 _B_、_ABC_、_ACD_ 和 _ABD_，当用户按下 _A_ 时，将仅显示 _ABC_、_ACD_ 和 _ABD_。 如果用户接着按下 _B_，将仅显示 _ABC_ 和 _ABD_。 如果用户按 Esc，会删除筛选的一个级别，同时显示 _ABC_、_ACD_ 和 _ABD_ 访问键。 如果用户再次按 Esc，将删除筛选的另一个级别，同时启用所有访问键（即 _B_、_ABC_、_ACD_ 和 _ABD_）并显示其可视化提示。
- Esc 键会导航回前一个作用域。 访问键可以属于不同作用域，以使其更轻松地在具有许多命令的应用间导航。 访问键序列始终起始于主作用域。 除了指定特定 UI 元素为其作用域所有者的访问键之外，其他所有访问键都属于主作用域。 当用户调用为作用域所有者的元素的访问键时，XAML 框架会自动将该作用域移动给它，然后将它添加到内部的访问键导航堆栈。 Esc 键会反向移动访问键导航堆栈。
- 有多种方法可消除访问键序列：
    - 用户可以按下 Alt 消除正在进行的访问键序列。 请记住，按下 Alt 也会启动访问键序列。
    - 如果访问键位于主作用域并且未经过筛选，则 Esc 键将消除访问键序列。
        > [!NOTE]
        > 还会将 Esc 击键传递到 UI 层以在该位置进行处理。
- Tab 键消除访问键序列，并返回到 Tab 键导航。
- Enter 键消除访问键序列，并将击键发送到具有焦点的元素。
- 箭头键消除访问键序列，并将击键发送到具有焦点的元素。
- 指针向下事件（如鼠标单击或触摸）消除访问键序列。
- 默认情况下，当调用访问键时，会消除访问键序列。  不过，你可以通过将 [ExitDisplayModeOnAccessKeyInvoked](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.uielement.exitdisplaymodeonaccesskeyinvoked.aspx) 属性设置为 **false** 来替代此行为。
- 当确定性的有限自动化不可能实现时，会发生访问键冲突。 不希望发生访问键冲突，但大量的命令、本地化问题或访问键的运行时生成可能会导致此问题。

 在两种情况下会发生冲突：
 - 当两个 UI 元素具有相同的访问键值并且属于相同的访问键作用域时。 例如，属于默认作用域的以下两个访问键：`button1` 的 _A1_ 和 `button2` 的 _A1_。 在此情况下，系统会按以下方式解决该冲突：处理已添加到可视化树的第一个元素的访问键。 忽略其余元素的访问键。
 - 当相同的访问键作用域中存在多个计算选项时。 例如，_A_ 和 _A1_。 当用户按下 _A_ 时，系统有两个选项：调用 _A_ 访问键或继续执行并使用 _A1_ 访问键中的 A 字符。 在此情况下，系统将仅处理自动机已到达的第一个访问键调用。 例如，_A_ 和 _A1_，系统将仅调用 _A_ 访问键。
-   当用户按下访问键序列中的无效访问键值时，什么也不会发生。 有两种类别的键视为访问键序列中的有效访问键：
 - 用于退出访问键序列的特殊键：即 Esc、Alt、箭头键、Enter 和 Tab。
 - 已分配给访问键的字母数字字符。

## 访问键 API

为了支持访问键用户交互，XAML 框架提供了此处所述的 API。

**AccessKeyManager**

[AccessKeyManager](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.input.accesskeymanager.aspx) 是一个帮助程序类，可用于在显示或隐藏访问键时管理 UI。 每当应用进入和退出访问键序列时，就会引发 [IsDisplayModeEnabledChanged](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.input.accesskeymanager.isdisplaymodeenabledchanged.aspx) 事件。 可以查询 [IsDisplayModeEnabled](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.input.accesskeymanager.isdisplaymodeenabled.aspx) 属性以确定可视化提示是处于显示状态，还是处于隐藏状态。  还可以调用 [ExitDisplayMode](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.input.accesskeymanager.exitdisplaymode.aspx) 以强制消除访问键序列。

> [!NOTE]
> 并未内置访问键的可视化实现；必须提供该可视化。  

**AccessKey**

借助 [AccessKey](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.uielement.accesskey.aspx) 属性，你可以指定 UIElement 或 [TextElement](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.documents.textelement.accesskey.aspx) 上的访问键。 如果两个元素具有相同的访问键和相同的作用域，将仅处理已添加到可视化树的第一个元素。

若要确保 XAML 框架能够处理访问键，必须在可视化树中实现 UI 元素。 如果具有访问键的可视化树中没有任何元素，则不会引发任何访问键事件。

访问键 API 不支持需要生成两个击键的字符。 单个字符必须对应于特定语言的本地键盘布局上的键。  

**AccessKeyDisplayRequested/Dismissed**

当应显示或消除访问键可视化提示时，将引发 [AccessKeyDisplayRequested](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.uielement.accesskeydisplayrequested.aspx) 和 [AccessKeyDisplayDismissed](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.uielement.accesskeydisplaydismissed.aspx) 事件。 不会为其 [Visibility](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.uielement.visibility.aspx) 属性设置为 **Collapsed** 的元素引发这些事件。 每次用户按下访问键使用的字符时，都在访问键序列期间引发 AccessKeyDisplayRequested 事件。 例如，如果访问键设置为 _AB_，当用户按下 Alt 时会引发此事件，当用户按下 _A_ 时会再次引发此事件。当用户按下 _B_ 时，会引发 AccessKeyDisplayDismissed 事件

**AccessKeyInvoked**

当用户到达访问键的最后一个字符时，会引发 [AccessKeyInvoked](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.uielement.accesskeyinvoked.aspx) 事件。 访问键可以具有一个或多个字符。 例如，对于访问键 _A_ 和 _BC_，当用户按下 _Alt, A_ 或 _Alt, B, C_ 时，将引发该事件；但当用户仅按下 _Alt, B_ 时，不会引发该事件。该事件在按下键而不是释放键时引发。

**IsAccessKeyScope**

[IsAccessKeyScope](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.uielement.isaccesskeyscope.aspx) 属性使你可以指定 UIElement 是否是访问键作用域的根。 针对此元素引发 AccessKeyDisplayRequested 事件，但不会针对其子元素引发该事件。 当用户调用此元素时，XAML 框架会自动更改作用域，并针对其子元素引发 AccessKeyDisplayRequested 事件，针对其他 UI 元素（包括父元素）引发 AccessKeyDisplayDismissed 事件。  作用域发生更改时，不会退出访问键序列。

**AccessKeyScopeOwner**

若要使某个元素加入可视化树中不是其父元素的另一个元素（源）的作用域，可以设置 [AccessKeyScopeOwner](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.uielement.accesskeyscopeowner.aspx) 属性。 与 AccessKeyScopeOwner 属性绑定的元素必须将 IsAccessKeyScope 设置为 **true**。 否则，会引发异常。

**ExitDisplayModeOnAccessKeyInvoked**

默认情况下，当调用访问键但元素不是作用域所有者时，会结束该访问键序列，并引发 [AccessKeyManager.IsDisplayModeEnabledChanged](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.input.accesskeymanager.isdisplaymodeenabledchanged.aspx) 事件。 可以将 [ExitDisplayModeOnAccessKeyInvoked](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.uielement.exitdisplaymodeonaccesskeyinvoked.aspx) 属性设置为 **false** 以替代此行为，并阻止在调用访问键后退出该访问键序列。 （[UIElement](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.uielement.exitdisplaymodeonaccesskeyinvoked.aspx) 和 [TextElement](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.documents.textelement.exitdisplaymodeonaccesskeyinvoked.aspx) 具有此属性）。

> [!NOTE]
> 如果元素为作用域所有者 (`IsAccessKeyScope="True"`)，应用将进入新的访问键作用域，不会引发 IsDisplayModeEnabledChanged 事件。

**本地化**

访问键可以本地化为多种语言，并在运行时使用 [ResourceLoader](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.resources.resourceloader.aspx) API 进行加载。

## 调用访问键时所用的控件模式

控件模式是公开常见控件功能的接口实现；例如，按钮实现 **Invoke** 控件模式，这将引发 **Click** 事件。 当调用访问键时，XAML 框架会查找已调用的元素是否实现控件模式并执行它（如果要执行）。 如果元素具有多个控件模式，将仅调用一个控件模式，忽略其余控件模式。 按以下顺序搜索控件模式：

1.  调用。 例如，一个按钮。
2.  切换。 例如，一个复选框。
3.  选择。 例如，一个单选按钮。
4.  展开/折叠。 例如，一个组合框。

如果未找到控件模式，访问键调用将显示为空操作，会记录调试消息以帮助你调试此种情况：“未找到此组件的任何自动化模式。 在 AccessKeyInvoked 的事件处理程序中实现所需行为。 在事件处理程序中将 Handled 设置为 true 会取消显示此消息。”

> [!NOTE]
> 在 Visual Studio 调试设置中，调试器的应用程序处理类型必须为“混合(托管和本机)”__或“本机”__才可以看到此消息。

如果不希望访问键执行其默认控件模式，或者如果元素不具有控件模式，则应该处理 [AccessKeyInvoked](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.uielement.accesskeyinvoked.aspx) 事件并实现所需行为。
```csharp
private void OnAccessKeyInvoked(UIElement sender, AccessKeyInvokedEventArgs args)
{
    args.Handled = true;
    //Do something
}
```

有关控件模式的详细信息，请参阅 [UI 自动化控件模式概述](https://msdn.microsoft.com/library/windows/desktop/ee671194.aspx)。

## 访问键和讲述人

Windows 运行时具有的 UI 自动化提供程序会公开 Microsoft UI 自动化元素上的属性。 通过这些属性，UI 自动化客户端应用程序可以发现有关用户界面部分的信息。 通过 [AutomationProperties.AccessKey](https://msdn.microsoft.com/library/windows/apps/hh759763) 属性，客户端（如“讲述人”）可以发现与某个元素关联的访问键。 每当元素获得焦点时，“讲述人”都会读取此属性。 如果 AutomationProperties.AccessKey 未赋值，XAML 框架会返回 UIElement 或 TextElement 的 [AccessKey](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.uielement.accesskey.aspx) 属性值。 如果 AccessKey 属性已赋值，则不需要设置 AutomationProperties.AccessKey。

## 示例：按钮的访问键

本示例演示如何为按钮创建访问键。 它将工具提示用作可视化提示来实现包含访问键的浮动锁屏提醒。

> [!NOTE]
> 为简单起见，使用了工具提示，但我们建议你自行创建用于显示访问键的控件（例如，[Popup](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.primitives.popup.aspx)）。

XAML 框架会自动调用 Click 事件的处理程序，因此无需处理 AccessKeyInvoked 事件。 该示例仅为用于通过使用 [AccessKeyDisplayRequestedEventArgs.PressedKeys](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.input.accesskeydisplayrequestedeventargs.pressedkeys.aspx) 属性调用访问键的保留字符提供可视化提示。 例如，如果有三个已显示的访问键：_A1_、_A2_ 和 _C_，用户按下 _A_，仅 _A1_ 和 _A2_ 访问键未被筛选掉，并显示为 _1_ 和 _2_，而不是 _A1_ 和 _A2_。

```xaml
<StackPanel
        VerticalAlignment="Center"
        HorizontalAlignment="Center"
        Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
        <Button Content="Press"
                AccessKey="PB"
                AccessKeyDisplayDismissed="OnAccessKeyDisplayDismissed"
                AccessKeyDisplayRequested="OnAccessKeyDisplayRequested"
                Click="DoSomething" />
        <TextBlock Text="" x:Name="textBlock" />
    </StackPanel>
```

```csharp
 public sealed partial class ButtonSample : Page
    {
        public ButtonSample()
        {
            this.InitializeComponent();
        }

        private void DoSomething(object sender, RoutedEventArgs args)
        {
            textBlock.Text = "Access Key is working!";
        }

        private void OnAccessKeyDisplayRequested(UIElement sender, AccessKeyDisplayRequestedEventArgs args)
        {
            var tooltip = ToolTipService.GetToolTip(sender) as ToolTip;

            if (tooltip == null)
            {
                tooltip = new ToolTip();
                tooltip.Background = new SolidColorBrush(Windows.UI.Colors.Black);
                tooltip.Foreground = new SolidColorBrush(Windows.UI.Colors.White);
                tooltip.Padding = new Thickness(4, 4, 4, 4);
                tooltip.VerticalOffset = -20;
                tooltip.Placement = PlacementMode.Bottom;
                ToolTipService.SetToolTip(sender, tooltip);
            }

            if (string.IsNullOrEmpty(args.PressedKeys))
            {
                tooltip.Content = sender.AccessKey;
            }
            else
            {
                tooltip.Content = sender.AccessKey.Remove(0, args.PressedKeys.Length);
            }

            tooltip.IsOpen = true;
        }
        private void OnAccessKeyDisplayDismissed(UIElement sender, AccessKeyDisplayDismissedEventArgs args)
        {
            var tooltip = ToolTipService.GetToolTip(sender) as ToolTip;
            if (tooltip != null)
            {
                tooltip.IsOpen = false;
                //Fix to avoid show tooltip with mouse
                ToolTipService.SetToolTip(sender, null);
            }
        }
    }
```

## 示例：限定作用域的访问键

本示例演示如何创建限定作用域的访问键。 当用户按下 Alt 时，PivotItem 的 IsAccessKeyScope 属性会阻止显示 PivotItem 子元素的访问键。 仅当用户调用 PivotItem 时才会显示这些访问键，因为 XAML 框架会自动切换作用域。 该框架还会隐藏其他作用域的访问键。

本示例还演示如何处理 AccessKeyInvoked 事件。 由于 PivotItem 未实现任何控件模式，因此默认情况下，XAML 框架不会调用任何操作。 本实现演示如何使用访问键选择已调用的 PivotItem。

最后，本示例演示显示模式更改时，可以在其中执行某些操作的 IsDisplayModeChanged 事件。 在此示例中，透视控件处于折叠状态，直到用户按下 Alt。 当用户结束与透视的交互时，它会重新折叠起来。 可以使用 IsDisplayModeEnabled 检查访问键显示模式处于启用状态，还是处于禁用状态。

```xaml   
<Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
        <Pivot x:Name="MyPivot" VerticalAlignment="Center" HorizontalAlignment="Center" >
            <Pivot.Items>
                <PivotItem
                    x:Name="PivotItem1"
                    AccessKey="A"
                    AccessKeyInvoked="OnAccessKeyInvoked"
                    AccessKeyDisplayDismissed="OnAccessKeyDisplayDismissed"
                    AccessKeyDisplayRequested="OnAccessKeyDisplayRequested"
                    IsAccessKeyScope="True">
                    <PivotItem.Header>
                        <TextBlock Text="A Options"/>
                    </PivotItem.Header>
                    <StackPanel Orientation="Horizontal" >
                        <Button Content="ButtonAA" AccessKey="A"
                                AccessKeyDisplayDismissed="OnAccessKeyDisplayDismissed"
                                AccessKeyDisplayRequested="OnAccessKeyDisplayRequested" />
                        <Button Content="ButtonAD1" AccessKey="D1"
                                AccessKeyDisplayDismissed="OnAccessKeyDisplayDismissed"
                                AccessKeyDisplayRequested="OnAccessKeyDisplayRequested"  />
                        <Button Content="ButtonAD2" AccessKey="D2"
                                AccessKeyDisplayDismissed="OnAccessKeyDisplayDismissed"
                                AccessKeyDisplayRequested="OnAccessKeyDisplayRequested"/>
                    </StackPanel>
                </PivotItem>
                <PivotItem
                    x:Name="PivotItem2"
                    AccessKeyInvoked="OnAccessKeyInvoked"
                    AccessKeyDisplayDismissed="OnAccessKeyDisplayDismissed"
                    AccessKeyDisplayRequested="OnAccessKeyDisplayRequested"
                    AccessKey="B"
                    IsAccessKeyScope="true">
                    <PivotItem.Header>
                        <TextBlock Text="B Options"/>
                    </PivotItem.Header>
                    <StackPanel Orientation="Horizontal">
                        <Button AccessKey="B" Content="ButtonBB"
                                AccessKeyDisplayDismissed="OnAccessKeyDisplayDismissed"
                                AccessKeyDisplayRequested="OnAccessKeyDisplayRequested"  />
                        <Button AccessKey="F1" Content="ButtonBF1"
                                AccessKeyDisplayDismissed="OnAccessKeyDisplayDismissed"
                                AccessKeyDisplayRequested="OnAccessKeyDisplayRequested"  />
                        <Button AccessKey="F2" Content="ButtonBF2"  
                                AccessKeyDisplayDismissed="OnAccessKeyDisplayDismissed"
                                AccessKeyDisplayRequested="OnAccessKeyDisplayRequested"/>
                    </StackPanel>
                </PivotItem>
            </Pivot.Items>
        </Pivot>
    </Grid>
```

```csharp
public sealed partial class ScopedAccessKeys : Page
    {
        public ScopedAccessKeys()
        {
            this.InitializeComponent();
            AccessKeyManager.IsDisplayModeEnabledChanged += OnDisplayModeEnabledChanged;
            this.Loaded += OnLoaded;
        }

        void OnLoaded(object sender, object e)
        {
            //To let the framework discover the access keys, the elements should be realized
            //on the visual tree. If there are no elements in the visual
            //tree with access key, the framework won't raise the events.
            //In this sample, if you define the Pivot as collapsed on the constructor, the Pivot
            //will have a lazy loading and the access keys won't be enabled.
            //For this reason, we make it visible when creating the object
            //and we collapse it when we load the page.
            MyPivot.Visibility = Visibility.Collapsed;
        }

        void OnAccessKeyInvoked(UIElement sender, AccessKeyInvokedEventArgs args)
        {
            args.Handled = true;
            MyPivot.SelectedItem = sender as PivotItem;
        }
        void OnDisplayModeEnabledChanged(object sender, object e)
        {
            if (AccessKeyManager.IsDisplayModeEnabled)
            {
                MyPivot.Visibility = Visibility.Visible;
            }
            else
            {
                MyPivot.Visibility = Visibility.Collapsed;

            }
        }

        DependencyObject AdjustTarget(UIElement sender)
        {
            DependencyObject target = sender;
            if (sender is PivotItem)
            {
                PivotItem pivotItem = target as PivotItem;
                target = (sender as PivotItem).Header as TextBlock;
            }
            return target;
        }

        void OnAccessKeyDisplayRequested(UIElement sender, AccessKeyDisplayRequestedEventArgs args)
        {
            DependencyObject target = AdjustTarget(sender);
            var tooltip = ToolTipService.GetToolTip(target) as ToolTip;

            if (tooltip == null)
            {
                tooltip = new ToolTip();
                tooltip.Background = new SolidColorBrush(Windows.UI.Colors.Black);
                tooltip.Foreground = new SolidColorBrush(Windows.UI.Colors.White);
                tooltip.Padding = new Thickness(4, 4, 4, 4);
                tooltip.VerticalOffset = -20;
                tooltip.Placement = PlacementMode.Bottom;
                ToolTipService.SetToolTip(target, tooltip);
            }

            if (string.IsNullOrEmpty(args.PressedKeys))
            {
                tooltip.Content = sender.AccessKey;
            }
            else
            {
                tooltip.Content = sender.AccessKey.Remove(0, args.PressedKeys.Length);
            }

            tooltip.IsOpen = true;
        }
        void OnAccessKeyDisplayDismissed(UIElement sender, AccessKeyDisplayDismissedEventArgs args)
        {
            DependencyObject target = AdjustTarget(sender);

            var tooltip = ToolTipService.GetToolTip(target) as ToolTip;
            if (tooltip != null)
            {
                tooltip.IsOpen = false;
                //Fix to avoid show tooltip with mouse
                ToolTipService.SetToolTip(target, null);
            }
        }
    }
```



<!--HONumber=Aug16_HO3-->


