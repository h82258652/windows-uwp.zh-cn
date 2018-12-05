---
Description: You can define custom panels for XAML layout by deriving a custom class from the Panel class.
MS-HAID: dev\_ctrl\_layout\_txt.xaml\_custom\_panels\_overview
MSHAttr: PreferredLib:/library/windows/apps
Search.Product: eADQiWindows 10XVcnh
title: XAML 自定义面板概述
ms.assetid: 0CD395CD-E2AB-429D-BB49-56A71C5CC35D
label: XAML custom panels overview (Windows apps)
template: detail.hbs
op-migration-status: ready
ms.date: 05/19/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 9999ebb121916a7804546784ea98ac4e0f4222e5
ms.sourcegitcommit: c01c29cd97f1cbf050950526e18e15823b6a12a0
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/05/2018
ms.locfileid: "8695051"
---
# <a name="xaml-custom-panels-overview"></a>XAML 自定义面板概述

 

*面板*是在 Extensible Application Markup Language (XAML) 布局系统运行且呈现应用 UI 时为所包含的子元素提供布局行为的对象。 


> **重要 API**：[**面板**](https://msdn.microsoft.com/library/windows/apps/br227511)、[**ArrangeOverride**](https://msdn.microsoft.com/library/windows/apps/br208711)、[**MeasureOverride**](https://msdn.microsoft.com/library/windows/apps/br208730)

你可以通过从 [**Panel**](https://msdn.microsoft.com/library/windows/apps/br227511) 类派生自定义类来为 XAML 布局定义自定义面板。 通过替代 [**MeasureOverride**](https://msdn.microsoft.com/library/windows/apps/br208730) 和 [**ArrangeOverride**](https://msdn.microsoft.com/library/windows/apps/br208711)、提供度量和排列子元素的逻辑来为面板提供行为。

## <a name="the-panel-base-class"></a>**Panel** 基类


要定义自定义面板类，你可以直接从 [**Panel**](https://msdn.microsoft.com/library/windows/apps/br227511) 类派生，或从其中一个未封装的实际面板类派生，例如 [**Grid**](https://msdn.microsoft.com/library/windows/apps/br242704) 或 [**StackPanel**](https://msdn.microsoft.com/library/windows/apps/br209635)。 从 **Panel** 派生更简单，因为替代已有布局行为的面板的现有布局逻辑可能会很困难。 而且，带有行为的面板可能有与面板的布局功能不相关的现有属性。

从 [**Panel**](https://msdn.microsoft.com/library/windows/apps/br227511)，自定义面板可继承这些 API：

-   [**Children**](https://msdn.microsoft.com/library/windows/apps/br227514) 属性。
-   [**Background**](https://msdn.microsoft.com/library/windows/apps/br227512)、[**ChildrenTransitions**](https://msdn.microsoft.com/library/windows/apps/br227515) 和 [**IsItemsHost**](https://msdn.microsoft.com/library/windows/apps/br227517) 属性，以及依赖属性标识符。 这些属性都不是虚拟的，所以你通常无需替代或替换它们。 你通常不需要将这些属性用于自定义面板方案，甚至不用于读取值。
-   布局覆盖方法 [**MeasureOverride**](https://msdn.microsoft.com/library/windows/apps/br208730) 和 [**ArrangeOverride**](https://msdn.microsoft.com/library/windows/apps/br208711)。 它们最初由 [**FrameworkElement**](https://msdn.microsoft.com/library/windows/apps/br208706) 定义。 基本 [**Panel**](https://msdn.microsoft.com/library/windows/apps/br227511) 类不替代它们，但是实际面板（例如 [**Grid**](https://msdn.microsoft.com/library/windows/apps/br242704)）确实有替代实现，此实现作为本机代码实现且由系统运行。 为 **ArrangeOverride** 和 **MeasureOverride** 提供新的（或附加的）实现占定义自定义面板所需工作的大部分。
-   [**FrameworkElement**](https://msdn.microsoft.com/library/windows/apps/br208706)、[**UIElement**](https://msdn.microsoft.com/library/windows/apps/br208911) 和 [**DependencyObject**](https://msdn.microsoft.com/library/windows/apps/br242356) 的所有其他 API，例如 [**Height**](/uwp/api/Windows.UI.Xaml.FrameworkElement.Height)、[**Visibility**](https://msdn.microsoft.com/library/windows/apps/br208992) 等。 你有时在布局替代中参考这些属性的值，但是它们不是虚拟的，所以你通常不会替代或替换它们。

此处的重点是介绍 XAML 布局概念，以便你考虑自定义面板能够以及应该在布局中如何表现的所有可能性。 如果你希望直接查看自定义面板实现的示例，请参阅 [BoxPanel：一个自定义面板示例](boxpanel-example-custom-panel.md)。

## <a name="the-children-property"></a>**Children** 属性


[**Children**](https://msdn.microsoft.com/library/windows/apps/br227514) 属性与自定义面板相关，因为从 [**Panel**](https://msdn.microsoft.com/library/windows/apps/br227511) 派生的所有类均使用 **Children** 属性作为在集合中存储它们所包含的子元素的位置。 **Children** 被指定为 **Panel** 类的 XAML 内容属性，并且从 **Panel** 派生的所有类均可继承 XAML 内容属性行为。 如果某个属性被指定为 XAML 内容属性，这意味着 XAML 标记可以在标记中指定该属性时省略某个属性元素，而且值设置为直接标记子元素（“内容”）。 例如，如果你从未定义新行为的 **Panel** 派生出名为 **CustomPanel** 的类，则你仍然可以使用此标记：

```XAML
<local:CustomPanel>
  <Button Name="button1"/>
  <Button Name="button2"/>
</local:CustomPanel>
```

当 XAML 分析程序读取此标记时，[**Children**](https://msdn.microsoft.com/library/windows/apps/br227514) 被认为所有 [**Panel**](https://msdn.microsoft.com/library/windows/apps/br227511) 派生的类型的 XAML 内容属性，因此分析程序会将两个 [**Button**](https://msdn.microsoft.com/library/windows/apps/br209265) 元素添加到 **Children** 属性的 [**UIElementCollection**](https://msdn.microsoft.com/library/windows/apps/br227633) 值中。 XAML 内容属性有助于简化用于 UI 定义的 XAML 标记中的父子关系。 有关 XAML 内容属性的详细信息以及如何在解析 XAML 时填充集合属性，请参阅 [XAML 语法指南](https://msdn.microsoft.com/library/windows/apps/mt185596)。

保持 [**Children**](https://msdn.microsoft.com/library/windows/apps/br227514) 属性的值的集合类型是 [**UIElementCollection**](https://msdn.microsoft.com/library/windows/apps/br227633) 类。 **UIElementCollection** 是强类型集合，此集合使用 [**UIElement**](https://msdn.microsoft.com/library/windows/apps/br208911) 作为它的强制项类型。 **UIElement** 是基本类型，此类型由数百个实际 UI 元素类型继承，因此此处的类型强制故意较为宽松。 但是它强制你无法拥有作为 [**Panel**](/uwp/api/Windows.UI.Xaml.Media.Brush) 的直接子元素的 [**Brush**](https://msdn.microsoft.com/library/windows/apps/br227511)，并且通常表示仅预期在 UI 中可见且参与布局的元素将被视为 **Panel** 中的子元素。

通常，自定义面板只需按原样使用 [**Children**](https://msdn.microsoft.com/library/windows/apps/br208911) 属性的特征，便可接受由 XAML 定义的任何 [**UIElement**](https://msdn.microsoft.com/library/windows/apps/br227514) 子元素。 作为高级方案，当你在布局替代中的集合上迭代时，可以支持子元素的进一步类型检查。

除了在替代中的 [**Children**](https://msdn.microsoft.com/library/windows/apps/br227514) 集合中循环，面板逻辑还可能受到 `Children.Count` 的影响。 你可能有这样的逻辑：至少部分基于项目数而非所需大小和其他独立项的特征来分配空间。

## <a name="overriding-the-layout-methods"></a>替代布局方法


布局替代方法（[**MeasureOverride**](https://msdn.microsoft.com/library/windows/apps/br208730) 和 [**ArrangeOverride**](https://msdn.microsoft.com/library/windows/apps/br208711)）的基本模型是它们应当迭代所有子元素并调用每个子元素的特定布局方法。 当 XAML 布局系统为根窗口设置视觉对象时，第一个布局周期开始。 由于每个父元素对其子元素调用布局，这会将对布局方法的调用传播到每个应当是布局的一部分的可能的 UI 元素。 在 XAML 布局中，有两个阶段：度量，然后是排列。

无法从基本 [**Panel**](https://msdn.microsoft.com/library/windows/apps/br208730) 类获取任何 [**MeasureOverride**](https://msdn.microsoft.com/library/windows/apps/br208711) 和 [**ArrangeOverride**](https://msdn.microsoft.com/library/windows/apps/br227511) 的内置布局方法行为。 [**Children**](https://msdn.microsoft.com/library/windows/apps/br227514) 中的项目不会自动作为 XAML 可视化树的一部分呈现。 由你来使项目对布局过程已知，方法是通过在 **MeasureOverride** 和 **ArrangeOverride** 实现内传递布局，从而针对你在 **Children** 中找到的每个项目调用布局方法。

没有理由调用布局替代中的基本实现，除非你有自己的继承。 布局行为的本机方法（如果存在）仍在运行，由于不会从替代调用基本实现，因而不会阻止本机行为的发生。

在度量传递期间，布局逻辑查询每个子元素的所需大小，方法是对该子元素调用 [**Measure**](https://msdn.microsoft.com/library/windows/apps/br208952) 方法。 调用 **Measure** 方法可为 [**DesiredSize**](https://msdn.microsoft.com/library/windows/apps/br208921) 属性创建值。 [**MeasureOverride**](https://msdn.microsoft.com/library/windows/apps/br208730) 返回值是面板本身的所需大小。

在排列期间，子元素的位置和大小由 x-y 空间确定，准备布局构成用于呈现。 你的代码必须对 [**Children**](https://msdn.microsoft.com/library/windows/apps/br208914) 中的每个子元素调用 [**Arrange**](https://msdn.microsoft.com/library/windows/apps/br227514)，以便使布局系统检测到元素属于该布局。 **Arrange** 调用是构成和呈现的初期形式；它通知布局系统元素的去向、构成提交以用于呈现的时间。

许多属性和值影响布局逻辑在运行时的工作方式。 一个思考布局过程的方法是，不带有子元素的元素（通常是 UI 中嵌套最深的元素）是可以首先完成度量的元素。 它们对影响所需大小的子元素没有任何依赖性。 它们可能有自己的所需大小，而且在布局实际发生之前，这些都是大小建议。 那时，度量传递继续浏览可视化树，直到根元素有自己的度量，并且所有度量都可完成。

候选布局必须适合当前应用窗口，否则将剪裁 UI 的其他部分。 面板通常是确定剪裁逻辑的位置。 面板逻辑可以确定何种大小从 [**MeasureOverride**](https://msdn.microsoft.com/library/windows/apps/br208730) 实现内可用，而且可能必须向子元素施加大小限制并在子元素间划分空间以便达到最佳配置。 在理想情况下，布局的结果使用布局的所有部分的各个属性，但仍然能够适合应用窗口。 这需要良好实现面板的布局逻辑，以及对任何使用该面板构建 UI 的应用代码部分执行正确的 UI 设计。 如果总体 UI 设计包含多于应用可容纳的子元素，则面板设计不可能美观。

使布局系统工作的重要因素是，任何基于 [**FrameworkElement**](https://msdn.microsoft.com/library/windows/apps/br208706) 的元素在作为容器中的子元素操作时，已拥有一些其自身的固有行为。 例如，一些 **FrameworkElement** 的 API 可通知布局行为，或为布局工作所必需。 其中包括：

-   [**DesiredSize**](https://msdn.microsoft.com/library/windows/apps/br208921) （实际为 [**UIElement**](https://msdn.microsoft.com/library/windows/apps/br208911) 属性）
-   [**ActualHeight**](https://msdn.microsoft.com/library/windows/apps/br208707) 和 [**ActualWidth**](https://msdn.microsoft.com/library/windows/apps/br208709)
-   [**Height**](/uwp/api/Windows.UI.Xaml.FrameworkElement.Height) 和 [**Width**](/uwp/api/Windows.UI.Xaml.FrameworkElement.Width)
-   [**边距**](https://msdn.microsoft.com/library/windows/apps/br208724)
-   [**LayoutUpdated**](https://msdn.microsoft.com/library/windows/apps/br208722) 事件
-   [**HorizontalAlignment**](https://msdn.microsoft.com/library/windows/apps/br208720) 和 [**VerticalAlignment**](https://msdn.microsoft.com/library/windows/apps/br208749)
-   [**ArrangeOverride**](https://msdn.microsoft.com/library/windows/apps/br208711) 和 [**MeasureOverride**](https://msdn.microsoft.com/library/windows/apps/br208730) 方法
-   [**Arrange**](https://msdn.microsoft.com/library/windows/apps/br208914) 和 [**Measure**](https://msdn.microsoft.com/library/windows/apps/br208952) 方法：它们有在 [**FrameworkElement**](https://msdn.microsoft.com/library/windows/apps/br208706) 级别定义的本机实现，可处理元素级别的布局操作

## **<a name="measureoverride"></a>MeasureOverride**


[**MeasureOverride**](https://msdn.microsoft.com/library/windows/apps/br208730) 方法有返回值，当 [**Measure**](https://msdn.microsoft.com/library/windows/apps/br208921) 方法在面板上受到布局中的父元素调用时，布局系统将使用该值作为面板自身的起始 [**DesiredSize**](https://msdn.microsoft.com/library/windows/apps/br208952)。 方法内的逻辑选择与它返回的内容同等重要，而且逻辑经常影响返回的值。

所有 [**MeasureOverride**](https://msdn.microsoft.com/library/windows/apps/br208730) 实现应当循环访问 [**Children**](https://msdn.microsoft.com/library/windows/apps/br227514)，并且对每个子元素调用 [**Measure**](https://msdn.microsoft.com/library/windows/apps/br208952) 方法。 调用 **Measure** 方法可为 [**DesiredSize**](https://msdn.microsoft.com/library/windows/apps/br208921) 属性创建值。 这可能会通知面板本身需要多少空间，以及如何在元素间划分空间或为特定的子元素调整大小。

以下是 [**MeasureOverride**](https://msdn.microsoft.com/library/windows/apps/br208730) 方法非常基本的框架：

```CSharp
protected override Size MeasureOverride(Size availableSize)
{
    Size returnSize; //TODO might return availableSize, might do something else
     
    //loop through each Child, call Measure on each
    foreach (UIElement child in Children)
    {
        child.Measure(new Size()); // TODO determine how much space the panel allots for this child, that's what you pass to Measure
        Size childDesiredSize = child.DesiredSize; //TODO determine how the returned Size is influenced by each child's DesiredSize
        //TODO, logic if passed-in Size and net DesiredSize are different, does that matter?
    }
    return returnSize;
}
```

当元素为布局准备就绪时，它们通常拥有自然大小。 度量传递后，如果你为 [**Measure**](https://msdn.microsoft.com/library/windows/apps/br208921) 传递的 *availableSize* 较小，[**DesiredSize**](https://msdn.microsoft.com/library/windows/apps/br208952) 可能显示自然大小。 如果自然大小比你为 **Measure** 传递的 *availableSize* 更大，则 **DesiredSize** 被约束为 *availableSize*。 这是 **Measure** 的内部实现行为方式，而且你的布局替代应当将行为纳入考虑之内。

一些元素没有自然大小，因为它们的 [**Height**](/uwp/api/Windows.UI.Xaml.FrameworkElement.Height) 和 [**Width**](/uwp/api/Windows.UI.Xaml.FrameworkElement.Width) 的值为 **Auto**。 然后，这些元素将使用完整的 *availableSize*，因为这是 **Auto** 值显示的内容：将元素调整为最大可用大小，由直接布局父元素通过调用带有 *availableSize* 的 [**Measure**](https://msdn.microsoft.com/library/windows/apps/br208952) 来传达。 事实上，始终存在一些度量，可使你将 UI 大小调整到其值（即使它是顶级窗口）。最后，度量传递将所有 **Auto** 值解析为父约束，并且所有 **Auto** 值元素获取真正的度量（布局完成后，你可以通过查看 [**ActualWidth**](https://msdn.microsoft.com/library/windows/apps/br208709) 和 [**ActualHeight**](https://msdn.microsoft.com/library/windows/apps/br208707) 来获取度量）。

将一个大小传递到至少有一个无限维度的 [**Measure**](https://msdn.microsoft.com/library/windows/apps/br208952) 是合法操作，从而显示面板可以尝试调整自身的大小以适合其内容的度量。 每个被度量的子元素使用其自然大小设置它的 [**DesiredSize**](https://msdn.microsoft.com/library/windows/apps/br208921) 值。 然后，在排列传递期间，面板通常使用该大小排列。

即使未设置 [**Height**](https://msdn.microsoft.com/library/windows/apps/br209652) 或 [**Width**](https://msdn.microsoft.com/library/windows/apps/br208709) 值，文本元素（例如 [**TextBlock**](https://msdn.microsoft.com/library/windows/apps/br208707)）拥有基于它们的文本字符串的经计算的 [**ActualWidth**](/uwp/api/Windows.UI.Xaml.FrameworkElement.Height) 和 [**ActualHeight**](/uwp/api/Windows.UI.Xaml.FrameworkElement.Width)，而且面板逻辑应当遵循这些维度。 剪裁文本是非常差的 UI 体验。

即使你的实现不使用所需大小度量，最好对每个子元素调用 [**Measure**](https://msdn.microsoft.com/library/windows/apps/br208952) 方法，因为由 **Measure** 触发的内部和本机行为正在被调用。 要使元素参加布局，每个子元素必须在度量传递期间使 **Measure** 对其调用，并且在排列传递期间使 [**Arrange**](https://msdn.microsoft.com/library/windows/apps/br208914) 方法对其调用。 调用这些方法将在对象上设置内部标记并填充值（例如 [**DesiredSize**](https://msdn.microsoft.com/library/windows/apps/br208921) 属性），系统的布局逻辑在构建可视化树和呈现 UI 时需要这些值。

[**MeasureOverride**](https://msdn.microsoft.com/library/windows/apps/br208730) 返回值基于面板的逻辑，此逻辑为 [**Children**](https://msdn.microsoft.com/library/windows/apps/br208921) 中的每个子元素解释 [**DesiredSize**](https://msdn.microsoft.com/library/windows/apps/br227514) 或其他大小注意事项（当对子元素调用 [**Measure**](https://msdn.microsoft.com/library/windows/apps/br208952) 时）。 如何处理来自子元素的 **DesiredSize** 值以及 **MeasureOverride** 返回值应如何使用它们由你自己的逻辑解释来决定。 通常不要在未修改的情况下累加值，因为 **MeasureOverride** 的输入经常是固定的可用大小，此大小由面板的父元素建议。 如果你超过该大小，面板本身可能受到剪裁。 通常子元素的总大小与面板的可用大小进行比较，并在需要时进行调整。

### <a name="tips-and-guidance"></a>技巧与指南

-   理想情况下，自定义面板应当适合作为 UI 构成中的第一个真视觉对象，也许在紧接着 [**Page**](https://msdn.microsoft.com/library/windows/apps/br227503)、[**UserControl**](https://msdn.microsoft.com/library/windows/apps/br227647) 或其他 XAML 页面根元素之下的级别。 在 [**MeasureOverride**](https://msdn.microsoft.com/library/windows/apps/br208730) 实现中，不要在未检查值的情况下例行返回输入 [**Size**](https://msdn.microsoft.com/library/windows/apps/br225995)。 如果返回的 **Size** 在其中有 **Infinity** 值，这可能在运行时布局逻辑中引发异常。 **Infinity** 值可能来自主应用窗口，此窗口可滚动，因此没有最大高度。 其他可滚动内容可能有相同的行为。
-   [**MeasureOverride**](https://msdn.microsoft.com/library/windows/apps/br208730) 实现中的其他常见错误是，返回新的默认 [**Size**](https://msdn.microsoft.com/library/windows/apps/br225995)（高度和宽度的值为 0）。 你可以从该值开始，并且在面板确定不呈现任何子元素时，它甚至可能为正确的值。 但是，默认的 **Size** 将导致面板的主机错误调整面板大小。 它不请求 UI 中的空间，因此不获取空间且不呈现。 即使所有面板代码均可正常运行，但如果面板的高度和宽度均为零，你仍然无法看到面板或其内容。
-   在替代内，请避免尝试将子元素转换为 [**FrameworkElement**](https://msdn.microsoft.com/library/windows/apps/br208706) 并使用计算的属性作为布局结果，尤其是 [**ActualWidth**](https://msdn.microsoft.com/library/windows/apps/br208709) 和 [**ActualHeight**](https://msdn.microsoft.com/library/windows/apps/br208707)。 对于大部分常见方案，你可以将逻辑基于子元素的 [**DesiredSize**](https://msdn.microsoft.com/library/windows/apps/br208921) 值，而且无需子元素的任何 [**Height**](/uwp/api/Windows.UI.Xaml.FrameworkElement.Height) 或 [**Width**](/uwp/api/Windows.UI.Xaml.FrameworkElement.Width) 相关属性。 对于你知道元素类型且拥有其他信息（例如图像文件的自然大小）的特殊用例，你可以使用你的元素的专业信息，因为它不是布局系统主动修改的值。 将经布局计算的属性包括为布局逻辑的一部分将大幅增加定义意外布局循环的风险。 这些循环会导致无法创建有效布局的情况，而且如果循环不可恢复，系统可能引发 [**LayoutCycleException**](https://msdn.microsoft.com/library/windows/apps/hh673799)。
-   尽管具体划分空间的方式不同，但面板通常在多个子元素间划分它们的可用空间。 例如，[**Grid**](https://msdn.microsoft.com/library/windows/apps/br242704) 实现布局逻辑，此逻辑使用其 [**RowDefinition**](https://msdn.microsoft.com/library/windows/apps/br227606) 和 [**ColumnDefinition**](https://msdn.microsoft.com/library/windows/apps/br209324) 值将空间划分为 **Grid** 单元格，并支持比例缩放和像素值。 如果它们是像素值，则适用于每个子元素的大小已知，因此作为网格样式 [**Measure**](https://msdn.microsoft.com/library/windows/apps/br208952) 的输入大小传递。
-   面板本身可以引入用于在项目间填充的保留空间。 如果你执行此操作，请确保将度量显示为与 [**Margin**](https://msdn.microsoft.com/library/windows/apps/br208724) 或任何 **Padding** 属性不同的属性。
-   元素可能含有 [**ActualWidth**](https://msdn.microsoft.com/library/windows/apps/br208709) 和 [**ActualHeight**](https://msdn.microsoft.com/library/windows/apps/br208707) 属性的值，这些值基于上一次布局传递。 如果值更改，应用 UI 代码可以在元素上放置 [**LayoutUpdated**](https://msdn.microsoft.com/library/windows/apps/br208722) 的处理程序（如果有要运行的特殊逻辑），但是面板逻辑通常不需要使用事件处理检查更改。 布局系统已在确定何时重新运行布局，因为布局相关的属性已更改值，而且在适当情况下自动调用面板的 [**MeasureOverride**](https://msdn.microsoft.com/library/windows/apps/br208730) 或 [**ArrangeOverride**](https://msdn.microsoft.com/library/windows/apps/br208711)。

## **<a name="arrangeoverride"></a>ArrangeOverride**


[**ArrangeOverride**](https://msdn.microsoft.com/library/windows/apps/br208711) 方法有 [**Size**](https://msdn.microsoft.com/library/windows/apps/br225995) 返回值，当 [**Arrange**](https://msdn.microsoft.com/library/windows/apps/br208914) 在面板上受到布局中的父元素调用时，布局系统将在呈现面板本身时使用该值。 通常输入 *finalSize* 和 **ArrangeOverride** 返回的 **Size** 相同。 如果不相同，这意味着面板正尝试将自己调整为不同的大小，而不是布局中的其他参与者声明可用的大小。 最终大小基于之前已通过面板代码运行布局的度量传递，这是通常不返回不同大小的原因：这意味着你在有意地忽略度量逻辑。

请勿返回具有 **Infinity** 组件的 [**Size**](https://msdn.microsoft.com/library/windows/apps/br225995)。 尝试使用这样的 **Size** 将从内部布局引发异常。

所有 [**ArrangeOverride**](https://msdn.microsoft.com/library/windows/apps/br208711) 实现应当循环访问 [**Children**](https://msdn.microsoft.com/library/windows/apps/br227514)，并且对每个子元素调用 [**Arrange**](https://msdn.microsoft.com/library/windows/apps/br208914) 方法。 和 [**Measure**](https://msdn.microsoft.com/library/windows/apps/br208952) 一样，**Arrange** 没有返回值。 与 **Measure** 不同，经计算的属性不会设置为结果（但是，问题中的元素通常引发 [**LayoutUpdated**](https://msdn.microsoft.com/library/windows/apps/br208722) 事件）。

以下是 [**ArrangeOverride**](https://msdn.microsoft.com/library/windows/apps/br208711) 方法非常基本的框架：

```CSharp
protected override Size ArrangeOverride(Size finalSize)
{
    //loop through each Child, call Arrange on each
    foreach (UIElement child in Children)
    {
        Point anchorPoint = new Point(); //TODO more logic for topleft corner placement in your panel
       // for this child, and based on finalSize or other internal state of your panel
        child.Arrange(new Rect(anchorPoint, child.DesiredSize)); //OR, set a different Size 
    }
    return finalSize; //OR, return a different Size, but that's rare
}
```

布局的排列传递可能在未事先进行度量传递的情况下发生。 但是，这仅在布局系统已确定未出现可能影响之前度量的属性更改时发生。 例如，如果对齐发生更改，无需重新度量该特定元素，因为它的 [**DesiredSize**](https://msdn.microsoft.com/library/windows/apps/br208921) 不会在对齐选择更改时发生更改。 另一方面，如果 [**ActualHeight**](https://msdn.microsoft.com/library/windows/apps/br208707) 在布局中的任何元素上更改，将需要新的度量传递。 布局系统自动检测真度量更改，并且再次调用度量传递，然后运行另一次排列传递。

[**Arrange**](https://msdn.microsoft.com/library/windows/apps/br208914) 输入需要 [**Rect**](https://msdn.microsoft.com/library/windows/apps/br225994) 值。 构造此 **Rect** 的最常见方法是，使用具有 [**Point**](https://msdn.microsoft.com/library/windows/apps/br225870) 输入和 [**Size**](https://msdn.microsoft.com/library/windows/apps/br225995) 输入的构造函数。 **Point** 是应当放置元素的边界框左上角的点。 **Size** 是用于呈现该特定元素的维度。 请经常使用该元素的 [**DesiredSize**](https://msdn.microsoft.com/library/windows/apps/br208921) 作为此 **Size** 值，因为针对在布局中调用的所有元素建立 **DesiredSize** 是布局度量传递的目的。 （度量传递以迭代方式确定元素的全方位缩放，以便布局系统在开始排列传递后优化元素的放置方式。）

通常在 [**ArrangeOverride**](https://msdn.microsoft.com/library/windows/apps/br208711) 实现间变化的是面板确定如何安排每个子元素的 [**Point**](https://msdn.microsoft.com/library/windows/apps/br225870) 组件所依据的逻辑。 绝对定位面板（例如 [**Canvas**](https://msdn.microsoft.com/library/windows/apps/br209267)）使用它通过 [**Canvas.Left**](https://msdn.microsoft.com/library/windows/apps/hh759771) 和 [**Canvas.Top**](https://msdn.microsoft.com/library/windows/apps/hh759772) 值从每个元素获取的明确放置信息。 空间划分面板（例如 [**Grid**](https://msdn.microsoft.com/library/windows/apps/br242704)）将包含数学操作，此操作将可用空间划分为单元格，而且每个单元格将具有 x-y 值，以用于确定应当放置和排列其内容的位置。 自适应面板（例如 [**StackPanel**](https://msdn.microsoft.com/library/windows/apps/br209635)）可能扩展自身以在其方向维度中适合内容。

布局中的元素仍然受到其他定位影响，超出你直接控制的范围并且传递到 [**Arrange**](https://msdn.microsoft.com/library/windows/apps/br208914)。 它们来自 **Arrange** 的内部本机实现，此实现对 [**FrameworkElement**](https://msdn.microsoft.com/library/windows/apps/br208706) 派生的类型很常见，并且由一些其他的类型（例如文本元素）增强。 例如，元素可以具有边距和对齐，而且一些元素可以具有填充。 这些属性经常交互。 有关详细信息，请参阅[对齐、边距和填充](alignment-margin-padding.md)。

## <a name="panels-and-controls"></a>面板和控件


避免将功能放置到本应作为自定义控件构建的自定义面板中。 面板的作用是作为自动发生的布局功能，显示存在于其中的任何子元素内容。 面板可能将装饰添加到内容（与 [**Border**](https://msdn.microsoft.com/library/windows/apps/br209250) 在它所显示的元素周围添加边框的方式类似），或执行其他与布局相关的调整，例如填充。 但是，在扩展可视化树输出时，除了报告和使用来自子元素的信息外，你仅应当执行以上操作。

如果有任何交互可供用户访问，你应当编写自定义控件，而不是面板。 例如，面板不应当将滚动视区添加到它所显示的内容，即使目标是阻止剪裁，因为滚动栏、缩略图等是交互式控件部件。 （内容可能仍然具有滚动条，但是你应当使其遵循子元素的逻辑）。 请勿通过将滚动添加为布局操作来强制它。）你可以创建控件，还可以编写在控件的可视化树中发挥重要作用的自定义面板，用于在该控件中显示内容。 但是控件和面板应当是有区别的代码对象。

控件和面板之间的区别之所以重要的原因之一是 Microsoft UI 自动化和辅助功能。 面板提供视觉布局行为，而不是逻辑行为。 UI 元素在视觉上如何显示通常对辅助功能方案不是重要的 UI 特性。 辅助功能与展示逻辑上对理解 UI 重要的应用的部分相关。 当需要交互时，控件应当向 UI 自动化基础结构显示交互可能性。 有关详细信息，请参阅[自定义的自动化对等](https://msdn.microsoft.com/library/windows/apps/mt297667)。

## <a name="other-layout-api"></a>其他布局 API


有一些其他 API 是布局系统的一部分，但并非由 [**Panel**](https://msdn.microsoft.com/library/windows/apps/br227511) 声明。 你可以在面板实现中或在使用面板的自定义控件中使用它们。

-   [**UpdateLayout**](https://msdn.microsoft.com/library/windows/apps/br208989)、[**InvalidateMeasure**](https://msdn.microsoft.com/library/windows/apps/br208930) 和 [**InvalidateArrange**](https://msdn.microsoft.com/library/windows/apps/br208929) 是启动布局传递的方法。 **InvalidateArrange** 可能不会触发度量传递，但另外两个可以。 从不在布局方法内部调用这些方法替代，因为它们几乎肯定会导致布局循环。 控件代码通常也不需要调用它们。 布局的大部分特性通过检测对框架定义的布局属性（例如 [**Width**](/uwp/api/Windows.UI.Xaml.FrameworkElement.Width) 等）的更改来自动触发。
-   [**LayoutUpdated**](https://msdn.microsoft.com/library/windows/apps/br208722) 是当元素布局的一些特性已发生更改时引发的事件。 它不特定于面板；该事件由 [**FrameworkElement**](https://msdn.microsoft.com/library/windows/apps/br208706) 定义。
-   [**SizeChanged**](https://msdn.microsoft.com/library/windows/apps/br208742) 是仅在布局传递完成后引发的事件，并且显示 [**ActualHeight**](https://msdn.microsoft.com/library/windows/apps/br208707) 或 [**ActualWidth**](https://msdn.microsoft.com/library/windows/apps/br208709) 已更改作为结果。 这是另一个 [**FrameworkElement**](https://msdn.microsoft.com/library/windows/apps/br208706) 事件。 在某些情况下，[**LayoutUpdated**](https://msdn.microsoft.com/library/windows/apps/br208722) 会引发，但 **SizeChanged** 不会。 例如，内部内容可能已重新安排，但元素的大小并未更改。


## <a name="related-topics"></a>相关主题

**参考**
* [**FrameworkElement.ArrangeOverride**](https://msdn.microsoft.com/library/windows/apps/br208711)
* [**FrameworkElement.MeasureOverride**](https://msdn.microsoft.com/library/windows/apps/br208730)
* [**面板**](https://msdn.microsoft.com/library/windows/apps/br227511)

**概念**
* [对齐、边距和填充](alignment-margin-padding.md)
