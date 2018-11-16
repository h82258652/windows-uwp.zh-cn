---
author: TylerMSFT
title: 什么是通用 Windows 平台 (UWP) 应用？
description: 了解通用 Windows 平台 (UWP) 应用，此类应用可跨多种使用 Windows10 的设备运行。
ms.assetid: 59849197-B5C7-493C-8581-ADD6F5F8800B
ms.author: twhitney
ms.date: 5/7/2018
ms.topic: article
keywords: windows 10, uwp, 通用
ms.localizationpriority: medium
ms.openlocfilehash: a687aa7ed1115692f765e516dc8c8b15504b709e
ms.sourcegitcommit: e2fca6c79f31e521ba76f7ecf343cf8f278e6a15
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/15/2018
ms.locfileid: "6969512"
---
# <a name="whats-a-universal-windows-platform-uwp-app"></a>什么是通用 Windows 平台 (UWP) 应用？

![通用 Windows 平台应用的各种设备上运行，支持自适应式用户界面、 自然用户输入、 一个应用商店、 合作伙伴中心和云服务](images/universalapps-overview.png)

UWP 应用是：

- 安全：UWP 应用声明其访问哪些设备资源和数据 用户必须对该访问授权。
- 能够在运行 Windows 10 的所有设备上使用常见的 API。
- 可以使用设备的特定功能并让 UI 适应不同的设备屏幕尺寸、分辨率和 DPI。
- 通过运行 Windows 10 的所有设备（或只是你指定的设备）上的 Microsoft Store 提供。 Microsoft Store 提供了多种利你的应用赚钱的方式。
- 能够在不对计算机构成风险或引起“计算机腐烂”的情况下安装和卸载。
- 吸引：使用动态磁贴、推送通知以及与 Windows 时间线和 Cortana 的“Pick Up Where I Left Off”交互的用户活动吸引用户。
- 可使用 C#、C++、Visual Basic 和 Javascript 编程。 对于 UI，使用 XAML、HTML 或 DirectX。

让我们来看一下更详细的介绍。

## <a name="secure"></a>安全

UWP 应用在其清单中声明所需的设备能力，如访问麦克风、位置、网络摄像头、USB 设备、文件等。 用户必须在应用被授予能力前确认并授权该访问。

## <a name="a-common-api-surface-across-all-devices"></a>跨所有设备的通用 API 设计面

Windows 10 引入了通用 Windows 平台 (UWP)，可运行 windows 10 的每个设备上提供的通用应用平台。 UWP 核心 API 在所有 Windows 设备上是相同的。 如果你的应用仅使用核心 Api，无论你面向台式机、 Xbox、 混合现实头戴显示设备，并因此任何 windows 10 设备上运行它。

使用 C++ /WinRT 或 C++ /CX 编写的 UWP 应用可以访问属于 UWP 的 Win32 API。 所有 windows 10 设备都实现这些 Win32 Api。

## <a name="extension-sdks-expose-the-unique-capabilities-of-specific-device-types"></a>扩展 SDK 公开特定设备类型的特殊能力

如果你要使用通用 API，你的应用可以在运行 Windows 10 的所有设备上运行。 但是如果你希望 UWP 应用利用设备特定的 API，则可以。

扩展 SDK 让你可以为不同设备调用专用的 API。 例如，如果你的 UWP 应用面向 IoT 设备，则可以在项目中添加 IoT 扩展 SDK 以利用特定于 IoT 设备的功能。 有关添加扩展 SDK 的详细信息，请参阅[设备系列概述](https://docs.microsoft.com/uwp/extension-sdks/device-families-overview#extension-sdks)的**扩展 SDK** 部分。

你可以编写应用，以让它仅在特定类型的设备上运行，然后限制从 Microsoft Store 将它分发到该设备类型。 或者，你可以有条件地测试 API 在运行时的状态，并相应调整应用的行为。 有关详细信息，请参阅[设备系列概述](https://docs.microsoft.com/uwp/extension-sdks/device-families-overview#writing-code)的**编写代码**部分。<br>

以下视频提供对设备系列和自适应编码的简要概述：
<iframe src="https://channel9.msdn.com/Blogs/One-Dev-Minute/Introduction-to-UWP-and-Device-Families/player" width="640" height="360" allowFullScreen frameBorder="0"></iframe>

## <a name="adaptive-controls-and-input"></a>自适应控制和输入

UI 元素通过调整其布局和比例响应应用所运行屏幕的尺寸和 DPI。 UWP 应用与多种输入类型（如键盘、鼠标、触摸、笔和 Xbox One 控制器）配合良好。 如果你需要进一步为特定屏幕大小或设备定制 UI，新的布局面板和工具将帮助你设计可以适应运行应用的不同设备和外形规格的 UI。

![Windows 支持的设备](images/1894834-hig-device-primer-01-500.png)

Windows 通过以下功能帮助你的 UI 面向多个设备：

- 通用控件和布局面板可帮助你针对设备的屏幕分辨率优化你的 UI。 例如，按钮和滑块等控件将自动适应设备屏幕的尺寸和 DPI 密度。 布局面板根据屏幕尺寸帮助调整内容的布局。 自适应缩放用于调整以适应不同设备的分辨率和 DPI。
- 常用的输入处理允许你通过触摸、笔、鼠标、键盘或控制器（如 Microsoft Xbox 控制器）接收输入。
- 工具可以帮助你设计出能够适应不同屏幕分辨率的 UI。

你的应用 UI 的某些方面将自动适应不同的设备。 但是，你的应用的用户体验设计可能需要根据正在运行该应用的设备进行调整。 例如，照片应用在手持式小型设备上运行时可以调整其 UI，以确保该用法是单手使用的理想之选。 当照片应用在台式机上运行时，UI 应进行调整以充分利用额外的屏幕空间。

## <a name="theres-one-store-for-all-devices"></a>存在一个适用于所有设备的应用商店

统一的 app store 让你的应用在 windows 10 设备，如 PC、 平板电脑、 Xbox、 HoloLens、 Surface Hub 和物联网 (IoT) 设备上可用。 你可以向 Microsoft Store 提交应用，并使其对所有类型的设备或仅对所选设备类型可用。 你将在一个位置上提交和管理适用于 Windows 设备的所有应用。 想要使用 UWP 功能实现 C++ 桌面应用的现代化并将其在 Microsoft store 内出售？ 这同样可以实现。

UWP 应用与 [Application Insights](http://azure.microsoft.com/services/application-insights/) 集成以获得详细的遥测和分析 - 用于了解用户、改进应用的重要工具。

### <a name="monetize-your-app"></a>获取应用收益

你可以选择如何获取应用收益。 可以通过多种方法利用你的应用盈利。 你只需选择最适合自身的方式即可，例如：

- 付费下载是最简单的选项， 你只需指定价格即可。
- 试用允许用户在购买前先试用你的应用，与更传统的“免费模式”选项相比，用户更易于发现你的应用并转而使用该应用。
- 激励用户的促销价格。
- 此外还提供应用内购买和广告。

### <a name="apps-from-the-microsoft-store-provide-a-seamless-install-uninstall-and-upgrade-experience"></a>来自 Microsoft Store 的应用提供无缝的安装、卸载和升级体验

所有 UWP 应用均使用保护用户、设备和系统的打包系统分发。 用户绝对不会后悔安装应用，因为 UWP 应用在卸载后除了应用所创建的文档外不会有任何残留。

应用可以无缝部署和更新。 应用打包可以实现模块化，以便你可以根据需要下载内容和扩展。

## <a name="deliver-relevant-real-time-info-to-your-users-to-keep-them-coming-back"></a>向用户提供相关且实时的信息以吸引他们再次访问

保持用户积极使用你的 UWP 应用有多种方法：

- 动态磁贴和锁屏界面磁贴可以从应用中显示上下文相关且实时的信息概览。
- 推送通知可提供实时提醒来引起用户的注意。
- 用户活动允许用户在应用中上次中断的位置继续，甚至可以跨设备实现。
- “操作中心”管理来自应用的通知。
- 后台执行和触发器使你的应用仅在用户需要时才会运行。
- 你的应用可以使用语音和蓝牙 LE 设备来帮助用户与周围的世界进行交互。
- 集成 Cortana 以将语音命令功能添加到你的应用。

##  <a name="use-a-language-you-already-know"></a>使用一种你已知道的语言

UWP 应用可以使用 Windows 运行时，它是操作系统提供的本机 API。 此 API 通过 C++ 实现，并且在 C#、Visual Basic、C++ 和 JavaScript 中均受支持。 适用于编写 UWP 应用的部分选项包括：

- XAML UI 和 C#、VB 或 C++
- DirectX UI 和 C++
- JavaScript 和 HTML

## <a name="links-to-help-you-get-going"></a>帮助你开始的链接

### <a name="get-set-up"></a>准备工作

查看[准备工作](get-set-up.md)以下载创建应用所需的工具，然后再[编写你的第一个应用](your-first-app.md)。

### <a name="design-your-app"></a>设计你的应用

Microsoft 的设计系统名为 Fluent。 Fluent Design 系统是一套结合了最佳实践的 UWP 功能，用于创建在所有类型的支持 Windows 的设备上都表现出色的应用。 Fluent 体验能够适应各类设备，并提供自然的使用感受，从平板电脑到笔记本电脑，从电脑到电视，再到虚拟现实设备。 有关 Fluent Design 的简介，请参阅 [UWP 应用的 Fluent Design 系统](https://docs.microsoft.com/windows/uwp/design/fluent-design-system)。

除了确定应用外观和运行方式外，良好的[设计](http://go.microsoft.com/fwlink/?LinkId=258848)还是确定用户如何与你的应用交互的过程。 用户体验极大地影响着用户对你的应用的满意度，所以请勿忽略此步骤。 [设计基础知识](https://dev.windows.com/design)介绍了如何设计通用 Windows 应用。 有关设计出令用户满意的 UWP 应用的信息，请参阅[面向设计人员的通用 Windows 平台 (UWP) 应用简介](https://msdn.microsoft.com/library/windows/apps/dn958439)。 在开始编写代码之前，请参阅[设备入门](../design/devices/index.md)，以帮助你全面考虑在你要针对的所有不同外形规格上使用应用的交互体验。

除了在不同设备上的交互外，还需[规划应用](https://msdn.microsoft.com/library/windows/apps/hh465427)以利用在多个设备之间运行的优势。 例如：

- 使用[适用于 UWP 应用的导航设计基础知识](https://msdn.microsoft.com/library/windows/apps/dn958438)设计你的工作流，以适应移动设备、小屏幕和大屏幕设备。 [设置你的用户界面布局](https://msdn.microsoft.com/library/windows/apps/dn958435)来响应不同的屏幕大小和分辨率。

- 请考虑如何容纳多种输入类型。 请参阅[交互指南](https://msdn.microsoft.com/library/windows/apps/dn611861)，了解用户如何使用 [Cortana](https://msdn.microsoft.com/library/windows/apps/dn974233)、[语音](https://msdn.microsoft.com/library/windows/apps/dn596121)、[触摸交互](https://msdn.microsoft.com/library/windows/apps/hh465370)、[触摸键盘](https://msdn.microsoft.com/library/windows/apps/hh972345)等与你的应用交互。  或请参阅[文本和文本输入指南](https://msdn.microsoft.com/library/windows/apps/dn611864)，以获取更多的传统交互体验。

### <a name="add-services"></a>添加服务

- 使用[云服务](http://go.microsoft.com/fwlink/?LinkId=526377)跨设备同步。
- 了解如何[连接到 Web 服务](https://msdn.microsoft.com/library/windows/apps/xaml/hh761504)以支持你的应用体验。
- 了解如何[将 Cortana 添加到应用](https://mva.microsoft.com/training-courses/integrating-cortana-in-your-apps-8487?l=20D3s5Xz_5904984382)以使你的应用能够响应语音命令。
- 将[推送通知](https://msdn.microsoft.com/library/windows/apps/mt187203)和[应用内购买](https://msdn.microsoft.com/library/windows/apps/mt219684)包含在你的规划中。 这些功能应该能够跨设备运行。

### <a name="submit-your-app-to-the-store"></a>你将应用提交到应用商店

[合作伙伴中心](https://partner.microsoft.com/dashboard)允许你管理和提交你的所有应用在同一位置针对 Windows 设备。 请参阅[发布 Windows 应用和游戏](../publish/index.md)以了解如何提交应用以在 Microsoft Store 中发布。

新功能简化了流程，同时给予你更多的控制。 你还会找到与[付款详细信息](https://msdn.microsoft.com/library/windows/apps/dn986925)组合的详细[分析报告](https://msdn.microsoft.com/library/windows/apps/mt148522)、[推广你的应用并与客户互动](https://msdn.microsoft.com/library/windows/apps/mt148526)的方式，等等。

有关更多入门材料，请参阅[生成适用于 Windows10 设备的 Windows 应用简介](https://msdn.microsoft.com/magazine/dn973012.aspx)

### <a name="more-advanced-topics"></a>更多高级主题

- 了解如何使用[用户活动](https://blogs.windows.com/buildingapps/2017/12/19/application-engagement-windows-timeline-user-activities/#tHuZ6tLPtCXqYKvw.97)，让应用中的用户活动显示在 Windows 时间线和 Cortana 的“Pick Up Where I Left Off”功能中。
- 了解如何使用[适用于 UWP 应用的磁贴、锁屏提醒和通知](https://docs.microsoft.com/windows/uwp/design/shell/tiles-and-notifications/)。
- 有关适用于 UWP 应用的 Win32 API 的完整列表，请参阅 [UWP 应用的 API 集](https://msdn.microsoft.com/library/windows/desktop/mt186421)和 [UWP 应用的 Dll](https://msdn.microsoft.com/library/windows/desktop/mt186422)。
- 请参阅 [.NET 中的通用 Windows 应用](https://blogs.msdn.microsoft.com/dotnet/2015/07/30/universal-windows-apps-in-net)查看编写 .NET UWP 应用的概述。
- 有关你可以在 UWP 应用中使用的 .NET 类型的列表，请参阅[适用于 UWP 应用的 .NET](https://msdn.microsoft.com/library/mt185501.aspx)
- [.NET Native - 对于通用 Windows 平台 (UWP) 开发人员的意义](https://blogs.windows.com/buildingapps/2015/08/20/net-native-what-it-means-for-universal-windows-platform-uwp-developers/#TYsD3tJuBJpK3Hc7.97)
- 了解如何将适合 Windows 10 用户的现代体验添加到现有的桌面应用，并通过[桌面桥](https://developer.microsoft.com/windows/bridges/desktop)在 Microsoft Store 中分发。

## <a name="how-the-universal-windows-platform-relates-to-windows-runtime-apis"></a>通用 Windows 平台如何与 Windows 运行时 Api 相关联
如果你正在构建的通用 Windows 平台 (UWP) 应用，你可以获得了大量的实际效果和退出术语"通用 Windows 平台 (UWP)"和"Windows 运行时 (WinRT)"视为同义词更多或更少的便利。 但它** 可能看起来将在背后的技术，并确定只是什么之处在于这些观点之间。 如果你不希望了解它，然后本节最后一个适合你。

Windows 运行时和 WinRT Api，Windows Api 的进化。 最初，Windows 已编程通过平面，C 样式 Win32 Api。 这些已添加到 COM Api ([DirectX](https://msdn.microsoft.com/library/windows/desktop/ee663274)正在突出的示例)。 Windows 窗体、 WPF、.NET 和托管的语言汇聚到自己地编写的 Windows 应用和自己风格的 API 技术。 Windows 运行时，深层下, 一个阶段的 com。 在实际应用程序二进制接口 (ABI) 层，在 COM 其根目录变得可见。 但 Windows 运行时已设计为可从出色各种不同的编程语言调用。 和可调用对每个这些语言非常自然的方式。 为此，访问 Windows 运行时可通过称为语言投影。 没有 Windows 运行时语言投影到 C#、 Visual Basic 到、 到标准 c + +、 到 JavaScript 中，依次类推。 此外，一次打包相应地 （请参阅[桌面桥](/windows/uwp/porting/desktop-to-uwp-root)），你可以从一个生成出色各种应用程序模型之一的应用调用 WinRT Api: Win32、.NET、 WinForms 和 WPF。

并且，当然，你可以从你的 UWP 应用中调用 WinRT Api。 UWP 是基于 Windows 运行时应用程序模型。 从技术上讲，对 UWP 应用程序模型基于[CoreApplication](/uwp/api/windows.applicationmodel.core.coreapplication)，尽管该细节可能从您，具体取决于你选择的编程语言中隐藏。 本主题已所述，价值主张的角度来看，从 UWP 借给编写单个二进制文件，你应该选择，可发布到 Microsoft 应用商店，并在任何一个绝佳广泛的设备外形规格上运行。 你的 UWP 应用的设备范围取决于 UWP Api 子集你限制你的应用调用，或者，也有条件地调用。

希望，本部分已成功地描述基础 Windows 运行时 Api 和机制的通用 Windows 平台的业务值的技术之间的区别。
