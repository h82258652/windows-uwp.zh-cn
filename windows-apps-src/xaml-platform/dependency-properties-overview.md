---
author: jwmsft
description: 本主题介绍了在使用 C++、C# 或 Visual Basic 编写 Windows 运行时应用以及 UI 的 XAML 定义时可用的依赖属性系统。
title: 依赖属性概述
ms.assetid: AD649E66-F71C-4DAA-9994-617C886FDA7E
ms.author: jimwalk
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 5fbf6e7ee8a224a6957428fddd11a2922adbecf4
ms.sourcegitcommit: e2fca6c79f31e521ba76f7ecf343cf8f278e6a15
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/15/2018
ms.locfileid: "6976778"
---
# <a name="dependency-properties-overview"></a>依赖属性概述

本主题介绍了在编写使用 C++、C# 或 Visual Basic 的 Windows 运行时应用并为 UI 使用 XAML 定义时可用的依赖属性系统。

## <a name="what-is-a-dependency-property"></a>什么是依赖属性？

依赖属性是一种特定类型的属性。 这种属性的特殊之处在于，其属性值受到 Windows 运行时中专用属性系统的跟踪和影响。

为了支持依赖属性，定义该属性的对象必须是一个 [**DependencyObject**](https://msdn.microsoft.com/library/windows/apps/br242356)（也就是说，一个在其继承中的某个位置具有 **DependencyObject** 基类的类）。 使用适用于 UWP 应用使用 XAML UI 定义的类型的许多**DependencyObject**子类，且会将支持依赖属性。 但是，对于任何来自 Windows 运行时命名空间的类型，如果其名称中没有“XAML”，便不支持依赖属性；这种类型的属性都是普通属性，它们不具有属性系统的依赖行为。

依赖属性的用途是提供一种系统方式，用来基于其他输入（在应用运行时其内部出现的其他属性、事件和状态）计算属性的值。 其他输入可能包括：

- 外部输入，例如用户首选项
- 即时属性确定机制，例如数据绑定、动画和故事板
- 多用途模板模式，例如资源和样式
- 通过与对象树中其他元素的父子关系知道的值

依赖属性代表或支持编程模型的某种特定功能，用于定义 Windows 运行时应用中的使用 XAML 编写 UI 和 C#、 Microsoft Visual Basic 或 VisualC + + 组件扩展 (C + + CX) 的代码。 这些功能包括：

- 数据绑定
- 样式
- 情节提要动画
- “PropertyChanged”行；一种依赖属性，实现该依赖属性可提供回调，从而将更改传播给其他依赖属性
- 使用来自属性元数据的默认值
- 一般属性系统实用工具，例如 [**ClearValue**](https://msdn.microsoft.com/library/windows/apps/br242357) 和元数据查找

## <a name="dependency-properties-and-windows-runtime-properties"></a>依赖属性和 Windows 运行时属性

依赖属性提供一种全局内部属性存储来在运行时支持应用内的所有依赖属性，从而扩展基本的 Windows 运行时属性功能。 这种方法可以替代为具有专用字段的属性（在属性定义类中为专用）提供支持的标准模式。 你可以将此内部属性存储视为任何特定对象的一组属性标识符和值（只要该对象是 [**DependencyObject**](https://msdn.microsoft.com/library/windows/apps/br242356) 即可）。 存储中的每个属性均通过 [**DependencyProperty**](https://msdn.microsoft.com/library/windows/apps/br242362) 实例（而不是通过名称）进行标识。 但是，大多数情况下，属性系统会隐藏该实现详细信息：你可以使用简单名称（你所使用的代码语言中的可编程属性名称，或是在编写 XAML 时使用的属性名称）频繁访问依赖属性。

提供依赖属性系统支持的基类型为 [**DependencyObject**](https://msdn.microsoft.com/library/windows/apps/br242356)。 **DependencyObject** 定义可访问依赖属性的方法，并且 **DependencyObject** 派生类的实例在内部支持我们之前提及的属性存储概念。

下面总结了本文在探讨依赖属性时使用的术语：

| 术语 | 说明 |
|------|-------------|
| 依赖属性 | 存在于 [**DependencyProperty**](https://msdn.microsoft.com/library/windows/apps/br242362) 标识符上的一个属性（如下所示）。 通常该标识符可用作定义 **DependencyObject** 派生类的一个静态成员。 |
| 依赖属性标识符 | 用于标识属性的常量值，它通常公开显示且只读。 |
| 属性包装器 | Windows 运行时属性的可调用 **get** 和 **set** 实现。 或者原始定义的特定于语言的投影。 **get** 属性包装器实现调用 [**GetValue**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.dependencyobject.getvalue.aspx)，传递相关的依赖属性标识符。 |

属性包装器不仅给调用方带来了方便，它还向任何为属性使用 Windows 运行时定义的过程、工具或投影公开该依赖属性。

下面的示例定义一个自定义“IsSpinning”依赖属性，就像 C# 中的定义一样，然后显示依赖属性标识符与属性包装器之间的关系。

```csharp
// IsSpinningProperty is the dependency property identifier
// no need for info in the last PropertyMetadata parameter, so we pass null
public static readonly DependencyProperty IsSpinningProperty =
    DependencyProperty.Register(
        "IsSpinning", typeof(Boolean),
        typeof(ExampleClass), null
    );
// The property wrapper, so that callers can use this property through a simple ExampleClassInstance.IsSpinning usage rather than requiring property system APIs
public bool IsSpinning
{
    get { return (bool)GetValue(IsSpinningProperty); }
    set { SetValue(IsSpinningProperty, value); }
}
```

> [!NOTE]
> 前面的示例不是演示如何创建自定义依赖属性的完整示例。 它旨在为希望通过代码学习概念的人说明依赖属性概念。 有关更为完整的示例，请参阅[自定义依赖属性](custom-dependency-properties.md)。

## <a name="dependency-property-value-precedence"></a>依赖属性值优先级

当获得一个依赖属性的值时，你获得的值是通过任何一个参与 Windows 运行时属性系统的输入，针对该属性确定的。 依赖属性值存在优先级，因此 Windows 运行时属性系统可通过一种可预测的方式计算值，并且你需要熟悉基本的优先级顺序，这一点很重要。 否则你可能会遇到这样的情况：当你尝试在一个优先级别设置某个属性时，某些东西（系统、第三方调用方、你自己的代码）却在另一个级别设置了该属性，你会很难明白使用了哪个属性值以及该值的原始位置。

例如，样式和模板用作建立属性值并因此建立控件外观的一个共同起点。 但在一个特定的控件实例上，你可能希望更改它的值，而不是更改通用模板化值，例如为该控件提供一种不同的背景颜色或提供一种不同的文本字符串作为内容。 Windows 运行时属性系统认为本地值的优先级比样式和模板所提供的值的优先级更高。 这使方案可以使用特定于应用的值覆盖模板，以便你可以在应用 UI 中自行使用这些控件。

### <a name="dependency-property-precedence-list"></a>依赖属性优先级列表

以下是属性系统在分配依赖属性的运行时值时采用的明确顺序。 优先级最高的会首先列出。 你将找到比此列表更加详细的解释。

1. **动画值：** 活动动画、视觉状态动画或具有 [**HoldEnd**](https://msdn.microsoft.com/library/windows/apps/br210306) 行为的动画。 若要拥有任何实用效果，则适用于属性的动画必须拥有比基础（无动画）值更高的优先级，即使该值进行了本地设置也是如此。
1. **本地值：** 本地值可通过属性包装器设置，这也等同于在 XAML 中设置为属性或属性元素，或通过使用特定实例的属性调用 [**SetValue**](https://msdn.microsoft.com/library/windows/apps/br242361) 方法来设置。 如果使用绑定或静态资源来设置本地值，优先级列表中的每个操作都认为本地值已设置，如果设置了一个新本地值，绑定或资源引用将被清除。
1. **模板属性：** 如果在某个模板（来自 [**ControlTemplate**](https://msdn.microsoft.com/library/windows/apps/br209391) 或 [**DataTemplate**](https://msdn.microsoft.com/library/windows/apps/br242348)）中创建一个元素，该元素就会拥有这些模板属性。
1. **样式设置器：** 来自页面或应用程序资源的样式内某个 [**Setter**](https://msdn.microsoft.com/library/windows/apps/br208817) 的值。
1. **默认值：** 一个依赖属性可在其元数据中包含一个默认值。

### <a name="templated-properties"></a>模板属性

模板属性作为一个优先级项，不适用于直接在 XAML 页面标记中声明的元素的任何属性。 模板属性概念只适用于 Windows 运行时将 XAML 模板应用于 UI 元素并因此而定义其视觉效果时所创建的对象。

从控件模板中设置的所有属性都具有某些类型的值。 其中的大部分值都类似于控件的一组扩展的默认值，并且通常与你可以稍后通过直接设置属性值重置的值相关联。 因此，必须将模板集值与真实的本地值区分开，以便任何新的本地值都可以覆盖它。

> [!NOTE]
> 在某些情况下，如果模板无法为本应可以在实例中设置的属性公开 [{TemplateBinding} 标记扩展](templatebinding-markup-extension.md)引用，则此模板甚至可能会替代本地值。 通常只有在确实不准备在实例中设置相应属性的情况下才会进行上述替代，例如，当属性只是与视觉效果和模板行为相关，而与使用模板的控件的预期功能或运行时逻辑不相关时。

### <a name="bindings-and-precedence"></a>绑定和优先级

绑定操作对于任何适用范围都具有相应的优先级。 例如，对于应用于本地值的 [{Binding}](binding-markup-extension.md)，其作用相当于本地值；对于属性设置器应用的 [{TemplateBinding} 标记扩展](templatebinding-markup-extension.md)，其作用相当于样式设置器。 由于绑定必须一直等到运行时才能获取来自数据源的值，因此确定任何属性的属性值优先级这一过程也会延迟到运行时。

绑定不仅具有与本地值相同的优先级，而且它们确实是本地值，区别在于绑定是延迟值的占位符。 如果一个属性值具有相应的绑定，则可在运行时为其设置一个本地值，用于完整替换该绑定。 同样地，如果你通过调用 [**SetBinding**](https://msdn.microsoft.com/library/windows/apps/br244257) 来定义只在运行时才存在的绑定，则会替换可能在 XAML 中应用过的所有本地值或之前执行的代码。

### <a name="storyboarded-animations-and-base-value"></a>情节提要动画和基值

情节提要动画要根据*基值*的概念来执行。 基值是由属性系统使用其优先级确定的值，在确定过程中会省略查找动画的最后步骤。 例如，一个基值可能来自控件模板，也可能来自在某个控件实例中设置的本地值。 无论是上述哪种情况，只要你的动画继续运行，应用此动画便会覆盖此基值并应用动画值。

对于一个动画属性，如果该动画没有显式指定 **From** 和 **To**，或如果动画在完成时将属性恢复到基值，那么基值仍然会影响动画的行为。 在这些情况下，一旦动画停止运行，便会再次使用余下的优先级。

但是，如果动画为 [**HoldEnd**](https://msdn.microsoft.com/library/windows/apps/br210306) 行为指定了 **To**，则在动画被删除以前，它可以一直替代本地值，甚至当动画在视觉上看似已经停止时也是如此。 从概念上来说，这类似于永久运行的动画，即使在 UI 中没有可视动画时也是如此。

可将多个动画应用于一个属性。 这其中每个动画都可能经过定义，以替换来自值优先级中不同点的基值。 但是，在运行时这些动画将同时运行，这通常意味着动画必须将它们的值组合起来，因为每个动画对值都具有同等的影响。 这取决于定义动画的方式和动画值的类型。

有关详细信息，请参阅[情节提要动画](https://msdn.microsoft.com/library/windows/apps/mt187354)。

### <a name="default-values"></a>默认值

有关使用 [**PropertyMetadata**](https://msdn.microsoft.com/library/windows/apps/br208771) 值为依赖属性创建默认值的详细信息，请参阅[自定义依赖属性](custom-dependency-properties.md)主题。

即使并未在依赖属性的元数据中显式定义默认值，依赖属性仍然具有这些默认值。 除非 Windows 运行时依赖属性的默认值由元数据进行更改，否则它们通常都是以下属性之一：

- 使用运行时对象或基本 **Object** 类型（一种*引用类型*）的属性的默认值为 **null**。 例如，在特意设置 [**DataContext**](https://msdn.microsoft.com/library/windows/apps/br208713) 或被继承之前，它的值一直会是 **null**。
- 使用数字或布尔值（一种*值类型*）等基值的属性会使用预期的默认值作为该值。 例如，使用 0 作为整数和浮点数，使用 **false** 作为布尔值。
- 使用 Windows 运行时结构的属性通过调用该结构的隐式默认构造函数获得默认值。 该构造函数会在结构中的每一个基值字段中使用默认值。 例如，[**Point**](https://msdn.microsoft.com/library/windows/apps/br225870) 值的默认值会通过其 **X** 和 **Y** 值初始化为 0。
- 使用枚举的属性以该枚举中首个定义成员的值为默认值。 检查特定枚举的引用，以查明默认值。
- 使用字符串的属性（用于 .NET 的 [**System.String**](https://msdn.microsoft.com/library/windows/apps/xaml/system.string.aspx)，用于 C++/CX 的 [**Platform::String**](https://msdn.microsoft.com/library/windows/apps/xaml/hh755812.aspx)）以空字符串 (**""**) 为默认值。
- 集合属性通常不会实现为依赖属性，本主题进一步讨论了其原因。 但是如果你实现了自定义集合属性，并且你希望使其成为依赖属性，请避免*意外的 singleton*，如[自定义依赖属性](custom-dependency-properties.md)结尾部分所述。

## <a name="property-functionality-provided-by-a-dependency-property"></a>依赖属性提供的属性功能

### <a name="data-binding"></a>数据绑定

依赖属性可使它的值通过应用数据绑定进行设置。 数据绑定使用 XAML 中的 [{Binding} 标记扩展](binding-markup-extension.md)语法、[{x:Bind} 标记扩展](x-bind-markup-extension.md)或代码中的 [**Binding**](https://msdn.microsoft.com/library/windows/apps/br209820) 类。 对于数据绑定属性，其属性值的最终确定要延迟到运行时。 届时，将从数据源中获取值。 依赖属性系统在这里所起到的作用是在值还未知时为诸如加载 XAML 等操作启用占位符行为，然后在运行时通过与 Windows 运行时数据绑定引擎交互来提供值。

以下示例使用 XAML 中的绑定设置 [**TextBlock**](https://msdn.microsoft.com/library/windows/apps/br209652) 元素的 [**Text**](https://msdn.microsoft.com/library/windows/apps/br209676) 值。 该绑定使用继承的数据上下文和对象数据源。 （这个简短示例中没有展示这些方面；有关展示上下文和来源的更完整示例，请参阅[深入了解数据绑定](https://msdn.microsoft.com/library/windows/apps/mt210946)。）

```xaml
<Canvas>
  <TextBlock Text="{Binding Team.TeamName}"/>
</Canvas>
```

你也可以使用代码（而不是 XAML）来建立绑定。 请参阅 [**SetBinding**](https://msdn.microsoft.com/library/windows/apps/br244257)。

> [!NOTE]
> 为了考虑依赖属性值的优先级，会将类似的绑定视为本地值。 如果你为最初存放 [**Binding**](https://msdn.microsoft.com/library/windows/apps/br209820) 值的属性设置其他本地值，则将完全改写绑定，而不只是改写绑定的运行时值。 {x:Bind} 绑定使用生成的代码（将为该属性设置本地值）实现。 如果为使用 {x:Bind} 的属性设置本地值，将在下次评估绑定时替换该值，例如在其源对象上观察属性更改时。

### <a name="binding-sources-binding-targets-the-role-of-frameworkelement"></a>绑定源、绑定目标、FrameworkElement 的角色

要作为绑定的来源，属性不需要是依赖属性；一般可以使用任何属性作为绑定源，不过这取决于你的编程语言，而且每个属性都具有特定的边缘方案。 但是，要作为 [{Binding} 标记扩展](binding-markup-extension.md)或 [**Binding**](https://msdn.microsoft.com/library/windows/apps/br209820) 的目标，该属性必须是依赖属性。 {x:Bind} 没有此项要求，因为它使用生成的代码应用其绑定值。

如果在代码中创建绑定，请注意 [**SetBinding**](https://msdn.microsoft.com/library/windows/apps/br244257) API 仅为 [**FrameworkElement**](https://msdn.microsoft.com/library/windows/apps/br208706) 定义。 但是，也可以使用 [**BindingOperations**](https://msdn.microsoft.com/library/windows/apps/br209823) 创建绑定定义，从而引用任何 [**DependencyObject**](https://msdn.microsoft.com/library/windows/apps/br242356) 属性。

对于代码或 XAML，请记住 [**DataContext**](https://msdn.microsoft.com/library/windows/apps/br208713) 是一个 [**FrameworkElement**](https://msdn.microsoft.com/library/windows/apps/br208706) 属性。 通过使用一种父子属性继承的形式（通常在 XAML 标记中建立），绑定系统可解析父元素上存在的 **DataContext**。 即使子对象（具有目标属性）不是 **FrameworkElement** 并且因此没有自身的 **DataContext** 值，此继承也可以进行评估。 但是，所继承的父元素必须是一个 **FrameworkElement**，才能设置和具有 **DataContext**。 否则，你必须定义绑定，这样它才可以使用 **DataContext** 的 **null** 值。

对于大部分数据绑定方案，连接绑定并不是唯一需要的。 要让单向或双向绑定生效，来源属性必须支持能够传播到绑定系统并进而传播到目标的更改通知。 对于自定义绑定源，这意味着该属性必须是依赖属性，或者该对象必须支持 [**INotifyPropertyChanged**](https://msdn.microsoft.com/library/windows/apps/xaml/system.componentmodel.inotifypropertychanged.aspx)。 集合应支持 [**INotifyCollectionChanged**](https://msdn.microsoft.com/library/windows/apps/xaml/system.collections.specialized.inotifycollectionchanged.aspx)。 某些类在其实现中支持这些接口，以便它们可在数据绑定方案中用作基类；这种类的一个示例是 [**ObservableCollection&lt;T&gt;**](https://msdn.microsoft.com/library/windows/apps/xaml/ms668604.aspx)。 有关数据绑定和数据绑定与属性系统之间关系的详细信息，请参阅[深入了解数据绑定](https://msdn.microsoft.com/library/windows/apps/mt210946)。

> [!NOTE]
> 此处所列的类型支持 Microsoft .NET 数据源。 C++/CX 数据源可针对更改通知或可观察行为使用不同的接口，请参阅[深入了解数据绑定](https://msdn.microsoft.com/library/windows/apps/mt210946)。

### <a name="styles-and-templates"></a>样式和模板

样式和模板是两种将属性定义为依赖属性的场景。 样式对设置可定义应用 UI 的属性非常有用。 样式在 XAML 中定义为资源，作为 [**Resources**](https://msdn.microsoft.com/library/windows/apps/br208740) 集合中的一个条目，或者 XAML 文件（如主题资源字典）中的一个条目。 样式与属性系统交互，因为它们包含属性的资源库。 此处最重要的属性是 [**Control**](https://msdn.microsoft.com/library/windows/apps/br209390) 的 [**Control.Template**](https://msdn.microsoft.com/library/windows/apps/br209465) 属性：它定义 **Control** 的大部分可视外观和视觉状态。 有关样式和定义一个 [**Style**](https://msdn.microsoft.com/library/windows/apps/br208849) 并使用资源库的某些示例 XAML 的详细信息，请参阅[设置控件样式](https://msdn.microsoft.com/library/windows/apps/mt210950)。

来自样式或模板的值是延迟值，类似于绑定。 这样，控件用户可以重新创建控件模板或重新定义样式。 正因如此，样式中的属性设置器才只根据依赖属性（而非普通属性）来执行。

### <a name="storyboarded-animations"></a>情节提要动画

你可以使用情节提要动画对依赖属性的值进行动画处理。 Windows 运行时中的情节提要动画不仅仅是视觉装饰。 更有用的做法是将动画视为一种状态机技术，你可以通过该技术设置单个属性或所有属性的值以及控件的视觉效果，并可在日后更改这些值。

若要创建动画，动画的目标属性必须是一个依赖属性。 此外，若要创建动画，目标属性的值类型必须受现有 [**Timeline**](https://msdn.microsoft.com/library/windows/apps/br210517) 派生的动画类型之一支持。 [**Color**](https://msdn.microsoft.com/library/windows/apps/hh673723)、[**Double**](https://msdn.microsoft.com/library/windows/apps/system.double.aspx) 和 [**Point**](https://msdn.microsoft.com/library/windows/apps/br225870) 的值可使用内插或关键帧技术实现动画效果。 大部分其他值都可以使用离散式 **Object** 关键帧实现动画效果。

当应用并运行一个动画时，动画值操作的优先级比该属性使用的任何其他值（例如本地值）更高。 动画还有一个可选的 [**HoldEnd**](https://msdn.microsoft.com/library/windows/apps/br210306) 行为，该行为可能导致动画应用于属性值，即使动画在视觉上已停止也是如此。

状态机原则可通过在控件的 [**VisualStateManager**](https://msdn.microsoft.com/library/windows/apps/br209021) 状态模型中使用情节提要动画来体现。 有关情节提要动画的详细信息，请参阅[情节提要动画](https://msdn.microsoft.com/library/windows/apps/mt187354)。 有关 **VisualStateManager** 和定义控件视觉状态的详细信息，请参阅[视觉状态的情节提要动画](https://msdn.microsoft.com/library/windows/apps/xaml/jj819808)或[控件模板](../design/controls-and-patterns/control-templates.md)。

### <a name="property-changed-behavior"></a>属性已更改行为

属性已更改行为是依赖属性术语中“依赖”部分的一个主要原因。 在另一个属性可以影响第一个属性值的情形下，维护一个属性的有效值是许多框架中一个很难的开发问题。 在 Windows 运行时属性系统中，每个依赖属性可指定一个回调，只要它的属性值更改，就会调用该回调。 此回调可用于通知或更改相关的属性值（通常采用一种同步方式）。 许多现有的依赖属性有一个属性已更改行为。 也可以向自定义依赖属性添加类似的回调行为，实现你自己的属性已更改回调。 有关示例，请参阅[自定义依赖属性](custom-dependency-properties.md)。

Windows 10 引入了 [**RegisterPropertyChangedCallback**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.dependencyobject.registerpropertychangedcallback.aspx) 方法。 这使应用程序代码可以注册更改通知（如果指定的依赖属性在 [**DependencyObject**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.dependencyobject.aspx) 的实例上发生更改）。

### <a name="default-value-and-clearvalue"></a>默认值和 **ClearValue**

一个依赖属性可在其属性元数据中定义一个默认值。 对于依赖属性而言，在首次对该属性进行设置之后，其默认值并不会完全失效。 只要值优先级中其他某个决定因素消失，就可以在运行时再次应用默认值。 （依赖属性值优先级将在下一节中讨论。）例如，你可能特意删除适用于属性的样式值或动画，但希望在执行此操作后有用作合理默认值的值。 依赖属性默认值可以提供此值，无需将专门设置每个属性的值作为额外步骤。

即使已使用本地值设置某个属性，你仍可以特意将其设置为默认值。 若要再次将属性值重置为默认值，并且启用优先级中其他可能会替代默认值（而非本地值）的其他参与者，可以调用 [**ClearValue**](https://msdn.microsoft.com/library/windows/apps/br242357) 方法（引用该属性以作为方法参数清除）。 有时你可能并不希望属性固定使用默认值，但是清除本地值并还原为默认值可能会启用优先级中需要立即执行的其他项目，例如使用来自控制模板中样式资源库的值。

## <a name="dependencyobject-and-threading"></a>**DependencyObject** 和线程处理

所有 [**DependencyObject**](https://msdn.microsoft.com/library/windows/apps/br242356) 实例都必须在与 Windows 运行时应用所显示的当前 [**Window**](https://msdn.microsoft.com/library/windows/apps/br209041) 相关联的 UI 线程上创建。 虽然每个 **DependencyObject** 都必须在主 UI 线程上创建，但可以通过访问 [**Dispatcher**](https://msdn.microsoft.com/library/windows/apps/br230616) 属性从其他线程使用调度程序引用来访问这些对象。 然后，你可以在 [**CoreDispatcher**](https://msdn.microsoft.com/library/windows/apps/br208211) 对象上调用诸如 [**RunAsync**](https://msdn.microsoft.com/library/windows/apps/hh750317) 的方法，并在 UI 线程上遵循线程限制规则执行你的代码。

[**DependencyObject**](https://msdn.microsoft.com/library/windows/apps/br242356) 的线程处理特性很重要，因为这通常意味着只有那些在 UI 线程上运行的代码才能更改甚至读取依赖属性的值。 在正确使用 **async** 模式和后台工作线程的典型 UI 代码中，通常可以避免线程处理问题。 通常，如果你定义自己的 **DependencyObject** 类型并尝试将这些类型用于 **DependencyObject** 未必适宜的数据源或其他场方案，只会遇到与 **DependencyObject** 相关的线程处理问题。

## <a name="related-topics"></a>相关主题

### <a name="conceptual-material"></a>概念材料

- [自定义依赖属性](custom-dependency-properties.md)
- [附加属性概述](attached-properties-overview.md)
- [深入了解数据绑定](https://msdn.microsoft.com/library/windows/apps/mt210946)
- [情节提要动画](https://msdn.microsoft.com/library/windows/apps/mt187354)
- [创建 Windows 运行时组件](https://msdn.microsoft.com/library/windows/apps/xaml/hh441572.aspx)
- [XAML 用户和自定义控件示例](http://go.microsoft.com/fwlink/p/?linkid=238581)

## <a name="apis-related-to-dependency-properties"></a>与依赖属性相关的 API

- [**DependencyObject**](https://msdn.microsoft.com/library/windows/apps/br242356)
- [**DependencyProperty**](https://msdn.microsoft.com/library/windows/apps/br242362)

