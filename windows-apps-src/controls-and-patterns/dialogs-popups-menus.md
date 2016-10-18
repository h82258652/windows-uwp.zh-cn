---
author: Jwmsft
redirect_url: https://msdn.microsoft.com/windows/uwp/controls-and-patterns/dialogs
Description: "浮出控件是轻型弹出窗口，用来临时显示与用户当前正在执行的操作相关的 UI。"
title: "上下文菜单和对话框"
ms.assetid: 7CA2600C-A1DB-46AE-8F72-24C25E224417
label: Menus, dialogs, and popups
template: detail.hbs
translationtype: Human Translation
ms.sourcegitcommit: eb6744968a4bf06a3766c45b73b428ad690edc06
ms.openlocfilehash: 6572acefa25e464b6edaca9fee5b2b3e3b46ff3f

---
# 菜单、对话框、浮出控件和弹出窗口

<link rel="stylesheet" href="https://az835927.vo.msecnd.net/sites/uwp/Resources/css/custom.css"> 

菜单、对话框、浮出控件和弹出窗口显示的瞬态 UI 元素在用户请求这些元素或是发生需要通知或许可的操作时出现。

<div class="important-apis" >
<b>重要的 API</b><br/>
<ul>
<li><a href="https://msdn.microsoft.com/library/windows/apps/dn299030">MenuFlyout 类</a></li>
<li><a href="https://msdn.microsoft.com/library/windows/apps/dn279496">Flyout 类</a></li>
<li><a href="https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.contentdialog.aspx">ContentDialog 类</a></li>
</ul>

</div>
</div>






上下文菜单向用户提供即时操作。 可以使用文本命令填充它。 可以通过点击或单击菜单之外的某个位置来轻型消除上下文菜单。

对话框是用于提供上下文应用信息的模式 UI 覆盖。 除非明确取消对话框，否则它会阻止与应用窗口的交互。 它们通常会请求用户进行某种类型的操作。

浮出控件是轻量级上下文弹出窗口，用于显示与用户正在执行的操作相关的 UI。 它包含放置和大小调整逻辑，可用于显示隐藏的控件、显示有关某个项目的更多详细信息，或者请求用户确认某个操作。 可以通过点击或单击弹出窗口之外的某个位置来轻型消除浮出控件。


## 这是正确的控件吗？

上下文菜单可用于：

-   上下文操作。
-   必须对其执行操作但是无法选择的对象的命令。

对话框可用于：

- 表示用户在继续操作之前必须阅读并确认的重要信息。
- 请求用户执行明确操作，或传递用户应确认的重要消息。 示例包括：
  - 当用户的安全可能受到威胁时
  - 当用户准备永久改变宝贵资产时
  - 当用户准备删除宝贵资产时
  - 确认应用内购买
- 应用于整个应用上下文的错误消息，如连接错误。
- 问题，在应用需要询问用户阻止问题时（例如当应用不能代表用户进行选择时）。 阻止问题不能忽略或延迟，并应该向用户提供明确定义的选项。

浮出控件可用于：

-   上下文、瞬态 UI。
-   警告和确认，包括与可能有破坏性的操作相关的警告和确认。
-   显示详细信息，例如页面上某个项目的详细信息或较长说明。


## 示例

下面是带有简单命令的短列表的典型单窗格上下文菜单。 如有必要，它可以滚动。 根据需要使用分隔符对相似命令进行分组。

![典型上下文菜单示例](images/controls_contextmenu_singlepane.png)

级联的上下文菜单可用于更全面的命令集。 它以多个浮出控件级别为特色，并且可以滚动。 根据需要使用分隔符对相似命令进行分组。

![级联的上下文菜单示例](images/controls_contextmenu_cascading.png)

这是一个全屏的单按钮确认对话框示例。 通过此类型的对话框，用户将看到应在按下按钮继续之前阅读的相当数量的信息。

![整页单按钮对话框示例](images/controls_dialog_singlebutton.png)

下面是为用户提供 A/B 选项的双按钮对话框示例。 通常，此对话框中显示的信息量十分简短。

![全按钮对话框示例](images/controls_dialog_twobutton.png)

## 模式和轻型消除

对话框是模式，这意味着它们会阻止所有与用户的交互，直到用户选择某个对话框按钮。 为了在视觉上强化其模式行为，对话框会绘制一个覆盖层，用于部分遮挡暂时无法访问的应用 UI。

**注意** 当“取消”是可用对话框选项之一时，应用可以选择让用户通过按 Escape 键消除对话框。 此行为不内置于该控件，但通常是所实现的快捷方式。

浮出控件和上下文菜单是轻型消除控件，这意味着用户可以从各种操作中进行选择以快速消除瞬态 UI。 这些交互预期为轻型并且非阻止性。 轻型消除操作包括
- 单击或点击瞬态 UI 之外
- 按 Esc 键
- 按“返回”按钮
- 调整应用窗口大小
- 更改设备方向


## 对话框使用指南

-   在对话框的第一行文本中清楚地标识问题或用户的目标。
-   对话框标题是主要说明并且是可选的。
    -   使用简短标题说明用户需要怎样处理对话框。 长标题不会换行而且将被截断。
    -   如果你使用对话框来传达简单的消息、错误或问题，则可以省略标题。 可依赖内容文本来传达这样的核心信息。
    -   确保标题与按钮选项直接相关。
-   对话框内容包含描述性文本，并且是必需的。
    -   提供尽可能简单的消息、错误或阻止问题。
    -   如果使用对话框标题，请使用内容区域提供更多详情或定义术语。 不要只是修改几个措词来重复标题。
-   必须至少显示一个对话框按钮。
    -   按钮是用户消除对话框的唯一机制。
    -   使用带有文本的按钮，该文本可标识对于主要说明或内容的响应。 例如，“你是否希望允许 AppName 访问你的位置”，后跟“允许”和“拒绝”按钮。 具体的响应可以使用户更快速的理解，以便进行高效的决策。
-   错误对话框在对话框中显示错误消息，以及任何相关的信息。 在错误对话框中使用的唯一按钮应为“关闭”或类似操作。
-   不要为与页面上的特定位置具有上下文关系的错误（例如，密码字段等位置的验证错误）使用对话框，请使用应用的画布本身显示内联错误。

## 浮出控件

浮出控件是可显示任意 UI 作为其内容的开放式容器。  浮出控件没有其自己的可视部分，它们只是内容控件。 浮出控件具有它们在其内容周围添加的边距和可选滚动栏。 若要设置浮出控件的样式，请修改其 [FlyoutPresenterStyle](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.flyout.flyoutpresenterstyle.aspx)。

以下代码显示一个环绕文本段落，并使屏幕阅读器可以访问该文本块。

````xaml
<Flyout>
  <Flyout.FlyoutPresenterStyle>
    <Style TargetType="FlyoutPresenter">
      <Setter Property="ScrollViewer.HorizontalScrollMode" Value="Disabled"/>
      <Setter Property="ScrollViewer.HorizontalScrollBarVisibility" Value="Disabled"/>
      <Setter Property="IsTabStop" Value="True"/>
      <Setter Property="TabNavigation" Value="Cycle"/>
    </Style>
  </Flyout.FlyoutPresenterStyle>
  <TextBlock Style="{StaticResource BodyTextBlockStyle}" Text="Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat."/>
</Flyout>
````

### 调用和放置

浮出控件和上下文菜单附加到特定控件。 当可见时，它们应固定到调用对象，并将其首选相对位置指定为对象：顶部、左侧、底部或右侧。 浮出控件还有一种完整放置模式，该模式尝试拉伸浮出控件，并使其在应用窗口内居中。

[Button 类](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.button.aspx)包含一个 [**Flyout**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.button.flyout.aspx) 属性，该属性可使你指定将在用户单击或点击按钮时打开的瞬态 UI。

````xaml
<Button Content="Click me">
  <Button.Flyout>
     <Flyout>
        <TextBlock Text="Yay!"/>
     </Flyout>
  </Button.Flyout>
</Button>
````


## 相关文章

**面向开发人员**
- [**MenuFlyout 类**](https://msdn.microsoft.com/library/windows/apps/dn299030)
- [**Flyout 类**](https://msdn.microsoft.com/library/windows/apps/dn279496)
- [**ContentDialog 类**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.contentdialog.aspx)



<!--HONumber=Aug16_HO3-->


