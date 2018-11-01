---
author: jwmsft
description: 可以使用 PropertyPath 类和字符串语法来实例化 XAML 或代码中的 PropertyPath 值。
title: Property-path 语法
ms.assetid: FF3ECF47-D81F-46E3-BE01-C839E0398025
ms.author: jimwalk
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: a94782165027c2194f677dfdbb9f2dec11541080
ms.sourcegitcommit: cd00bb829306871e5103db481cf224ea7fb613f0
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/31/2018
ms.locfileid: "5876999"
---
# <a name="property-path-syntax"></a>Property-path 语法


可以使用 [**PropertyPath**](https://msdn.microsoft.com/library/windows/apps/br244259) 类和字符串语法来实例化 XAML 或代码中的 **PropertyPath** 值。 **PropertyPath** 值由数据绑定使用。 目标情节提要动画使用相似的语法。 对于这两种情形，都由属性路径来描述最终解析为单个属性的一个或多个对象-属性关系的遍历。

可将属性路径字符串直接设置为 XAML 中的属性。 可使用相同的字符串语法在代码中构造设置 [**Binding**](https://msdn.microsoft.com/library/windows/apps/br209820) 的 [**PropertyPath**](https://msdn.microsoft.com/library/windows/apps/br244259)，或者使用 [**SetTargetProperty**](https://msdn.microsoft.com/library/windows/apps/br210503) 在代码中设置动画目标。 Windows 运行时中有两个不同的使用属性路径的功能区域：数据绑定和动画目标。 动画目标不在 Windows 运行时实现中创建基础 Property-path 语法值，它将此信息保留为字符串，但对象-属性遍历的概念非常相似。 数据绑定和动画目标各自计算属性路径的方式略有不同，因此我们分别描述它们的属性路径语法。

## <a name="property-path-for-objects-in-data-binding"></a>数据绑定中的对象的属性路径

在 Windows 运行时中，可以绑定到任何依赖属性的目标值。 数据绑定的源属性值不必是依赖属性；它可以是业务对象（例如使用 Microsoft .NET 语言或 C++ 编写的类）上的属性。 或者说，绑定值的源对象可以是已由应用定义的现有依赖对象。 源可由简单的属性名引用，也可由业务对象的对象图中的对象-属性关系的遍历引用。

可以绑定到单个的属性值，也可以绑定到包含列表或集合的目标属性。 如果源是集合，或者路径指定了集合属性，则数据绑定引擎将源的集合项与绑定目标匹配，从而导致一些行为，例如使用来自数据源集合的项的列表来填充 [**ListBox**](https://msdn.microsoft.com/library/windows/apps/br242868)，而无需预期该集合中的特定项。

### <a name="traversing-an-object-graph"></a>遍历对象图

指示对象图中对象-属性关系的遍历的语法元素是点 (**.**) 字符。 属性路径字符串中的每个点指示对象（点左侧）与该对象的属性（点右侧）的分界。 字符串按从左到右的顺序计算，这样可以逐一遍历多个对象-属性关系。 我们来看个示例：

``` syntax
"{Binding Path=Customer.Address.StreetAddress1}"
```

以下是评估此路径的方法：

1.  为名为“Customer”的属性搜索数据上下文对象（或由相同的 [**Binding**](https://msdn.microsoft.com/library/windows/apps/br209820) 指定的 [**Source**](https://msdn.microsoft.com/library/windows/apps/br209832)）。
2.  为名为“Address”的属性搜索作为“Customer”属性的值的对象。
3.  为名为“StreetAddress1”的属性搜索作为“Address”属性的值的对象。

在上述每一步中，值都视为对象。 仅当绑定应用于特定属性时，才检查结果的类型。 如果“Address”是一个未提供字符串的哪个部分是街道地址的字符串值，此示例就会失败。 通常，绑定指向具有缜密的已知信息结构的业务对象的特定嵌套属性值。

### <a name="rules-for-the-properties-in-a-data-binding-property-path"></a>数据绑定属性路径中的属性所遵守的规则

-   由属性路径引用的所有属性在源业务对象中都必须是公开的。
-   结束属性（路径中作为最后一个命名属性的属性）必须是公开的，而且必须是可变的 — 无法绑定到静态值。
-   如果此路径用作双向绑定的 [**Path**](https://msdn.microsoft.com/library/windows/apps/br209830) 信息，则结束属性必须是可读取/写入的。

### <a name="indexers"></a>索引器

数据绑定的属性路径可以包括对编制了索引的属性的引用。 这样便可绑定到已排序的列表/矢量，或绑定到字典/地图。 使用方括号“\[\]”字符指示编制了索引的属性。 位于这些括号中的内容可以是整数（对于排序列表），也可以是不加引号的字符串（对于字典）。 还可以绑定到键为整数的字典。 可以在同一路径中使用不同的编制了索引的属性，使用点分隔对象-属性。

例如，考虑一个业务对象，它有一个“Teams”的列表（排序列表），每个队有一本名为“Players”的字典，每个队员使用姓氏作为键。 指向二队的一个特定队员的示例属性路径为：“Teams[1].Players[Smith]”。 （使用 1 来指示“Teams”中的第二个项，因为该列表的索引是从零开始编制的。）

**注意**的 c + + 数据源的索引支持受到限制;请参阅[深入了解数据绑定](https://msdn.microsoft.com/library/windows/apps/mt210946)。

### <a name="attached-properties"></a>附加属性

属性路径可以包括对附加属性的引用。 因为附加属性的识别名称中已包括点，所以你必须将任何附加属性名称括在括号内，以便不会将点视为对象-属性的分隔符。 例如，用于指定你希望使用 [**Canvas.ZIndex**](https://msdn.microsoft.com/library/windows/apps/hh759773) 作为绑定路径的字符串为“(Canvas.ZIndex)”。 有关附加属性的详细信息，请参阅[附加属性概述](attached-properties-overview.md)。

### <a name="combining-property-path-syntax"></a>合并属性路径语法

可将属性路径语法的不同元素合并到一个字符串中。 例如，如果你的数据源具有编制了索引的附加属性，则你可以定义引用该属性的属性路径。

### <a name="debugging-a-binding-property-path"></a>调试绑定属性路径

因为属性路径由绑定引擎解释，并依赖于可能仅在运行时间才会呈现的信息，所以对于不能依靠开发工具中传统的设计时或编译时支持的绑定，你必须经常调试属性路径。 在许多情况下，无法解析属性路径的运行时结果是空值，但并不报错，因为这是绑定解决方案的由设计决定的回滚行为。 幸运的是，Microsoft Visual Studio 提供了调试输出模式，该模式可将指定绑定源解析失败的属性路径部分隔离。 有关使用此开发工具功能的详细信息，请参阅[“深入了解数据绑定”的“调试”部分](../data-binding/data-binding-in-depth.md#debugging)。

## <a name="property-path-for-animation-targeting"></a>动画目标的属性路径

动画依赖于选择在动画运行时便应用情节提要值的依赖属性作为目标。 为了标识存在待进行动画处理的属性的对象，动画按名称（[x:Name 属性](x-name-attribute.md)）选择元素作为目标。 通常需要定义以标识为 [**Storyboard.TargetName**](https://msdn.microsoft.com/library/windows/apps/hh759823) 的对象开始、以应该应用动画的特殊依赖属性值结束的属性路径。 属性路径用作 [**Storyboard.TargetProperty**](https://msdn.microsoft.com/library/windows/apps/hh759824) 的值。

有关如何在 XAML 中定义动画的详细信息，请参阅[情节提要动画](https://msdn.microsoft.com/library/windows/apps/mt187354)。

## <a name="simple-targeting"></a>简单目标处理

如果要对在作为目标的对象本身上存在的属性进行动画处理，且该属性的类型可以具有直接应用到属性（而不是应用到属性值的子属性）的动画，则只命名要进行动画处理的属性，无需进行进一步限定。 例如，如果要将 [**Rectangle**](/uwp/api/Windows.UI.Xaml.Shapes.Rectangle) 等 [**Shape**](/uwp/api/Windows.UI.Xaml.Shapes.Shape) 子类作为目标，并将经过动画处理的 [**Color**](https://msdn.microsoft.com/library/windows/apps/hh673723) 应用于 [**Fill**](/uwp/api/Windows.UI.Xaml.Shapes.Shape.Fill) 属性，则属性路径可以是“Fill”。

## <a name="indirect-property-targeting"></a>间接属性目标处理

可对作为目标对象的子属性的属性进行动画处理。 也就是说，如果存在的目标对象属性本身是一个对象，且该对象具有属性，则必须定义一个属性路径来解释如何逐一遍历该对象-属性关系。 无论何时，只要你指定希望对子属性进行动画处理的对象，就需要将属性名括在括号内，并以 *typename*.*propertyname* 格式指定该属性。 例如，要指定你需要目标对象的 [**RenderTransform**](https://msdn.microsoft.com/library/windows/apps/br208980) 属性的对象值，你就要指定“(UIElement.RenderTransform)”作为属性路径中的第一步。 这还不是一个完整的路径，因为没有可以直接应用到 [**Transform**](https://msdn.microsoft.com/library/windows/apps/br243006) 值的动画。 因此，对于此示例，现在将该属性路径完整化，以使结束属性作为可由 **Double** 值“(UIElement.RenderTransform).(CompositeTransform.TranslateX)”进行动画处理的 **Transform** 子类的属性。

## <a name="specifying-a-particular-child-in-a-collection"></a>指定集合中的特定子项

若要指定集合属性中的子项，可以使用数值索引器。 使用方括号“\[\]”字符将整数索引值括起来。 可以只引用排序列表，不引用字典。 因为集合不是可进行动画处理的值，所以使用索引器时绝不能将索引器作为属性路径中的结束属性。

例如，若要指定你希望对应用到控件的 [**Background**](https://msdn.microsoft.com/library/windows/apps/br209395) 属性的 [**LinearGradientBrush**](https://msdn.microsoft.com/library/windows/apps/br210108) 中的第一个彩色停止颜色进行动画处理，属性路径可以是：“(Control.Background).(GradientBrush.GradientStops)[0].(GradientStop.Color)”。 注意如何实现不将索引器作为路径中的最后一步，尤其要注意最后一步必须引用集合中项 0 的 [**GradientStop.Color**](https://msdn.microsoft.com/library/windows/apps/br210094) 属性来对它应用 [**Color**](https://msdn.microsoft.com/library/windows/apps/hh673723) 动画值。

## <a name="animating-an-attached-property"></a>对附加属性进行动画处理

虽然并不是常见情形，但可以对附加属性进行动画处理，前提是附加属性具有与动画类型匹配的属性值。 因为附加属性的识别名称中已包括点，所以你必须将任何附加属性名称括在括号内，以便不会将点视为对象-属性的分隔符。 例如，用于指定你希望对某个对象上的 [**LinearGradientBrush**](https://msdn.microsoft.com/library/windows/apps/hh759795) 附加属性进行动画处理的字符串使用属性路径“(Grid.Row)”。

**注意**此示例中，对于[**Grid.Row**](https://msdn.microsoft.com/library/windows/apps/hh759795)的值是**Int32**属性类型。 因此无法使用 **Double** 动画对其进行动画处理， 而应该定义一个具有 [**DiscreteObjectKeyFrame**](https://msdn.microsoft.com/library/windows/apps/br243132) 组件的 [**ObjectAnimationUsingKeyFrames**](https://msdn.microsoft.com/library/windows/apps/br210320)，其中 [**ObjectKeyFrame.Value**](https://msdn.microsoft.com/library/windows/apps/br210344) 设置为整数（如“0”或“1”）。

## <a name="rules-for-the-properties-in-an-animation-targeting-property-path"></a>动画目标属性路径中的属性所遵守的规则

-   属性路径的假定起始点是由 [**Storyboard.TargetName**](https://msdn.microsoft.com/library/windows/apps/hh759823) 标识的对象。
-   随属性路径引用的所有对象和属性都必须是公开的。
-   结束属性（路径中作为最后一个命名属性的属性）必须是公开的、可读写的，而且必须是依赖属性。
-   结束属性必须具有可由几大动画类型（[**Color**](https://msdn.microsoft.com/library/windows/apps/hh673723) 动画、**Double** 动画、[**Point**](https://msdn.microsoft.com/library/windows/apps/br225870) 动画、[**ObjectAnimationUsingKeyFrames**](https://msdn.microsoft.com/library/windows/apps/br210320)）中的一类进行动画处理的属性类型。

## <a name="the-propertypath-class"></a>PropertyPath 类

[**PropertyPath**](https://msdn.microsoft.com/library/windows/apps/br244259) 类是用于绑定方案的 [**Binding.Path**](https://msdn.microsoft.com/library/windows/apps/br209830) 的基础属性类型。

大多数情况下，你可以在 XAML 中应用 [**PropertyPath**](https://msdn.microsoft.com/library/windows/apps/br244259)，而根本不使用任何代码。 但在某些情况下，你可能希望使用代码定义一个 **PropertyPath** 对象并在运行时将其分配给某个属性。

[**PropertyPath**](https://msdn.microsoft.com/library/windows/apps/br244259) 有一个 [**PropertyPath(String)**](https://msdn.microsoft.com/library/windows/apps/br244261) 构造函数，没有默认构造函数。 你传递给此构造函数的字符串是一个使用我们前面介绍的属性路径语法定义的字符串。 这也是你用于将 [**Path**](https://msdn.microsoft.com/library/windows/apps/br209830) 分配为 XAML 属性的同一字符串。 **PropertyPath** 类的另一个（也是唯一一个）API 是 [**Path**](https://msdn.microsoft.com/library/windows/apps/br244260) 属性，该属性是只读的。 你可以将此属性用作另一个 **PropertyPath** 实例的构造字符串。

## <a name="related-topics"></a>相关主题

* [深入了解数据绑定](https://msdn.microsoft.com/library/windows/apps/mt210946)
* [情节提要动画](https://msdn.microsoft.com/library/windows/apps/mt187354)
* [{Binding} 标记扩展](binding-markup-extension.md)
* [**PropertyPath**](https://msdn.microsoft.com/library/windows/apps/br244259)
* [**绑定**](https://msdn.microsoft.com/library/windows/apps/br209820)
* [**绑定构造函数**](https://msdn.microsoft.com/library/windows/apps/br209825)
* [**DataContext**](https://msdn.microsoft.com/library/windows/apps/br208713)

