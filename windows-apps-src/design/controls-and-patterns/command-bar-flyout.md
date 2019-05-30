---
Description: 命令栏浮出控件向用户授予内联访问你的应用的最常见的任务。
title: 命令栏浮出控件
label: Command bar flyout
template: detail.hbs
ms.date: 10/02/2018
ms.topic: article
keywords: windows 10, uwp
pm-contact: abarlow
design-contact: ksulliv
dev-contact: llongley
doc-status: Draft
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: d5774b5301f7e8ce0616df72cfbf4fc81d0d0cf7
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/29/2019
ms.locfileid: "66363249"
---
# <a name="command-bar-flyout"></a>命令栏浮出控件

在命令栏浮出控件可以显示在浮动工具栏与 UI 画布上的元素相关的命令将用户提供轻松访问常见任务。

![展开的文本命令栏浮出控件](images/command-bar-flyout-header.png)

> CommandBarFlyout 需要 Windows 10，版本 1809年 ([SDK 17763](https://developer.microsoft.com/windows/downloads/windows-10-sdk)) 或更高版本，或[Windows 用户界面库](https://docs.microsoft.com/uwp/toolkits/winui/)。

> - **平台 Api**:[CommandBarFlyout 类](/uwp/api/windows.ui.xaml.controls.commandbarflyout)， [TextCommandBarFlyout 类](/uwp/api/windows.ui.xaml.controls.textcommandbarflyout)， [AppBarButton 类](/uwp/api/windows.ui.xaml.controls.appbarbutton)， [AppBarToggleButton 类](/uwp/api/windows.ui.xaml.controls.appbartogglebutton)， [AppBarSeparator 类](/uwp/api/windows.ui.xaml.controls.appbarseparator)
>- **Windows UI 库 Api**:[CommandBarFlyout 类](/uwp/api/microsoft.ui.xaml.controls.commandbarflyout)， [TextCommandBarFlyout 类](/uwp/api/microsoft.ui.xaml.controls.textcommandbarflyout)

像[CommandBar](app-bars.md)，CommandBarFlyout 已**PrimaryCommands**并**SecondaryCommands**属性可用于添加命令。 可以将命令放在集合中或两者。 何时以及如何显示主要和辅助命令取决于显示模式。

在命令栏浮出控件有两种显示模式：*折叠*并*展开*。

- 在折叠模式下，只有主命令会显示。 如果在命令栏浮出控件具有主和辅助命令，"查看更多"按钮，表示由省略号\[• • • 以\]，会显示。 这使用户可以通过转换为扩展模式获取辅助命令的访问。
- 在展开模式下，会显示这两个主要和辅助命令。 （如果该控件具有仅辅助项，它们会显示 MenuFlyout 控件相似的方式。）

## <a name="is-this-the-right-control"></a>这是正确的控件吗？

CommandBarFlyout 控件用于显示给用户，如按钮和菜单项，在应用画布上的元素的上下文中的命令的集合。

TextCommandBarFlyout TextBox、 TextBlock、 RichEditBox、 RichTextBlock 和 PasswordBox 控件中显示的文本命令。 命令自动正确配置到当前选定文本。 使用 CommandBarFlyout 替换文本控件的默认文本命令。

若要显示上下文列表项上的命令，请按照中的指导[集合和列表的上下文命令](collection-commanding.md)。

### <a name="commandbarflyout-vs-menuflyout"></a>CommandBarFlyout vs MenuFlyout

若要显示的上下文菜单中的命令，可以使用 CommandBarFlyout 或 MenuFlyout。 我们建议 CommandBarFlyout，因为它提供了比 MenuFlyout 更多的功能。 可以使用与仅辅助命令的 CommandBarFlyout 来获得的行为和查找的 MenuFlyout 或主要和辅助命令中使用完整的命令栏浮出控件。

> 有关相关信息，请参阅[浮出控件](../controls-and-patterns/dialogs-and-flyouts/flyouts.md)，[菜单和上下文菜单](menus.md)，并[命令栏](app-bars.md)。

## <a name="examples"></a>示例

<table>
<th align="left">XAML 控件库<th>
<tr>
<td><img src="images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>如果有<strong style="font-weight: semi-bold">XAML 控件库</strong>应用程序安装，请单击此处<a href="xamlcontrolsgallery:/item/CommandBarFlyout">打开应用，请参阅操作中的 CommandBarFlyout</a>。</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">获取 XAML 控件库应用 (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">获取源代码 (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="proactive-vs-reactive-invocation"></a>主动与被动调用

通常有两种方法来调用浮出控件或 UI 画布上的元素与关联的菜单：_主动调用_并_反应调用_。

在主动调用命令会在用户与命令关联的项交互时自动显示。 例如，文本格式设置命令可能会弹出当用户在文本框中选择文本。 在这种情况下，命令栏浮出控件才会焦点。 相反，它提供了接近与用户进行交互的项的相关命令。 如果用户不与命令进行交互，它们被否定。

在被动调用中，命令所示显式用户操作的响应来请求命令;例如，右键单击。 这对应于传统的概念[上下文菜单](menus.md)。

可以使用中的方法或甚至混合使用两个 CommandBarFlyout。

## <a name="create-a-command-bar-flyout"></a>创建命令栏浮出控件

此示例演示如何创建命令栏浮出控件和主动和被动都使用它。 点击该映像，浮出控件是显示在折叠模式下。 当显示为上下文菜单，浮出控件将显示在展开模式下。 在任一情况下，用户可以展开或折叠浮出控件，在打开后。

![折叠的命令栏浮出控件的示例](images/command-bar-flyout-img-collapsed.png)

> _折叠的命令栏浮出控件_

![扩展的命令栏浮出控件的示例](images/command-bar-flyout-img-expanded.png)

> _扩展的命令栏浮出控件_

```xaml
<Grid>
    <Grid.Resources>
        <CommandBarFlyout x:Name="ImageCommandsFlyout">
            <AppBarButton Icon="OutlineStar" ToolTipService.ToolTip="Favorite"/>
            <AppBarButton Icon="Copy" ToolTipService.ToolTip="Copy"/>
            <AppBarButton Icon="Share" ToolTipService.ToolTip="Share"/>
            <CommandBarFlyout.SecondaryCommands>
                <AppBarButton Label="Select all"/>
                <AppBarButton Label="Delete" Icon="Delete"/>
            </CommandBarFlyout.SecondaryCommands>
        </CommandBarFlyout>
    </Grid.Resources>

    <Image Source="Assets/image1.png" Width="300"
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

主动显示上下文命令时，只有主命令应显示默认情况下 （在命令栏浮出控件应已折叠）。 将最重要的命令放在主命令集合，并会传统上进入上下文菜单中的辅助命令集合的其他命令。

若要主动显示命令，通常处理[单击](/uwp/api/windows.ui.xaml.controls.primitives.buttonbase.click)或[点击](/uwp/api/windows.ui.xaml.uielement.tapped)事件显示在命令栏浮出控件。 设置在浮出控件[ShowMode](/uwp/api/windows.ui.xaml.controls.primitives.flyoutbase.showmode)到**暂时性**或**TransientWithDismissOnPointerMoveAway**在其折叠模式下打开浮出控件，而无需使焦点。

从 Windows 10 Insider Preview 开始，文本控件具有**SelectionFlyout**属性。 浮出控件分配给此属性后，自动显示选定文本。

### <a name="show-commands-reactively"></a>被动显示命令

被动，上下文菜单显示上下文命令，辅助命令 （命令栏浮出控件应已展开） 的默认情况下显示。 在这种情况下，命令栏浮出控件可能主要和辅助命令或仅辅助命令。

若要在上下文菜单中显示的命令，您通常分配的 flyout [ContextFlyout](/uwp/api/windows.ui.xaml.uielement.contextflyout) UI 元素的属性。 这样一来，打开浮出控件由该元素，并且无需执行任何其他操作。

如果你处理其中显示用户在浮出控件 (例如，在[RightTapped](/uwp/api/windows.ui.xaml.uielement.righttapped)事件)，设置浮出控件的[ShowMode](/uwp/api/windows.ui.xaml.controls.primitives.flyoutbase.showmode)到**标准**在其扩展模式下打开浮出控件和为其提供焦点。

> [!TIP]
> 有关显示浮出控件以及如何控制的 flyout 的位置时的选项的详细信息，请参阅[浮出控件](../controls-and-patterns/dialogs-and-flyouts/flyouts.md)。

## <a name="commands-and-content"></a>命令和内容

CommandBarFlyout 控件具有可用于添加命令和内容的 2 个属性：[PrimaryCommands](/uwp/api/windows.ui.xaml.controls.commandbarflyout.primarycommands)并[SecondaryCommands](/uwp/api/windows.ui.xaml.controls.commandbarflyout.secondarycommands)。

默认情况下，添加到命令栏中的项目也会添加到 **PrimaryCommands** 集合中。 这些命令的命令栏中所示，在折叠和展开模式中可见。 与不同的 CommandBar 中，主命令不会自动向辅助命令的溢出，并可能会被截断。

您还可以添加到命令**SecondaryCommands**集合。 辅助命令的控件的菜单部分中显示，并仅在展开模式中可见。

### <a name="app-bar-buttons"></a>应用栏按钮

您可以填充 PrimaryCommands 和直接与 SecondaryCommands [AppBarButton](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.AppBarButton)， [AppBarToggleButton](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.AppBarToggleButton)，并[AppBarSeparator](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.AppBarSeparator)控件。

应用栏按钮控件以一个图标和文本标签为特征。 这些控件进行优化以使用命令栏中，具体取决于是否在控件显示在命令栏或溢出菜单中更改其外观。

- 与仅其图标，在命令栏中显示应用栏按钮用作主要命令不显示的文本标签。 我们建议你使用工具提示显示该命令的文本说明如下所示。
    ```xaml
    <AppBarButton Icon="Copy" ToolTipService.ToolTip="Copy"/>
    ```
- 应用程序栏按钮用作辅助命令所示的菜单中，使用标签和可见的图标。

### <a name="other-content"></a>其他内容

通过将它们放置在 AppBarElementContainer，可以向命令栏浮出控件中添加其他控件。 这允许您添加的控件，如[DropDownButton](buttons.md)或[SplitButton](buttons.md)，或添加容器类似[StackPanel](buttons.md)创建更复杂的 UI。

要添加到命令栏浮出控件主要或辅助命令集合中，元素必须实现[ICommandBarElement](/uwp/api/windows.ui.xaml.controls.icommandbarelement)接口。 AppBarElementContainer 是实现此接口，因此可以将元素添加到命令栏中，即使它不实现接口本身的包装。

在这里，AppBarElementContainer 用于将额外的元素添加到命令栏浮出控件。 拆分按钮添加到的主要命令，以允许所选内容的颜色。 StackPanel 添加到辅助的命令，以允许更复杂的缩放控件的布局。

> [!TIP]
> 默认情况下，为应用画布设计元素看起来不太正确命令栏中。 当添加使用 AppBarElementContainer 对元素时，有将与其他命令栏元素相匹配的元素时应执行一些步骤：
>
> - 重写的默认画笔[轻型样式](/windows/uwp/design/controls-and-patterns/xaml-styles#lightweight-styling)以使该元素的背景和匹配应用程序栏按钮的边框。
> - 调整大小和元素的位置。
> - 在使用宽度和高度 16px Viewbox 中换行的图标。

> [!NOTE]
> 此示例演示仅在命令栏浮出控件 UI，它不实现任何显示的命令。 实现命令的详细信息，请参阅[按钮](buttons.md)并[命令设计基础知识](../basics/commanding-basics.md)。

![命令栏浮出控件使用的拆分按钮](images/command-bar-flyout-split-button.png)

> _折叠的命令栏浮出控件与打开的拆分按钮_

![命令栏浮出控件使用复杂的 UI](images/command-bar-flyout-custom-ui.png)

> _使用菜单中的自定义显示比例 UI 扩展的命令栏浮出控件_


```xaml
<CommandBarFlyout>
    <AppBarButton Icon="Cut" ToolTipService.ToolTip="Cut"/>
    <AppBarButton Icon="Copy" ToolTipService.ToolTip="Copy"/>
    <AppBarButton Icon="Paste" ToolTipService.ToolTip="Paste"/>
    <!-- Alignment controls -->
    <AppBarElementContainer>
        <SplitButton ToolTipService.ToolTip="Alignment">
            <SplitButton.Resources>
                <!-- Override default brushes to make the SplitButton 
                     match other command bar elements. -->
                <Style TargetType="SplitButton">
                    <Setter Property="Height" Value="38"/>
                </Style>
                <SolidColorBrush x:Key="SplitButtonBackground"
                                 Color="Transparent"/>
                <SolidColorBrush x:Key="SplitButtonBackgroundPressed"
                                 Color="{ThemeResource SystemListMediumColor}"/>
                <SolidColorBrush x:Key="SplitButtonBackgroundPointerOver"
                                 Color="{ThemeResource SystemListLowColor}"/>
                <SolidColorBrush x:Key="SplitButtonBorderBrush" Color="Transparent"/>
                <SolidColorBrush x:Key="SplitButtonBorderBrushPointerOver"
                                 Color="Transparent"/>
                <SolidColorBrush x:Key="SplitButtonBorderBrushChecked"
                                 Color="Transparent"/>
            </SplitButton.Resources>
            <SplitButton.Content>
                <Viewbox Width="16" Height="16" Margin="0,2,0,0">
                    <SymbolIcon Symbol="AlignLeft"/>
                </Viewbox>
            </SplitButton.Content>
            <SplitButton.Flyout>
                <MenuFlyout>
                    <MenuFlyoutItem Icon="AlignLeft" Text="Align left"/>
                    <MenuFlyoutItem Icon="AlignCenter" Text="Center"/>
                    <MenuFlyoutItem Icon="AlignRight" Text="Align right"/>
                </MenuFlyout>
            </SplitButton.Flyout>
        </SplitButton>
    </AppBarElementContainer>
    <!-- end Alignment controls -->
    <CommandBarFlyout.SecondaryCommands>
        <!-- Zoom controls -->
        <AppBarElementContainer>
            <AppBarElementContainer.Resources>
                <!-- Override default brushes to make the Buttons 
                     match other command bar elements. -->
                <SolidColorBrush x:Key="ButtonBackground"
                                 Color="Transparent"/>
                <SolidColorBrush x:Key="ButtonBackgroundPressed"
                                 Color="{ThemeResource SystemListMediumColor}"/>
                <SolidColorBrush x:Key="ButtonBackgroundPointerOver"
                                 Color="{ThemeResource SystemListLowColor}"/>
                <SolidColorBrush x:Key="ButtonBorderBrush"
                                 Color="Transparent"/>
                <SolidColorBrush x:Key="ButtonBorderBrushPointerOver"
                                 Color="Transparent"/>
                <SolidColorBrush x:Key="ButtonBorderBrushChecked"
                                 Color="Transparent"/>
                <Style TargetType="TextBlock">
                    <Setter Property="VerticalAlignment" Value="Center"/>
                </Style>
                <Style TargetType="Button">
                    <Setter Property="Height" Value="40"/>
                    <Setter Property="Width" Value="40"/>
                </Style>
            </AppBarElementContainer.Resources>
            <Grid Margin="12,-4">
                <Grid.ColumnDefinitions>
                    <ColumnDefinition Width="Auto"/>
                    <ColumnDefinition Width="76"/>
                    <ColumnDefinition Width="Auto"/>
                </Grid.ColumnDefinitions>
                <Viewbox Width="16" Height="16" Margin="0,2,0,0">
                    <SymbolIcon Symbol="Zoom"/>
                </Viewbox>
                <TextBlock Text="Zoom" Margin="10,0,0,0" Grid.Column="1"/>
                <StackPanel Orientation="Horizontal" Grid.Column="2">
                    <Button ToolTipService.ToolTip="Zoom out">
                        <Viewbox Width="16" Height="16">
                            <SymbolIcon Symbol="ZoomOut"/>
                        </Viewbox>
                    </Button>
                    <TextBlock Text="50%" Width="40"
                               HorizontalTextAlignment="Center"/>
                    <Button ToolTipService.ToolTip="Zoom in">
                        <Viewbox Width="16" Height="16">
                            <SymbolIcon Symbol="ZoomIn"/>
                        </Viewbox>
                    </Button>
                </StackPanel>
            </Grid>
        </AppBarElementContainer>
        <!-- end Zoom controls -->
        <AppBarSeparator/>
        <AppBarButton Label="Undo" Icon="Undo"/>
        <AppBarButton Label="Redo" Icon="Redo"/>
        <AppBarButton Label="Select all" Icon="SelectAll"/>
    </CommandBarFlyout.SecondaryCommands>
</CommandBarFlyout>
```

## <a name="create-a-context-menu-with-secondary-commands-only"></a>使用辅助命令仅创建一个上下文菜单

可以使用辅助命令仅为使用 CommandBarFlyout[上下文菜单](menus.md)，代替 MenuFlyout。

![仅辅助命令与命令栏浮出控件](images/command-bar-flyout-context-menu.png)

> _命令栏浮出控件为上下文菜单_

```xaml
<Grid>
    <Grid.Resources>
        <!-- A command bar flyout with only secondary commands. -->
        <CommandBarFlyout x:Name="ContextMenu">
            <CommandBarFlyout.SecondaryCommands>
                <AppBarButton Label="Copy" Icon="Copy"/>
                <AppBarButton Label="Save" Icon="Save"/>
                <AppBarButton Label="Print" Icon="Print"/>
                <AppBarSeparator />
                <AppBarButton Label="Properties"/>
            </CommandBarFlyout.SecondaryCommands>
        </CommandBarFlyout>
    </Grid.Resources>

    <Image Source="Assets/image1.png" Width="300"
           ContextFlyout="{x:Bind ContextMenu}"/>
</Grid>
```

此外可以使用与 DropDownButton CommandBarFlyout 创建标准菜单。

![命令栏浮出控件与作为下拉列表按钮菜单](images/command-bar-flyout-dropdown.png)

> _下拉列表按钮菜单命令栏浮出控件中_

```xaml
<CommandBarFlyout>
    <AppBarButton Icon="Placeholder"/>
    <AppBarElementContainer>
        <DropDownButton Content="Mail">
            <DropDownButton.Resources>
                <!-- Override default brushes to make the DropDownButton 
                     match other command bar elements. -->
                <Style TargetType="DropDownButton">
                    <Setter Property="Height" Value="38"/>
                </Style>
                <SolidColorBrush x:Key="ButtonBackground"
                                 Color="Transparent"/>
                <SolidColorBrush x:Key="ButtonBackgroundPressed"
                                 Color="{ThemeResource SystemListMediumColor}"/>
                <SolidColorBrush x:Key="ButtonBackgroundPointerOver"
                                 Color="{ThemeResource SystemListLowColor}"/>

                <SolidColorBrush x:Key="ButtonBorderBrush"
                                 Color="Transparent"/>
                <SolidColorBrush x:Key="ButtonBorderBrushPointerOver"
                                 Color="Transparent"/>
                <SolidColorBrush x:Key="ButtonBorderBrushChecked"
                                 Color="Transparent"/>
            </DropDownButton.Resources>
            <DropDownButton.Flyout>
                <CommandBarFlyout Placement="BottomEdgeAlignedLeft">
                    <CommandBarFlyout.SecondaryCommands>
                        <AppBarButton Icon="MailReply" Label="Reply"/>
                        <AppBarButton Icon="MailReplyAll" Label="Reply all"/>
                        <AppBarButton Icon="MailForward" Label="Forward"/>
                    </CommandBarFlyout.SecondaryCommands>
                </CommandBarFlyout>
            </DropDownButton.Flyout>
        </DropDownButton>
    </AppBarElementContainer>
    <AppBarButton Icon="Placeholder"/>
    <AppBarButton Icon="Placeholder"/>
</CommandBarFlyout>
```

## <a name="command-bar-flyouts-for-text-controls"></a>命令栏浮出控件的文本控件

[TextCommandBarFlyout](/uwp/api/microsoft.ui.xaml.controls.textcommandbarflyout)是专用的命令栏浮出控件包含编辑文本的命令。 每个文本控件显示 TextCommandBarFlyout 自动为上下文菜单 （右键单击），或选择文本。 文本命令栏浮出控件调整以仅显示相关命令的文本选定内容。

![折叠的文本命令栏浮出控件](images/command-bar-flyout-text-selection.png)

> _文本的所选文本命令栏浮出控件_

![展开的文本命令栏浮出控件](images/command-bar-flyout-text-full.png)

> _展开的文本命令栏浮出控件_


### <a name="available-commands"></a>可用的命令

此表显示了包含在 TextCommandBarFlyout，它们会在显示时的命令。

| Command | 所示... |
| ------- | -------- |
| Bold | 当文本控件不是只读的 (RichEditBox 仅)。 |
| Italic | 当文本控件不是只读的 (RichEditBox 仅)。 |
| Underline | 当文本控件不是只读的 (RichEditBox 仅)。 |
| 校对 | 何时 IsSpellCheckEnabled **，则返回 true**和拼写错误的文本选择。 |
| 剪切 | 当文本控件不是只读的和已选定文本。 |
| 复制 | 当选定文本。 |
| 粘贴 | 当文本控件不是只读的并且剪贴板包含的内容。 |
| 撤销 | 当没有可撤消的操作。 |
| 全选 | 何时可以选择文本。 |

### <a name="custom-text-command-bar-flyouts"></a>自定义文本命令栏浮出控件

TextCommandBarFlyout 不能自定义，并且每个文本控件将自动管理。 但是，可以将默认 TextCommandBarFlyout 为使用自定义命令。

- 若要替换默认文本所选内容显示的 TextCommandBarFlyout，可以创建自定义 CommandBarFlyout （或其他浮出控件类型），并将其分配给**SelectionFlyout**属性。 如果将为 SelectionFlyout **null**，没有命令显示所选内容上。
- 若要替换默认 TextCommandBarFlyout 显示为上下文菜单上，将分配自定义 CommandBarFlyout （或其他浮出控件类型） 到**ContextFlyout**文本控件上的属性。 如果 ContextFlyout 设置为**null**、 菜单浮出控件显示在上一版本中的文本的控件显示而不是 TextCommandBarFlyout。

## <a name="get-the-sample-code"></a>获取示例代码

- [XAML 控件库示例](https://github.com/Microsoft/Xaml-Controls-Gallery) - 以交互式格式查看所有 XAML 控件。
- [XAML 命令示例](https://go.microsoft.com/fwlink/p/?LinkId=620019)

## <a name="related-articles"></a>相关文章

- [适用于 UWP 应用的命令设计基础知识](../basics/commanding-basics.md)
- [CommandBar 类](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.CommandBar)
