---
author: mijacobs
Description: A flyout is a lightweight popup that is used to temporarily show UI that is related to what the user is currently doing.
title: 菜单和上下文菜单
label: Menus and context menus
template: detail.hbs
ms.author: mijacobs
ms.date: 05/19/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.assetid: 0327d8c1-8329-4be2-84e3-66e1e9a0aa60
pm-contact: yulikl
design-contact: kimsea
dev-contact: llongley
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: 68c22c12ac5c5dbd90878e8828160e0f56831898
ms.sourcegitcommit: ef5a1e1807313a2caa9c9b35ea20b129ff7155d0
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/08/2018
ms.locfileid: "1638987"
---
# <a name="menus-and-context-menus"></a>菜单和上下文菜单



菜单和上下文菜单会在用户发出请求时显示命令或选项列表。

> **重要 API**：[MenuFlyout 类](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.MenuFlyout)，[ContextFlyout 属性](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.uielement.contextflyout.aspx)，[FlyoutBase.AttachedFlyout 属性](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.primitives.flyoutbase.attachedflyout.aspx)

![典型上下文菜单示例](images/contextmenu_rs2_icons.png)


## <a name="is-this-the-right-control"></a>这是正确的控件吗？
菜单和上下文菜单通过在用户不需要使用时对命令进行组织和隐藏，从而节省空间。 如果需要经常使用某一特定命令并希望有可用的空间，请考虑将该命令直接置于其自己的元素（而非菜单）中，以便用户无需遍历菜单即可访问它。

菜单和上下文菜单用于整理命令；若要显示任意内容（如通知）或请求确认，则使用[对话框或浮出控件](dialogs.md)。  

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

菜单和上下文菜单在外观和可以包含的内容方面完全相同。 事实上，你使用相同的控件 [MenuFlyout](https://msdn.microsoft.com/library/windows/apps/dn299030) 来创建它们。 唯一的区别是允许用户访问它的方式。

何时应使用菜单或上下文菜单？
* 如果主机元素是一个按钮或其他一些命令元素（其主要作用是显示其他命令），则使用菜单。
* 如果主机元素是一些具有另一主要用途（如显示文本或图像）的其他类型的元素，则使用上下文菜单。

例如，在导航窗格的按钮上使用菜单来提供其他导航选项。 在此方案中，按钮控件的主要用途是提供对菜单的访问权限。

![邮件中的菜单示例](images/Mail_Menu.png)

如果要将命令（如剪切、复制和粘贴）添加到某个文本元素，请使用上下文菜单而不是菜单。 在此方案中，文本元素的主要作用是显示和编辑文本；其他命令（如剪切、复制和粘贴）是辅助命令，属于上下文菜单。

![照片库中的上下文菜单示例](images/ContextMenu_example.png) 

<div class="side-by-side">
<div class="side-by-side-content">
  <div class="side-by-side-content-left">
   <p><b>菜单</b></p>
<ul>
<li>具有始终显示的单个入口点（例如，位于屏幕顶部的“文件”菜单）。</li>
<li>通常附加到某个按钮或父菜单项。</li>
<li>通过左键单击（或等效操作，例如用手指点击）进行调用。</li><li>通过 [Flyout](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.button.flyout.aspx) 或 [FlyoutBase.AttachedFlyout](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.primitives.flyoutbase.attachedflyout.aspx) 属性与元素相关联。</li>
</ul>
</div>
  <div class="side-by-side-content-right">
   <p><b>上下文菜单</b></p>

<ul>
<li>附加到单个元素并显示辅助命令。</li>
<li>通过右键单击（或等效操作，例如用手指按住）进行调用。</li><li>通过 [ContextFlyout](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.uielement.contextflyout.aspx) 属性与元素相关联。</li>
</ul>
  </div>
</div>
</div>

## <a name="icons"></a>图标

请考虑提供菜单项图标：

<ul>
<li> 最常使用的项目 </li>
<li> 图标为标准或知名的菜单项 </li>
<li> 图标可明确说明命令功能的菜单项 </li>
</ul>

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
> MenuFlyoutItems 中图标的大小为 16x16 像素。 如果你使用 SymbolIcon、FontIcon 或 PathIcon，图标将自动缩放到正确的大小，且不会失真。 如果你使用 BitmapIcon，请确保你的资产为 16x16 像素。  

## <a name="create-a-menu-or-a-context-menu"></a>创建菜单或上下文菜单

若要创建菜单或上下文菜单，请使用 [MenuFlyout 类](https://msdn.microsoft.com/library/windows/apps/dn299030)。 
通过将 [MenuFlyoutItem](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.menuflyoutitem.aspx)、[ToggleMenuFlyoutItem](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.togglemenuflyoutitem.aspx) 和 [MenuFlyoutSeparator](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.menuflyoutseparator.aspx) 对象添加到 MenuFlyout 来定义菜单的内容。 这些对象如下：
* [MenuFlyoutItem](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.menuflyoutitem.aspx) - 执行即时操作。
* [ToggleMenuFlyoutItem](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.togglemenuflyoutitem.aspx) - 打开或关闭选项。
* [MenuFlyoutSeparator](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.menuflyoutseparator.aspx) - 直观地区分菜单项。


此示例将创建 [MenuFlyout 类](https://msdn.microsoft.com/library/windows/apps/dn299030)，并使用 [ContextFlyout](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.uielement.contextflyout.aspx) 属性（该属性适用于大多数控件），以显示 [MenuFlyout 类](https://msdn.microsoft.com/library/windows/apps/dn299030)作为上下文菜单。

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


> 轻型消除控件（如菜单、上下文菜单和其他浮出控件）会捕获瞬态 UI 内的键盘焦点和游戏板焦点，直到消除为止。 若要为此行为提供视觉提示，Xbox 上的轻型消除控件将绘制覆盖，以便使 UI 范围之外的可见性变暗。 可以使用 [LightDismissOverlayMode](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.primitives.flyoutbase.lightdismissoverlaymode.aspx) 属性来修改此行为。 默认情况下，瞬态 UI 将在 Xbox（而非其他设备系列）上绘制轻型消除覆盖（**自动**），不过应用可以选择强制使覆盖始终**打开**或始终**关闭**。

> ```xaml
> <MenuFlyout LightDismissOverlayMode="Off" />
> ```

## <a name="get-the-sample-code"></a>获取示例代码

- [XAML 控件库示例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics) - 以交互式格式查看所有 XAML 控件。
- [XAML 关联菜单示例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlContextMenu)

## <a name="related-articles"></a>相关文章

- [MenuFlyout 类](https://msdn.microsoft.com/library/windows/apps/dn299030)
