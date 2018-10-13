---
author: jwmsft
description: 将一个控件模板中的属性值链接到模板控件上的某个其他公开的属性的值。 TemplateBinding 只能在 XAML 中的 ControlTemplate 定义中使用。
title: TemplateBinding 标记扩展
ms.assetid: FDE71086-9D42-4287-89ED-8FBFCDF169DC
ms.author: jimwalk
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 8c1812adc9d5610fffd6f9d275b4e093a4fa96e6
ms.sourcegitcommit: d10fb9eb5f75f2d10e1c543a177402b50fe4019e
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/12/2018
ms.locfileid: "4570256"
---
# <a name="templatebinding-markup-extension"></a>{TemplateBinding} 标记扩展


将一个控件模板中的属性值链接到模板控件上的某个其他公开的属性的值。 **TemplateBinding** 只能在 XAML 中的 [**ControlTemplate**](https://msdn.microsoft.com/library/windows/apps/br209391) 定义中使用。

## <a name="xaml-attribute-usage"></a>XAML 属性使用方法

``` syntax
<object propertyName="{TemplateBinding sourceProperty}" .../>
```

## <a name="xaml-attribute-usage-for-setter-property-in-template-or-style"></a>XAML 属性使用方法（针对模板或样式中的 Setter 属性）

``` syntax
<Setter Property="propertyName" Value="{TemplateBinding sourceProperty}" .../>
```

## <a name="xaml-values"></a>XAML 值

| 术语 | 说明 |
|------|-------------|
| propertyName | 在 setter 语法中设置的属性的名称。 这必须是一个依赖属性。 |
| sourceProperty | 存在于模板化的类型上的其他依赖属性的名称。 |

## <a name="remarks"></a>备注

如果你是自定义控件的作者或者要替换现有控件的控件模板，使用 **TemplateBinding** 是如何定义控件模板的基础部分。 有关详细信息，请参阅[快速入门：控件模板](https://msdn.microsoft.com/library/windows/apps/xaml/hh465374)。

*propertyName* 和 *targetProperty* 常常使用相同的属性名称。 在此情况下，一个控件可以在自身定义一个属性，并将该属性转发到其组件部分之一的直观命名的现有属性。 例如，一个在其结构中合并了 [**TextBlock**](https://msdn.microsoft.com/library/windows/apps/br209652) 的控件（用于显示控件自己的 **Text** 属性）可能包含此 XAML 作为控件模板的一部分： `<TextBlock Text="{TemplateBinding Text}" .... />`

用作源属性值和目标属性值的类型必须匹配。 使用 **TemplateBinding** 时，无法引入转换器。 解析 XAML 时，无法匹配值将导致错误。 如果你需要一个可将详细语法用于模板绑定的转换器，例如： `{Binding RelativeSource={RelativeSource TemplatedParent}, Converter="..." ...}`

尝试在 XAML 中的 [**ControlTemplate**](https://msdn.microsoft.com/library/windows/apps/br209391) 定义外使用 **TemplateBinding** 将导致分析器错误。

在模板化父值还会因为其他绑定而推迟的情况下可以使用 **TemplateBinding**。 **TemplateBinding** 的评估可以等待任何所需运行时绑定都具有值。

**TemplateBinding** 始终是单向绑定。 涉及的这两个属性都必须是依赖属性。

**TemplateBinding** 是标记扩展。 当需要将属性值转义为除文字值或处理程序名称之外的值时，以及当需求更具全局性而不是仅仅将类型转换器放在某些类型或属性上时，通常需要实现标记扩展。 XAML 中的所有标记扩展在其属性语法中都使用“{”和“}”字符，通过此约定，XAML 处理器可以知道标记扩展必须处理属性。

**注意** 在 Windows 运行时 XAML 处理器实现中，**TemplateBinding** 没有支持类表示。 **TemplateBinding** 专用于 XAML 标记中。 无法通过一种简单的方式来重现代码中的行为。

### <a name="xbind-in-controltemplate"></a>X:bind ControlTemplate 中

从开始到 Windows 10 的下一个主要更新，你可以使用**X:bind**标记扩展在[**ControlTemplate**](https://msdn.microsoft.com/library/windows/apps/br209391)中使用**TemplateBinding**的任意位置。 

[TargetType](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.controltemplate.targettype#Windows_UI_Xaml_Controls_ControlTemplate_TargetType)属性将需要 （不是可选的） 上[ControlTemplate](https://msdn.microsoft.com/library/windows/apps/br209391)使用**X:bind**时。

通过**X:bind**支持，你现在可以使用这两个[函数绑定](../data-binding/function-bindings.md) [ControlTemplate](https://msdn.microsoft.com/library/windows/apps/br209391)中的良好为双向绑定

在以下示例中，TextBlock.Text 评估为 Button.Content.ToString()。 在该 ControlTemplate TargetType 充当数据源，并完成与到父 TemplateBinding 相同的结果。

```xaml
<ControlTemplate TargetType="Button">
    <Grid>
        <TextBlock Text="{x:Bind Content}" />
    </Grid>
</ControlTemplate>
```

## <a name="related-topics"></a>相关主题

* [快速入门：控件模板](https://msdn.microsoft.com/library/windows/apps/xaml/hh465374)
* [深入了解数据绑定](https://msdn.microsoft.com/library/windows/apps/mt210946)
* [**ControlTemplate**](https://msdn.microsoft.com/library/windows/apps/br209391)
* [XAML 概述](xaml-overview.md)
* [依赖属性概述](dependency-properties-overview.md)
 

