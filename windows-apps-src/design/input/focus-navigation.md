---
title: 适用于键盘、手柄、遥控器和辅助功能工具的焦点导航
Description: ''
label: ''
template: detail.hbs
keywords: 键盘, 游戏控制器, 遥控器, 导航, 定向内部导航, 定向区域, 导航策略, 输入, 用户交互, 辅助功能, 可用性
ms.date: 03/02/2018
ms.topic: article
pm-contact: miguelrb
design-contact: kimsea
dev-contact: niallm
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: 4c76e62a4fcccec91fc6b3a083671ff68af2202e
ms.sourcegitcommit: b11f305dbf7649c4b68550b666487c77ea30d98f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/27/2018
ms.locfileid: "7849352"
---
# <a name="focus-navigation-for-keyboard-gamepad-remote-control-and-accessibility-tools"></a>适用于键盘、手柄、遥控器和辅助功能工具的焦点导航

![键盘、遥控器和方向键](images/dpad-remote/dpad-remote-keyboard.png)

使用焦点导航可以在你的 UWP 应用中提供全面且一致的交互体验，并可为使用键盘的超级用户、残障人士及具有其他辅助功能要求的用户提供自定义控件，还可提供 10 英尺电视屏幕和 Xbox One 体验。

## <a name="overview"></a>概述

焦点导航指的是可以让用户使用键盘、手柄或遥控器进行导航并与 UWP 应用程序的 UI 进行交互的基础机制。

> [!NOTE] 
> 输入设备通常分类为指针设备（例如触控、触摸板、触控笔和鼠标）和非指针设备（例如键盘、手柄和遥控器）。

本主题介绍如何优化 UWP 应用程序并为依赖于非指针输入类型的用户构建自定义交互体验。 

虽然我们对电脑上 UWP 应用中的自定义控件侧重于键盘输入，但精心设计的键盘体验对于触摸键盘和屏幕键盘 (OSK) 等支持辅助功能工具（例如 Windows 讲述人）并支持 10 英尺体验的软件键盘也很重要。

有关在适用于指针设备的 UWP 应用程序中构建自定义体验的指导，请参阅[处理指针输入](handle-pointer-input.md)。

有关构建适用于键盘的应用和体验的更多一般信息，请参阅[键盘交互](keyboard-interactions.md)。

## <a name="general-guidance"></a>一般指南
只有那些需要用户交互的 UI 元素才应支持焦点导航，不需要操作的元素（例如静态图像）不需要键盘焦点。 屏幕阅读器和类似的辅助功能工具即使未包括在焦点导航中，仍会公告这些静态元素。 

请务必记住，和使用鼠标或触控等指针设备进行导航不同，焦点导航是线性的。 在实现焦点导航时，请考虑用户如何与你的应用程序交互以及逻辑导航应是什么。 在大多数情况下，我们建议自定义焦点导航行为遵循用户文化的首选阅读模式。

其他一些焦点导航注意事项包括：
- 控件是否按逻辑分组？
- 是否有重要性更高的控件组？ 
   - 如果是，这些组是否包含子组？
- 布局是需要自定义定向导航（箭头键）还是 Tab 键顺序？

[针对辅助功能设计软件](https://www.microsoft.com/download/details.aspx?id=19262)电子书包含有关*设计逻辑层次结构*的出色章节。

## <a name="2d-directional-navigation-for-keyboard"></a>适用于键盘的 2D 定向导航

控件或控件组的 2D 内部导航区域称为其“定向区域”。 当焦点平移到此对象上时，可以使用键盘箭头键（向左、向右、向上和向下）在定向区域内的子元素之间进行导航。

![定向区域](images/keyboard/directional-area-small.png)

*控件组的 2D 内部导航区域或定向区域*

你可以使用 [XYFocusKeyboardNavigation](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement#Windows_UI_Xaml_UIElement_XYFocusKeyboardNavigation) 属性（值可能为[自动](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.xyfocuskeyboardnavigationmode)、[启用](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.xyfocuskeyboardnavigationmode)或[禁用](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.xyfocuskeyboardnavigationmode)）通过键盘箭头键来管理 2D 内部导航。

> [!NOTE]  
> 此属性不影响 Tab 键顺序。 为了避免令人混淆的导航体验，我们建议*不要*在应用程序的 Tab 键导航顺序中显式指定定向区域的子元素。 有关元素的 Tab 键导航行为的更多详细信息，请参阅 [UIElement.TabFocusNavigation](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement#Windows_UI_Xaml_UIElement_TabFocusNavigation) 和 [TabIndex](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.control#Windows_UI_Xaml_Controls_Control_TabIndex) 属性。  

### <a name="autohttpsdocsmicrosoftcomuwpapiwindowsuixamlinputxyfocuskeyboardnavigationmode-default-behavior"></a>[自动](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.xyfocuskeyboardnavigationmode)（默认行为）

设置为“自动”时，定向导航行为由元素的体系或继承层次结构确定。 如果所有上级都处于默认模式（设置为**自动**），则*不*支持使用键盘进行定向导航。

### [<a name="disabled"></a>禁用](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.xyfocuskeyboardnavigationmode)

将 **XYFocusKeyboardNavigation** 设置为**禁用**可阻止定向导航到该控件及其子元素。

![XYFocusKeyboardNavigation 禁用行为](images/keyboard/xyfocuskeyboardnav-disabled.gif)

*XYFocusKeyboardNavigation 禁用行为*

在此示例中，主 [StackPanel](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.StackPanel) (ContainerPrimary) 的 **XYFocusKeyboardNavigation** 设置为**启用**。 所有子元素都继承此设置，并可使用箭头键导航到这些子元素。 但 B3 和 B4 元素位于辅助 [StackPanel](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.StackPanel) (ContainerSecondary) 中，其 **XYFocusKeyboardNavigation** 设置为**禁用**，这将覆盖主容器并禁用以它为目标以及在其子元素之间进行箭头键导航。

```XAML
<Grid 
    Background="{ThemeResource ApplicationPageBackgroundThemeBrush}" 
    TabFocusNavigation="Cycle">
    <Grid.RowDefinitions>
        <RowDefinition Height="40"/>
        <RowDefinition Height="75"/>
        <RowDefinition Height="*"/>
    </Grid.RowDefinitions>
    <TextBlock Name="KeyPressed"
                Grid.Row="0" 
                FontWeight="ExtraBold" 
                HorizontalTextAlignment="Center"
                TextWrapping="Wrap" 
                Padding="10" />
    <StackPanel Name="ContainerPrimary" 
                XYFocusKeyboardNavigation="Enabled" 
                KeyDown="ContainerPrimary_KeyDown" 
                Orientation="Horizontal" 
                BorderBrush="Green" 
                BorderThickness="2" 
                Grid.Row="1" 
                Padding="10" 
                MaxWidth="200">
        <Button Name="B1" 
                Content="B1" 
                GettingFocus="Btn_GettingFocus" />
        <Button Name="B2" 
                Content="B2" 
                GettingFocus="Btn_GettingFocus" />
        <StackPanel Name="ContainerSecondary" 
                    XYFocusKeyboardNavigation="Disabled" 
                    Orientation="Horizontal" 
                    BorderBrush="Red" 
                    BorderThickness="2">
            <Button Name="B3" 
                    Content="B3" 
                    GettingFocus="Btn_GettingFocus" />
            <Button Name="B4" 
                    Content="B4" 
                    GettingFocus="Btn_GettingFocus" />
        </StackPanel>
    </StackPanel>
</Grid>
```

### [<a name="enabled"></a>启用](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.xyfocuskeyboardnavigationmode)

将 **XYFocusKeyboardNavigation** 设置为**启用**将支持以某个控件及其每个 [UIElement](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.UIElement) 子对象为目标的 2D 定向导航。

设置后，使用箭头键进行的导航将仅限于定向区域内的元素。 Tab 键导航不受影响，因为所有控件仍然可以通过其 Tab 键顺序层次结构进行访问。

![XYFocusKeyboardNavigation 启用行为](images/keyboard/xyfocuskeyboardnav-enabled.gif)

*XYFocusKeyboardNavigation 启用行为*

在此示例中，主 [StackPanel](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.StackPanel) (ContainerPrimary) 的 **XYFocusKeyboardNavigation** 设置为**启用**。 所有子元素都继承此设置，并可使用箭头键导航到这些子元素。 B3 和 B4 元素位于辅助 [StackPanel](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.StackPanel) (ContainerSecondary) 中，其中 **XYFocusKeyboardNavigation** 未设置，该属性随后将继承主容器设置。 B5 元素不在声明的定向区域内，并且不支持箭头键导航，但支持标准 Tab 键导航行为。

```xaml
<Grid
    Background="{ThemeResource ApplicationPageBackgroundThemeBrush}"
    TabFocusNavigation="Cycle">
    <Grid.RowDefinitions>
        <RowDefinition Height="40"/>
        <RowDefinition Height="100"/>
        <RowDefinition Height="*"/>
    </Grid.RowDefinitions>
    <TextBlock Name="KeyPressed"
               Grid.Row="0"
               FontWeight="ExtraBold"
               HorizontalTextAlignment="Center"
               TextWrapping="Wrap"
               Padding="10" />
    <StackPanel Grid.Row="1"
                Orientation="Horizontal"
                HorizontalAlignment="Center">
        <StackPanel Name="ContainerPrimary"
                    XYFocusKeyboardNavigation="Enabled"
                    KeyDown="ContainerPrimary_KeyDown"
                    Orientation="Horizontal"
                    BorderBrush="Green"
                    BorderThickness="2"
                    Padding="5" Margin="5">
            <Button Name="B1"
                    Content="B1"
                    GettingFocus="Btn_GettingFocus" Margin="5" />
            <Button Name="B2"
                    Content="B2"
                    GettingFocus="Btn_GettingFocus" />
            <StackPanel Name="ContainerSecondary"
                        Orientation="Horizontal"
                        BorderBrush="Red"
                        BorderThickness="2"
                        Margin="5">
                <Button Name="B3"
                        Content="B3"
                        GettingFocus="Btn_GettingFocus"
                        Margin="5" />
                <Button Name="B4"
                        Content="B4"
                        GettingFocus="Btn_GettingFocus"
                        Margin="5" />
            </StackPanel>
        </StackPanel>
        <Button Name="B5"
                Content="B5"
                GettingFocus="Btn_GettingFocus"
                Margin="5" />
    </StackPanel>
</Grid>
```

你可以有多个级别的嵌套定向区域。 如果所有父元素都将 XYFocusKeyboardNavigation 设置为“启用”，则将忽略内部导航区域边界。

下面的示例显示了某个元素内不显式支持 2D 定向导航的两个嵌套的定向区域。 在这种情况下，不支持在两个嵌套区域之间进行定向导航。

![XYFocusKeyboardNavigation 启用和嵌套行为](images/keyboard/xyfocuskeyboardnav-enabled-nested1.gif)

*XYFocusKeyboardNavigation 启用和嵌套行为*

下面是一个更复杂的示例，显示了三个嵌套的定向区域：

-   当 B1 获得焦点时，只能导航到 B5（反之亦然），因为有一个 XYFocusKeyboardNavigation 设置为“禁用”的定向区域边界，因此不能通过箭头键到达 B2、B3 和 B4
-   当 B2 获得焦点时，只能导航到 B3（反之亦然），因为定向区域边界阻止通过箭头键导航到 B1、B4 和 B5
-   当 B4 获得焦点时，必须使用 Tab 键才能在控件之间进行导航

![XYFocusKeyboardNavigation 启用和复杂嵌套行为](images/keyboard/xyfocuskeyboardnav-enabled-nested2.gif)

*XYFocusKeyboardNavigation 启用和复杂嵌套行为*

## <a name="tab-navigation"></a>Tab 键导航

虽然箭头键可用于在某个控件或控件组内进行 2D 定向导航，但 Tab 键可用于在 UWP 应用程序内的所有控件之间进行导航。 

所有交互控件默认情况下都支持 Tab 键导航（[IsEnabled](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.control#Windows_UI_Xaml_Controls_Control_IsEnabled) 和 [IsTabStop](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Control#Windows_UI_Xaml_Controls_Control_IsTabStop) 属性为 **true**），其逻辑 Tab 键顺序派生自应用程序中的控件布局。 但是，默认顺序并非一定与视觉顺序对应。 实际的显示位置可能取决于父布局容器以及可以在子元素上进行设置以影响布局的某些属性。

避免使用会让焦点在应用程序中跳来跳去的自定义 Tab 键顺序。 例如，表单中的控件列表的 Tab 键顺序应当是从顶部到底部，从左边到右边（取决于区域设置）。

本部分介绍如何完全自定义该 Tab 键顺序以适应你的应用。

### <a name="set-the-tab-navigation-behavior"></a>设置 Tab 键导航行为

[UIElement](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.UIElement) 的 [TabFocusNavigation](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.UIElement#Windows_UI_Xaml_UIElement_TabFocusNavigation) 属性指定整个对象树（或定向区域）的 Tab 键导航行为。

> [!NOTE]
> 对于不使用 [ControlTemplate](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.controltemplate) 定义其外观的对象，请使用此属性而非 [Control.TabNavigation](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.control#Windows_UI_Xaml_Controls_Control_TabNavigation) 属性。

正如我们在上一部分中所述的那样，为了避免令人混淆的导航体验，我们建议*不要*在应用程序的 Tab 键导航顺序中显式指定定向区域的子元素。 有关元素的 Tab 键导航行为的更多详细信息，请参阅 [UIElement.TabFocusNavigation](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement#Windows_UI_Xaml_UIElement_TabFocusNavigation) 和 [TabIndex](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.control#Windows_UI_Xaml_Controls_Control_TabIndex) 属性。   
> 对于 Windows 10 创意者更新（内部版本 10.0.15063）以前的版本，Tab 键设置仅限于 [ControlTemplate](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.controltemplate) 对象。 有关详细信息，请参阅 [Control.TabNavigation](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.control#Windows_UI_Xaml_Controls_Control_TabNavigation)。

[TabFocusNavigation](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement#Windows_UI_Xaml_UIElement_TabFocusNavigation) 具有一个类型为 [KeyboardNavigationMode](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.keyboardnavigationmode) 的值，可能的值如下（请注意，这些示例不是自定义控件组，不需要使用箭头键进行内部导航）：

- **本地**（默认）   
  在容器内的本地子树上识别 Tab 键索引。 对于本示例，Tab 键顺序为 B1、B2、B3、B4、B5、B6、B7、B1。

   ![“本地”Tab 键导航行为](images/keyboard/tabnav-local.gif)

   *“本地”Tab 键导航行为*

- **一次**  
  容器和所有子元素接收一次焦点。 对于本示例，Tab 键顺序为 B1、B2、B7、B1（还演示了使用箭头键进行内部导航）。

   ![“一次”Tab 键导航行为](images/keyboard/tabnav-once.gif)

   *“一次”Tab 键导航行为*

- **循环**   
  焦点循环回容器内的初始可聚焦元素。 对于本示例，Tab 键顺序为 B1、B2、B3、B4、B5、B6、B2...

   ![“循环”Tab 键导航行为](images/keyboard/tabnav-cycle.gif)

   *“循环”Tab 键导航行为*

此处是上述示例的代码（TabFocusNavigation =“循环”）。

```XAML
<Grid 
    Background="{ThemeResource ApplicationPageBackgroundThemeBrush}" 
    TabFocusNavigation="Cycle">
    <Grid.RowDefinitions>
        <RowDefinition Height="40"/>
        <RowDefinition Height="300"/>
        <RowDefinition Height="*"/>
    </Grid.RowDefinitions>
    <TextBlock Name="KeyPressed"
               Grid.Row="0" 
               FontWeight="ExtraBold" 
               HorizontalTextAlignment="Center"
               TextWrapping="Wrap" 
               Padding="10" />
    <StackPanel Name="ContainerPrimary"
                KeyDown="Container_KeyDown" 
                Orientation="Horizontal" 
                HorizontalAlignment="Center"
                BorderBrush="Green" 
                BorderThickness="2" 
                Grid.Row="1" 
                Padding="10" 
                MaxWidth="200">
        <Button Name="B1" 
                Content="B1" 
                GettingFocus="Btn_GettingFocus" 
                Margin="5"/>
        <StackPanel Name="ContainerSecondary" 
                    KeyDown="Container_KeyDown"
                    XYFocusKeyboardNavigation="Enabled" 
                    TabFocusNavigation ="Cycle"
                    Orientation="Vertical" 
                    VerticalAlignment="Center"
                    BorderBrush="Red" 
                    BorderThickness="2"
                    Padding="5" Margin="5">
            <Button Name="B2" 
                    Content="B2" 
                    GettingFocus="Btn_GettingFocus" 
                    Margin="5"/>
            <Button Name="B3" 
                    Content="B3" 
                    GettingFocus="Btn_GettingFocus" 
                    Margin="5"/>
            <Button Name="B4" 
                    Content="B4" 
                    GettingFocus="Btn_GettingFocus" 
                    Margin="5"/>
            <Button Name="B5" 
                    Content="B5" 
                    GettingFocus="Btn_GettingFocus" 
                    Margin="5"/>
            <Button Name="B6" 
                    Content="B6" 
                    GettingFocus="Btn_GettingFocus" 
                    Margin="5"/>
        </StackPanel>
        <Button Name="B7" 
                Content="B7" 
                GettingFocus="Btn_GettingFocus" 
                Margin="5"/>
    </StackPanel>
</Grid>
```

### [<a name="tabindex"></a>TabIndex](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.control#Windows_UI_Xaml_Controls_Control_TabIndex)

使用 [TabIndex](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.control#Windows_UI_Xaml_Controls_Control_TabIndex) 指定用户使用 Tab 键在控件之间进行导航时元素获得焦点的顺序。 Tab 键索引较低的控件比索引较高的控件先获得焦点。

如果未为控件指定 [TabIndex](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Control#Windows_UI_Xaml_Controls_Control_TabIndex)，系统将根据范围为其分配一个比可视化树中的所有交互控件的当前最高索引值（以及最低优先级）更高的索引值。 

控件的所有子元素均被视为一个范围，如果其中一个元素也具有子元素，则会将其视为另一个范围。 通过选择范围的可视化树上的第一个元素，解决了所有混乱情况。 

若要按照 Tab 键顺序执行某个控件，请将 [IsTabStop](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Control#Windows_UI_Xaml_Controls_Control_IsTabStop) 属性设置为 **false**。

通过设置 [TabIndex](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Control#Windows_UI_Xaml_Controls_Control_TabIndex) 属性覆盖默认的 Tab 键顺序。

> [!NOTE] 
> [TabIndex](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Control#Windows_UI_Xaml_Controls_Control_TabIndex) 的工作方式与 [UIElement.TabFocusNavigation](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement#Windows_UI_Xaml_UIElement_TabFocusNavigation) 和 [Control.TabNavigation](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.control#Windows_UI_Xaml_Controls_Control_TabNavigation) 相同。


此处，我们显示了特定元素上的 [TabIndex](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Control#Windows_UI_Xaml_Controls_Control_TabIndex) 属性如何影响焦点导航。 

![使用 TabIndex 的“本地”Tab 键导航行为](images/keyboard/tabnav-tabindex.gif)

*使用 TabIndex 的“本地”Tab 键导航行为*

在上面的示例中，有两个范围： 
- B1、定向区域 (B2 - B6) 和 B7
- 定向区域 (B2 - B6)

当 B3（在定向区域中）获得焦点时，范围将发生变化，Tab 键导航将转移到标识了后续焦点最佳候选对象的定向区域。 在本例中，B2 后面将是 B4、B5 和 B6。 之后，范围再次变化，焦点将移动到 B1。

下面是此示例的代码。

```xaml
<Grid
    Background="{ThemeResource ApplicationPageBackgroundThemeBrush}"
    TabFocusNavigation="Cycle">
    <Grid.RowDefinitions>
        <RowDefinition Height="40"/>
        <RowDefinition Height="300"/>
        <RowDefinition Height="*"/>
    </Grid.RowDefinitions>
    <TextBlock Name="KeyPressed"
               Grid.Row="0"
               FontWeight="ExtraBold"
               HorizontalTextAlignment="Center"
               TextWrapping="Wrap"
               Padding="10" />
    <StackPanel Name="ContainerPrimary"
                KeyDown="Container_KeyDown"
                Orientation="Horizontal"
                HorizontalAlignment="Center"
                BorderBrush="Green"
                BorderThickness="2"
                Grid.Row="1"
                Padding="10"
                MaxWidth="200">
        <Button Name="B1"
                Content="B1"
                TabIndex="1"
                ToolTipService.ToolTip="TabIndex = 1"
                GettingFocus="Btn_GettingFocus"
                Margin="5"/>
        <StackPanel Name="ContainerSecondary"
                    KeyDown="Container_KeyDown"
                    TabFocusNavigation ="Local"
                    Orientation="Vertical"
                    VerticalAlignment="Center"
                    BorderBrush="Red"
                    BorderThickness="2"
                    Padding="5" Margin="5">
            <Button Name="B2"
                    Content="B2"
                    GettingFocus="Btn_GettingFocus"
                    Margin="5"/>
            <Button Name="B3"
                    Content="B3"
                    TabIndex="3"
                    ToolTipService.ToolTip="TabIndex = 3"
                    GettingFocus="Btn_GettingFocus"
                    Margin="5"/>
            <Button Name="B4"
                    Content="B4"
                    GettingFocus="Btn_GettingFocus"
                    Margin="5"/>
            <Button Name="B5"
                    Content="B5"
                    GettingFocus="Btn_GettingFocus"
                    Margin="5"/>
            <Button Name="B6"
                    Content="B6"
                    GettingFocus="Btn_GettingFocus"
                    Margin="5"/>
        </StackPanel>
        <Button Name="B7"
                Content="B7"
                TabIndex="2"
                ToolTipService.ToolTip="TabIndex = 2"
                GettingFocus="Btn_GettingFocus"
                Margin="5"/>
    </StackPanel>
</Grid>
```

## <a name="2d-directional-navigation-for-keyboard-gamepad-and-remote-control"></a>适用于键盘、手柄和遥控器的 2D 定向导航

对于与 UWP 应用程序的 UI 之间的导航和交互，非指针输入类型（例如键盘、手柄、遥控器）和辅助功能工具（如 Windows 讲述人）共享一种通用的基础机制。

在本部分中，我们将介绍如何在你的应用程序中通过一组支持所有基于焦点的非指针输入类型的导航策略属性指定首选的导航策略并微调焦点导航。

有关构建适用于 Xbox/电视的应用和体验的更多一般信息，请参阅[键盘交互](keyboard-interactions.md)和[针对 Xbox 和电视进行设计](../devices/designing-for-tv.md#xy-focus-navigation-and-interaction)。

### <a name="navigation-strategies"></a>导航策略

> 导航策略适用于键盘、手柄、遥控器和各种辅助功能工具。

以下导航策略属性可以根据按下的箭头键、方向板 (D-pad) 按钮或类似元素影响获得焦点的控件。 

-   XYFocusUpNavigationStrategy
-   XYFocusDownNavigationStrategy
-   XYFocusLeftNavigationStrategy
-   XYFocusRightNavigationStrategy

这些属性的可能值包括[自动](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.xyfocusnavigationstrategy)（默认值）、[NavigationDirectionDistance](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.xyfocusnavigationstrategy)、[Projection](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.xyfocusnavigationstrategy) 或 [RectilinearDistance ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.xyfocusnavigationstrategy)。

如果设为**自动**，则元素的行为基于元素的上级。 如果所有元素均设为**自动**，则将使用**投影**。

> [!NOTE]
> 诸如先前聚焦的元素或靠近导航方向轴之类的其他因素可能会影响结果。

### <a name="projection"></a>投影

“投影”策略会将焦点移至在导航方向上*投影*当前聚焦元素的边缘时所遇到的第一个元素。

在此示例中，每个焦点导航方向均设置为“投影”。 请注意焦点如何绕过 B3 从 B1 向下移动到 B4。 这是因为 B3 不在投影区域内。 另请注意，在从 B1 向左移动时，某个候选焦点未被识别。 这是因为 B2 相对于 B1 的位置排除了作为候选项的 B3。 如果 B3 与 B2 位于同一行，则它将会是向左导航时的可行候选项。 B2 是一个可行候选项，因为它靠近导航方向轴，并且不受任何阻挡。

![投影导航策略](images/keyboard/xyfocusnavigationstrategy-projection.gif)

*投影导航策略*

### <a name="navigationdirectiondistance"></a>NavigationDirectionDistance

NavigationDirectionDistance 策略会将焦点移至最靠近导航方向轴的元素。

与导航方向相对应的边界矩形的边缘会被*扩展*和*投影*以标识候选目标。 遇到的第一个元素被标识为目标。 在有多个候选项的情况下，最近的元素被标识为目标。 如果仍有多个候选项，则最顶层/最左端的元素被标识为候选项。

![NavigationDirectionDistance 导航策略](images/keyboard/xyfocusnavigationstrategy-navigationdirectiondistance.gif)

*NavigationDirectionDistance 导航策略*

### <a name="rectilineardistance"></a>RectilinearDistance

RectilinearDistance 策略会根据 2D 直线距离（[曼哈顿距离](https://en.wikipedia.org/wiki/Taxicab_geometry)）将焦点移至最近的元素。

使用与每个潜在候选项的主距离和辅助距离之和来确定最佳候选项。 因此，如果请求的方向是向上或向下，则会选择左侧的第一个元素；如果请求的方向是向左或向右，则会选择顶部的第一个元素。

![RectilinearDistance 导航策略](images/keyboard/xyfocusnavigationstrategy-rectilineardistance.gif)

*RectilinearDistance 导航策略*

此图像显示了当 B1 具有焦点，并且请求的方向是向下时，B3 是 RectilinearDistance 候选焦点。 对于本例，这是根据下面的计算得出的：
-   距离（B1，B3，向下）为 10 + 0 = 10
-   距离（B1，B2，向下）为 0 + 40 = 30
-   距离（B1，D，向下）为 30 + 0 = 30


## <a name="related-articles"></a>相关文章
- [编程焦点导航](focus-navigation-programmatic.md)
- [键盘交互](keyboard-interactions.md)
- [键盘辅助功能](../accessibility/keyboard-accessibility.md) 



