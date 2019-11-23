---
Description: 您可以定义用于容器的附加布局，如 ItemsRepeater 控件。
title: AttachedLayout
label: AttachedLayout
template: detail.hbs
ms.date: 03/13/2019
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: dc23e86f85c5db3dd10c5cec152047be387d4513
ms.sourcegitcommit: 445320ff0ee7323d823194d4ec9cfa6e710ed85d
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/11/2019
ms.locfileid: "72282288"
---
# <a name="attached-layouts"></a>附加布局

将其布局逻辑委托给另一个对象的容器（例如面板）依赖于附加的布局对象来提供其子元素的布局行为。  附加的布局模型为应用程序提供了灵活性，以便在运行时更改项的布局，或在 UI 的不同部分之间更轻松地共享布局的各个方面（例如，在列中显示为对齐的表的行中的项）。

在本主题中，我们介绍了创建附加布局（虚拟化和非虚拟化）所涉及的内容、需要了解的概念和类，以及在确定它们之间需要考虑的折衷。

| **获取 Windows UI 库** |
| - |
| 此控件作为 Windows UI 库的一部分提供，该库是一个 Nuget 包，包含新控件和 UWP 应用的 UI 功能。 有关详细信息（包括安装说明），请参阅 [Windows UI 库概述](https://docs.microsoft.com/uwp/toolkits/winui/)。 |

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

执行布局需要为每个元素回答两个问题：

1. 此元素的***大小***是多少？

2. 此元素的***位置***是什么？

XAML 的布局系统（回答这些问题）将在对[自定义面板](/windows/uwp/design/layout/custom-panels-overview)的讨论中进行简要介绍。

### <a name="containers-and-context"></a>容器和上下文

从概念上讲，XAML 的[面板](/uwp/api/windows.ui.xaml.controls.panel)填充了框架中的两个重要角色：

1. 它可以包含子元素，并在元素树中引入了分支。
2. 它将特定的布局策略应用于这些子策略。

出于此原因，XAML 中的面板通常与布局同义，但从技术上说，不仅仅是布局。

[ItemsRepeater](/windows/uwp/design/controls-and-patterns/items-repeater)的行为与面板的行为类似，但与面板不同，它不会公开允许以编程方式添加或删除 UIElement 子项的子属性。  相反，它的子级生存期由框架自动管理，以对应于数据项的集合。  尽管它不是从面板派生的，但它的行为和由框架（如面板）处理。

> [!NOTE]
> [LayoutPanel](/uwp/api/microsoft.ui.xaml.controls.layoutpanel)是一个从 Panel 派生的容器，它将其逻辑委托给附加的[布局](/uwp/api/microsoft.ui.xaml.controls.layoutpanel.layout)对象。  LayoutPanel 处于*预览阶段*，目前仅在 WinUI 包的*预发行*版本中可用。

#### <a name="containers"></a>容器

从概念上讲， [Panel](/uwp/api/windows.ui.xaml.controls.panel)是元素的容器，还可以呈现[背景](/uwp/api/windows.ui.xaml.controls.panel.background)像素。  面板提供了在易于使用的包中封装公共布局逻辑的方法。

**附加布局**的概念使得容器和布局的两个角色之间的区别更加清晰。  如果容器将其布局逻辑委托给另一个对象，我们会将该对象称为附加的布局，如以下代码片段所示。 从[FrameworkElement](/uwp/api/windows.ui.xaml.frameworkelement)继承的容器（如 LayoutPanel）会自动公开提供 XAML 布局进程（例如，Height 和 Width）的输入的通用属性。

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

在布局过程中，容器依靠附加的*UniformGridLayout*来测量和排列其子级。

#### <a name="per-container-state"></a>每容器状态

使用附加布局时，布局对象的单个实例可能与*多个*容器关联，如下面的代码段中所示;因此，它不能依赖于或直接引用宿主容器。  例如：

```xaml
<!-- ... --->
<Page.Resources>
    <ExampleLayout x:Name="exampleLayout"/>
<Page.Resources>

<LayoutPanel x:Name="example1" Layout="{StaticResource exampleLayout}"/>
<LayoutPanel x:Name="example2" Layout="{StaticResource exampleLayout}"/>
<!-- ... --->
```

对于这种情况， *ExampleLayout*必须仔细考虑它在其布局计算中使用的状态，以及该状态的存储位置，以避免影响一个面板中元素的布局。  它类似于自定义面板，其 System.windows.frameworkelement.measureoverride 和 System.windows.frameworkelement.arrangeoverride 逻辑依赖于其*静态*属性的值。

#### <a name="layoutcontext"></a>LayoutContext

[LayoutContext](/uwp/api/microsoft.ui.xaml.controls.layoutcontext)的目的是应对这些挑战。  它为附加的布局提供与宿主容器交互的能力，如检索子元素，而不引入二者之间的直接依赖项。 上下文还启用布局，以存储可能与容器的子元素相关的任何状态。

简单的非虚拟化布局通常不需要维护任何状态，因此不会出现问题。 但是，更复杂的布局（如网格）可以选择在度量值和排列调用之间保持状态，以避免重新计算值。

虚拟化布局*通常*需要在度量值和排列之间以及迭代布局阶段之间维护某种状态。

#### <a name="initializing-and-uninitializing-per-container-state"></a>初始化和取消初始化每个容器的状态

将布局附加到容器时，会调用其[InitializeForContextCore](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayout.initializeforcontextcore)方法，并提供初始化对象以存储状态的机会。

同样，从容器中移除布局时，将调用[UninitializeForContextCore](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayout.uninitializeforcontextcore)方法。  这使布局有机会清除它与该容器关联的任何状态。

布局的状态对象可与上下文中的[LayoutState](/uwp/api/microsoft.ui.xaml.controls.layoutcontext.layoutstate)属性一起存储在容器中并从中检索。

### <a name="ui-virtualization"></a>UI 虚拟化

UI 虚拟化意味着在_需要时_延迟创建 UI 对象。  这是一种性能优化。  对于非滚动方案，确定_所需时间_可能基于特定于应用的任何数量。  在这些情况下，应用应考虑使用[x:Load](../../xaml-platform/x-load-attribute.md)。 不需要在布局中进行任何特殊处理。

在基于滚动的方案（如列表）中，确定_所需的时间_通常是基于 "对用户可见" 的，这种情况很大程度上取决于布局过程中的放置位置，需要特别注意。  此方案是本文档的重点。

> [!NOTE]
> 虽然本文档未涵盖，但在滚动方案中实现 UI 虚拟化的功能也可以应用于非滚动方案。  例如，一种数据驱动的工具栏控件，可管理它所呈现的命令的生存期，并通过在可见区域和溢出菜单之间回收/移动元素来响应可用空间的更改。

## <a name="getting-started"></a>入门

首先，确定需要创建的布局是否应支持 UI 虚拟化。

**请注意以下几点：**

1. 非虚拟化布局更易于创作。 如果项目数始终为小，则建议编写非虚拟化布局。
2. 该平台提供一组附加的布局，这些布局适用于[ItemsRepeater](/windows/uwp/design/controls-and-patterns/items-repeater#change-the-layout-of-items)和[LayoutPanel](/uwp/api/microsoft.ui.xaml.controls.layoutpanel) ，以满足常见需求。  在确定需要定义自定义布局之前，先熟悉这些要求。
3. 与非虚拟化布局相比，虚拟化布局始终具有额外的 CPU 和内存成本/复杂性/开销。  一般的经验法则是，如果布局需要进行管理，则可能会将其放在视区大小的3倍的区域，这可能不会从虚拟化布局中获得很大的收益。 此文档稍后将更详细地讨论3倍大小，但这是因为在 Windows 上滚动的异步性质及其对虚拟化的影响。

> [!TIP]
> 作为参考， [ListView](/uwp/api/windows.ui.xaml.controls.listview) （和[ItemsRepeater](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater)）的默认设置是循环不会开始循环，直到项数足以填充当前视区的大小。

**选择基类型**

![附加布局层次结构](images/xaml-attached-layout-hierarchy.png)

基本[布局](/uwp/api/microsoft.ui.xaml.controls.layout)类型具有两个派生类型，用作创作附加布局的起点：

1. [NonVirtualizingLayout](/uwp/api/microsoft.ui.xaml.controls.nonvirtualizinglayout)
2. [VirtualizingLayout](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayout)

## <a name="non-virtualizing-layout"></a>非虚拟化布局

创建非虚拟化布局的方法对于创建[自定义面板](/windows/uwp/design/layout/custom-panels-overview)的任何人都非常熟悉。  相同的概念也适用。  主要区别在于， [NonVirtualizingLayoutContext](/uwp/api/microsoft.ui.xaml.controls.nonvirtualizinglayoutcontext)用于访问[子](/uwp/api/microsoft.ui.xaml.controls.nonvirtualizinglayoutcontext.children)集合，布局可能选择存储状态。

1. 从基类型[NonVirtualizingLayout](/uwp/api/microsoft.ui.xaml.controls.nonvirtualizinglayout) （而不是面板）派生。
2. *（可选）* 定义发生更改时将使布局失效的依赖项属性。
3. _（**New**/Optional）_ 将布局所需的任何状态对象初始化为[InitializeForContextCore](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayout.initializeforcontextcore)的一部分。 使用随上下文提供的[LayoutState](/uwp/api/microsoft.ui.xaml.controls.layoutcontext.layoutstate)将其与主机容器一起使用。
4. 重写[system.windows.frameworkelement.measureoverride](/uwp/api/microsoft.ui.xaml.controls.nonvirtualizinglayout.measureoverride)并对所有子级调用[Measure](/uwp/api/windows.ui.xaml.uielement.measure)方法。
5. 重写[system.windows.frameworkelement.arrangeoverride](/uwp/api/microsoft.ui.xaml.controls.nonvirtualizinglayout.arrangeoverride)并对所有子级调用 "[排列](/uwp/api/windows.ui.xaml.uielement.arrange)" 方法。
6. *（**New**/Optional）* 清除[UninitializeForContextCore](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayout.uninitializeforcontextcore)中的任何已保存状态。

### <a name="example-a-simple-stack-layout-varying-sized-items"></a>示例：简单的堆栈布局（大小变化的项）

![MyStackLayout](images/xaml-attached-layout-mystacklayout.png)

下面是大小可变项的基本非虚拟化堆栈布局。 它缺少用于调整布局行为的任何属性。 以下实现阐释了布局如何依赖于容器提供的上下文对象：

1. 获取子级的计数，并
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

与非虚拟化布局类似，虚拟化布局的高级步骤是相同的。  复杂性很大程度上取决于确定哪些元素会落在视区内并且应该实现。

1. 派生自基类型[VirtualizingLayout](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayout)。
2. 可有可无定义依赖属性，更改该属性后，布局会失效。
3. 将布局所需的任何状态对象初始化为[InitializeForContextCore](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayout.initializeforcontextcore)的一部分。 使用随上下文提供的[LayoutState](/uwp/api/microsoft.ui.xaml.controls.layoutcontext.layoutstate)将其与主机容器一起使用。
4. 重写[system.windows.frameworkelement.measureoverride](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayout.measureoverride)并为应实现的每个子项调用[Measure](/uwp/api/windows.ui.xaml.uielement.measure)方法。
   1. [GetOrCreateElementAt](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.getorcreateelementat)方法用于检索已由框架准备的 UIElement （例如，已应用数据绑定）。
5. 重写[system.windows.frameworkelement.arrangeoverride](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayout.arrangeoverride)并为每个已实现的子级调用 "[排列](/uwp/api/windows.ui.xaml.uielement.arrange)" 方法。
6. 可有可无清除[UninitializeForContextCore](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayout.uninitializeforcontextcore)中的任何已保存状态。

> [!TIP]
> [System.windows.frameworkelement.measureoverride](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayout)返回的值用作虚拟化内容的大小。

在创作虚拟化布局时，有两种常规方法需要考虑。  在很大程度上取决于 "如何确定元素的大小"。  如果它足以知道数据集中某项的索引，或者数据本身决定了其最终大小，则会将其视为**依赖于数据的数据**。  这种创建更简单。  但是，如果确定某项的大小的唯一方法是创建并度量 UI，则会说它**依赖于内容**。  它们更复杂。

### <a name="the-layout-process"></a>布局过程

无论您创建的是数据还是依赖于内容的布局，理解布局过程和 Windows 异步滚动的影响都很重要。

（Over）简化了框架从启动到在屏幕上显示 UI 时执行的步骤：

1. 它分析标记。

2. 生成元素树。

3. 执行布局处理。

4. 执行渲染处理。

通过 UI 虚拟化，创建通常在步骤2中执行的元素将在确定已创建足够的内容以填充视区后提前延迟或结束。 虚拟化容器（例如，ItemsRepeater）会根据其附加布局来驱动此进程。 它为附加的布局提供一个[VirtualizingLayoutContext](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext) ，用于显示虚拟化布局所需的其他信息。

**RealizationRect （即视区）**

在 Windows 上滚动时，会异步执行到 UI 线程。 它不受框架布局的控制。  相反，交互和移动发生在系统的组合器中。 此方法的优点是只能在60fps 上完成平移内容。  但这一难题在于布局所看到的 "视区" 可能会相对于屏幕上实际显示的内容稍有不同。 如果用户快速滚动，则他们可能会越过 UI 线程生成新内容和 "平移到黑色" 的速度。 出于此原因，通常需要使用虚拟化布局来生成已准备好的元素的附加缓冲区，足以填充大于视区的区域。 如果在滚动过程中重负载较重，仍会向用户显示内容。

![实现 rect](images/xaml-attached-layout-realizationrect.png)

由于元素创建成本高昂，虚拟化容器（例如， [ItemsRepeater](/windows/uwp/design/controls-and-patterns/items-repeater)）将最初提供与视区匹配的[RealizationRect](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.realizationrect)的附加布局。 在空闲时，容器可以通过使用越来越大的实现矩形对布局进行重复调用，从而增加已准备的内容缓冲区。 此行为是一种性能优化，它尝试在快速启动时间与良好的平移体验之间取得平衡。 ItemsRepeater 将生成的最大缓冲区大小由其[VerticalCacheLength](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater.verticalcachelength)和[HorizontalCacheLength](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater.verticalcachelength)属性控制。

**重新使用元素（循环）**

布局应在每次运行时都要调整元素大小并定位，以填充[RealizationRect](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.realizationrect) 。 默认情况下， [VirtualizingLayout](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayout)将在每个布局处理结束时回收任何未使用的元素。

作为[system.windows.frameworkelement.measureoverride](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayout.measureoverride)和[system.windows.frameworkelement.arrangeoverride](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayout.arrangeoverride)的一部分传递到布局的[VirtualizingLayoutContext](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext)提供了虚拟化布局所需的其他信息。 它提供的一些最常用的功能是：

1. 查询数据中的项目数（[ItemCount](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.itemcount)）。
2. 使用[GetItemAt](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.getitemat)方法检索特定项。
3. 检索表示布局应使用已实现元素填充的视区和缓冲区的[RealizationRect](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.realizationrect) 。
4. 使用[GetOrCreateElement](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.getorcreateelementat)方法为特定项请求 UIElement。

为给定索引请求元素将导致该布局的传递将该元素标记为 "正在使用"。 如果该元素尚不存在，则它将实现并自动准备就绪（例如，因为这样做 System.windows.datatemplate> 中定义的 UI 树，处理任何数据绑定，等等）。  否则，将从现有实例的池中检索它。

每个度量值结束时，任何未标记为 "正在使用" 的现有已实现元素都将被自动视为可重复使用，除非通过[GetOrCreateElementAt](/uwp/api/microsoft.ui.xaml.controls.elementrealizationoptions) 方法检索元素是使用了 [SuppressAutoRecycle](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.getorcreateelementat)。 框架自动将其移动到回收池并使其可用。 它随后可能会被其他容器使用。 如果可能，该框架会尽量避免这种情况，因为存在一些与重新父级元素相关的成本。

如果虚拟化布局知道每个度量值开始时，哪些元素将不再位于实现矩形内，则它可以优化其重复使用。 而不是依赖于框架的默认行为。 布局可以提前使用[RecycleElement](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.recycleelement)方法将元素移动到回收池。  在请求新元素之前调用此方法会导致这些现有元素在布局之后对尚未与某个元素关联的索引发出[GetOrCreateElementAt](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.getorcreateelementat)请求时可用。

VirtualizingLayoutContext 提供了其他两个属性，这些属性设计用于创建内容相关布局的布局作者。 稍后将对其进行更详细的讨论。

1. 提供要布局的可选_输入_的[RecommendedAnchorIndex](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.recommendedanchorindex) 。
2. 作为布局的可选_输出_的[LayoutOrigin](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.layoutorigin) 。

## <a name="data-dependent-virtualizing-layouts"></a>数据相关的虚拟化布局

如果知道每个项的大小应该不需要衡量要显示的内容，则虚拟化布局会更容易。  在本文档中，我们只是将这种类型的虚拟化布局作为**数据布局**，因为它们通常涉及到数据的检查。  根据数据，应用可能会选取具有已知大小的视觉对象表示形式，这可能是因为它的数据部分或之前是由设计决定的。

常规方法是对的布局进行以下操作：

1. 计算每个项的大小和位置。
2. 作为[system.windows.frameworkelement.measureoverride](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayout.measureoverride)的一部分：
   1. 使用[RealizationRect](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.realizationrect)确定应在视区中显示哪些项。
   2. 检索应该用[GetOrCreateElementAt](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.getorcreateelementat)方法表示该项的 UIElement。
   3. 用预计算的大小[度量](/uwp/api/windows.ui.xaml.uielement.measure)UIElement。
3. 作为[system.windows.frameworkelement.arrangeoverride](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayout.arrangeoverride)的一部分，请将每个已实现的 UIElement 与预先计算的位置进行[排列](/uwp/api/windows.ui.xaml.uielement.arrange)。

> [!NOTE]
> 数据布局方法通常与_数据虚拟化_不兼容。  具体而言，仅将加载到内存中的数据加载到内存中的数据是填充用户可见内容所需的数据。  当用户向下滚动数据的驻留位置时，数据虚拟化不会引用延迟或增量加载数据。  相反，它会引用从内存中释放项的时间，因为它们会滚动到视图之外。  使用数据布局检查作为数据布局一部分的每个数据项会阻止数据虚拟化按预期方式工作。  异常是类似于 UniformGridLayout 的布局，它假定所有内容都具有相同的大小。

> [!TIP]
> 如果要为控件库创建自定义控件，而其他人将在各种情况下使用该控件，则可能不会为你选择数据布局。

### <a name="example-xbox-activity-feed-layout"></a>示例： Xbox 活动源布局

Xbox 活动源的 UI 使用重复模式，其中每行都有一个宽磁贴，后跟在后续行上反转的两个窄图块。 在此布局中，每个项的大小是项在数据集中的位置的功能，以及磁贴的已知大小（宽型和窄型）。

![Xbox 活动源](images/xaml-attached-layout-activityfeedscreenshot.png)

下面的代码演示了活动源的自定义虚拟化 UI 的定义，它可能是为了说明**数据布局**的一般方法。

<table>
<td>
    <p>如果已安装了<strong style="font-weight: semi-bold">XAML 控件库</strong>应用程序，请单击此处打开应用程序，并使用此示例布局查看<a href="xamlcontrolsgallery:/item/ItemsRepeater">ItemsRepeater</a>的操作。</p>
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

### <a name="optional-managing-the-item-to-uielement-mapping"></a>可有可无管理要 UIElement 映射的项

默认情况下， [VirtualizingLayoutContext](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext)在所表示的数据源中的已实现元素与索引之间维护映射。  布局可以选择在通过阻止默认自动回收行为的[GetOrCreateElementAt](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.getorcreateelementat)方法检索元素时，始终请求[SuppressAutoRecycle](/uwp/api/microsoft.ui.xaml.controls.elementrealizationoptions)的选项来管理此映射本身。  布局可以选择执行此操作，例如，仅当滚动仅限一个方向并且它认为的项将始终是连续的时（即，知道第一个和最后一个元素的索引就足以知道应该 rea 的所有元素。lized).

#### <a name="example-xbox-activity-feed-measure"></a>示例： Xbox 活动源度量值

下面的代码片段显示了可添加到前面示例中的 System.windows.frameworkelement.measureoverride 以管理映射的其他逻辑。

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

## <a name="content-dependent-virtualizing-layouts"></a>依赖内容的虚拟化布局

如果必须首先测量某个项的 UI 内容才能确定其准确大小，则它是**依赖于内容的布局**。  您还可以将其视为一种布局，其中每个项必须自行调整大小，而不是指示项大小的布局。 此类别中的虚拟化布局更多。

> [!NOTE]
> 依赖于内容的布局不会中断数据虚拟化。

### <a name="estimations"></a>估计

依赖于内容的布局会依赖于估计来猜测未实现内容的大小和已实现内容的位置。 随着这些估计值的更改，它将导致已实现的内容在可滚动区域内定期移动位置。 如果不减少，这可能会导致用户体验变得非常令人沮丧。 这里讨论了潜在的问题和缓解措施。

> [!NOTE]
> 数据布局可考虑每个项并了解所有项的准确大小（已实现或未实现）及其位置可完全避免这些问题。

**滚动定位**

XAML 提供一种机制，可通过实现[IScrollAnchorPovider](/uwp/api/windows.ui.xaml.controls.iscrollanchorprovider)接口，通过滚动控件支持[滚动定位](/uwp/api/windows.ui.xaml.controls.iscrollanchorprovider)来缓解突然的视区偏移。 当用户操作内容时，滚动控件会持续从选择要跟踪的候选集合中选择一个元素。 如果定位点元素在布局中的位置改变，滚动控件会自动将其视区移动到维护视区。

为布局提供的[RecommendedAnchorIndex](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.recommendedanchorindex)的值可能反映滚动控件所选择的当前选定定位点元素。 或者，如果开发人员显式请求使用[ItemsRepeater](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater)上的[GetOrCreateElement](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater.getorcreateelement)方法为索引实现某个元素，则在下一次布局传递时，该索引将被指定为[RecommendedAnchorIndex](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.recommendedanchorindex) 。 这样，便可以针对开发人员认识到元素，并随后请求通过[StartBringIntoView](/uwp/api/windows.ui.xaml.uielement.startbringintoview)方法进入视图中的情况来准备布局。

[RecommendedAnchorIndex](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.recommendedanchorindex)是数据源中的项的索引，与内容相关的布局在估计其项的位置时应首先定位。 它应充当定位其他已实现项的起点。

**滚动条的影响**

即使使用滚动定位，如果布局的估计值变化很大，可能是由于内容大小有重大变化，滚动条的滚动块的位置可能看上去像是跳跃。  如果在拖动滚动块时未显示滚动鼠标指针的位置，则可以为用户 jarring 这种情况。

布局可以在其估计中越精确，用户看到滚动条的滚动块的可能性就越小。

### <a name="layout-corrections"></a>布局更正

应准备好与内容相关的布局，以便通过现实来合理化其估算。  例如，当用户滚动到内容顶部并且布局实现了第一个元素时，它可能会发现，相对于其开始位置的元素，该元素的预期位置将导致该元素出现在除原点之外的任何位置（x:0, y:0). 出现这种情况时，布局可以使用[LayoutOrigin](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.layoutorigin)属性将它计算的位置设置为新布局源。  最终结果类似于滚动定位，滚动控件的视区会自动调整以反映布局报告的内容位置。

![更正 LayoutOrigin](images/xaml-attached-layout-origincorrection.png)

### <a name="disconnected-viewports"></a>断开连接的视区

从布局的[system.windows.frameworkelement.measureoverride](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayout.measureoverride)方法返回的大小表示最佳推测，即内容大小可能随每个连续布局发生变化。  当用户滚动时，将使用更新的[RealizationRect](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.realizationrect)持续重新计算布局。

如果用户从布局的角度来看，从布局的角度来看，这种滚动块速度非常快，则显示为在以前的位置与当前位置不重叠的情况下进行大幅跳转。  这是由于滚动的异步性质导致的。 还可能是使用布局的应用请求为当前未实现的项提供一个元素，并估计该元素在布局跟踪的当前范围之外进行布局。

当布局发现其推测不正确并且/或发现意外的视区变化时，它需要重定向其起始位置。  作为 XAML 控件一部分提供的虚拟化布局作为内容相关的布局进行开发，因为它们对要显示的内容的性质施加的限制更少。


### <a name="example-simple-virtualizing-stack-layout-for-variable-sized-items"></a>示例：简单的可变大小项的虚拟化堆栈布局

下面的示例演示了可变大小的项的简单堆栈布局：

* 支持 UI 虚拟化，
* 使用估算来推测未实现的项的大小，
* 了解潜在的不连续视区偏移量和
* 应用布局更正以考虑这些倒班。

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

**代码隐藏： Main.cs**

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

**代码： VirtualizingStackLayout.cs**

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
