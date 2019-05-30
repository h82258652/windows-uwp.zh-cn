---
description: 将一个控件模板中的属性值链接到模板控件上的某个其他公开的属性的值。 TemplateBinding 只能在 XAML 中的 ControlTemplate 定义中使用。
title: TemplateBinding 标记扩展
ms.assetid: FDE71086-9D42-4287-89ED-8FBFCDF169DC
ms.date: 10/29/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: e784b14c30222a28a0e10f8ba0bcf96e6c7f9afd
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/29/2019
ms.locfileid: "66372308"
---
# <a name="templatebinding-markup-extension"></a>{TemplateBinding} 标记扩展

将一个控件模板中的属性值链接到模板控件上的某个其他公开的属性的值。 **TemplateBinding** 只能在 XAML 中的 [**ControlTemplate**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ControlTemplate) 定义中使用。

## <a name="xaml-attribute-usage"></a>XAML 属性使用方法

``` syntax
<object propertyName="{TemplateBinding sourceProperty}" .../>
```

## <a name="xaml-attribute-usage-for-setter-property-in-template-or-style"></a>XAML 属性使用方法（针对模板或样式中的 Setter 属性）

``` syntax
<Setter Property="propertyName" Value="{TemplateBinding sourceProperty}" .../>
```

## <a name="xaml-values"></a>XAML 值

| 术语 | 描述 |
|------|-------------|
| propertyName | 在 setter 语法中设置的属性的名称。 这必须是一个依赖属性。 |
| sourceProperty | 存在于模板化的类型上的其他依赖属性的名称。 |

## <a name="remarks"></a>备注

如果你是自定义控件的作者或者要替换现有控件的控件模板，使用 **TemplateBinding** 是如何定义控件模板的基础部分。 有关详细信息，请参阅[快速入门：控件模板](https://docs.microsoft.com/previous-versions/windows/apps/hh465374(v=win.10))。

*propertyName* 和 *targetProperty* 常常使用相同的属性名称。 在此情况下，一个控件可以在自身定义一个属性，并将该属性转发到其组件部分之一的直观命名的现有属性。 例如，一个控件，其中包含[ **TextBlock** ](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TextBlock)在其组合的情况下，这用于显示控件自身的**文本**属性，可能包括一部分此 XAML在控件模板中： `<TextBlock Text="{TemplateBinding Text}" .... />`

用作源属性值和目标属性值的类型必须匹配。 使用 **TemplateBinding** 时，无法引入转换器。 解析 XAML 时，无法匹配值将导致错误。 如果你需要一个转换器可将详细的语法用于模板绑定，如： `{Binding RelativeSource={RelativeSource TemplatedParent}, Converter="..." ...}`

尝试在 XAML 中的 [**ControlTemplate**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ControlTemplate) 定义外使用 **TemplateBinding** 将导致分析器错误。

在模板化父值还会因为其他绑定而推迟的情况下可以使用 **TemplateBinding**。 **TemplateBinding** 的评估可以等待任何所需运行时绑定都具有值。

**TemplateBinding** 始终是单向绑定。 涉及的这两个属性都必须是依赖属性。

**TemplateBinding** 是标记扩展。 当需要将属性值转义为除文字值或处理程序名称之外的值时，以及当需求更具全局性而不是仅仅将类型转换器放在某些类型或属性上时，通常需要实现标记扩展。 XAML 中的所有标记扩展在其属性语法中都使用“{”和“}”字符，通过此约定，XAML 处理器可以知道标记扩展必须处理属性。

**请注意**  在 Windows 运行时 XAML 处理器实现中，为没有后备类表示形式**TemplateBinding**。 **TemplateBinding** 专用于 XAML 标记中。 无法通过一种简单的方式来重现代码中的行为。

### <a name="xbind-in-controltemplate"></a>在 ControlTemplate 中 x： 绑定

> [!NOTE]
> 在 ControlTemplate 中使用 x： 绑定需要 Windows 10，版本 1809年 ([SDK 17763](https://developer.microsoft.com/windows/downloads/windows-10-sdk)) 或更高版本。 有关目标版本的详细信息，请参阅[版本自适应代码](https://docs.microsoft.com/windows/uwp/debug-test-perf/version-adaptive-code)。

从 Windows 10，版本 1809，您可以使用**X:bind**标记扩展使用的任何地方**TemplateBinding**中[ **ControlTemplate** ](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ControlTemplate). 

[TargetType](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.controltemplate.targettype)属性必需 （不可选） 在[ControlTemplate](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ControlTemplate)时使用**x： 绑定**。

与**X:bind**支持，你可以同时使用[函数绑定](../data-binding/function-bindings.md)中的双向绑定以及[ControlTemplate](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ControlTemplate)。

在此示例中， **TextBlock.Text**属性的值为**Button.Content.ToString**。 在 ControlTemplate TargetType 充当数据源，并完成与 TemplateBinding 到父级相同的结果。

```xaml
<ControlTemplate TargetType="Button">
    <Grid>
        <TextBlock Text="{x:Bind Content, Mode=OneWay}"/>
    </Grid>
</ControlTemplate>
```

## <a name="related-topics"></a>相关主题

* [快速入门：控件模板](https://docs.microsoft.com/previous-versions/windows/apps/hh465374(v=win.10))
* [深入了解数据绑定](https://docs.microsoft.com/windows/uwp/data-binding/data-binding-in-depth)
* [**ControlTemplate**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ControlTemplate)
* [XAML概述](xaml-overview.md)
* [依赖属性概述](dependency-properties-overview.md)
 

