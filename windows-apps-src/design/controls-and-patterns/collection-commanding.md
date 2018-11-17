---
author: mijacobs
description: 如何通过为所有输入类型提供可能的最佳体验的方式，使用上下文命令来实现这些类型的操作。
title: 上下文命令
ms.assetid: ''
label: Contextual commanding in collections
template: detail.hbs
ms.author: mijacobs
ms.date: 10/25/2017
ms.topic: article
keywords: windows 10, uwp
pm-contact: chigy
design-contact: kimsea
dev-contact: niallm
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: f06d7015fcb208b55fe0cb57b96eaecbc99317cc
ms.sourcegitcommit: 3257416aebb5a7b1515e107866806f8bd57845a8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/16/2018
ms.locfileid: "7172155"
---
# <a name="contextual-commanding-for-collections-and-lists"></a>用于集合和列表的上下文命令



许多应用包含用户可以操作的列表、网格和树形式的内容集合。 例如，用户可能能够删除、重命名、标记或刷新项。 本文演示如何通过为所有输入类型提供可能的最佳体验的方式，使用上下文命令来实现这些类型的操作。  

> **重要 API**：[ICommand 接口](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Input.ICommand)、[UIElement.ContextFlyout 属性](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.UIElement.ContextFlyout)、[INotifyPropertyChanged 接口](https://docs.microsoft.com/uwp/api/windows.ui.xaml.data.inotifypropertychanged)

![使用各种输入执行收藏命令](images/ContextualCommand_AddFavorites.png)

## <a name="creating-commands-for-all-input-types"></a>为所有输入类型创建命令

因为用户可以使用[广泛的设备和输入](../devices/index.md)与 UWP 应用交互，所以应用应通过与输入无关的上下文菜单和特定于输入的快捷方式来公开命令。 通过包含这两者，用户可以对内容快速调用命令，而无论是何种输入或设备类型。

下表显示了一些典型集合命令以及用于公开这些命令的方式。 

| 命令          | 与输入无关 | 鼠标快捷方式 | 键盘快捷方式 | 触控快捷方式 |
| ---------------- | -------------- | ----------------- | -------------------- | ----------------- |
| 删除项      | 上下文菜单   | 悬停按钮      | DEL 键              | 轻扫以删除   |
| 标记项        | 上下文菜单   | 悬停按钮      | Ctrl+Shift+G         | 轻扫以标记     |
| 刷新数据     | 上下文菜单   | 不适用               | F5 键               | 下拉以刷新   |
| 收藏项 | 上下文菜单   | 悬停按钮      | F、Ctrl+S            | 轻扫以收藏 |


* **一般情况下，应在项的[上下文菜单](menus.md)中提供用于项的所有命令。** 无论是何种输入类型，上下文菜单都可供用户访问，且应包含用户可以执行的所有上下文命令。

* **对于经常访问的命令，请考虑使用输入快捷方式。** 输入快捷方式使用户可以基于其输入设备快速执行操作。 输入快捷方式包括：
    - 轻扫进行操作（触控快捷方式）
    - 下拉以刷新数据（触控快捷方式）
    - 键盘快捷键（键盘快捷方式）
    - 访问键（键盘快捷方式）
    - 鼠标和手写笔悬停按钮（指针快捷方式）

> [!NOTE]
> 用户应能够从任何类型的设备访问所有命令。 例如，如果应用的命令仅通过悬停按钮指针快捷方式进行公开，则触控用户无法访问它们。 至少使用上下文菜单提供对所有命令的访问。  

## <a name="example-the-podcastobject-data-model"></a>示例：PodcastObject 数据模型

为了演示命令建议，本文为一个播客应用创建播客列表。 示例代码演示如何使用户可以从列表中“收藏”特定播客。

下面是将使用的播客对象的定义： 

```csharp
public class PodcastObject : INotifyPropertyChanged
{
    // The title of the podcast
    public String Title { get; set; }

    // The podcast's description
    public String Description { get; set; }

    // Describes if the user has set this podcast as a favorite
    public bool IsFavorite
    {
        get
        {
            return _isFavorite;
        }
        set
        {
            _isFavorite = value;
            OnPropertyChanged("IsFavorite");
        }
    }
    private bool _isFavorite = false;

    public event PropertyChangedEventHandler PropertyChanged;

    private void OnPropertyChanged(String property)
    {
        PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(property));
    }
}
```

请注意，PodcastObject 实现 [INotifyPropertyChanged](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Data.INotifyPropertyChanged) 以在用户切换 IsFavorite 属性时响应属性更改。

## <a name="defining-commands-with-the-icommand-interface"></a>使用 ICommand 接口定义命令

[ICommand 接口](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Input.ICommand)可帮助定义可用于多种输入类型的命令。 例如，可以将删除逻辑以 [ICommand](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Input.ICommand) 的形式实现一次，然后使它可用于不同输入类型，而不是在两个不同的事件处理程序（一个在用户按下 Delete 键时使用，另一个在用户右键单击上下文菜单中的“删除”时使用）中为删除命令编写相同代码。

需要定义表示“收藏”操作的 ICommand。 我们会使用命令的 [Execute](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Input.ICommand.Execute) 方法收藏播客。 特定播客会通过命令参数（可以使用 CommandParameter 属性进行绑定）提供给执行方法。

```csharp
public class FavoriteCommand: ICommand
{
    public event EventHandler CanExecuteChanged;

    public bool CanExecute(object parameter)
    {
        return true;
    }
    public void Execute(object parameter)
    {
        // Perform the logic to "favorite" an item.
        (parameter as PodcastObject).IsFavorite = true;
    }
}
```

若要对多个集合和元素使用相同命令，可以将命令存储为页面或应用中的资源。

```xaml
<Application.Resources>
    <local:FavoriteCommand x:Key="favoriteCommand" />
</Application.Resources>
```

若要执行命令，请调用其 [Execute](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Input.ICommand.Execute) 方法。

```csharp
// Favorite the item using the defined command
var favoriteCommand = Application.Current.Resources["favoriteCommand"] as ICommand;
favoriteCommand.Execute(PodcastObject);
```


## <a name="creating-a-usercontrol-to-respond-to-a-variety-of-inputs"></a>创建 UserControl 以响应各种输入

具有项列表并且其中每个项都应响应多个输入时，可以通过为项定义 [UserControl](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.UserControl) 并使用它定义项的上下文菜单和事件处理程序，来简化代码。 

在 Visual Studio 中创建 UserControl：
1. 在解决方案资源管理器中，右键单击项目。 上下文菜单随即出现。
2. 选择**添加 > 新建项...** <br />**添加新项**对话框随即出现。 
3. 从项列表中选择 UserControl。 向它提供所需名称并单击**添加**。 Visual Studio 会为你生成存根 UserControl。 

在播客示例中，每个播客都会显示在列表中，这会公开各种用于“收藏”播客的方式。 用户能够执行以下操作以“收藏”播客：
- 调用上下文菜单
- 执行键盘快捷键
- 显示悬停按钮
- 执行轻扫手势

为了封装这些行为并使用 FavoriteCommand，我们来创建名为“PodcastUserControl”的新 [UserControl](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.UserControl)，以表示列表中的播客。

PodcastUserControl 将 PodcastObject 的字段显示为 TextBlock，并响应各种用户交互。 我们会在本文中通篇引用并扩展 PodcastUserControl。

**PodcastUserControl.xaml**
```xaml
<UserControl
    x:Class="ContextCommanding.PodcastUserControl"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
    xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
    mc:Ignorable="d"
    IsTabStop="True" UseSystemFocusVisuals="True"
    >
    <Grid Margin="12,0,12,0">
        <StackPanel>
            <TextBlock Text="{x:Bind PodcastObject.Title, Mode=OneWay}" Style="{StaticResource TitleTextBlockStyle}" />
            <TextBlock Text="{x:Bind PodcastObject.Description, Mode=OneWay}" Style="{StaticResource SubtitleTextBlockStyle}" />
            <TextBlock Text="{x:Bind PodcastObject.IsFavorite, Mode=OneWay}" Style="{StaticResource SubtitleTextBlockStyle}"/>
        </StackPanel>
    </Grid>
</UserControl>
```

**PodcastUserControl.xaml.cs**
```csharp
public sealed partial class PodcastUserControl : UserControl
{
    public static readonly DependencyProperty PodcastObjectProperty =
        DependencyProperty.Register(
            "PodcastObject",
            typeof(PodcastObject),
            typeof(PodcastUserControl),
            new PropertyMetadata(null));

    public PodcastObject PodcastObject
    {
        get { return (PodcastObject)GetValue(PodcastObjectProperty); }
        set { SetValue(PodcastObjectProperty, value); }
    }

    public PodcastUserControl()
    {
        this.InitializeComponent();

        // TODO: We will add event handlers here.
    }
}
```

请注意，PodcastUserControl 将对 PodcastObject 的引用作为 DependencyProperty 进行维护。 这使我们可以将 PodcastObject 绑定到 PodcastUserControl。

生成一些 PodcastObject 之后，可以通过将 PodcastObject 绑定到 ListView 来创建播客的列表。 PodcastUserControl 对象描述 PodcastObject 的可视化，因此使用 ListView 的 ItemTemplate 进行设置。

**MainPage.xaml**
```xaml
<ListView x:Name="ListOfPodcasts"
            ItemsSource="{x:Bind podcasts}">
    <ListView.ItemTemplate>
        <DataTemplate x:DataType="local:PodcastObject">
            <local:PodcastUserControl PodcastObject="{x:Bind Mode=OneWay}" />
        </DataTemplate>
    </ListView.ItemTemplate>
    <ListView.ItemContainerStyle>
        <!-- The PodcastUserControl will entirely fill the ListView item and handle tabbing within itself. -->
        <Style TargetType="ListViewItem" BasedOn="{StaticResource ListViewItemRevealStyle}">
            <Setter Property="HorizontalContentAlignment" Value="Stretch" />
            <Setter Property="Padding" Value="0"/>
            <Setter Property="IsTabStop" Value="False"/>
        </Style>
    </ListView.ItemContainerStyle>
</ListView>
```

## <a name="creating-context-menus"></a>创建上下文菜单

上下文菜单会在用户发出请求时显示命令或选项列表。 上下文菜单提供与其附加元素相关的上下文命令，通常保留用于特定于该项的辅助操作。

![在项上显示上下文菜单](images/ContextualCommand_RightClick.png)

用户可以使用以下这些“上下文操作”调用上下文菜单：

| 输入    | 上下文操作                          |
| -------- | --------------------------------------- |
| 鼠标    | 右键单击                             |
| 键盘 | Shift+F10、“菜单”按钮                  |
| 触控    | 长按项                      |
| 手写笔      | 按筒状按钮、长按项 |
| 游戏板  | “菜单”按钮                             |

**由于无论是何种输入类型用户都可以打开上下文菜单，因此上下文菜单应包含所有可用于列表项的上下文命令。**

### <a name="contextflyout"></a>ContextFlyout

通过 UIElement 类定义的 [ContextFlyout 属性](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.UIElement.ContextFlyout)支持轻松创建适用于所有输入类型的上下文菜单。 可使用 MenuFlyout 提供表示上下文菜单的浮出控件，当用户执行上面定义的“上下文操作”时，与项对应的 MenuFlyout 会显示。

我们会将 ContextFlyout 添加到 PodcastUserControl。 指定为 ContextFlyout 的 MenuFlyout 包含用于收藏播客的单个项。 请注意，此 MenuFlyoutItem 将上面定义的 favoriteCommand 与绑定到 PodcastObject 的 CommandParamter 结合使用。

**PodcastUserControl.xaml**
```xaml
<UserControl>
    <UserControl.ContextFlyout>
        <MenuFlyout>
            <MenuFlyoutItem Text="Favorite" Command="{StaticResource favoriteCommand}" CommandParameter="{x:Bind PodcastObject, Mode=OneWay}" />
        </MenuFlyout>
    </UserControl.ContextFlyout>
    <Grid Margin="12,0,12,0">
        <!-- ... -->
    </Grid>
</UserControl>

```

请注意，还可以使用 [ContextRequested 事件](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.UIElement.ContextRequested)响应上下文操作。 如果已指定 ContextFlyout，则不会触发 ContextRequested 事件。

## <a name="creating-input-accelerators"></a>创建输入快捷方式

虽然集合中的每个项都应具有包含所有上下文命令的上下文菜单，但是你可能要使用户可以快速执行一小组经常执行的命令。 例如，邮件应用可能会具有在上下文菜单中出现的辅助命令（如“回复”、“存档”、“移动到文件夹”、“设置标志”和“删除”），但是最常用的命令是“删除”和“标记”。 确定最常用的命令之后，可以使用基于输入的快捷方式使这些命令更加便于用户执行。

在播客应用中，经常执行的命令是“收藏”命令。

### <a name="keyboard-accelerators"></a>键盘快捷方式

#### <a name="shortcuts-and-direct-key-handling"></a>快捷键和直接键处理

![按 Ctrl 和 F 执行操作](images/ContextualCommand_Keyboard.png)

根据内容的类型，可以确定应执行某个操作的特定组合件。 例如在邮件应用中，DEL 键可以用于删除选择的电子邮件。 在播客应用中，Ctrl+S 或 F 键可以收藏播客以供将来观看。 虽然某些命令具有常用的已知键盘快捷键（如用于删除的 DEL），其他命令也具有特定于应用或域的快捷键。 在可能的情况下使用已知快捷键，或考虑在工具提示中提供提醒文本以指导用户了解快捷键命令。

应用可以使用 [KeyDown](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.UIElement.KeyDownEvent) 事件在用户按某个键时进行响应。 一般情况下，用户期望应用在他们先按下键时进行响应，而不是等到他们释放键。

此示例演练如何将 KeyDown 处理程序添加到 PodcastUserControl，以便在用户按 Ctrl+S 或 F 时收藏播客。它使用与前面相同的命令。

**PodcastUserControl.xaml.cs**
```csharp
// Respond to the F and Ctrl+S keys to favorite the focused item.
protected override void OnKeyDown(KeyRoutedEventArgs e)
{
    var ctrlState = CoreWindow.GetForCurrentThread().GetKeyState(VirtualKey.Control);
    var isCtrlPressed = (ctrlState & CoreVirtualKeyStates.Down) == CoreVirtualKeyStates.Down || (ctrlState & CoreVirtualKeyStates.Locked) == CoreVirtualKeyStates.Locked;

    if (e.Key == Windows.System.VirtualKey.F || (e.Key == Windows.System.VirtualKey.S && isCtrlPressed))
    {
        // Favorite the item using the defined command
        var favoriteCommand = Application.Current.Resources["favoriteCommand"] as ICommand;
        favoriteCommand.Execute(PodcastObject);
    }
}
```

### <a name="mouse-accelerators"></a>鼠标快捷方式

![将鼠标悬停在某个项上方以显示按钮](images/ContextualCommand_HovertoReveal.png)

用户熟悉右键单击上下文菜单，但是你可能希望让用户能够仅使用单次鼠标单击便可执行常用命令。 若要实现此体验，可以在几何项的画布上包含专用按钮。 若要使用户能够使用鼠标快速操作，并且最大程度减少视觉混乱，则可以选择仅当用户让其指针处于特定列表项中时，才显示这些按钮。

在此示例中，通过直接在 PodcastUserControl 中定义的按钮来表示收藏命令。 请注意，此示例中的按钮使用与前面相同的命令，即 FavoriteCommand。 若要切换此按钮的可见性，可以使用 VisualStateManager 在指针进入和退出控件时切换可视状态。

**PodcastUserControl.xaml**
```xaml
<UserControl>
    <UserControl.ContextFlyout>
        <!-- ... -->
    </UserControl.ContextFlyout>
    <Grid Margin="12,0,12,0">
        <VisualStateManager.VisualStateGroups>
            <VisualStateGroup x:Name="HoveringStates">
                <VisualState x:Name="HoverButtonsShown">
                    <VisualState.Setters>
                        <Setter Target="hoverArea.Visibility" Value="Visible" />
                    </VisualState.Setters>
                </VisualState>
                <VisualState x:Name="HoverButtonsHidden" />
            </VisualStateGroup>
        </VisualStateManager.VisualStateGroups>
        <Grid.ColumnDefinitions>
            <ColumnDefinition Width="*" />
            <ColumnDefinition Width="Auto" />
        </Grid.ColumnDefinitions>
        <StackPanel>
            <TextBlock Text="{x:Bind PodcastObject.Title, Mode=OneWay}" Style="{StaticResource TitleTextBlockStyle}" />
            <TextBlock Text="{x:Bind PodcastObject.Description, Mode=OneWay}" Style="{StaticResource SubtitleTextBlockStyle}" />
            <TextBlock Text="{x:Bind PodcastObject.IsFavorite, Mode=OneWay}" Style="{StaticResource SubtitleTextBlockStyle}"/>
        </StackPanel>
        <Grid Grid.Column="1" x:Name="hoverArea" Visibility="Collapsed" VerticalAlignment="Stretch">
            <AppBarButton Icon="OutlineStar" Label="Favorite" Command="{StaticResource favoriteCommand}" CommandParameter="{x:Bind PodcastObject, Mode=OneWay}" IsTabStop="False" VerticalAlignment="Stretch"  />
        </Grid>
    </Grid>
</UserControl>
```

悬停按钮应在鼠标进入和退出项时出现和消失。 若要响应鼠标事件，可以对 PodcastUserControl 使用 [PointerEntered](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.UIElement.PointerEnteredEvent) 和 [PointerExited](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.UIElement.PointerExitedEvent) 事件。

**PodcastUserControl.xaml.cs**
```csharp
protected override void OnPointerEntered(PointerRoutedEventArgs e)
{
    base.OnPointerEntered(e);

    // Only show hover buttons when the user is using mouse or pen.
    if (e.Pointer.PointerDeviceType == Windows.Devices.Input.PointerDeviceType.Mouse || e.Pointer.PointerDeviceType == Windows.Devices.Input.PointerDeviceType.Pen)
    {
        VisualStateManager.GoToState(this, "HoverButtonsShown", true);
    }
}

protected override void OnPointerExited(PointerRoutedEventArgs e)
{
    base.OnPointerExited(e);

    VisualStateManager.GoToState(this, "HoverButtonsHidden", true);
}
```

在悬停状态下显示的按钮只能通过指针输入类型进行访问。 因为这些按钮仅限于指针输入，所以可以选择最大程度最小或删除按钮图标周围的边距以便为指针输入进行优化。 如果选择这样做，请确保按钮占用空间至少是 20x20 像素以便保持可通过手写笔和鼠标进行使用。

### <a name="touch-accelerators"></a>触控快捷方式

#### <a name="swipe"></a>轻扫

![轻扫某个项以显示命令](images/ContextualCommand_Swipe.png)

轻扫命令是使触控设备上的用户可以使用触控执行常用辅助操作的触控快捷方式。 轻扫使触控用户能够使用常用操作（如轻扫以删除或轻扫以调用）快速且自然地与内容进行交互。 请参阅[轻扫命令](swipe.md)一文以了解详细信息。

若要将轻扫集成到集合中，需要两个组件：承载命令的 SwipeItems，以及包装项并允许进行轻扫交互的 SwipeControl。

SwipeItems 可以定义为 PodcastUserControl 中的资源。 在此示例中，SwipeItems 包含用于收藏项的命令。

```xaml
<UserControl.Resources>
    <SymbolIconSource x:Key="FavoriteIcon" Symbol="Favorite"/>
    <SwipeItems x:Key="RevealOtherCommands" Mode="Reveal">
        <SwipeItem IconSource="{StaticResource FavoriteIcon}" Text="Favorite" Background="Yellow" Invoked="SwipeItem_Invoked"/>
    </SwipeItems>
</UserControl.Resources>
```

SwipeControl 会包装项，并使用户可以使用轻扫手势与它进行交互。 请注意，SwipeControl 将对 SwipeItems 的引用作为其 RightItems 包含在内。 当用户从右向左轻扫时，会显示收藏项命令。

```xaml
<SwipeControl x:Name="swipeContainer" RightItems="{StaticResource RevealOtherCommands}">
   <!-- The visual state groups moved from the Grid to the SwipeControl, since the SwipeControl wraps the Grid. -->
   <VisualStateManager.VisualStateGroups>
       <VisualStateGroup x:Name="HoveringStates">
           <VisualState x:Name="HoverButtonsShown">
               <VisualState.Setters>
                   <Setter Target="hoverArea.Visibility" Value="Visible" />
               </VisualState.Setters>
           </VisualState>
           <VisualState x:Name="HoverButtonsHidden" />
       </VisualStateGroup>
   </VisualStateManager.VisualStateGroups>
   <Grid Margin="12,0,12,0">
       <Grid.ColumnDefinitions>
           <ColumnDefinition Width="*" />
           <ColumnDefinition Width="Auto" />
       </Grid.ColumnDefinitions>
       <StackPanel>
           <TextBlock Text="{x:Bind PodcastObject.Title, Mode=OneWay}" Style="{StaticResource TitleTextBlockStyle}" />
           <TextBlock Text="{x:Bind PodcastObject.Description, Mode=OneWay}" Style="{StaticResource SubtitleTextBlockStyle}" />
           <TextBlock Text="{x:Bind PodcastObject.IsFavorite, Mode=OneWay}" Style="{StaticResource SubtitleTextBlockStyle}"/>
       </StackPanel>
       <Grid Grid.Column="1" x:Name="hoverArea" Visibility="Collapsed" VerticalAlignment="Stretch">
           <AppBarButton Icon="OutlineStar" Command="{StaticResource favoriteCommand}" CommandParameter="{x:Bind PodcastObject, Mode=OneWay}" IsTabStop="False" LabelPosition="Collapsed" VerticalAlignment="Stretch"  />
       </Grid>
   </Grid>
</SwipeControl>
```

当用户轻扫以调用收藏命令时，会调用 Invoked 方法。

```csharp
private void SwipeItem_Invoked(SwipeItem sender, SwipeItemInvokedEventArgs args)
{
    // Favorite the item using the defined command
    var favoriteCommand = Application.Current.Resources["favoriteCommand"] as ICommand;
    favoriteCommand.Execute(PodcastObject);
}
```

#### <a name="pull-to-refresh"></a>下拉以刷新

下拉以刷新使用户可以使用触控下拉数据集合，以检索更多数据。 请参阅[下拉以刷新](pull-to-refresh.md)一文以了解详细信息。

### <a name="pen-accelerators"></a>手写笔快捷方式

手写笔输入类型提供指针输入的精度。 用户可以使用基于手写笔的快捷方式来执行常用操作，如打开上下文菜单。 若要打开上下文菜单，用户可以在按筒状按钮的情况下触碰屏幕，或长按内容。 用户还可以使用手写笔使鼠标悬停在内容上方以更深入地了解 UI（如显示工具提示），或显示辅助悬停操作（与鼠标类似）。

若要针对手写笔输入优化应用，请参阅[手写笔和触笔交互](../input/pen-and-stylus-interactions.md)一文。


## <a name="dos-and-donts"></a>应做事项和禁止事项

* 确保用户可以从所有类型的 UWP 设备访问所有命令。
* 包含上下文菜单，通过它可以访问所有可用于集合项的命令。 
* 为常用命令提供输入快捷方式。 
* 使用 [ICommand 接口](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Input.ICommand)实现命令。 

## <a name="related-topics"></a>相关主题
* [ICommand 接口](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Input.ICommand)
* [菜单和上下文菜单](menus.md)
* [轻扫](swipe.md)
* [下拉以刷新](pull-to-refresh.md)
* [手写笔和触笔交互](../input/pen-and-stylus-interactions.md)
* [针对游戏板和 Xbox 定制应用](../devices/designing-for-tv.md)
