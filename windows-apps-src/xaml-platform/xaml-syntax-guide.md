---
author: jwmsft
description: 我们介绍了 XAML 语法规则，以及用于描述 XAML 语法中存在的限制或选项的术语。
title: XAML 语法指南
ms.assetid: A57FE7B4-9947-4AA0-BC99-5FE4686B611D
ms.author: jimwalk
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 1fe2460dfc5ab11a9168f1d1d87207d2b9490026
ms.sourcegitcommit: 38f06f1714334273d865935d9afb80efffe97a17
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/09/2018
ms.locfileid: "6207609"
---
# <a name="xaml-syntax-guide"></a>XAML 语法指南


我们介绍了 XAML 语法规则，以及用于描述 XAML 语法中存在的限制或选项的术语。 当出现以下情况时你会发现本主题很有用：不熟悉 XAML 语言的使用，希望加强对术语或某些语法部分的理解，或者对 XAML 语言的工作原理感兴趣，因而希望了解更多背景知识。

## <a name="xaml-is-xml"></a>XAML 即为 XML

Extensible Application Markup Language (XAML) 的基本语法基于 XML，且定义有效的 XAML 必须也是有效的 XML。 但是，XAML 也有自己的用来扩展 XAML 的语法概念。 给定的 XML 实体可能在纯 XML 中有效，但该语法在 XAML 中可能具有不同且更加完整的含义。 本主题介绍这些 XAML 语法概念。

## <a name="xaml-vocabularies"></a>XAML 词汇

XAML 与大部分 XML 用法之间的一大区别在于，XAML 通常并非通过架构（如 XSD 文件）执行。 这是因为 XAML 具有固有的可扩展性，XAML 的缩略词中的“X”正是此意。 在对 XAML 进行分析后，发现你在 XAML 中引用的元素和属性预期将以某种支持代码表示的形式存在，要么是由 Windows 运行时定义的核心类型，要么是扩展的类型或基于 Windows 运行时的类型。 有时 SDK 文档引用的类型已经内置到 Windows 运行时，并且作为 Windows 运行时的 *XAML 词汇*，可以在 XAML 中使用。 Microsoft Visual Studio 可帮助你生成在此 XAML 词汇中有效的标记。 Visual Studio 还可以包含适用于 XAML 的自定义类型，只需在项目中正确引用这些类型的源即可。 有关 XAML 和自定义类型的详细信息，请参阅 [XAML 命名空间和命名空间映射](xaml-namespaces-and-namespace-mapping.md)。

##  <a name="declaring-objects"></a>声明对象

程序员常常从对象和成员方面思考，而标记语言已概念化为各种元素和属性。 在大部分基本场景中，在 XAML 标记中声明的元素会变为支持运行时对象表示中的对象。 若要为应用创建运行时对象，需要在 XAML 标记中声明一个 XAML 元素。 当 Windows 运行时加载 XAML 时，会创建该对象。

一个 XAML 文件始终只有一个用作根的元素，该元素声明的对象将在概念上作为一些编程结构的根，这些结构包括页面或一个应用程序完整运行时定义的对象图等。

对于 XAML 语法，可采用 3 种方式在 XAML 中声明对象：

-   **直接方式，使用对象元素语法：** 该方式使用起始和结束标记将一个对象实例化为 XML 格式的元素。 你可以使用此语法声明根对象或创建可设置属性值的嵌套对象。
-   **间接方式，使用属性语法：** 该方式使用一个内联字符串值，该值中包含有关如何创建对象的说明。 XAML 分析程序使用该字符串将属性值设置为新创建的引用值。 对于它的支持仅限于某些常见对象和属性。
-   使用一种标记扩展。

这并不意味着你始终可以选择任何语法来在 XAML 词汇表中创建对象。 一些对象只能使用对象元素语法创建， 一些对象只能通过最初在属性中设置来创建。 事实上，可使用对象元素或属性语法创建的对象在 XAML 词汇表中相对较少。 即使这两种语法形式都可用，其中一种语法样式也将更常见。
还可在 XAML 中使用一些技术来引用现有的对象，而不是创建新值。 现有对象可以在其他 XAML 区域定义，也可以通过平台的某种行为和它的应用程序或编程模型显式存在。

### <a name="declaring-an-object-by-using-object-element-syntax"></a>使用对象元素语法声明对象

若要使用对象元素语法声明对象，可以编写类似于 `<objectName>  </objectName>` 的标记，其中 *objectName* 是你希望实例化的对象的类型名称。 下面说明如何使用元素来声明 [**Canvas**](https://msdn.microsoft.com/library/windows/apps/br209267) 对象：

```xml
<Canvas>
</Canvas>
```

如果一个对象不包含其他对象，可以使用一个自结束标记代替起始/结束标记对来声明对象元素： `<Canvas />`

### <a name="containers"></a>容器

许多用作 UI 元素的对象（例如 [**Canvas**](https://msdn.microsoft.com/library/windows/apps/br209267)）可包含其他对象。 这些对象有时称作容器。 下面的示例显示了一个 **Canvas** 容器，该容器仅包含一个元素对象，即 [**Rectangle**](/uwp/api/Windows.UI.Xaml.Shapes.Rectangle)。

```xml
<Canvas>
  <Rectangle />
</Canvas>
```

### <a name="declaring-an-object-by-using-attribute-syntax"></a>使用属性语法声明对象

由于此行为绑定到属性设置，因此我们将在接下来的几个部分中深入讨论这一点。

### <a name="initialization-text"></a>初始化文本

对于某些对象，可以使用将用作构造初始化值的内部文本来声明新值。 在 XAML 中，这种技术和语法称为*初始化文本*。 在概念上，初始化文本与调用具有参数的构造函数相似。 初始化文本对设置某些结构的初始值很有用。

如果想要一个具有 **x:Key** 值并因此可存在于 [**ResourceDictionary**](https://msdn.microsoft.com/library/windows/apps/br208794) 中的结构实例，通常会使用具有初始化文本的对象元素语法。 如果你要在多个目标属性之间共享该结构值，你可能会这么做。 对于某些结构，无法使用属性语法设置结构的值：初始化文本是生成有用而且可共享的 [**CornerRadius**](https://msdn.microsoft.com/library/windows/apps/br242343)、[**Thickness**](https://msdn.microsoft.com/library/windows/apps/br208864)、[**GridLength**](https://msdn.microsoft.com/library/windows/apps/br208754) 或 [**Color**](https://msdn.microsoft.com/library/windows/apps/hh673723) 资源的唯一方式。

这个简短示例使用初始化文本来指定 [**Thickness**](https://msdn.microsoft.com/library/windows/apps/br208864) 的值，在本例中指定的值将 **Left** 和 **Right** 都设置为 20，将 **Top** 和 **Bottom** 都设置为 10。 此示例显示了创建为键资源的 **Thickness**，还给出了该资源的引用。 有关 [**Thickness**](https://msdn.microsoft.com/library/windows/apps/br208864) 初始化文本的详细信息，请参阅 [**Thickness**](https://msdn.microsoft.com/library/windows/apps/br208864)。

```xml
<UserControl ...>
  <UserControl.Resources>
    <Thickness x:Key="TwentyTenThickness">20,10</Thickness>
    ....
  </UserControl.Resources>
  ...
  <Grid Margin="{StaticResource TwentyTenThickness}">
  ...
  </Grid>
</UserControl ...>
```

**请注意**一些结构无法声明为对象元素。 初始化文本不受支持，而且不能用作资源。 你必须使用属性语法才能在 XAML 中将属性设置为这些值。 这些类型包括：[**Duration**](https://msdn.microsoft.com/library/windows/apps/br242377)、[**RepeatBehavior**](https://msdn.microsoft.com/library/windows/apps/br210411)、[**Point**](https://msdn.microsoft.com/library/windows/apps/br225870)、[**Rect**](https://msdn.microsoft.com/library/windows/apps/br225994) 和 [**Size**](https://msdn.microsoft.com/library/windows/apps/br225995)。

## <a name="setting-properties"></a>设置属性

你可以在使用对象元素语法声明的对象上设置属性。 可采用多种方式在 XAML 中设置属性：

-   使用属性语法。
-   使用属性元素语法。
-   使用元素语法，其中的内容（内部文本或子元素）用于设置对象的 XAML 内容属性。
-   使用集合语法（通常是隐式的集合语法）。

正如对于对象声明一样，此列表并不意味着可以使用所有的技术来设置属性。 一些属性仅支持一种技术。
一些属性（例如，可以使用属性元素语法或特性语法的属性）可能支持多种技术。 可能的技术同时取决于属性和该属性所使用的对象类型。 在 Windows 运行时 API 引用中，你在“语法”**** 部分将会看到可以使用的 XAML 用法。 有时还有可行的备用用法，但可能会比较繁琐。 我们通常不会显示这些繁琐用法，因为我们会尽量向你展示在 XAML 中使用该属性的最佳做法或实用方案。 对于可在 XAML 中设置的属性，已在其参考页面中有关“XAML 用法”**** 的部分提供了供参考的 XAML 语法。

还有一些对象属性无法通过任何方式在 XAML 中设置，所以只能使用代码设置。 这些属性通常更适于在代码隐藏文件中（而非 XAML 中）处理。

只读属性不能在 XAML 中设置。 即使在代码中，自有类型将必须支持一些其他方式（如构造函数重载、帮助程序方法或对计算属性的支持）才能进行设置。 计算的属性依赖于其他可设置属性的值，有时还依赖具有内置处理的事件；这些功能在依赖属性系统中可用。 有关依赖属性如何用于支持计算属性的详细信息，请参阅[依赖属性概述](dependency-properties-overview.md)。

XAML 中的集合语法给人一种设置只读属性的感觉，但其实不是。 请参阅本文稍后部分中的“使用集合语法设置属性”部分。

### <a name="setting-a-property-by-using-attribute-syntax"></a>使用属性语法设置属性

设置特性值是在标记语言中设置属性值的典型方法，例如在 XML 或 HTML 中。 设置 XAML 属性与在 XML 中设置属性值相似。 属性名称在元素名称之后的标记内的任意点指定，与元素名称之间至少间隔一个空格。 属性名称之后是一个等号。 属性值包含在一对引号内。 引号既可以是双引号，也可以是单引号，只要两个引号相匹配并包含值即可。 属性值本身必须可表示为字符串。 字符串通常包含数字，但是对于 XAML，除非涉及到 XAML 分析程序而且该分析程序执行了一些基本的值转换，否则所有的属性值都是字符串值。

此示例为 4 个属性使用属性语法来设置 [**Rectangle**](/uwp/api/Windows.UI.Xaml.Shapes.Rectangle) 对象的 [**Name**](https://msdn.microsoft.com/library/windows/apps/br208735)、[**Width**](/uwp/api/Windows.UI.Xaml.FrameworkElement.Width)、[**Height**](/uwp/api/Windows.UI.Xaml.FrameworkElement.Height) 和 [**Fill**](/uwp/api/Windows.UI.Xaml.Shapes.Shape.Fill)。

```xml
<Rectangle Name="rectangle1" Width="100" Height="100" Fill="Blue" />
```

### <a name="setting-a-property-by-using-property-element-syntax"></a>使用属性元素语法设置属性

一个对象的许多属性可使用属性元素语法设置。 属性元素看上去如下所示：`<`*object*`.`*property*`>`。

若要使用属性元素语法，需要为想要设置的属性创建 XAML 属性元素。 在标准 XML 中，此元素仅被视为一个名称中包含一个点的元素。 但是在 XAML 中，元素名称中的这个点将该元素标识为属性元素，*property* 应该是支持对象模型实现中的 *object* 的一个成员。 若要使用属性元素语法，必须可指定一个对象元素，才能“填充”属性元素标记。 属性元素将始终具有一些内容（单个元素、多个元素或内部文本）；任何点都不具有自结束属性元素。

在下面的语法中，*property* 是你想要设置的属性的名称，*propertyValueAsObjectElement* 是应当满足属性的值类型要求的单个对象元素。

`<`*object*`>`

`<`*object*`.`*property*`>`

*propertyValueAsObjectElement*

`</`*object*`.`*property*`>`

`</`*object*`>`

下面的示例使用属性元素语法来设置一个具有 [**SolidColorBrush**](https://msdn.microsoft.com/library/windows/apps/br242962) 对象元素 [**Rectangle**](/uwp/api/Windows.UI.Xaml.Shapes.Rectangle) 的 [**Fill**](/uwp/api/Windows.UI.Xaml.Shapes.Shape.Fill)。 （在 **SolidColorBrush** 内，[**Color**](/uwp/api/Windows.UI.Xaml.Media.SolidColorBrush.Color) 被设置为一个属性。）此 XAML 的分析结果等同于前面使用属性语法设置 **Fill** 的 XAML 示例。

```xml
<Rectangle
  Name="rectangle1"
  Width="100" 
  Height="100"
> 
  <Rectangle.Fill> 
    <SolidColorBrush Color="Blue"/> 
  </Rectangle.Fill>
</Rectangle>
```

### <a name="xaml-vocabularies-and-object-oriented-programming"></a>XAML 词汇和面向对象的编程

显示为 Windows 运行时 XAML 类型的 XAML 成员的属性和事件通常是从基本类型继承的。 看看下面的示例：`<Button Background="Blue" .../>`。 [**Background**](https://msdn.microsoft.com/library/windows/apps/br209395) 属性不是在 [**Button**](https://msdn.microsoft.com/library/windows/apps/br209265) 类上直接声明的属性。 **Background** 是从 [**Control**](https://msdn.microsoft.com/library/windows/apps/br209390) 基类继承的。 实际上，如果你查看 **Button** 的参考主题，你将看到成员列表中至少包含一个从一系列连续的基本类中的每个类继承的成员：[**ButtonBase**](https://msdn.microsoft.com/library/windows/apps/br227736)、[**Control**](https://msdn.microsoft.com/library/windows/apps/br209390)、[**FrameworkElement**](https://msdn.microsoft.com/library/windows/apps/br208706)、[**UIElement**](https://msdn.microsoft.com/library/windows/apps/br208911) 和 [**DependencyObject**](https://msdn.microsoft.com/library/windows/apps/br242356)。 从 XAML 词汇意义上讲，“属性”**** 列表中所有的读写属性和集合属性都是继承的。 事件（如各种 [**UIElement**](https://msdn.microsoft.com/library/windows/apps/br208911) 事件）也是继承的。

如果你使用 Windows 运行时参考来进行 XAML 指导，则语法或示例代码中显示的元素名称有时针对的是最初定义该属性的类型，这是因为该参考主题是从基本类继承它的所有可能类型共享。 如果你在 XML 编辑器中使用 Visual Studio 的用于 XAML 的 IntelliSense，则 IntelliSense 及其下拉菜单能够很好地合并继承功能，并提供一个准确的属性列表，一旦你开始使用用于类实例的对象元素，这些属性就可供设置。

### <a name="xaml-content-properties"></a>XAML 内容属性

一些类型定义其某个属性，这样该属性才能支持 XAML 内容语法。 对于某种类型的 XAML 内容属性，在 XAML 中指定它时可以省略它的属性元素。 或者，你可以将该属性设置为内部文本值，方法是直接在拥有类型的对象元素标记中提供该内部文本。 XAML 内容属性对于该属性支持简单的标记语法，并通过减少嵌套使 XAML 更容易让人理解。

如果有一种 XAML 内容语法可用，Windows 运行时参考文档中针对该属性“语法”**** 的“XAML”部分将提供该语法。 例如，[**Border**](https://msdn.microsoft.com/library/windows/apps/br209250) 的 [**Child**](https://msdn.microsoft.com/library/windows/apps/br209258) 属性页显示了用来设置 **Border** 的单对象 **Border.Child** 值的 XAML 内容语法（而不是属性元素语法），如下所示：

```xml
<Border>
  <Button .../>
</Border>
```

如果声明为 XAML 内容属性的属性的类型为 **Object** 或 **String**，则 XAML 内容语法支持 XAML 内容模型中的主要内部文本：一个位于起始和结束对象标记之间的字符串。 例如，[**TextBlock**](https://msdn.microsoft.com/library/windows/apps/br209652) 的 [**Text**](https://msdn.microsoft.com/library/windows/apps/br209676) 属性页显示的 XAML 内容语法将一个内部文本值设置为 **Text**，但是字符串“Text”从未出现在标记中。 下面是一个示例用法：

```xml
<TextBlock>Hello!</TextBlock>
```

如果某个类存在一个 XAML 内容属性，则这会在该类的参考主题的“属性”部分指示出来。 查找 [**ContentPropertyAttribute**](https://msdn.microsoft.com/library/windows/apps/br228011) 的值。 此属性使用一个名为“Name”的命名字段。 “Name”的值是该类的作为 XAML 内容属性的属性的名称。 例如，在 [**Border**](https://msdn.microsoft.com/library/windows/apps/br209250) 参考页上，你将看到如下内容：ContentProperty("Name=Child")。

有一个重要的 XAML 语法规则需要引起我们的注意，那就是不能将 XAML 内容属性和你在该元素上设置的其他属性元素混用。 XAML 内容属性必须在所有其他属性元素之前或之后完全设置。 例如，下面的 XAML 无效：

``` syntax
<StackPanel>
  <Button>This example</Button>
  <StackPanel.Resources>
    <SolidColorBrush x:Key="BlueBrush" Color="Blue"/>
  </StackPanel.Resources>
  <Button>... is illegal XAML</Button>
</StackPanel>
```

## <a name="collection-syntax"></a>集合语法

目前为止给出的所有语法都将属性设置为单一对象。 但是，许多 UI 场景要求一个给定的父元素可拥有多个子元素。 例如，一个输入窗体的 UI 需要多个文本框元素、一些标签以及可能一个“Submit”按钮。 如果打算使用一种编程对象模型访问多个这样的元素，它们通常是单个集合属性中的项，并非每个项都是不同属性的值。 XAML 支持多个子元素，还支持一种典型的支持集合模型，将使用集合类型的属性视为隐式的，并对一个集合类型的任何子元素执行特殊处理。

许多集合属性还被标识为类的 XAML 内容属性。 隐式集合处理/XAML 内容语法组合在用于控件组合（如面板、视图或项目控件）的类型中经常看到。 例如，以下示例显示了在一个 [**StackPanel**](https://msdn.microsoft.com/library/windows/apps/br209635) 内组合两个对等 UI 元素的最简单的 XAML。

```xml
<StackPanel>
  <TextBlock>Hello</TextBlock>
  <TextBlock>World</TextBlock>
</StackPanel>
```

### <a name="the-mechanism-of-xaml-collection-syntax"></a>XAML 集合语法的机制

乍看起来，XAML 似乎支持一“组”只读的集合属性。 事实上，XAML 在这里支持的是向一个现有集合添加项。 实现 XAML 支持的 XAML 语言和 XAML 处理器依赖于支持集合类型中的一种约定来支持此语法。 通常会有一个支持属性，例如索引器或引用特定集合项的 **Items** 属性。 一般而言，该属性在 XAML 语法中不是显式的。 对于集合，XAML 分析的基础机制不是一个属性，而是一个方法：具体来讲在大部分情形下是 **Add** 方法。 当 XAML 处理器在 XAML 集合语法中遇到一个或多个对象元素时，首先根据元素创建每个对象，然后调用集合的 **Add** 方法添加每个新对象，从而包含集合。

当一个 XAML 分析器向集合添加项时，**Add** 方法的逻辑会确定给定 XAML 元素是否为集合对象的一个允许的子项。 许多集合类型都由支持实现设置了严格的类型，这意味着 **Add** 的输入参数希望传递的内容其类型必须与 **Add** 参数类型匹配。

对于集合属性，在尝试将集合明确指定为对象元素时一定要谨慎。 XAML 分析程序在遇到对象元素时将创建一个新对象。 如果要尝试使用的集合属性是只读的，这可能会引发 XAML 分析异常。 只需使用隐式集合语法就会消除该异常。

## <a name="when-to-use-attribute-or-property-element-syntax"></a>何时使用属性或属性元素语法

任何支持在 XAML 中设置的属性都将支持使用属性或属性元素语法来直接设置值，但可能不会支持交替使用两种语法。 一些属性支持一种语法，而另一些属性支持其他语法选项，例如 XAML 内容属性。 一个属性支持的 XAML 语法类型在依赖于该属性用作其属性类型的对象类型。 如果属性类型为原语类型，例如双精度（浮点或小数）、整型、布尔或字符串，该属性将始终支持属性语法。

如果用于设置属性的对象类型可通过处理字符串来创建，你也可以使用属性语法来设置该属性。 对于原语，始终会这样，因为类型转换内置到分析程序中。 但是，其他一些对象类型也可使用一个指定为属性值（而不是属性元素内的对象元素）的字符串来创建。 为了实现此目标，必须有一种基础类型转换（受该特定属性支持，或受使用该属性类型的所有值广泛支持）。 属性的字符串值用于设置对初始化新对象值非常重要的属性。 一种特定的类型转换器也可以创建一种常用属性类型的不同子类，具体取决于它如何独特地处理字符串中的信息。 对于支持此行为的对象类型，我们将在参考文档的语法一节中列出一种特殊的语法。 例如，[**Brush**](/uwp/api/Windows.UI.Xaml.Media.Brush) 的 XAML 语法显示如何使用属性语法来为 **Brush** 类型的任何属性（在 Windows 运行时 XAML 中，存在许多 **Brush** 属性）创建新 [**SolidColorBrush**](https://msdn.microsoft.com/library/windows/apps/br242962) 值。

## <a name="xaml-parsing-logic-and-rules"></a>XAML 分析逻辑和规则

有时，最好按照与 XAML 分析程序读取 XAML 相类似的方式来读取 XAML，也就是说将 XAML 作为一组按线性顺序遇到的字符串令牌。 XAML 分析程序必须按照一组规则来解释这些令牌，这些规则属于对 XAML 工作原理的定义。

设置特性值是在标记语言中设置属性值的典型方法，例如在 XML 或 HTML 中。 在下面的语法中，*objectName* 是你想要实例化的对象，*propertyName* 是你希望在该对象上设置的属性名称，*propertyValue* 是要设置的值。

```xml
<objectName propertyName="propertyValue" .../>

-or-

<objectName propertyName="propertyValue">

...<!--element children -->

</objectName>
```

每种语法都支持你声明一个对象并在该对象上设置一个属性。 第一个示例是标记中的一个元素，但实际上 XAML 处理器分析此标记的过程包含多个步骤。

首先，对象元素的存在表明必须实例化一个新 *objectName* 对象。 只有存在这样一个实例，才能在它之上设置实例属性 *propertyName*。

另一个 XAML 规则是元素的属性必须能够按照任何顺序进行设置。 例如，`<Rectangle Height="50" Width="100" />` 和 `<Rectangle Width="100"  Height="50" />` 之间没有区别。 使用哪种顺序只是样式问题。

**请注意**XAML 设计人员通常提升排序约定，如果你使用的设计表面不是 XML 编辑器，但你可以在后来任意编辑该 XAML，以对属性重新排序或者引入新属性。

## <a name="attached-properties"></a>附加属性

XAML通过添加一个名为*附加属性*的语法元素对 XML 进行了扩展。 类似于属性元素语法，附加属性语法包含一个点，这个点对 XAML 分析具有特殊的含义。 具体来讲，这个点将附加属性的所有者提供程序与属性名称分开。

在 XAML 中，使用语法 *AttachedPropertyProvider*.*PropertyName* 设置附加属性。以下是一个在 XAML 中设置附加属性 [**Canvas.Left**](https://msdn.microsoft.com/library/windows/apps/hh759771) 的示例：

```xml
<Canvas>
  <Button Canvas.Left="50">Hello</Button>
</Canvas>
```

你可以在某些元素上设置附加属性，因为这些元素的支持类型中没有命名为该名称的属性，这样所设置附加属性的功能在一定程度上类似于全局属性，或由其他某个 XML 命名空间定义的属性（例如 **xml:space** 属性）。

在 Windows 运行时 XAML 中，你将看到支持以下方案的附加属性：

-   子元素可以将其在布局中的行为方式（[**Canvas**](https://msdn.microsoft.com/library/windows/apps/br209267),、[**Grid**](https://msdn.microsoft.com/library/windows/apps/br242704) 和 [**VariableSizedWrapGrid**](https://msdn.microsoft.com/library/windows/apps/br227651)）通知给父容器面板。
-   控件用法会影响来自控件模板的重要控件部分（[**ScrollViewer**](https://msdn.microsoft.com/library/windows/apps/br209527) 和 [**VirtualizingStackPanel**](https://msdn.microsoft.com/library/windows/apps/br227689)）的行为。
-   使用相关类中提供的服务，其中使用该服务的服务和类不共享继承功能：[**Typography**](https://msdn.microsoft.com/library/windows/apps/hh702143)、[**VisualStateManager**](https://msdn.microsoft.com/library/windows/apps/br209021)、[**AutomationProperties**](https://msdn.microsoft.com/library/windows/apps/br209081) 和 [**ToolTipService**](https://msdn.microsoft.com/library/windows/apps/br227609)。
-   动画目标：[**Storyboard**](https://msdn.microsoft.com/library/windows/apps/br210490)。

有关详细信息，请参阅[附加属性概述](attached-properties-overview.md)。

## <a name="literal--values"></a>文字“{”值

因为左括号 \{ 是标记扩展序列的开始，所以必须使用一个转义序列来指定一个以“\{”开始的文字字符串值。 该转义序列是“\{\}”。 例如，要指定一个是单个左括号的字符串值，可以将该属性值指定为“\{\}\{”。 你也可以使用引号（例如，一个由 **""** 分隔的属性值中的 **'**）来以字符串形式提供“\{”值。

**请注意**内的引用的属性时，还可以运行"\}"。
 
## <a name="enumeration-values"></a>枚举值

Windows 运行时 API 中的许多属性都使用枚举作为值。 如果成员是读写属性，则可以通过提供一个特性值来设置这样的属性。 可以通过使用常量名称的非限定名称来确定哪个枚举值要用作该属性的值。 例如，下面介绍如何设置 XAML 形式的 [**UIElement.Visibility**](https://msdn.microsoft.com/library/windows/apps/br208992)：`<Button Visibility="Visible"/>`。 下面作为字符串的“Visible”可以直接映射到 [**Visibility**](https://msdn.microsoft.com/library/windows/apps/br209006) 枚举的命名常量 **Visible**。

-   请勿使用限定形式，因为它不起作用。 例如，下面的 XAML 无效：`<Button Visibility="Visibility.Visible"/>`
-   请勿使用常量的值。 换句话说，请勿依赖显式或隐式依赖枚举定义方式的枚举的整数值。 尽管看似可行，但无论采用 XAML 形式还是代码形式，这都是不合适的做法，因为你依赖的可能是过渡实现的详细信息。 例如，请勿执行如下操作：`<Button Visibility="1"/>`。

**请注意**在使用 XAML 和使用枚举的 Api 参考主题中，单击枚举类型的**属性值**部分中的**语法**的链接。 在指向枚举页面的此链接中，你可以发现该枚举的命名常量。

枚举可以具有与标志相同的作用，即，它们可以被归类于 **FlagsAttribute**。 如果你需要将具有标志作用的枚举的值组合指定为一个 XAML 属性值，请使用每个枚举常量的名称，在每个名称之间加一个逗号 (,)，不要有任何多余的空格字符。 具有标志作用的属性在 Windows 运行时 XAML 词汇中并不常见，但 [**ManipulationModes**](https://msdn.microsoft.com/library/windows/apps/br227934) 示例表明了支持以 XAML 的形式设置具有标志作用的枚举值。

## <a name="interfaces-in-xaml"></a>XAML 中的接口

只有在极少情况下你会看到其中属性类型为接口的 XAML 语法。 在 XAML 类型系统中，可以接受实现该接口的类型，在解析时将其作为一个值。 肯定存在一个可用作值的此类类型的一个创建实例。 在 [**ButtonBase**](https://msdn.microsoft.com/library/windows/apps/br227736) 的 [**Command**](https://msdn.microsoft.com/library/windows/apps/br227740) 和 [**CommandParameter**](https://msdn.microsoft.com/library/windows/apps/br227741) 属性的 XAML 语法中，你将看到用作类型的一个接口。 这些属性支持 Model-View-ViewModel (MVVM) 设计模式，其中 **ICommand** 接口是针对视图和模型交互方式的合约。

## <a name="xaml-placeholder-conventions-in-windows-runtime-reference"></a>Windows 运行时参考中的 XAML 占位符约定

参考主题中有针对可以使用 XAML 的 Windows 运行时 API 的“语法”**** 部分，如果你阅读过其中某个部分，就可能会看到语法中包含很多占位符。 XAML 语法是不同于 C#、 Microsoft Visual Basic 或 VisualC + + 组件扩展 (C + + CX) 语法因为 XAML 语法是一种用法语法。 它会提示你在自己的 XAML 文件中的最终用法，但不会过度规定可以使用的值。 因此，通常用法描述混合了文本和占位符的语法类型，并定义“XAML 值”**** 部分中的某些占位符。

当你在某个属性的 XAML 语法中看到类型名称/元素名称时，显示的名称为最初定义该属性的类型的名称。 但 Windows 运行时 XAML 支持基于 [**DependencyObject**](https://msdn.microsoft.com/library/windows/apps/br242356) 的类的类继承模型。 因此，通常你可以使用以下类中的属性：该类从字面上看不属于定义类，但派生自首先定义该属性/特性的类。 例如，你可以将 [**Visibility**](https://msdn.microsoft.com/library/windows/apps/br208992) 设置为使用深度继承的任何 [**UIElement**](https://msdn.microsoft.com/library/windows/apps/br208911) 派生类上的属性。 例如：`<Button Visibility="Visible" />`。 因此，对于任意 XAML 用法语法中显示的元素名称，都不要过于拘泥于字面意思；该语法对于表示该类的元素以及表示派生类的元素也可能同样适用。 在类型很少或不能显示为采用真实用法的定义元素时，该类型名称在语法中刻意小写。 例如，你看到的 **UIElement.Visibility** 的语法为：

``` syntax
<uiElement Visibility="Visible"/>
-or-
<uiElement Visibility="Collapsed"/>
```

许多 XAML 语法部分的“用法”中均包含有占位符，随后将在“语法”**** 部分下的“XAML 值”**** 部分对这些占位符进行定义。

XAML 用法部分还使用各种通用性占位符。 有些情况下，这些占位符可能并不需要在“XAML 值”**** 中重新定义，因为你可以猜到或最终会了解到这些占位符代表的含义。 我们认为大部分读者不愿意在“XAML 值”**** 中一再看到这些占位符，因而我们不定义这些占位符。 出于参考目的，下面列出了这些占位符中的部分占位符以及这些占位符的常规含义：

-   *object*：从理论上讲是任意对象值，但实际上通常被限制为特定类型的对象（例如“字符串或对象”选择），并且应查看参考页面上的“备注”部分以获取更多信息。
-   *object* *property*：*object* *property* 组合用于以下情况：要显示的语法所适用于的类型可用作许多属性的属性值。 例如，为 [**Brush**](/uwp/api/Windows.UI.Xaml.Media.Brush) 显示的 **“Xaml 属性用法”** 包括：&lt;*object* *property*="*predefinedColorName*"/&gt;
-   *eventhandler*：这将显示为为事件属性显示的每个 XAML 语法的属性值。 此处所提供的内容为事件处理程序函数的函数名。 该函数必须在 XAML 页面的代码隐藏中定义。 在编程级别上，该函数必须与你处理的事件的委派签名匹配，或者你的应用代码未编译。 但这实际是出于编程方面的考虑，而不是 XAML 方面的考虑，因此我们不尝试在 XAML 语法中暗示有关委派类型的任何内容。 如果你希望了解你应实现事件的哪个委托，请在标记为“委托”**** 的表行中参阅该事件参考主题的“事件信息”**** 部分。
-   *enumMemberName*：显示在所有枚举的属性语法中。 存在使用枚举值的属性的类似占位符，但它通常为占位符附加该枚举的名称提示以作为前缀。 例如，为 [**FrameworkElement.FlowDirection**](https://msdn.microsoft.com/library/windows/apps/br208716) 显示的语法为 <*frameworkElement***FlowDirection**="* flowDirectionMemberName*"/>。 如果你位于某个属性参考页面上，请在“类型:”**** 字样旁边的“属性值”**** 部分单击枚举类型的链接。 对于使用该枚举的属性的属性值，你可以使用“成员”**** 列表的“成员”**** 列中列出的任意字符串。
-   *double*、*int*、*string*、*bool*：存在 XAML 语言已知的基元类型。 如果你使用 C# 或 Visual Basic 进行编程，则这些类型将映射到 Microsoft .NET 等价类型，例如 [**Double**](https://msdn.microsoft.com/library/windows/apps/xaml/system.double.aspx)、[**Int32**](https://msdn.microsoft.com/library/windows/apps/xaml/system.int32.aspx)、[**String**](https://msdn.microsoft.com/library/windows/apps/xaml/system.string.aspx) 和 [**Boolean**](https://msdn.microsoft.com/library/windows/apps/xaml/system.boolean.aspx)，并且在使用 .NET 代码隐藏中的 XAML 定义的值时你可以使用这些 .NET 类型上的任意成员。 如果你使用 C++/CX 进行编程，则可以使用 C++ 基元类型，但也可以考虑使用 [**Platform**](https://msdn.microsoft.com/library/windows/apps/xaml/hh710417.aspx) 命名空间定义的类型的等价类型，例如 [**Platform::String**](https://msdn.microsoft.com/library/windows/apps/xaml/hh755812.aspx)。 有时，对于特定属性存在其他值限制。 但你通常会在“属性值”**** 部分或“备注”部分（而非 XAML 部分）看到这些限制，因为所有此类限制都既适用于代码用法也适用于 XAML 用法。

## <a name="tips-and-tricks-notes-on-style"></a>有关样式的提示、技巧和注释

-   主 [XAML 概述](xaml-overview.md)中介绍了全体的标记扩展。 但是，对本主题中提供的指导影响最大的标记扩展是 [StaticResource](staticresource-markup-extension.md) 标记扩展（以及相关 [ThemeResource](themeresource-markup-extension.md)）。 StaticResource 标记扩展的作用是允许将你的 XAML 计入来自 XAML [**ResourceDictionary**](https://msdn.microsoft.com/library/windows/apps/br208794) 的可重用资源中。 几乎始终在 **ResourceDictionary** 中定义控件模板和相关样式。 通常还在 **ResourceDictionary** 中定义控件模板定义的较小部件或特定于应用的样式，例如，对于由应用中的不同 UI 部件多次使用的某种颜色，定义 [**SolidColorBrush**](https://msdn.microsoft.com/library/windows/apps/br242962)。 通过使用 StaticResource，任何之前需要使用属性元素来设置的属性现在都可以使用属性语法进行设置。 但是，使 XAML 可重用所带来的好处绝不仅仅是简化了页面级语法。 有关详细信息，请参阅 [ResourceDictionary 和 XAML 资源参考](https://msdn.microsoft.com/library/windows/apps/mt187273)。
-   对于 XAML 示例中如何应用空格和换行符，你将看到几种不同的约定。 具体而言，对于如何拆分设置了许多不同属性的对象元素存在不同的约定。 这只是样式问题。 Visual Studio XML 编辑器会在你编辑 XAML 时应用默认的样式规则，但是你可以在设置中更改默认样式。 在少数情况下，XAML 文件中的空格被视为非常重要；有关详细信息，请参阅 [XAML 和空格](xaml-and-whitespace.md)。

## <a name="related-topics"></a>相关主题

* [XAML 概述](xaml-overview.md)
* [XAML 命名空间和命名空间映射](xaml-namespaces-and-namespace-mapping.md)
* [ResourceDictionary 和 XAML 资源引用](https://msdn.microsoft.com/library/windows/apps/mt187273)
 

