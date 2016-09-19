---
author: jwmsft
description: "XAML 名称范围存储 XAML 定义的对象名称和它们的对等实例之间的关系。 此概念类似于其他编程语言和技术中的术语“名称范围”的更广泛的含义。"
title: "XAML 名称范围"
ms.assetid: EB060CBD-A589-475E-B83D-B24068B54C21
translationtype: Human Translation
ms.sourcegitcommit: 98b9bca2528c041d2fdfc6a0adead321737932b4
ms.openlocfilehash: 34ef0bf246abe49a5e19adef66bddda7004a3441

---

# XAML 名称范围

\[ 已针对 Windows 10 上的 UWP 应用更新。 有关 Windows 8.x 的文章，请参阅[存档](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

*XAML 名称范围*存储 XAML 定义的对象名称和它们的对等实例之间的关系。 此概念类似于其他编程语言和技术中的术语*名称范围*的更广泛的含义。

## 定义 XAML 名称范围的方式

XAML 名称范围中的名称使用户代码能够引用最初在 XAML 中声明的对象。 分析 XAML 的内部结果是，运行时创建一组对象，保留这些对象在 XAML 声明中拥有的部分或所有关系。 这些关系作为所创建对象的特定对象属性来维护，或者向编程模型 API 中的实用工具方法公开。

对于 XAML 名称范围中的名称，最典型的用途是作为对象实例的直接引用，由标记编译过程以一种项目生成操作的形式，结合分部类模板中生成的 **InitializeComponent** 方法来实现。

你也可以在运行时自行使用实用工具方法 [**FindName**](https://msdn.microsoft.com/library/windows/apps/br208715) 返回对象的引用，该对象使用 XAML 标记中的名称定义。

### 有关生成操作和 XAML 的详细信息

从技术上讲，所发生的事情是，在 XAML 和它为代码隐藏定义的分部类一起编译时，XAML 本身也会经历标记编译器过程。 每个在标记中定义了 **Name** 或 [x:Name 属性](x-name-attribute.md)的对象元素都会生成一个内部字段，该字段的名称与 XAML 名称相匹配。 此字段最初没有内容。 然后，该类生成一个 **InitializeComponent** 方法，只有在加载了所有 XAML 之后才会调用该方法。 在 **InitializeComponent** 逻辑内，然后会向每个内部字段填充每个等效名称字符串的 [**FindName**](https://msdn.microsoft.com/library/windows/apps/br208715) 返回值。 要自行查看此基础结构，可以在编译后查看 Windows 运行时应用项目的 /obj 子文件夹中为每个 XAML 页面创建的“.g”（生成的）文件。 如果反射你最终的程序集或检查它们的接口语言内容，也可以看到字段和 **InitializeComponent** 方法是这些结果程序集的成员。

**注意** 特别是对于 Visual C++ 组件扩展 (C++/CX) 应用，不会为 XAML 文件的根元素创建 **x:Name** 引用的支持字段。 如果你需要从 C++/CX 代码隐藏来引用根对象，请使用其他 API 或树形遍历。 例如，你可以为已知的命名子元素调用 [**FindName**](https://msdn.microsoft.com/library/windows/apps/br208715)，然后调用 [**Parent**](https://msdn.microsoft.com/library/windows/apps/br208739)。

## 在运行时使用 XamlReader.Load 创建对象

XAML 也可用作 [**XamlReader.Load**](https://msdn.microsoft.com/library/windows/apps/br228048) 方法的字符串输入，该方法的行为类似于最初的 XAML 源代码分析操作。 **XamlReader.Load** 在运行时创建一个断开连接的新对象树。 然后可将断开连接的树附加到主要对象树上的某个点。 必须显式连接你创建的对象树，无论是通过将它添加到一个内容属性集合（例如 **Children**），还是设置其他某个接受对象值的属性（例如为 [**Fill**](https://msdn.microsoft.com/library/windows/apps/br243378) 属性值加载一个新的 [**ImageBrush**](https://msdn.microsoft.com/library/windows/apps/br210101)）。

### XamlReader.Load 对 XAML 名称范围的意义

[**XamlReader.Load**](https://msdn.microsoft.com/library/windows/apps/br228048) 创建的新对象树所定义的初步 XAML 名称范围会在所提供的 XAML 中计算任何已定义的名称，以确定其是否唯一。 如果所提供的 XAML 中的名称此时在内部不是唯一的，**XamlReader.Load** 会抛出一个异常。 如果或当断开连接的对象树连接到主要应用程序对象树时，它不会尝试将它的 XAML 名称范围与主要应用程序 XAML 名称范围合并。 连接树后，你的应用有一个统一的对象树，但该树中具有离散 XAML 名称范围。 这种分歧发生在对象之间的连接点上，你在这些连接点将一个属性设置为从一个 **XamlReader.Load** 调用返回的值。

拥有离散且断开连接的 XAML 名称范围的复杂性在于，调用 [**FindName**](https://msdn.microsoft.com/library/windows/apps/br208715) 方法以及直接管理的对象引用不再针对统一的 XAML 名称范围执行。 对其调用 **FindName** 的特定对象将指定范围，该范围就是调用对象所在的 XAML 名称范围。 在直接管理的对象引用情况中，该范围由代码所在的类指定。 通常，用于一个应用内容“页面”的运行时交互的代码隐藏位于支持根“页面”的分部类中，因此 XAML 名称范围是根 XAML 名称范围。

如果调用 [**FindName**](https://msdn.microsoft.com/library/windows/apps/br208715) 获得根 XAML 名称范围中的一个命名对象，该方法不会找到来自 [**XamlReader.Load**](https://msdn.microsoft.com/library/windows/apps/br228048) 创建的离散 XAML 名称范围的对象。 相反，如果调用的 **FindName** 来自从该离散 XAML 名称范围获得的对象，该方法不会找到根 XAML 名称范围中的命名对象。

这个离散 XAML 名称范围问题只会影响在使用 [**FindName**](https://msdn.microsoft.com/library/windows/apps/br208715) 调用时按 XAML 名称范围中的名称查找对象的操作。

要获得在不同 XAML 名称范围中定义的对象的引用，你可以使用多种技术：

-   使用 [**Parent**](https://msdn.microsoft.com/library/windows/apps/br208739) 和/或已知存在于你的对象树结构中的集合属性（例如 [**Panel.Children**](https://msdn.microsoft.com/library/windows/apps/br227514) 返回的集合）在离散的步骤中遍历整个树。
-   如果从一个离散 XAML 名称范围调用并且希望使用根 XAML 名称范围，始终可轻松获得当前显示的主要窗口的引用。 只需使用一行包含调用 `Window.Current.Content` 的代码，即可获得当前应用程序窗口的可视根（根 XAML 元素，也称为内容源）。 然后可转换为 [**FrameworkElement**](https://msdn.microsoft.com/library/windows/apps/br208706) 并从此范围调用 [**FindName**](https://msdn.microsoft.com/library/windows/apps/br208715)。
-   如果从根 XAML 名称范围调用并且希望一个离散 XAML 名称范围中的对象，最好在你的代码中提前计划，保留对 [**XamlReader.Load**](https://msdn.microsoft.com/library/windows/apps/br228048) 返回并随后添加到主要对象树的对象的引用。 此对象现在是一个可在离散 XAML 名称范围中调用 [**FindName**](https://msdn.microsoft.com/library/windows/apps/br208715) 的有效对象。 你可以保持此对象用作全局变量，或者使用方法参数传递它。
-   你可以通过检查可视树来完全避免名称和 XAML 名称范围考虑因素。 [**VisualTreeHelper**](https://msdn.microsoft.com/library/windows/apps/br243038) API 支持单独基于位置和索引，遍历可视树以查找父对象和子集合。

## 模板中的 XAML 名称范围

XAML 中的模板提供了以一种直观方式重用和重新应用内容的能力，但模板可能还包含各种元素，这些元素具有在模板级别定义的名称。 该名称模板可在一个页面中多次使用。 出于此原因，模板定义它们自己的 XAML 名称范围，不依赖于样式或模板所应用到的包含页面。 考虑此示例：

```xml
<Page
  xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation" 
  xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"  >
  <Page.Resources>
    <ControlTemplate x:Key="MyTemplate">
      ....
      <TextBlock x:Name="MyTextBlock" />
    </ControlTemplate>
  </Page.Resources>
  <StackPanel>
    <SomeControl Template="{StaticResource MyTemplate}" />
    <SomeControl Template="{StaticResource MyTemplate}" />
  </StackPanel>
</Page>
```

此处同一个模板被应用于两个不同的控件。 如果模板没有离散 XAML 名称范围，模板中使用的“MyTextBlock”名称将导致名称冲突。 模板的每次实例化都拥有自己的 XAML 名称范围，所以在本例中，每个已实例化模板的 XAML 名称范围将仅包含一个名称。 但是，根 XAML 名称范围不包含来自每个模板的名称。

由于采用了不同的 XAML 名称范围，需要一种不同技术来从一个模板所应用到的页面范围中查找该模板内的指定元素。 无需在对象树中的某个对象上调用 [**FindName**](https://msdn.microsoft.com/library/windows/apps/br208715)，你首先获得已应用了模板的对象，然后调用 [**GetTemplateChild**](https://msdn.microsoft.com/library/windows/apps/br209416)。 如果你是一位控件作者并且正在生成一种约定，其中已应用的模板中一个特定的命名元素是控件本身所定义的一种行为的目标，你可以使用你的控件实现代码中的 **GetTemplateChild** 方法。 **GetTemplateChild** 方法是受保护的，所以只有控件作者能够访问它。 另外，控件作者应该遵守一些约定来命名各部分和为各部分创建模板，并以应用到控件类的属性值的形式报告这些部分。 此技术使重要部分的名称可供那些可能希望应用不同模板的控件用户发现，这将需要替换已命名的部分才能维护控件功能。

## 相关主题

* [XAML 概述](xaml-overview.md)
* [x:Name 属性](x-name-attribute.md)
* [快速入门：控件模板](https://msdn.microsoft.com/library/windows/apps/xaml/hh465374)
* [**XamlReader.Load**](https://msdn.microsoft.com/library/windows/apps/br228048)
* [**FindName**](https://msdn.microsoft.com/library/windows/apps/br208715)
 




<!--HONumber=Aug16_HO3-->


