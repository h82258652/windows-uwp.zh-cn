---
author: jwmsft
description: XBind 标记扩展是 Binding 的高性能的替代方法。 xBind-新的 Windows 10-运行在更少的时间和更低的内存比绑定和支持更好的调试。
title: xBind 标记扩展
ms.assetid: 529FBEB5-E589-486F-A204-B310ACDC5C06
ms.author: jimwalk
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 9674aeed846fab53de3ecb057ebcc46dbda560c3
ms.sourcegitcommit: ed0304b8a214c03b8aab74b8ef12c9f82b8e3c5f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/19/2018
ms.locfileid: "7300435"
---
# <a name="xbind-markup-extension"></a>{x:Bind} 标记扩展

**注意**有关使用数据的常规信息随 **{x: Bind}** （和 **{x: Bind}** 和 **{Binding}** 之间的全方位比较），在应用中的绑定，请参阅[深入了解数据绑定](https://msdn.microsoft.com/library/windows/apps/mt210946)。

**{X: Bind}** 标记扩展-windows 10 的新功能 — 是 **{Binding}** 的替代方法。 **{x: Bind}** 运行在更少的时间和更低的内存比 **{绑定}** 和支持更好的调试。

XAML 编译时，**{x:Bind}** 将转换为从数据源上的某一属性中获取相关值的代码，并将其设置到标记中指定的属性上。 绑定对象可以配置为观察数据源属性值的更改，并基于这些更改自行刷新 (`Mode="OneWay"`)。 该对象也可以配置为将其自己的值的更改推送回源属性 (`Mode="TwoWay"`)。

由 **{x:Bind}** 和 **{Binding}** 创建的绑定对象在功能上大致等同。 不过，**{x:Bind}** 执行编译时所生成的专用代码，而 **{Binding}** 使用通用的运行时对象检查。 因此，**{x:Bind}** 绑定（通常指已编译的绑定）具有出色的性能、提供编译时对绑定表达式的验证，并支持通过允许你在作为页面的部分类生成的代码文件中设置断点进行调试。 可以在 `obj` 文件夹中找到这些文件，其名称类似于（适用于 C#）`<view name>.g.cs`。

> [!TIP]
> **{x:Bind}** 的默认模式为 **OneTime**，与 **{Binding}** 不同，后者的默认模式为 **OneWay**。 选择它是出于性能原因，因为使用 **OneWay** 将导致生成更多代码以连接到和处理更改检测。 你可以明确指定使用 OneWay 或 TwoWay 绑定的模式。 也可以使用 [x:DefaultBindMode](x-defaultbindmode-attribute.md) 来针对标记树的特定段更改 **{x:Bind}** 的默认模式。 指定的模式将应用该元素及其子元素上的任何 **{x:Bind}** 表达，不明确指定某个模式作为绑定的一部分。

**用于演示 {x:Bind} 的应用示例**

-   [{x:Bind} 示例](http://go.microsoft.com/fwlink/p/?linkid=619989)
-   [QuizGame](https://github.com/Microsoft/Windows-appsample-quizgame)
-   [XAML UI 基本示例](http://go.microsoft.com/fwlink/p/?linkid=619992)

## <a name="xaml-attribute-usage"></a>XAML 属性使用方法

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
| _propertyPath_ | 一个指定绑定的属性路径的字符串。 下面的[属性路径](#property-path)部分中提供了更多信息。 |
| _bindingProperties_ |
| _propName_=_value_\[, _propName_=_value_\]* | 使用一个名称/值对语法指定的一个或多个绑定属性。 |
| _propName_ | 要在绑定对象上设置的属性的字符串名称。 例如，“Converter”。 |
| _value_ | 要将属性设置为的值。 参数的语法取决于要设置的属性。 下面是 _propName_=_value_ 用法的示例，其中该值本身就是一个标记扩展：`Converter={StaticResource myConverterClass}`。 有关详细信息，请参阅下面的[可使用 {x:Bind} 设置的属性](#properties-you-can-set)部分。 |

## <a name="examples"></a>示例

```XAML
<Page x:Class="QuizGame.View.HostView" ... >
    <Button Content="{x:Bind Path=ViewModel.NextButtonText, Mode=OneWay}" ... />
</Page>
```

本示例中 XAML 会将 **{x:Bind}** 与 **ListView.ItemTemplate** 属性结合使用。 请注意 **x:DataType** 值的声明。

```XAML
  <DataTemplate x:Key="SimpleItemTemplate" x:DataType="data:SampleDataGroup">
    <StackPanel Orientation="Vertical" Height="50">
      <TextBlock Text="{x:Bind Title}"/>
      <TextBlock Text="{x:Bind Description}"/>
    </StackPanel>
  </DataTemplate>
```

## <a name="property-path"></a>属性路径

*PropertyPath* 为 **{x:Bind}** 表达式设置 **Path**。 **Path** 是一个属性路径，用于指定要绑定到的（源）属性、子属性、字段或方法的值。 你可以明确指出 **Path** 属性名称：`{Binding Path=...}`。 也可以将其省略：`{Binding ...}`。

### <a name="property-path-resolution"></a>属性路径解决方案

**{x:Bind}** 不会将 **DataContext** 用作默认源，而是将改用页面或用户控件本身。 因此，它将针对属性、字段和方法查找代码隐藏的页面或用户控件。 要向 **{x:Bind}** 显示视图模型，你通常需要将新字段或属性添加到代码隐藏的页面或用户控件。 属性路径中的步骤由点号 (.) 分隔，并且可包含多个分隔符以遍历连续的子属性。 无论使用何种编程语言，均可将点号分隔符用于实现要绑定到的对象。

例如，在某一页面中，**Text="{x:Bind Employee.FirstName}"** 将查找该页面上的 **Employee** 成员，然后该对象上的 **FirstName** 成员将由 **Employee** 返回。 如果将一个项目控件绑定到一个包含员工家属的属性，则属性路径可能是“Employee.Dependents”，并且项目控件的项目模板将负责显示“Dependents”中的项。

对于 C++/CX，**{x:Bind}** 无法绑定到页面或数据模型中的私有字段和属性，你需要具有其可绑定的公共属性。 绑定的图面区域需显示为 CX 类/接口，以便我们可以获取相关的元数据。 无需使用 **\[Bindable\]** 属性。

使用 **x:Bind** 时，无需将 **ElementName=xxx** 用作绑定表达式的一部分。 相反，你可以使用元素的名称作为路径的第一部分为绑定因为已命名的元素变为表示根绑定源的页面或用户控件内的字段。 


### <a name="collections"></a>集合

如果数据源是一个集合，则属性路径可以按照位置或索引来指定集合中的项目。 例如“Teams\[0\].Players”，其中文本“\[\]”中包含“0”，用以请求从零开始编制索引的集合中的第一个项目。

若要使用索引器，该模型需要在将编入索引的属性类型上实现 **IList&lt;T&gt;** 或 **IVector&lt;T&gt;**。 如果已编入索引的属性类型支持 **INotifyCollectionChanged** 或 **IObservableVector** 且绑定是单向或双向，则它将针对这些接口上的更改通知进行注册和侦听。 更改检测逻辑将基于所有集合更改进行更新，即使这不会影响特定的索引值也是如此。 这是因为侦听逻辑在集合的所有实例中是通用的。

如果数据源是字典或地图，则属性路径可以按字符串名称指定集合中的项。 例如，**&lt;TextBlock Text="{x:Bind Players\['John Smith'\]" /&gt;** 将在字典中查找名为“John Smith”的项。 名称需要使用引号括起来，单引号或双引号都可以使用。 乘幂号 (^) 可用于转义字符串中的引号。 通常最简单的做法是替换使用用于 XAML 属性的引号。

若要使用字符串索引器，该模型需要在将编入索引的属性类型上实现 **IDictionary&lt;string, T&gt;** 或 **IMap&lt;string, T&gt;**。 如果已编入索引的属性类型支持 **IObservableMap** 且绑定是单向或双向，则它将针对这些接口上的更改通知进行注册和侦听。 更改检测逻辑将基于所有集合更改进行更新，即使这不会影响特定的索引值也是如此。 这是因为侦听逻辑在集合的所有实例中是通用的。

### <a name="attached-properties"></a>附加属性

若要绑定到附加属性，你需要将类和属性名称放入点号后面的括号内。 例如 **Text="{x:Bind Button22.(Grid.Row)}"**。 如果未在 Xaml 命名空间中声明该属性，你需要在其前面加上 xml 命名空间，这应该映射到文档的标头处的代码命名空间中。

### <a name="casting"></a>强制转换

已编译的绑定为强类型，并且将解析路径中的每个步骤的类型。 如果返回的类型没有成员，则它将在编译时失败。 你可以指定转换来告知绑定对象的实际类型。 在以下用例中，**obj** 为类型对象的属性，但包含一个文本框，因此我们可以使用 **Text="{x:Bind ((TextBox)obj).Text}"** 或 **Text="{x:Bind obj.(TextBox.Text)}"**。
**Text="{x:Bind ((data:SampleDataGroup)groups3\[0\]).Title}"** 中的 **groups3** 字段是一个对象字典，因此必须将其转换为 **data:SampleDataGroup**。 请注意 xml **data:** 命名空间前缀的用法，可用于将对象类型映射到不是默认 XAML 命名空间组成部分的某一代码命名空间。

_注意：C# 样式的强制转换语法较附加属性语法更灵活，是接下来建议使用的语法。_

## <a name="functions-in-binding-paths"></a>绑定路径中的函数

从 Windows10 版本 1607 开始，**{x:Bind}** 支持使用某个函数作为绑定路径的叶步。 这是一项强大功能启用多个方案在标记中的数据绑定。 [函数绑定](../data-binding/function-bindings.md)的详细信息，请参阅。

## <a name="event-binding"></a>事件绑定

事件绑定是编译绑定的一项独特功能。 它允许你使用绑定为事件指定处理程序，而无需使其成为代码隐藏的方法。 例如：**Click="{x:Bind rootFrame.GoForward}"**。

对于事件，目标方法不能重载，而且还必须：

- 匹配事件的签名。
- 或者没有任何参数。
- 或者具有相同数量的参数类型，这些参数根据事件参数的类型进行赋值。

在生成的代码隐藏中，已编译的绑定将处理事件并将其路由到模型上的对应方法，并在该事件发生时计算绑定表达式的路径。 这意味着，与属性绑定不同的是，它不跟踪模型的更改。

有关属性路径的字符串语法的详细信息，请参阅 [Property-path 语法](property-path-syntax.md)，记住此处所述的 **{x:Bind}** 的不同之处。

## <a name="properties-that-you-can-set-with-xbind"></a>可使用 {x:Bind} 设置的属性

**{x:Bind}** 通过 *bindingProperties* 占位符语法来解释，因为标记扩展中可以设置多个读/写属性。 这些属性可按任何顺序设置，并带有以逗号分隔的 *propName*=*value* 对。 请注意，不得将换行符包含在绑定表达式中。 由于其中一些属性需要不具有类型转换的类型，因此它们需要一些自己嵌套在 **{x:Bind}** 内的标记扩展。

这些属性的工作方式与 [**Binding**](https://msdn.microsoft.com/library/windows/apps/br209820) 类的属性的工作方式大体相同。

| 属性 | 说明 |
|----------|-------------|
| **Path** | 请参阅上面的[属性路径](#property-path)部分。 |
| **转换器** | 指定绑定引擎所调用的转换器对象。 转换器可以在 XAML 中设置，但仅限于你引用你在对资源字典中的该对象的 [{StaticResource} 标记扩展](staticresource-markup-extension.md)引用中分配的对象实例。 |
| **ConverterLanguage** | 指定转换器要使用的区域性。 （如果要设置 **ConverterLanguage**，还应该设置 **Converter**。）区域性可设置为一个基于标准的标识符。 有关详细信息，请参阅 [**ConverterLanguage**](https://msdn.microsoft.com/library/windows/apps/hh701880)。 |
| **ConverterParameter** | 指定可在转换器逻辑中使用的转换器参数。 （如果要设置 **ConverterParameter**，还应该设置 **Converter**。）大多数转换器使用可从要转换的传递值获取所有所需信息的简单逻辑，不需要 **ConverterParameter** 值。 **ConverterParameter** 参数适用于具有多个逻辑的中等高级转换器实现，这些逻辑可切断传入 **ConverterParameter** 的内容。 你可以编写一个转换器，使用除字符串之外的值，但这种情况并不常见，请参阅 [**ConverterParameter**](https://msdn.microsoft.com/library/windows/apps/br209827) 中的备注，以获取详细信息。 |
| **FallbackValue** | 指定要在无法解析源或路径时显示的值。 |
| **模式** | 将绑定模式指定为以下字符串之一：“OneTime”、“OneWay”或“TwoWay”。 默认值是“OneTime”。 请注意，该值不是 **{Binding}** 的默认值，大多数情况下为“OneWay”。 |
| **TargetNullValue** | 指定要在源值解析但并非显式 **null** 时显示的值。 |
| **BindBack** | 指定要用于双向绑定的相反方向的函数。 |
| **UpdateSourceTrigger** | 指定何时将更改从控件推送回 TwoWay 绑定中的模式。 除 TextBox.Text 以外的所有属性默认为 PropertyChanged，TextBox.Text 默认为 LostFocus。|

> [!NOTE]
> 如果你要将标记从 **{Binding}** 转换为 **{x:Bind}**，请注意在 **Mode** 属性默认值方面的差异。
 
> [**x:DefaultBindMode**](https://docs.microsoft.com/windows/uwp/xaml-platform/x-defaultbindmode-attribute) 可用于针对标记树的特定段更改 x:Bind 的默认模式。 所选的模式将应用该元素及其子元素上的任何 x:Bind 表达，不明确指定某个模式作为绑定的一部分。 使用 OneTime 时的性能比使用 OneWay 时更好，因为使用 OneWay 将导致生成更多代码以连接到和处理更改检测。

## <a name="remarks"></a>备注

因为 **{x:Bind}** 使用生成的代码来达成目标，所以它在编译时需使用类型信息。 这意味着你无法绑定到预先不知道类型的属性。 因此，不能将 **{x:Bind}** 与 **DataContext** 属性结合使用，因为它为 **Object** 类型，并且还可能会在运行时出现更改。

在将 **{x:Bind}** 用于数据模板时，需通过设置 **x:DataType** 值来指示要绑定到的类型，如以下示例所示。 你也可以将类型设置为接口或基类类型，然后按需使用转换，从而编写一个完整的表达式。

已编译的绑定取决于代码生成。 因此，如果你在资源字典中使用 **{x:Bind}**，则该资源字典需要有一个代码隐藏类。 有关代码示例，请参阅[带有 {x:Bind}](../data-binding/data-binding-in-depth.md#resource-dictionaries-with-x-bind) 的资源字典。

包含编译绑定的页面和用户控件将在生成的代码中具有“Bindings”属性。 这包括以下方法：

- **Update()** - 此方法将更新所有编译绑定的值。 任何单向/双向绑定都将具有与检测更改挂钩的侦听器。
- **Initialize()** - 如果尚未初始化绑定，则该方法会调用 Update() 初始化绑定
- **StopTracking()** - 此方法将脱钩为单向和双向绑定创建的所有侦听器。 可以 Update() 方法重新初始化这些绑定。

> [!NOTE]
> 从 Windows10 版本 1607 开始，XAML 框架向 Visibility 转换器提供内置布尔值。 转换器将 **true** 映射到 **Visible** 枚举值并将 **false** 映射到 **Collapsed**，以便你可以将 Visibility 属性绑定到布尔值，无需创建转换器。 注意，这不是函数绑定的特点，只是属性绑定。 若要使用内置转换器，你的应用的最低目标 SDK 版本必须为 14393 或更高版本。 当你的应用面向较早版本的 Windows10 时，你无法使用它。 有关目标版本的详细信息，请参阅[版本自适应代码](https://msdn.microsoft.com/windows/uwp/debug-test-perf/version-adaptive-code)。

**提示**如果你需要指定单个花括号为某个值，如[**路径**](https://msdn.microsoft.com/library/windows/apps/br209830)或[**ConverterParameter**](https://msdn.microsoft.com/library/windows/apps/br209827)，它前面加反斜杠： `\{`。 此外，将包含需要转义的括号的整个字符串放在第二组引号中，例如 `ConverterParameter='{Mix}'`。

[**Converter**](https://msdn.microsoft.com/library/windows/apps/br209826)、[**ConverterLanguage**](https://msdn.microsoft.com/library/windows/apps/hh701880) 和 **ConverterLanguage** 都与如下方案相关：将绑定源中的值或类型转换为与绑定目标属性兼容的类型或值。 有关详细信息和相关示例，请参阅[深入了解数据绑定](https://msdn.microsoft.com/library/windows/apps/mt210946)的“数据转换”部分。

**{x:Bind}** 仅为标记扩展，且无法以编程方式创建或处理此类绑定。 有关标记扩展的详细信息，请参阅 [XAML 概述](xaml-overview.md)。

