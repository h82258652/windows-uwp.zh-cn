---
Description: 命令栏浮出控件使用户能够以内联方式访问应用的最常见任务。
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
ms.openlocfilehash: 9388df4159d7e9acd68c75163465339183b41314
ms.sourcegitcommit: 0dee502484df798a0595ac1fe7fb7d0f5a982821
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/08/2020
ms.locfileid: "82968792"
---
# <a name="command-bar-flyout"></a>命令栏浮出控件

使用命令栏浮出控件时，可以在浮动工具栏中显示与 UI 画布上的某个元素相关的命令，方便用户访问常见任务。

![扩展的文本命令栏浮出控件](images/command-bar-flyout-header.png)

与 [CommandBar](app-bars.md) 一样，CommandBarFlyout 的 **PrimaryCommands** 和 **SecondaryCommands** 属性可以用来添加命令。 可以将命令置于这两个集合中，或者置于其中的一个中。 主要命令和辅助命令何时显示以及以何种方式显示取决于显示模式。

命令栏浮出控件有两个显示模式：折叠和展开。  

- 在折叠模式下，仅显示主要命令。 如果命令栏浮出控件包含主要命令和辅助命令，则会显示由省略号 \[***\] 表示的“查看更多”按钮。 这样用户就可以通过切换到展开模式来访问辅助命令。
- 在展开模式下，主要命令和辅助命令都会显示。 （如果此控件只有辅助项，则这些辅助项的显示方式类似于 MenuFlyout 控件。）

**获取 Windows UI 库**

|  |  |
| - | - |
| ![WinUI 徽标](images/winui-logo-64x64.png) |  CommandBarFlyout 控件作为 Windows UI 库的一部分提供，该库是一个 NuGet 包，其中包含用于 Windows 应用的新控件和 UI 功能。 有关详细信息（包括安装说明），请参阅 [Windows UI 库](https://docs.microsoft.com/uwp/toolkits/winui/)。 |

>**Windows UI 库 API**：[CommandBarFlyout 类](/uwp/api/microsoft.ui.xaml.controls.commandbarflyout)、[TextCommandBarFlyout 类](/uwp/api/microsoft.ui.xaml.controls.textcommandbarflyout)
>
>**平台 API**：[CommandBarFlyout 类](/uwp/api/windows.ui.xaml.controls.commandbarflyout)、[TextCommandBarFlyout 类](/uwp/api/windows.ui.xaml.controls.textcommandbarflyout)、[AppBarButton 类](/uwp/api/windows.ui.xaml.controls.appbarbutton)、[AppBarToggleButton 类](/uwp/api/windows.ui.xaml.controls.appbartogglebutton)、[AppBarSeparator 类](/uwp/api/windows.ui.xaml.controls.appbarseparator)
>
> CommandBarFlyout 需要 Windows 10 版本 1809（[SDK 17763](https://developer.microsoft.com/windows/downloads/windows-10-sdk)）或更高版本，或 [Windows UI 库](https://docs.microsoft.com/uwp/toolkits/winui/)。

## <a name="is-this-the-right-control"></a>这是正确的控件吗？

使用 CommandBarFlyout 控件，可以在应用画布的元素上下文中为用户显示一系列命令，例如按钮和菜单项。

TextCommandBarFlyout 在 TextBox、TextBlock、RichEditBox、RichTextBlock 和 PasswordBox 控件中显示文本命令。 系统会根据当前的文本选择情况自动配置这些命令。 使用 CommandBarFlyout 可以替换文本控件上的默认文本命令。

若要在列表项上显示上下文命令，请按[用于集合和列表的上下文命令](collection-commanding.md)中的指南操作。

### <a name="commandbarflyout-vs-menuflyout"></a>CommandBarFlyout 与 MenuFlyout

若要在上下文菜单中显示命令，可以使用 CommandBarFlyout 或 MenuFlyout。 建议使用 CommandBarFlyout，因为它提供的功能比 MenuFlyout 多。 可以将 CommandBarFlyout 仅仅与辅助命令配合使用以获取 MenuFlyout 的行为和外观，也可以将整个命令栏浮出控件与主要命令和辅助命令配合使用。

> 如需相关信息，请参阅[浮出控件](../controls-and-patterns/dialogs-and-flyouts/flyouts.md)、[菜单和上下文菜单](menus.md)和[命令栏](app-bars.md)。

## <a name="examples"></a>示例

<table>
<th align="left">XAML 控件库<th>
<tr>
<td><img src="images/xaml-controls-gallery-app-icon-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>如果已安装 <strong style="font-weight: semi-bold">XAML 控件库</strong>应用，请单击此处<a href="xamlcontrolsgallery:/item/CommandBarFlyout">打开此应用，了解 CommandBarFlyout 的实际应用</a>。</p>
    <ul>
    <li><a href="https://www.microsoft.com/p/xaml-controls-gallery/9msvh128x2zt">获取 XAML 控件库应用 (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">获取源代码 (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="proactive-vs-reactive-invocation"></a>主动调用与被动调用

通常可以通过两种方式来调用与 UI 画布上的某个元素相关联的浮出控件或菜单：主动调用和被动调用。  

在主动调用中，当用户与项交互时，与该项关联的命令会自动显示。 例如，当用户选择文本框中的文本时，文本格式设置命令可能会弹出。 在这种情况下，命令栏浮出控件不占据焦点位置， 而是为与用户进行交互的项呈现相关命令。 如果用户不与这些命令交互，这些命令会被取消。

在被动调用中，只有在用户通过显式操作（例如右键单击）来请求命令时，命令才会显示。 这对应于传统的[上下文菜单](menus.md)概念。

可以通过任一方式使用 CommandBarFlyout，甚至可以将这两种方式混合使用。

## <a name="create-a-command-bar-flyout"></a>创建命令栏浮出控件

以下示例介绍了如何创建命令栏浮出控件并以主动和被动两种方式使用它。 点击图像时，浮出控件以折叠模式显示。 作为上下文菜单显示时，浮出控件以展开模式显示。 不管哪一种情况，用户都可以在浮出控件打开后将其展开或折叠。

![折叠的命令栏浮出控件示例](images/command-bar-flyout-img-collapsed.png)

> 折叠的命令栏浮出控件 

![展开的命令栏浮出控件示例](images/command-bar-flyout-img-expanded.png)

> 展开的命令栏浮出控件 

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

主动显示上下文命令时，默认情况下只应显示主要命令（命令栏浮出控件应该折叠）。 请将最重要的命令置于主要命令集合中，将其他会按传统方式进入上下文菜单中的命令置于辅助命令集合中。

若要主动显示命令，通常需要通过处理 [Click](/uwp/api/windows.ui.xaml.controls.primitives.buttonbase.click) 或 [Tapped](/uwp/api/windows.ui.xaml.uielement.tapped) 事件来显示命令栏浮出控件。 将浮出控件的 [ShowMode](/uwp/api/windows.ui.xaml.controls.primitives.flyoutbase.showmode) 设置为 **Transient** 或 **TransientWithDismissOnPointerMoveAway** 可以在折叠模式下打开浮出控件，不获取焦点。

从 Windows 10 Insider Preview 开始，文本控件有一个 **SelectionFlyout** 属性。 为此属性分配浮出控件后，当文本处于选中状态时，该控件就会自动显示。

### <a name="show-commands-reactively"></a>被动显示命令

被动显示上下文命令时，默认情况下会显示充当上下文菜单的辅助命令（命令栏浮出控件应该展开）。 在这种情况下，命令栏浮出控件可能有主要命令和辅助命令，也可能只有辅助命令。

若要在上下文菜单中显示命令，通常需将浮出控件分配给 UI 元素的 [ContextFlyout](/uwp/api/windows.ui.xaml.uielement.contextflyout) 属性。 这样就可以由该元素负责打开浮出控件，你不需执行任何其他操作。

如果你自己负责显示浮出控件（例如，在出现 [RightTapped](/uwp/api/windows.ui.xaml.uielement.righttapped) 事件时这样做），请将浮出控件的 [ShowMode](/uwp/api/windows.ui.xaml.controls.primitives.flyoutbase.showmode) 设置为 **Standard**，以便在展开模式下打开浮出控件并为其提供焦点。

> [!TIP]
> 若要详细了解显示浮出控件时的选项以及如何控制浮出控件的放置，请参阅[浮出控件](../controls-and-patterns/dialogs-and-flyouts/flyouts.md)。

## <a name="commands-and-content"></a>命令和内容

CommandBarFlyout 控件有 2 个可用于添加命令和内容的属性：[PrimaryCommands](/uwp/api/windows.ui.xaml.controls.commandbarflyout.primarycommands) 和 [SecondaryCommands](/uwp/api/windows.ui.xaml.controls.commandbarflyout.secondarycommands)。

默认情况下，添加到命令栏中的项目也会添加到 PrimaryCommands 集合中  。 这些命令显示在命令栏中，在折叠模式和展开模式下均可见。 与 CommandBar 不同，主要命令不会自动溢出到辅助命令，可能会被截断。

也可将命令添加到 SecondaryCommands 集合中  。 辅助命令显示在控件的菜单部分，仅在展开模式下可见。

### <a name="app-bar-buttons"></a>应用栏按钮

可以直接使用 [AppBarButton](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.AppBarButton)、[AppBarToggleButton](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.AppBarToggleButton) 和 [AppBarSeparator](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.AppBarSeparator) 控件填充 PrimaryCommands 和 SecondaryCommands。

应用栏按钮控件以一个图标和文本标签为特征。 这些控件经优化后适合在命令栏中使用，其外观会变化，具体取决于控件是显示在命令栏中还是显示在溢出菜单中。

- 用作主要命令的应用栏按钮在应用栏中显示时只有其图标；文本标签未显示。 建议使用工具提示来显示命令的文本说明，如下所示。
    ```xaml
    <AppBarButton Icon="Copy" ToolTipService.ToolTip="Copy"/>
    ```
- 用作辅助命令的应用栏按钮显示在菜单中，标签和图标均可见。

### <a name="other-content"></a>其他内容

可以将其他控件添加到命令栏浮出控件中，只需将它们包装在 AppBarElementContainer 中即可。 这样就可以添加 [DropDownButton](buttons.md) 或 [SplitButton](buttons.md) 之类的控件，或者添加 [StackPanel](buttons.md) 之类的容器，以便创建更复杂的 UI。

元素必须实现 [ICommandBarElement](/uwp/api/windows.ui.xaml.controls.icommandbarelement) 接口才能添加到命令栏浮出控件的主要命令或辅助命令集合中。 AppBarElementContainer 是一个实现此接口的包装器，因此即使某个元素不自行实现此接口，也可将该元素添加到命令栏。

在这里，使用了 AppBarElementContainer 将额外元素添加到命令栏浮出控件。 为主要命令添加了 SplitButton，用于选择颜色。 为辅助命令添加了 StackPanel，以便为缩放控件实现更复杂的布局。

> [!TIP]
> 默认情况下，为应用画布设计的元素可能会在应用栏中显示不正常。 使用 AppBarElementContainer 添加某个元素时，应该执行某些步骤，使该元素与其他命令栏元素匹配：
>
> - 使用[轻型样式设置](/windows/uwp/design/controls-and-patterns/xaml-styles#lightweight-styling)覆盖默认的画笔，使该元素的背景和边框与应用栏按钮匹配。
> - 调整元素的大小和位置。
> - 将图标包装在宽度和高度为 16px 的 Viewbox 中。

> [!NOTE]
> 此示例仅显示命令栏浮出控件 UI，并未实现任何显示的命令。 若要详细了解如何实现这些命令，请参阅[按钮](buttons.md)和[命令设计基础知识](../basics/commanding-basics.md)。

![带拆分按钮的命令栏浮出控件](images/command-bar-flyout-split-button.png)

> SplitButton 已打开的折叠的命令栏浮出控件 

![带复杂 UI 的命令栏浮出控件](images/command-bar-flyout-custom-ui.png)

> 菜单中包含自定义缩放 UI 的展开的命令栏浮出控件 


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

## <a name="create-a-context-menu-with-secondary-commands-only"></a>创建仅包含辅助命令的上下文菜单

可以将仅包含辅助命令的 CommandBarFlyout 用作[上下文菜单](menus.md)，代替 MenuFlyout。

![仅包含辅助命令的命令栏浮出控件](images/command-bar-flyout-context-menu.png)

> 充当上下文菜单的命令栏浮出控件 

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

也可将 CommandBarFlyout 与 DropDownButton 配合使用来创建标准菜单。

![带下拉按钮菜单的命令栏浮出控件](images/command-bar-flyout-dropdown.png)

> 命令栏浮出控件中的下拉按钮菜单 

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

## <a name="command-bar-flyouts-for-text-controls"></a>用于文本控件的命令栏浮出控件

[TextCommandBarFlyout](/uwp/api/microsoft.ui.xaml.controls.textcommandbarflyout) 是专用的命令栏浮出控件，其中包含用于编辑文本的命令。 每个文本控件会自动将 TextCommandBarFlyout 显示为上下文菜单（右键单击），或者会在文本处于选中状态时显示它。 文本命令栏浮出控件会根据文本选择情况进行调整，仅显示相关命令。

![折叠的文本命令栏浮出控件](images/command-bar-flyout-text-selection.png)

> 在选中文本后出现的文本命令栏浮出控件 

![扩展的文本命令栏浮出控件](images/command-bar-flyout-text-full.png)

> 扩展的文本命令栏浮出控件 


### <a name="available-commands"></a>可用命令

下表介绍了包含在 TextCommandBarFlyout 中的命令，以及这些命令何时显示。

| 命令 | 在以下情况下显示... |
| ------- | -------- |
| 粗体 | 当文本控件不是只读时（仅 RichEditBox）。 |
| 斜体 | 当文本控件不是只读时（仅 RichEditBox）。 |
| 下划线 | 当文本控件不是只读时（仅 RichEditBox）。 |
| 校对 | 当 IsSpellCheckEnabled 为 **true** 且拼写错误的文本处于选中状态时。 |
| 剪切 | 当文本控件不是只读且文本处于选中状态时。 |
| 复制 | 当文本处于选中状态时。 |
| 粘贴 | 当文本控件不是只读且剪贴板有内容时。 |
| 撤消 | 当存在可以撤消的操作时。 |
| 全选 | 当文本可以选择时。 |

### <a name="custom-text-command-bar-flyouts"></a>自定义文本命令栏浮出控件

TextCommandBarFlyout 不能自定义，由每个文本控件自动管理。 不过，可以将默认的 TextCommandBarFlyout 替换为自定义命令。

- 若要替换在选中文本时会显示的默认 TextCommandBarFlyout，可以创建一个自定义 CommandBarFlyout（或其他浮出控件类型）并将其分配给 **SelectionFlyout** 属性。 如果将 SelectionFlyout 设置为 **null**，则在选中文本时不显示任何命令。
- 若要替换默认的作为上下文菜单显示的 TextCommandBarFlyout，请将自定义 CommandBarFlyout（或其他浮出控件类型）分配给文本控件上的 **ContextFlyout** 属性。 如果将 ContextFlyout 设置为 **null**，则会显示在旧版文本控件中显示的菜单浮出控件，代替 TextCommandBarFlyout。

## <a name="get-the-sample-code"></a>获取示例代码

- [XAML 控件库示例](https://github.com/Microsoft/Xaml-Controls-Gallery) - 以交互式格式查看所有 XAML 控件。
- [XAML 命令示例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlCommanding)

## <a name="related-articles"></a>相关文章

- [Windows 应用的命令设计基础知识](../basics/commanding-basics.md)
- [CommandBar 类](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.CommandBar)
