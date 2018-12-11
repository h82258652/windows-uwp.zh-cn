---
description: 通过计算对一个已定义资源的引用，为任何 XAML 属性提供一个值。 资源在 ResourceDictionary 中定义，而 StaticResource 用法则在 ResourceDictionary 中引用该资源的键。
title: StaticResource 标记扩展
ms.assetid: D50349B5-4588-4EBD-9458-75F629CCC395
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 012827165aaa4067c9844af0491afb77a53c5f50
ms.sourcegitcommit: 8921a9cc0dd3e5665345ae8eca7ab7aeb83ccc6f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/11/2018
ms.locfileid: "8885963"
---
# <a name="staticresource-markup-extension"></a>{StaticResource} 标记扩展


通过计算对一个已定义资源的引用，为任何 XAML 属性提供一个值。 资源在 [**ResourceDictionary**](https://msdn.microsoft.com/library/windows/apps/br208794) 中定义，而 **StaticResource** 用法则在 **ResourceDictionary** 中引用该资源的键。

## <a name="xaml-attribute-usage"></a>XAML 属性使用方法

``` syntax
<object property="{StaticResource key}" .../>
```

## <a name="xaml-values"></a>XAML 值

| 术语 | 说明 |
|------|-------------|
| 键 | 所请求资源的键。 此键最初通过 [**ResourceDictionary**](https://msdn.microsoft.com/library/windows/apps/br208794) 分配。 资源键可以是以 XamlName 语法定义的任何字符串。 |

## <a name="remarks"></a>备注

**StaticResource** 是一种用于获取 XAML 属性值的技术，这些值在资源字典中的其他地方定义。 这些值可以放在资源字典中，因为它们供多个属性值共享，或者因为 XAML 资源字典用作 XAML 打包或重构技术。 一种 XAML 打包技术的示例是一个控件的主题字典。 另一个示例是用于资源回滚的合并资源字典。

**StaticResource** 使用一个参数，该参数用来为所请求的资源指定键。 在 Windows 运行时 XAML 中，资源键始终是一个字符串。 有关最初如何指定资源键的详细信息，请参阅 [x:Key 属性](x-key-attribute.md)。

本主题未介绍 **StaticResource** 解析为资源字典中的项时所遵循的规则。 这些规则取决于引用和资源是否都存在于模板中，以及是否使用了合并的资源字典，等等。 有关如何定义资源和正确使用 [**ResourceDictionary**](https://msdn.microsoft.com/library/windows/apps/br208794) 的详细信息（包括示例代码），请参阅 [ResourceDictionary 和 XAML 资源引用](https://msdn.microsoft.com/library/windows/apps/mt187273)。

**重要提示** **StaticResource**不应尝试将引用转发到资源定义在 XAML 文件中按词法进一步。 这样的尝试不受支持。 即使前向引用没有失败，尝试进行这样的引用也会对性能造成不利影响。 为实现最佳效果，请调整你的资源字典的组成，以避免使用前向引用。

尝试为无法解析的键指定 **StaticResource** 会在运行时引发 XAML 分析异常。 设计工具还可能会提供警告或错误。

在 Windows 运行时 XAML 处理器实现中，没有针对 **StaticResource** 功能的后备类表示。 **StaticResource** 专用于 XAML 中。 在代码中最接近的等效体是使用 [**ResourceDictionary**](https://msdn.microsoft.com/library/windows/apps/br208794) 的集合 API，例如调用 [**Contains**](https://msdn.microsoft.com/library/windows/apps/jj635925) 或 [**TryGetValue**](https://msdn.microsoft.com/library/windows/apps/jj603139)。

[{ThemeResource} 标记扩展](themeresource-markup-extension.md)是在其他位置中引用命名资源的类似标记扩展。 不同之处在于 {ThemeResource} 标记扩展能够返回不同的资源，具体取决于处于活动状态的系统主题。 有关详细信息，请参阅 [{ThemeResource} 标记扩展](themeresource-markup-extension.md)。

**StaticResource** 是标记扩展。 当需要将属性值转义为除文字值或处理程序名称之外的值时，以及当需求更具全局性而不是仅仅将类型转换器放在某些类型或属性上时，通常需要实现标记扩展。 XAML 中的所有标记扩展在其属性语法中都使用“\{”和“\}”字符，通过此约定，XAML 处理器可以知道标记扩展必须处理属性。

### <a name="an-example-staticresource-usage"></a>示例 {StaticResource} 用法

以下示例 XAML 摘自 [XAML 数据绑定示例](http://go.microsoft.com/fwlink/p/?linkid=226854)。

```xml
<StackPanel Margin="5">
    <!-- Add converter as a resource to reference it from a Binding. --> 
    <StackPanel.Resources>
        <local:S2Formatter x:Key="GradeConverter"/>
    </StackPanel.Resources>
    <TextBlock Style="{StaticResource BasicTextStyle}" Text="Percent grade:" Margin="5" />
    <Slider x:Name="sliderValueConverter" Minimum="1" Maximum="100" Value="70" Margin="5"/>
    <TextBlock Style="{StaticResource BasicTextStyle}" Text="Letter grade:" Margin="5"/>
    <TextBox x:Name="tbValueConverterDataBound"
      Text="{Binding ElementName=sliderValueConverter, Path=Value, Mode=OneWay,  
        Converter={StaticResource GradeConverter}}" Margin="5" Width="150"/> 
</StackPanel> 
```

这个特定示例创建一个由自定义类支持的对象，并将它作为 [**ResourceDictionary**](https://msdn.microsoft.com/library/windows/apps/br208794) 中的一个资源。 为了使此 `local:S2Formatter` 元素成为有效的资源，它还必须具有一个 **x:Key** 属性值。 该属性的值设置为“GradeConverter”。

随后会将该资源进一步请求到 XAML 中，在 XAML 中可以看到 `{StaticResource GradeConverter}`。

请注意 {StaticResource} 标记扩展用法如何设置另一个标记扩展 [{Binding} 标记扩展](binding-markup-extension.md)的属性，以便在这里有两个嵌套标记扩展用法。 内部嵌套将先求值，以便该资源首先获取并且可以用作一个值。 此相同示例也显示在 {Binding} 标记扩展中。

## <a name="design-time-tools-support-for-the-staticresource-markup-extension"></a>设计时工具支持 **{StaticResource}** 标记扩展

当你在 XAML 页面中使用 **{StaticResource}** 标记扩展时，Microsoft Visual Studio2013 可以在 Microsoft IntelliSense 下拉菜单中包含可能的键值。 例如，只要键入“{StaticResource”，来自当前查找作用域的任何资源键就会显示在 IntelliSense 下拉菜单中。 除了你在页面级别 ([**FrameworkElement.Resources**](https://msdn.microsoft.com/library/windows/apps/br208740)) 和应用级别 ([**Application.Resources**](https://msdn.microsoft.com/library/windows/apps/br242338)) 上具有的典型资源外，你还可以查看 [XAML 主题资源](https://msdn.microsoft.com/library/windows/apps/mt187274)以及你的项目正在使用的任何扩展中的资源。

在资源键作为任何 **{StaticResource}** 用法的一部分存在后，“转至定义”****(F12) 功能可以解析该资源并向你显示其定义所在的字典。 若要获取主题资源，请转到设计时 generic.xaml。

## <a name="related-topics"></a>相关主题

* [ResourceDictionary 和 XAML 资源引用](https://msdn.microsoft.com/library/windows/apps/mt187273)
* [**ResourceDictionary**](https://msdn.microsoft.com/library/windows/apps/br208794)
* [x:Key 特性](x-key-attribute.md)
* [{ThemeResource} 标记扩展](themeresource-markup-extension.md)

