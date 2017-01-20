---
author: Jwmsft
Description: "使用对齐、边距和填充来影响页面的元素布局。"
title: "适用于通用 Windows 平台 (UWP) 应用的对齐、边距和填充"
ms.assetid: 9412ABD4-3674-4865-B07D-64C7C26E4842
label: Alignment, margin, and padding
template: detail.hbs
op-migration-status: ready
translationtype: Human Translation
ms.sourcegitcommit: a3924fef520d7ba70873d6838f8e194e5fc96c62
ms.openlocfilehash: f6c8665c935380078efd2e8ea9d225967cf45eba

---
# <a name="alignment-margin-and-padding"></a>对齐、边距和填充

除了维度属性（宽度、高度和约束），元素还可以具有对齐、边距和填充属性，当元素经过布局传递并在 UI 中呈现时，这些属性可影响布局行为。 当定位 [**FrameworkElement**](https://msdn.microsoft.com/library/windows/apps/br208706) 对象时，一些对齐、边距、填充和维度属性之间的关系有通常的逻辑流，即根据情况有时使用有时忽略这些值。

## <a name="alignment-properties"></a>对齐属性

[**HorizontalAlignment**](https://msdn.microsoft.com/library/windows/apps/br208720) 和 [**VerticalAlignment**](https://msdn.microsoft.com/library/windows/apps/br208749) 属性描述子元素应当如何在父元素的已分配布局空间中定位。 通过一起使用这些属性，容器的布局逻辑可以在容器（面板或控件）内定位子元素。 对齐属性旨在向自适应布局容器提示所需布局，所以基本上它们在 [**FrameworkElement**](https://msdn.microsoft.com/library/windows/apps/br208706) 子元素上设置，并由另一个 **FrameworkElement** 容器父元素解释。 对齐值可指定元素是与某个方向的两个边缘中的其中一个对齐，还是与中心对齐。 但是，这两个对齐属性的默认值为 **Stretch**。 使用 **Stretch** 对齐，元素可以填充在布局中提供给它们的空间。 **Stretch** 为默认值，所以在没有明确度量或没有来自布局的度量传递的 [**DesiredSize**](https://msdn.microsoft.com/library/windows/apps/br208921) 值的情况下，更易于使用自适应布局技术。 使用此默认值，在你调整每个容器的大小之前，不存在明确高度/宽度不适合容器且进行剪裁的风险。

> [!NOTE]
> 作为常规布局原则，最好仅将度量应用于特定关键元素，并为其他元素使用自适应布局行为。 当用户调整顶部应用窗口的大小（通常随时可能发生）时，这可提供灵活的布局行为。

 
如果自适应容器内有 [**Height**](https://msdn.microsoft.com/library/windows/apps/br208718) 和 [**Width**](https://msdn.microsoft.com/library/windows/apps/br208751) 值或剪裁，则即使 **Stretch** 设置为对齐值，布局仍然由其容器的行为控制。 在面板中，已由 **Height** 和 **Width** 消除的 **Stretch** 值的操作如同值为 **Center** 一样。

如果没有自然或经计算的高度和宽度值，这些维度值在数学上为 **NaN**（不是数字）。 元素在等待它们的布局容器以给予它们维度。 在运行布局后，使用 **Stretch** 对齐的位置将有 [**ActualHeight**](https://msdn.microsoft.com/library/windows/apps/br208707) 和 [**ActualWidth**](https://msdn.microsoft.com/library/windows/apps/br208709) 属性的值。 **NaN** 值保留在子元素的 [**Height**](https://msdn.microsoft.com/library/windows/apps/br208718) 和 [**Width**](https://msdn.microsoft.com/library/windows/apps/br208751) 中以便自适应行为再次运行，例如，如果与布局相关的更改（例如应用窗口大小调整）导致另一个布局周期。

文本元素（例如 [**TextBlock**](https://msdn.microsoft.com/library/windows/apps/br209652)）通常不具有明确声明的宽度，但它们有可使用 [**ActualWidth**](https://msdn.microsoft.com/library/windows/apps/br208709) 查询的经计算的宽度，而且该宽度还会取消 **Stretch** 对齐。 （[**FontSize**](https://msdn.microsoft.com/library/windows/apps/br209657) 属性和其他文本属性以及文本本身，已提示所需的布局大小。 通常情况下不需要拉伸文本。）作为控件中的内容使用的文本有相同的效果；需要呈现的文本状态导致需要计算 **ActualWidth**，而且这还会将所需宽度和大小减小到包含的控件。 根据每行字号、分行符号和其他文本属性，文本元素还具有 [**ActualHeight**](https://msdn.microsoft.com/library/windows/apps/br208707)。

面板（例如 [**Grid**](https://msdn.microsoft.com/library/windows/apps/br242704)）已拥有布局的其他逻辑（行和列定义，以及附加的属性，例如在元素上设置以指示要在其中进行绘制的单元格的 [**Grid.Row**](https://msdn.microsoft.com/library/windows/apps/hh759795)）。 在该情况下，对齐属性影响内容在该单元格的区域内的对齐方式，但单元格结构和大小调整由 **Grid** 上的设置控制。

项目控件有时显示项目的基本类型为数据的项目。 这涉及到 [**ItemsPresenter**](https://msdn.microsoft.com/library/windows/apps/br242843)。 尽管数据本身不是 [**FrameworkElement**](https://msdn.microsoft.com/library/windows/apps/br208706) 派生的类型，但 **ItemsPresenter** 是，所以你可以为演示者设置 [**HorizontalAlignment**](https://msdn.microsoft.com/library/windows/apps/br208720) 和 [**VerticalAlignment**](https://msdn.microsoft.com/library/windows/apps/br208749)，而且该对齐在项目控件中显示时适用于数据项目。

对齐属性仅与父布局容器的维度中有附加空间可用的情况相关。 如果布局容器已剪裁内容，则对齐可能影响剪裁将适用的元素区域。 例如，如果你设置 `HorizontalAlignment="Left"`，则元素的正确大小将受到剪裁。

## <a name="margin"></a>边距

[**Margin**](https://msdn.microsoft.com/library/windows/apps/br208724) 属性描述元素和其对等之间在布局情境中的距离，以及元素和包含元素的容器内容区域之间的距离。 如果你将元素视为边界框或矩形，其中维度为 [**ActualHeight**](https://msdn.microsoft.com/library/windows/apps/br208707) 和 [**ActualWidth**](https://msdn.microsoft.com/library/windows/apps/br208709)，则 **Margin** 布局适用于该矩形的外部，而且不会将像素添加到 **ActualHeight** 和 **ActualWidth**。 出于点击测试和源输入事件的目的，边距同样不视为元素的一部分。

在常规布局行为中，最后约束 [**Margin**](https://msdn.microsoft.com/library/windows/apps/br208724) 值的组件，而且仅在 [**Height**](https://msdn.microsoft.com/library/windows/apps/br208718) 和 [**Width**](https://msdn.microsoft.com/library/windows/apps/br208751) 已约束为 0 后进行约束。 所以，当容器已剪裁或约束元素时，请小心处理边距；否则，你的边距可能导致元素无法显示以进行呈现（因为其维度之一已在应用边距后约束为 0）。

通过使用语法（例如 `Margin="20"`），边距值可以是统一的。 借助此语法，20 像素的统一边距将应用到元素，左侧、顶部、右侧和底部各有 20 像素的边距。 边距值还可以采用四个不同值的形式，每个值描述一个要应用到左侧、顶部、右侧和底部（采用该顺序）的不同边距。 例如，`Margin="0,10,5,25"`。 [**Margin**](https://msdn.microsoft.com/library/windows/apps/br208724) 属性的基本类型是 [**Thickness**](https://msdn.microsoft.com/library/windows/apps/br208864) 结构，此结构具有将 [**Left**](https://msdn.microsoft.com/library/windows/apps/hh673893)、[**Top**](https://msdn.microsoft.com/library/windows/apps/hh673840)、[**Right**](https://msdn.microsoft.com/library/windows/apps/hh673881) 和 [**Bottom**](https://msdn.microsoft.com/library/windows/apps/hh673775) 值作为独立 **Double** 值保留的属性。

边距可累加。 例如，如果两个元素中的每一个都指定 10 像素的统一边距，而且它们在任何方向上均为相邻对等，则元素间的距离为 20 像素。

允许负边距。 但是，使用负边距经常导致剪裁，或导致对等的过度绘制，所以使用负边距不是常见的技术。

[**Margin**](https://msdn.microsoft.com/library/windows/apps/br208724) 属性的正确使用可很好地控制元素及其相邻元素和子元素的呈现位置。 当你使用元素拖动在 Visual Studio 中的 XAML 设计器内定位元素时，你将看到经修改的 XAML 通常具有用于该元素的 **Margin** 的值，这些值曾用于将你的定位更改序列化回 XAML 中。

[**Block**](https://msdn.microsoft.com/library/windows/apps/br244379) 类（[**Paragraph**](https://msdn.microsoft.com/library/windows/apps/br244503) 的基类）也有 [**Margin**](https://msdn.microsoft.com/library/windows/apps/jj191725) 属性。 它对该 **Paragraph** 在其父容器（通常为 [**RichTextBlock**](https://msdn.microsoft.com/library/windows/apps/br227565) 或 [**RichEditBox**](https://msdn.microsoft.com/library/windows/apps/br227548) 对象）中的定位方式以及多个段落相对于[**RichTextBlock.Blocks**](https://msdn.microsoft.com/library/windows/apps/br244347) 集合中的其他 **Block** 对等的定位方式有相似的影响。

## <a name="padding"></a>填充

**Padding** 属性描述元素和任何子元素或它所包含的内容之间的距离。 如果是允许多个子元素的元素，则将内容视为包含所有内容的单个边界框。 例如，如果有包含两个项目的 [**ItemsControl**](https://msdn.microsoft.com/library/windows/apps/br242803)，则 [**Padding**](https://msdn.microsoft.com/library/windows/apps/br209459) 应用于包含这些项目的边界框周围。 当要为该容器进行度量和排列传递计算时，**Padding** 从可用大小中减去，并且在容器本身为它所包含的内容经过布局传递时，它是所需大小值的一部分。 和 [**Margin**](https://msdn.microsoft.com/library/windows/apps/br208724) 不同，**Padding** 不是 [**FrameworkElement**](https://msdn.microsoft.com/library/windows/apps/br208706) 的属性，而且事实上有多个类各自定义其自身的 **Padding** 属性：

-   [**Control.Padding**](https://msdn.microsoft.com/library/windows/apps/br209459)：继承到所有 [**Control**](https://msdn.microsoft.com/library/windows/apps/br209390) 派生的类。 并非所有控件都有内容，因此对一些控件来说（例如 [**AppBarSeparator**](https://msdn.microsoft.com/library/windows/apps/dn279268)），设置属性不执行任何操作。 如果控件有边框（请参阅 [**Control.BorderThickness**](https://msdn.microsoft.com/library/windows/apps/br209399)），填充将应用到该边框内部。
-   [**Border.Padding**](https://msdn.microsoft.com/library/windows/apps/br209263)：定义由 [**BorderThickness**](https://msdn.microsoft.com/library/windows/apps/br209256)/[**BorderBrush**](https://msdn.microsoft.com/library/windows/apps/br209254) 创建的矩形线和 [**Child**](https://msdn.microsoft.com/library/windows/apps/br209258) 元素之间的空间。
-   [**ItemsPresenter.Padding**](https://msdn.microsoft.com/library/windows/apps/hh968021)：帮助显示为项目控件中的项目生成的视觉对象，在每个项目周围放置指定的填充。
-   [**TextBlock.Padding**](https://msdn.microsoft.com/library/windows/apps/br209673) 和 [**RichTextBlock.Padding**](https://msdn.microsoft.com/library/windows/apps/br227596)：扩展文本元素的文本周围的边界框。 这些文本元素不含有 **Background**，所以对比由文本元素容器应用的其他布局行为，视觉上可能难以看到文本的填充。 由于该原因，文本元素填充很少使用，在所包含的 [**Block**](https://msdn.microsoft.com/library/windows/apps/jj191725) 容器上使用 [**Margin**](https://msdn.microsoft.com/library/windows/apps/br244379) 设置（用于使用 [**RichTextBlock**](https://msdn.microsoft.com/library/windows/apps/br227565) 的情况）更常见。

在其中每一种情况下，相同的元素还具有 **Margin** 属性。 如果同时应用边距和填充，它们可累加，因为外部容器和任何内部内容之间的明显距离将是边距加填充。 如果有不同的背景值应用到内容、元素或容器，边距结束的点和填充开始的点可能在呈现中可见。

## <a name="dimensions-height-width"></a>尺寸（高度、宽度）

[**FrameworkElement**](https://msdn.microsoft.com/library/windows/apps/br208718) 的 [**Height**](https://msdn.microsoft.com/library/windows/apps/br208751) 和 [**Width**](https://msdn.microsoft.com/library/windows/apps/br208706) 属性经常影响对齐、边距和填充属性在布局传递发生时的行为方式。 尤其是，实数 **Height** 和 **Width** 值取消 **Stretch** 对齐，而且还提升为 [**DesiredSize**](https://msdn.microsoft.com/library/windows/apps/br208921) 值的可能组件，此值在布局的度量传递期间创建。 **Height** 和 **Width** 具有约束属性：**Height** 值可以约束在 [**MinHeight**](https://msdn.microsoft.com/library/windows/apps/br208731) 和 [**MaxHeight**](https://msdn.microsoft.com/library/windows/apps/br208726) 范围内，**Width** 可以约束在 [**MinWidth**](https://msdn.microsoft.com/library/windows/apps/br208733) 和 [**MaxWidth**](https://msdn.microsoft.com/library/windows/apps/br208728) 范围内。 而且，[**ActualWidth**](https://msdn.microsoft.com/library/windows/apps/br208709) 和 [**ActualHeight**](https://msdn.microsoft.com/library/windows/apps/br208707) 是经计算的只读属性，这些属性在布局传递完成后仅包含有效值。 有关维度和约束或经计算的属性如何关联的详细信息，请参阅 [**FrameworkElement.Height**](https://msdn.microsoft.com/library/windows/apps/br208718) 和 [**FrameworkElement.Width**](https://msdn.microsoft.com/library/windows/apps/br208751) 中的“备注”。

## <a name="related-topics"></a>相关主题

**参考**

* [**FrameworkElement.Height**](https://msdn.microsoft.com/library/windows/apps/br208718)
* [**FrameworkElement.Width**](https://msdn.microsoft.com/library/windows/apps/br208751)
* [**FrameworkElement.HorizontalAlignment**](https://msdn.microsoft.com/library/windows/apps/br208720)
* [**FrameworkElement.VerticalAlignment**](https://msdn.microsoft.com/library/windows/apps/br208749)
* [**FrameworkElement.Margin**](https://msdn.microsoft.com/library/windows/apps/br208724)
* [**Control.Padding**](https://msdn.microsoft.com/library/windows/apps/br209459)



<!--HONumber=Dec16_HO2-->


