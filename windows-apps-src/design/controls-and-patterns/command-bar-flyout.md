---
author: jwmsft
Description: Command bar flyouts give users inline access to your app's most common tasks.
title: 命令栏浮出控件
label: Command bar flyout
template: detail.hbs
ms.author: jimwalk
ms.date: 07/19/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
pm-contact: abarlow
design-contact: ksulliv
dev-contact: llongley
doc-status: Draft
ms.localizationpriority: medium
ms.openlocfilehash: ec532749fc2dacfc56e80ee2830da36f71c75b2f
ms.sourcegitcommit: f5321b525034e2b3af202709e9b942ad5557e193
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/18/2018
ms.locfileid: "4016319"
---
# <a name="command-bar-flyout"></a>命令栏浮出控件

> [!IMPORTANT]
> 本文介绍的功能尚未发布，在商业发行之前可能发生实质性修改。 Microsoft 对于此处提供的信息不作任何明示或默示的担保。 预览功能需要的[最新的 Windows 10 Insider Preview 版本和 SDK](https://insider.windows.com/for-developers/)或[Windows UI 库](https://docs.microsoft.com/uwp/toolkits/winui/)。

命令栏浮出控件可使你能够轻松访问常见任务的用户提供通过显示与你的 UI 画布上的元素相关的浮动工具栏中的命令。

![展开的文本命令栏浮出控件](images/command-bar-flyout-text-full.png)

> 相关信息，请参阅[浮出控件](../controls-and-patterns/dialogs-and-flyouts/flyouts.md)、[菜单和上下文菜单](menus.md)和[命令栏](app-bars.md)。

[命令栏](app-bars.md)，如 CommandBarFlyout 具有可用于添加命令的**PrimaryCommands**和**SecondaryCommands**属性。 你可以将命令放置在集合，或者两者。 何时以及如何显示主要和辅助命令取决于显示模式。

在命令栏浮出控件中有两个显示模式：*折叠*和*扩展*。

- 在折叠模式中，显示仅主要命令。 如果你的命令栏浮出控件具有主要和辅助命令，"查看更多"按钮，该按钮由省略号 \ [• • • \] 中，将显示。 这使用户可以通过过渡到扩展模式来获取对辅助命令的访问。
- 在扩展模式，显示主要和辅助命令。 （如果控件有只有辅助的项目，它们所示的 MenuFlyout 控件类似的方法。）

| **获取 Windows UI 库** |
| - |
| 此控件是 Windows UI 库，包含新控件和适用于 UWP 应用的 UI 功能的 NuGet 包的一部分。 有关详细信息，包括安装说明，请参阅[Windows UI 库概述](https://docs.microsoft.com/uwp/toolkits/winui/)。 |

| **平台 Api** | **Windows UI 库 Api** |
| - | - |
| [CommandBarFlyout 类](/uwp/api/windows.ui.xaml.controls.commandbarflyout)、 [TextCommandBarFlyout 类](/uwp/api/windows.ui.xaml.controls.textcommandbarflyout)、 [AppBarButton 类](/uwp/api/windows.ui.xaml.controls.appbarbutton)、 [AppBarToggleButton 类](/uwp/api/windows.ui.xaml.controls.appbartogglebutton)、 [AppBarSeparator 类](/uwp/api/windows.ui.xaml.controls.appbarseparator) | [CommandBarFlyout 类](/uwp/api/microsoft.ui.xaml.controls.commandbarflyout)， [TextCommandBarFlyout 类](/uwp/api/microsoft.ui.xaml.controls.textcommandbarflyout) |

## <a name="is-this-the-right-control"></a>这是正确的控件吗？

使用 CommandBarFlyout 控件来显示给用户，如按钮和菜单项，应用画布上的元素的上下文中的命令的集合。

TextCommandBarFlyout TextBox、 TextBlock、 RichEditBox、 RichTextBlock、 和 PasswordBox 的控件中显示文本的命令。 为当前选定文本，命令会自动适当配置。 使用 CommandBarFlyout 替换文本控件的默认文本命令。

若要显示上下文列表项上的命令，请遵循[命令用于集合和列表的上下文](collection-commanding.md)中的指南。

### <a name="commandbarflyout-vs-menuflyout"></a>CommandBarFlyout vs MenuFlyout

若要显示命令的上下文菜单中，你可以使用 CommandBarFlyout 或 MenuFlyout。 我们建议 CommandBarFlyout，因为它提供更多的功能比 MenuFlyout。 你可以使用 CommandBarFlyout 仅辅助命令来获取的行为和外观的 MenuFlyout，或使用主要和辅助命令的完整命令栏浮出控件。

## <a name="examples"></a>示例

<table>
<th align="left">XAML 控件库<th>
<tr>
<td><img src="images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>如果你安装了该<strong style="font-weight: semi-bold">XAML 控件库</strong>应用，单击此处<a href="xamlcontrolsgallery:/item/CommandBarFlyout">打开该应用，请参阅的实际 CommandBarFlyout</a>。</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">获取 XAML 控件库应用 (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics">获取源代码 (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="proactive-vs-reactive-invocation"></a>积极与被动的调用

通常有两种方法来调用浮出控件或菜单与 UI 画布上的元素相关联：_主动调用_和_被动的调用_。

在主动调用中，命令时自动显示与命令关联的项的用户交互。 例如，文本格式化命令可能会弹出当用户在文本框中选择文本。 在此情况下，命令栏浮出控件不会焦点。 相反，它显示靠近用户的交互时使用的项目的相关命令。 如果用户不会与命令交互，它们可以消除。

在被动调用中，命令显示在响应显式用户操作请求命令;例如，右键单击。 这对应于传统的[上下文菜单](menus.md)概念。

你可以使用中的方式，或者甚至是两个混合 CommandBarFlyout。

## <a name="create-a-command-bar-flyout"></a>创建命令栏浮出控件

> **预览**： CommandBarFlyout 需要的[最新的 Windows 10 Insider Preview 版本和 SDK](https://insider.windows.com/for-developers/)或[Windows UI 库](https://docs.microsoft.com/uwp/toolkits/winui/)。

此示例显示了如何创建命令栏浮出控件，并使用它主动和被动。 点击该图像时, 处于折叠模式下显示浮出控件。 显示为上下文菜单，浮出控件是显示在其扩展模式。 在任一情况下，用户可以展开或折叠浮出控件之后它已打开。

:::row:::
    :::column:::
        折叠的命令栏浮出控件<br/>
        ![折叠的命令栏浮出控件的示例](images/command-bar-flyout-img-collapsed.png)
    :::column-end:::
    :::column:::
        扩展的命令栏浮出控件<br/>
        ![扩展的命令栏浮出控件的示例](images/command-bar-flyout-img-expanded.png)
    :::column-end:::
:::row-end:::

```xaml
<Grid>
    <Grid.Resources>
        <CommandBarFlyout x:Name="ImageCommandsFlyout">
            <AppBarButton Icon="OutlineStar" ToolTipService.ToolTip="Favorite"/>
            <AppBarButton Icon="Copy" ToolTipService.ToolTip="Copy"/>
            <AppBarButton Icon="Share" ToolTipService.ToolTip="Share"/>
            <CommandBarFlyout.SecondaryCommands>
                <AppBarButton Label="Rotate" Icon="Rotate"/>
                <AppBarButton Label="Delete" Icon="Delete"/>
            </CommandBarFlyout.SecondaryCommands>
        </CommandBarFlyout>
    </Grid.Resources>

    <Image Source="Assets/licorice.png" Width="300"
           Tapped="Image_Tapped" FlyoutBase.AttachedFlyout="{x:Bind ImageCommandsFlyout}"
           ContextFlyout="{x:Bind ImageCommandsFlyout}"/>
</Grid>
```

```csharp
private void Image_Tapped(object sender, TappedRoutedEventArgs e)
{
    var flyout = FlyoutBase.GetAttachedFlyout((FrameworkElement)sender);
    var options = new FlyoutShowOptions()
    {
        // Position shows the flyout next to the pointer.
        // "Transient" ShowMode makes the flyout open in its collapsed state.
        Position = e.GetPosition((FrameworkElement)sender),
        ShowMode = FlyoutShowMode.Transient
    };
    flyout?.ShowAt((FrameworkElement)sender, options);
}
```

### <a name="show-commands-proactively"></a>主动显示命令

主动显示上下文命令时，仅主要命令应显示默认情况下 （命令栏浮出控件应处于折叠）。 将最重要的命令放置在主要命令集合中，将从传统上讲进入上下文菜单中的辅助命令集合的其他命令。

若要主动显示命令，通常处理[单击](/uwp/api/windows.ui.xaml.controls.primitives.buttonbase.click)或[Tapped](/uwp/api/windows.ui.xaml.uielement.tapped)事件，以显示在命令栏浮出控件。 设置浮出控件的[ShowMode](/uwp/api/windows.ui.xaml.controls.primitives.flyoutbase.showmode)为**瞬态**或**TransientWithDismissOnPointerMoveAway**以打开浮出控件处于折叠模式下无需使焦点。

从 Windows 10 Insider Preview 中，文本控件具有**SelectionFlyout**属性。 当你将在浮出控件分配给此属性时，它时自动显示文本处于选中状态。

### <a name="show-commands-reactively"></a>被动显示命令

被动，为上下文菜单显示上下文命令时，辅助命令显示默认情况下 （应扩展在命令栏浮出控件）。 在此情况下，命令栏浮出控件可能有主要和辅助命令或仅辅助命令。

以显示命令的上下文菜单中，你通常将浮出控件分配到 UI 元素的[ContextFlyout](/uwp/api/windows.ui.xaml.uielement.contextflyout)属性中。 这样一来，打开浮出控件由元素，并且你无需执行任何其他操作。

如果你处理自行显示浮出控件 （例如，在上一个[RightTapped](/uwp/api/windows.ui.xaml.uielement.righttapped)事件），则设置为**标准**处于扩展模式下打开浮出控件，并使其获得焦点的浮出控件的[ShowMode](/uwp/api/windows.ui.xaml.controls.primitives.flyoutbase.showmode) 。

> [!TIP]
> 有关选项时显示浮出控件和如何控制放置的浮出控件的详细信息，请参阅[浮出控件](../controls-and-patterns/dialogs-and-flyouts/flyouts.md)。

## <a name="commands-and-content"></a>命令和内容

CommandBarFlyout 控件具有可用于添加命令和内容的 2 个属性： [PrimaryCommands](/uwp/api/windows.ui.xaml.controls.commandbarflyout.primarycommands)和[SecondaryCommands](/uwp/api/windows.ui.xaml.controls.commandbarflyout.secondarycommands)。

默认情况下，添加到命令栏中的项目也会添加到 **PrimaryCommands** 集合中。 这些命令显示在命令栏中，在折叠和扩展模式中可见。 命令栏，与主要命令不自动溢出辅助命令，以及可能会被截断。

你还可以将命令添加到**SecondaryCommands**集合。 辅助命令显示在控件的菜单部分中，仅在扩展模式中可见。

### <a name="app-bar-buttons"></a>应用栏按钮

你可以直接使用[AppBarButton](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.appbarbutton.aspx)、 [AppBarToggleButton](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.appbartogglebutton.aspx)和[AppBarSeparator](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.appbarseparator.aspx)控件填充 PrimaryCommands 和 SecondaryCommands。

应用栏按钮控件以一个图标和文本标签为特征。 在命令栏中使用优化这些控件，具体取决于是否在命令栏中还是溢出菜单中显示控件来更改其外观。

- 仅其图标; 在命令栏中显示应用栏按钮用作主要命令不显示的文本标签。 我们建议你使用工具提示显示的命令，一个文本描述如下所示。
    ```xaml
    <AppBarButton Icon="Copy" ToolTipService.ToolTip="Copy"/>
    ```
- 应用栏按钮用作辅助命令显示在菜单中，使用标签和可见的图标。

### <a name="other-content"></a>其他内容

它们包装在 AppBarElementContainer 中，可以将其他控件添加到命令栏浮出控件。 这允许你添加控件如[DropDownButton]()或[拆分按钮]()，或添加如[StackPanel]()创建更复杂的 UI 的容器。

> [!NOTE]
> 若要添加到命令栏浮出控件的主要或辅助命令集合，元素必须实现[ICommandBarElement](/uwp/api/windows.ui.xaml.controls.icommandbarelement)接口。 AppBarElementContainer 是包装器实现此接口，以便你可以将某个元素添加到命令栏，即使它不会实现接口本身。

在这里，AppBarElementContainer 用于将额外的元素添加到命令栏浮出控件。 拆分按钮添加到主要命令以允许选择的颜色。 StackPanel 将添加到允许的缩放控件具有更复杂的布局的辅助命令。

> [!NOTE]
> 此示例显示仅在命令栏浮出控件的 UI，它不实现的任何命令所示。 有关实现这些命令的详细信息，请参阅[按钮](buttons.md)和[命令设计基础知识](../basics/commanding-basics.md)。

:::row:::
    :::column:::
        打开拆分按钮带有折叠的命令栏浮出控件<br/>
        ![命令栏浮出控件与拆分按钮](images/command-bar-flyout-split-button.png)
    :::column-end:::
    :::column:::
        在菜单中的自定义缩放 UI 与扩展的命令栏浮出控件<br/>
        ![命令栏浮出控件与复杂的 UI](images/command-bar-flyout-complex-ui.png)
    :::column-end:::
:::row-end:::

```xaml
<CommandBarFlyout>
    <AppBarButton Icon="Cut" ToolTipService.ToolTip="Cut"/>
    <AppBarButton Icon="Copy" ToolTipService.ToolTip="Copy"/>
    <AppBarButton Icon="Paste" ToolTipService.ToolTip="Paste"/>
    <!-- Color controls -->
    <AppBarElementContainer>
        <SplitButton Height="Auto" Margin="0,4,0,0"
                     ToolTipService.ToolTip="Colors"
                     Background="{ThemeResource AppBarItemBackgroundThemeBrush}">
            <SplitButton.Content>
                <Rectangle Width="20" Height="20">
                    <Rectangle.Fill>
                        <SolidColorBrush Color="Red"/>
                    </Rectangle.Fill>
                </Rectangle>
            </SplitButton.Content>
            <SplitButton.Flyout>
                <MenuFlyout>
                    <MenuFlyoutItem Text="Red"/>
                    <MenuFlyoutItem Text="Yellow"/>
                    <MenuFlyoutItem Text="Green"/>
                    <MenuFlyoutItem Text="Blue"/>
                </MenuFlyout>
            </SplitButton.Flyout>
        </SplitButton>
    </AppBarElementContainer>
    <!-- end Color controls -->
    <CommandBarFlyout.SecondaryCommands>
        <!-- Zoom controls -->
        <AppBarElementContainer>
            <AppBarElementContainer.Resources>
                <Style TargetType="Button">
                    <Setter Property="Background"
                            Value="{ThemeResource AppBarItemBackgroundThemeBrush}"/>
                </Style>
                <Style TargetType="TextBlock">
                    <Setter Property="VerticalAlignment" Value="Center"/>
                </Style>
            </AppBarElementContainer.Resources>
            <Grid Margin="12,0">
                <Grid.ColumnDefinitions>
                    <ColumnDefinition Width="86"/>
                    <ColumnDefinition Width="Auto"/>
                </Grid.ColumnDefinitions>
                <TextBlock Text="Zoom"/>
                <StackPanel Orientation="Horizontal" Grid.Column="1">
                    <Button>
                        <SymbolIcon Symbol="Remove"/>
                    </Button>
                    <TextBlock Text="50%" Width="40"
                               HorizontalTextAlignment="Center"/>
                    <Button>
                        <SymbolIcon Symbol="Add"/>
                    </Button>
                </StackPanel>
            </Grid>
        </AppBarElementContainer>
        <!-- end Zoom controls -->
        <AppBarSeparator/>
        <AppBarButton Label="Undo" Icon="Undo"/>
        <AppBarButton Label="Redo" Icon="Redo"/>
        <AppBarButton Label="Select all"/>
    </CommandBarFlyout.SecondaryCommands>
</CommandBarFlyout>
```

## <a name="create-a-context-menu-with-secondary-commands-only"></a>使用辅助命令仅创建上下文菜单

你可以使用 CommandBarFlyout 使用仅辅助命令作为[上下文菜单](menus.md)，MenuFlyout 代替。

![使用辅助命令仅在命令栏浮出控件](images/command-bar-flyout-context-menu.png)

```xaml
<Grid>
    <Grid.Resources>
        <!-- A command bar flyout with only secondary commands. -->
        <CommandBarFlyout x:Name="ContextMenu">
            <CommandBarFlyout.SecondaryCommands>
                <AppBarButton Label="Pin" Icon="Pin"/>
                <AppBarButton Label="Unpin" Icon="UnPin"/>
                <AppBarButton Label="Copy" Icon="Copy"/>
                <AppBarSeparator />
                <AppBarButton Label="Properties"/>
            </CommandBarFlyout.SecondaryCommands>
        </CommandBarFlyout>
    </Grid.Resources>

    <Image Source="Assets/licorice.png" Width="300"
           ContextFlyout="{x:Bind ContextMenu}"/>
</Grid>
```

你还可以使用与 DropDownButton CommandBarFlyout 创建标准菜单。

![命令栏浮出控件与作为按钮下拉菜单](images/command-bar-flyout-button-menu.png)

```xaml
<DropDownButton Content="Mail">
    <DropDownButton.Flyout>
        <CommandBarFlyout Placement="BottomEdgeAlignedLeft">
            <CommandBarFlyout.SecondaryCommands>
                <AppBarButton Icon="MailForward" Label="Forward"/>
                <AppBarButton Icon="MailReply" Label="Reply"/>
                <AppBarButton Icon="MailReplyAll" Label="Reply all"/>
            </CommandBarFlyout.SecondaryCommands>
        </CommandBarFlyout>
    </DropDownButton.Flyout>
</DropDownButton>
```

## <a name="command-bar-flyouts-for-text-controls"></a>命令栏浮出控件对于文本控件

[TextCommandBarFlyout](/uwp/api/microsoft.ui.xaml.controls.textcommandbarflyout)是专用的命令栏浮出控件，其中包含正在编辑的文本的命令。 为上下文菜单 （右键单击），或选择文本时，每个文本控件将自动显示 TextCommandBarFlyout。 文本命令栏浮出控件适应文本选择，以仅显示相关的命令。

:::row:::
    :::column:::
        文本选择文本命令栏浮出控件<br/>
        ![折叠的文本命令栏浮出控件](images/command-bar-flyout-text-selection.png)
    :::column-end:::
    :::column:::
        展开的文本命令栏浮出控件<br/>
        ![展开的文本命令栏浮出控件](images/command-bar-flyout-text-full.png)
    :::column-end:::
:::row-end:::

### <a name="available-commands"></a>可用的命令

此表显示包含 TextCommandBarFlyout，而且当它们显示的命令。

| 命令 | 显示... |
| ------- | -------- |
| 加粗 | 当文本控件不是只读的 (RichEditBox 仅)。 |
| 倾斜 | 当文本控件不是只读的 (RichEditBox 仅)。 |
| 加下划线 | 当文本控件不是只读的 (RichEditBox 仅)。 |
| 篡改 | 当 IsSpellCheckEnabled 为**true** ，并且拼写错误文本处于选中状态。 |
| 剪切 | 当文本控件不是只读的和文本处于选中状态。 |
| 复制 | 当选择文本。 |
| 粘帖 | 当文本控件不是只读的并且剪贴板包含的内容。 |
| 撤销 | 可以撤消的操作时。 |
| 全选 | 当可以选择文本。 |

### <a name="custom-text-command-bar-flyouts"></a>自定义文本命令栏浮出控件

TextCommandBarFlyout 不能自定义，并自动由每个文本控件。 但是，你可以使用自定义命令替换默认 TextCommandBarFlyout。

- 若要替换默认在文本选择时显示的 TextCommandBarFlyout，可以创建自定义 CommandBarFlyout （或其他浮出控件类型），并将其分配到**SelectionFlyout**属性。 将 SelectionFlyout 设置为**null**，如果没有命令显示在所选内容。
- 若要替换默认显示为上下文菜单的 TextCommandBarFlyout，将在文本控件上的**ContextFlyout**属性自定义 CommandBarFlyout （或其他浮出控件类型）。 如果将 ContextFlyout 设置为**null**，而不是 TextCommandBarFlyout 显示菜单浮出控件显示在以前版本的文本控件。

## <a name="get-the-sample-code"></a>获取示例代码

- [XAML 控件库示例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics) - 以交互式格式查看所有 XAML 控件。
- [XAML 命令示例](http://go.microsoft.com/fwlink/p/?LinkId=620019)

## <a name="related-articles"></a>相关文章

- [UWP 应用的命令设计基础知识](../basics/commanding-basics.md)
- [CommandBar 类](https://msdn.microsoft.com/library/windows/apps/dn279427)
