---
author: jwmsft
ms.assetid: 9899F6A0-7EDD-4988-A76E-79D7C0C58126
title: 通用 Windows 平台组件和优化互操作
description: 创建使用 UWP 组件的通用 Windows 平台 (UWP) 应用并在本机和托管类型之间进行互操作，同时避免互操作性能问题。
ms.author: jimwalk
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 726dc4aaa34b9b68aa198e236abcef57b78b21f4
ms.sourcegitcommit: ca96031debe1e76d4501621a7680079244ef1c60
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/31/2018
ms.locfileid: "5839838"
---
# <a name="uwp-components-and-optimizing-interop"></a>UWP 组件和优化互操作


创建使用 UWP 组件的通用 Windows 平台 (UWP) 应用并在本机和托管类型之间进行互操作，同时避免互操作性能问题。

## <a name="best-practices-for-interoperability-with-uwp-components"></a>UWP 组件互操作性的最佳做法

如果你不够细心，使用 UWP 组件可对你的应用性能造成很大的影响。 本章节讨论了在你的应用使用 UWP 组件时如何获取良好的性能。

### <a name="introduction"></a>介绍

互操作性 可对性能造成很大的影响，你可能在使用互操作性的时甚至都未意识到你正在使用它。 UWP 将为你处理大量互操作，以便你提高效率，和重复使用以其他语言编写的代码。 我们鼓励你利用 UWP 为你所做的一切，但请注意，这可能会影响性能。 本节内容讨论了你为减少互操作对应用性能的影响而采取的行动。

UWP 具有一个类型库，可通过编写 UWP 应用的任何语言来访问这些类型。 你可在 C# 或 Microsoft Visual Basic 中使用 UWP 类型，同样，你也可使用 .NET 对象。 你无需让平台使用方法调用来访问 UWP 组件。 这会让编写应用变得没那么复杂，但请务必知道，可能会发生比你预期更多的互操作。 如果使用 C# 或 Visual Basic 以外的语言来编写 UWP 组件，在你使用此组件时可跨越互操作性边界。 跨域互操作性边界可影响应用的性能。

当你使用 C# 或 Visual Basic 开发 UWP 应用时，你所使用的两个最常见的 API 组合是 UWP API 和 UWP 应用的 .NET API。 通常，在 UWP 中定义的类型位于以“Windows”开头的命名空间中。 而 .NET 类型位于以“System”开头的命名空间中。 但是，存在例外情况。 UWP 应用的 .NET 中的类型在使用时不要求互操作性。 如果你发现在使用 UWP 的区域出现较差性能，你可以使用适用于 UWP 应用的 .NET 来获取更佳性能。

**注意**与 windows 10 交付的 UWP 组件的大多数 c + + 实现，因此你从 C# 或 Visual Basic 使用它们时可跨越互操作性边界。 如往常一样，在更改代码前，请务必监测你的应用，确认使用 UWP 组件是否影响你的应用性能。

在本主题中，我们提及“UWP 组件”时，我们是指以 C# 或 Visual Basic 以外的语言编写的组件。

 

每次你访问 UWP 组件的属性或调用其方法时，都会产生互操作成本。 实际上，创建 UWP 组件的成本比创建 .NET 对象的成本更高。 原因是 UWP 必须执行从你的应用语言转换为组件语言的代码。 此外，如果你将数据传递给该组件，数据必须在已管理和未管理类型中转换。

### <a name="using-uwp-components-efficiently"></a>高效地使用 UWP 组件

如果你发现你需要获得更佳的性能，你可确保你的代码尽可能高效地使用 UWP 组件。 本节内容讨论了在使用 UWP 组件时的一些改善性能的提示。

要发现性能影响，需在短时间内产生大量调用。 一个设计完好的应用程序（封装了业务逻辑和其他托管代码中对 UWP 组件的调用）不应产生巨额的互操作成本。 但如果你的测试表明，使用 UWP 组件确实影响你的应用性能，本节内容所讨论的提示将有助你改善性能。

### <a name="consider-using-net-for-uwp-apps"></a>考虑将 .NET 用于 UWP 应用

在某些情况下，你既可以使用 UWP，也可以使用适用于 UWP 应用的 .NET 来完成任务。 最好不要尝试混合使用 .NET 类型和 UWP 类型。 尽量坚持使用其中一种类型或另一种类型。 例如，你既可使用 [**Windows.Data.Xml.Dom.XmlDocument**](https://msdn.microsoft.com/library/windows/apps/BR206173) 类型（UWP 类型），也可使用 [**System.Xml.XmlReader**](https://msdn.microsoft.com/library/windows/apps/xaml/system.xml.xmlreader.aspx) 类型（.NET 类型）来分析 xml 流。 使用来自与流相同的技术的 API。 例如，如果你读取来自 [**MemoryStream**](https://msdn.microsoft.com/library/windows/apps/xaml/system.io.memorystream.aspx) 的 xml，请使用 **System.Xml.XmlReader** 类型，因为两种类型均为 .NET 类型。 如果你从文件读取，请使用 **Windows.Data.Xml.Dom.XmlDocument** 类型，因为文件 API 和 **XmlDocument** 均为 UWP 组件。

### <a name="copy-window-runtime-objects-to-net-types"></a>将 Window 运行时对象复制至 .NET 类型

当 UWP 组件返回 UWP 对象时，将该返回的对象复制入 .NET 对象可能很有利。 尤其重要的两个位置就是你在处理集合和流时的两个位置。

如果你调用返回集合的 UWP API，然后多次保存和访问该集合，则将该集合复制入 .NET 集合并从此使用 .NET 版本可能很有利。

### <a name="cache-the-results-of-calls-to-uwp-components-for-later-use"></a>缓存对 UWP 组件的调用结果，供以后使用

通过将值保存入本地变量（而非多次访问 UWP 类型），你可能可以获取更佳的性能。 如果你使用循环内的值，这会更加有利。 监测你的应用，以确认使用本地变量是否可改善你的应用性能。 使用已缓存的值可提高应用的速度，因为这会花费更少的互操作性时间。

### <a name="combine-calls-to-uwp-components"></a>合并对 UWP 组件的调用

尽量尝试以最少次数调用 UWP 对象来完成任务。 例如，读取大量流数据通常要优于一次读取少量数据。

使用尽可能以较少调用来捆绑工作的 API，而不使用处理更少工作而要求更多调用的 API。 例如，首选通过调用初始化多个属性的构造函数来创建对象，而非调用默认构造函数和每次指派属性。

### <a name="building-a-uwp-components"></a>生成 UWP 组件

如果你编写可为以 C++ 或 JavaScript 编写的应用所使用的 UWP 组件，请确保你的组件专为良好性能而设计。 所有在应用中获取良好性能的建议都适用于在组件中获取良好性能。 监测你的组件，以发现哪个 API 拥有较高通信模式，且在这些区域考虑提供可让用户使用较少调用来处理工作的 API。

## <a name="keep-your-app-fast-when-you-use-interop-in-managed-code"></a>在通过托管代码使用互操作时，使应用保持高速

使用 UWP 可以在本机代码和托管代码之间轻松地互操作，但是如果你不小心，它可能会产生性能成本。 下面，我们向你展示当在托管的 UWP 应用中使用互操作时，如何获得良好性能。

借助每种语言中提供的 UWP API 投射，UWP 允许开发人员使用 XAML 和他们自己选择的语言编写应用。 使用 C# 或 Visual Basic 编写应用时，此便利会带来互操作成本，因为 UWP API 通常是使用本机代码实现的，且从 C# 或 Visual Basic 进行的任何 UWP 调用都要求从托管堆栈帧到本机堆栈帧进行 CLR 过渡以及从编组函数参数到可由本机代码访问的表示进行 CLR 过渡。 对于大多数应用来说，此开销是微不足道的。 但是，如果在应用的关键路径中多次（几十万到几百万）调用 UWP API，则此成本可能会变得很显著。 通常你会希望确保在语言转换上花费的时间相对少于其余代码的执行时间。 下面图表对此进行了展示。

![互操作转换不应在程序执行时间中占优势地位。](images/interop-transitions.png)

从 C# 或 Visual Basic 使用时，在 [**.NET for Windows apps**](https://msdn.microsoft.com/library/windows/apps/xaml/br230232.aspx) 上列出的类型不会产生此互操作成本。 根据经验，你可以假定命名空间中以“Windows.”开头的类型 是 UWP 的一部分，以及命名空间中以“System.”开头的类型 属于 .NET 类型。 谨记，即使简单使用 UWP 类型（例如，分配或属性访问）也会产生互操作成本。

你应衡量你的应用并在优化你的互操作成本前确定互操作是否占据你的应用执行时间的一大部分时间。 当使用 Visual Studio 分析你的应用的性能时，可以通过使用“函数”**** 视图并查看调用 UWP 的方法所花费的包含时间，来轻松获取你的互操作成本上限。

如果你的应用由于互操作开销变得很慢，你可以通过减少对热门代码路径上的 UWP API 的调用来提高应用的性能。 例如，通过定期查询 [**UIElements**](https://msdn.microsoft.com/library/windows/apps/BR208911) 的位置和维数来执行大量物理计算的某个游戏引擎可以通过以下操作节省大量时间：存储从 **UIElements** 到本地变量的必要信息、对这些已缓存值执行计算，以及在完成计算后将最终结果分配回 **UIElements**。 另一个示例：如果 C# 或 Visual Basic 代码频繁访问某个集合，则从 [**System.Collections**](https://msdn.microsoft.com/library/windows/apps/xaml/system.collections.aspx) 命名空间使用该集合比从 [**Windows.Foundation.Collections**](https://msdn.microsoft.com/library/windows/apps/BR206657) 命名空间使用该集合更加有效。 你还应考虑组合调用 UWP 组件；一个可行的示例是使用 [**Windows.Storage.BulkAccess**](https://msdn.microsoft.com/library/windows/apps/BR207676) API。

### <a name="building-a-uwp-component"></a>生成 UWP 组件

如果要编写可用于以 C++ 或 JavaScript 编写的应用的 UWP 组件，请确保你的组件专为获得良好性能而设计。 你的 API 表面定义了你的互操作边界，也定义了用户必须考虑本主题中指导原则的程度。 如果你向其他方分发你的组件，则这些原则变得尤为重要。

所有在应用中获取良好性能的建议都适用于在组件中获取良好性能。 衡量你的组件，查明哪些 API 具有较高通信模式，并从这些方面考虑提供可让用户使用较少调用来处理工作的 API。 在设计 UWP 时投入极大的努力，以便使应用使用这些设计成果，同时无需频繁地超越互操作边界。

 

