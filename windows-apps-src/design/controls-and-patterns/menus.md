---
author: mijacobs
Description: Menus and context menus display a list of commands or options when the user requests them.
title: 菜单和上下文菜单
label: Menus and context menus
template: detail.hbs
ms.author: mijacobs
ms.date: 10/02/2018
ms.topic: article
keywords: windows 10, uwp
ms.assetid: 0327d8c1-8329-4be2-84e3-66e1e9a0aa60
pm-contact: yulikl
design-contact: kimsea
dev-contact: llongley
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: 7a2b58ef505c4b6d045197dee525c5264a7dd518
ms.sourcegitcommit: 70ab58b88d248de2332096b20dbd6a4643d137a4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/01/2018
ms.locfileid: "5927884"
---
# <a name="menus-and-context-menus"></a>菜单和上下文菜单

菜单和上下文菜单会在用户发出请求时显示命令或选项列表。 使用菜单浮出控件显示单个，内联菜单。 使用菜单栏显示在水平行中，通常在应用窗口顶部菜单的一组。 每个菜单可以具有菜单项和子菜单。

![典型上下文菜单示例](images/contextmenu_rs2_icons.png)

| **获取 Windows UI 库** |
| - |
| 此控件是 Windows UI 库，包含新的控件和适用于 UWP 应用的 UI 功能的 NuGet 包的一部分。 有关详细信息，包括安装说明，请参阅[Windows UI 库概述](https://docs.microsoft.com/uwp/toolkits/winui/)。 |

| **平台 Api** | **Windows UI 库 Api** |
| - | - |
| [MenuFlyout 类](/uwp/api/windows.ui.xaml.controls.menuflyout)，[菜单栏类](/uwp/api/windows.ui.xaml.controls.menubar)， [ContextFlyout 属性](/uwp/api/windows.ui.xaml.uielement.contextflyout)， [FlyoutBase.AttachedFlyout 属性](/uwp/api/windows.ui.xaml.controls.primitives.flyoutbase.attachedflyout) | [菜单栏类](/uwp/api/microsoft.ui.xaml.controls.menubar) |

## <a name="is-this-the-right-control"></a>这是正确的控件吗？

菜单和上下文菜单通过在用户不需要使用时对命令进行组织和隐藏，从而节省空间。 如果需要经常使用某一特定命令并希望有可用的空间，请考虑将该命令直接置于其自己的元素（而非菜单）中，以便用户无需遍历菜单即可访问它。

菜单和上下文菜单用于整理命令; 在若要显示任意内容，例如通知或确认请求使用[对话框或浮出控件](dialogs.md)。

### <a name="menubar-vs-menuflyout"></a>MenuFlyout 与菜单栏

若要显示菜单浮出控件附加到的画布上的 UI 元素中，使用 MenuFlyout 控件托管菜单项。 作为常规菜单或上下文菜单，你可以调用菜单浮出控件。 菜单浮出控件承载单个顶级菜单 （和可选的子菜单）。

若要在水平行显示多个顶级菜单的一组，请使用菜单栏。 你通常定位在应用窗口顶部的菜单栏。

### <a name="menubar-vs-commandbar"></a>菜单栏与 CommandBar

菜单栏和命令栏两者都表示可用于向用户公开的命令图面。 在菜单栏提供了一种快速而简单的方式来公开，可能需要更多的组织或分组比 CommandBar 允许的应用的命令集。

你还可以将菜单栏 CommandBar 结合使用。 在菜单栏用于提供大部分命令，而 CommandBar 将突出显示的最常用的命令。

## <a name="examples"></a>示例

<table>
<th align="left">XAML 控件库<th>
<tr>
<td><img src="images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>如果已安装 <strong style="font-weight: semi-bold">XAML 控件库</strong>应用，请单击此处<a href="xamlcontrolsgallery:/item/MenuFlyout">打开此应用，了解 MenuFlyout 的实际应用</a>。</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">获取 XAML 控件库应用 (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics">获取源代码 (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="menus-vs-context-menus"></a>菜单与上下文菜单

菜单和上下文菜单在外观和它们可以包含类似。 事实上，你可以使用相同的控件， [MenuFlyout](https://msdn.microsoft.com/library/windows/apps/dn299030)，创建它们。 不同之处在于，允许用户访问它的方式。

何时应使用菜单或上下文菜单？

- 如果主机元素是一个按钮或其他一些命令元素（其主要作用是显示其他命令），则使用菜单。
- 如果主机元素是一些具有另一主要用途（如显示文本或图像）的其他类型的元素，则使用上下文菜单。

例如，使用在按钮上菜单提供筛选和排序列表的选项。 在此方案中，按钮控件的主要用途是提供对菜单的访问权限。

![邮件中的菜单示例](images/Mail_Menu.png)

如果要将命令（如剪切、复制和粘贴）添加到某个文本元素，请使用上下文菜单而不是菜单。 在此方案中，文本元素的主要作用是显示和编辑文本；其他命令（如剪切、复制和粘贴）是辅助命令，属于上下文菜单。

![照片库中的上下文菜单示例](images/ContextMenu_example.png)

### <a name="menus"></a>菜单

- 具有始终显示的单个入口点（例如，位于屏幕顶部的“文件”菜单）。
- 通常附加到某个按钮或父菜单项。
- 通过左键单击（或等效操作，例如用手指点击）进行调用。
- 在应用窗口顶部菜单栏中分组是否与其[浮出控件](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.button.flyout.aspx)或[FlyoutBase.AttachedFlyout](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.primitives.flyoutbase.attachedflyout.aspx)属性，通过某个元素关联。

### <a name="context-menus"></a>上下文菜单

- 附加到单个元素并显示辅助命令。
- 通过右键单击（或等效操作，例如用手指按住）进行调用。
- 通过 [ContextFlyout](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.uielement.contextflyout.aspx) 属性与元素相关联。

## <a name="icons"></a>图标

请考虑提供菜单项图标：

- 最常使用的项目。
- 图标为标准或知名的菜单项。
- 图标可明确说明命令功能的菜单项。

对于没有标准可视化的命令可不必提供图标。 加密图标没有帮助，创建可视的待筛选邮件，并阻止用户关注重要菜单项。

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
> MenuFlyoutItem 中的大小是图标的 16x16 像素。 如果你使用 SymbolIcon、 FontIcon 或 PathIcon，图标自动缩放到正确的大小，且不会失真。 如果你使用 BitmapIcon，请确保你的资产为 16x16 像素。  

## <a name="create-a-menu-flyout-or-a-context-menu"></a>创建菜单浮出控件或上下文菜单

若要创建菜单浮出控件或上下文菜单，你可以使用[MenuFlyout 类](https://msdn.microsoft.com/library/windows/apps/dn299030)。 
通过将 [MenuFlyoutItem](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.menuflyoutitem.aspx)、[ToggleMenuFlyoutItem](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.togglemenuflyoutitem.aspx) 和 [MenuFlyoutSeparator](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.menuflyoutseparator.aspx) 对象添加到 MenuFlyout 来定义菜单的内容。

这些对象如下：

- [MenuFlyoutItem](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.menuflyoutitem.aspx) - 执行即时操作。
- [ToggleMenuFlyoutItem](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.togglemenuflyoutitem.aspx) - 打开或关闭选项。
- [MenuFlyoutSeparator](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.menuflyoutseparator.aspx) - 直观地区分菜单项。

此示例创建[MenuFlyout](https://msdn.microsoft.com/library/windows/apps/dn299030)并[ContextFlyout](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.uielement.contextflyout.aspx)属性，该属性适用于大多数控件，用于显示为上下文菜单的 MenuFlyout。

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

下一个示例几乎相同，但是该示例使用 [FlyoutBase.ShowAttachedFlyout](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.primitives.flyoutbase.showattachedflyout) 属性将 [MenuFlyout 类](https://msdn.microsoft.com/library/windows/apps/dn299030)显示为菜单，而不是使用 [ContextFlyout](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.uielement.contextflyout.aspx) 属性将其显示为上下文菜单。

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

轻型消除控件（如菜单、上下文菜单和其他浮出控件）会捕获瞬态 UI 内的键盘焦点和游戏板焦点，直到消除为止。 若要为此行为提供视觉提示，Xbox 上的轻型消除控件将绘制覆盖，以便使 UI 范围之外的可见性变暗。 可以使用 [LightDismissOverlayMode](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.primitives.flyoutbase.lightdismissoverlaymode.aspx) 属性来修改此行为。 默认情况下，瞬态 UI 将在 Xbox（而非其他设备系列）上绘制轻型消除覆盖（**自动**），不过应用可以选择强制使覆盖始终**打开**或始终**关闭**。

```xaml
<MenuFlyout LightDismissOverlayMode="Off" />
```

## <a name="create-a-menu-bar"></a>创建菜单栏

> **预览**： 菜单栏需要的[最新的 Windows 10 Insider Preview 版本和 SDK](https://insider.windows.com/for-developers/)或[Windows UI 库](https://docs.microsoft.com/uwp/toolkits/winui/)。

你可以使用相同的元素来创建如菜单浮出控件中所示的菜单栏中的菜单。 但是，而不是组合 MenuFlyout MenuFlyoutItem 对象，你将它们组合 MenuBarItem 元素中。 每个 MenuBarItem 作为顶级菜单添加到在菜单栏。

![菜单栏示例](images/menu-bar-submenu.png)

> [!NOTE]
> 本示例显示仅是说明了如何创建 UI 结构，但不是显示任何命令的实现。

```xaml
<MenuBar>
    <MenuBarItem Title="File">
        <MenuFlyoutSubItem Text="New">
            <MenuFlyoutItem Text="Plain Text Document"/>
            <MenuFlyoutItem Text="Rich Text Document"/>
            <MenuFlyoutItem Text="Other Formats..."/>
        </MenuFlyoutSubItem>
        <MenuFlyoutItem Text="Open..."/>
        <MenuFlyoutItem Text="Save">
        <MenuFlyoutSeparator />
        <MenuFlyoutItem Text="Exit"/>
    </MenuBarItem>

    <MenuBarItem Title="Edit">
        <MenuFlyoutItem Text="Undo"/>
        <MenuFlyoutItem Text="Cut"/>
        <MenuFlyoutItem Text="Copy"/>
        <MenuFlyoutItem Text="Paste"/>
    </MenuBarItem>

    <MenuBarItem Title="Help">
        <MenuFlyoutItem Text="About"/>
    </MenuBarItem>
</MenuBar>
```

## <a name="get-the-sample-code"></a>获取示例代码

- [XAML 控件库示例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics) - 以交互式格式查看所有 XAML 控件。
- [XAML 关联菜单示例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlContextMenu)

## <a name="related-articles"></a>相关文章

- [MenuFlyout 类](https://msdn.microsoft.com/library/windows/apps/dn299030)
- [菜单栏类](/uwp/api/Windows.UI.Xaml.Controls.MenuBar)
