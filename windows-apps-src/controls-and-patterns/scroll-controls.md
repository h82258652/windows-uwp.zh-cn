---
Description: 平移和滚动使用户可以获取超出屏幕边界的内容。
title: 滚动条指南
ms.assetid: 1BFF0E81-BF9C-43F7-95F6-EFC6BDD5EC31
label: 滚动栏
template: detail.hbs
---
# 滚动栏

<span class="sidebar_heading" style="font-weight: bold;">重要的 API</span>

-   [**ScrollViewer 类**](https://msdn.microsoft.com/library/windows/apps/br209527)
-   [**ZoomMode 属性**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.scrollviewer.zoommode.aspx)

平移和滚动使用户可以获取超出屏幕边界的内容。

滚动查看器控件包含的内容量以适合视区为准，并具有一个或两个滚动条。 触摸手势可用于平移和缩放（滚动条仅在操作期间淡入），指针可用于滚动。 轻拂手势可以利用惯性平移。

**注意** Windows：基于检测到的输入设备，提供两种平移显示模式：适用于触控的平移指示器；适用于其他输入设备（包括鼠标、触摸板、键盘和触笔）的滚动栏。

![标准滚动栏和平移指示器控件的外观示例](images/SCROLLBAR.png)

## 示例

[
            **ScrollViewer**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.scrollviewer.aspx) 能让内容按比其实际大小更小的区域显示。 当滚动查看器内容并非完全可见时，它会显示用户可以移动可见内容区域的滚动条。 包括滚动查看器所有内容在内的区域是*范围*。 内容的可见区域是*视口*。

![说明标准滚动栏控件的屏幕截图](images/ScrollBar_Standard.jpg)

## 创建滚动查看器

此 XAML 显示如何在滚动查看器中放置图像并启用缩放。

```xaml
<ScrollViewer ZoomMode="Enabled" MaxZoomFactor="10" 
              HorizontalScrollMode="Enabled" HorizontalScrollBarVisibility="Visible"
              Height="200" Width="200">
    <Image Source="Assets/Logo.png" Height="400" Width="400"/>
</ScrollViewer>
```

## 控件模板中的 ScrollViewer

ScrollViewer 控件作为其他控件复合部分的形式存在是普遍情况。 仅在主机控件的布局空间限制为比扩展的内容大小更小的时候，ScrollViewer 部件和提供支持的 [**ScrollContentPresenter**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.scrollcontentpresenter.aspx) 类才会显示视口和滚动条。 列表经常会发生此情况，因此 [**ListView**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.listview.aspx) 和 [**GridView**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.gridview.aspx) 模板始终会包括 ScrollViewer。 [
            **TextBox**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.textbox.aspx) 和 [**RichEditBox**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.richeditbox.aspx) 也在其模板中包括了 ScrollViewer。

当 **ScrollViewer** 部件存在于控件中时，主机控件通常对某些能够使内容滚动的输入事件和操作内置了事件处理。 例如，GridView 解释轻扫手势，而这会使内容水平滚动。 主机控件接收的输入事件和原始操作视作由该控件处理，并且低级别事件（例如 [**PointerPressed**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.uielement.pointerpressed.aspx)）不会引发，也不会浮升到任何父容器。 你可以更改某些内置控件处理，方法是覆盖控件类和事件的 **On*** 虚拟方法，或重新模板化控件。 但这两种情况要重现原始默认行为都不简单，这种行为通常已存在，以便控件以预期方式对事件以及用户输入操作和手势做出反应。 因此应该考虑是否正需要引发该输入事件。 你可能想调查是否有不受控件处理的其他输入事件或手势，并在应用或控件交互设计中使用它们。

为让包括 ScrollViewer 的控件能够影响 ScrollViewer 部件的某些行为和属性，ScrollViewer 定义了大量能够在样式中设置并在模板绑定中使用的 XAML 附加属性。 有关附加属性的详细信息，请参阅[附加属性概述](../xaml-platform/attached-properties-overview.md)。

**ScrollViewer XAML 附加属性**

ScrollViewer 定义以下 XAML 附加属性：
- [ScrollViewer.BringIntoViewOnFocusChange](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.scrollviewer.bringintoviewonfocuschange.aspx) 
- [ScrollViewer.HorizontalScrollBarVisibility](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.scrollviewer.horizontalscrollbarvisibility.aspx) 
- [ScrollViewer.HorizontalScrollMode](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.scrollviewer.horizontalscrollmode.aspx)
- [ScrollViewer.IsDeferredScrollingEnabled](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.scrollviewer.isdeferredscrollingenabled.aspx) 
- [ScrollViewer.IsHorizontalRailEnabled](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.scrollviewer.ishorizontalrailenabled.aspx)
- [ScrollViewer.IsHorizontalScrollChainingEnabled](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.scrollviewer.ishorizontalscrollchainingenabled.aspx) 
- [ScrollViewer.IsScrollInertiaEnabled](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.scrollviewer.isscrollinertiaenabled.aspx)
- [ScrollViewer.IsVerticalRailEnabled](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.scrollviewer.isverticalrailenabled.aspx)
- [ScrollViewer.IsVerticalScrollChainingEnabled](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.scrollviewer.isverticalscrollchainingenabled.aspx) 
- [ScrollViewer.IsZoomChainingEnabled](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.scrollviewer.iszoominertiaenabled.aspx)
- [ScrollViewer.IsZoomInertiaEnabled](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.scrollviewer.iszoominertiaenabled.aspx)
- [ScrollViewer.VerticalScrollBarVisibility](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.scrollviewer.verticalscrollbarvisibilityproperty.aspx) 
- [ScrollViewer.VerticalScrollMode](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.scrollviewer.verticalscrollmode.aspx)
- [ScrollViewer.ZoomMode](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.scrollviewer.zoommode.aspx)

这些 XAML 附加属性计划用于 ScrollViewer 处于隐式状态的情况，例如 ScrollViewer 存在于 ListView 或 GridView 的默认模板中，并且你想要在不访问模板部件的情况下影响控件的滚动行为。

例如，下面是如何使 ListView 内置滚动查看器的垂直滚动条始终可见的示例。
```xaml
<ListView ScrollViewer.VerticalScrollBarVisibility="Visible"/> 
```

对于在 XAML 中 ScrollViewer 处于显式状态的情况，正如示例代码所示，你无需使用附加属性语法。 只需使用属性语法，例如 `<ScrollViewer VerticalScrollBarVisibility="Visible"/>`。


## 建议

-   对于超出一条视口边界（垂直或水平）的内容区域使用单轴平移。 对于超出两条视区边界（垂直和水平）的内容区域使用双轴平移。
-   在列表框、下拉列表、文本输入框、网格视图、列表视图和中心控件中使用内置滚动功能。 如果项目过多不能一次显示，用户可以借助这些控件在项目列表中水平或垂直滚动。
-   如果你希望用户可在较大的区域中在两个方向上平移并缩放，请将该图像放置到滚动查看器中。例如，如果你希望用户可以在完整大小的图像（不是调整大小以适应屏幕的图像）中平移和缩放。
-   如果用户将滚动查看一段较长的文本，请配置滚动查看器，使其仅在垂直方向滚动。
-   使用滚动查看器仅包含一个对象。 请注意，该唯一对象可能是版式面板，它反过来包含自身的任意数量的对象。

## 相关主题

**对于开发人员 (XAML)**
* [**ScrollViewer 类**](https://msdn.microsoft.com/library/windows/apps/br209527)


<!--HONumber=Mar16_HO1-->


