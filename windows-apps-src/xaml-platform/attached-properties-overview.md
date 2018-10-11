---
author: jwmsft
description: 介绍 XAML 中的附加属性概念，并提供一些示例。
title: 附加属性概述
ms.assetid: 098C1DE0-D640-48B1-9961-D0ADF33266E2
ms.author: jimwalk
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, uwp
ms.localizationpriority: medium
dev_langs:
- csharp
- vb
- cpp
ms.openlocfilehash: 7f92b12ab9c8962fe98d8eed22b21e7d10330c99
ms.sourcegitcommit: 933e71a31989f8063b020746fdd16e9da94a44c4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/11/2018
ms.locfileid: "4531722"
---
# <a name="attached-properties-overview"></a>附加属性概述

*附加属性*是一种 XAML 概念。 使用附加属性，可以在对象上设置其他属性/值对，但这些属性并不是原始对象定义的组成部分。 附加属性通常定义为一种专门形式的依赖属性，在所有者类型的对象模型中没有传统的属性包装器。

## <a name="prerequisites"></a>先决条件

我们假设你理解依赖属性的基本概念，并且已阅读[依赖属性概述](dependency-properties-overview.md)。

## <a name="attached-properties-in-xaml"></a>XAML 中的附加属性

在 XAML 中，可使用语法 _AttachedPropertyProvider.PropertyName_ 设置附加属性。 以下是如何在 XAML 中设置 [**Canvas.Left**](https://msdn.microsoft.com/library/windows/apps/hh759771) 的一个示例。

```xaml
<Canvas>
  <Button Canvas.Left="50">Hello</Button>
</Canvas>
```

> [!NOTE]
> 我们只需将用作示例附加属性使用[**Canvas.Left**](https://msdn.microsoft.com/library/windows/apps/hh759771) ，而不完全介绍使用它的原因。 如果你希望了解有关 **Canvas.Left** 的目的以及 [**Canvas**](https://msdn.microsoft.com/library/windows/apps/br209267) 如何处理其布局子项的详细信息，请参阅 [**Canvas**](https://msdn.microsoft.com/library/windows/apps/br209267) 参考主题或[使用 XAML 定义布局](https://msdn.microsoft.com/library/windows/apps/mt228350)。

## <a name="why-use-attached-properties"></a>为什么使用附加属性？

使用附加属性，可以避开可能会防止一个关系中的不同对象在运行时相互传递信息的编码约定。 一定可以针对常见的基类设置属性，以便每个对象只需获取和设置该属性即可。 但是，你可能希望在很多情况下这样做，这会使你的基类最终充斥着大量可共享的属性。 它甚至可能会引入以下情况：在数百个后代中，只有两个后代尝试使用一个属性。 这样的类设计很糟糕。 为了解决此问题，我们使用附加属性概念来允许对象为不是由它自己的类结构定义的属性赋值。 在对象树中完成创建各个对象之后，定义类可以在运行时从子对象中读取此值。

例如，子元素可使用附加属性通知父元素它们如何在 UI 中显示。 [**Canvas.Left**](https://msdn.microsoft.com/library/windows/apps/hh759771) 附加属性就属于此情况。 **Canvas.Left** 创建为一个附加属性，因为它在 [**Canvas**](https://msdn.microsoft.com/library/windows/apps/br209267) 元素内包含的元素上设置，而不是在 **Canvas** 本身上设置。 然后，任何可能的子元素使用 **Canvas.Left** 和 [**Canvas.Top**](https://msdn.microsoft.com/library/windows/apps/hh759772) 在 **Canvas** 布局容器父元素中指定它的布局偏移。 附加属性使这一场景的实现成为可能，而无需将基础元素的对象模型与大量属性聚集在一起，并且每个属性仅应用于许多可能的布局容器中的一种。 相反，许多布局容器实现它们自己的附加属性集。

为了实现附加属性，[**Canvas**](https://msdn.microsoft.com/library/windows/apps/br209267) 类定义一个名为 [**Canvas.LeftProperty**](https://msdn.microsoft.com/library/windows/apps/br209272) 的静态 [**DependencyProperty**](https://msdn.microsoft.com/library/windows/apps/br242362) 字段。 然后，**Canvas** 提供 [**SetLeft**](https://msdn.microsoft.com/library/windows/apps/br209273) 和 [**GetLeft**](https://msdn.microsoft.com/library/windows/apps/br209269) 方法作为附加属性的公共访问器，以同时支持 XAML 设置和运行时值访问。 对于 XAML 和依赖属性系统，这组 API 实现了一种模式，支持为附加属性使用特定的 XAML 语法并将值存储在依赖属性存储中。

## <a name="how-the-owning-type-uses-attached-properties"></a>拥有类型如何使用附加属性

尽管附加属性可在任何 XAML 元素（或任何基础 [**DependencyObject**](https://msdn.microsoft.com/library/windows/apps/br242356)）上设置，但这不会自动表明设置该属性将生成实际的结果，或者该值将会被访问。 定义附加属性的类型通常采用下列方案之一：

- 用来定义附加属性的类型在与其他对象的关系中作为父对象。 子对象将为附加属性设置值。 附加属性的所有者类型具有一些固有的行为，该行为循环访问附加属性的子元素、获取它们的值并在对象生存期的某个时间点对这些值执行操作（布局操作、[**SizeChanged**](https://msdn.microsoft.com/library/windows/apps/br208742) 等）。
- 定义附加属性的类型用作各种可能的父元素和内容模型的子元素，但是此信息不一定是布局信息。
- 附加属性向一个服务（而不是另一个 UI 元素）报告信息。

有关这些方案和拥有类型的详细信息，请参阅[自定义的附加属性](custom-attached-properties.md)的“有关 Canvas.Left 的详细信息”部分。

## <a name="attached-properties-in-code"></a>代码中的附加属性

与其他依赖属性不同，附加属性没有典型的属性包装器用于简化获取和设置访问。 这是因为附加属性不一定是设置属性的实例的以代码为中心的对象模型。 （允许[但不常用]定义这样一个属性，它既是其他类型可在自身上设置的附加属性，也在拥有类型上有一种方便的属性用法。）

有两种在代码中设置附加属性的方式：使用属性系统 API 或使用 XAML 模式访问器。 这些技术的最终结果大体相同，所以决定使用哪种技术主要在于编码风格。

### <a name="using-the-property-system"></a>使用属性系统

Windows 运行时的附加属性实现为依赖属性，以便这些值可以由属性系统存储在共享依赖属性存储中。 因此附加属性在拥有类上公开一个依赖属性标识符。

若要在代码中设置附加属性，你可以调用 [**SetValue**](https://msdn.microsoft.com/library/windows/apps/br242361) 方法，传递用作该附加属性标识符的 [**DependencyProperty**](https://msdn.microsoft.com/library/windows/apps/br242362) 字段。 （还需传递要设置的值。）

若要在代码中获得附加属性的值，你可以调用 [**GetValue**](https://msdn.microsoft.com/library/windows/apps/br242359) 方法，再次传递用作标识符的 [**DependencyProperty**](https://msdn.microsoft.com/library/windows/apps/br242362) 字段。

### <a name="using-the-xaml-accessor-pattern"></a>使用 XAML 访问器模式

XAML 处理器必须能够在将 XAML 分析为对象树时设置附加属性值。 附加属性的所有者类型必须实现名为表单中的专用的访问器方法 **获取 *** PropertyName*和 **设置 *** PropertyName*。 这些专用的访问器方法也是一种在代码中获取或设置附加属性的方式。 从代码角度讲，附加属性类似于拥有方法访问器而不是属性访问器的支持字段，并且该支持字段可存在于任何对象上，而不需要专门定义。

下面的示例展示了如何通过 XAML 访问器 API 在代码中设置附加属性。 在此示例中，`myCheckBox` 是 [**CheckBox**](https://msdn.microsoft.com/library/windows/apps/br209316) 类的一个实例。 最后一行是实际设置值的代码，该行之前的行只是建立实例及其父子关系。 未注释掉的最后一行是使用属性系统的语法。 注释掉的最后一行是使用 XAML 访问器模式的语法。

```csharp
    Canvas myC = new Canvas();
    CheckBox myCheckBox = new CheckBox();
    myCheckBox.Content = "Hello";
    myC.Children.Add(myCheckBox);
    myCheckBox.SetValue(Canvas.TopProperty,75);
    //Canvas.SetTop(myCheckBox, 75);
```

```vb
    Dim myC As Canvas = New Canvas()
    Dim myCheckBox As CheckBox= New CheckBox()
    myCheckBox.Content = "Hello"
    myC.Children.Add(myCheckBox)
    myCheckBox.SetValue(Canvas.TopProperty,75)
    ' Canvas.SetTop(myCheckBox, 75)
```

```cppwinrt
Canvas myC;
CheckBox myCheckBox;
myCheckBox.Content(winrt::box_value(L"Hello"));
myC.Children().Append(myCheckBox);
myCheckBox.SetValue(Canvas::TopProperty(), winrt::box_value(75));
// Canvas::SetTop(myCheckBox, 75);
```

```cpp
    Canvas^ myC = ref new Canvas();
    CheckBox^ myCheckBox = ref new CheckBox();
    myCheckBox->Content="Hello";
    myC->Children->Append(myCheckBox);
    myCheckBox->SetValue(Canvas::TopProperty,75);
    // Canvas::SetTop(myCheckBox, 75);
```

## <a name="custom-attached-properties"></a>自定义附加属性

有关定义自定义附加属性的代码示例，以及有关使用附加属性的场景的详细信息，请参阅[自定义附加属性](custom-attached-properties.md)。

## <a name="special-syntax-for-attached-property-references"></a>附加属性引用的特殊语法

附加属性名称中的点是标识模式的一个关键部分。 在某种语法或情况认为点拥有其他某种含义时，就会导致多义性。 例如，一个点可视为对绑定路径的对象模型遍历。 在大部分涉及这种歧义性的情况下，附加属性有一种特殊语法，使内部点仍被分析为附加属性的 _owner_**.**_property_ 分隔符。

- 若要将一个附加属性指定为动画目标路径的一部分，可以将附加属性名称放在圆括号（“()”）中，例如“(Canvas.Left)”。 有关详细信息，请参阅 [Property-path 语法](property-path-syntax.md)。

> [!WARNING]
> Windows 运行时 XAML 实现的一个现有限制是，你无法动画处理自定义附加属性。

- 若要将附加属性指定为从一个资源文件到 **x:Uid** 的资源引用的目标属性，可以使用一种特殊语法，即将代码样式的完全限定的 **using:** 声明放在方括号（“\[\]”）内，以创建一种专门的领域分隔效果。 例如，假设存在一个元素`<TextBlock x:Uid="Title" />`，该实例上的**Canvas.Top**值为目标的资源文件中的资源键是"Title.\[using:Windows.UI.Xaml.Controls\]Canvas.Top"。 有关资源文件和 XAML 的详细信息，请参阅[快速入门：翻译 UI 资源](https://msdn.microsoft.com/library/windows/apps/xaml/hh965329)。

## <a name="related-topics"></a>相关主题

- [自定义附加属性](custom-attached-properties.md)
- [依赖属性概述](dependency-properties-overview.md)
- [使用 XAML 定义布局](https://msdn.microsoft.com/library/windows/apps/mt228350)
- [快速入门：翻译 UI 资源](https://msdn.microsoft.com/library/windows/apps/hh943060)
- [**SetValue**](https://msdn.microsoft.com/library/windows/apps/br242361)
- [**GetValue**](https://msdn.microsoft.com/library/windows/apps/br242359)
