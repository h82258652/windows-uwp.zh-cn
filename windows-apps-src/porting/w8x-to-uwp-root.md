---
author: stevewhims
description: 如果你有一个通用 8.1 应用 \#8212;whether 它面向 Windows8.1、 Windows Phone 8.1 还是同时 \#8212;then 你会发现，你的源代码和技能将顺利地移植到 windows 10。
title: 从 Windows 运行时 8.x 移动到 UWP
ms.assetid: ac163b57-dee0-43fa-bab9-8c37fbee3913
ms.author: stwhi
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: eebd0467696b78458835425f7feac903ba435f42
ms.sourcegitcommit: 6cc275f2151f78db40c11ace381ee2d35f0155f9
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/26/2018
ms.locfileid: "5556057"
---
# <a name="move-from-windows-runtime-8x-to-uwp"></a>从 Windows 运行时 8.x 移动到 UWP


如果你有一个通用 8.1 应用，无论它面向 Windows8.1、 Windows Phone 8.1 或两者 —，你会发现，你的源代码和技能将顺利地移植到 windows 10。 与 windows 10，你可以创建通用 Windows 平台 (UWP) 应用，这是可供客户安装到各种设备的单个应用包。 在 windows 10 上的更多背景，UWP 应用和自适应代码和我们将在此移植指南中提及的自适应 UI 的概念有关[向 UWP 应用的指南](https://msdn.microsoft.com/library/windows/apps/dn894631)。

在移植时，你会发现 windows 10 与之前的平台，以及 XAML 标记、 UI 框架和工具，共享大部分 Api，你会发现全都很熟悉。 和以前一样，你仍可以在 C++、C# 和 Visual Basic 之中选择要与 XAML UI 框架一起使用的编程语言。 规划要对当前的一个或多个应用具体执行哪些操作的前期步骤将取决于你所拥有的应用和项目的种类。 这将在以下部分中介绍。

## <a name="if-you-have-a-universal-81-app"></a>如果你有一个 Universal 8.1 App

通用 8.1 应用是基于 8.1 通用应用项目生成的。 假设该项目的名称是 AppName\_81。 它包含这些子项目。

-   AppName\_81.Windows。 这是为 Windows8.1 生成应用包的项目。
-   AppName\_81.WindowsPhone。 这是为 Windows Phone 8.1 生成应用包的项目。
-   AppName\_81.Shared。 这是包含由其他两个项目使用的源代码、标记文件以及其他资源的项目。

通常，8.1 通用 Windows 应用提供相同的功能，因此会使用相同的代码和标记 — 其 Windows8.1 和 Windows Phone 8.1 形式中。 这样的应用是移植到面向通用设备系列 （并且可安装到种类最广泛的设备） 的单个 windows 10 应用的理想之选。 实际上，你将移植共享项目的内容，并且只需要使用很少或不使用其他两个项目中的内容，因为它们中只有很少内容或没有内容。

其他情况下，Windows8.1 和/或应用的 Windows Phone 8.1 形式包含独有的功能。 或者它们虽然包含相同的功能，但使用不同的技术来实现这些功能。 对于这样的应用，你可以选择将其移植到面向通用设备系列的单个应用（在此情况下你要使应用适应不同的设备），或者可以选择将其移植为多个应用（比如，面向桌面设备系列的应用和面向移动设备系列的应用）。 通用 8.1 应用的性质将决定其中哪些选项最适合你的情况。

1.  将共享项目的内容移植到面向通用设备系列的应用。 如果适用，从 Windows 和 WindowsPhone 项目回收任何其他内容，并无条件地在应用中使用该内容或在当时恰巧运行应用的设备上有条件地使用该内容（后者的行为称为*自适应*）。
2.  将 WindowsPhone 项目的内容移植到面向跨平台设备系列的应用。 如果适用，从 Windows 项目中回收任何其他内容，并无条件或自适应地使用该内容。
3.  将 Windows 项目的内容移植到面向跨平台设备系列的应用。 如果适用，从 WindowsPhone 项目中回收任何其他内容，并无条件或自适应地使用该内容。
4.  将 Windows 项目的内容移植到面向跨平台或桌面设备系列的应用，同时将 WindowsPhone 项目的内容移植到面向跨平台或移动设备系列的应用。 你可以使用共享项目创建解决方案，并在这两个项目之间继续共享源代码、标记文件以及其他资源。 或者，你可以创建不同的解决方案并且仍可使用链接共享相同的项目。

## <a name="if-you-have-a-windows-81-app"></a>如果你有 Windows 8.1 应用

将项目移植到面向通用或桌面设备系列的应用。 如果你选择通用设备系列，并且你的应用调用仅在桌面设备系列中实现的 API，那么你可以使用自适应代码保护这些调用。

## <a name="if-you-have-a-windows-phone-81-app"></a>如果你有 Windows Phone 8.1 应用

将项目移植到面向通用或移动设备系列的应用。 如果你选择通用设备系列，并且你的应用调用仅在移动设备系列中实现的 API，那么你可以使用自适应代码保护这些调用。

## <a name="adapting-your-app-to-multiple-form-factors"></a>使你的应用适应多种外形规格

你从之前的部分中选择的选项将决定你的应用可运行的设备范围，并且这可能是非常广泛的设备范围。 即使将应用限制到移动设备系列，它仍然支持多种屏幕大小。 因此，如果你的应用要在之前不支持的外形规格上运行，请在这些外形规格上测试你的 UI 并进行任何必要的更改，以便你的 UI 可针对每种外形规格进行相应调整。 你可以将其视为一个移植后任务或移植延伸目标，[Bookstore2](w8x-to-uwp-case-study-bookstore2.md) 和 [QuizGame](w8x-to-uwp-case-study-quizgame.md) 案例研究中提供了一些关于它的实际应用示例。

## <a name="approaching-porting-layer-by-layer"></a>按层实现移植

在将 Universal 8.1 App 移植到 UWP App 的模型时，几乎你的所有知识和经验都将进行转移，同时你所使用的大多数源代码和标记以及软件模式也将进行转移。

-   **视图**。 视图（以及视图模型）组成了应用的 UI。 理想情况下，视图由绑定到视图模型的可观察属性的标记组成。 其他模式（常见且方便，但仅在短期内使用）适用于代码隐藏文件中的命令式代码，以直接操作 UI 元素。 在任何一种情况下，你的 UI 标记和设计（甚至是操纵 UI 元素的命令式代码）都可以简单地进行移植。
-   **视图模型和数据模型**。 即使未正式地体现分离出关注内容模式（例如 MVVM），应用中也必然会存在用于执行视图模型和数据模型的功能的代码。 视图模型代码将使用 UI 框架命名空间中的类型。 视图模型代码和数据模型代码还将使用非可视的操作系统和 .NET Framework API（包括用于数据访问的 API）。 这些 API [也适用于 UWP App](https://msdn.microsoft.com/library/windows/apps/br211369)，因此该代码中即使不是全部，也会有大部分可在无更改的情况下进行移植。
-   **云服务**。 应用的部分内容（或许其大部分内容）可能会以服务形式在云中运行。 在客户端设备上运行的应用的部分内容将连接到这些服务。 在移植分发的应用的客户端部分时，这是该应用中最有可能保持不变的部分。 如果你还没有云服务选项，则适用于 UWP App 的良好的云服务选项是 [Microsoft Azure 移动服务](http://azure.microsoft.com/services/mobile-services/)，它提供了强大的后端组件，你的应用可借助它们调用服务，范围从简单的动态磁贴更新通知到服务器场可提供的复杂可扩展性。

在移植之前或移植过程中，应考虑是否可以通过合并应用来改进它，以便可以将具有类似目的的代码集中在图层中，而不是使它们随意地分散。 按照上述步骤将应用重构到图层中，以便你可以更加轻松地更正应用、测试它，随后读取并维护它。 通过遵循模型-视图-视图模型 ([MVVM](http://msdn.microsoft.com/magazine/dd419663.aspx)) 模式，你可以使功能更容易重复使用。 此模式可使应用的数据、业务和 UI 部分彼此分隔开。 即使是在 UI 中，该模式也可以将状态和行为与视觉效果分隔开并且可分开测试。 借助 MVVM，你可以编写一次数据和业务逻辑并在所有设备上使用它，而不考虑 UI。 你可能还可以跨设备重复使用许多视图模型和视图部分。

| 主题 | 说明 |
|-------|-------------|
| [移植项目](w8x-to-uwp-porting-to-a-uwp-project.md) | 在开始移植过程时，你有两个选择。 一是编辑现有项目文件的副本，包括应用包清单（对于该选项，请参阅[将应用迁移到通用 Windows 平台应用 (UWP)](https://msdn.microsoft.com/library/mt148501.aspx) 中有关更新项目文件的信息）。 另一个是在 Visual Studio 中创建一个新的 Windows 10 项目，并将你的文件复制到其中。 |
| [疑难解答](w8x-to-uwp-troubleshooting.md) | 我们强烈建议阅读到此移植指南的末尾，但是我们也理解你希望尽快前进到项目生成和运行的阶段。 阅读到该末尾后，你可以注释或排除非必要的代码，然后稍后返回支付该债务，从而临时加快进度。 本主题中的疑难解答症状和补救办法的表格可能在此阶段对你有用，尽管它无法替代阅读接下来的一些主题。 在你进行到以后的主题时，你可以随时重新参考该表。 |
| [移植 XAML 和 UI](w8x-to-uwp-porting-xaml-and-ui.md) | 以声明性 XAML 标记的形式定义 UI 的做法非常好地将通用 8.1 应用转换为 UWP 应用。 你会发现，你的大多数标记是兼容的，尽管你可能需要针对正在使用的系统资源键或自定义模板作相应调整。 |
| [针对 I/O、设备和应用模型进行移植](w8x-to-uwp-input-and-sensors.md) | 与设备本身及其传感器集成的代码涉及到与用户之间的输入和输出。 它还可以涉及处理数据。 但是通常不将此代码视为 UI 层或数据层。 此代码包含与振动控制器、加速计、陀螺仪、麦克风和扬声器（与语音识别和合成交叉）、（地理）位置和输入形式（例如触摸、鼠标、键盘和笔）的集成。 |
| [案例研究：Bookstore1](w8x-to-uwp-case-study-bookstore1.md) | 本主题提供了一个将非常简单的通用 8.1 应用移植到 Windows 10 UWP 应用的案例研究。 通用 8.1 应用是为 Windows 8.1 生成一个应用包，并为 Windows Phone 8.1 生成另一个应用包的应用。 在 Windows 10 中，你可以创建可供客户安装到种类广泛的设备上的单个应用包，而这正是我们要在此案例研究中实现的目标。 请参阅 [UWP 应用指南](https://msdn.microsoft.com/library/windows/apps/dn894631)。 |
| [案例研究：Bookstore2](w8x-to-uwp-case-study-bookstore2.md) | 此案例研究基于 [SemanticZoom](https://msdn.microsoft.com/library/windows/apps/hh702601) 控件中提供的信息生成。 在视图模型中，类 Author 的每个实例都表示一组由该作者创作的书籍，而在 SemanticZoom 中，我们可以按作者查看分组书籍的列表，或者可以缩小到能够看到包含作者的跳转列表。 |
| [案例研究：QuizGame](w8x-to-uwp-case-study-quizgame.md) | 本主题介绍了一个将正在运行的对等测验游戏 WinRT 8.1 示例应用移植到 Windows 10 UWP 应用的案例研究。 |

## <a name="related-topics"></a>相关主题

**文档**
* [Windows 运行时参考](https://msdn.microsoft.com/library/windows/apps/br211377)
* [为所有 Windows 设备生成通用 Windows 应用](http://go.microsoft.com/fwlink/p/?LinkID=397871)
* [为应用设计 UX](https://msdn.microsoft.com/library/windows/apps/hh767284)
