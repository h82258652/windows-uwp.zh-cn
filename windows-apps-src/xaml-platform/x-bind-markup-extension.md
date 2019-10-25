---
description: XBind 标记扩展是一种用于绑定的高性能替代方法。 xBind-Windows 10 的新版-运行时间比绑定更少，内存更少，并支持更好的调试。
title: xBind 标记扩展
ms.assetid: 529FBEB5-E589-486F-A204-B310ACDC5C06
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10，uwp
ms.localizationpriority: medium
ms.openlocfilehash: a25797f50ee76542b8f9543cb76453d2916368ac
ms.sourcegitcommit: 82d202478ab4d3011c5ddd2e852958c34336830d
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/22/2019
ms.locfileid: "72715854"
---
# <a name="xbind-markup-extension"></a>{x:Bind} 标记扩展

**请注意**， 有关在应用中使用数据绑定的常规信息，请参阅 **{x:Bind}** （以及 **{x:Bind}** and **{binding}** 之间的所有比较），请参阅[深入了解数据绑定](https://docs.microsoft.com/windows/uwp/data-binding/data-binding-in-depth)。

**{X:Bind}** 标记扩展（适用于 Windows 10 的新）是 **{Binding}** 的替代项。 **{x:Bind}** 在比 **{Binding}** 更少的时间和较少的内存中运行，并且支持更好的调试。

在 XAML 编译时， **{x:Bind}** 转换为将从数据源的属性获取值的代码，并在标记中指定的属性上设置该值。 可以选择性地将绑定对象配置为观察 "数据源" 属性的值中的更改，并根据这些更改（`Mode="OneWay"`）来刷新自身。 还可以根据需要将其配置为将自己的值中的更改推送回源属性（`Mode="TwoWay"`）。

**{X:Bind}** **} 和 {binding}** 创建的绑定对象在功能上是等效的。 但 **{x:Bind}** 执行了在编译时生成的特殊用途的代码，而 **{Binding}** 使用常规用途的运行时对象检查。 因此， **{x:Bind}** 绑定（通常称为编译绑定）具有良好的性能，提供绑定表达式的编译时验证，并支持调试，方法是在代码文件中设置断点生成为页面的分部类。 可以在 `obj` 文件夹中找到这些文件，其名称类似于（对于C#）`<view name>.g.cs`。

> [!TIP]
> **{x:Bind}** 的默认模式为**一次性**，其默认模式为 "**单向** **"。** 出于性能原因，我们选择了此功能，因为使用**单向**会导致生成更多代码以进行挂钩和处理更改检测。 可以显式指定模式以使用单向或双向绑定。 还可以使用[x:DefaultBindMode](x-defaultbindmode-attribute.md)为标记树的特定段更改 **{x:Bind}** 的默认模式。 指定模式适用于该元素及其子级上的任何 **{x:Bind}** 表达式，这些表达式不会将模式显式指定为绑定的一部分。

**演示 {x:Bind} 的示例应用**

-   [{x:Bind} 示例](https://go.microsoft.com/fwlink/p/?linkid=619989)
-   [QuizGame](https://github.com/microsoft/Windows-appsample-networkhelper)
-   [XAML UI 基础示例](https://go.microsoft.com/fwlink/p/?linkid=619992)

## <a name="xaml-attribute-usage"></a>XAML 属性用法

``` syntax
<object property="{x:Bind}" .../>
-or-
<object property="{x:Bind propertyPath}" .../>
-or-
<object property="{x:Bind bindingProperties}" .../>
-or-
<object property="{x:Bind propertyPath, bindingProperties}" .../>
-or-
<object property="{x:Bind pathToFunction.functionName(functionParameter1, functionParameter2, ...), bindingProperties}" .../>
```

| 术语 | 说明 |
|------|-------------|
| _propertyPath_ | 一个指定绑定的属性路径的字符串。 有关详细信息，请在下面的 "[属性路径](#property-path)" 一节中。 |
| _bindingProperties_ |
| _propName_=_值_\[， _propName_=_值_\]* | 使用名称/值对语法指定的一个或多个绑定属性。 |
| _propName_ | 要在绑定对象上设置的属性的字符串名称。 例如，"转换器"。 |
| value | 要将属性设置为的值。 参数的语法取决于所设置的属性。 下面是_propName_=_值_使用情况的示例，其中值本身就是标记扩展： `Converter={StaticResource myConverterClass}`。 有关详细信息，请参阅下面的[{x:Bind} 部分可设置的属性](#properties-that-you-can-set-with-xbind)。 |

## <a name="examples"></a>示例

```XAML
<Page x:Class="QuizGame.View.HostView" ... >
    <Button Content="{x:Bind Path=ViewModel.NextButtonText, Mode=OneWay}" ... />
</Page>
```

此示例 XAML 将 **{x:Bind}** 与**ListView**属性一起使用。 请注意**x:DataType**值的声明。

```XAML
  <DataTemplate x:Key="SimpleItemTemplate" x:DataType="data:SampleDataGroup">
    <StackPanel Orientation="Vertical" Height="50">
      <TextBlock Text="{x:Bind Title}"/>
      <TextBlock Text="{x:Bind Description}"/>
    </StackPanel>
  </DataTemplate>
```

## <a name="property-path"></a>属性路径

*PropertyPath*设置 **{X:Bind}** 表达式的**路径**。 **Path**是一个属性路径，该路径指定要绑定到的属性、子属性、字段或方法的值（源）。 您可以显式提到**路径**属性的名称： `{x:Bind Path=...}`。 或者，您可以省略它： `{x:Bind ...}`。

### <a name="property-path-resolution"></a>属性路径解析

**{x:Bind}** 不使用**DataContext**作为默认源，而是使用页面或用户控件本身。 因此，它将在页面或用户控件的代码隐藏中查找属性、字段和方法。 若要向 **{x:Bind}** 公开视图模型，通常需要向页面或用户控件的隐藏代码中添加新的字段或属性。 属性路径中的步骤由点（.）分隔，可以包含多个分隔符来遍历后续的子属性。 使用点分隔符，而不考虑用于实现要绑定到的对象的编程语言。

例如：在页中， **Text = "{x:Bind}"** 将在页上查找**employee**成员，然后在**员工**返回的对象上查找**FirstName**成员。 如果要将 items 控件绑定到包含雇员依赖项的属性，则属性路径可能为 "Employee"，items 控件的项模板将负责显示 "依赖项" 中的项。

对于C++/cx， **{x:Bind}** 不能绑定到页或数据模型中的私有字段和属性–您需要有一个公共属性才能绑定。 绑定的外围应用需要作为 CX 类/接口公开，以便我们可以获取相关元数据。 不应需要 **\[可绑定的\]** 属性。

使用**x:Bind**，无需将**ElementName = xxx**用作绑定表达式的一部分。 相反，你可以使用元素的名称作为绑定路径的第一部分，因为命名元素成为表示根绑定源的页或用户控件中的字段。 


### <a name="collections"></a>集合

如果数据源是集合，则属性路径可以按位置或索引指定集合中的项。 例如，"团队\[0\]。扮演者 "，其中文本"\[\]"包含请求零索引集合中第一项的" 0 "。

若要使用索引器，该模型需要实现**IList&lt;t&gt;** 或**IVector&lt;t&gt;** 要建立索引的属性的类型。 （请注意，IReadOnlyList&lt;T&gt; 和 IVectorView&lt;T&gt; 不支持索引器语法。）如果已编制索引的属性的类型支持**INotifyCollectionChanged**或**IObservableVector** ，而绑定是单向或双向，则它将在这些接口上注册和侦听更改通知。 更改检测逻辑将基于所有集合更改进行更新，即使这不会影响特定的索引值也是如此。 这是因为侦听逻辑在集合的所有实例中都是通用的。

如果数据源是字典或映射，则属性路径可以通过其字符串名称来指定集合中的项。 例如 **&lt;TextBlock Text = "{X:Bind 运动员\[" John smith "\]"/&gt;** 将在名为 "john smith" 的字典中查找一项。 名称必须用引号括起来，并且可以使用单引号或双引号。 Hat （^）可用于对字符串中的引号进行转义。 最简单的方法是使用适用于 XAML 属性的替代引号。 （请注意，System.collections.generic.ireadonlydictionary<tkey&lt;T&gt; 和 IMapView&lt;T&gt; 不支持索引器语法。）

若要使用字符串索引器，模型需要实现**IDictionary&lt;字符串、t&gt;** 或**IMap&lt;string、t&gt;** 要建立索引的属性的类型。 如果已编制索引的属性的类型支持**IObservableMap** ，并且绑定是单向或双向，则它将在这些接口上注册和侦听更改通知。 更改检测逻辑将基于所有集合更改进行更新，即使这不会影响特定的索引值也是如此。 这是因为侦听逻辑在集合的所有实例中都是通用的。

### <a name="attached-properties"></a>附加属性

若要绑定到[附加属性](./attached-properties-overview.md)，需要将类和属性名称放在点后的括号中。 例如**Text = "{X:Bind Button22 （Grid Row）} "** 。 如果未在 Xaml 命名空间中声明该属性，则需要将其作为 xml 命名空间的前缀，该命名空间应映射到文档开头的代码命名空间。

### <a name="casting"></a>强制转换

已编译的绑定是强类型化的，将解析路径中每个步骤的类型。 如果返回的类型不具有成员，则在编译时将失败。 您可以指定强制转换以通知绑定对象的实类型。 在以下情况下， **obj**是 object 类型的属性，但包含文本框，因此，我们可以使用**text = "{X:Bind （（TextBox） obj）"。Text} "** 或**text =" {x:Bind obj. （TextBox. Text）} "** 。
**Text = "{x:Bind （groups3） groups3\[0\]）中的字段。Title} "** 是对象的字典，因此必须将其强制转换为**数据： SampleDataGroup**。 请注意用于将对象类型映射到不属于默认 XAML 命名空间的代码命名空间的 xml**数据：** 命名空间前缀。

_注意： C#-样式强制转换语法比附加的属性语法更为灵活，并且是以后建议使用的语法。_

## <a name="functions-in-binding-paths"></a>绑定路径中的函数

从 Windows 10 版本1607开始， **{x:Bind}** 支持将函数用作绑定路径的叶步骤。 这是一项功能强大的数据绑定功能，可在标记中启用多个方案。 有关详细信息，请参阅[函数绑定](../data-binding/function-bindings.md)。

## <a name="event-binding"></a>事件绑定

事件绑定是编译绑定的独特功能。 它使你能够使用绑定指定事件的处理程序，而不是将其作为代码隐藏的方法。 例如：**单击 = "{X:Bind rootFrame. GoForward}"** 。

对于事件，不能重载目标方法，还必须执行以下操作：

- 与事件的签名相匹配。
- 或没有参数。
- 或具有可从事件参数的类型赋值的类型的相同数目的参数。

在生成的代码隐藏中，编译的绑定处理事件并将其路由到模型上的方法，并在事件发生时计算绑定表达式的路径。 这意味着，与属性绑定不同，它不跟踪对模型所做的更改。

有关属性路径的字符串语法的详细信息，请参阅此处为 **{x:Bind}** 所述的[属性路径语法](property-path-syntax.md)。

## <a name="properties-that-you-can-set-with-xbind"></a>可以用 {x:Bind} 设置的属性

**{x:Bind}** 用*bindingProperties*占位符语法阐释，因为有多个可在标记扩展中设置的读/写属性。 这些属性可以按任意顺序进行设置，以逗号分隔的*propName*=*值*对。 请注意，不能在绑定表达式中包含分行符。 某些属性需要类型转换的类型，因此它们需要嵌套在 **{x:Bind}** 中的标记扩展。

这些属性的工作方式与[**绑定**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Data.Binding)类的属性大致相同。

| 属性 | 说明 |
|----------|-------------|
| **路径** | 请参阅上面的 "[属性路径](#property-path)" 一节。 |
| **转换器** | 指定绑定引擎调用的转换器对象。 转换器可以在 XAML 中设置，但仅当你引用在资源字典中对该对象的[{StaticResource} 标记扩展](staticresource-markup-extension.md)引用中分配的对象实例时。 |
| **ConverterLanguage** | 指定转换器要使用的区域性。 （如果要设置**ConverterLanguage** ，则还应设置**转换器**。）区域性设置为基于标准的标识符。 有关详细信息，请参阅[**ConverterLanguage**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.data.binding.converterlanguage)。 |
| **ConverterParameter** | 指定可在转换器逻辑中使用的转换器参数。 （如果要设置**ConverterParameter** ，则还应设置**转换器**。）大多数转换器使用简单的逻辑从传递的值获取所需的所有信息以进行转换，而无需使用**ConverterParameter**值。 **ConverterParameter**参数适用于适度的高级转换器实现，这些实现具有多个逻辑，它们会关闭**ConverterParameter**中传递的内容。 您可以编写使用字符串以外的值的转换器，但这种情况并不常见，有关详细信息，请参阅[**ConverterParameter**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.data.binding.converterparameter)中的备注。 |
| **FallbackValue** | 指定在无法解析源或路径时要显示的值。 |
| **模式** | 指定绑定模式，其形式为以下字符串之一： "一次性"、"单向" 或 "双向"。 默认值为 "一次性"。 请注意，这与 **{Binding}** 的默认值不同，后者在大多数情况下为 "单向"。 |
| **TargetNullValue** | 指定一个值，该值在源值解析但显式为**空**时显示。 |
| **BindBack** | 指定一个用于双向绑定反向方向的函数。 |
| **System.windows.data.binding.updatesourcetrigger** | 指定何时将更改从控件推送回双向绑定中的模型。 除 TextBox 之外的所有属性的默认值。 Text 为 PropertyChanged;TextBox。文本为 LostFocus。|

> [!NOTE]
> 如果要将标记从 **{Binding}** 转换为 **{x:Bind}** ，请注意**Mode**属性的默认值之间的差异。
> 可以使用[**x:DefaultBindMode**](https://docs.microsoft.com/windows/uwp/xaml-platform/x-defaultbindmode-attribute)为标记树的特定段更改 x:Bind 的默认模式。 所选模式将应用该元素及其子级上的任何 x:Bind 表达式，这些表达式不会将模式显式指定为绑定的一部分。 一次性的性能比使用单向时的性能更高，因为使用单向将导致生成更多代码以进行挂钩并处理更改检测。

## <a name="remarks"></a>备注

因为 **{x:Bind}** 使用生成的代码来实现其优点，所以它在编译时需要类型信息。 这意味着你不能在不事先知道类型的情况下绑定到属性。 因此，不能将 **{x:Bind}** 与**DataContext**属性（其类型为**Object**）一起使用，也不能在运行时进行更改。

将 **{x:Bind}** 与数据模板结合使用时，必须通过设置**x:DataType**值来指示要绑定到的类型，如 "[示例](#examples)" 部分所示。 你还可以将类型设置为接口或基类类型，并在必要时使用强制转换来构建完整表达式。

编译的绑定依赖于代码生成。 如果在资源字典中使用 **{x:Bind}** ，则资源字典需要具有代码隐藏类。 有关代码示例，请参阅[具有 {x:Bind} 的资源字典](../data-binding/data-binding-in-depth.md#resource-dictionaries-with-x-bind)。

包含已编译绑定的页和用户控件将在生成的代码中包含 "绑定" 属性。 这包括以下方法：

- **Update （）** -这将更新所有已编译绑定的值。 任何单向/双向绑定都将具有挂接到的侦听器来检测更改。
- **Initialize （）** -如果绑定尚未初始化，则它将调用 Update （）来初始化绑定。
- **StopTracking （）** -这会将为单向和双向绑定创建的所有侦听器解除挂钩。 可以使用 Update （）方法重新初始化它们。

> [!NOTE]
> 从 Windows 10 版本1607开始，XAML 框架为可见性转换器提供内置的布尔值。 转换器将**true**映射到**可见**的枚举值，将**false**映射到**折叠**，以便可以将可见性属性绑定到布尔值，而无需创建转换器。 请注意，这并不是函数绑定的功能，只是属性绑定。 若要使用内置转换器，应用的最低目标 SDK 版本必须为14393或更高版本。 如果你的应用面向 Windows 10 的早期版本，则无法使用它。 有关目标版本的详细信息，请参阅[版本自适应代码](https://docs.microsoft.com/windows/uwp/debug-test-perf/version-adaptive-code)。

**提示**  如果需要为一个值指定一个大括号（如在[**Path**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.data.binding.path)或[**ConverterParameter**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.data.binding.converterparameter)中），请在它前面加上一个反斜杠： `\{`。 或者，将包含需要转义的大括号的整个字符串括在辅助引号集中，例如 `ConverterParameter='{Mix}'`。

[**转换器**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.data.binding.converter)、 [**ConverterLanguage**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.data.binding.converterlanguage)和**ConverterLanguage**都与将值或类型从绑定源转换为与绑定目标属性兼容的类型或值相关。 有关详细信息和示例，请参阅[数据绑定深度中](https://docs.microsoft.com/windows/uwp/data-binding/data-binding-in-depth)的 "数据转换" 部分。

**{x:Bind}** 仅为标记扩展，无法以编程方式创建或操作此类绑定。 有关标记扩展的详细信息，请参阅[XAML 概述](xaml-overview.md)。

