---
author: muhsinking
Description: Learn to write code for a custom Panel class, implementing ArrangeOverride and MeasureOverride methods, and using the Children property.
MS-HAID: dev\_ctrl\_layout\_txt.boxpanel\_example\_custom\_panel
MSHAttr: PreferredLib:/library/windows/apps
Search.Product: eADQiWindows 10XVcnh
title: BoxPanel，一个自定义面板示例（Windows 应用）
ms.assetid: 981999DB-81B1-4B9C-A786-3025B62B74D6
label: BoxPanel, an example custom panel
template: detail.hbs
op-migration-status: ready
ms.author: mukin
ms.date: 05/19/2017
ms.topic: article
keywords: Windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 7d29b85c7ec3ec9ec0114a3a49dff834f859511e
ms.sourcegitcommit: 753e0a7160a88830d9908b446ef0907cc71c64e7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/29/2018
ms.locfileid: "5756529"
---
# <a name="boxpanel-an-example-custom-panel"></a>BoxPanel，一个自定义面板示例

 

学习为自定义 [**Panel**](https://msdn.microsoft.com/library/windows/apps/br227511) 类编写代码、实现 [**ArrangeOverride**](https://msdn.microsoft.com/library/windows/apps/br208711) 和 [**MeasureOverride**](https://msdn.microsoft.com/library/windows/apps/br208730) 方法，以及使用 [**Children**](https://msdn.microsoft.com/library/windows/apps/br227514) 属性。 

> **重要 API**：[**面板**](https://msdn.microsoft.com/library/windows/apps/br227511)、[**ArrangeOverride**](https://msdn.microsoft.com/library/windows/apps/br208711)、[**MeasureOverride**](https://msdn.microsoft.com/library/windows/apps/br208730) 

示例代码显示了一个自定义面板实现，但我们不会花费许多时间来解释影响为不同的布局方案自定义面板方式的布局概念。 如果你需要关于这些布局概念以及它们可能如何应用到特定布局方案的详细信息，请参阅 [XAML 自定义面板概述](custom-panels-overview.md)。

*panel* 是当 XAML 布局系统运行且呈现应用 UI 时为所包含的子元素提供布局行为的对象。 你可以通过从 [**Panel**](https://msdn.microsoft.com/library/windows/apps/br227511) 类派生自定义类来为 XAML 布局定义自定义面板。 通过替代 [**ArrangeOverride**](https://msdn.microsoft.com/library/windows/apps/br208711) 和 [**MeasureOverride**](https://msdn.microsoft.com/library/windows/apps/br208730) 方法、提供度量和排列子元素的逻辑来为面板提供行为。 此示例派生自 **Panel**。 当你从 **Panel** 开始时，**ArrangeOverride** 和 **MeasureOverride** 方法没有开始行为。 你的代码可提供网关，子元素通过此网关变为对 XAML 布局系统已知并在 UI 中获得呈现。 因此，你的代码考虑到所有子元素并遵循布局系统预期的模式十分重要。

## <a name="your-layout-scenario"></a>你的布局方案

当你定义自定义面板时，你在定义布局方案。

布局方案通过以下内容表达：

-   当面板拥有子元素时将执行何种操作
-   面板何时在其自己的空间中有约束
-   面板的逻辑如何确定最终呈现子元素的 UI 布局的所有度量、放置、位置和大小

基于这一点，此处显示的 `BoxPanel` 适用于特定方案。 为了保持代码在此示例中的重要性，我们将不详细解释方案，而是专注于所需的步骤和编码模式。 如果你希望首先了解有关方案的详细信息，则跳到[“适用于 `BoxPanel` 的方案”](#the-scenario-for-boxpanel)，然后返回到代码。

## <a name="start-by-deriving-from-panel"></a>由从 **Panel** 派生开始

由从 [**Panel**](https://msdn.microsoft.com/library/windows/apps/br227511) 派生自定义类开始。 执行此操作的最简单方法可能是为此类定义一个独立的代码文件，方法是从 Microsoft Visual Studio 中的**解决方案资源管理器**中针对项目使用**添加** | **新建项** | **类**上下文菜单选项。 将该类（以及文件）命名为 `BoxPanel`。

类的模板文件不会从许多 **using** 语句开始，因为它不特别适用于通用 Windows 平台 (UWP) 应用。 因此，首先添加 **using** 语句。 模板文件的开头部分还包含一些你可能不需要的 **using** 语句，可以将其删除。 以下是可解析类型的 **using** 语句的建议列表，你将需要将这些语句用于典型的自定义面板代码：

```CSharp
using System;
using System.Collections.Generic; // if you need to cast IEnumerable for iteration, or define your own collection properties
using Windows.Foundation; // Point, Size, and Rect
using Windows.UI.Xaml; // DependencyObject, UIElement, and FrameworkElement
using Windows.UI.Xaml.Controls; // Panel
using Windows.UI.Xaml.Media; // if you need Brushes or other utilities
```

既然你可以解析 [**Panel**](https://msdn.microsoft.com/library/windows/apps/br227511)，使其成为 `BoxPanel` 的基类。 还可以使 `BoxPanel` 成为公共对象：

```CSharp
public class BoxPanel : Panel
{
}
```

在类级别上，定义一些将由几个逻辑函数共享的 **int** 和 **double** 值，但是这些值无需显示为公共 API。 在该示例中，它们命名为：`maxrc`、`rowcount`、`colcount`、`cellwidth`、`cellheight`、`maxcellheight`、`aspectratio`。

在你完成此操作后，完整代码文件外观如下（既然你已知道我们为何拥有它们，可以删除 **using** 上的注释了）：

```CSharp
using System;
using System.Collections.Generic;
using Windows.Foundation;
using Windows.UI.Xaml;
using Windows.UI.Xaml.Controls;
using Windows.UI.Xaml.Media;

public class BoxPanel : Panel 
{
    int maxrc, rowcount, colcount;
    double cellwidth, cellheight, maxcellheight, aspectratio;
}
```

从现在起，我们将一次向你显示一个成员定义，无论它是方法替代还是支持内容（例如依赖属性）。 你可以将它们以任何顺序添加到上述框架，而且我们将不再在代码段中显示 **using** 语句或类范围的定义，直到我们显示最终代码。

## **<a name="measureoverride"></a>MeasureOverride**


```CSharp
protected override Size MeasureOverride(Size availableSize)
{
    Size returnSize;
    // Determine the square that can contain this number of items.
    maxrc = (int)Math.Ceiling(Math.Sqrt(Children.Count));
    // Get an aspect ratio from availableSize, decides whether to trim row or column.
    aspectratio = availableSize.Width / availableSize.Height;

    // Now trim this square down to a rect, many times an entire row or column can be omitted.
    if (aspectratio > 1)
    {
        rowcount = maxrc;
        colcount = (maxrc > 2 && Children.Count < maxrc * (maxrc - 1)) ? maxrc - 1 : maxrc;
    } 
    else 
    {
        rowcount = (maxrc > 2 && Children.Count < maxrc * (maxrc - 1)) ? maxrc - 1 : maxrc;
        colcount = maxrc;
    }

    // Now that we have a column count, divide available horizontal, that's our cell width.
    cellwidth = (int)Math.Floor(availableSize.Width / colcount);
    // Next get a cell height, same logic of dividing available vertical by rowcount.
    cellheight = Double.IsInfinity(availableSize.Height) ? Double.PositiveInfinity : availableSize.Height / rowcount;
           
    foreach (UIElement child in Children)
    {
        child.Measure(new Size(cellwidth, cellheight));
        maxcellheight = (child.DesiredSize.Height > maxcellheight) ? child.DesiredSize.Height : maxcellheight;
    }
    return LimitUnboundedSize(availableSize);
}
```

[**MeasureOverride**](https://msdn.microsoft.com/library/windows/apps/br208730) 实现的必要模式是循环访问 [**Panel.Children**](https://msdn.microsoft.com/library/windows/apps/br227514) 中的每个元素。 始终对这些元素中的每一个调用 [**Measure**](https://msdn.microsoft.com/library/windows/apps/br208952) 方法。 **Measure** 具有类型 [**Size**](https://msdn.microsoft.com/library/windows/apps/br225995) 的参数。 你在此处传递的内容是，面板致力于获取的适用于该特定子元素的大小。 因此，在你可以进行循环访问并开始调用 **Measure** 之前，你需要知道每个单元格可以提供多少空间。 从 **MeasureOverride** 方法本身，你具有 *availableSize* 值。 它是面板的父元素在调用 **Measure** 时使用的大小，它是最初调用此 **MeasureOverride** 的触发器。 因此典型的逻辑是制定一个方案，每个子元素通过此方案划分面板的整体 *availableSize* 的空间。 然后你将大小的每个划分传递到每个子元素的 **Measure**。

`BoxPanel` 划分大小的方式相当简单：它将它的空间划分为一些框，这些框很大程度上受项目数量的控制。 基于行列计数和可用大小调整框的大小。 有时因为不需要正方形中的一行或一列而将其删除，因此就行列比而言，面板变成了矩形而不是正方形。 有关如何到达此逻辑的详细信息，请跳到[“面向 BoxPanel 的方案”](#the-scenario-for-boxpanel)。

那么度量传递的作用是什么？ 它为在其中调用 [**Measure**](https://msdn.microsoft.com/library/windows/apps/br208921) 的每个元素的只读 [**DesiredSize**](https://msdn.microsoft.com/library/windows/apps/br208952) 属性设置值。 一旦你进行排列传递，拥有 **DesiredSize** 值可能很重要，因为 **DesiredSize** 传达排列和最终呈现时可以设置和应当设置的大小。 即使你不在你自己的逻辑中使用 **DesiredSize**，系统仍然需要它。

当 *availableSize* 的高度组件未绑定时，可能使用此面板。 如果确实如此，则面板没有要划分的已知高度。 在此情况下，度量传递的逻辑会通知每个子元素它尚未具有绑定的高度。 它通过为 [**Size.Height**](https://msdn.microsoft.com/library/windows/apps/br225995) 为无限的子元素将 [**Size**](https://msdn.microsoft.com/library/windows/apps/br208952) 传递到 [**Measure**](https://msdn.microsoft.com/library/windows/apps/hh763910) 调用来执行此操作。 这是合法的。 当调用 **Measure** 时，逻辑为将 [**DesiredSize**](https://msdn.microsoft.com/library/windows/apps/br208921) 设置为以下值中的最小值：传递到 **Measure** 的内容，或来自系数（例如明确设置的 [**Height**](/uwp/api/Windows.UI.Xaml.FrameworkElement.Height) 和 [**Width**](/uwp/api/Windows.UI.Xaml.FrameworkElement.Width)）的该元素的自然大小。

> [!NOTE]
> [**StackPanel**](https://msdn.microsoft.com/library/windows/apps/br209635) 的内部逻辑也具有此行为：**StackPanel** 将一个无线维度值传递到子元素上的 [**Measure**](https://msdn.microsoft.com/library/windows/apps/br208952)，表示在方向维度中的子元素上没有约束。 **StackPanel** 通常动态调整自身大小，以使在该维度中增长的堆栈容纳所有子元素。

但是，面板本身无法从 [**MeasureOverride**](https://msdn.microsoft.com/library/windows/apps/br225995) 返回带有无限值的 [**Size**](https://msdn.microsoft.com/library/windows/apps/br208730)；这将在布局期间引发异常。 因此部分逻辑要找出任何子元素请求的最大高度，并将高度用作单元格高度，以防尚未从面板的自身大小约束中获得该高度的情况。 以下是在之前的代码中引用的帮助程序函数 `LimitUnboundedSize`，此函数随后获取该最大单元格高度，并使用它向面板提供一个用于返回的有限高度，以及确保在启动排列传递之前 `cellheight` 为有限值：

```CSharp
// This method is called only if one of the availableSize dimensions of measure is infinite.
// That can happen to height if the panel is close to the root of main app window.
// In this case, base the height of a cell on the max height from desired size
// and base the height of the panel on that number times the #rows.

Size LimitUnboundedSize(Size input)
{
    if (Double.IsInfinity(input.Height))
    {
        input.Height = maxcellheight * colcount;
        cellheight = maxcellheight;
    }
    return input;
}
```

## **<a name="arrangeoverride"></a>ArrangeOverride**

```CSharp
protected override Size ArrangeOverride(Size finalSize)
{
     int count = 1
     double x, y;
     foreach (UIElement child in Children)
     {
          x = (count - 1) % colcount * cellwidth;
          y = ((int)(count - 1) / colcount) * cellheight;
          Point anchorPoint = new Point(x, y);
          child.Arrange(new Rect(anchorPoint, child.DesiredSize));
          count++;
     }
     return finalSize;
}
```

[**ArrangeOverride**](https://msdn.microsoft.com/library/windows/apps/br208711) 实现的必要模式是循环访问 [**Panel.Children**](https://msdn.microsoft.com/library/windows/apps/br227514) 中的每个元素。 始终对这些元素中的每一个调用 [**Arrange**](https://msdn.microsoft.com/library/windows/apps/br208914) 方法。

请注意，计算不如 [**MeasureOverride**](https://msdn.microsoft.com/library/windows/apps/br208730) 中多；这很正常。 子元素的大小已经从面板的自身 **MeasureOverride** 逻辑获知，或从度量传递期间每个子元素集合的 [**DesiredSize**](https://msdn.microsoft.com/library/windows/apps/br208921) 值获知。 但是，我们仍然需要确定每个子元素在面板中出现的位置。 在典型的面板中，每个子元素应当在不同的位置呈现。 典型方案不需要创建重叠元素的面板（然而如果那是你真正所需的方案，创建带有有意重叠的面板也并非不可能）。

此面板按行和列的概念排列。 行和列的数量已经过计算（对于度量是必需的）。 因此，现在行和列的形状以及每个单元格的已知大小有助于定义此面板包含的每个元素的呈现位置 (`anchorPoint`) 的逻辑。 从度量已知的 [**Point**](https://msdn.microsoft.com/library/windows/apps/br225870) 以及 [**Size**](https://msdn.microsoft.com/library/windows/apps/br225995) 用作构造 [**Rect**](https://msdn.microsoft.com/library/windows/apps/br225994) 的两个组件。 **Rect** 是 [**Arrange**](https://msdn.microsoft.com/library/windows/apps/br208914) 的输入类型。

面板有时需要剪裁它们的内容。 如果它们执行此操作，剪裁的大小是 [**DesiredSize**](https://msdn.microsoft.com/library/windows/apps/br208921) 中显示的大小，因为 [**Measure**](https://msdn.microsoft.com/library/windows/apps/br208952) 逻辑将其设置为传递到 **Measure** 的内容的最小值，或其他自然大小系数。 因此你通常不需要在 [**Arrange**](https://msdn.microsoft.com/library/windows/apps/br208914) 期间专门检查剪裁，剪裁的发生仅基于将 **DesiredSize** 传递到每个 **Arrange** 调用。

如果定义呈现位置所需的所有信息通过其他方式已知，则你在经过循环访问时不需要始终计数。 例如，在 [**Canvas**](https://msdn.microsoft.com/library/windows/apps/br209267) 布局逻辑中，[**Children**](https://msdn.microsoft.com/library/windows/apps/br227514) 集合中的位置不重要。 在 **Canvas** 中定位每个元素所需的所有信息通过读取子元素的 [**Canvas.Left**](https://msdn.microsoft.com/library/windows/apps/hh759771) 和 [**Canvas.Top**](https://msdn.microsoft.com/library/windows/apps/hh759772) 值作为排列逻辑的一部分已知。 `BoxPanel` 逻辑恰好需要计数以对 *colcount* 进行比较，因此已知何时开始一个新行并抵消 *y* 值。

通常输入 *finalSize* 和你从 [**ArrangeOverride**](https://msdn.microsoft.com/library/windows/apps/br225995) 实现返回的 [**Size**](https://msdn.microsoft.com/library/windows/apps/br208711) 相同。 有关原因的详细信息，请参阅 [XAML 自定义面板概述](custom-panels-overview.md)的“**ArrangeOverride**”部分。

## <a name="a-refinement-controlling-the-row-vs-column-count"></a>优化：控制行与列计数

你可以按照当前现状编译并使用此面板。 但是，我们将再添加一个优化。 在刚才显示的代码中，逻辑将附加行或列放置到在纵横比中最长的边上。 但为了更好地控制单元格的形状，最好选择 4x3 单元格设置，而不是 3x4，即使面板的自身纵横比是“纵向”。 因此我们将添加一个可选的依赖属性，面板使用者可设置此属性以控制该行为。 以下是依赖属性定义，此定义非常基本：

```CSharp
public static readonly DependencyProperty UseOppositeRCRatioProperty =
   DependencyProperty.Register("UseOppositeRCRatio", typeof(bool), typeof(BoxPanel), null);

public bool UseSquareCells
{
    get { return (bool)GetValue(UseOppositeRCRatioProperty); }
    set { SetValue(UseOppositeRCRatioProperty, value); }
}
```

以下为使用 `UseOppositeRCRatio` 对度量逻辑的影响。 它所执行的所有操作实际上是更改 `rowcount` 和 `colcount` 从 `maxrc` 和真纵横比派生的方式，因此每个单元格都有相应的大小差异。 当 `UseOppositeRCRatio` 为 **true** 时，它在将真纵横比的值用于行和列计数前反转它。

```CSharp
if (UseOppositeRCRatio) { aspectratio = 1 / aspectratio;}
```

## <a name="the-scenario-for-boxpanel"></a>面向 BoxPanel 的方案

面向 `BoxPanel` 的特定方案具有这样的面板：面板中的一个如何划分空间的主要决定因素是，通过知道子项目的数量并为面板划分已知可用空间。 面板固有为矩形。 许多面板通过将该矩形空间划分为更多的矩形来操作；这是 [**Grid**](https://msdn.microsoft.com/library/windows/apps/br242704) 对其单元格执行的操作。 对于 **Grid**，单元格的大小由 [**ColumnDefinition**](https://msdn.microsoft.com/library/windows/apps/br209324) 和 [**RowDefinition**](https://msdn.microsoft.com/library/windows/apps/br227606) 值设置，而且元素通过 [**Grid.Row**](https://msdn.microsoft.com/library/windows/apps/hh759795) 和 [**Grid.Column**](https://msdn.microsoft.com/library/windows/apps/hh759774) 附加属性声明它们进入的确切单元格。 从 **Grid** 获取良好的布局通常需要事先知道子元素的数量，以便有足够单元格且每个子元素设置其附加属性以使其适合自己的单元格。

但是如果子元素的数量是动态的该怎么办？ 这当然可能。你的应用代码可将项目添加到集合中，以响应任何你认为足够重要且值得更新 UI 的动态运行时状况。 如果你对备份集合/业务对象使用数据绑定，获取此类更新和更新 UI 将自动处理，因此这通常是首选技术（请参阅[数据绑定概述](https://msdn.microsoft.com/library/windows/apps/mt210946)）。

但是并非所有应用方案都适合数据绑定。 有时你在运行时需要创建新 UI 元素并使它们可见。 `BoxPanel` 用于此方案。 变化的子项目数对 `BoxPanel` 不构成问题，因为它在计算中使用子元素计数，并且将现有和新子元素调整为新布局，以使它们全部适合。

用于进一步扩展 `BoxPanel` 的高级方案（此处未显示）可同时容纳动态子元素并使用子元素的 [**DesiredSize**](https://msdn.microsoft.com/library/windows/apps/br208921) 作为调整个别单元格大小的更强系数。 此方案可能使用不同的行或列大小或者非网格形状来减少“浪费的”空间。 这需要一种战略，用于确定大小和纵横比各异的多个矩形如何同时出于视觉美学和最小大小而全部适合包含的矩形。 `BoxPanel` 不执行此操作，而是使用更简单的技术用于划分空间。 `BoxPanel`的技术用于确定大于子元素计数的最小正方形数。 例如，9 个项目可容纳到 3x3 正方形中。 10 个项目需要 4x4 正方形。 不过，你可以经常调整项目，同时仍然可以删除开始正方形的一行或一列，以便节省空间。 在 count=10 示例中，它可容纳到 4x3 或 3x4 矩形中。

你可能想知道为什么该面板不为 10 个项目选择 5x2，因为那将完美地容纳该项目数。 但是，在实际中，会将面板大小调整为很少有方向明显的纵横比的矩形。 最小二乘技术是一种影响大小逻辑的方式，以便良好适合典型布局形状，并且不鼓励单元格形状可获取奇数纵横比的大小调整。

## <a name="related-topics"></a>相关主题

**参考**

* [**FrameworkElement.ArrangeOverride**](https://msdn.microsoft.com/library/windows/apps/br208711)
* [**FrameworkElement.MeasureOverride**](https://msdn.microsoft.com/library/windows/apps/br208730)
* [**面板**](https://msdn.microsoft.com/library/windows/apps/br227511)

**概念**

* [对齐、边距和填充](alignment-margin-padding.md)
