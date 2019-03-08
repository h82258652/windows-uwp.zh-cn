---
title: 命令在通用 Windows 平台 (UWP) 应用程序
description: 如何使用 XamlUICommand 和 StandardUICommand 类 （以及 ICommand 接口） 来共享和管理命令的各种控件类型，而不考虑设备和所使用的输入的类型。
author: Karl-Bridge-Microsoft
ms.service: ''
ms.topic: overview
ms.date: 11/01/2018
ms.author: kbridge
ms.openlocfilehash: 32d5005f9965b14d5080344832eb185f0e711689
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2019
ms.locfileid: "57646522"
---
# <a name="commanding-in-universal-windows-platform-uwp-apps-using-standarduicommand-xamluicommand-and-icommand"></a>命令中使用 StandardUICommand、 XamlUICommand 和 ICommand 的通用 Windows 平台 (UWP) 应用

在本主题中，我们将介绍在通用 Windows 平台 (UWP) 应用程序中发出命令。 具体而言，我们将讨论如何使用[XamlUICommand](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.xamluicommand)并[StandardUICommand](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.standarduicommand)类 （以及 ICommand 接口） 来共享和跨不同的控件类型，而不考虑管理命令正在使用的设备和输入的类型。

![关系图表示共享命令的常见用法： 多个 UI 显示与收藏夹命令](images/commanding/generic-commanding.png)

*在各种控件，而不考虑设备和输入的类型之间共享命令*

## <a name="important-apis"></a>重要的 API

- [Windows.UI.Xaml.Input.ICommand](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.icommand)和[System.Windows.Input.ICommand](https://docs.microsoft.com/dotnet/api/system.windows.input.icommand)
- [XamlUICommand](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.xamluicommand)
- [StandardUICommand](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.standarduicommand)

## <a name="overview"></a>概述

<!-- See https://blogs.msdn.microsoft.com/jebarson/2017/07/26/writing-an-asynchronous-relaycommand-implementing-icommand/ -->

<!-- A command describes an action but not its implementation (in other words, the what, but not the how). For example, the "Paste" command indicates that the user wants to copy something from the clipboard to a target control, but does not specify how. -->

可以直接通过如单击按钮或从上下文菜单中选择项 UI 交互调用命令。 它们也可以调用间接通过如键盘快捷键、 手势，语音识别或自动化/可访问性工具的输入设备。 在调用之后，该命令可以随后处理由控件 （在编辑控件中的文本导航），窗口 （后退导航） 或应用程序 （退出）。

命令可以对在应用中，删除文本或撤消操作，例如特定的上下文或它们可以是无上下文的例如音频静音或调整亮度。

下图显示了两个命令接口 ( [CommandBar](app-bars.md)和上下文的浮点[CommandBarFlyout](command-bar-flyout.md)) 的共享许多相同的命令。

![命令界面示例](images/commanding/command-interface-example.png)

## <a name="command-interactions"></a>命令的交互

由于各种设备、 输入的类型和 UI 图面，可能会影响如何调用命令，我们建议尽可能公开任意多个命令显示通过你的命令。 其中可包括的组合[轻扫](swipe.md)，[菜单栏](menus.md)， [CommandBar](app-bars.md)， [CommandBarFlyout](command-bar-flyout.md)，和传统[上下文菜单](menus.md)。

**对于关键命令，使用特定于输入的加速器。** 输入的加速器允许用户执行操作更快地根据它们所使用的输入设备。

下面是一些常见的输入的加速器为各种输入类型：

- **指针**-鼠标 （& p） 将鼠标悬停在按钮
- **键盘**-快捷方式 （访问键和加速键）
- **触摸**-轻扫
- **触摸**-下拉以刷新数据

您必须考虑的输入的类型和用户体验，要使应用程序的功能的多种方式来访问。 例如，集合 （尤其是用户可编辑的） 通常包括各种具体取决于输入设备方式大不相同执行的特定命令。

下表显示了一些典型集合命令以及提供这些命令的方法。

| 命令          | 与输入无关 | 鼠标快捷方式 | 键盘快捷方式 | 触控快捷方式 |
| ---------------- | -------------- | ----------------- | -------------------- | ----------------- |
| 删除项      | 上下文菜单   | 悬停按钮      | DEL 键              | 轻扫以删除   |
| 标记项        | 上下文菜单   | 悬停按钮      | Ctrl+Shift+G         | 轻扫以标记     |
| 刷新数据     | 上下文菜单   | 不适用               | F5 键               | 下拉刷新   |
| 收藏项 | 上下文菜单   | 悬停按钮      | F、Ctrl+S            | 轻扫以收藏 |

**始终提供上下文菜单**我们建议在传统的上下文菜单或 CommandBarFlyout，包括所有相关的上下文命令如同时支持的所有输入类型。 例如，如果仅在指针悬停事件公开命令，它不能使用仅限触摸设备上。

## <a name="commands-in-uwp-applications"></a>在 UWP 应用程序中的命令

有几种方法可以共享和管理命令中的 UWP 应用程序体验。 可以在代码隐藏 （这可以是效率很低，具体取决于你的 UI 的复杂） 中定义的标准交互操作，例如 Click，事件处理程序，可以将标准交互操作的事件侦听器绑定到共享的处理程序，也可以绑定控件的描述命令逻辑的 ICommand 实现的命令属性。

若要有效和最少的代码重复跨命令界面提供丰富和全面的用户体验，我们建议使用绑定功能，本主题中所述的命令 （对于标准事件处理，请参阅单个事件主题）。

若要将控件绑定到共享的命令资源，可以自己，实现 ICommand 接口，也可以生成您的命令从[XamlUICommand](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.xamluicommand)基类或定义的平台的命令之一[StandardUICommand](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.standarduicommand)派生的类。

- ICommand 接口 ([Windows.UI.Xaml.Input.ICommand](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.icommand)或[System.Windows.Input.ICommand](https://docs.microsoft.com/dotnet/api/system.windows.input.icommand))，您可以创建完全自定义，在您的应用程序之间的可重用命令。
- [XamlUICommand](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.xamluicommand)还提供了此功能，但通过公开一组命令行为、 键盘快捷方式 （访问键和加速键）、 图标、 标签和说明等内置命令属性简化了开发。
- [StandardUICommand](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.standarduicommand)可进一步简化通过让你可以选择一组预定义的属性与标准平台命令。

> [!Important]
> 在 UWP 应用程序中，命令都是为实现[Windows.UI.Xaml.Input.ICommand](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.icommand) （c + +） 或[System.Windows.Input.ICommand](https://docs.microsoft.com/dotnet/api/system.windows.input.icommand) (C#) 接口，具体取决于你所选择的语言框架。

## <a name="command-experiences-using-the-standarduicommand-class"></a>命令遇到使用 StandardUICommand 类

派生自[XamlUiCommand](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.xamluicommand) (派生自[Windows.UI.Xaml.Input.ICommand](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.icommand) c + + 或[System.Windows.Input.ICommand](https://docs.microsoft.com/dotnet/api/system.windows.input.icommand)为C#)， [StandardUICommand](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.standarduicommand)类公开一组标准的平台与预定义属性，如图标、 键盘快捷键和描述的命令。

一个[StandardUICommand](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.standarduicommand)提供了快速且一致的方式来定义的常用命令，如`Save`或`Delete`。 您需要做的就是提供的执行和 canExecute 函数。

### <a name="example"></a>示例

![StandardUICommand 示例](images/commanding/StandardUICommandSampleOptimized.gif)

*StandardUICommandSample*

在此示例中，我们演示如何以增强基本[ListView](listview-and-gridview.md)通过删除项命令通过实现[StandardUICommand](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.standarduicommand)类，同时优化的各种输入类型的用户体验使用[菜单栏](menus.md)，[轻扫](swipe.md)控件、 悬停按钮、 并[上下文菜单](menus.md)。

> [!NOTE]
> 此示例要求 Microsoft.UI.Xaml.Controls NuGet 包的一部分[Microsoft Windows 用户界面库](https://docs.microsoft.com/uwp/toolkits/winui/)。

**Xaml:**

示例包括用户界面[ListView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.listview)的五个项。 Delete [StandardUICommand](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.standarduicommand)绑定到[MenuBarItem](https://docs.microsoft.com/uwp/api/microsoft.ui.xaml.controls.menubaritem)即[SwipeItem](https://docs.microsoft.com/uwp/api/microsoft.ui.xaml.controls.swipeitem)，则[AppBarButton](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.appbarbutton)，和[ContextFlyout 菜单](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.contextflyout)。

``` xaml
<Page
    x:Class="StandardUICommandSample.MainPage"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:local="using:StandardUICommandSample"
    xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
    xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
    xmlns:muxcontrols="using:Microsoft.UI.Xaml.Controls"
    mc:Ignorable="d"
    Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">

    <Page.Resources>
        <Style x:Key="HorizontalSwipe" 
               TargetType="ListViewItem" 
               BasedOn="{StaticResource ListViewItemRevealStyle}">
            <Setter Property="Height" Value="60"/>
            <Setter Property="Padding" Value="0"/>
            <Setter Property="HorizontalContentAlignment" Value="Stretch"/>
            <Setter Property="VerticalContentAlignment" Value="Stretch"/>
            <Setter Property="BorderThickness" Value="0"/>
        </Style>
    </Page.Resources>

    <Grid Loaded="ControlExample_Loaded">
        <Grid.RowDefinitions>
            <RowDefinition Height="Auto" />
            <RowDefinition Height="Auto" />
            <RowDefinition Height="*"/>
        </Grid.RowDefinitions>

        <StackPanel Grid.Row="0" 
                    Padding="10" 
                    BorderThickness="0,0,0,1" 
                    BorderBrush="LightBlue"
                    Background="AliceBlue">
            <TextBlock Style="{StaticResource HeaderTextBlockStyle}">
                StandardUICommand sample
            </TextBlock>
            <TextBlock Style="{StaticResource SubtitleTextBlockStyle}" Margin="0,0,0,10">
                This sample shows how to use the StandardUICommand class to 
                share a platform command and consistent user experiences 
                across various controls.
            </TextBlock>
            <TextBlock Style="{StaticResource SubtitleTextBlockStyle}" Margin="0,0,0,0">
                Specifically, we define a standard delete command and add it 
                to a variety of command surfaces, all of which share a common 
                icon, label, keyboard accelerator, and description.
            </TextBlock>
        </StackPanel>

        <muxcontrols:MenuBar Grid.Row="1" Padding="10">
            <muxcontrols:MenuBarItem Title="File">
            </muxcontrols:MenuBarItem>
            <muxcontrols:MenuBarItem Title="Edit">
                <MenuFlyoutItem x:Name="DeleteFlyoutItem"/>
            </muxcontrols:MenuBarItem>
            <muxcontrols:MenuBarItem Title="Help">
            </muxcontrols:MenuBarItem>
        </muxcontrols:MenuBar>

        <ListView x:Name="ListViewRight" Grid.Row="2" 
                  Loaded="ListView_Loaded" 
                  IsItemClickEnabled="True" 
                  SelectionMode="Single" 
                  SelectionChanged="ListView_SelectionChanged" 
                  ItemContainerStyle="{StaticResource HorizontalSwipe}">
            <ListView.ItemTemplate>
                <DataTemplate x:DataType="local:ListItemData">
                    <UserControl PointerEntered="ListViewSwipeContainer_PointerEntered" 
                                 PointerExited="ListViewSwipeContainer_PointerExited">
                        <UserControl.ContextFlyout>
                            <MenuFlyout>
                                <MenuFlyoutItem 
                                    Command="{x:Bind Command}" 
                                    CommandParameter="{x:Bind Text}" />
                            </MenuFlyout>
                        </UserControl.ContextFlyout>
                        <Grid AutomationProperties.Name="{x:Bind Text}">
                            <VisualStateManager.VisualStateGroups>
                                <VisualStateGroup x:Name="HoveringStates">
                                    <VisualState x:Name="HoverButtonsHidden" />
                                    <VisualState x:Name="HoverButtonsShown">
                                        <VisualState.Setters>
                                            <Setter Target="HoverButton.Visibility" 
                                                    Value="Visible" />
                                        </VisualState.Setters>
                                    </VisualState>
                                </VisualStateGroup>
                            </VisualStateManager.VisualStateGroups>
                            <SwipeControl x:Name="ListViewSwipeContainer" >
                                <SwipeControl.RightItems>
                                    <SwipeItems Mode="Execute">
                                        <SwipeItem x:Name="DeleteSwipeItem" 
                                                   Background="Red" 
                                                   Command="{x:Bind Command}" 
                                                   CommandParameter="{x:Bind Text}"/>
                                    </SwipeItems>
                                </SwipeControl.RightItems>
                                <Grid VerticalAlignment="Center">
                                    <TextBlock Text="{x:Bind Text}" 
                                               Margin="10" 
                                               FontSize="18" 
                                               HorizontalAlignment="Left" 
                                               VerticalAlignment="Center"/>
                                    <AppBarButton x:Name="HoverButton" 
                                                  IsTabStop="False" 
                                                  HorizontalAlignment="Right" 
                                                  Visibility="Collapsed" 
                                                  Command="{x:Bind Command}" 
                                                  CommandParameter="{x:Bind Text}"/>
                                </Grid>
                            </SwipeControl>
                        </Grid>
                    </UserControl>
                </DataTemplate>
            </ListView.ItemTemplate>
        </ListView>
    </Grid>
</Page>
```

**Code-behind**

1. 首先，我们定义`ListItemData`包含我们 ListView 中每个列表视图项的文本字符串和 ICommand 的类。

```csharp
public class ListItemData
{
    public String Text { get; set; }
    public ICommand Command { get; set; }
}
```

2. 在 MainPage 类中，我们可以定义一系列`ListItemData`对象的[DataTemplate](https://docs.microsoft.com/uwp/api/windows.ui.xaml.datatemplate)的[ListView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.listview) [ItemTemplate](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.itemscontrol.itemtemplate)。 我们然后填充它与初始的五个项集合 (与文本和相关联[StandardUICommand](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.standarduicommand)删除)。

```csharp
ObservableCollection<ListItemData> collection = new ObservableCollection<ListItemData>();

private void ControlExample_Loaded(object sender, RoutedEventArgs e)
{
    var deleteCommand = new StandardUICommand(StandardUICommandKind.Delete);
    deleteCommand.ExecuteRequested += DeleteCommand_ExecuteRequested;

    DeleteFlyoutItem.Command = deleteCommand;

    for (var i = 0; i < 5; i++)
    {
        collection.Add(new ListItemData { Text = "List item " + i.ToString(), Command = deleteCommand });
    }
}

private void ListView_Loaded(object sender, RoutedEventArgs e)
{
    var listView = (ListView)sender;
    listView.ItemsSource = collection;
}
```

3. 接下来，我们定义了我们在其中实施项 delete 命令的 ICommand ExecuteRequested 处理程序。

``` csharp
private void DeleteCommand_ExecuteRequested(XamlUICommand sender, ExecuteRequestedEventArgs args)
{
    if (args.Parameter != null)
    {
        foreach (var i in collection)
        {
            if (i.Text == (args.Parameter as string))
            {
                collection.Remove(i);
                return;
            }
        }
    }
    if (ListViewRight.SelectedIndex != -1)
    {
        collection.RemoveAt(ListViewRight.SelectedIndex);
    }
}
```

4. 最后，我们定义了各种 ListView 事件，其中包括的处理程序[PointerEntered](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.pointerentered)， [PointerExited](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.pointerexited)，并[SelectionChanged](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.selector.selectionchanged)事件。 指针事件处理程序用于显示或隐藏的每一项删除按钮。

```csharp
private void ListView_SelectionChanged(object sender, SelectionChangedEventArgs e)
{
    if (ListViewRight.SelectedIndex != -1)
    {
        var item = collection[ListViewRight.SelectedIndex];
    }
}

private void ListViewSwipeContainer_PointerEntered(object sender, PointerRoutedEventArgs e)
{
    if (e.Pointer.PointerDeviceType == Windows.Devices.Input.PointerDeviceType.Mouse || e.Pointer.PointerDeviceType == Windows.Devices.Input.PointerDeviceType.Pen)
    {
        VisualStateManager.GoToState(sender as Control, "HoverButtonsShown", true);
    }
}

private void ListViewSwipeContainer_PointerExited(object sender, PointerRoutedEventArgs e)
{
    VisualStateManager.GoToState(sender as Control, "HoverButtonsHidden", true);
}
```

## <a name="command-experiences-using-the-xamluicommand-class"></a>命令遇到使用 XamlUICommand 类

如果需要创建命令并不定义由[StandardUICommand](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.standarduicommand)类，或你想要更好地控制命令外观[XamlUiCommand](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.xamluicommand)类派生自[ICommand](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.icommand)接口，将添加各种 UI （如图标、 标签、 说明和键盘快捷方式） 的属性、 方法和事件，以快速定义 UI 和行为的自定义命令。

[XamlUICommand](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.xamluicommand) ，可以指定用户界面管理绑定，如一个图标，通过添加标签说明，和键盘快捷方式 （访问键和键盘快捷键），而无需设置单独的属性。

### <a name="example"></a>示例

![XamlUICommand 示例](images/commanding/XamlUICommandSampleOptimized.gif)

*XamlUICommandSample*

此示例中共享的上一个删除功能[StandardUICommand](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.standarduicommand)示例中，但显示了如何[XamlUICommand](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.xamluicommand)类可以定义自定义删除命令自己字体图标时，标签，键盘快捷键和描述。 像[StandardUICommand](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.standarduicommand)示例中，我们增强基本[ListView](listview-and-gridview.md)通过删除项命令通过实现[XamlUICommand](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.xamluicommand)类，同时优化不同的输入类型使用的用户体验[菜单栏](menus.md)，[轻扫](swipe.md)控件、 悬停按钮、 并[上下文菜单](menus.md)。

许多平台控件使用事实上，就像在上一部分中我们 StandardUICommand 示例 XamlUICommand 属性。 

> [!NOTE]
> 此示例要求 Microsoft.UI.Xaml.Controls NuGet 包的一部分[Microsoft Windows 用户界面库](https://docs.microsoft.com/uwp/toolkits/winui/)。

**Xaml:**

示例包括用户界面[ListView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.listview)的五个项。 自定义[XamlUICommand](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.xamluicommand)删除绑定到[MenuBarItem](https://docs.microsoft.com/uwp/api/microsoft.ui.xaml.controls.menubaritem)即[SwipeItem](https://docs.microsoft.com/uwp/api/microsoft.ui.xaml.controls.swipeitem)，则[AppBarButton](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.appbarbutton)，和[ContextFlyout 菜单](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.contextflyout)。

``` xaml
<Page
    x:Class="XamlUICommand_Sample.MainPage"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:local="using:XamlUICommand_Sample"
    xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
    xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
    xmlns:muxcontrols="using:Microsoft.UI.Xaml.Controls"
    mc:Ignorable="d"
    Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">

    <Page.Resources>
        <XamlUICommand x:Name="CustomXamlUICommand" ExecuteRequested="DeleteCommand_ExecuteRequested" Description="Custom XamlUICommand" Label="Custom XamlUICommand">
            <XamlUICommand.IconSource>
                <FontIconSource FontFamily="Wingdings" Glyph="&#x4D;"/>
            </XamlUICommand.IconSource>
            <XamlUICommand.KeyboardAccelerators>
                <KeyboardAccelerator Key="D" Modifiers="Control"/>
            </XamlUICommand.KeyboardAccelerators>
        </XamlUICommand>

        <Style x:Key="HorizontalSwipe" 
               TargetType="ListViewItem" 
               BasedOn="{StaticResource ListViewItemRevealStyle}">
            <Setter Property="Height" Value="70"/>
            <Setter Property="Padding" Value="0"/>
            <Setter Property="HorizontalContentAlignment" Value="Stretch"/>
            <Setter Property="VerticalContentAlignment" Value="Stretch"/>
            <Setter Property="BorderThickness" Value="0"/>
        </Style>
        
    </Page.Resources>

    <Grid Loaded="ControlExample_Loaded" Name="MainGrid">
        <Grid.RowDefinitions>
            <RowDefinition Height="Auto" />
            <RowDefinition Height="Auto" />
            <RowDefinition Height="*"/>
        </Grid.RowDefinitions>
        
        <StackPanel Grid.Row="0" 
                    Padding="10" 
                    BorderThickness="0,0,0,1" 
                    BorderBrush="LightBlue"
                    Background="AliceBlue">
            <TextBlock Style="{StaticResource HeaderTextBlockStyle}">
                XamlUICommand sample
            </TextBlock>
            <TextBlock Style="{StaticResource SubtitleTextBlockStyle}" Margin="0,0,0,10">
                This sample shows how to use the XamlUICommand class to 
                share a custom command with consistent user experiences 
                across various controls.
            </TextBlock>
            <TextBlock Style="{StaticResource SubtitleTextBlockStyle}" Margin="0,0,0,0">
                Specifically, we define a custom delete command and add it 
                to a variety of command surfaces, all of which share a common 
                icon, label, keyboard accelerator, and description.
            </TextBlock>
        </StackPanel>

        <muxcontrols:MenuBar Grid.Row="1">
            <muxcontrols:MenuBarItem Title="File">
            </muxcontrols:MenuBarItem>
            <muxcontrols:MenuBarItem Title="Edit">
                <MenuFlyoutItem x:Name="DeleteFlyoutItem" Command="{StaticResource CustomXamlUICommand}"/>
            </muxcontrols:MenuBarItem>
            <muxcontrols:MenuBarItem Title="Help">
            </muxcontrols:MenuBarItem>
        </muxcontrols:MenuBar>

        <ListView x:Name="ListViewRight" Grid.Row="2" 
                  Loaded="ListView_Loaded" 
                  IsItemClickEnabled="True"
                  SelectionMode="Single" 
                  SelectionChanged="ListView_SelectionChanged" 
                  ItemContainerStyle="{StaticResource HorizontalSwipe}">
            <ListView.ItemTemplate>
                <DataTemplate x:DataType="local:ListItemData">
                    <UserControl PointerEntered="ListViewSwipeContainer_PointerEntered"
                                 PointerExited="ListViewSwipeContainer_PointerExited">
                        <UserControl.ContextFlyout>
                            <MenuFlyout>
                                <MenuFlyoutItem 
                                    Command="{x:Bind Command}" 
                                    CommandParameter="{x:Bind Text}" />
                            </MenuFlyout>
                        </UserControl.ContextFlyout>
                        <Grid AutomationProperties.Name="{x:Bind Text}">
                            <VisualStateManager.VisualStateGroups>
                                <VisualStateGroup x:Name="HoveringStates">
                                    <VisualState x:Name="HoverButtonsHidden" />
                                    <VisualState x:Name="HoverButtonsShown">
                                        <VisualState.Setters>
                                            <Setter Target="HoverButton.Visibility" 
                                                    Value="Visible" />
                                        </VisualState.Setters>
                                    </VisualState>
                                </VisualStateGroup>
                            </VisualStateManager.VisualStateGroups>
                            <SwipeControl x:Name="ListViewSwipeContainer">
                                <SwipeControl.RightItems>
                                    <SwipeItems Mode="Execute">
                                        <SwipeItem x:Name="DeleteSwipeItem"
                                                   Background="Red" 
                                                   Command="{x:Bind Command}" 
                                                   CommandParameter="{x:Bind Text}"/>
                                    </SwipeItems>
                                </SwipeControl.RightItems>
                                <Grid VerticalAlignment="Center">
                                    <TextBlock Text="{x:Bind Text}" 
                                               Margin="10" 
                                               FontSize="18" 
                                               HorizontalAlignment="Left"       
                                               VerticalAlignment="Center"/>
                                    <AppBarButton x:Name="HoverButton" 
                                                  IsTabStop="False" 
                                                  HorizontalAlignment="Right" 
                                                  Visibility="Collapsed" 
                                                  Command="{x:Bind Command}" 
                                                  CommandParameter="{x:Bind Text}"/>
                                </Grid>
                            </SwipeControl>
                        </Grid>
                    </UserControl>
                </DataTemplate>
            </ListView.ItemTemplate>
        </ListView>
    </Grid>
</Page>
```

**Code-behind**

1. 首先，我们定义`ListItemData`包含我们 ListView 中每个列表视图项的文本字符串和 ICommand 的类。

```csharp
public class ListItemData
{
    public String Text { get; set; }
    public ICommand Command { get; set; }
}
```

2. 在 MainPage 类中，我们可以定义一系列`ListItemData`对象的[DataTemplate](https://docs.microsoft.com/uwp/api/windows.ui.xaml.datatemplate)的[ListView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.listview) [ItemTemplate](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.itemscontrol.itemtemplate)。 我们然后填充它与初始的五个项集合 (与文本和相关联[XamlUICommand](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.xamluicommand))。

```csharp
ObservableCollection<ListItemData> collection = new ObservableCollection<ListItemData>();

private void ControlExample_Loaded(object sender, RoutedEventArgs e)
{
    for (var i = 0; i < 5; i++)
    {
        collection.Add(new ListItemData { Text = "List item " + i.ToString(), Command = CustomXamlUICommand });
    }
}

private void ListView_Loaded(object sender, RoutedEventArgs e)
{
    var listView = (ListView)sender;
    listView.ItemsSource = collection;
}
```

3. 接下来，我们定义了我们在其中实施项 delete 命令的 ICommand ExecuteRequested 处理程序。

``` csharp
private void DeleteCommand_ExecuteRequested(XamlUICommand sender, ExecuteRequestedEventArgs args)
{
    if (args.Parameter != null)
    {
        foreach (var i in collection)
        {
            if (i.Text == (args.Parameter as string))
            {
                collection.Remove(i);
                return;
            }
        }
    }
    if (ListViewRight.SelectedIndex != -1)
    {
        collection.RemoveAt(ListViewRight.SelectedIndex);
    }
}
```

4. 最后，我们定义了各种 ListView 事件，其中包括的处理程序[PointerEntered](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.pointerentered)， [PointerExited](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.pointerexited)，并[SelectionChanged](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.selector.selectionchanged)事件。 指针事件处理程序用于显示或隐藏的每一项删除按钮。

```csharp
private void ListView_SelectionChanged(object sender, SelectionChangedEventArgs e)
{
    if (ListViewRight.SelectedIndex != -1)
    {
        var item = collection[ListViewRight.SelectedIndex];
    }
}

private void ListViewSwipeContainer_PointerEntered(object sender, PointerRoutedEventArgs e)
{
    if (e.Pointer.PointerDeviceType == Windows.Devices.Input.PointerDeviceType.Mouse || e.Pointer.PointerDeviceType == Windows.Devices.Input.PointerDeviceType.Pen)
    {
        VisualStateManager.GoToState(sender as Control, "HoverButtonsShown", true);
    }
}

private void ListViewSwipeContainer_PointerExited(object sender, PointerRoutedEventArgs e)
{
    VisualStateManager.GoToState(sender as Control, "HoverButtonsHidden", true);
}
```

## <a name="command-experiences-using-the-icommand-interface"></a>使用 ICommand 界面的命令体验

标准 UWP 控件 （按钮、 列表、 选择、 日历、 预测性文本） 的许多常见命令体验奠定了基础。 控件类型的完整列表，请参阅[控件和适用于 UWP 应用的模式](index.md)。

若要支持结构化的命令体验的最基本方法是定义 ICommand 接口的实现 ([Windows.UI.Xaml.Input.ICommand](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.icommand) c + + 或[System.Windows.Input.ICommand](https://docs.microsoft.com/dotnet/api/system.windows.input.icommand)为C#)。  此 ICommand 实例然后可以绑定到控件，如按钮。

> [!NOTE]
> 在某些情况下，可能只是效率要绑定到单击事件和属性的 IsEnabled 属性和方法。

#### <a name="example"></a>示例

![命令界面示例](images/commanding/icommand.gif)

*ICommand 示例*
 
在此示例中，我们将演示如何使用一个按钮可以调用单个命令单击、 键盘快捷键，并旋转鼠标滚轮。

我们使用两个[Listview](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.listview)、 一个填充了五个项和其他为空和两个按钮，一个用于将从 ListView 左侧项移动到右侧，ListView 和从右到左的另一个用于移动项。 每个按钮绑定到相应的命令 (ViewModel.MoveRightCommand 和 ViewModel.MoveLeftCommand，分别)，并启用和禁用自动根据其关联的 ListView 中的项数目。

**以下 XAML 代码定义我们的示例中的 UI。**

```xaml
<Page
    x:Class="UICommand1.View.MainPage"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:vm="using:UICommand1.ViewModel"
    xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
    xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
    mc:Ignorable="d"
    Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">

    <Page.Resources>
        <vm:OpacityConverter x:Key="opaque" />
    </Page.Resources>

    <Grid Name="ItemGrid"
          Background="AliceBlue"
          PointerWheelChanged="Page_PointerWheelChanged">
        <Grid.ColumnDefinitions>
            <ColumnDefinition Width="*"/>
            <ColumnDefinition Width="2*"/>
            <ColumnDefinition Width="*"/>
        </Grid.ColumnDefinitions>
        <ListView Grid.Column="0" VerticalAlignment="Center"
                  x:Name="CommandListView" 
                  ItemsSource="{x:Bind Path=ViewModel.ListItemLeft}" 
                  SelectionMode="None" IsItemClickEnabled="False" 
                  HorizontalAlignment="Right">
            <ListView.ItemTemplate>
                <DataTemplate x:DataType="vm:ListItemData">
                    <Grid VerticalAlignment="Center">
                        <AppBarButton Label="{x:Bind ListItemText}">
                            <AppBarButton.Icon>
                                <SymbolIcon Symbol="{x:Bind ListItemIcon}"/>
                            </AppBarButton.Icon>
                        </AppBarButton>
                    </Grid>
                </DataTemplate>
            </ListView.ItemTemplate>
        </ListView>
        <Grid Grid.Column="1" Margin="0,0,5,0"
              HorizontalAlignment="Center" 
              VerticalAlignment="Center">
            <Grid.RowDefinitions>
                <RowDefinition Height="*"/>
                <RowDefinition Height="*"/>
                <RowDefinition Height="*"/>
            </Grid.RowDefinitions>
            <StackPanel Grid.Row="1">
                <FontIcon FontFamily="{StaticResource SymbolThemeFontFamily}" 
                          FontSize="40" Glyph="&#xE893;" 
                          Opacity="{x:Bind Path=ViewModel.listItemLeft.Count, Mode=OneWay, Converter={StaticResource opaque}}"/>
                <Button Name="MoveItemRightButton" ToolTipService.ToolTip="Tooltip"
                        Margin="0,10,0,10" Width="120" HorizontalAlignment="Center"
                        Command="{x:Bind Path=ViewModel.MoveRightCommand, Mode=OneWay}">
                    <Button.KeyboardAccelerators>
                        <KeyboardAccelerator 
                            Modifiers="Control" 
                            Key="Add" />
                    </Button.KeyboardAccelerators>
                    <StackPanel>
                        <SymbolIcon Symbol="Next"/>
                        <TextBlock>Move item right</TextBlock>
                    </StackPanel>
                </Button>
                <Button Name="MoveItemLeftButton" 
                            Margin="0,10,0,10" Width="120" HorizontalAlignment="Center"
                            Command="{x:Bind Path=ViewModel.MoveLeftCommand, Mode=OneWay}">
                    <Button.KeyboardAccelerators>
                        <KeyboardAccelerator 
                            Modifiers="Control" 
                            Key="Subtract" />
                    </Button.KeyboardAccelerators>
                    <StackPanel>
                        <SymbolIcon Symbol="Previous"/>
                        <TextBlock>Move item left</TextBlock>
                    </StackPanel>
                </Button>
                <FontIcon FontFamily="{StaticResource SymbolThemeFontFamily}" 
                          FontSize="40" Glyph="&#xE892;"
                          Opacity="{x:Bind Path=ViewModel.listItemRight.Count, Mode=OneWay, Converter={StaticResource opaque}}"/>
            </StackPanel>
        </Grid>
        <ListView Grid.Column="2" 
                  x:Name="CommandListViewRight" 
                  VerticalAlignment="Center" 
                  IsItemClickEnabled="False" 
                  SelectionMode="None"
                  ItemsSource="{x:Bind Path=ViewModel.ListItemRight}" 
                  HorizontalAlignment="Left">
            <ListView.ItemTemplate>
                <DataTemplate x:DataType="vm:ListItemData">
                    <Grid VerticalAlignment="Center">
                        <AppBarButton Label="{x:Bind ListItemText}">
                            <AppBarButton.Icon>
                                <SymbolIcon Symbol="{x:Bind ListItemIcon}"/>
                            </AppBarButton.Icon>
                        </AppBarButton>
                    </Grid>
                </DataTemplate>
            </ListView.ItemTemplate>
        </ListView>
    </Grid>
</Page>
```

**下面是前面的 UI 与代码隐藏。**

在代码隐藏中，我们连接到我们的视图模型，其中包含我们命令的代码。 此外，我们定义鼠标滚轮，还会联系，我们命令的代码来自输入的处理程序。

```csharp
using Windows.UI.Xaml;
using Windows.UI.Xaml.Input;
using Windows.UI.Xaml.Controls;
using UICommand1.ViewModel;
using Windows.System;
using Windows.UI.Core;

namespace UICommand1.View
{
    /// <summary>
    /// An empty page that can be used on its own or navigated to within a Frame.
    /// </summary>
    public sealed partial class MainPage : Page
    {
        // Reference to our view model.
        public UICommand1ViewModel ViewModel { get; set; }

        // Initialize our view and view model.
        public MainPage()
        {
            this.InitializeComponent();
            ViewModel = new UICommand1ViewModel();
        }

        /// <summary>
        /// Handle mouse wheel input and assign our
        /// commands to appropriate direction of rotation.
        /// </summary>
        /// <param name="sender"></param>
        /// <param name="e"></param>
        private void Page_PointerWheelChanged(object sender, PointerRoutedEventArgs e)
        {
            var props = e.GetCurrentPoint(sender as UIElement).Properties;

            // Require CTRL key and accept only vertical mouse wheel movement 
            // to eliminate accidental wheel input.
            if ((Window.Current.CoreWindow.GetKeyState(VirtualKey.Control) != 
                CoreVirtualKeyStates.None) && !props.IsHorizontalMouseWheel)
            {
                bool delta = props.MouseWheelDelta < 0 ? true : false;

                switch (delta)
                {
                    case true:
                        ViewModel.MoveRight();
                        break;
                    case false:
                        ViewModel.MoveLeft();
                        break;
                    default:
                        break;
                }
            }
        }
    }
}
```

**下面是从我们的视图模型的代码**

我们的视图模型是我们在我们的应用程序中定义的两个命令的执行详细信息，填充一个 ListView 和用于隐藏或显示某些其他 UI 基于每个 ListView 项计数的不透明度值转换器的位置。

```csharp
using System;
using System.Collections.ObjectModel;
using System.Collections.Specialized;
using System.ComponentModel;
using Windows.UI.Xaml;
using Windows.UI.Xaml.Controls;
using Windows.UI.Xaml.Data;

namespace UICommand1.ViewModel
{
    /// <summary>
    /// UI properties for our list items.
    /// </summary>
    public class ListItemData
    {
        /// <summary>
        /// Gets and sets the list item content string.
        /// </summary>
        public string ListItemText { get; set; }
        /// <summary>
        /// Gets and sets the list item icon.
        /// </summary>
        public Symbol ListItemIcon { get; set; }
    }

    /// <summary>
    /// View Model that sets up a command to handle invoking the move item buttons.
    /// </summary>
    public class UICommand1ViewModel : INotifyPropertyChanged
    {
        /// <summary>
        /// The command to invoke when the Move item left button is pressed.
        /// </summary>
        public RelayCommand moveLeftCommand;
        public RelayCommand MoveLeftCommand { get => moveLeftCommand; private set { } }

        /// <summary>
        /// The command to invoke when the Move item right button is pressed.
        /// </summary>
        public RelayCommand moveRightCommand;
        public RelayCommand MoveRightCommand { get => moveRightCommand; private set { } }

        // Item collections
        public ObservableCollection<ListItemData> listItemLeft;
        public ObservableCollection<ListItemData> ListItemLeft { get => listItemLeft; private set { } }
        public ObservableCollection<ListItemData> listItemRight;
        public ObservableCollection<ListItemData> ListItemRight { get => listItemRight; private set { } }

        public ListItemData listItem;

        /// <summary>
        /// Sets up a command to handle invoking the move item buttons.
        /// </summary>
        public UICommand1ViewModel()
        {
            moveLeftCommand = new RelayCommand(new Action(MoveLeft), CanExecuteMoveLeftCommand);
            moveRightCommand = new RelayCommand(new Action(MoveRight), CanExecuteMoveRightCommand);

            listItemLeft = new ObservableCollection<ListItemData>();
            listItemRight = new ObservableCollection<ListItemData>();

            LoadItems();
        }

        /// <summary>
        ///  Populate our list of items.
        /// </summary>
        public void LoadItems()
        {
            for (var x = 0; x <= 4; x++)
            {
                listItem = new ListItemData();
                listItemLeft.Add(listItem);
                listItem.ListItemText = "Item " + listItemLeft.Count.ToString();
                listItem.ListItemIcon = Symbol.Emoji;
            }
        }

        /// <summary>
        /// Move left command valid when items present in the list on right.
        /// </summary>
        /// <returns>True, if count is greater than 0.</returns>
        private bool CanExecuteMoveLeftCommand()
        {
            return listItemRight.Count > 0;
        }

        /// <summary>
        /// Move right command valid when items present in the list on left.
        /// </summary>
        /// <returns>True, if count is greater than 0.</returns>
        private bool CanExecuteMoveRightCommand()
        {
            return listItemLeft.Count > 0;
        }

        /// <summary>
        /// The command implementation to execute when the Move item right button is pressed.
        /// </summary>
        public void MoveRight()
        {
            if (listItemLeft.Count > 0)
            {
                listItem = new ListItemData();
                listItemRight.Add(listItem);
                listItem.ListItemText = "Item " + listItemRight.Count.ToString();
                listItem.ListItemIcon = Symbol.Emoji;
                listItemLeft.RemoveAt(listItemLeft.Count - 1);
                moveRightCommand.RaiseCanExecuteChanged();
                moveLeftCommand.RaiseCanExecuteChanged();
            }
        }

        /// <summary>
        /// The command implementation to execute when the Move item left button is pressed.
        /// </summary>
        public void MoveLeft()
        {
            if (listItemRight.Count > 0)
            {
                listItem = new ListItemData();
                listItemLeft.Add(listItem);
                listItem.ListItemText = "Item " + listItemRight.Count.ToString();
                listItem.ListItemIcon = Symbol.Emoji;
                listItemRight.RemoveAt(listItemRight.Count - 1);
                moveRightCommand.RaiseCanExecuteChanged();
                moveLeftCommand.RaiseCanExecuteChanged();
            }
        }
    }

    /// <summary>
    /// Convert a collection count to an opacity value of 0.0 or 1.0.
    /// </summary>
    public class OpacityConverter : IValueConverter
    {
        /// <summary>
        /// Converts a collection count to an opacity value of 0.0 or 1.0.
        /// </summary>
        /// <param name="value">The bool passed in</param>
        /// <param name="targetType">Ignored.</param>
        /// <param name="parameter">Ignored</param>
        /// <param name="language">Ignored</param>
        /// <returns>1.0 if count > 0, otherwise returns 0.0</returns>
        public object Convert(object value, Type targetType, object parameter, string language)
        {
            return ((int)value > 0 ? 1.0 : 0.0);
        }

        /// <summary>
        /// Not used, converter is not intended for two-way binding. 
        /// </summary>
        /// <param name="value">Ignored</param>
        /// <param name="targetType">Ignored</param>
        /// <param name="parameter">Ignored</param>
        /// <param name="language">Ignored</param>
        /// <returns></returns>
        public object ConvertBack(object value, Type targetType, object parameter, string language)
        {
            throw new NotImplementedException();
        }
    }
}
```

**最后，下面是我们的 ICommand 接口实现**

在这里，我们定义了实现的命令[ICommand](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.icommand)接口，只需将中继到其他对象其功能。

```csharp
using System;
using System.Windows.Input;

namespace UICommand1
{
    /// <summary>
    /// A command whose sole purpose is to relay its functionality 
    /// to other objects by invoking delegates. 
    /// The default return value for the CanExecute method is 'true'.
    /// <see cref="RaiseCanExecuteChanged"/> needs to be called whenever
    /// <see cref="CanExecute"/> is expected to return a different value.
    /// </summary>
    public class RelayCommand : ICommand
    {
        private readonly Action _execute;
        private readonly Func<bool> _canExecute;

        /// <summary>
        /// Raised when RaiseCanExecuteChanged is called.
        /// </summary>
        public event EventHandler CanExecuteChanged;

        /// <summary>
        /// Creates a new command that can always execute.
        /// </summary>
        /// <param name="execute">The execution logic.</param>
        public RelayCommand(Action execute)
            : this(execute, null)
        {
        }

        /// <summary>
        /// Creates a new command.
        /// </summary>
        /// <param name="execute">The execution logic.</param>
        /// <param name="canExecute">The execution status logic.</param>
        public RelayCommand(Action execute, Func<bool> canExecute)
        {
            if (execute == null)
                throw new ArgumentNullException("execute");
            _execute = execute;
            _canExecute = canExecute;
        }

        /// <summary>
        /// Determines whether this <see cref="RelayCommand"/> can execute in its current state.
        /// </summary>
        /// <param name="parameter">
        /// Data used by the command. If the command does not require data to be passed, this object can be set to null.
        /// </param>
        /// <returns>true if this command can be executed; otherwise, false.</returns>
        public bool CanExecute(object parameter)
        {
            return _canExecute == null ? true : _canExecute();
        }

        /// <summary>
        /// Executes the <see cref="RelayCommand"/> on the current command target.
        /// </summary>
        /// <param name="parameter">
        /// Data used by the command. If the command does not require data to be passed, this object can be set to null.
        /// </param>
        public void Execute(object parameter)
        {
            _execute();
        }

        /// <summary>
        /// Method used to raise the <see cref="CanExecuteChanged"/> event
        /// to indicate that the return value of the <see cref="CanExecute"/>
        /// method has changed.
        /// </summary>
        public void RaiseCanExecuteChanged()
        {
            var handler = CanExecuteChanged;
            if (handler != null)
            {
                handler(this, EventArgs.Empty);
            }
        }
    }
}
```

## <a name="summary"></a>摘要

通用 Windows 平台提供了强大而灵活的命令系统，可用于构建应用程序的共享和管理跨控件类型、 设备和输入的类型的命令。

生成适用于 UWP 应用的命令时，请使用以下方法：

- 侦听并处理 XAML/代码隐藏中的事件
- 将绑定到事件处理方法，例如单击
- 定义您自己的 ICommand 实现
- 使用你自己的一组预定义的属性的值创建 XamlUICommand 对象
- 创建具有预定义的平台属性和值的一组 StandardUICommand 对象

## <a name="next-steps"></a>后续步骤

有关完整示例，演示[XamlUiCommand](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.xamluicommand)并[StandardUICommand](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.standarduicommand)实现中，请参阅[XAML 控件库](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics)示例。

## <a name="see-also"></a>另请参阅

[控件和适用于 UWP 应用的模式](index.md)

<!---Some context for the following links goes here
- [link to next logical step for the customer](global-quickstart-template.md)--->

<!--- Required:
In Overview articles, provide at least one next step and no more than three.
Next steps in overview articles will often link to a quickstart.
Use regular links; do not use a blue box link. What you link to will depend on what is really a next step for the customer.
Do not use a "More info section" or a "Resources section" or a "See also section".
--->