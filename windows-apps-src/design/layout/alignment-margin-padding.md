---
Description: 使用对齐、 边距和填充属性排列的页面上的元素布局。
title: 布局的对齐、边距和填充
ms.date: 03/19/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 3c7ca724279a6a4d41b1f7757428af8eab403549
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2019
ms.locfileid: "57600932"
---
# <a name="alignment-margin-padding"></a>对齐、边距和填充

在 UWP 应用中，大部分用户界面 (UI) 元素继承自 [**FrameworkElement**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.FrameworkElement) 类。 每个 FrameworkElement 都有尺寸、对齐、边距和填充属性，这些属性将影响布局行为。 以下指南概要介绍了如何使用这些布局属性，以确保应用的 UI 在任何上下文中均清晰可辨且易于使用。

## <a name="dimensions-height-width"></a>尺寸（高度、宽度）
适当的大小调整将确保所有内容都清晰可辨。 用户应该不必进行滚动或缩放即可看清主要内容。

![显示尺寸的图示](images/dimensions.svg)

- [**高度**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.height)并[**宽度**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.width)指定元素的大小。 默认值是数学意义上的 NaN（非数字）。 可以使用固定的值（以[有效像素](../basics/design-and-ui-intro.md#effective-pixels-and-scaling)为单位测量），或者可以针对流行为使用 **Auto** 或[成比例调整大小](layout-panels.md#grid)。

- [**ActualHeight** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.actualheight)并[ **ActualWidth** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.actualwidth)是只读的属性，提供在运行时的元素的大小。 如果流布局放大或缩小，则值会在 [**SizeChanged**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.sizechanged) 事件中发生更改。 请注意，[**RenderTransform**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.rendertransform) 不会更改 ActualHeight 和 ActualWidth 的值。

- [**MinWidth**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.minwidth)/[**MaxWidth** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.maxwidth)并[ **MinHeight** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.minheight) / [**MaxHeight** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.maxheight)指定约束同时允许流畅调整元素大小的值。

- [**FontSize** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.textblock.fontsize)和其他文本属性控制文本元素的布局大小。 尽管文本元素没有明确声明尺寸，但它们具有计算得到的 ActualWidth 和 ActualHeight。 

## <a name="alignment"></a>对齐
通过对齐，可以让 UI 看起来整洁、有序和均衡，此外还可以建立视觉层次和关系。

![显示对齐的图示](images/alignment.svg)

- [**HorizontalAlignment** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.horizontalalignment)并[ **VerticalAlignment** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.verticalalignment)指定应该如何在其父容器内定位元素。
    - **HorizontalAlignment** 的值是 **Left**、**Center**、**Right** 和 **Stretch**。
    - **VerticalAlignment** 的值是 **Top**、**Center**、**Bottom** 和 **Stretch**。

- **Stretch** 是这两个属性的默认值，元素将填充父容器提供给它们的全部空间。 为 Height 和 Width 设定实际的数值之后，Stretch 值将被取消并改为充当 Center 值。 某些控件（如 Button）会在其默认样式中替换默认的 Stretch 值。

- [**HorizontalContentAlignment** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.control.horizontalcontentalignment)并[ **VerticalContentAlignment** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.control.verticalcontentalignment)指定子元素的容器中的位置如何。

- 对齐方式可能会影响布局面板中的剪切。 例如，在使用 `HorizontalAlignment="Left"` 时，如果内容大于 ActualWidth，则会剪切元素的右侧。

- 文本元素使用 [**TextAlignment**](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.textalignment) 属性。 一般情况下，我们建议使用向左对齐（默认值）。 有关设置文本样式的详细信息，请参阅[版式](../style/typography.md)。

## <a name="margin-and-padding"></a>边距和填充
边距和填充属性可以防止 UI 看起来过于杂乱或稀疏，而且还可方便使用某些输入，例如手写笔和触摸。 下面是显示一个容器及其内容的边距和填充的图示。

![xaml 边距和填充图示](images/xaml-layout-margins-padding.svg)

### <a name="margin"></a>边距
[**边距**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.margin)控制元素周围的空白空间量。 对于点击测试和源输入事件，Margin 不会向 ActualHeight 和 ActualWidth 添加像素，也不会被视为元素的一部分。

- Margin 值可以是统一的，也可以是不同的。 在使用 `Margin="20"` 时，将对元素的左侧、顶部、右侧和底部应用 20 像素的统一边距。 在使用 `Margin="0,10,5,25"` 时，将左侧、顶部、右侧和底部（按此顺序）分别应用不同的值。 

- 边距可累加。 如果两个元素都指定 10 像素的统一边距，而且它们在任何方向上均为相邻对等，则两个元素间的距离为 20 像素。

- 允许负边距。 但是，使用负边距经常导致剪裁，或导致对等的过度绘制，所以使用负边距不是常见的技术。

- Margin 值将在最后受到限制，由于容器可以剪裁或限制元素，因此在使用边距时要谨慎。 Margin 值可能会造成元素无法呈现；在应用边距后，元素的尺寸可以限制为 0。

### <a name="padding"></a>填充
[**填充**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.padding)控制元素及其子内容或元素的内部边框之间的空间量。 正 Padding 值会降低该元素的内容区域。 

与 Margin 不同，Padding 不是 FrameworkElement 的属性。 有几个类定义了自己的 Padding 属性：

-   [**Control.Padding**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.control.padding)： 所有继承[**控制**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls)派生的类。 并非所有控件都有内容，因此对一些控件来说，设置属性不会执行任何操作。 如果控件有边框，填充将应用到该边框内部。
-   [**Border.Padding**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.border.padding)： 定义由创建的矩形行之间的空间[ **BorderThickness**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.border.borderthickness)/[**BorderBrush** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.border.borderbrush)并[**子**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.border.child)元素。
-   [**ItemsPresenter.Padding**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.itemspresenter.padding)： 可能会导致在项控件中，将指定的填充每个项周围放置的项的外观。
-   [**TextBlock.Padding** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.textblock.padding)并[ **RichTextBlock.Padding**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.richtextblock.padding)： 展开的文本元素的文本周围的边界框。 这些文本元素没有**背景**，因此很难直观地看见。 出于这个原因，请在 [**Block**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.documents.block) 容器中改为使用[**Margin**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.documents.block.margin) 设置。

在每一种情况下，元素还具有 Margin 属性。 如果同时应用了 Margin 和 Padding，它们可以累加：外部容器和任何内部内容之间的明显距离将是边距加填充。

### <a name="example"></a>示例
让我们看一下 Margin 和 Padding 在真实控件上的效果。 下面介绍了 Grid 内的 TextBox，且 Margin 和 Padding 的默认值为 0。

![边距和填充为 0 的文本框](images/xaml-layout-textbox-no-margins-padding.svg)

下面介绍了 TextBox 和 Grid 与 TextBox 上的 Margin 和 Padding 值相同，如此 XAML 中所示。

```xaml
<Grid BorderBrush="Blue" BorderThickness="4" Width="200">
    <TextBox Text="This is text in a TextBox." Margin="20" Padding="16,24"/>
</Grid>
```

![边距和填充为正值的文本框](images/xaml-layout-textbox-with-margins-padding.svg)


## <a name="style-resources"></a>样式资源
你无需在控件上单独设置每个属性值。 通常更高效的做法是将属性值分组到 [**Style**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Style) 资源，并将 Style 应用到控件。 当你需要将相同的属性值应用于许多控件时更是如此。 有关使用样式的详细信息，请参阅 [XAML 样式](../controls-and-patterns/xaml-styles.md)。

## <a name="general-recommendations"></a>常规建议
- 仅将测量值应用于某些主要元素，并对其他元素使用流布局行为。 当窗口宽度发生更改，这将提供[响应式 UI](responsive-design.md)。

- 如果你确实要使用测量值，则**所有尺寸、边距和填充的增量都应为 4 epx**。 当 UWP 使用[有效像素和缩放](../basics/design-and-ui-intro.md#effective-pixels-and-scaling)以使应用在所有设备和屏幕上清晰可辨，它按 4 的倍数缩放 UI 元素。 通过使用以 4 为增量的值，可以对齐整个像素，从而获得最佳呈现。

- 对于较小的窗口宽度（小于 640 像素），我们建议使用 12 epx 装订线，而对于较大的窗口宽度，我们建议使用 24 epx 装订线。

![建议的装订线](images/12-gutter.svg)

## <a name="related-topics"></a>相关主题
* [**FrameworkElement.Height**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.height)
* [**FrameworkElement.Width**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.width)
* [**FrameworkElement.HorizontalAlignment**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.horizontalalignment)
* [**FrameworkElement.VerticalAlignment**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.verticalalignment)
* [**FrameworkElement.Margin**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.margin)
* [**Control.Padding**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.control.padding)
