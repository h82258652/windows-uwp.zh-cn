---
Description: 使用对齐、边距和填充属性来安排页面上元素的布局。
title: 通过对齐、边距和填充调整布局
ms.date: 03/19/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 0d7f702d145740703b9fbc4ca2e7fd8eba8957cc
ms.sourcegitcommit: 26bb75084b9d2d2b4a76d4aa131066e8da716679
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/06/2020
ms.locfileid: "75684466"
---
# <a name="alignment-margin-padding"></a>对齐、边距和填充

在 UWP 应用中，大部分用户界面 (UI) 元素继承自 [FrameworkElement  ](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.FrameworkElement) 类。 每个 FrameworkElement 都有尺寸、对齐、边距和填充属性，这些属性将影响布局行为。 以下指南概要介绍了如何使用这些布局属性，以确保应用的 UI 在任何上下文中均清晰可辨且易于使用。

## <a name="dimensions-height-width"></a>尺寸（高度、宽度）
适当的大小调整将确保所有内容都清晰可辨。 用户应该不必进行滚动或缩放即可看清主要内容。

![显示尺寸的图示](images/dimensions.svg)

- [Height  ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.height) 和 [Width  ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.width) 指定元素的大小。 默认值是数学意义上的 NaN（非数字）。 可以使用固定的值（以[有效像素](../basics/design-and-ui-intro.md#effective-pixels-and-scaling)为单位测量），或者使用“自动”  或[按比例调整大小](layout-panels.md#grid)以实现灵活行为。

- [ActualHeight  ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.actualheight) 和 [ActualWidth  ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.actualwidth) 是只读属性，用于提供元素在运行时的大小。 如果灵活布局放大或缩小，这两个元素的值会在 [SizeChanged  ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.sizechanged) 事件中发生变化。 请注意，[RenderTransform  ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.rendertransform) 不会改变 ActualHeight 和 ActualWidth 的值。

- [MinWidth  ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.minwidth)/[MaxWidth  ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.maxwidth) 和 [MinHeight  ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.minheight)/[MaxHeight  ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.maxheight) 指定用于在灵活调整大小时约束元素大小的值。

- [FontSize  ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.textblock.fontsize) 和其他文本属性控制文本元素的布局大小。 尽管文本元素没有明确声明尺寸，但它们具有计算得到的 ActualWidth 和 ActualHeight。 

## <a name="alignment"></a>符合方式
通过对齐，可以让 UI 看起来整洁、有序和均衡，还可建立视觉层次和关系。

![显示对齐的图示](images/alignment.svg)

- [HorizontalAlignment  ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.horizontalalignment) 和 [VerticalAlignment  ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.verticalalignment) 指定元素在其父容器中的位置。
    - **HorizontalAlignment** 的值是 **Left**、**Center**、**Right** 和 **Stretch**。
    - **VerticalAlignment** 的值是 **Top**、**Center**、**Bottom** 和 **Stretch**。

- Stretch  是这两个属性的默认值，使用此值时，元素将填充父容器为其提供的全部空间。 为 Height 和 Width 设定实际的数值之后，Stretch 值将失效，元素的行为将与使用 Center 值时相同。 某些控件（如按钮）会在其默认样式中替换默认的 Stretch 值。

- [HorizontalContentAlignment  ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.control.horizontalcontentalignment) 和 [VerticalContentAlignment  ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.control.verticalcontentalignment) 指定子元素在容器中的位置。

- Alignment 可能影响布局面板中的剪裁方式。 例如，在使用 `HorizontalAlignment="Left"` 时，如果元素内容的宽度大于 ActualWidth，将从右侧剪裁元素。

- 文本元素使用 [TextAlignment  ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.textalignment) 属性。 一般情况下，建议使用左对齐（默认值）。 有关设置文本样式的详细信息，请参阅[版式](../style/typography.md)。

## <a name="margin-and-padding"></a>边距和填充
边距和填充属性可以防止 UI 看起来过于杂乱或稀疏，还可方便使用某些输入方式，例如手写笔和触控。 下图显示了一个容器及其内容的边距和填充。

![xaml 边距和填充图示](images/xaml-layout-margins-padding.svg)

### <a name="margin"></a>边距
[Margin  ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.margin) 控制元素周围的空白空间量。 Margin 不会增加 ActualHeight 和 ActualWidth 的像素，在点击测试和溯源输入事件中不会被视为元素的一部分。

- 各边的 Margin 值可以是统一的，也可以是不同的。 使用 `Margin="20"` 时，将对元素的左侧、顶部、右侧和底部应用 20 像素的统一边距。 使用 `Margin="0,10,5,25"` 时，将对左侧、顶部、右侧和底部（按此顺序）分别应用这些值。 

- 边距可累加。 如果两个元素都指定 10 像素的统一边距，并且它们在任意方向并列，则这两个元素间的距离为 20 像素。

- 允许负边距。 但是，使用负边距经常导致剪裁，或导致对等的过度绘制，所以使用负边距不是常见的技术。

- 呈现时最后才会约束 Margin 值，由于容器可能剪裁或约束元素，请谨慎设置边距。 设定 Margin 值可能会使元素不呈现出来，因为应用 Margin 后，元素的尺寸可能被约束为 0。

### <a name="padding"></a>填充
[Padding  ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.padding) 控制元素的内边框与其子内容或子元素之间的空间量。 正 Padding 值会降低该元素的内容区域。 

与 Margin 不同，Padding 不是 FrameworkElement 的属性。 有几个类定义会自己的 Padding 属性：

-   [Control.Padding  ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.control.padding)：继承到所有 [Control  ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls) 派生类。 并非所有控件都有内容，对于没有内容的控件，设置属性没有任何效果。 如果控件有边框，填充将应用到该边框以内。
-   [Border.Padding  ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.border.padding)：定义由 [BorderThickness  ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.border.borderthickness)/[BorderBrush  ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.border.borderbrush) 创建的矩形线和 [Child  ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.border.child) 元素之间的空间。
-   [ItemsPresenter.Padding  ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.itemspresenter.padding)：在每个项目周围设置指定的填充，影响项目控件中项目的外观。
-   [TextBlock.Padding  ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.textblock.padding) 和 [RichTextBlock.Padding  ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.richtextblock.padding)：扩展文本元素的文本周围的边界框。 这些文本元素没有背景  ，因此视觉上很难看清。 出于这个原因，请改为在 [Block  ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.documents.block) 容器上使用 [Margin  ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.documents.block.margin) 设置。

在以上的每一种情况下，元素都还有 Margin 属性。 如果同时应用了 Margin 和 Padding，它们可以累加：外部容器和任何内部内容之间显示的距离是边距与填充之和。

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
- 仅对某些主要元素应用度量值，而对其他元素使用灵活布局行为。 当窗口宽度发生更改，这将提供[响应式 UI](responsive-design.md)。

- 如果使用了度量值，则所有尺寸、边距和填充都应以 4 有效像素 (epx) 为增量进行缩放  。 UWP 使用[有效像素和缩放](../basics/design-and-ui-intro.md#effective-pixels-and-scaling)，以 4 的倍数为单位缩放 UI 元素，使应用在所有设备和屏幕上清晰可辨。 使用 4 的倍数值，可以使元素按整数个像素对齐，从而获得最佳呈现效果。

- 对于较小的窗口宽度（小于 640 像素），建议使用 12 epx 装订线；对于较大的窗口宽度，建议使用 24 epx 装订线。

![建议的装订线](images/12-gutter.svg)

## <a name="related-topics"></a>相关主题
* [FrameworkElement.Height  ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.height)
* [FrameworkElement.Width  ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.width)
* [FrameworkElement.HorizontalAlignment  ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.horizontalalignment)
* [FrameworkElement.VerticalAlignment  ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.verticalalignment)
* [FrameworkElement.Margin  ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.margin)
* [Control.Padding  ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.control.padding)
