---
author: mijacobs
Description: "浮出控件是轻型弹出窗口，用于临时显示与用户当前正在执行的操作相关的 UI。"
title: "菜单和上下文菜单"
label: Menus and context menus
template: detail.hbs
translationtype: Human Translation
ms.sourcegitcommit: eb6744968a4bf06a3766c45b73b428ad690edc06
ms.openlocfilehash: f6ce4bc08e3647cd26dc1537bba5499ddb646a49

---
# 菜单和上下文菜单

<link rel="stylesheet" href="https://az835927.vo.msecnd.net/sites/uwp/Resources/css/custom.css"> 

菜单和上下文菜单会在用户发出请求时显示命令或选项列表。

![典型上下文菜单示例](images/controls_contextmenu_singlepane.png)

<div class="important-apis" >
<b>重要的 API</b><br/>
<ul>
<li><a href="https://msdn.microsoft.com/library/windows/apps/dn299030">MenuFlyout 类</a></li>
<li><a href="https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.uielement.contextflyout.aspx">ContextFlyout 属性</a></li>
<li><a href="https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.primitives.flyoutbase.attachedflyout.aspx">FlyoutBase.AttachedFlyout 属性</a></li>
</ul>

</div>
</div>




## 这是正确的控件吗？
菜单和上下文菜单通过在用户不需要使用时对命令进行组织和隐藏，从而节省空间。 如果需要经常使用某一特定命令并希望有可用的空间，请考虑将该命令直接置于其自己的元素（而非菜单）中，以便用户无需遍历菜单即可访问它。 

菜单和上下文菜单用于整理命令；若要显示任意内容（如通知）或请求确认，则使用[对话框或浮出控件](dialogs.md)。  


## 菜单与上下文菜单

菜单和上下文菜单在外观和可以包含的内容方面完全相同。 事实上，你使用相同的控件 [MenuFlyout](https://msdn.microsoft.com/library/windows/apps/dn299030) 来创建它们。 唯一的区别是允许用户访问它的方式。 

何时应使用菜单或上下文菜单？
* 如果主机元素是一个按钮或其他一些命令元素（其主要作用是显示其他命令），则使用菜单。
* 如果主机元素是一些具有另一主要用途（如显示文本或图像）的其他类型的元素，则使用上下文菜单。 

例如，在导航窗格的按钮上使用菜单来提供其他导航选项。 在此方案中，按钮控件的主要用途是提供对菜单的访问权限。 

如果要将命令（如剪切、复制和粘贴）添加到某个文本元素，请使用上下文菜单而不是菜单。 在此方案中，文本元素的主要作用是显示和编辑文本；其他命令（如剪切、复制和粘贴）是辅助命令，属于上下文菜单。 

<div class="side-by-side">
<div class="side-by-side-content">
  <div class="side-by-side-content-left">
   <p><b>菜单</b></p>
<p>
<ul>
<li>具有始终显示的单个入口点（例如，位于屏幕顶部的“文件”菜单）。</li>
<li>通常附加到某个按钮或父菜单项。</li>
<li>通过左键单击（或等效操作，例如用手指点击）进行调用。</li>  
<li>通过 [Flyout](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.button.flyout.aspx) 或 [FlyoutBase.AttachedFlyout](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.primitives.flyoutbase.attachedflyout.aspx) 属性与元素相关联。</li> 
</ul>
</p><br/>

  </div>
  <div class="side-by-side-content-right">
   <p><b>上下文菜单</b></p>
   
<ul>
<li>附加到单个元素，但仅在上下文有意义时才可以访问。</li>
<li>通过右键单击（或等效操作，例如用手指按住）进行调用。</li>
<li>通过 [ContextFlyout](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.uielement.contextflyout.aspx) 属性与元素相关联。  
</ul><br/>

  </div>
</div>
</div>

## 创建菜单或上下文菜单

若要创建菜单或上下文菜单，请使用 [MenuFlyout 类](https://msdn.microsoft.com/library/windows/apps/dn299030)。 通过将 [MenuFlyoutItem](https://msdn.microsoft.com/en-us/library/windows/apps/xaml/windows.ui.xaml.controls.menuflyoutitem.aspx)、[ToggleMenuFlyoutItem](https://msdn.microsoft.com/en-us/library/windows/apps/xaml/windows.ui.xaml.controls.togglemenuflyoutitem.aspx) 和 [MenuFlyoutSeparator](https://msdn.microsoft.com/en-us/library/windows/apps/xaml/windows.ui.xaml.controls.menuflyoutseparator.aspx) 对象添加到 MenuFlyout 来定义菜单的内容。 这些对象如下：
* [MenuFlyoutItem](https://msdn.microsoft.com/en-us/library/windows/apps/xaml/windows.ui.xaml.controls.menuflyoutitem.aspx) - 执行即时操作。
* [ToggleMenuFlyoutItem](https://msdn.microsoft.com/en-us/library/windows/apps/xaml/windows.ui.xaml.controls.togglemenuflyoutitem.aspx) - 打开或关闭选项。
* [MenuFlyoutSeparator](https://msdn.microsoft.com/en-us/library/windows/apps/xaml/windows.ui.xaml.controls.menuflyoutseparator.aspx) - 直观地区分菜单项。


此示例将创建 [MenuFlyout 类](https://msdn.microsoft.com/library/windows/apps/dn299030)，并使用 [ContextFlyout](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.uielement.contextflyout.aspx) 属性（该属性适用于大多数控件），以显示 [MenuFlyout 类](https://msdn.microsoft.com/library/windows/apps/dn299030)作为上下文菜单。

````xaml
<Rectangle 
  Height="100" Width="100" 
  Tapped="Rectangle_Tapped">
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

下一个示例几乎完全相同，但该示例使用 [FlyoutBase.ShowAttachedFlyout](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.primitives.flyoutbase.showattachedflyout) 属性将其显示为菜单，而不是使用 [ContextFlyout](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.uielement.contextflyout.aspx) 属性来显示 [MenuFlyout 类](https://msdn.microsoft.com/library/windows/apps/dn299030)作为上下文菜单。 

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

> **注意**&nbsp;&nbsp;轻型消除控件（如菜单、上下文菜单和其他浮出控件）会捕获瞬态 UI 内的键盘焦点和游戏板焦点，直到消除为止。 若要为此行为提供视觉提示，Xbox 上的轻型消除控件将绘制覆盖，以便使 UI 范围之外的可见性变暗。 可以使用新的 [LightDismissOverlayMode](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.primitives.flyoutbase.lightdismissoverlaymode.aspx) 属性来修改此行为。 默认情况下，瞬态 UI 将在 Xbox（而非其他设备系列）上绘制轻型消除覆盖，不过应用可以选择强制使覆盖始终**打开**或始终**关闭**。
> 
> ```xaml
> <MenuFlyout LightDismissOverlayMode="Off">
```

## Get the samples
*   [XAML UI basics](https://github.com/Microsoft/Windows-universal-samples/blob/master/Samples/XamlUIBasics)<br/>
    See all of the XAML controls in an interactive format.

## Related articles

- [**MenuFlyout class**](https://msdn.microsoft.com/library/windows/apps/dn299030)



<!--HONumber=Aug16_HO3-->


