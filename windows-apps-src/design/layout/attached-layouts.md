---
Description: 您可以定义附加的布局以用于如 ItemsRepeater 控件的容器。
title: AttachedLayout
label: AttachedLayout
template: detail.hbs
ms.date: 03/13/2019
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 6ff73b13acb5f5970bb79755b0bf5706fb12545a
ms.sourcegitcommit: c10d7843ccacb8529cb1f53948ee0077298a886d
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/04/2019
ms.locfileid: "58913987"
---
# <a name="attached-layouts"></a>附加的布局

委托到另一个对象其布局逻辑的容器 （例如面板） 依赖于要为其子元素提供的布局行为的附加的布局对象。  附加的布局模型提供了灵活性来更改在运行时，项的布局的应用程序或轻松共享的 UI （例如似乎对齐列中的表的行中的项） 的不同部分之间的布局的方面。

在本主题中，我们将介绍在创建的附加的布局 （虚拟化和非虚拟化） 的概念和类都需要了解，所涉及的内容和需要它们之间做出选择时要考虑的权衡取舍。

| **获取 Windows UI 库** |
| - |
| 此控件是作为 Windows UI 库，包含新控件和适用于 UWP 应用的 UI 功能的 NuGet 包的一部分。 有关详细信息，包括安装说明，请参阅[Windows 用户界面库概述](https://docs.microsoft.com/uwp/toolkits/winui/)。 |

> **重要的 API**：

> * [ScrollViewer](/uwp/api/windows.ui.xaml.controls.scrollviewer)
> * [ItemsRepeater](/windows/uwp/design/controls-and-patterns/items-repeater)
> * [布局](/uwp/api/microsoft.ui.xaml.controls.layout)
>     * [NonVirtualizingLayout](/uwp/api/microsoft.ui.xaml.controls.nonvirtualizinglayout)
>     * [VirtualizingLayout](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayout)
> * [LayoutContext](/uwp/api/microsoft.ui.xaml.controls.layoutcontext)
>     * [NonVirtualizingLayoutContext](/uwp/api/microsoft.ui.xaml.controls.nonvirtualizinglayoutcontext)
>     * [VirtualizingLayoutContext](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext)
> * [LayoutPanel](/uwp/api/microsoft.ui.xaml.controls.layoutpanel) （预览版）

## <a name="key-concepts"></a>关键概念

执行布局需要为每个元素，回答两个问题：

1. 什么***大小***将此元素是？

2. 将会什么***位置***此元素是？

XAML 的布局系统，回答这些问题，简要介绍作为一部分的讨论[自定义面板](/windows/uwp/design/layout/custom-panels-overview)。

### <a name="containers-and-context"></a>容器和上下文

从概念上讲，XAML 的[面板](/uwp/api/windows.ui.xaml.controls.panel)填充 framework 中的两个重要角色：

1. 它可以包含子元素，并引入了中的元素树的分支。
2. 它适用于这些子的一个特定的布局，策略。

出于此原因，XAML 中的一个面板通常已与布局，但从技术上讲，不止是布局。

[ItemsRepeater](/windows/uwp/design/controls-and-patterns/items-repeater)具体操作方面也像面板中，但，与不同面板，它不显示将允许以编程方式添加或删除 UIElement 子级的子级属性。  相反，对应于数据项的集合的框架会自动管理其子级的生存期。  尽管它不从 Panel 派生的但它的行为，由一个面板等框架进行处理。

> [!NOTE]
> [LayoutPanel](/uwp/api/microsoft.ui.xaml.controls.layoutpanel)是派生自面板，它将委托附加到其逻辑的容器[布局](/uwp/api/microsoft.ui.xaml.controls.layoutpanel.layout)对象。  LayoutPanel 处于*预览版*和目前仅以*预发行版*WinUI 包的删除。

#### <a name="containers"></a>容器

从概念上讲，[面板](/uwp/api/windows.ui.xaml.controls.panel)是一个容器元素，还具有的功能来呈现的像素[背景](/uwp/api/windows.ui.xaml.controls.panel.background)。  面板提供一种方法来封装在一个易于使用的包的常见布局逻辑。

这一概念**附加布局**使容器的两个角色和更清晰的布局之间的区别。  如果容器委托到另一个对象其布局逻辑我们将调用该对象的附加的布局如下面的代码段中所示。 继承的容器[FrameworkElement](/uwp/api/windows.ui.xaml.frameworkelement)，例如 LayoutPanel，自动使提供 XAML 的布局过程 （例如高度和宽度） 的输入的常见属性。

```xaml
<LayoutPanel>
    <LayoutPanel.Layout>
        <UniformGridLayout/>
    </LayoutPanel.Layout>
    <Button Content="1"/>
    <Button Content="2"/>
    <Button Content="3"/>
</LayoutPanel>
```

在布局过程依赖于上附加的容器*UniformGridLayout*测量和排列其子级。

#### <a name="per-container-state"></a>每个容器状态

使用附加的布局，可能与关联的布局对象的单一实例*许多*容器类似下面的代码段中; 因此，它不能依赖于或直接引用宿主容器。  例如：

```xaml
<!-- ... --->
<Page.Resources>
    <ExampleLayout x:Name="exampleLayout"/>
<Page.Resources>

<LayoutPanel x:Name="example1" Layout="{StaticResource exampleLayout}"/>
<LayoutPanel x:Name="example2" Layout="{StaticResource exampleLayout}"/>
<!-- ... --->
```

这种情况下*ExampleLayout*必须仔细考虑它使用在其布局计算并存储该状态，以免影响与另一个面板中的元素的布局的状态。  它将类似于自定义面板的值取决于其 MeasureOverride 和 ArrangeOverride 逻辑及其*静态*属性。

#### <a name="layoutcontext"></a>LayoutContext

目的[LayoutContext](/uwp/api/microsoft.ui.xaml.controls.layoutcontext)是应对这些挑战。  它提供附加的布局与宿主容器，如检索子元素，而不会引入两者之间的直接依赖项进行交互的功能。 上下文还使布局来存储它要求可能会与容器的子元素相关的任何状态。

简单、 非虚拟化布局通常不需要维护任何状态，使其不产生问题。 更复杂的布局，如网格中，但是，可以选择该度量值之间维护状态，并安排调用，以避免重新计算的值。

虚拟化布局*通常*需要维护一些状态之间的这两个度量值和迭代布局传递之间也排列。

#### <a name="initializing-and-uninitializing-per-container-state"></a>初始化和取消初始化每个容器状态

当布局附加到容器，其[InitializeForContextCore](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayout.initializeforcontextcore)方法调用，并且提供初始化某个对象以存储状态的机会。

同样，当布局正在删除容器中，从[UninitializeForContextCore](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayout.uninitializeforcontextcore)将调用方法。  这使布局有机会清理它必须与该容器关联的任何状态。

布局的状态对象可以与存储在一起并从与容器中检索[LayoutState](/uwp/api/microsoft.ui.xaml.controls.layoutcontext.layoutstate)上下文属性。

### <a name="ui-virtualization"></a>UI 虚拟化

UI 虚拟化意味着延迟创建 UI 对象，直到_需要时_。  它是一种性能优化。  对于非滚动方案确定_需要时_可以基于任意数量的那些特定于应用的对象。  在这些情况下，应用应考虑使用[x： 负载](../../xaml-platform/x-load-attribute.md)。 它不需要在布局中的任何特殊处理。

在基于滚动的方案，如列表，确定_需要时_通常基于"无法对用户可见"这主要取决于它已放置在布局过程中，需要特别注意的事项。  此方案是本文档的重点。

> [!NOTE]
> 尽管本文档中未涉及，但无法在非滚动的情况下应用启用 UI 虚拟化中滚动方案的相同功能。  例如，数据驱动工具栏控件，用于管理它呈现并且通过回收 / 可见区域和溢出菜单之间移动元素中的可用空间的更改进行响应的命令的生存期。

## <a name="getting-started"></a>入门

首先，确定是否需要创建的布局应支持 UI 虚拟化。

**需要记住的一些事项...**

1. 非虚拟化布局是作者更容易。 如果项的数目将始终为小然后创作非虚拟化布局建议。
2. 该平台提供了一组使用的附加布局[ItemsRepeater](/windows/uwp/design/controls-and-patterns/items-repeater#change-the-layout-of-items)并[LayoutPanel](/uwp/api/microsoft.ui.xaml.controls.layoutpanel)以涵盖了常见的需求。  熟悉这些之前您需要定义自定义布局。
3. 虚拟化布局始终具有一些其他的 CPU 和内存成本/复杂/开销与非虚拟化布局相比。  常规的规则的经验，如果子级布局将需要管理将可能能放在一个区域中的是 3 倍的视区的大小，则不可能有虚拟化布局多获得提升。 3 倍大小更高版本中此文档更详细地讨论，但由于在 Windows 和虚拟化其影响上滚动的异步特性。

> [!TIP]
> 作为参考的默认设置[ListView](/uwp/api/windows.ui.xaml.controls.listview) (和[ItemsRepeater](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater)) 是回收不会开始直到项的数目都不足以填满当前视区的大小 x 3。

**选择基类型**

![附加的布局层次结构](images/xaml-attached-layout-hierarchy.png)

基[布局](/uwp/api/microsoft.ui.xaml.controls.layout)类型具有两个充当用于创作附加的布局起点的派生的类型：

1. [NonVirtualizingLayout](/uwp/api/microsoft.ui.xaml.controls.nonvirtualizinglayout)
2. [VirtualizingLayout](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayout)

## <a name="non-virtualizing-layout"></a>非虚拟化布局

创建非虚拟化布局的方法应该感到很熟悉已创建的任何人都[自定义面板](/windows/uwp/design/layout/custom-panels-overview)。  涉及的概念同样适用。  主要区别在于[NonVirtualizingLayoutContext](/uwp/api/microsoft.ui.xaml.controls.nonvirtualizinglayoutcontext)使用访问[子级](/uwp/api/microsoft.ui.xaml.controls.nonvirtualizinglayoutcontext.children)集合和布局可以选择用于存储状态。

1. 派生自基类型[NonVirtualizingLayout](/uwp/api/microsoft.ui.xaml.controls.nonvirtualizinglayout) （而不是面板）。
2. *（可选）* 定义依赖属性，当更改将使布局失效。
3. _(**新建**/可选)_ 初始化所需的布局的一部分的任何状态对象[InitializeForContextCore](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayout.initializeforcontextcore)。 将其与宿主容器存储通过使用[LayoutState](/uwp/api/microsoft.ui.xaml.controls.layoutcontext.layoutstate)与上下文一起提供。
4. 重写[MeasureOverride](/uwp/api/microsoft.ui.xaml.controls.nonvirtualizinglayout.measureoverride) ，并调用[度量值](/uwp/api/windows.ui.xaml.uielement.measure)对所有的子对象的方法。
5. 重写[ArrangeOverride](/uwp/api/microsoft.ui.xaml.controls.nonvirtualizinglayout.arrangeoverride) ，并调用[排列](/uwp/api/windows.ui.xaml.uielement.arrange)对所有的子对象的方法。
6. *(**新建**/可选)* 清除任何已保存状态的一部分[UninitializeForContextCore](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayout.uninitializeforcontextcore)。

### <a name="example-a-simple-stack-layout-varying-sized-items"></a>例如：简单的堆栈布局 （大小不同的项）

![MyStackLayout](images/xaml-attached-layout-mystacklayout.png)

下面是非常基本非虚拟化堆栈布局的不同大小的项目。 它缺少要调整的布局行为的任何属性。 下面的实现说明了如何布局依赖为容器提供的上下文对象：

1. 获取子项的计数和
2. 按索引访问每个子元素。

```csharp
public class MyStackLayout : NonVirtualizingLayout
{
    protected override Size MeasureOverride(NonVirtualizingLayoutContext context, Size availableSize)
    {
        double extentHeight = 0.0;
        foreach (var element in context.Children)
        {
            element.Measure(availableSize);
            extentHeight += element.DesiredSize.Height;
        }

        return new Size(availableSize.Width, extentHeight);
    }

    protected override Size ArrangeOverride(NonVirtualizingLayoutContext context, Size finalSize)
    {
        double offset = 0.0;
        foreach (var element in context.Children)
        {
            element.Arrange(
                new Rect(0, offset, finalSize.Width, element.DesiredSize.Height));
            offset += element.DesiredSize.Height;
        }

        return finalSize;
    }
}
```

```xaml
 <LayoutPanel MaxWidth="196">
    <LayoutPanel.Layout>
        <local:MyStackLayout/>
    </LayoutPanel.Layout>

    <Button HorizontalAlignment="Stretch">1</Button>
    <Button HorizontalAlignment="Right">2</Button>
    <Button HorizontalAlignment="Center">3</Button>
    <Button>4</Button>

</LayoutPanel>
```

## <a name="virtualizing-layouts"></a>虚拟化布局

与非虚拟化布局类似，虚拟化布局的高级步骤是相同的。  复杂性是很大程度上在确定元素将位于视区和应意识到。

1. 派生自基类型[VirtualizingLayout](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayout)。
2. （可选）定义依赖属性，当更改将使布局失效。
3. 初始化所需的布局的一部分的任何状态对象[InitializeForContextCore](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayout.initializeforcontextcore)。 将其与宿主容器存储通过使用[LayoutState](/uwp/api/microsoft.ui.xaml.controls.layoutcontext.layoutstate)与上下文一起提供。
4. 重写[MeasureOverride](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayout.measureoverride) ，并调用[度量值](/uwp/api/windows.ui.xaml.uielement.measure)应意识到每个子级的方法。
   1. [GetOrCreateElementAt](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.getorcreateelementat)方法用于检索已准备好由框架 （例如应用的数据绑定） 的 UIElement。
5. 重写[ArrangeOverride](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayout.arrangeoverride) ，并调用[排列](/uwp/api/windows.ui.xaml.uielement.arrange)方法为每个已实现的孩子。
6. （可选）清除任何保存状态的一部分[UninitializeForContextCore](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayout.uninitializeforcontextcore)。

> [!TIP]
> 返回的值[MeasureOverride](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayout)用作虚拟化的内容的大小。

有两种创作虚拟化布局时需要考虑的常规方法。  若要选择一个或另一个很大程度上取决于"如何将确定元素大小"。  如果其足以知道数据集或数据本身中的项的索引可以确定其最终大小，则我们认为它很**依赖于数据**。  这些是更轻松地创建。  如果，但是，确定大小的项是创建并度量值 UI，那么我们所说的唯一方法是**依赖于内容的**。  这些是更复杂。

### <a name="the-layout-process"></a>布局过程

无论你要创建数据或依赖于内容的布局，务必了解布局过程和 Windows 的异步滚动的影响。

（通过） 到屏幕上显示用户界面的框架，启动时执行的步骤的简化的视图是：

1. 分析标记。

2. 生成元素的树。

3. 执行布局处理过程。

4. 执行呈现处理。

使用 UI 虚拟化，在步骤 2 中创建的元素通常所做的是延迟，或提前结束一次其已确定该足够内容已创建，以填充在视区。 虚拟化容器 (例如 ItemsRepeater) 遵从其附加布局来推动该过程。 它提供了包含的附加的布局[VirtualizingLayoutContext](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext)显示其他虚拟化布局所需的信息。

**RealizationRect （即视区）**

在 Windows 上滚动发生异步到 UI 线程。 不受框架的布局。  而是交互和移动发生在系统的复合器。 此方法的优点是，平移内容始终可按 60 fps。  这方面的挑战，但"视区"，显示的布局可能稍有过期相对于什么是在屏幕上可见。 如果用户快速滚动，它们可能会越过的 UI 线程以生成新的内容和"平移到黑色"的速度。 出于此原因，是通常所需的虚拟化的布局，以生成已准备好元素不足以填充区域大于视区的附加缓冲区。 当用户滚动过程中的较重负载下仍出示的内容。

![实现 rect](images/xaml-attached-layout-realizationrect.png)

由于创建元素时是代价高昂，虚拟化容器 (例如[ItemsRepeater](/windows/uwp/design/controls-and-patterns/items-repeater)) 程序先提供包含的附加的布局[RealizationRect](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.realizationrect)匹配视区。 空闲时容器可能会增长已准备好内容的缓冲区的重复调用使用越来越大的认识到矩形布局 此行为是一种性能优化，尝试进行快速的启动时间和良好的平移体验之间取得平衡。 最大缓冲区大小，将生成 ItemsRepeater 控制通过其[VerticalCacheLength](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater.verticalcachelength)并[HorizontalCacheLength](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater.verticalcachelength)属性。

**重新使用元素 （回收）**

布局预期大小和位置的元素以填充[RealizationRect](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.realizationrect)每次运行时。 默认情况下[VirtualizingLayout](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayout)将回收任何未使用的元素，每个布局处理过程结束时。

[VirtualizingLayoutContext](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext)作为的一部分传递给布局[MeasureOverride](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayout.measureoverride)并[ArrangeOverride](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayout.arrangeoverride)提供其他信息需要虚拟化布局。 一些它提供的最常用内容的功能如下：

1. 查询的数据中的项数 ([ItemCount](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.itemcount))。
2. 检索特定项使用[GetItemAt](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.getitemat)方法。
3. 检索[RealizationRect](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.realizationrect)表示视区和布局应具有填充的缓冲区意识到元素。
4. 请求特定的项与 UIElement [GetOrCreateElement](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.getorcreateelementat)方法。

请求给定索引的元素将导致该元素将被标记为"使用中"布局的阶段。 如果尚不存在元素，然后将会意识到，并自动准备好使用 （例如以下 UI 树定义中的 DataTemplate，处理任何数据绑定，等等。）。  否则，它将检索从现有实例池。

在每个测量处理过程结束时，任何现有的、 实现"中使用"未标记为元素自动被视为可供重复使用除非选项[SuppressAutoRecycle](/uwp/api/microsoft.ui.xaml.controls.elementrealizationoptions)通过检索到元素时使用[GetOrCreateElementAt](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.getorcreateelementat)方法。 框架会自动将其移动到回收池，并使其可。 它可能会随后将请求使用的不同容器。 框架尝试避免此如有可能因为没有重设关系元素与关联一些费用。

如果虚拟化布局知道每个度量值的元素将不再属于实现 rect 的开头，则它可以优化其重复使用。 而不是依赖框架的默认行为。 布局可提前移动元素到回收池通过使用[RecycleElement](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.recycleelement)方法。  请求新元素之前调用此方法使布局更高版本发出时能够使用这些现有元素[GetOrCreateElementAt](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.getorcreateelementat)的索引，尚未与元素关联的请求。

VirtualizingLayoutContext 提供两个面向布局作者创建依赖于内容的布局的其他属性。 它们是更详细地讨论更高版本。

1. 一个[RecommendedAnchorIndex](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.recommendedanchorindex)提供了一个可选_输入_到布局。
2. 一个[LayoutOrigin](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.layoutorigin) ，它是一个可选_输出_的布局。

## <a name="data-dependent-virtualizing-layouts"></a>依赖于数据的虚拟化布局

如果您知道每个项的大小应为无需度量值要显示的内容，则虚拟化布局容易。  在此文档中我们将只需引用此类别的虚拟化作为布局**数据布局**因为它们通常涉及检查数据。  基于应用程序可能会因为也许选取已知大小的可视表示形式的数据在其部分数据或以前由设计决定。

常规方法是对布局：

1. 计算大小和每个项的位置。
2. 作为的一部分[MeasureOverride](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayout.measureoverride):
   1. 使用[RealizationRect](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.realizationrect)来确定哪些项显示在视区内。
   2. 检索表示应具有的项的 UIElement [GetOrCreateElementAt](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.getorcreateelementat)方法。
   3. [度量值](/uwp/api/windows.ui.xaml.uielement.measure)UIElement 预先计算的大小。
3. 作为的一部分[ArrangeOverride](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayout.arrangeoverride)，[排列](/uwp/api/windows.ui.xaml.uielement.arrange)每个使用预先计算好的位置实现 UIElement。

> [!NOTE]
> 数据布局方法通常是与不兼容_数据虚拟化_。  具体而言，唯一的数据加载到内存是填充了对用户可见的内容所需的数据。  随着用户向该数据仍驻留下滚动，数据虚拟化不指延迟或增量加载的数据。  相反，它指当项从内存释放，因为会将它们滚动出视野之外。  无检查每个数据项，因为数据布局的一部分将会按预期方式阻止正常使用的数据虚拟化的数据布局。  例外情况是像 UniformGridLayout 假定所有一切必须大小相同的布局。

> [!TIP]
> 如果要创建自定义控件中各种情况下的其他人将使用的控件库的数据布局可能不是为你的选项。

### <a name="example-xbox-activity-feed-layout"></a>例如：Xbox 活动源的布局

Xbox 活动源的 UI 使用了其中每一行都具有宽磁贴，然后两个相反的窄磁贴上的后续行重复图案。 在此布局中，每个项的大小取决于项的位置中的数据集和磁贴的已知的大小 （宽 vs 缩小）。

![Xbox 活动源](images/xaml-attached-layout-activityfeedscreenshot.png)

下面演练哪些自定义虚拟化的活动源的 UI 可能为了说明常规何种方法的代码可能需要**的数据布局**。

<table>
<td>
    <p>如果有<strong style="font-weight: semi-bold">XAML 控件库</strong>应用程序安装，请单击此处可打开应用，请参阅<a href="xamlcontrolsgallery:/item/ItemsRepeater">ItemsRepeater</a>在此示例布局操作。</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">获取 XAML 控件库应用 (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics">获取源代码 (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

#### <a name="implementation"></a>实现

```csharp
/// <summary>
///  This is a custom layout that displays elements in two different sizes
///  wide (w) and narrow (n). There are two types of rows 
///  odd rows - narrow narrow wide
///  even rows - wide narrow narrow
///  This pattern repeats.
/// </summary>

public class ActivityFeedLayout : VirtualizingLayout // STEP #1 Inherit from base attached layout
{
    // STEP #2 - Parameterize the layout
    #region Layout parameters

    // We'll cache copies of the dependency properties to avoid calling GetValue during layout since that
    // can be quite expensive due to the number of times we'd end up calling these.
    private double _rowSpacing;
    private double _colSpacing;
    private Size _minItemSize = Size.Empty;

    /// <summary>
    /// Gets or sets the size of the whitespace gutter to include between rows
    /// </summary>
    public double RowSpacing
    {
        get { return _rowSpacing; }
        set { SetValue(RowSpacingProperty, value); }
    }

    /// <summary>
    /// Gets or sets the size of the whitespace gutter to include between items on the same row
    /// </summary>
    public double ColumnSpacing
    {
        get { return _colSpacing; }
        set { SetValue(ColumnSpacingProperty, value); }
    }

    public Size MinItemSize
    {
        get { return _minItemSize; }
        set { SetValue(MinItemSizeProperty, value); }
    }

    public static readonly DependencyProperty RowSpacingProperty =
        DependencyProperty.Register(
            nameof(RowSpacing),
            typeof(double),
            typeof(ActivityFeedLayout),
            new PropertyMetadata(0, OnPropertyChanged));

    public static readonly DependencyProperty ColumnSpacingProperty =
        DependencyProperty.Register(
            nameof(ColumnSpacing),
            typeof(double),
            typeof(ActivityFeedLayout),
            new PropertyMetadata(0, OnPropertyChanged));

    public static readonly DependencyProperty MinItemSizeProperty =
        DependencyProperty.Register(
            nameof(MinItemSize),
            typeof(Size),
            typeof(ActivityFeedLayout),
            new PropertyMetadata(Size.Empty, OnPropertyChanged));

    private static void OnPropertyChanged(DependencyObject obj, DependencyPropertyChangedEventArgs args)
    {
        var layout = obj as ActivityFeedLayout;
        if (args.Property == RowSpacingProperty)
        {
            layout._rowSpacing = (double)args.NewValue;
        }
        else if (args.Property == ColumnSpacingProperty)
        {
            layout._colSpacing = (double)args.NewValue;
        }
        else if (args.Property == MinItemSizeProperty)
        {
            layout._minItemSize = (Size)args.NewValue;
        }
        else
        {
            throw new InvalidOperationException("Don't know what you are talking about!");
        }

        layout.InvalidateMeasure();
    }

    #endregion

    #region Setup / teardown // STEP #3: Initialize state

    protected override void InitializeForContextCore(VirtualizingLayoutContext context)
    {
        base.InitializeForContextCore(context);

        var state = context.LayoutState as ActivityFeedLayoutState;
        if (state == null)
        {
            // Store any state we might need since (in theory) the layout could be in use by multiple
            // elements simultaneously
            // In reality for the Xbox Activity Feed there's probably only a single instance.
            context.LayoutState = new ActivityFeedLayoutState();
        }
    }

    protected override void UninitializeForContextCore(VirtualizingLayoutContext context)
    {
        base.UninitializeForContextCore(context);

        // clear any state
        context.LayoutState = null;
    }

    #endregion

    #region Layout // STEP #4,5 - Measure and Arrange

    protected override Size MeasureOverride(VirtualizingLayoutContext context, Size availableSize)
    {
        if (this.MinItemSize == Size.Empty)
        {
            var firstElement = context.GetOrCreateElementAt(0);
            firstElement.Measure(new Size(double.PositiveInfinity, double.PositiveInfinity));

            // setting the member value directly to skip invalidating layout
            this._minItemSize = firstElement.DesiredSize;
        }

        // Determine which rows need to be realized.  We know every row will have the same height and
        // only contain 3 items.  Use that to determine the index for the first and last item that
        // will be within that realization rect.
        var firstRowIndex = Math.Max(
            (int)(context.RealizationRect.Y / (this.MinItemSize.Height + this.RowSpacing)) - 1,
            0);
        var lastRowIndex = Math.Min(
            (int)(context.RealizationRect.Bottom / (this.MinItemSize.Height + this.RowSpacing)) + 1,
            (int)(context.ItemCount / 3));

        // Determine which items will appear on those rows and what the rect will be for each item
        var state = context.LayoutState as ActivityFeedLayoutState;
        state.LayoutRects.Clear();

        // Save the index of the first realized item.  We'll use it as a starting point during arrange.
        state.FirstRealizedIndex = firstRowIndex * 3;

        // ideal item width that will expand/shrink to fill available space
        double desiredItemWidth = Math.Max(this.MinItemSize.Width, (availableSize.Width - this.ColumnSpacing * 3) / 4);

        // Foreach item between the first and last index,
        //     Call GetElementOrCreateElementAt which causes an element to either be realized or retrieved
        //       from a recycle pool
        //     Measure the element using an appropriate size
        //
        // Any element that was previously realized which we don't retrieve in this pass (via a call to
        // GetElementOrCreateAt) will be automatically cleared and set aside for later re-use.
        // Note: While this work fine, it does mean that more elements than are required may be
        // created because it isn't until after our MeasureOverride completes that the unused elements
        // will be recycled and available to use.  We could avoid this by choosing to track the first/last
        // index from the previous layout pass.  The diff between the previous range and current range
        // would represent the elements that we can pre-emptively make available for re-use by calling
        // context.RecycleElement(element).
        for (int rowIndex = firstRowIndex; rowIndex < lastRowIndex; rowIndex++)
        {
            int firstItemIndex = rowIndex * 3;
            var boundsForCurrentRow = CalculateLayoutBoundsForRow(rowIndex, desiredItemWidth);

            for (int columnIndex = 0; columnIndex < 3; columnIndex++)
            {
                var index = firstItemIndex + columnIndex;
                var rect = boundsForCurrentRow[index % 3];
                var container = context.GetOrCreateElementAt(index);

                container.Measure(
                    new Size(boundsForCurrentRow[columnIndex].Width, boundsForCurrentRow[columnIndex].Height));

                state.LayoutRects.Add(boundsForCurrentRow[columnIndex]);
            }
        }

        // Calculate and return the size of all the content (realized or not) by figuring out
        // what the bottom/right position of the last item would be.
        var extentHeight = ((int)(context.ItemCount / 3) - 1) * (this.MinItemSize.Height + this.RowSpacing) + this.MinItemSize.Height;

        // Report this as the desired size for the layout
        return new Size(desiredItemWidth * 4 + this.ColumnSpacing * 2, extentHeight);
    }

    protected override Size ArrangeOverride(VirtualizingLayoutContext context, Size finalSize)
    {
        // walk through the cache of containers and arrange
        var state = context.LayoutState as ActivityFeedLayoutState;
        var virtualContext = context as VirtualizingLayoutContext;
        int currentIndex = state.FirstRealizedIndex;

        foreach (var arrangeRect in state.LayoutRects)
        {
            var container = virtualContext.GetOrCreateElementAt(currentIndex);
            container.Arrange(arrangeRect);
            currentIndex++;
        }

        return finalSize;
    }

    #endregion
    #region Helper methods

    private Rect[] CalculateLayoutBoundsForRow(int rowIndex, double desiredItemWidth)
    {
        var boundsForRow = new Rect[3];

        var yoffset = rowIndex * (this.MinItemSize.Height + this.RowSpacing);
        boundsForRow[0].Y = boundsForRow[1].Y = boundsForRow[2].Y = yoffset;
        boundsForRow[0].Height = boundsForRow[1].Height = boundsForRow[2].Height = this.MinItemSize.Height;

        if (rowIndex % 2 == 0)
        {
            // Left tile (narrow)
            boundsForRow[0].X = 0;
            boundsForRow[0].Width = desiredItemWidth;
            // Middle tile (narrow)
            boundsForRow[1].X = boundsForRow[0].Right + this.ColumnSpacing;
            boundsForRow[1].Width = desiredItemWidth;
            // Right tile (wide)
            boundsForRow[2].X = boundsForRow[1].Right + this.ColumnSpacing;
            boundsForRow[2].Width = desiredItemWidth * 2 + this.ColumnSpacing;
        }
        else
        {
            // Left tile (wide)
            boundsForRow[0].X = 0;
            boundsForRow[0].Width = (desiredItemWidth * 2 + this.ColumnSpacing);
            // Middle tile (narrow)
            boundsForRow[1].X = boundsForRow[0].Right + this.ColumnSpacing;
            boundsForRow[1].Width = desiredItemWidth;
            // Right tile (narrow)
            boundsForRow[2].X = boundsForRow[1].Right + this.ColumnSpacing;
            boundsForRow[2].Width = desiredItemWidth;
        }

        return boundsForRow;
    }

    #endregion
}

internal class ActivityFeedLayoutState
{
    public int FirstRealizedIndex { get; set; }

    /// <summary>
    /// List of layout bounds for items starting with the
    /// FirstRealizedIndex.
    /// </summary>
    public List<Rect> LayoutRects
    {
        get
        {
            if (_layoutRects == null)
            {
                _layoutRects = new List<Rect>();
            }

            return _layoutRects;
        }
    }

    private List<Rect> _layoutRects;
}
```

### <a name="optional-managing-the-item-to-uielement-mapping"></a>（可选）管理到 UIElement 的映射项

默认情况下[VirtualizingLayoutContext](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext)维护已实现的元素和它们所代表的数据源中的索引之间的映射。  可以选择一种布局来管理此映射本身始终请求的选项来[SuppressAutoRecycle](/uwp/api/microsoft.ui.xaml.controls.elementrealizationoptions)检索通过的元素时[GetOrCreateElementAt](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.getorcreateelementat)方法，这将阻止默认值自动回收行为。  布局可能选择执行此操作，例如，如果它将仅用于时滚动仅限于一个方向，则将其视为的项会总是连续 （即知道第一个和最后一个元素的索引只需知道应为 rea 的所有元素lized)。

#### <a name="example-xbox-activity-feed-measure"></a>例如：Xbox 活动源度量值

下面的代码段显示了可添加到在前面的示例 MeasureOverride，若要管理映射的其他逻辑。

```csharp
    protected override Size MeasureOverride(VirtualizingLayoutContext context, Size availableSize)
    {
        //...

        // Determine which items will appear on those rows and what the rect will be for each item
        var state = context.LayoutState as ActivityFeedLayoutState;
        state.LayoutRects.Clear();

         // Recycle previously realized elements that we know we won't need so that they can be used to
        // fill in gaps without requiring us to realize additional elements.
        var newFirstRealizedIndex = firstRowIndex * 3;
        var newLastRealizedIndex = lastRowIndex * 3 + 3;
        for (int i = state.FirstRealizedIndex; i < newFirstRealizedIndex; i++)
        {
            context.RecycleElement(state.IndexToElementMap.Get(i));
            state.IndexToElementMap.Clear(i);
        }

        for (int i = state.LastRealizedIndex; i < newLastRealizedIndex; i++)
        {
            context.RecycleElement(context.IndexElementMap.Get(i));
            state.IndexToElementMap.Clear(i);
        }

        // ...

        // Foreach item between the first and last index,
        //     Call GetElementOrCreateElementAt which causes an element to either be realized or retrieved
        //       from a recycle pool
        //     Measure the element using an appropriate size
        //
        for (int rowIndex = firstRowIndex; rowIndex < lastRowIndex; rowIndex++)
        {
            int firstItemIndex = rowIndex * 3;
            var boundsForCurrentRow = CalculateLayoutBoundsForRow(rowIndex, desiredItemWidth);

            for (int columnIndex = 0; columnIndex < 3; columnIndex++)
            {
                var index = firstItemIndex + columnIndex;
                var rect = boundsForCurrentRow[index % 3];
                UIElement container = null;
                if (state.IndexToElementMap.Contains(index))
                {
                    container = state.IndexToElementMap.Get(index);
                }
                else
                {
                    container = context = context.GetOrCreateElementAt(index, ElementRealizationOptions.ForceCreate | ElementRealizationOptions.SuppressAutoRecycle);
                    state.IndexToElementMap.Add(index, container);
                }

                container.Measure(
                    new Size(boundsForCurrentRow[columnIndex].Width, boundsForCurrentRow[columnIndex].Height));

                state.LayoutRects.Add(boundsForCurrentRow[columnIndex]);
            }
        }

        // ...
   }

internal class ActivityFeedLayoutState
{
    // ...
    Dictionary<int, UIElement> IndexToElementMap { get; set; }
    // ...
}
```

## <a name="content-dependent-virtualizing-layouts"></a>依赖于内容的虚拟化布局

如果您必须首先测量要找出其精确大小的项的 UI 内容，则它是**依赖于内容的布局**。  您还可以为每个项必须调整自身的布局，而不是告知其大小的项的布局将它。 属于此类别的虚拟化布局是更为复杂。

> [!NOTE]
> 不依赖于内容的布局 （不应该） 破坏数据虚拟化。

### <a name="estimations"></a>估计

依赖于内容的布局依赖估计猜出未实现内容的大小和已实现的内容的位置。 在这些估计值更改将导致已实现的内容来定期转变可滚动区域内的位置。 如果未得到缓解，这可能会导致非常令人沮丧而且不和谐的用户体验。 此处讨论的潜在问题和缓解措施。

> [!NOTE]
> 数据布局，请考虑每个项并知道的所有项目，请意识到和其位置的确切的大小可以完全避免这些问题。

**滚动锚定**

XAML 提供了一种机制来通过具有滚动控件支持来缓解突然视区 shifts[滚动锚定](/uwp/api/windows.ui.xaml.controls.iscrollanchorprovider)通过实现[IScrollAnchorPovider](/uwp/api/windows.ui.xaml.controls.iscrollanchorprovider)接口。 用户会操作内容，如滚动控件不断地从选择-中要跟踪的候选项的集中选择一个元素。 如果定位点元素的位置将在布局期间移动滚动控件将会自动转移其视区维护视区。

值[RecommendedAnchorIndex](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.recommendedanchorindex)提供给布局可能反映该选择的滚动控件的当前所选的定位点元素。 或者，如果开发人员显式请求一个元素意识到其索引[GetOrCreateElement](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater.getorcreateelement)方法[ItemsRepeater](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater)，则该索引指定为[RecommendedAnchorIndex](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.recommendedanchorindex)在下一步布局处理过程。 这使开发人员意识到一个元素，并随后请求，它会进入视图通过可能的方案准备好布局[StartBringIntoView](/uwp/api/windows.ui.xaml.uielement.startbringintoview)方法。

[RecommendedAnchorIndex](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.recommendedanchorindex)是估计其项的位置时，依赖于内容的布局应首先定位数据源中的项的索引。 它应作为用于定位其他已实现的项的起始点。

**对滚动条的影响**

即使使用滚动锚定，如果布局的估计值多种多样，可能是由于重大的变化的内容，然后滚动条滚动块的位置可能看起来周围跳转。  如果滚动块不会显示为跟踪其鼠标指针的位置，它们在拖动时，这可以是用户他们疑惑不解。

更准确地布局可以是在其估计，则用户不太可能将会看到一个在跳转的滚动条的滚动块。

### <a name="layout-corrections"></a>布局的更正

依赖于内容的布局应准备好与现实合理估算。  例如，当用户滚动到的内容和布局顶部意识到第一个元素，它可能会发现元素的预期的位置相对于从中启动它的元素将导致其原点 (x: 0 之外的地方出现y:0)。 当发生这种情况时，可以使用布局[LayoutOrigin](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.layoutorigin)属性设置为新的布局起点它计算而来的位置。  最终结果是类似于滚动锚定在其中滚动控件的视区自动调整到的内容位置的帐户报告的布局。

![更正 LayoutOrigin](images/xaml-attached-layout-origincorrection.png)

### <a name="disconnected-viewports"></a>断开连接的视区

返回与该布局的大小[MeasureOverride](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayout.measureoverride)方法表示的内容，可能会随每个后续布局的大小最佳猜测。  当用户滚动将不断地重新计算使用更新布局[RealizationRect](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.realizationrect)。

如果用户拖动滚动块非常快速地从布局，才会显示以使大的角度来看，视区则可以跳个之前的位置不会重叠现在当前位置。  这是因为滚动的异步特性。 还有可能使用布局来请求一个元素放入的项的当前未实现，预计当前跟踪的布局的范围之外的布局视图的应用。

在布局发现其猜测值不正确和/或看到意外的视区 shift，它需要重新调整其起始位置。  XAML 控件的一部分发布的虚拟化布局作为依赖于内容的布局开发，因为它们会将受限制较少的性质将显示的内容。


### <a name="example-simple-virtualizing-stack-layout-for-variable-sized-items"></a>例如：大小可变的项的简单虚拟化堆栈布局

下面的示例演示简单堆栈布局的大小可变的项：

* 支持用户界面虚拟化
* 使用估计猜出未实现项目的大小
* 识别潜在的不连续的视区 shifts 和
* 应用布局更正来应对这些变化。

**用法：标记**

```xaml
<ScrollViewer>

  <ItemsRepeater x:Name="repeater" >
    <ItemsRepeater.Layout>

      <local:VirtualizingStackLayout />

    </ItemsRepeater.Layout>
    <ItemsRepeater.ItemTemplate>
      <DataTemplate x:Key="item">
        <UserControl IsTabStop="True" UseSystemFocusVisuals="True" Margin="5">
          <StackPanel BorderThickness="1" Background="LightGray" Margin="5">
            <Image x:Name="recipeImage" Source="{Binding ImageUri}"  Width="100" Height="100"/>
              <TextBlock x:Name="recipeDescription"
                         Text="{Binding Description}"
                         TextWrapping="Wrap"
                         Margin="10" />
          </StackPanel>
        </UserControl>
      </DataTemplate>
    </ItemsRepeater.ItemTemplate>
  </ItemsRepeater>

</ScrollViewer>
```

**代码隐藏文件：Main.cs**

```csharp
string _lorem = @"Lorem ipsum dolor sit amet, consectetur adipiscing elit. Etiam laoreet erat vel massa rutrum, eget mollis massa vulputate. Vivamus semper augue leo, eget faucibus nulla mattis nec. Donec scelerisque lacus at dui ultricies, eget auctor ipsum placerat. Integer aliquet libero sed nisi eleifend, nec rutrum arcu lacinia. Sed a sem et ante gravida congue sit amet ut augue. Donec quis pellentesque urna, non finibus metus. Proin sed ornare tellus. Lorem ipsum dolor sit amet, consectetur adipiscing elit. Etiam laoreet erat vel massa rutrum, eget mollis massa vulputate. Vivamus semper augue leo, eget faucibus nulla mattis nec. Donec scelerisque lacus at dui ultricies, eget auctor ipsum placerat. Integer aliquet libero sed nisi eleifend, nec rutrum arcu lacinia. Sed a sem et ante gravida congue sit amet ut augue. Donec quis pellentesque urna, non finibus metus. Proin sed ornare tellus.";

var rnd = new Random();
var data = new ObservableCollection<Recipe>(Enumerable.Range(0, 300).Select(k =>
               new Recipe
               {
                   ImageUri = new Uri(string.Format("ms-appx:///Images/recipe{0}.png", k % 8 + 1)),
                   Description = k + " - " + _lorem.Substring(0, rnd.Next(50, 350))
               }));

repeater.ItemsSource = data;
```

**代码：VirtualizingStackLayout.cs**

```csharp
// This is a sample layout that stacks elements one after
// the other where each item can be of variable height. This is
// also a virtualizing layout - we measure and arrange only elements
// that are in the viewport. Not measuring/arranging all elements means
// that we do not have the complete picture and need to estimate sometimes.
// For example the size of the layout (extent) is an estimation based on the
// average heights we have seen so far. Also, if you drag the mouse thumb
// and yank it quickly, then we estimate what goes in the new viewport.

// The layout caches the bounds of everything that are in the current viewport.
// During measure, we might get a suggested anchor (or start index), we use that
// index to start and layout the rest of the items in the viewport relative to that
// index. Note that since we are estimating, we can end up with negative origin when
// the viewport is somewhere in the middle of the extent. This is achieved by setting the
// LayoutOrigin property on the context. Once this is set, future viewport will account
// for the origin.
public class VirtualizingStackLayout : VirtualizingLayout
{
    // Estimation state
    List<double> m_estimationBuffer = Enumerable.Repeat(0d, 100).ToList();
    int m_numItemsUsedForEstimation = 0;
    double m_totalHeightForEstimation = 0;

    // State to keep track of realized bounds
    int m_firstRealizedDataIndex = 0;
    List<Rect> m_realizedElementBounds = new List<Rect>();

    Rect m_lastExtent = new Rect();

    protected override Size MeasureOverride(VirtualizingLayoutContext context, Size availableSize)
    {
        var viewport = context.RealizationRect;
        DebugTrace("MeasureOverride: Viewport " + viewport);

        // Remove bounds for elements that are now outside the viewport.
        // Proactive recycling elements means we can reuse it during this measure pass again.
        RemoveCachedBoundsOutsideViewport(viewport);

        // Find the index of the element to start laying out from - the anchor
        int startIndex = GetStartIndex(context, availableSize);

        // Measure and layout elements starting from the start index, forward and backward.
        Generate(context, availableSize, startIndex, forward:true);
        Generate(context, availableSize, startIndex, forward:false);

        // Estimate the extent size. Note that this can have a non 0 origin.
        m_lastExtent = EstimateExtent(context, availableSize);
        context.LayoutOrigin = new Point(m_lastExtent.X, m_lastExtent.Y);
        return new Size(m_lastExtent.Width, m_lastExtent.Height);
    }

    protected override Size ArrangeOverride(VirtualizingLayoutContext context, Size finalSize)
    {
        DebugTrace("ArrangeOverride: Viewport" + context.RealizationRect);
        for (int realizationIndex = 0; realizationIndex < m_realizedElementBounds.Count; realizationIndex++)
        {
            int currentDataIndex = m_firstRealizedDataIndex + realizationIndex;
            DebugTrace("Arranging " + currentDataIndex);

            // Arrange the child. If any alignment needs to be done, it
            // can be done here.
            var child = context.GetOrCreateElementAt(currentDataIndex);
            var arrangeBounds = m_realizedElementBounds[realizationIndex];
            arrangeBounds.X -= m_lastExtent.X;
            arrangeBounds.Y -= m_lastExtent.Y;
            child.Arrange(arrangeBounds);
        }

        return finalSize;
    }

    // The data collection has changed, since we are maintaining the bounds of elements
    // in the viewport, we will update the list to account for the collection change.
    protected override void OnItemsChangedCore(VirtualizingLayoutContext context, object source, NotifyCollectionChangedEventArgs args)
    {
        InvalidateMeasure();
        if (m_realizedElementBounds.Count > 0)
        {
            switch (args.Action)
            {
                case NotifyCollectionChangedAction.Add:
                    OnItemsAdded(args.NewStartingIndex, args.NewItems.Count);
                    break;
                case NotifyCollectionChangedAction.Replace:
                    OnItemsRemoved(args.OldStartingIndex, args.OldItems.Count);
                    OnItemsAdded(args.NewStartingIndex, args.NewItems.Count);
                    break;
                case NotifyCollectionChangedAction.Remove:
                    OnItemsRemoved(args.OldStartingIndex, args.OldItems.Count);
                    break;
                case NotifyCollectionChangedAction.Reset:
                    m_realizedElementBounds.Clear();
                    m_firstRealizedDataIndex = 0;
                    break;
                default:
                    throw new NotImplementedException();
            }
        }
    }

    // Figure out which index to use as the anchor and start laying out around it.
    private int GetStartIndex(VirtualizingLayoutContext context, Size availableSize)
    {
        int startDataIndex = -1;
        var recommendedAnchorIndex = context.RecommendedAnchorIndex;
        bool isSuggestedAnchorValid = recommendedAnchorIndex != -1;

        if (isSuggestedAnchorValid)
        {
            if (IsRealized(recommendedAnchorIndex))
            {
                startDataIndex = recommendedAnchorIndex;
            }
            else
            {
                ClearRealizedRange();
                startDataIndex = recommendedAnchorIndex;
            }
        }
        else
        {
            // Find the first realized element that is visible in the viewport.
            startDataIndex = GetFirstRealizedDataIndexInViewport(context.RealizationRect);
            if (startDataIndex < 0)
            {
                startDataIndex = EstimateIndexForViewport(context.RealizationRect, context.ItemCount);
                ClearRealizedRange();
            }
        }

        // We have an anchorIndex, realize and measure it and
        // figure out its bounds.
        if (startDataIndex != -1 & context.ItemCount > 0)
        {
            if (m_realizedElementBounds.Count == 0)
            {
                m_firstRealizedDataIndex = startDataIndex;
            }

            var newAnchor = EnsureRealized(startDataIndex);
            DebugTrace("Measuring start index " + startDataIndex);
            var desiredSize = MeasureElement(context, startDataIndex, availableSize);

            var bounds = new Rect(
                0,
                newAnchor ?
                    (m_totalHeightForEstimation / m_numItemsUsedForEstimation) * startDataIndex : GetCachedBoundsForDataIndex(startDataIndex).Y,
                availableSize.Width,
                desiredSize.Height);
            SetCachedBoundsForDataIndex(startDataIndex, bounds);
        }

        return startDataIndex;
    }


    private void Generate(VirtualizingLayoutContext context, Size availableSize, int anchorDataIndex, bool forward)
    {
        // Generate forward or backward from anchorIndex until we hit the end of the viewport
        int step = forward ? 1 : -1;
        int previousDataIndex = anchorDataIndex;
        int currentDataIndex = previousDataIndex + step;
        var viewport = context.RealizationRect;
        while (IsDataIndexValid(currentDataIndex, context.ItemCount) &&
            ShouldContinueFillingUpSpace(previousDataIndex, forward, viewport))
        {
            EnsureRealized(currentDataIndex);
            DebugTrace("Measuring " + currentDataIndex);
            var desiredSize = MeasureElement(context, currentDataIndex, availableSize);
            var previousBounds = GetCachedBoundsForDataIndex(previousDataIndex);
            Rect currentBounds = new Rect(0,
                                          forward ? previousBounds.Y + previousBounds.Height : previousBounds.Y - desiredSize.Height,
                                          availableSize.Width,
                                          desiredSize.Height);
            SetCachedBoundsForDataIndex(currentDataIndex, currentBounds);
            previousDataIndex = currentDataIndex;
            currentDataIndex += step;
        }
    }

    // Remove bounds that are outside the viewport, leaving one extra since our
    // generate stops after generating one extra to know that we are outside the
    // viewport.
    private void RemoveCachedBoundsOutsideViewport(Rect viewport)
    {
        int firstRealizedIndexInViewport = 0;
        while (firstRealizedIndexInViewport < m_realizedElementBounds.Count &&
               !Intersects(m_realizedElementBounds[firstRealizedIndexInViewport], viewport))
        {
            firstRealizedIndexInViewport++;
        }

        int lastRealizedIndexInViewport = m_realizedElementBounds.Count - 1;
        while (lastRealizedIndexInViewport >= 0 &&
            !Intersects(m_realizedElementBounds[lastRealizedIndexInViewport], viewport))
        {
            lastRealizedIndexInViewport--;
        }

        if (firstRealizedIndexInViewport > 0)
        {
            m_firstRealizedDataIndex += firstRealizedIndexInViewport;
            m_realizedElementBounds.RemoveRange(0, firstRealizedIndexInViewport);
        }

        if (lastRealizedIndexInViewport >= 0 && lastRealizedIndexInViewport < m_realizedElementBounds.Count - 2)
        {
            m_realizedElementBounds.RemoveRange(lastRealizedIndexInViewport + 2, m_realizedElementBounds.Count - lastRealizedIndexInViewport - 3);
        }
    }

    private bool Intersects(Rect bounds, Rect viewport)
    {
        return !(bounds.Bottom < viewport.Top ||
            bounds.Top > viewport.Bottom);
    }

    private bool ShouldContinueFillingUpSpace(int dataIndex, bool forward, Rect viewport)
    {
        var bounds = GetCachedBoundsForDataIndex(dataIndex);
        return forward ?
            bounds.Y < viewport.Bottom :
            bounds.Y > viewport.Top;
    }

    private bool IsDataIndexValid(int currentDataIndex, int itemCount)
    {
        return currentDataIndex >= 0 && currentDataIndex < itemCount;
    }

    private int EstimateIndexForViewport(Rect viewport, int dataCount)
    {
        double averageHeight = m_totalHeightForEstimation / m_numItemsUsedForEstimation;
        int estimatedIndex = (int)(viewport.Top / averageHeight);
        // clamp to an index within the collection
        estimatedIndex = Math.Max(0, Math.Min(estimatedIndex, dataCount));
        return estimatedIndex;
    }

    private int GetFirstRealizedDataIndexInViewport(Rect viewport)
    {
        int index = -1;
        if (m_realizedElementBounds.Count > 0)
        {
            for (int i = 0; i < m_realizedElementBounds.Count; i++)
            {
                if (m_realizedElementBounds[i].Y < viewport.Bottom &&
                   m_realizedElementBounds[i].Bottom > viewport.Top)
                {
                    index = m_firstRealizedDataIndex + i;
                    break;
                }
            }
        }

        return index;
    }

    private Size MeasureElement(VirtualizingLayoutContext context, int index, Size availableSize)
    {
        var child = context.GetOrCreateElementAt(index);
        child.Measure(availableSize);

        int estimationBufferIndex = index % m_estimationBuffer.Count;
        bool alreadyMeasured = m_estimationBuffer[estimationBufferIndex] != 0;
        if (!alreadyMeasured)
        {
            m_numItemsUsedForEstimation++;
        }

        m_totalHeightForEstimation -= m_estimationBuffer[estimationBufferIndex];
        m_totalHeightForEstimation += child.DesiredSize.Height;
        m_estimationBuffer[estimationBufferIndex] = child.DesiredSize.Height;

        return child.DesiredSize;
    }

    private bool EnsureRealized(int dataIndex)
    {
        if (!IsRealized(dataIndex))
        {
            int realizationIndex = RealizationIndex(dataIndex);
            Debug.Assert(dataIndex == m_firstRealizedDataIndex - 1 ||
                dataIndex == m_firstRealizedDataIndex + m_realizedElementBounds.Count ||
                m_realizedElementBounds.Count == 0);

            if (realizationIndex == -1)
            {
                m_realizedElementBounds.Insert(0, new Rect());
            }
            else
            {
                m_realizedElementBounds.Add(new Rect());
            }

            if (m_firstRealizedDataIndex > dataIndex)
            {
                m_firstRealizedDataIndex = dataIndex;
            }

            return true;
        }

        return false;
    }

    // Figure out the extent of the layout by getting the number of items remaining
    // above and below the realized elements and getting an estimation based on
    // average item heights seen so far.
    private Rect EstimateExtent(VirtualizingLayoutContext context, Size availableSize)
    {
        double averageHeight = m_totalHeightForEstimation / m_numItemsUsedForEstimation;

        Rect extent = new Rect(0, 0, availableSize.Width, context.ItemCount * averageHeight);

        if (context.ItemCount > 0 && m_realizedElementBounds.Count > 0)
        {
            extent.Y = m_firstRealizedDataIndex == 0 ?
                            m_realizedElementBounds[0].Y :
                            m_realizedElementBounds[0].Y - (m_firstRealizedDataIndex - 1) * averageHeight;

            int lastRealizedIndex = m_firstRealizedDataIndex + m_realizedElementBounds.Count;
            if (lastRealizedIndex == context.ItemCount - 1)
            {
                var lastBounds = m_realizedElementBounds[m_realizedElementBounds.Count - 1];
                extent.Y = lastBounds.Bottom;
            }
            else
            {
                var lastBounds = m_realizedElementBounds[m_realizedElementBounds.Count - 1];
                int lastRealizedDataIndex = m_firstRealizedDataIndex + m_realizedElementBounds.Count;
                int numItemsAfterLastRealizedIndex = context.ItemCount - lastRealizedDataIndex;
                extent.Height = lastBounds.Bottom + numItemsAfterLastRealizedIndex * averageHeight - extent.Y;
            }
        }

        DebugTrace("Extent " + extent + " with average height " + averageHeight);
        return extent;
    }

    private bool IsRealized(int dataIndex)
    {
        int realizationIndex = dataIndex - m_firstRealizedDataIndex;
        return realizationIndex >= 0 && realizationIndex < m_realizedElementBounds.Count;
    }

    // Index in the m_realizedElementBounds collection
    private int RealizationIndex(int dataIndex)
    {
        return dataIndex - m_firstRealizedDataIndex;
    }

    private void OnItemsAdded(int index, int count)
    {
        // Using the old indexes here (before it was updated by the collection change)
        // if the insert data index is between the first and last realized data index, we need
        // to insert items.
        int lastRealizedDataIndex = m_firstRealizedDataIndex + m_realizedElementBounds.Count - 1;
        int newStartingIndex = index;
        if (newStartingIndex > m_firstRealizedDataIndex &&
            newStartingIndex <= lastRealizedDataIndex)
        {
            // Inserted within the realized range
            int insertRangeStartIndex = newStartingIndex - m_firstRealizedDataIndex;
            for (int i = 0; i < count; i++)
            {
                // Insert null (sentinel) here instead of an element, that way we do not
                // end up creating a lot of elements only to be thrown out in the next layout.
                int insertRangeIndex = insertRangeStartIndex + i;
                int dataIndex = newStartingIndex + i;
                // This is to keep the contiguousness of the mapping
                m_realizedElementBounds.Insert(insertRangeIndex, new Rect());
            }
        }
        else if (index <= m_firstRealizedDataIndex)
        {
            // Items were inserted before the realized range.
            // We need to update m_firstRealizedDataIndex;
            m_firstRealizedDataIndex += count;
        }
    }

    private void OnItemsRemoved(int index, int count)
    {
        int lastRealizedDataIndex = m_firstRealizedDataIndex + m_realizedElementBounds.Count - 1;
        int startIndex = Math.Max(m_firstRealizedDataIndex, index);
        int endIndex = Math.Min(lastRealizedDataIndex, index + count - 1);
        bool removeAffectsFirstRealizedDataIndex = (index <= m_firstRealizedDataIndex);

        if (endIndex >= startIndex)
        {
            ClearRealizedRange(RealizationIndex(startIndex), endIndex - startIndex + 1);
        }

        if (removeAffectsFirstRealizedDataIndex &&
            m_firstRealizedDataIndex != -1)
        {
            m_firstRealizedDataIndex -= count;
        }
    }

    private void ClearRealizedRange(int startRealizedIndex, int count)
    {
        m_realizedElementBounds.RemoveRange(startRealizedIndex, count);
        if (startRealizedIndex == 0)
        {
            m_firstRealizedDataIndex = m_realizedElementBounds.Count == 0 ? 0 : m_firstRealizedDataIndex + count;
        }
    }

    private void ClearRealizedRange()
    {
        m_realizedElementBounds.Clear();
        m_firstRealizedDataIndex = 0;
    }

    private Rect GetCachedBoundsForDataIndex(int dataIndex)
    {
        return m_realizedElementBounds[RealizationIndex(dataIndex)];
    }

    private void SetCachedBoundsForDataIndex(int dataIndex, Rect bounds)
    {
        m_realizedElementBounds[RealizationIndex(dataIndex)] = bounds;
    }

    private Rect GetCachedBoundsForRealizationIndex(int relativeIndex)
    {
        return m_realizedElementBounds[relativeIndex];
    }

    void DebugTrace(string message, params object[] args)
    {
        Debug.WriteLine(message, args);
    }
}
```

## <a name="related-articles"></a>相关文章

- [ItemsRepeater](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater)
- [ScrollViewer](/uwp/api/windows.ui.xaml.controls.scrollviewer)
