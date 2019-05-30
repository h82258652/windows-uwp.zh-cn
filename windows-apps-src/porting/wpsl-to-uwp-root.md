---
description: 如果你是 Windows Phone Silverlight 应用程序的开发人员，然后可充分利用您的技能和你的源代码中移动到 Windows 10。
title: 从 Windows Phone Silverlight 移动到 UWP
ms.assetid: 9E0C0315-6097-488B-A3AF-7120CCED651A
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: c9088f695fb70d171f0b9d5474a4a0f2a63cae05
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/29/2019
ms.locfileid: "66372425"
---
#  <a name="move-from-windowsphone-silverlight-to-uwp"></a>从 Windows Phone Silverlight 移动到 UWP


如果你是 Windows Phone Silverlight 应用程序的开发人员，然后可充分利用您的技能和你的源代码中移动到 Windows 10。 使用 Windows 10 中，可以创建一个通用 Windows 平台 (UWP) 应用，这是你的客户可以安装到每种设备上的单个应用包。 有关 Windows 10、 UWP 应用和自适应代码和自适应 UI，我们将在此迁移指南中介绍的概念的更多背景，请参阅[通用 Windows 平台 (UWP) 应用的指南](https://docs.microsoft.com/windows/uwp/get-started/universal-application-platform-guide)。

当你将 Windows Phone Silverlight 应用移植到 Windows 10 应用时，你将能够赶上上的移动功能的[在 Windows Phone 8.1 中引入的](https://docs.microsoft.com/previous-versions/windows/apps/ff402535(v%3dvs.105))，和远不止他们使用通用 Windows 平台 (UWP) 应用程序模型和 UI 框架则是通用跨所有 Windows 10 设备。 这使得通过一个基本代码和一个应用包支持 PC、平板电脑、手机和大量其他种类的设备成为可能。 这将大幅增加应用的潜在受众，并通过共享数据、购买消费品等创造新的可能性。 有关新功能的详细信息，请参阅[What's new for Windows 10 中的开发人员](https://dev.windows.com/getstarted/whats-new-windows-10)。

如果你愿意，您的应用程序的 Windows Phone Silverlight 版本和它的 Windows 10 版本都可以是向客户提供在同一时间。

**请注意**  本指南旨在帮助您手动移植到 Windows 10 在 Windows Phone Silverlight 应用。 除了使用本指南中的信息来移植你的应用外，你还可以尝试 **Mobilize.NET Silverlight Bridge** 的开发者预览版来帮助实现移植过程的自动化。 此工具分析应用的源代码，并将对 Windows Phone Silverlight 控件的引用和 Api 转换为对应的 UWP。 因为此工具仍为开发人员预览版，所以它还无法处理所有转换方案。 但是，大多数开发人员通过开始使用此工具可以节省一些时间和精力。 若要尝试开发者预览版，请访问 [Mobilize.NET](https://go.microsoft.com/fwlink/p/?LinkId=624546) 的网站。

## <a name="xaml-and-net-or-html"></a>XAML 和 .NET 或 HTML？

Windows Phone Silverlight 有一个基于 Silverlight 4.0 和程序针对的.NET framework 版本和 UWP Api 的一小部分的 XAML UI 框架。 由于 Windows Phone Silverlight 应用程序中使用 Extensible Application Markup Language (XAML)，很可能 XAML 将是你的 Windows 10 版本以您所选，因为大部分知识和经验将传输，也将很多源代码和使用软件模式。 甚至你的 UI 标记和设计也可以随时进行移植。 你会发现托管 API、XAML 标记、UI 框架和工具全都令人熟悉，并且你可以在 UWP 应用中将 C++、C# 或 Visual Basic 与 XAML 一起使用。 即便过程中存在一些挑战，你可能仍会对过程是如此的简单感到惊讶。

请参阅[使用 C# 或 Visual Basic 的通用 Windows 平台 (UWP) 应用的路线图](https://docs.microsoft.com/previous-versions/windows/apps/br229583(v=win.10))。

**请注意**  Windows 10 支持更多的.NET Framework 的 Windows Phone 应用商店应用比。 例如，Windows 10 提供了几个 System.ServiceModel。\*命名空间以及 System.Net、 System.Net.NetworkInformation 和 System.Net.Sockets。 因此，现在是端口 Windows Phone Silverlight，并具有你只需编译和运行新的平台上的.NET 代码的好时机。 请参阅[命名空间和类映射](wpsl-to-uwp-namespace-and-class-mappings.md)。
重新应用现有的.NET 源代码编译到 Windows 10 应用另一个重要原因是，将受益于.NET Native，这一预的实时编译技术，它将 MSIL 转换为本机可运行代码。 .NET Native 应用启动速度更快、使用的内存更少，并且比其对应的 MSIL 更省电。

此移植指南将侧重于 XAML，或者你可以使用 JavaScript、级联样式表 (CSS) 和 HTML5 以及 Windows JavaScript 库，生成在功能上等效的应用（通过调用许多相同的 UWP API）。 尽管使用 XAML 的 Windows 运行时 UI 框架不同于使用 HTML 的 Windows 运行时 UI 框架，但无论你选择哪一个，它都需要通用于所有类型的 Windows 设备。

## <a name="targeting-the-universal-or-the-mobile-device-family"></a>面向通用或移动设备系列

你可以选择将应用移植到面向通用设备系列的应用。 在此情况下，该应用可安装到种类最广泛的设备上。 如果你的应用调用仅在移动设备系列中实现的 API，那么你可以使用自适应代码保护这些调用。 或者，你可以选择将你的应用移植到面向移动设备系列的应用，在此情况下你无需编写自适应代码。

## <a name="adapting-your-app-to-multiple-form-factors"></a>使你的应用适应多种外形规格

你从之前的部分中选择的选项将确定你的应用可运行的设备范围，并且这可能是非常广泛的设备范围。 即使将应用限制到移动设备系列，它仍然支持许多种屏幕大小。 因此，由于你的应用要在之前不支持的外形规格上运行，因此请在这些外形规格上测试你的 UI，并进行任何必要的更改，以便你的 UI 可针对每种外形规格进行相应调整。 你可以将其视为一个移植后任务或移植延伸目标，[Bookstore2](wpsl-to-uwp-case-study-bookstore2.md) 案例研究中有一个它的实际示例。

## <a name="approaching-porting-layer-by-layer"></a>按层实现移植

-   **视图**。 视图（以及视图模型）组成了应用的 UI。 理想情况下，视图由绑定到视图模型的可观察属性的标记组成。 其他模式（常见且方便，但仅在短期内使用）适用于代码隐藏文件中的命令式代码，以直接操作 UI 元素。 在任何一种情况下，你的大部分 UI 标记和设计（甚至是操纵 UI 元素的命令式代码）都可以简单地进行移植。
-   **视图模型和数据模型**。 即使未正式地体现分离出关注内容模式（例如 MVVM），应用中也必然会存在用于执行视图模型和数据模型的功能的代码。 视图模型代码将使用 UI 框架命名空间中的类型。 视图模型代码和数据模型代码还将使用非可视的操作系统和 .NET API（包括用于数据访问的 API）。 绝大多数这些代码都[适用于 UWP 应用](https://docs.microsoft.com/previous-versions/windows/br211369(v=win.10))，你进而希望能够在不进行更改的情况下移植大部分此类代码。 请记住，视图模型是视图的一个模型或*抽象图*。 视图模型提供 UI 的状态和行为，而视图本身提供视觉效果。 出于此原因，借助 UWP 可运行的适应不同外观规格的所有 UI 可能都需要进行相应的视图模型更改。 对于联网和调用云服务，你通常可以选择使用 .NET 或 UWP API。 有关涉及到该决策的因素，请参阅[云服务、联网和数据库](wpsl-to-uwp-business-and-data.md)。
-   **云服务**。 应用的部分内容（或许其大部分内容）可能会以服务形式在云中运行。 在客户端设备上运行的应用的部分内容将连接到这些服务。 在移植分发的应用的客户端部分时，这是该应用中最有可能保持不变的部分。 如果你还没有云服务选项，则适用于 UWP 应用的良好云服务选项是 [Microsoft Azure 移动服务](https://azure.microsoft.com/services/mobile-services/)，它提供了强大的后端组件，通用 Windows 应用可借助它们调用服务，范围从动态磁贴更新的简单通知到服务器场提供的难以扩展的服务类型。

在移植之前或移植过程中，应考虑是否可以通过合并应用来改进它，以便可以将具有类似目的的代码集中在图层中，而不是使它们随意地分散。 按照上述步骤将 UWP 应用构建到图层中，以便你可以更加轻松地更正应用、测试它，随后读取并维护它。 通过遵循 Model-View-ViewModel ([MVVM](https://msdn.microsoft.com/magazine/dd419663.aspx)) 模式，你可以更好地重新使用功能（还可以避免平台之间的 UI API 差异问题）。 此模式可使应用的数据、业务和 UI 部分彼此分隔开。 即使是在 UI 中，该模式也可以将状态和行为与视觉效果分隔开并且可分开测试。 借助 MVVM，你可以编写一次数据和业务逻辑并在所有设备上使用它，而不考虑 UI。 你可能还可以跨设备重复使用许多视图模型和视图部分。

## <a name="one-or-two-exceptions-to-the-rule"></a>该规则存在一两个例外

当你阅读此移植指南时，你可以参考[命名空间和类映射](wpsl-to-uwp-namespace-and-class-mappings.md)。 简单明了的映射是通用规则，命名空间和类映射表描述了任何例外。

在功能级别上，好消息是在 UWP 中不受支持的功能非常少。 在阅读本移植指南的剩余部分时，大多数技能组合和源代码均可以非常好地转换到 UWP 应用。 但是，以下是几个 Windows Phone Silverlight 功能，您可能有用于其中没有 UWP 等效项。

| 没有 UWP 等效项的功能 | Windows Phone Silverlight 文档的功能 |
|----------------------------------------------|---------------------------------------------------------|
| Microsoft XNA。 通常情况下，使用 C++ 的 [Microsoft DirectX](https://docs.microsoft.com/windows/desktop/directx) 是替代项。 请参阅[开发游戏](https://docs.microsoft.com/previous-versions/windows/apps/hh452744(v=win.10))和 [DirectX 和 XAML 互操作](https://docs.microsoft.com/previous-versions/windows/apps/hh825871(v=win.10))。 | [XNA Framework 类库](https://docs.microsoft.com/previous-versions/windows/xna/bb200104(v=xnagamestudio.41)) | 
|滤镜应用 | [对于 Windows Phone 8 的可重用功能区](https://docs.microsoft.com/previous-versions/windows/apps/jj206990(v=vs.105)) |

&nbsp;

| 主题| 描述|
|------|------------| 
| [Namespace 和类映射](wpsl-to-uwp-namespace-and-class-mappings.md) | 本主题提供 Windows Phone Silverlight Api 的完整的映射为 UWP 的等效项。 |
| [迁移项目](wpsl-to-uwp-porting-to-a-uwp-project.md) | 移植过程首先在 Visual Studio 中创建新的 Windows 10 项目并将文件复制到它。 |
| [疑难解答](wpsl-to-uwp-troubleshooting.md) | 我们强烈建议阅读到此移植指南的末尾，但是我们也理解你希望尽快前进到项目生成和运行的阶段。 阅读到该末尾后，你可以注释或排除非必要的代码，然后稍后返回支付该债务，从而临时加快进度。 本主题中的疑难解答症状和补救办法的表格可能在此阶段对你有用，尽管它无法替代阅读接下来的一些主题。 在你执行到以后的主题时，你可以一直重新参考该表。 |
| [移植的 XAML 和 UI](wpsl-to-uwp-porting-xaml-and-ui.md) | 在声明性 XAML 标记的窗体中定义 UI 的做法特别适合从 Windows Phone Silverlight 转换为 UWP 应用。 你将发现，更新了系统资源键引用、更改了某些元素类型名称并将“clr-namespace”更改为“using”后，标记的一大部分将可兼容。 |
| [移植 I/O、 设备和应用模型](wpsl-to-uwp-input-and-sensors.md) | 与设备本身及其传感器集成的代码涉及到与用户之间的输入和输出。 它还可以涉及处理数据。 但是通常不将此代码视为 UI 层或数据层。 此代码包含与振动控制器、加速计、陀螺仪、麦克风和扬声器（与语音识别和合成交叉）、（地理）位置和输入形式（例如触摸、鼠标、键盘和笔）的集成。 |
| [移植业务和数据层](wpsl-to-uwp-business-and-data.md) | 业务和数据层位于你的 UI 之后。 这两个层中的代码将调用操作系统和 .NET Framework API（例如，后台处理、位置、相机、文件系统、网络和其他数据访问）。 绝大多数这些代码都[适用于 UWP 应用](https://docs.microsoft.com/previous-versions/windows/br211369(v=win.10))，你进而希望能够在不进行更改的情况下移植大部分此类代码。 |
| [移植外形规格和用户体验](wpsl-to-uwp-form-factors-and-ux.md) | Windows 应用跨电脑、移动设备以及许多其他类型的设备共享常见的外观。 用户界面、输入和交互模式都非常相似，并且用户在设备之间移动的操作也将是熟悉的体验。|
|[案例研究：Bookstore1](wpsl-to-uwp-case-study-bookstore1.md) | 本主题介绍了案例研究的移植到 Windows 10 UWP 应用非常简单的 Windows Phone Silverlight 应用程序。 Windows 10 中，您可以创建单个应用包，你的客户可以将安装到各种设备，并且这就是我们要在此案例研究。 |
| [案例研究：Bookstore2](wpsl-to-uwp-case-study-bookstore2.md) | 本案例研究-哪些上中提供的信息生成[Bookstore1](wpsl-to-uwp-case-study-bookstore1.md)-开始使用应用来显示分组数据在 Windows Phone Silverlight **LongListSelector**。 在视图模型中，类 **Author** 的每个实例都表示一组由该作者创作的书籍，而在 **LongListSelector** 中，我们可以按作者查看分组书籍的列表，或者可以缩小到可以看到包含作者的跳转列表。 |

## <a name="related-topics"></a>相关主题

**文档**
* [什么是 Windows 10 中的开发人员的新增功能](https://dev.windows.com/getstarted/whats-new-windows-10)
* [通用 Windows 平台 (UWP) 应用指南](https://docs.microsoft.com/windows/uwp/get-started/universal-application-platform-guide)
* [使用通用 Windows 平台 (UWP) 应用的路线图C#或 Visual Basic](https://docs.microsoft.com/previous-versions/windows/apps/br229583(v=win.10))
* [什么是 Windows Phone 8 开发人员下一步](https://docs.microsoft.com/previous-versions/windows/apps/dn655121(v=vs.105))

**杂志文章**
* [Visual Studio 杂志：Windows Phone 8.1:转发的收敛领域的巨大突破](https://go.microsoft.com/fwlink/p/?LinkID=398541)

**演示文稿**
* [到 Windows 8 将从 Windows Phone 的 Nokia 音乐的故事](https://go.microsoft.com/fwlink/p/?LinkId=321521)
 

