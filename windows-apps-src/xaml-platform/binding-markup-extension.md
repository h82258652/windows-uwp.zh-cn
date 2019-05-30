---
description: Binding 标记扩展会在 XAML 加载时转换为 Binding 类的实例。
title: Binding 标记扩展'
ms.assetid: 3BAFE7B5-AF33-487F-9AD5-BEAFD65D04C3
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: c2d54e9077429c6526168492c31a9d24a43a1be8
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/29/2019
ms.locfileid: "66366537"
---
# <a name="binding-markup-extension"></a>{Binding} 标记扩展


**请注意**  新绑定机制是适用于 Windows 10，针对性能和开发人员工作效率进行了优化。 请参阅 [{x:Bind} 标记扩展](x-bind-markup-extension.md)。

**请注意**  有关如何使用在应用中使用数据绑定的常规信息 **{Binding}** (和之间的全部比较 **{x： 绑定}** 和 **{Binding}** )，请参阅[深度中的数据绑定](https://docs.microsoft.com/windows/uwp/data-binding/data-binding-in-depth)。

**{Binding}** 为来自数据源，如代码的值的控件上的数据绑定属性用于标记扩展。 **{Binding}** 标记扩展会在 XAML 加载时转换为 [**Binding**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Data.Binding) 类的实例。 此绑定对象可获取来自数据源上的某个属性的值，并将其推送到控件上的该属性。 绑定对象可以配置为观察数据源属性值的更改，并基于这些更改自行更新。 该对象也可以配置为将对控件值的更改推送回源属性。 作为数据绑定目标的属性必须是依赖属性。 有关详细信息，请参阅[依赖属性概述](dependency-properties-overview.md)。

**{Binding}** 具有与本地值相同的依赖属性优先级，而在强制性代码中设置本地值将删除在标记中设置的任何 **{Binding}** 。

## <a name="xaml-attribute-usage"></a>XAML 属性使用方法


``` syntax
<object property="{Binding}" .../>
-or-
<object property="{Binding propertyPath}" .../>
-or-
<object property="{Binding bindingProperties}" .../>
-or-
<object property="{Binding propertyPath, bindingProperties}" .../>
```

| 术语 | 描述 |
|------|-------------|
| *propertyPath* | 一个指定绑定的属性路径的字符串。 下面的[属性路径](#property-path)部分中提供了更多信息。 |
| *bindingProperties* | *propName*=*值*\[， *propName*=*值*\]*<br/>使用一个名称/值对语法指定的一个或多个绑定属性。 |
| *propName* | 要在 [**Binding**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Data.Binding) 对象上设置的属性的字符串名称。 例如，“Converter”。 |
| *value* | 要将属性设置为的值。 参数的语法取决于下面的[可使用 {Binding} 设置的 Binding 类的属性](#properties-of-the-binding-class-that-can-be-set-with-binding)部分的属性。 |

## <a name="property-path"></a>属性路径

[**路径**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.data.binding.path)描述要绑定到 （源属性） 的属性。 Path 是位置参数，这意味着可显式使用参数名称 (`{Binding Path=EmployeeID}`)，或者将其指定为第一个未命名的参数 (`{Binding EmployeeID}`)。

[  **Path**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.data.binding.path) 类型为属性路径，其求值结果为自定义类型或框架类型的属性或子属性的字符串。 该类型可能是（但并不一定是）[**DependencyObject**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.DependencyObject)。 属性路径中的步骤由点号 (.) 分隔，并且可包含多个分隔符以遍历连续的子属性。 无论使用何种编程语言，均可将点号分隔符用于实现要绑定到的对象。

例如，若要将 UI 绑定到员工对象的第一个名称属性，属性路径可能是“Employee.FirstName”。 如果将一个项目控件绑定到一个包含员工家属的属性，则属性路径可能是“Employee.Dependents”，并且项目控件的项目模板将负责显示“Dependents”中的项。

如果数据源是一个集合，则属性路径可以按照位置或索引来指定集合中的项目。 例如，"团队\[0\]。播放机"，其中文本"\[\]"包含"0"，指定集合中的第一项。

当使用绑定到现有 [**DependencyObject**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.DependencyObject) 的 [**ElementName**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.data.binding.elementname) 时，你可以使用附加属性作为属性路径的一部分。 若要消除附加属性的多义性，以使附加属性名称中的中间点不被视为属性路径中的一个步骤，可以将所有者限定的附加属性名称放在圆括号中；例如 `(AutomationProperties.Name)`。

属性路径中间对象作为 [**PropertyPath**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.PropertyPath) 对象存储在运行时表示形式中，但大多数情况不需要与代码中的 **PropertyPath** 对象交互。 通常，可以使用 XAML 来指定所需的绑定信息。

有关属性路径的字符串语法、动画功能区域中的属性路径和构造 [**PropertyPath**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.PropertyPath) 对象的详细信息，请参阅 [Property-path 语法](property-path-syntax.md)。

## <a name="properties-of-the-binding-class-that-can-be-set-with-binding"></a>可使用 {Binding} 设置的 Binding 类的属性


**{Binding}** 通过 *bindingProperties* 占位符语法来解释，因为 [**Binding**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Data.Binding) 中存在多个可在标记扩展中进行设置的读/写属性。 这些属性可按任何顺序设置，并带有以逗号分隔的 *propName*=*value* 对。 由于其中一些属性需要不具有类型转换的类型，因此它们需要一些自己嵌套在 **{Binding}** 内的标记扩展。

| 属性 | 描述 |
|----------|-------------|
| [**Path**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.data.binding.path) | 请参阅上面的[属性路径](#property-path)部分。 |
| [**Converter**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.data.binding.converter) | 指定绑定引擎所调用的转换器对象。 可使用 [{StaticResource} 标记扩展](staticresource-markup-extension.md)在标记中设置转换器，以从资源字典引用该对象。 |
| [**ConverterLanguage**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.data.binding.converterlanguage) | 指定转换器要使用的区域性。 (如果要设置[**转换器**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.data.binding.converter)。)作为基于标准的标识符设置的区域性。 有关详细信息，请参阅 [**ConverterLanguage**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.data.binding.converterlanguage) |
| [**ConverterParameter**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.data.binding.converterparameter) | 指定可在转换器逻辑中使用的转换器参数。 (如果要设置[**转换器**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.data.binding.converter)。)多数转换器使用从传递的值将转换，获取所需的所有信息的简单逻辑，而无须**ConverterParameter**值。 **ConverterParameter** 参数适用于具有条件逻辑的更复杂转换器实现，这些逻辑可切断传入 **ConverterParameter** 的内容。 你可以编写一个转换器，使用除字符串之外的值，但这种情况并不常见，请参阅 **ConverterParameter** 中的备注获取详细信息。 |
| [**ElementName**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.data.binding.elementname) | 通过引用同一个 XAML 构造中的另一个元素（具有 **Name** 属性或 [x:Name 属性](x-name-attribute.md)）来指定数据源。 这种方法通常用于共享相关值，或者使用一个 UI 元素的子属性为另一个元素提供特定值，例如在 XAML 控件模板中。 |
| [**FallbackValue**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.data.binding.fallbackvalue) | 指定要在无法解析源或路径时显示的值。 |
| [**Mode**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.data.binding.mode) | 指定绑定模式中，为下列值之一："一次性"、"单向"或"双向"。 这些模式对应于 [**BindingMode**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Data.BindingMode) 枚举的常量名称。 默认值是“OneWay”。 请注意，该值不是 **{x:Bind}** 的默认值，而是“OneTime”。 | 
| [**RelativeSource**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.data.binding.relativesource) | 通过描述绑定源的位置相对于绑定目标的位置指定数据源。 这在 XAML 控件模板内的绑定中最常用。 设置 [{RelativeSource} 标记扩展](relativesource-markup-extension.md)。 |
| [**Source**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.data.binding.source) | 指定对象数据源。 在 **Binding** 标记扩展中，[**Source**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.data.binding.source) 属性需要一个对象引用，例如 [{StaticResource} 标记扩展](staticresource-markup-extension.md)引用。 如果未指定此属性，则操作数据上下文指定来源。 更加常见的做法是不在单个绑定中指定 Source 值，而是依赖于共享的 **DataContext** 进行多个绑定。 有关详细信息，请参阅 [**DataContext**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.datacontext) 或[深入了解数据绑定](https://docs.microsoft.com/windows/uwp/data-binding/data-binding-in-depth)。 |
| [**TargetNullValue**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.data.binding.targetnullvalue) | 指定要在源值解析但并非显式 **null** 时显示的值。 |
| [**UpdateSourceTrigger**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.data.binding.updatesourcetrigger) | 指定绑定源更新的定时。 如果没有指定，则默认值为 **Default**。 |

**请注意**  如果您要转换从标记 **{x： 绑定}** 到 **{Binding}** ，然后注意中的默认值的差异**模式**属性。

[**转换器**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.data.binding.converter)， [ **ConverterLanguage** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.data.binding.converterlanguage)并**ConverterLanguage**所有相关的方案将值转换到或从类型源绑定到类型或兼容的绑定目标属性的值。 有关详细信息和相关示例，请参阅[深入了解数据绑定](https://docs.microsoft.com/windows/uwp/data-binding/data-binding-in-depth)的“数据转换”部分。

> [!NOTE]
> 从 Windows 10 版本 1607 开始，XAML 框架向 Visibility 转换器提供内置布尔值。 转换器将 **true** 映射到 **Visible** 枚举值并将 **false** 映射到 **Collapsed**，以便你可以将 Visibility 属性绑定到布尔值，无需创建转换器。 若要使用内置转换器，你的应用的最低目标 SDK 版本必须为 14393 或更高版本。 当你的应用面向较早版本的 Windows 10 时，你无法使用它。 有关目标版本的详细信息，请参阅[版本自适应代码](https://docs.microsoft.com/windows/uwp/debug-test-perf/version-adaptive-code)。

[**源**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.data.binding.source)， [ **RelativeSource**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.data.binding.relativesource)，以及[ **ElementName** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.data.binding.elementname)指定绑定源，因此它们互相排斥。

**提示**  如果你需要指定单大括号的值，例如在[**路径**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.data.binding.path)或者[ **ConverterParameter** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.data.binding.converterparameter)，然后它前面加反斜杠： `\{`。 此外，将包含需要转义的括号的整个字符串放在第二组引号中，例如 `ConverterParameter='{Mix}'`。

## <a name="examples"></a>示例

```XML
<!-- binding a UI element to a view model -->    
<Page ... >
    <Page.DataContext>
        <local:BookstoreViewModel/>
    </Page.DataContext>

    <GridView ItemsSource="{Binding BookSkus}" SelectedItem="{Binding SelectedBookSku, Mode=TwoWay}" ... />
</Page>
```

```XML
<!-- binding a UI element to another UI element -->
<Page ... >
    <Page.Resources>
        <local:S2Formatter x:Key="GradeConverter"/>
    </Page.Resources>

    <Slider x:Name="sliderValueConverter" ... />
    <TextBox Text="{Binding Path=Value, ElementName=sliderValueConverter,
        Mode=OneWay,
        Converter={StaticResource GradeConverter}}"/>
</Page>
```

第二个示例设置四个不同[**绑定**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Data.Binding)属性：[**ElementName**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.data.binding.elementname)， [**路径**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.data.binding.path)， [**模式**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.data.binding.mode)并[**转换器**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.data.binding.converter). 本例中显示的 **Path** 明确命名为 **Binding** 属性。 **Path** 的求值结果是一个数据绑定源，该绑定源是同一个运行时对象树中的另一个对象（即，名为 `sliderValueConverter` 的 [**Slider**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Slider)）。

请注意 [**Converter**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.data.binding.converter) 属性值如何使用另一个标记扩展 [{StaticResource} 标记扩展](staticresource-markup-extension.md)，以便在这里有两个嵌套标记扩展用法。 内部嵌套先求值，以便在获取资源之后，有一个可以由绑定使用的实际 [**IValueConverter**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Data.IValueConverter)（一个由资源中的 `local:S2Formatter` 元素实例化的自定义类）。

## <a name="tools-support"></a>工具支持

当在 XAML 标记编辑器中创作 **{Binding}** 时，Microsoft Visual Studio 中的 Microsoft IntelliSense 将显示数据上下文的相关属性。 只要你键入“{Binding”，适合 [**Path**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.data.binding.path) 的数据上下文属性便会显示在下拉列表中。 IntelliSense 对 [**Binding**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Data.Binding) 的其他属性也能起到帮助作用。 为实现此操作，你必须在标记页中设置数据上下文或设计时数据上下文。 **Go To Definition** (F12) 也可以与 **{Binding}** 一起使用。 或者，你也可以使用数据绑定对话框。

 
