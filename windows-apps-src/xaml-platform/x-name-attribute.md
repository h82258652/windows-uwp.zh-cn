---
author: jwmsft
description: "唯一标识对象元素，可方便从代码隐藏或一般代码中访问已实例化的对象。"
title: "xName 属性"
ms.assetid: 4FF1F3ED-903A-4305-B2BD-DCD29E0C9E6D
translationtype: Human Translation
ms.sourcegitcommit: ba620bc89265cbe8756947e1531759103c3cafef
ms.openlocfilehash: 442c2fa103e1e968ef47ea990bfe8e166daec88b

---

# x&#58;Name 属性

\[ 已针对 Windows 10 上的 UWP 应用更新。 有关 Windows 8.x 的文章，请参阅[存档](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

唯一标识对象元素，可方便从代码隐藏或一般代码中访问已实例化的对象。 应用于支持的编程模型之后，**x:Name** 可视为等效于持有一个对象引用（由一个构造函数返回）的变量。

## XAML 属性使用方法

``` syntax
<object x:Name="XAMLNameValue".../>
```

## XAML 值

| 术语 | 说明 |
|------|-------------|
| XAMLNameValue | 一个符合 XamlName 语法限制的字符串。 |

##  XamlName 语法

以下是在此 XAML 实现中作为密钥使用的字符串的规范语法：

``` syntax
XamlName ::= NameStartChar (NameChar)*
NameStartChar ::= LetterCharacter | '_'
NameChar ::= NameStartChar | DecimalDigit
LetterCharacter ::= ('a'-'z') | ('A'-'Z')
DecimalDigit ::= '0'-'9'
CombiningCharacter::= none
```

-   字符被限制在较低的 ASCII 范围，具体而言，就是大写和小写罗马字母、数字和下划线 (\_) 字符。
-   不支持 Unicode 字符范围。
-   名称不能以数字开头。 某些工具实现会在用户以数字作为起始字符时在字符串前附加一个下划线 (\_)，工具也可以根据包含数字的其他值自动生成 **x:Name** 值。

## 备注

当处理 XAML 时，指定的 **x:Name** 变成一个在基础代码中创建的字段的名称，该字段持有该对象的引用。 创建此字段的过程由 MSBuild 目标步骤执行，这些步骤还负责连接一个 XAML 文件和它的代码隐藏的分部类。 此行为不一定是由 XAML 语言指定的，它是通用 Windows 平台 (UWP) XAML 编程的特定实现，应用在其编程和应用程序模型中使用 **x:Name**。

每个已定义的 **x:Name** 在一个 XAML 名称范围中必须是唯一的。 通常，XAML 名称范围在所加载页面的根元素级别定义，其中包含单个 XAML 页面中该元素下面的所有元素。 其他 XAML 名称范围由在该页面上定义的任何控件模板或数据模板定义。 在运行时，其他 XAML 名称范围是为从所应用的控件模板创建的对象树的根，以及由从 [**XamlReader.Load**](https://msdn.microsoft.com/library/windows/apps/br228048) 调用创建的对象树定义的。 有关详细信息，请参阅 [XAML 空间范围](xaml-namescopes.md)。

在将元素引入设计界面时，设计工具常常会自动为它们生成 **x:Name** 值。 该自动生成架构因使用的设计器不同而不同，但一种典型的架构是生成一个字符串，它以支持该元素的类名开头，后跟一个延长的整数。 例如，如果向设计器引入第一个 [**Button**](https://msdn.microsoft.com/library/windows/apps/br209265) 元素，则可以在 XAML 中看到，此元素拥有 **x:Name** 属性值“Button1”。

无法在 XAML 属性元素语法中或在代码中使用 [**SetValue**](https://msdn.microsoft.com/library/windows/apps/br242361) 设置 **x:Name**。 只能在元素上使用 XAML 属性语法来设置 **x:Name**。

**注意** **x:Name** 引用的支持字段是专为 C++/CX 应用创建的，而不是为 XAML 文件或页面的根元素创建的。 如果你需要从 C++ 代码隐藏来引用根对象，请使用其他 API 或树形遍历。 例如，你可以为已知的命名子元素调用 [**FindName**](https://msdn.microsoft.com/library/windows/apps/br208715)，然后调用 [**Parent**](https://msdn.microsoft.com/library/windows/apps/br208739)。

### x:Name 和其他 Name 属性

UWP XAML 中使用的一些类型还具有一个名为 **Name** 的属性。 例如，[**FrameworkElement.Name**](https://msdn.microsoft.com/library/windows/apps/br208735) 和 [**TextElement.Name**](https://msdn.microsoft.com/library/windows/apps/hh702125)。

如果 **Name** 可用作一个元素上的可设置属性，**Name** 和 **x:Name** 可在 XAML 中交替使用，但如果在相同元素上指定了这两个属性，会发生错误。 有时，会存在一个只读的 **Name** 属性（如 [**VisualState.Name**](https://msdn.microsoft.com/library/windows/apps/br209031)）。 如果出现这种情况，请在 XAML 中始终使用 **x:Name** 对该元素进行命名，而且对于一些少见的代码方案使用只读的 **Name**。

**注意** [**FrameworkElement.Name**](https://msdn.microsoft.com/library/windows/apps/br208735) 通常不应当用于更改最初由 **x:Name** 设置的值，但这个一般规则有一些例外的场景。 在典型的场景中，XAML 名称范围的创建和定义是一个 XAML 处理器操作。 在运行时修改 **FrameworkElement.Name** 可能会导致不一致的 XAML 名称范围/专用字段命名对齐，这种不一致在代码隐藏文件中很难跟踪。

### x:Name 和 x:Key

**x:Name** 可以作为一个属性应用到 [**ResourceDictionary**](https://msdn.microsoft.com/library/windows/apps/br208794) 内的元素，以充当 [x:Key 属性](x-key-attribute.md)的替代属性。 （通常，要求 **ResourceDictionary** 中的所有元素都必须具有一个 x:Key 属性。）这常见于[情节提要动画](https://msdn.microsoft.com/library/windows/apps/mt187354)。 有关详细信息，请参阅 [ResourceDictionary 和 XAML 资源引用](https://msdn.microsoft.com/library/windows/apps/mt187273)部分。




<!--HONumber=Jun16_HO4-->


