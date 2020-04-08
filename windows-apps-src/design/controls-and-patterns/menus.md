---
Description: 菜单和上下文菜单会在用户发出请求时显示命令或选项列表。
title: 菜单和上下文菜单
label: Menus and context menus
template: detail.hbs
ms.date: 04/19/2019
ms.topic: article
ms.custom: RS5, 19H1
keywords: windows 10, uwp
ms.assetid: 0327d8c1-8329-4be2-84e3-66e1e9a0aa60
pm-contact: yulikl
design-contact: kimsea
dev-contact: llongley
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: b008b12c5f92d56c127c5ec8026d305d3d57a869
ms.sourcegitcommit: af4050f69168c15b0afaaa8eea66a5ee38b88fed
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/21/2020
ms.locfileid: "80081681"
---
# <a name="menus-and-context-menus"></a>菜单和上下文菜单

菜单和上下文菜单会在用户发出请求时显示命令或选项列表。 使用菜单浮出控件以显示单个内联菜单。 使用菜单栏在水平行中显示一组菜单，通常位于应用窗口的顶部。 每个菜单都有菜单项和子菜单。

![典型上下文菜单示例](images/contextmenu_rs2_icons.png)

**获取 Windows UI 库**

|  |  |
| - | - |
| ![WinUI 徽标](images/winui-logo-64x64.png) | MenuBar 控件作为 Windows UI 库的一部分提供，该库是一个 Nuget 包，包含 UWP 应用的新控件和 UI 功能  。 有关详细信息（包括安装说明），请参阅 [Windows UI 库概述](https://docs.microsoft.com/uwp/toolkits/winui/)。 |

> **Windows UI 库 API：** [MenuBar 类](/uwp/api/microsoft.ui.xaml.controls.menubar)
>
> **平台 API：** [MenuFlyout 类](/uwp/api/windows.ui.xaml.controls.menuflyout)、[MenuBar 类](/uwp/api/windows.ui.xaml.controls.menubar)、[ContextFlyout 属性](/uwp/api/windows.ui.xaml.uielement.contextflyout)、[FlyoutBase.AttachedFlyout 属性](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.flyoutbase#xaml-attached-properties)

## <a name="is-this-the-right-control"></a>这是正确的控件吗？

菜单和上下文菜单通过在用户不需要使用时对命令进行组织和隐藏，从而节省空间。 如果需要经常使用某一特定命令并希望有可用的空间，请考虑将该命令直接置于其自己的元素（而非菜单）中，以便用户无需遍历菜单即可访问它。

菜单和上下文菜单用于整理命令；若要显示任意内容（如通知）或请求确认，则使用[对话框或浮出控件](dialogs.md)。

### <a name="menubar-vs-menuflyout"></a>MenuBar 与MenuFlyout

若要在附加到画布 UI 元素的浮出控件中显示菜单，请使用 MenuFlyout 控件来承载菜单项。 可调用菜单浮出控件作为常规菜单或上下文菜单。 菜单浮出控件包含单个顶级菜单（和可选的子菜单）。

要在水平行中显示一组多个顶级菜单，请使用菜单栏。 通常可将菜单栏放在应用窗口的顶部。

### <a name="menubar-vs-commandbar"></a>MenuBar 与CommandBar

MenuBar 和 CommandBar 都表示可用于向用户公开命令的表面。 MenuBar 为那些需要的组织或分组功能可能无法通过 CommandBar 来满足的应用提供一种公开命令集的方式，这种方式既快速又简单。

还可以将 MenuBar 与 CommandBar 结合使用。 使用 MenuBar 提供大量命令，使用 CommandBar 突出显示最常用的命令。

## <a name="examples"></a>示例

<table>
<th align="left">XAML 控件库<th>
<tr>
<td><img src="images/xaml-controls-gallery-app-icon-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>如果已安装 <strong style="font-weight: semi-bold">XAML 控件库</strong>应用，请单击此处<a href="xamlcontrolsgallery:/item/MenuFlyout">打开此应用，了解 MenuFlyout 的实际应用</a>。</p>
    <ul>
    <li><a href="https://www.microsoft.com/p/xaml-controls-gallery/9msvh128x2zt">获取 XAML 控件库应用 (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">获取源代码 (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="menus-vs-context-menus"></a>菜单与上下文菜单

菜单和上下文菜单在外观和可以包含的内容方面类似。 事实上，你可使用相同的控件 [MenuFlyout](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.MenuFlyout) 来创建它们。 区别是允许用户访问它的方式。

何时应使用菜单或上下文菜单？

- 如果主机元素是一个按钮或其他一些命令元素（其主要作用是显示其他命令），则使用菜单。
- 如果主机元素是一些具有另一主要用途（如显示文本或图像）的其他类型的元素，则使用上下文菜单。

例如，使用按钮上的菜单为列表提供筛选和排序选项。 在此方案中，按钮控件的主要用途是提供对菜单的访问权限。

![邮件中的菜单示例](images/Mail_Menu.png)

如果要将命令（如剪切、复制和粘贴）添加到某个文本元素，请使用上下文菜单而不是菜单。 在此方案中，文本元素的主要作用是显示和编辑文本；其他命令（如剪切、复制和粘贴）是辅助命令，属于上下文菜单。

![照片库中的上下文菜单示例](images/ContextMenu_example.png)

### <a name="menus"></a>菜单

- 具有始终显示的单个入口点（例如，位于屏幕顶部的“文件”菜单）。
- 通常附加到某个按钮或父菜单项。
- 通过左键单击（或等效操作，例如用手指点击）进行调用。
- 通过 [Flyout](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.button.flyout) 或 [FlyoutBase.AttachedFlyout](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.flyoutbase#xaml-attached-properties) 属性与元素相关联，或在应用窗口顶部的菜单栏中进行分组。

### <a name="context-menus"></a>上下文菜单

- 附加到单个元素并显示辅助命令。
- 通过右键单击（或等效操作，例如用手指按住）进行调用。
- 通过 [ContextFlyout](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.contextflyout) 属性与元素相关联。

## <a name="icons"></a>图标

请考虑提供菜单项图标：

- 最常使用的项。
- 图标为标准或知名的菜单项。
- 图标可明确说明命令功能的菜单项。

对于没有标准可视化的命令可不必提供图标。 加密图标没有帮助，需创建可视的待筛选邮件，并阻止用户关注重要菜单项。

![带图标的示例上下文菜单](images/contextmenu_rs2_icons.png)

````xaml
<MenuFlyout>
  <MenuFlyoutItem Text="Share" >
    <MenuFlyoutItem.Icon>
      <FontIcon Glyph="&#xE72D;" />
    </MenuFlyoutItem.Icon>
  </MenuFlyoutItem>
  <MenuFlyoutItem Text="Copy" Icon="Copy" />
  <MenuFlyoutItem Text="Delete" Icon="Delete" />
  <MenuFlyoutSeparator />
  <MenuFlyoutItem Text="Rename" />
  <MenuFlyoutItem Text="Select" />
</MenuFlyout>
````

> [!TIP]
> MenuFlyoutItem 中图标的大小为 16x16 像素。 如果使用 SymbolIcon、FontIcon 或 PathIcon，图标将自动缩放到正确的大小，且不会失真。 如果使用 BitmapIcon，请确保你的资产为 16x16 像素。  

## <a name="create-a-menu-flyout-or-a-context-menu"></a>创建菜单浮出控件或上下文菜单

若要创建菜单浮出控件或上下文菜单，请使用 [MenuFlyout 类](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.menuflyout)。 通过将 [MenuFlyoutItem](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.menuflyoutitem)、[MenuFlyoutSubItem](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.menuflyoutsubitem)、[ToggleMenuFlyoutItem](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.togglemenuflyoutitem)、[RadioMenuFlyoutItem](https://docs.microsoft.com/uwp/api/microsoft.ui.xaml.controls.radiomenuflyoutitem) 和 [MenuFlyoutSeparator](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.menuflyoutseparator) 对象添加到 MenuFlyout 来定义菜单的内容。

这些对象如下：

- [MenuFlyoutItem](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.menuflyoutitem) - 执行即时操作。
- [MenuFlyoutSubItem](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.menuflyoutsubitem) - 包含菜单项的级联列表。
- [ToggleMenuFlyoutItem](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.togglemenuflyoutitem) - 打开或关闭选项。
- [RadioMenuFlyoutItem](https://docs.microsoft.com/uwp/api/microsoft.ui.xaml.controls.radiomenuflyoutitem) - 在互斥菜单项之间切换。
- [MenuFlyoutSeparator](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.menuflyoutseparator) - 直观地区分菜单项。

此示例将创建 [MenuFlyout](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.menuflyout)，并使用 [ContextFlyout](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.contextflyout) 属性（该属性适用于大多数控件），以显示 MenuFlyout 作为上下文菜单。

````xaml
<Rectangle
  Height="100" Width="100">
  <Rectangle.ContextFlyout>
    <MenuFlyout>
      <MenuFlyoutItem Text="Change color" Click="ChangeColorItem_Click" />
    </MenuFlyout>
  </Rectangle.ContextFlyout>
  <Rectangle.Fill>
    <SolidColorBrush x:Name="rectangleFill" Color="Red" />
  </Rectangle.Fill>
</Rectangle>
````

````csharp
private void ChangeColorItem_Click(object sender, RoutedEventArgs e)
{
    // Change the color from red to blue or blue to red.
    if (rectangleFill.Color == Windows.UI.Colors.Red)
    {
        rectangleFill.Color = Windows.UI.Colors.Blue;
    }
    else
    {
        rectangleFill.Color = Windows.UI.Colors.Red;
    }
}
````

下一个示例几乎完全相同，但该示例使用 [FlyoutBase.ShowAttachedFlyout](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.flyoutbase.showattachedflyout) 属性将其显示为菜单，而不是使用 [ContextFlyout](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.contextflyout) 属性来显示 [MenuFlyout 类](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.menuflyout)作为上下文菜单。

````xaml
<Rectangle
  Height="100" Width="100"
  Tapped="Rectangle_Tapped">
  <FlyoutBase.AttachedFlyout>
    <MenuFlyout>
      <MenuFlyoutItem Text="Change color" Click="ChangeColorItem_Click" />
    </MenuFlyout>
  </FlyoutBase.AttachedFlyout>
  <Rectangle.Fill>
    <SolidColorBrush x:Name="rectangleFill" Color="Red" />
  </Rectangle.Fill>
</Rectangle>
````

````csharp
private void Rectangle_Tapped(object sender, TappedRoutedEventArgs e)
{
    FlyoutBase.ShowAttachedFlyout((FrameworkElement)sender);
}

private void ChangeColorItem_Click(object sender, RoutedEventArgs e)
{
    // Change the color from red to blue or blue to red.
    if (rectangleFill.Color == Windows.UI.Colors.Red)
    {
        rectangleFill.Color = Windows.UI.Colors.Blue;
    }
    else
    {
        rectangleFill.Color = Windows.UI.Colors.Red;
    }
}
````

### <a name="light-dismiss"></a>轻型消除

轻型消除控件（如菜单、上下文菜单和其他浮出控件）会捕获瞬态 UI 内的键盘焦点和游戏板焦点，直到消除为止。 若要为此行为提供视觉提示，Xbox 上的轻型消除控件将绘制覆盖，以便使 UI 范围之外的可见性变暗。 可以使用 [LightDismissOverlayMode](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.flyoutbase.lightdismissoverlaymode) 属性来修改此行为。 默认情况下，瞬态 UI 将在 Xbox（“自动”）上绘制轻型消除覆盖层，但不会绘制其他设备系列  。 你可选择强制覆盖始终为“On”或始终为“Off”   。

```xaml
<MenuFlyout LightDismissOverlayMode="Off" />
```

## <a name="create-a-menu-bar"></a>创建菜单栏

> [!IMPORTANT]
> MenuBar 需要 Windows 10 版本 1809（[SDK 17763](https://developer.microsoft.com/windows/downloads/windows-10-sdk)）或更高版本，或 [Windows UI 库](https://docs.microsoft.com/uwp/toolkits/winui/)。

可以使用相同的元素在菜单栏中创建菜单，就像在菜单浮出控件中一样。 但是，不是在 MenuFlyout 中对 MenuFlyoutItem 对象进行分组，而是在 MenuBarItem 元素中进行分组。 每个 MenuBarItem 都作为顶级菜单添加到 MenuBar。

![菜单栏的示例](images/menu-bar-submenu.png)

> [!NOTE]
> 此示例仅显示如何创建 UI 结构，但不显示任何命令的实现。

```xaml
<muxc:MenuBar>
    <muxc:MenuBarItem Title="File">
        <MenuFlyoutSubItem Text="New">
            <MenuFlyoutItem Text="Plain Text Document"/>
            <MenuFlyoutItem Text="Rich Text Document"/>
            <MenuFlyoutItem Text="Other Formats..."/>
        </MenuFlyoutSubItem>
        <MenuFlyoutItem Text="Open..."/>
        <MenuFlyoutItem Text="Save"/>
        <MenuFlyoutSeparator />
        <MenuFlyoutItem Text="Exit"/>
    </muxc:MenuBarItem>

    <muxc:MenuBarItem Title="Edit">
        <MenuFlyoutItem Text="Undo"/>
        <MenuFlyoutItem Text="Cut"/>
        <MenuFlyoutItem Text="Copy"/>
        <MenuFlyoutItem Text="Paste"/>
    </muxc:MenuBarItem>

    <muxc:MenuBarItem Title="View">
        <MenuFlyoutItem Text="Output"/>
        <MenuFlyoutSeparator/>
        <muxc:RadioMenuFlyoutItem Text="Landscape" GroupName="OrientationGroup"/>
        <muxc:RadioMenuFlyoutItem Text="Portrait" GroupName="OrientationGroup" IsChecked="True"/>
        <MenuFlyoutSeparator/>
        <muxc:RadioMenuFlyoutItem Text="Small icons" GroupName="SizeGroup"/>
        <muxc:RadioMenuFlyoutItem Text="Medium icons" IsChecked="True" GroupName="SizeGroup"/>
        <muxc:RadioMenuFlyoutItem Text="Large icons" GroupName="SizeGroup"/>
    </muxc:MenuBarItem>

    <muxc:MenuBarItem Title="Help">
        <MenuFlyoutItem Text="About"/>
    </muxc:MenuBarItem>
</muxc:MenuBar>
```

## <a name="get-the-sample-code"></a>获取示例代码

- [XAML 控件库示例](https://github.com/Microsoft/Xaml-Controls-Gallery) - 以交互式格式查看所有 XAML 控件。
- [XAML 上下文菜单示例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlContextMenu)

## <a name="related-articles"></a>相关文章

- [MenuFlyout 类](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.menuflyout)
- [MenuBar 类](https://docs.microsoft.com/uwp/api/microsoft.ui.xaml.controls.menubar)
