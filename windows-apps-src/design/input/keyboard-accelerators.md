---
author: Karl-Bridge-Microsoft
Description: Learn how accelerator keys can improve the usability and accessibility of UWP apps.
title: 键盘加速键
label: Keyboard accelerators
template: detail.hbs
keywords: 键盘, 加速键, 键盘快捷方式, 辅助功能, 导航, 焦点, 文本, 输入, 用户交互, 手柄, 远程
ms.author: kbridge
ms.date: 10/10/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
pm-contact: chigy
design-contact: miguelrb
doc-status: Draft
ms.localizationpriority: medium
ms.openlocfilehash: ce84debc3422f923c7c88aae1fa216665ef1ef0f
ms.sourcegitcommit: c8f6866100a4b38fdda8394ea185b02d7af66411
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/13/2018
ms.locfileid: "3962937"
---
# <a name="keyboard-accelerators"></a>键盘加速键

![Surface 键盘](images/accelerators/accelerators_hero2.png)

加速键（或键盘加速键）为键盘快捷方式，可以为用户提供一种直观的方式，使其能够调用常用操作或命令而无需导航应用 UI，从而改善 Windows 应用程序的实用功能和辅助功能。

有关使用键盘快捷方式导航 Windows 应用程序的 UI 的详细信息，请参阅[访问键](access-keys.md)主题。

> [!NOTE]
> 对于残疾人士用户而言，键盘是必不可少的工具（请参阅[键盘辅助功能](https://docs.microsoft.com/windows/uwp/accessibility/keyboard-accessibility)），并且对于将键盘认为是与应用交互的较有效方法的用户而言，键盘也非常重要。

## <a name="overview"></a>概述

快捷方式通常包括功能键 F1 到 F12 或标准键与一个或多个配对的修改键（Ctrl、Shift）的某种组合。

> [!NOTE]
> UWP 平台控件具有内置的键盘加速键。 例如，ListView 支持使用 Ctrl+A 来选择列表中的所有项，RichEditBox 支持使用 Ctrl+Tab 在文本框中插入一个制表符。 这些内置键盘加速键称为**控件加速键**，只有当焦点位于元素或其子项之一时才会执行。 你使用此处所讨论的键盘加速键 API 定义的加速键被称为**应用加速键**。

键盘加速键并非适用于每一项操作，但通常与菜单中公开的命令相关联（并且应该用菜单项内容来指定）。 快捷方式也可能与没有等效菜单项的操作相关联。 但是，因为用户依赖于应用程序菜单来发现和了解可用的命令集，所以你应该尝试让加速键的发现尽可能容易（使用标签或既定模式对此有帮助）。

![菜单项标签中所描述的键盘加速键](images/accelerators/accelerators_menuitemlabel.png)  
*菜单项标签中所描述的键盘加速键*

## <a name="when-to-use-keyboard-accelerators"></a>何时使用键盘加速键

我们建议你在 UI 的合适位置指定键盘加速键并在所有自定义控件中支持加速键。

- 对于行动有障碍的用户，包括一次只能按一个键或者使用鼠标有困难的用户，键盘加速键可以让他们更容易地访问应用。**

  具有良好设计的键盘 UI 是软件辅助功能的一个重要方面。 它使具有视力缺陷或行动有障碍的用户能够在应用中导航并与应用的功能交互。 这些用户可能无法操作鼠标，而是依靠各种辅助技术，如键盘增强工具、屏幕键盘、屏幕放大器、屏幕阅读器、语音输入实用工具。 对于这些用户，广泛的命令覆盖面非常重要。

- 对于喜欢通过键盘进行交互的高级用户，键盘加速键可以让他们更轻松地使用应用。

  有经验的用户通常强烈倾向于使用键盘，因为可以更为快速地输入可让基于键盘的命令，而无需将双手从键盘上挪开。 对于这些用户，效率性和一致性体验至关重要；综合性体验仅对常用命令十分重要。

## <a name="specify-a-keyboard-accelerator"></a>指定键盘加速键

使用 [KeyboardAccelerator](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.input.keyboardaccelerator.-ctor) API 在 UWP 应用中创建键盘加速键。 使用这些 API，你不必处理多个 KeyDown 事件即可检测到按下的组合键，并且可以在应用资源中对快捷方式进行本地化。

我们建议你为应用中最常见的操作设置键盘快捷方式并使用菜单项标签或工具提示记录它们。 在此示例中，我们只为重命名和复制命令声明键盘快捷方式。

``` xaml
<CommandBar Margin="0,200" AccessKey="M">
  <AppBarButton 
    Icon="Share" 
    Label="Share" 
    Click="OnShare" 
    AccessKey="S" />
  <AppBarButton 
    Icon="Copy" 
    Label="Copy" 
    ToolTipService.ToolTip="Copy (Ctrl+C)" 
    Click="OnCopy" 
    AccessKey="C">
    <AppBarButton.KeyboardAccelerators>
      <KeyboardAccelerator 
        Modifiers="Control" 
        Key="C" />
    </AppBarButton.KeyboardAccelerators>
  </AppBarButton>

  <AppBarButton 
    Icon="Delete" 
    Label="Delete" 
    Click="OnDelete" 
    AccessKey="D" />
  <AppBarSeparator/>
  <AppBarButton 
    Icon="Rename" 
    Label="Rename" 
    ToolTipService.ToolTip="Rename (F2)" 
    Click="OnRename" 
    AccessKey="R">
    <AppBarButton.KeyboardAccelerators>
      <KeyboardAccelerator 
        Modifiers="None" Key="F2" />
    </AppBarButton.KeyboardAccelerators>
  </AppBarButton>

  <AppBarButton 
    Icon="SelectAll" 
    Label="Select" 
    Click="OnSelect" 
    AccessKey="A" />
  
  <CommandBar.SecondaryCommands>
    <AppBarButton 
      Icon="OpenWith" 
      Label="Sources" 
      AccessKey="S">
      <AppBarButton.Flyout>
        <MenuFlyout>
          <ToggleMenuFlyoutItem Text="OneDrive" />
          <ToggleMenuFlyoutItem Text="Contacts" />
          <ToggleMenuFlyoutItem Text="Photos"/>
          <ToggleMenuFlyoutItem Text="Videos"/>
        </MenuFlyout>
      </AppBarButton.Flyout>
    </AppBarButton>
    <AppBarToggleButton 
      Icon="Save" 
      Label="Auto Save" 
      IsChecked="True" 
      AccessKey="A"/>
  </CommandBar.SecondaryCommands>

</CommandBar>
```

![工具提示中所描述的键盘快捷方式](images/accelerators/accelerators_tooltip.png)  
***工具提示中所描述的键盘快捷方式***

[UIElement](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement) 对象具有一个 [KeyboardAccelerator](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.keyboardaccelerator) 集合，即 [KeyboardAccelerators](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.KeyboardAccelerators)，你可以在其中指定自定义的 KeyboardAccelerator 对象并定义键盘快捷方式的击键：

-   **[Key](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.keyboardaccelerator.Key)** - 用于键盘快捷方式的 [VirtualKey](https://docs.microsoft.com/uwp/api/windows.system.virtualkey)。

-   **[Modifiers](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.keyboardaccelerator.Modifiers)** - 用于键盘快捷方式的 [VirtualKeyModifiers](https://docs.microsoft.com/uwp/api/windows.system.virtualkeymodifiers)。 如果未设置 Modifiers，则默认值是“无”。

> [!NOTE]
> 支持单键（A、Delete、F2、空格键、Esc、多媒体键）快捷方式和多键快捷方式 (Ctrl+Shift+M)。 但是，不支持游戏板虚拟键。

## <a name="scoped-accelerators"></a>限定作用域的快捷方式

一些快捷方式只能在特定范围内使用，而其他快捷方式则可以在应用范围内使用。

例如，Microsoft Outlook 包含以下快捷方式：
-   Ctrl+B、Ctrl+I 和 ESC 只适用于发送电子邮件表单的范围
-   Ctrl+1 和 Ctrl+2 适用于应用范围

### <a name="context-menus"></a>上下文菜单

上下文菜单操作仅影响特定区域或元素，例如文本编辑器中的选定字符或播放列表中的歌曲。 因此，我们建议将上下文菜单项的键盘快捷方式的作用域设置为上下文菜单的父项。

使用 [ScopeOwner](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.keyboardaccelerator.ScopeOwner) 属性指定键盘快捷方式的作用域。 这段代码演示如何使用限定作用域的键盘快捷方式在 ListView 上实现上下文菜单：

``` xaml
<ListView x:Name="MyList">
  <ListView.ContextFlyout>
    <MenuFlyout>
      <MenuFlyoutItem Text="Share" Icon="Share"/>
      <MenuFlyoutItem Text="Copy" Icon="Copy">
        <MenuFlyoutItem.KeyboardAccelerators>
          <KeyboardAccelerator 
            Modifiers="Control" 
            Key="C" 
            ScopeOwner="{x:Bind MyList }" />
        </MenuFlyoutItem.KeyboardAccelerators>
      </MenuFlyoutItem>
      
      <MenuFlyoutItem Text="Delete" Icon="Delete" />
      <MenuFlyoutSeparator />
      
      <MenuFlyoutItem Text="Rename">
        <MenuFlyoutItem.KeyboardAccelerators>
          <KeyboardAccelerator 
            Modifiers="None" 
            Key="F2" 
            ScopeOwner="{x:Bind MyList}" />
        </MenuFlyoutItem.KeyboardAccelerators>
      </MenuFlyoutItem>
      
      <MenuFlyoutItem Text="Select" />
    </MenuFlyout>
    
  </ListView.ContextFlyout>
    
  <ListViewItem>Track 1</ListViewItem>
  <ListViewItem>Alternative Track 1</ListViewItem>

</ListView>
```

MenuFlyoutItem.KeyboardAccelerators 元素的 ScopeOwner 属性会将快捷方式标记为已限定作用域而非全局（默认为 Null 或全局）。 有关更多详细信息，请参阅本主题后面的**解析加速键**部分。

## <a name="invoke-a-keyboard-accelerator"></a>调用键盘加速键 

[KeyboardAccelerator](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.keyboardaccelerator) 对象使用 [UI 自动化 (UIA) 控件模式](https://msdn.microsoft.com/library/windows/desktop/ee671194(v=vs.85).aspx)在调用加速键时采取相应的操作。

UIA [控件模式] 将公开常见的控件功能。 例如，Button 控件可实现 [Invoke](https://msdn.microsoft.com/library/windows/desktop/ee671279(v=vs.85).aspx) 控件模式以支持 Click 事件（通常，通过单击、双击或按 Enter、预定义的键盘快捷方式或某个替代击键组合来调用控件）。 使用键盘快捷方式调用控件时，XAML 框架会查找控件是否实现了 Invoke 控件模式，如果已实现，则会激活该模式（侦听 KeyboardAcceleratorInvoked 事件并不需要该模式）。

在以下示例中，Control+S 会触发 Click 事件，因为按钮会实现 Invoke 模式。

``` xaml 
<Button Content="Save" Click="OnSave">
  <Button.KeyboardAccelerators>
    <KeyboardAccelerator Key="S" Modifiers="Control" />
  </Button.KeyboardAccelerators>
</Button>
```

如果元素实现多个控件模式，则只能通过快捷方式激活一种模式。 控件模式的优先顺序如下所示：
1.  调用（按钮）
2.  切换（复选框）
3.  选择 (ListView)
4.  展开/折叠 (ComboBox) 

如果未找到匹配项，则快捷方式无效，并且会提供调试消息（“未找到此组件的任何自动化模式。 在 Invoked 事件中实现所有所需行为。 在事件处理程序中将 Handled 设置为 true 会取消显示此消息。”）

## <a name="custom-keyboard-accelerator-behavior"></a>自定义键盘快捷方式行为

[KeyboardAccelerator](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.keyboardaccelerator) 对象的 Invoked 事件是在执行快捷方式时激发的。 [KeyboardAcceleratorInvokedEventArgs](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.keyboardacceleratorinvokedeventargs) 事件对象包含以下属性：
- **Handled**（布尔值）：将其设置为 true 可以阻止事件触发控件模式并停止快捷方式事件浮升。 默认值为 false。
- **Element** (DependencyObject)：包含快捷方式的对象。

在此，我们将演示如何定义键盘快捷方式集合以及如何处理 Invoked 事件。

``` xaml
<ListView x:Name="MyListView">
  <ListView.KeyboardAccelerators>
    <KeyboardAccelerator Key="A" Modifiers="Control,Shift" Invoked="SelectAllInvoked" />
    <KeyboardAccelerator Key="F5" Invoked="RefreshInvoked"  />
  </ListView.KeyboardAccelerators>
</ListView>   
```

``` csharp
void SelectAllInvoked (KeyboardAccelerator sender, KeyboardAcceleratorInvokedEventArgs args)
{
  CustomSelectAll(MyListView);
  args.Handled = true;
}

void RefreshInvoked(KeyboardAccelerator sender, KeyboardAcceleratorInvokedEventArgs args)
{
  Refresh(MyListView);
  args.Handled = true;
}
```

## <a name="disable-a-keyboard-accelerator"></a>禁用键盘快捷方式 

如果禁用控件，则也会禁用关联的快捷方式。 在以下示例中，因为 ListView 的 IsEnabled 属性设置为 false，所以无法调用关联的 Ctrl+A 快捷方式。

``` xaml
<ListView >
  <ListView.KeyboardAccelerators>
    <KeyboardAccelerator Key="A" 
      Modifiers="Control"
      Invoked="CustomListViewSelecAllInvoked" />
  </ListView.KeyboardAccelerators>
  
  <TextBox>
    <TextBox.KeyboardAccelerators>
      <KeyboardAccelerator 
        Key="A" 
        Modifiers="Control" 
        Invoked="CustomTextSelecAllInvoked" 
        IsEnabled="False" />
    </TextBox.KeyboardAccelerators>
  </TextBox>

<ListView>
```

父控件和子控件可以共享相同快捷方式。 在此情况下，即使子项具有焦点并且禁用了其快捷方式，也可以调用父控件。

## <a name="screen-readers-and-keyboard-accelerators"></a>屏幕阅读器和键盘快捷方式 

诸如“讲述人”之类的屏幕阅读器可以向用户宣布键盘快捷方式组合键。 默认情况下，这是后面跟有键（由“+”符号分隔）的每个修改键（采用 VirtualModifiers 枚举顺序）。 你可以通过 [AcceleratorKey](https://docs.microsoft.com/uwp/api/windows.ui.xaml.automation.automationproperties.AcceleratorKeyProperty) AutomationProperties 附加属性自定义此项。 如果指定了多个快捷方式，则仅宣布第一个。

在此示例中，AutomationProperty.AcceleratorKey 会返回字符串“Ctrl+Shift+A”：

``` xaml
<ListView x:Name="MyListView">
  <ListView.KeyboardAccelerators>

    <KeyboardAccelerator 
      Key="A" 
      Modifiers="Control,Shift" 
      Invoked="CustomSelectAllInvoked" />
      
    <KeyboardAccelerator 
      Key="F5" 
      Modifiers="None" 
      Invoked="RefreshInvoked" />

  </ListView.KeyboardAccelerators>

</ListView>   
```

> [!NOTE] 
> 设置 AutomationProperties.AcceleratorKey 并不会启用键盘功能，只是向 UIA 框架指示使用了哪些键。

## <a name="common-keyboard-accelerators"></a>常用键盘加速键

我们建议你在 UWP 应用程序中使键盘加速键保持一致。 用户必须记住键盘快捷方式并且期望获得相同（或类似）的结果。

这或许并不总能实现，因为各个应用的功能不同。

| **编辑** | **常用键盘加速键** |
| ------------- | ----------------------------------- |
| 启动编辑模式 | Ctrl + E |
| 选择聚焦的控件或窗口中的所有项 | Ctrl + A |
| 搜索和替换 | Ctrl + H |
| 撤销 | Ctrl + Z |
| 重做 | Ctrl + Y |
| 删除所选内容并将其复制到剪贴板 | Ctrl + X |
| 将所选内容复制到剪贴板 | Ctrl + C、Ctrl + Insert |
| 粘贴剪贴板的内容 | Ctrl + V、Shift + Insert |
| （利用选项）粘贴剪贴板的内容 | Ctrl + Alt + V |
| 重命名项 | F2 |
| 添加新项 | Ctrl + N |
| 添加第二个新项 | Ctrl + Shift + N |
| （利用撤消）删除所选项 | Del、Ctrl+D |
| （不用撤消）删除所选项 | Shift + Del |
| 加粗 | Ctrl + B |
| 加下划线 | Ctrl + U |
| 倾斜 | Ctrl + I |

| **导航** | |
| ------------- | ----------------------------------- |
| 在聚焦的控件或窗口中查找内容 | Ctrl + F |
| 转至下一个搜索结果 | F3 |

| **其他操作** | |
| ------------- | ----------------------------------- |
| 添加收藏夹 | Ctrl + D | 
| 刷新 | F5 或 Ctrl + R | 
| 放大 | Ctrl + + | 
| 缩小 | Ctrl + - | 
| 缩放到默认视图 | Ctrl + 0 | 
| 保存 | Ctrl + S | 
| 关闭 | Ctrl + W | 
| 打印 | Ctrl + P | 

请注意，某些组合对本地化版本的 Windows 无效。 例如，在西班牙语版本的 Windows 中，Ctrl+N 用于改为粗体，而不是 Ctrl+B。 如果对应用进行了本地化，那么我们建议提供本地化的键盘加速键。

## <a name="usability-affordances-for-keyboard-accelerators"></a>键盘加速键的可用性提示

### <a name="tooltips"></a>工具提示

由于 UWP 应用程序的 UI 中一般并不直接说明键盘加速键，因此，你可以通过[工具提示](../controls-and-patterns/tooltips.md)提高可发现性，当用户将焦点移动到某个控件、按住某个控件或将鼠标指针悬停在某个控件上时，会自动显示工具提示。 工具提示可以标识某个控件是否有关联的键盘加速键，如果有，将会标识加速键组合。

**Windows 10，版本 1803年 （2018 年 4 月更新） 和更高版本**

默认情况下，当声明键盘快捷方式时，所有控件 （除了[MenuFlyoutItem](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.MenuFlyoutItem)和[ToggleMenuFlyoutItem](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.togglemenuflyoutitem)） 对应的键组合中都显示工具提示。

> [!NOTE] 
> 如果控件有多个定义的加速键，则显示只有第一个。

![加速键工具提示](images/accelerators/accelerators_tooltip_savebutton_small.png)

*工具提示中的加速键组合*

[按钮](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.button)、 [AppBarButton](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.appbarbutton)，和[AppBarToggleButton](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.appbartogglebutton)对象，键盘加速键附加到控件的默认工具提示。 [MenuFlyoutItem](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.appbarbutton)和[ToggleMenuFlyoutItem](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.togglemenuflyoutitem)） 对象，键盘加速键显示浮出控件文本。

> [!NOTE]
> 指定工具提示 （在以下示例中查看 Button1） 替代此行为。

```xaml
<StackPanel x:Name="Container" Grid.Row="0" Background="AliceBlue">
    <Button Content="Button1" Margin="20"
            Click="OnSave" 
            KeyboardAcceleratorPlacementMode="Auto" 
            ToolTipService.ToolTip="Tooltip">
        <Button.KeyboardAccelerators>
            <KeyboardAccelerator  Key="A" Modifiers="Windows"/>
        </Button.KeyboardAccelerators>
    </Button>
    <Button Content="Button2"  Margin="20"
            Click="OnSave" 
            KeyboardAcceleratorPlacementMode="Auto">
        <Button.KeyboardAccelerators>
            <KeyboardAccelerator  Key="B" Modifiers="Windows"/>
        </Button.KeyboardAccelerators>
    </Button>
    <Button Content="Button3"  Margin="20"
            Click="OnSave" 
            KeyboardAcceleratorPlacementMode="Auto">
        <Button.KeyboardAccelerators>
            <KeyboardAccelerator  Key="C" Modifiers="Windows"/>
        </Button.KeyboardAccelerators>
    </Button>
</StackPanel>
```

![加速键工具提示](images/accelerators/accelerators-button-small.png)

*加速键组合附加到按钮的默认工具提示*

```xaml
<AppBarButton Icon="Save" Label="Save">
    <AppBarButton.KeyboardAccelerators>
        <KeyboardAccelerator Key="S" Modifiers="Control"/>
    </AppBarButton.KeyboardAccelerators>
</AppBarButton>
```

![加速键工具提示](images/accelerators/accelerators-appbarbutton-small.png)

*加速键组合附加到 AppBarButton 的默认工具提示*

```xaml
<AppBarButton AccessKey="R" Icon="Refresh" Label="Refresh" IsAccessKeyScope="True">
    <AppBarButton.Flyout>
        <MenuFlyout>
            <MenuFlyoutItem AccessKey="A" Icon="Refresh" Text="Refresh A">
                <MenuFlyoutItem.KeyboardAccelerators>
                    <KeyboardAccelerator Key="R" Modifiers="Control"/>
                </MenuFlyoutItem.KeyboardAccelerators>
            </MenuFlyoutItem>
            <MenuFlyoutItem AccessKey="B" Icon="Globe" Text="Refresh B" />
            <MenuFlyoutItem AccessKey="C" Icon="Globe" Text="Refresh C" />
            <MenuFlyoutItem AccessKey="D" Icon="Globe" Text="Refresh D" />
            <ToggleMenuFlyoutItem AccessKey="E" Icon="Globe" Text="ToggleMe">
                <MenuFlyoutItem.KeyboardAccelerators>
                    <KeyboardAccelerator Key="Q" Modifiers="Control"/>
                </MenuFlyoutItem.KeyboardAccelerators>
            </ToggleMenuFlyoutItem>
        </MenuFlyout>
    </AppBarButton.Flyout>
</AppBarButton>
```

![加速键工具提示](images/accelerators/accelerators-appbar-menuflyoutitem-small.png)

*加速键组合附加到 MenuFlyoutItem 的文本*

通过使用 [KeyboardAcceleratorPlacementMode](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.KeyboardAcceleratorPlacementMode) 属性控制显示行为，该属性接受两个值：[自动](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.keyboardacceleratorplacementmode)或[隐藏](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.keyboardacceleratorplacementmode)。    

```xaml
<Button Content="Save" Click="OnSave" KeyboardAcceleratorPlacementMode="Auto">
    <Button.KeyboardAccelerators>
        <KeyboardAccelerator Key="S" Modifiers="Control" />
    </Button.KeyboardAccelerators>
</Button>
```

在某些情况下，你可能需要相对于另一个元素（通常是容器对象）显示工具提示。 例如，“透视”控件通过“透视”标头显示 PivotItem 的工具提示。 

下面，我们介绍如何使用 KeyboardAcceleratorPlacementTarget 属性通过网格容器而不是按钮来显示“保存”按钮的键盘加速键组合。

```xaml
<Grid x:Name="Container" Padding="30">
  <Button Content="Save"
    Click="OnSave"
    KeyboardAcceleratorPlacementMode="Auto"
    KeyboardAcceleratorPlacementTarget="{x:Bind Container}">
    <Button.KeyboardAccelerators>
      <KeyboardAccelerator  Key="S" Modifiers="Control" />
    </Button.KeyboardAccelerators>
  </Button>
</Grid>
```

### <a name="labels"></a>标签

在某些情况下，我们建议使用控件的标签来标识该控件是否有关联的键盘加速键，如果有，将会标识加速键组合。 

有些平台控件默认具有此行为，尤其是 [MenuFlyoutItem](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.MenuFlyoutItem) 和 [ToggleMenuFlyoutItem](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.togglemenuflyoutitem) 对象，而 [AppBarButton](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.appbarbutton) 和 [AppBarToggleButton](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.appbartogglebutton) 在出现在 [CommandBar](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.commandbar) 的溢出菜单中时具有此行为。

![菜单项标签中所描述的键盘加速键](images/accelerators/accelerators_menuitemlabel.png)  
*菜单项标签中所描述的键盘加速键*

你可以通过 [MenuFlyoutItem](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.MenuFlyoutItem)、[ToggleMenuFlyoutItem](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.togglemenuflyoutitem)、[AppBarButton](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.appbarbutton) 和 [AppBarToggleButton](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.appbartogglebutton) 控件的 [KeyboardAcceleratorTextOverride](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.appbarbutton.KeyboardAcceleratorTextOverride) 属性覆盖标签的默认加速键文本（无文本时使用一个空格）。 

> [!NOTE] 
> 如果系统无法检测连接的键盘（你可以通过 [KeyboardPresent](https://docs.microsoft.com/uwp/api/windows.devices.input.keyboardcapabilities.KeyboardPresent) 属性自行检查），则不显示覆盖文本。

## <a name="advanced-concepts"></a>高级概念

下面，我们回顾一下键盘加速键的某些低层次方面的内容。

### <a name="when-an-accelerator-is-invoked"></a>调用快捷方式时

快捷方式由两种类型的键组成：修改键和非修改键。 修改键包括 Shift、菜单、Ctrl 和 Windows 键，这些键通过 [VirtualKeyModifiers](http://msdn.microsoft.com/library/windows/apps/xaml/Windows.System.VirtualKeyModifiers) 公开。 非修改键包括任何虚拟键，如 Delete、F3、空格键、Esc 以及所有字母数字和标点符号键。 用户长按一个或多个修改键期间按非修改键时，会调用键盘快捷方式。 例如，如果用户按 Ctrl+Shift+M，则在用户按 M 时，框架将检查修改键（Ctrl 和 Shift），并触发快捷方式（如果存在）。

> [!NOTE]
> 按照设计，快捷方式会自动重复（例如，当用户按 Ctrl+Shift 然后按住 M 时，会重复调用快捷方式，直到松开 M 为止）。 此行为无法修改。

### <a name="input-event-priority"></a>输入事件优先级
输入事件按特定顺序发生，你可以根据应用的要求拦截和处理这些事件。 

#### <a name="the-keydownkeyup-bubbling-event"></a>KeyDown/KeyUp 浮升事件 

在 XAML 中，会像只有一个输入浮升管道那样来处理键击。 KeyDown/KeyUp 事件和字符输入会使用此输入管道。 例如，如果元素具有焦点并且用户按下了某个键，则会对此元素引发 KeyDown 事件，随后对此元素的父项引发此事件，一直向上处理到树顶，直到 args.Handled 属性为 true 为止。

KeyDown 事件也被一些控件用来实现内置的控件快捷方式。 当控件具有键盘快捷方式时，它会处理 KeyDown 事件，这意味着将不会有 KeyDown 事件浮升。 例如，RichEditBox 支持使用 Ctrl+C 进行复制。 当按下 Ctrl 时，KeyDown 事件会被触发并浮升，但是当用户同时按下 C 时，KeyDown 事件会被标记为“已处理”，并且不会被触发（除非将 [UIElement.AddHandler](http://msdn.microsoft.com/library/windows/apps/xaml/Windows.UI.Xaml.UIElement.AddHandler) 的 handledEventsToo 参数设置为 true）。

#### <a name="the-characterreceived-event"></a>CharacterReceived 事件

由于 [CharacterReceived](https://docs.microsoft.com/uwp/api/windows.ui.core.corewindow.CharacterReceived) 事件在 TextBox 等文本控件的 [KeyDown](https://docs.microsoft.com/uwp/api/windows.ui.core.corewindow.KeyDown) 事件之后触发，因此，你可以在 KeyDown 事件处理程序中取消字符输入。

#### <a name="the-previewkeydown-and-previewkeyup-events"></a>PreviewKeyDown 和 PreviewKeyUp 事件

预览输入事件在任何其他事件之前触发。 如果不处理这些事件，则会触发具有焦点的元素的快捷方式，然后触发 KeyDown 事件。 这两个事件均会浮升，直到被处理为止。


![键事件序列](images/accelerators/accelerators_keyevents.png)
***键事件序列***

事件的顺序：

预览 KeyDown 事件...
应用快捷方式、OnKeyDown 方法、KeyDown 事件、父项上的应用快捷方式、父项上的 OnKeyDown 方法、父项上的 KeyDown 事件（浮升至根）...
CharacterReceived 事件 PreviewKeyUp 事件 KeyUpEvents

处理快捷方式事件后，也会将 KeyDown 事件标记为已处理。 KeyUp 事件仍未处理。

### <a name="resolving-accelerators"></a>解析快捷方式

键盘快捷方式事件从具有焦点的元素向上浮升至根。 如果未处理事件，那么 XAML 框架会在浮升路径之外寻找其他未限定作用域的应用快捷方式。

如果使用相同的组合键定义了两个键盘快捷方式，则调用可视化树上找到的第一个键盘快捷方式。

仅当焦点在特定作用域内时，才会调用限定作用域的键盘快捷方式。 例如，在包含数十个控件的 Grid 中，只有当焦点位于此 Grid（作用域所有者）内时，才能为控件调用键盘快捷方式。

### <a name="scoping-accelerators-programmatically"></a>以编程方式限定快捷方式的作用域

[UIElement.TryInvokeKeyboardAccelerator](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.uielement.tryinvokekeyboardaccelerator) 方法会调用元素子树中任何匹配的快捷方式。

[UIElement.OnProcessKeyboardAccelerators](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.uielement.onprocesskeyboardaccelerators) 方法在键盘快捷方式之前执行。 此方法将传递一个 [ProcessKeyboardAcceleratorArgs](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.processkeyboardacceleratoreventargs) 对象，该对象包含键、修改键以及一个指示是否处理了键盘快捷方式的布尔值。 如果键盘快捷方式被标记为已处理，则它会浮升（因此决不会调用外部键盘快捷方式）。

> [!NOTE]
> 无论是否被处理，系统始终会触发 OnProcessKeyboardAccelerators（类似于 OnKeyDown 事件）。 你必须检查事件是否被标记为已处理。

在此示例中，我们使用 OnProcessKeyboardAccelerators 和 TryInvokeKeyboardAccelerator 将键盘快捷方式的作用域限定为 Page 对象：

``` csharp
protected override void OnProcessKeyboardAccelerators(
  ProcessKeyboardAcceleratorArgs args)
{
  if(args.Handled != true)
  {
    this.TryInvokeKeyboardAccelerator(args);
    args.Handled = true;
  }
}
```

### <a name="localize-the-accelerators"></a>本地化快捷方式

我们建议本地化所有键盘快捷方式。 你可以使用标准 UWP 资源 (.resw) 文件和 XAML 声明中的 X:uid 属性来执行此操作。 在此示例中，Windows 运行时将自动加载资源。

![使用 UWP 资源文件进行键盘快捷方式本地化](images/accelerators/accelerators_localization.png)
***使用 UWP 资源文件进行键盘快捷方式本地化***

``` xaml
<Button x:Uid="myButton" Click="OnSave">
  <Button.KeyboardAccelerators>
    <KeyboardAccelerator x:Uid="myKeyAccelerator" Modifiers="Control"/>
  </Button.KeyboardAccelerators>
</Button>
```

### <a name="setup-an-accelerator-programmatically"></a>以编程方式设置快捷方式

下面是以编程方式定义快捷方式的示例：

``` csharp
void AddAccelerator(
  VirtualKeyModifiers keyModifiers, 
  VirtualKey key, 
  TypedEventHandler<KeyboardAccelerator, KeyboardAcceleratorInvokedEventArgs> handler )
  {
    var accelerator = 
      new KeyboardAccelerator() 
      { 
        Modifiers = keyModifiers, Key = key
      };
    accelerator.Invoked = handler;
    this.KeyAccelerators.Add(accelerator);
  }
```

> [!NOTE]
> KeyboardAccelerator 不可共享，并且相同的 KeyboardAccelerator 无法添加到多个元素中。

### <a name="override-keyboard-accelerator-behavior"></a>覆盖键盘加速键行为

你可以处理 [KeyboardAccelerator.Invoked](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.keyboardaccelerator.Invoked) 事件以覆盖默认的 KeyboardAccelerator 行为。

此示例显示了如何在自定义 ListView 控件中覆盖“全选”命令（Ctrl+A 键盘加速键）。 我们还将 Handled 属性设置为 true 以停止以后的事件浮升。

```csharp
public class MyListView : ListView
{
  …
  protected override void OnKeyboardAcceleratorInvoked(KeyboardAcceleratorInvokedEventArgs args) 
  {
    if(args.Accelerator.Key == VirtualKey.A 
      && args.Accelerator.Modifiers == KeyboardModifiers.Control)
    {
      CustomSelectAll(TypeOfSelection.OnlyNumbers); 
      args.Handled = true;
    }
  }
  …
}
```

## <a name="related-articles"></a>相关文章

* [键盘交互](keyboard-interactions.md)
* [访问键](access-keys.md)

**示例**
* [XAML 控件库 (也称为 XamlUiBasics)](https://github.com/Microsoft/Windows-universal-samples/tree/c2aeaa588d9b134466bbd2cc387c8ff4018f151e/Samples/XamlUIBasics)


 

