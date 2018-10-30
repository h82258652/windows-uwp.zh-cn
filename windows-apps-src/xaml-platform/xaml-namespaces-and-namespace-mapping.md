---
author: jwmsft
description: 本主题将介绍大部分 XAML 文件的根元素中存在的 XML/XAML 命名空间 (xmlns) 映射。 它还将介绍如何为自定义类型和程序集生成类似的映射。
title: XAML 命名空间和命名空间映射
ms.assetid: A19DFF78-E692-47AE-8221-AB5EA9470E8B
ms.author: jimwalk
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: a1aebe3d9aac460d444a5dffcd63142300c022b7
ms.sourcegitcommit: 753e0a7160a88830d9908b446ef0907cc71c64e7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/30/2018
ms.locfileid: "5761029"
---
# <a name="xaml-namespaces-and-namespace-mapping"></a>XAML 命名空间和命名空间映射


本主题介绍大多数 XAML 文件的根元素中用到的 XML/XAML 命名空间 (**xmlns**) 映射。 它还将介绍如何为自定义类型和程序集生成类似的映射。

## <a name="how-xaml-namespaces-relate-to-code-definition-and-type-libraries"></a>XAML 命名空间如何与代码定义和类型库相关

无论是其一般用途还是 Windows 运行时应用编程的应用上，XAML 都用于声明对象、这些对象的属性以及表示为层次结构的对象-属性关系。 你在 XAML 中声明的对象受其他编程技术和语言定义的类型库或其他表示支持。 这些库可能是：

-   Windows 运行时的内置对象集合。 这是一个固定的对象集合，从 XAML 访问这些对象使用内部类型映射和激活逻辑。
-   Microsoft 或第三方提供的分布式库。
-   该库表示你的应用包含的以及你的包重新分发的第三方控件的定义。
-   你自己的库（属于你的项目的一部分），它包含部分或所有用户代码定义。

支持类型信息与特定 XAML 命名空间定义相关联。 XAML 框架（如 Windows 运行时）可聚合多个程序集和多个代码命名空间，以映射到单个 XAML 命名空间。 这就支持涉及更大的编程框架或技术的 XAML 词汇表概念。 XAML 词汇表可能非常大，例如该引用中为 Windows 运行时应用记录的大部分 XAML 构成单个 XAML 词汇表。 XAML 词汇表也可扩展：通过向支持代码定义添加类型来扩展它，从而确保在代码命名空间（已经用作 XAML 词汇表的映射的命名空间来源）中包含这些类型。

XAML 处理器在创建运行时对象表示时，可查找与该 XAML 命名空间关联的支持程序集的类型和成员。 出于此原因，XAML 可用作一种形式化和交换对象构造定义行为的方式，并且 XAML 可用作 UWP 应用的 UI 定义技术。

## <a name="xaml-namespaces-in-typical-xaml-markup-usage"></a>典型 XAML 标记中使用的 XAML 命名空间

XAML 文件几乎总是在其根元素中声明一个默认 XAML 命名空间。 默认 XAML 命名空间定义了无需使用前缀来限定即可声明哪些元素。 例如，如果声明一个元素 `<Balloon />`，XAML 分析器期望一个 **Balloon** 元素存在并且在默认的 XAML 命名空间中是有效的。 相反，如果 **Balloon** 不在已定义的默认 XAML 命名空间中，就必须使用一个前缀限定该元素名称，例如 `<party:Balloon />`。 该前缀表明该元素存在于与默认命名空间不同的 XAML 命名空间中，必须将一个 XAML 命名空间映射到前缀 **party**，然后才能使用此元素。 XAML 命名空间适用于在其中声明它们的特定元素，也适用于该元素在 XAML 结构中包含的任何元素。 出于此原因，XAML 命名空间几乎总是在 XAML 文件的根元素上声明，以充分利用这种继承性。

## <a name="the-default-and-xaml-language-xaml-namespace-declarations"></a>默认和 XAML 语言 XAML 命名空间声明

在大多数 XAML 文件的根元素中，有两个 **xmlns** 声明。 第一个声明将一个 XAML 命名空间映射为默认命名空间： `xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"`

这是多个也使用 XAML 作为 UI 定义标记格式的预处理器 Microsoft 技术中使用的相同 XAML 命名空间标识符。 使用相同的标识符是经过深思熟虑的，在将以前定义的 UI 迁移到使用 C++、C# 或 Visual Basic 的 Windows 运行时应用时很有用。

第二个声明映射 XAML 定义的语言元素的一个独立的 XAML 命名空间，（通常）将它映射到“x:”前缀： `xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"`

此 **xmlns** 值和它所映射到的“x:”前缀对于在多个使用 XAML 的前置任务 Microsoft 技术中使用的定义也是相同的。

这些声明之间的关系是，XAML 是一种语言定义，Windows 运行时是一种实现，它使用 XAML 作为语言并定义一个特定的词汇表，它的类型在这里供 XAML 引用。

XAML 语言指定某些语言元素，其中每个元素应可通过适用于 XAML 命名空间的 XAML 处理器实现进行访问。 项目模板、示例代码和语言特性文档遵循 XAML 语言 XAML 命名空间的“x:”映射约定。 XAML 语言命名空间定义多个常用的功能，甚至对于使用 C++、C# 或 Visual Basic 的基本 Windows 运行时应用，这些功能也是必要的。 例如，为了将任何代码隐藏通过分部类联接到 XAML 文件，必须将该类命名为相关 XAML 文件的根元素中的 [x:Class 属性](x-class-attribute.md)。 或者，任何在 XAML 页面中定义为 [ResourceDictionary 和 XAML 资源引用](https://msdn.microsoft.com/library/windows/apps/mt187273)中一个键资源的元素必须在相关的对象元素上设置 [x:Key 特性](x-key-attribute.md)。

<span id="other-XAML-namespaces"/>

## <a name="other-xaml-namespaces"></a>其他 XAML 命名空间

除了默认命名空间和 XAML 语言 XAML 命名空间“x:”，你也可能在 Microsoft Visual Studio 生成的应用的初始默认 XAML 中看到其他的已映射 XAML 命名空间。

### **<a name="d-httpschemasmicrosoftcomexpressionblend2008"></a>d: (`http://schemas.microsoft.com/expression/blend/2008`)**

“d:”XAML 命名空间旨在提供设计器支持，尤其是 Microsoft Visual Studio 的 XAML 设计界面中的设计器支持。 “d:”XAML 命名空间支持 XAML 元素上的设计器或设计时特性。 这些设计器特性只影响 XAML 行为的设计方面。 如果 Windows 运行时 XAML 分析器在一个应用运行时加载相同的 XAML，设计器特性会被忽略。 一般而言，设计器特性在任何 XAML 元素上是有效的，但在实际情况中，只有某些场景适合应用设计器特性。 具体来讲，许多设计器特性是为了在你开发使用数据绑定的 XAML 和代码时，提供一种与数据上下文和数据源交互的更好体验。

-   **d:DesignHeight 和 d:DesignWidth 属性：** 这些属性有时应用于 Visual Studio 或其他 XAML 设计器图面为你创建的 XAML 文件的根。 例如，如果你向应用项目中添加了新的 **UserControl**，则这些属性是针对所创建的 XAML 的 [**UserControl**](https://msdn.microsoft.com/library/windows/apps/br227647) 根设置的。 这些属性更便于设计 XAML 内容组合，以便在将该 XAML 内容用于控件示例或更大 UI 页面的其他部分之后，你能够预测可能存在的布局约束。

   **注意**如果你要从 Microsoft Silverlight 迁移 XAML 在代表整个 UI 页面的根元素上可能存在这些属性。 在这种情况下，你可能希望删除这些特性。 与使用 **d:DesignHeight** 和 **d:DesignWidth** 的固定大小页面布局相比，XAML 设计器的其他功能（如模拟器）对于设计能够很好地处理缩放和视图状态的页面布局或许更有用。

-   **d:DataContext 特性：** 可以针对页面根或控件设置此特性，以便替代该对象所拥有的任何显式或继承的 [**DataContext**](https://msdn.microsoft.com/library/windows/apps/br208713)。
-   **d:DesignSource 特性：** 为 [**CollectionViewSource**](https://msdn.microsoft.com/library/windows/apps/br209833) 指定设计时数据源，并替代 [**Source**](https://msdn.microsoft.com/library/windows/apps/br209835)。
-   **d:DesignInstance 和 d:DesignData 标记扩展：** 这些标记扩展用于为 **d:DataContext** 或 **d:DesignSource** 提供设计时数据资源。 在这里，我们不会完全记录如何使用设计时数据源资源。 有关详细信息，请参阅[设计时特性](http://go.microsoft.com/fwlink/p/?LinkId=272504)。 有关一些使用示例，请参阅[设计面图以及用于原型制作的示例数据](https://msdn.microsoft.com/library/windows/apps/mt517866)。

### **<a name="mc-httpschemasopenxmlformatsorgmarkup-compatibility2006"></a>mc: (`http://schemas.openxmlformats.org/markup-compatibility/2006`)**

“mc:”表示并支持读取 XAML 的标记兼容性模式。 通常，“d:”前缀与特性 **mc:Ignorable** 相关联。 此技术使运行时 XAML 分析器忽略“d:”中的设计特性。

### <a name="local-and-common"></a>**local:** 和 **common:**

“local:”是一个前缀，通常会在模板化 UWP 应用项目的 XAML 页面中为你映射它。 它映射为引用相同的命名空间，该命名空间旨在包含 [x:Class 特性](x-class-attribute.md)和所有 XAML 文件（包括 app.xaml）的代码。 只要你在此相同命名空间中定义你要在 XAML 中使用的任何自定义类，你就可以使用 **local:** 前缀在 XAML 中引用你的自定义类型。 来自模板化的 UWP 应用项目的相关前缀是 **common:**。 此前缀引用包含实用程序类（例如转换器和命令）的嵌套“Common”命名空间，你可以在**解决方案资源管理器**视图的“Common”文件夹中找到定义。

### **<a name="vsm"></a>vsm:**

请勿使用。 “vsm:”是有时在从其他 Microsoft 技术导入的较老 XAML 模板中会看到的一个前缀。 该命名空间最初解决了旧版命名空间工具问题。 你应该在用于 Windows 运行时的任何 XAML 中删除“vsm:”的 XAML 命名空间定义，更改 [**VisualState**](https://msdn.microsoft.com/library/windows/apps/br209007)、[**VisualStateGroup**](https://msdn.microsoft.com/library/windows/apps/br209014) 和相关对象的任何前缀的用法，从而使用默认的 XAML 命名空间。 有关 XAML 迁移的详细信息，请参阅[将 Silverlight 或 WPF XAML/代码迁移到 Windows 运行时应用](https://msdn.microsoft.com/library/windows/apps/br229571)。

## <a name="mapping-custom-types-to-xaml-namespaces-and-prefixes"></a>将自定义类型映射到 XAML 命名空间和前缀

你可以映射一个 XAML 命名空间，这样可使用 XAML 访问你自己的自定义类型。 换句话说，你正在映射一个代码命名空间，因为它存在于一个定义了自定义类型的代码表示中，为它分配一个 XAML 命名空间以及一个前缀供其使用。 针对 XAML 的自定义类型可在 Microsoft .NET 语言（C# 或 Microsoft Visual Basic）或 C++ 中定义。 映射通过定义一个 **xmlns** 前缀来执行。 例如，`xmlns:myTypes` 定义一个新 XAML 命名空间，通过在所有用法中添加令牌 `myTypes:` 作为前缀来访问这个命名空间。

**xmlns** 定义包含一个值以及前缀命名。 该值是一个包含在引号内的字符串，后跟一个等号。 一种常见的 XML 约定是将 XML 命名空间与一个统一资源标识符 (URI) 相关联，这样就实现了唯一性和标识约定。 你也可以在默认 XAML 命名空间和 XAML 语言 XAML 命名空间中看到此约定，也可以在 Windows 运行时 XAML 所使用的且不太常见的 XAML 命名空间中看到此约定。 对于映射自定义类型（而不是指定一个 URI）的 XAML 命名空间，你可以为定义添加令牌“using:”作为前缀。 在“using:”令牌后，可命名代码命名空间。

例如，要映射一个允许你引用“CustomClasses”命名空间的“custom1”前缀，并使用来自该命名空间或程序集的类作为 XAML 中的前缀，你的 XAML 页面应在根元素上包含以下映射： `xmlns:custom1="using:CustomClasses"`

不需要映射同一页面范围中的分部类。 例如，不需要前缀即可引用你为处理来自页面的 XAML UI 定义的事件而定义的任何事件处理程序。 另外，Visual Studio 生成的使用 C++、C# 或 Visual Basic 的 Windows 运行时应用项目的许多起始 XAML 页面已映射“local:”前缀，它引用项目指定的默认命名空间和分部类定义所使用的命名空间。

### <a name="clr-language-rules"></a>CLR 语言规则

如果使用 .NET 语言（C# 或 Microsoft Visual Basic）编写支持代码，你可能会在命名空间名称中使用一个点（“.”）的约定，以创建代码命名空间的概念性层次结构。 如果命名空间定义包含一个点，则这个点应该是你在“using:”令牌之后指定的值的一部分。

如果代码隐藏文件或代码定义文件是 C++ 文件，那么某些约定仍然遵守公共语言运行时 (CLR) 语言形式，因此在 XAML 语法上没有区别。 如果在 C++ 中声明嵌套的命名空间，则在指定“using:”令牌后的值时，连续的嵌套命名空间字符串之间的分隔符也应是一个“.”，而不是“::”。

当你定义代码以供使用 XAML 时，请勿使用嵌套类型（例如在某个类中嵌套枚举）。 无法评估嵌套类型。 XAML 分析器无法区分某个点是嵌套类型名称的一部分，而不是命名空间名称的一部分。

## <a name="custom-types-and-assemblies"></a>自定义类型和程序集

定义 XAML 命名空间的支持类型的程序集名称不是在映射中指定的。 关于哪些程序集可用的逻辑在应用定义级别控制，包含在基本应用部署和安全原则中。 在项目设置中，将你希望作为 XAML 的一个代码定义源包含的任何程序集声明为一个独立程序集。 有关详细信息，请参阅[在 C# 和 Visual Basic 中创建 Windows 运行时组件](https://msdn.microsoft.com/library/windows/apps/xaml/hh441572.aspx)。

如果从主要应用的应用程序定义或页面定义中引用自定义类型，这些类型无需进一步的依赖程序集配置即可使用，但你仍然必须映射包含这些类型的代码命名空间。 一种常见的约定是映射任何给定 XAML 页面的默认代码命名空间的前缀“local”。 此约定常常包含在 XAML 项目的初始项目模板中。

## <a name="attached-properties"></a>附加属性

如果你引用附加属性，附加属性名称的所有者键入部分必须在默认 XAML 命名空间中，或者必须带有前缀。 很少会独立于属性元素向属性添加前缀，但这种情况有时是必需的，特别是对于自定义附加属性而言。 有关详细信息，请参阅[自定义附加属性](custom-attached-properties.md)。

## <a name="related-topics"></a>相关主题

* [XAML 概述](xaml-overview.md)
* [XAML 语法指南](xaml-syntax-guide.md)
* [在 C# 和 Visual Basic 中创建 Windows 运行时组件](https://msdn.microsoft.com/library/windows/apps/xaml/hh441572.aspx)
* [适用于 Windows 运行时应用的 C#、VB 和 C++ 项目模板](https://msdn.microsoft.com/library/windows/apps/hh768232)
* [将 Silverlight 或 WPF XAML/代码迁移到 Windows 运行时应用](https://msdn.microsoft.com/library/windows/apps/br229571)
 

