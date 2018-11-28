---
description: 如果你是一名开发人员的 WindowsPhone Silverlight 应用，然后你可以使用你的技能组合和源代码在进行移动到 windows 10。
title: 从 WindowsPhone Silverlight 转到 UWP
ms.assetid: 9E0C0315-6097-488B-A3AF-7120CCED651A
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 17d107e7886838071567a1368c6b542ae5ecfad0
ms.sourcegitcommit: b11f305dbf7649c4b68550b666487c77ea30d98f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/27/2018
ms.locfileid: "7844846"
---
#  <a name="move-from-windowsphone-silverlight-to-uwp"></a>从 WindowsPhone Silverlight 转到 UWP


如果你是一名开发人员的 WindowsPhone Silverlight 应用，然后你可以使用你的技能组合和源代码在进行移动到 windows 10。 与 windows 10，你可以创建通用 Windows 平台 (UWP) 应用，这是可供客户安装到各种设备的单个应用包。 有关更多背景 windows 10，UWP 应用和自适应代码和我们将在此移植指南中提及的自适应 UI 的概念看到[通用 Windows 平台 (UWP) 应用指南](https://msdn.microsoft.com/library/windows/apps/dn894631)。

当你将 WindowsPhone Silverlight 应用移植到 windows 10 应用时，你将能够补充在已[在 Windows Phone 8.1 中引入](https://msdn.microsoft.com/library/windows/apps/dn632424)，移动功能和远不止他们使用其应用模型和 UI 框架的通用 Windows 平台 (UWP)在所有 windows 10 设备上的通用。 这使得通过一个基本代码和一个应用包支持 PC、平板电脑、手机和大量其他种类的设备成为可能。 这将大幅增加应用的潜在受众，并通过共享数据、购买消费品等创造新的可能性。 有关新功能的详细信息，请参阅[什么是 windows 10 中的开发人员的新增功能](https://dev.windows.com/getstarted/whats-new-windows-10)。

如果你选择，你的应用的 WindowsPhone Silverlight 版本和 windows 10 版本都可以提供给客户在同一时间。

**注意**本指南旨在帮助你将 WindowsPhone Silverlight 应用移植到 windows 10 手动。 除了使用本指南中的信息来移植你的应用外，你还可以尝试 **Mobilize.NET Silverlight Bridge** 的开发者预览版来帮助实现移植过程的自动化。 此工具分析你的应用的源代码，并将 Api 和 WindowsPhone Silverlight 控件的引用转换为其 UWP 的对应项。 因为此工具仍为开发人员预览版，所以它还无法处理所有转换方案。 但是，大多数开发人员通过开始使用此工具可以节省一些时间和精力。 若要尝试开发者预览版，请访问 [Mobilize.NET](http://go.microsoft.com/fwlink/p/?LinkId=624546) 的网站。

## <a name="xaml-and-net-or-html"></a>XAML 和 .NET 或 HTML？

WindowsPhone Silverlight 具有基于 Silverlight 4.0 和计划针对.NET Framework 的版本和 UWP Api 的一小部分的 XAML UI 框架。 由于 WindowsPhone Silverlight 应用中使用 Extensible Application Markup Language (XAML)，它是，XAML 可能将是 windows 10 版本所选因为大部分知识和经验将传输，也将许多源代码和使用软件模式。 甚至你的 UI 标记和设计也可以随时进行移植。 你会发现托管 API、XAML 标记、UI 框架和工具全都令人熟悉，并且你可以在 UWP 应用中将 C++、C# 或 Visual Basic 与 XAML 一起使用。 即便过程中存在一些挑战，你可能仍会对过程是如此的简单感到惊讶。

请参阅[使用 C# 或 Visual Basic 的通用 Windows 平台 (UWP) 应用的路线图](https://msdn.microsoft.com/library/windows/apps/br229583)。

**注意**windows 10 支持更多的.NET framework 比 Windows Phone 应用商店应用的功能。 例如，windows 10 具有多个 System.ServiceModel.\* 命名空间，以及 System.Net、 System.Net.NetworkInformation 和 System.Net.Sockets。 因此，现在是移植你 WindowsPhone Silverlight 并使你的.NET 代码刚好编译并起效新的平台上的绝佳时机。 请参阅[命名空间和类映射](wpsl-to-uwp-namespace-and-class-mappings.md)。
将你的现有.NET 源代码重新编译到 windows 10 应用的另一个绝佳理由是，你将受益于.NET Native，这是先进障碍的编译技术，可将 MSIL 转换为可本机运行的计算机代码。 .NET Native 应用启动速度更快、使用的内存更少，并且比其对应的 MSIL 更省电。

此移植指南将侧重于 XAML，或者你可以使用 JavaScript、级联样式表 (CSS) 和 HTML5 以及 Windows JavaScript 库，生成在功能上等效的应用（通过调用许多相同的 UWP API）。 尽管使用 XAML 的 Windows 运行时 UI 框架不同于使用 HTML 的 Windows 运行时 UI 框架，但无论你选择哪一个，它都需要通用于所有类型的 Windows 设备。

## <a name="targeting-the-universal-or-the-mobile-device-family"></a>面向通用或移动设备系列

你可以选择将应用移植到面向通用设备系列的应用。 在此情况下，该应用可安装到种类最广泛的设备上。 如果你的应用调用仅在移动设备系列中实现的 API，那么你可以使用自适应代码保护这些调用。 或者，你可以选择将你的应用移植到面向移动设备系列的应用，在此情况下你无需编写自适应代码。

## <a name="adapting-your-app-to-multiple-form-factors"></a>使你的应用适应多种外形规格

你从之前的部分中选择的选项将确定你的应用可运行的设备范围，并且这可能是非常广泛的设备范围。 即使将应用限制到移动设备系列，它仍然支持许多种屏幕大小。 因此，由于你的应用要在之前不支持的外形规格上运行，因此请在这些外形规格上测试你的 UI，并进行任何必要的更改，以便你的 UI 可针对每种外形规格进行相应调整。 你可以将其视为一个移植后任务或移植延伸目标，[Bookstore2](wpsl-to-uwp-case-study-bookstore2.md) 案例研究中有一个它的实际示例。

## <a name="approaching-porting-layer-by-layer"></a>按层实现移植

-   **视图**。 视图（以及视图模型）组成了应用的 UI。 理想情况下，视图由绑定到视图模型的可观察属性的标记组成。 其他模式（常见且方便，但仅在短期内使用）适用于代码隐藏文件中的命令式代码，以直接操作 UI 元素。 在任何一种情况下，你的大部分 UI 标记和设计（甚至是操纵 UI 元素的命令式代码）都可以简单地进行移植。
-   **视图模型和数据模型**。 即使未正式地体现分离出关注内容模式（例如 MVVM），应用中也必然会存在用于执行视图模型和数据模型的功能的代码。 视图模型代码将使用 UI 框架命名空间中的类型。 视图模型代码和数据模型代码还将使用非可视的操作系统和 .NET API（包括用于数据访问的 API）。 绝大多数这些代码都[适用于 UWP 应用](https://msdn.microsoft.com/library/windows/apps/br211369)，你进而希望能够在不进行更改的情况下移植大部分此类代码。 请记住，视图模型是视图的一个模型或*抽象图*。 视图模型提供 UI 的状态和行为，而视图本身提供视觉效果。 出于此原因，借助 UWP 可运行的适应不同外观规格的所有 UI 可能都需要进行相应的视图模型更改。 对于联网和调用云服务，你通常可以选择使用 .NET 或 UWP API。 有关涉及到该决策的因素，请参阅[云服务、联网和数据库](wpsl-to-uwp-business-and-data.md)。
-   **云服务**。 应用的部分内容（或许其大部分内容）可能会以服务形式在云中运行。 在客户端设备上运行的应用的部分内容将连接到这些服务。 在移植分发的应用的客户端部分时，这是该应用中最有可能保持不变的部分。 如果你还没有云服务选项，则适用于 UWP 应用的良好云服务选项是 [Microsoft Azure 移动服务](http://azure.microsoft.com/services/mobile-services/)，它提供了强大的后端组件，通用 Windows 应用可借助它们调用服务，范围从动态磁贴更新的简单通知到服务器场提供的难以扩展的服务类型。

在移植之前或移植过程中，应考虑是否可以通过合并应用来改进它，以便可以将具有类似目的的代码集中在图层中，而不是使它们随意地分散。 按照上述步骤将 UWP 应用构建到图层中，以便你可以更加轻松地更正应用、测试它，随后读取并维护它。 通过遵循 Model-View-ViewModel ([MVVM](http://msdn.microsoft.com/magazine/dd419663.aspx)) 模式，你可以更好地重新使用功能（还可以避免平台之间的 UI API 差异问题）。 此模式可使应用的数据、业务和 UI 部分彼此分隔开。 即使是在 UI 中，该模式也可以将状态和行为与视觉效果分隔开并且可分开测试。 借助 MVVM，你可以编写一次数据和业务逻辑并在所有设备上使用它，而不考虑 UI。 你可能还可以跨设备重复使用许多视图模型和视图部分。

## <a name="one-or-two-exceptions-to-the-rule"></a>该规则存在一两个例外

当你阅读此移植指南时，你可以参考[命名空间和类映射](wpsl-to-uwp-namespace-and-class-mappings.md)。 简单明了的映射是通用规则，命名空间和类映射表描述了任何例外。

在功能级别上，好消息是在 UWP 中不受支持的功能非常少。 在阅读本移植指南的剩余部分时，大多数技能组合和源代码均可以非常好地转换到 UWP 应用。 但是，以下是你可能已使用的几个 WindowsPhone Silverlight 功能有关的没有 UWP 等效项。

| 没有 UWP 等效项的功能 | 有关该功能的 WindowsPhone Silverlight 文档 |
|----------------------------------------------|---------------------------------------------------------|
| Microsoft XNA。 通常情况下，使用 C++ 的 [Microsoft DirectX](https://msdn.microsoft.com/library/windows/desktop/ee663274) 是替代项。 请参阅[开发游戏](https://msdn.microsoft.com/library/windows/apps/hh452744)和 [DirectX 和 XAML 互操作](https://msdn.microsoft.com/library/windows/apps/hh825871)。 | [XNA Framework 类库](http://msdn.microsoft.com/library/bb203940.aspx) | 
|滤镜应用 | [Windows Phone 8 的镜头](http://msdn.microsoft.com/library/windows/apps/jj206990.aspx) |

&nbsp;

| 主题| 说明|
|------|------------| 
| [命名空间和类映射](wpsl-to-uwp-namespace-and-class-mappings.md) | 本主题提供 WindowsPhone Silverlight api 到其 UWP 等效项的全面映射。 |
| [移植项目](wpsl-to-uwp-porting-to-a-uwp-project.md) | 通过在 Visual Studio 中创建一个新的 windows 10 项目，并将文件复制到其中开始移植过程。 |
| [疑难解答](wpsl-to-uwp-troubleshooting.md) | 我们强烈建议阅读到此移植指南的末尾，但是我们也理解你希望尽快前进到项目生成和运行的阶段。 阅读到该末尾后，你可以注释或排除非必要的代码，然后稍后返回支付该债务，从而临时加快进度。 本主题中的疑难解答症状和补救办法的表格可能在此阶段对你有用，尽管它无法替代阅读接下来的一些主题。 在你进行到以后的主题时，你可以随时重新参考该表。 |
| [移植 XAML 和 UI](wpsl-to-uwp-porting-xaml-and-ui.md) | 声明性 XAML 标记的形式定义 UI 的做法非常好地将 WindowsPhone Silverlight 转换为 UWP 应用。 你将发现，更新了系统资源键引用、更改了某些元素类型名称并将“clr-namespace”更改为“using”后，标记的一大部分将可兼容。 |
| [针对 I/O、设备和应用模型进行移植](wpsl-to-uwp-input-and-sensors.md) | 与设备本身及其传感器集成的代码涉及到与用户之间的输入和输出。 它还可以涉及处理数据。 但是通常不将此代码视为 UI 层或数据层。 此代码包含与振动控制器、加速计、陀螺仪、麦克风和扬声器（与语音识别和合成交叉）、（地理）位置和输入形式（例如触摸、鼠标、键盘和笔）的集成。 |
| [移植业务和数据层](wpsl-to-uwp-business-and-data.md) | 业务和数据层位于你的 UI 之后。 这两个层中的代码将调用操作系统和 .NET Framework API（例如，后台处理、位置、相机、文件系统、网络和其他数据访问）。 绝大多数这些代码都[适用于 UWP 应用](https://msdn.microsoft.com/library/windows/apps/br211369)，你进而希望能够在不进行更改的情况下移植大部分此类代码。 |
| [针对外形规格和 UX 进行移植](wpsl-to-uwp-form-factors-and-ux.md) | Windows 应用跨电脑、移动设备以及许多其他类型的设备共享常见的外观。 用户界面、输入和交互模式都非常相似，并且用户在设备之间移动的操作也将是熟悉的体验。|
|[案例研究：Bookstore1](wpsl-to-uwp-case-study-bookstore1.md) | 本主题介绍了移植非常简单的 WindowsPhone Silverlight 应用到 Windows10UWP 应用的案例研究。 借助 windows 10，你可以创建一个应用包，可供客户安装到种类广泛的设备，并且我们将在此案例研究中所执行的操作。 |
| [案例研究：Bookstore2](wpsl-to-uwp-case-study-bookstore2.md) | 此案例研究-基于[Bookstore1](wpsl-to-uwp-case-study-bookstore1.md)中提供的信息，首先研究可显示分组数据**的 LongListSelector**中 WindowsPhone Silverlight 应用。 在视图模型中，类 **Author** 的每个实例都表示一组由该作者创作的书籍，而在 **LongListSelector** 中，我们可以按作者查看分组书籍的列表，或者可以缩小到可以看到包含作者的跳转列表。 |

## <a name="related-topics"></a>相关主题

**文档**
* [Windows 10 中的开发人员的新增功能](https://dev.windows.com/getstarted/whats-new-windows-10)
* [通用 Windows 平台 (UWP) 应用指南](https://msdn.microsoft.com/library/windows/apps/dn894631)
* [使用 C# 或 Visual Basic 的通用 Windows 平台 (UWP) 应用的路线图](https://msdn.microsoft.com/library/windows/apps/br229583)
* [Windows Phone 8 开发人员的下一步计划](https://msdn.microsoft.com/library/windows/apps/xaml/dn655121.aspx)

**杂志文章**
* [Visual Studio 杂志：Windows Phone 8.1：面向融合的巨大飞跃](http://go.microsoft.com/fwlink/p/?LinkID=398541)

**演示文稿**
* [将 Nokia 音乐从 Windows Phone 到 Windows8 篇文章](http://go.microsoft.com/fwlink/p/?LinkId=321521)
 

