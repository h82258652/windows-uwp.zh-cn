---
title: 面向开发人员的 Windows 10 中的新增功能
description: Windows 10 内部版本 18362 和新的开发人员工具提供由通用 Windows 平台支持的工具、功能和体验。
keywords: 新增功能, 新功能, 更新, 刷新, 功能, 新, Windows 10, 最新, 开发人员, 18362, 五月
ms.date: 04/19/2019
ms.topic: article
ms.localizationpriority: medium
ms.custom: 19H1
ms.openlocfilehash: 4e92afd112ce7600bcfa650e0bb3bbeffabd7bd0
ms.sourcegitcommit: f120968069702a7210756b508dabc4a1a8c20d53
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/16/2019
ms.locfileid: "72438215"
---
# <a name="whats-new-in-windows-10-for-developers-build-18362"></a>面向开发人员的 Windows 10 内部版本 18362 中的新增功能

Windows 10 内部版本 18362（也称为 SDK 版本 1903）与 Visual Studio 2019 相结合，提供所需的工具、功能和体验来创建非凡的 Windows 应用。 只需在 Windows 10 上[安装工具和 SDK](https://go.microsoft.com/fwlink/?LinkId=821431)，你便可以随时[创建新的通用 Windows 应用](../get-started/create-uwp-apps.md)，或了解如何使用 [Windows 上的现有应用代码](../porting/index.md)。

该版本集合了 Windows 开发人员感兴趣的新增和改进功能及指南。 有关已添加到 Windows SDK 的新命名空间的完整列表，请参阅 [Windows 10 内部版本 18362 API 更改](windows-10-build-18362-api-diff.md)。 有关 Windows 10 亮点功能的详细信息，请参阅 [Windows 10 中的酷炫功能](https://go.microsoft.com/fwlink/?LinkId=823181)。

## <a name="design--ui"></a>设计和 UI

功能 | 描述
:------ | :------
AnimatedVisualPlayer | [AnimatedVisualPlayer](https://docs.microsoft.com/uwp/api/microsoft.ui.xaml.controls.animatedvisualplayer) API 用于承载和控制应用中动画视觉对象的播放。 此 API 用于控制和显示 [Lottie](https://docs.microsoft.com/windows/communitytoolkit/animations/lottie) 视觉对象等内容，可用于在应用程序本地渲染 Adobe AfterEffects 动画。
CompactDensity | 在应用中启用[紧密模式](../design/style/spacing.md)可以实现密集、信息丰富的控件组。 这有助于浏览大量内容，最大化页面上的可视化内容，或者在用户使用指针输入时帮助其导航和交互。
Items Repeater | [ItemsRepeater](../design/controls-and-patterns/items-repeater.md) 控件可以创建用于向用户显示集合的自定义体验。 ItemsRepeater 不提供综合性的最终用户体验或默认 UI， 而是一个构建基块，你可以使用它来创建自己的基于唯一集合的体验和自定义控件。
教学提示 | [教学提示](../design/controls-and-patterns/dialogs-and-flyouts/teaching-tip.md)是一个提供上下文信息的半持久且包含丰富内容的浮出控件。 可以使用此控件向用户通知、提醒和传授新的或重要的功能。
UI 命令控制 | 借助 [UWP 应用中的命令控制](../design/controls-and-patterns/commanding.md)，可以使用 [XamlUICommand](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.standarduicommand) 和 [StandardUICommand](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.standarduicommand) 类（以及 ICommand 接口）在不同的控件类型之间共享和管理命令，不管所用的设备和输入类型是什么。
Windows UI 库 | 最新官方版 Windows UI 库 – [WinUI 2.1](https://docs.microsoft.com/uwp/toolkits/winui/release-notes/winui-2.1) – 提供适用于 Windows 应用的丰富多样的新 XAML 控件。 WinUI 库 API 可以在较早的 Windows 10 版本上运行，所以你不需要添加版本检查或条件 XAML 以支持未使用最新版本的用户。
桌面应用中的视觉层 | 现在可以[在桌面应用程序中使用 UWP 视觉层 API](../composition/visual-layer-in-desktop-apps.md)。 这些 API 为图形、效果和动画提供高性能的保留模式 API，是 Windows 设备上的 UI 的基础。
Z 深度和阴影 | 使用 [Z 深度和阴影](../design/layout/depth-shadow.md)在 UWP 应用中创建垂直面。 使用这些新功能可使应用的 UI 更易于扫描，并且可以更好地向用户传达需要重点关注的内容。

## <a name="develop-windows-apps"></a>开发 Windows 应用

功能 | 描述
:------ | :------
反恶意软件扫描接口 (AMSI) | 了解[反恶意软件扫描接口 (AMSI) 如何帮助你防范恶意软件](https://docs.microsoft.com/windows/desktop/amsi/how-amsi-helps)，然后查看[示例代码](https://docs.microsoft.com/windows/desktop/amsi/dev-audience)了解如何在桌面应用中实现它。
C++/WinRT 2.0 | C++/WinRT 版本 2.0 现已发布。 请查看 [C++/WinRT 中的新增功能](../cpp-and-winrt-apis/news.md)，了解所有新的更改和补充功能的完整概述。
选择平台 | 希望新建桌面应用程序？ 查看我们改进的[选择平台](https://docs.microsoft.com/windows/desktop/choose-your-technology)页面，获取关于 UWP、WPF 和 Windows 窗体平台的详细介绍和对比，以及关于 Win32 API 的详细信息。
对话代理 | 使用 [Windows.ApplicationModel.ConversationalAgent](https://docs.microsoft.com/uwp/api/windows.applicationmodel.conversationalagent) 命名空间可将 Windows 平台代理激活运行时 (AAR) 支持的任何数字助手添加到 Windows 应用。
云文件 API | 使用云文件 API 可以[构建一个支持占位符文件的云同步引擎](https://docs.microsoft.com/windows/desktop/cfapi/build-a-cloud-file-sync-engine)。 
Direct 3D 12 | 如果渲染器基于“基于磁贴的延迟渲染”(TBDR) 以及其他技术，则 [Direct3D 12 渲染器通道](/windows/desktop/direct3d12/direct3d-12-render-passes)可提高渲染器的性能。 该技术通过使应用程序更好地识别资源渲染排序要求和数据依赖项，来帮助渲染器提高 GPU 效率。 这可以减少流入/流出芯片外内存的内存流量。
直接机器学习 (DirectML) | [DirectML](https://docs.microsoft.com/windows/desktop/direct3d12/dml) 是用于机器学习的低级别硬件加速 API。 它具有常见的（本机 C++、nano-COM）编程接口和 DirectX 12 样式的工作流。 可将机器学习推断工作负荷集成到游戏、引擎、中间件、后端或其他应用程序中。 所有与 DirectX 12 兼容的硬件都支持 DirectML。
DirectX HLSL | [HLSL 着色器模型 6.4](https://docs.microsoft.com/windows/desktop/direct3dhlsl/hlsl-shader-model-6-4-features-for-direct3d-12) 提供可在 DirectML 中使用的新机器学习内部函数。
驱动程序开发 | 为 Windows 驱动程序开发人员添加了新的音频、相机、显示、网络、移动宽带、打印、传感器、存储和 WiFi 功能。 有关更多详细信息，请查看[驱动程序开发中的新增功能](https://docs.microsoft.com/windows-hardware/drivers/what-s-new-in-driver-development#whats-new-in-windows-10-version-1903-latest)。
文件系统操作 | 此[最佳做法指南](../files/best-practices-for-writing-to-files.md)可帮助你充分利用 Windows.Storage.FileIO 和 Windows.Storage.PathIO 类来执行文件系统 I/O 操作。
游戏板和遥控器交互 | 使用[游戏手柄和远程控制交互](../design/input/gamepad-and-remote-interactions.md)构建易于使用和访问的交互体验。 借助这种交互，可以在两英尺远的位置直观轻松地使用应用程序，就如同它与你相距 10 英尺一样。
日本年号更改 | 我们提供了[这些说明](../design/globalizing/japanese-era-change.md)来展示如何确保 Windows 应用程序针对于 2019 年 5 月 1 日进行的日本纪元更改做好准备。 [此页面也提供了日语版](https://docs.microsoft.com/ja-jp/windows/uwp/design/globalizing/japanese-era-change)。
WPF、Windows 窗体和 WinUI 的开源 | WPF、Windows 窗体和 WinUI UX 框架现可用于 GitHub 上的开源内容。 有关详细信息和链接，请参阅[生成 Windows 应用博客](https://blogs.windows.com/buildingapps/2018/12/04/announcing-open-source-of-wpf-windows-forms-and-winui-at-microsoft-connect-2018/#OKZjJs1VVTrMMtkL.97)。
适用于 Xbox 的渐进式 Web 应用 | 使用[适用于 Xbox One 的渐进式 Web 应用](https://docs.microsoft.com/microsoft-edge/progressive-web-apps/xbox-considerations)，可以扩展 Web 应用程序，并通过 Microsoft Store 将其作为 Xbox One 应用提供，同时仍继续使用现有框架、CDN 和服务器后端。 在大多数情况下，可以像在 Windows 中一样打包 Xbox One 的 PWA。 此指南会引导你完成该过程，并强调重要的差异。
Project Rome | Project Rome SDK 现在适用于 Android 和 iOS。 了解如何将图形通知与每个平台集成：[Android](https://docs.microsoft.com/windows/project-rome/notifications/how-to-guide-for-android) 和 [iOS](https://docs.microsoft.com/windows/project-rome/notifications/how-to-guide-for-ios)。
远程相机 | 使用 DeviceWatcher 类可以[连接到远程相机](../audio-video-camera/connect-to-remote-cameras.md)，并将这些相机中的帧读入 Windows 应用。
桌面应用程序中的 UWP 控件（XAML 岛） | Windows SDK 中用于承载 WPF、Windows 窗体和 C++ Win32 桌面应用程序内的 UWP 控件的 API 不再以开发人员预览版提供。 有关详细信息，请参阅[桌面应用中的 UWP 控件](../xaml-platform/xaml-host-controls.md)。
Visual Studio 2019 | Visual Studio 2019 已发布，其中包含面向任何开发人员、应用或平台的最新工具和服务。 请查看 [Visual Studio 2019 中的新增功能](https://docs.microsoft.com/visualstudio/ide/whats-new-visual-studio-2019?view=vs-2019)了解最新版本和入门知识。
Win32 WebView | [常见问题解答](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/webview#frequently-asked-questions-faqs)为在桌面应用程序中使用 Microsoft Edge WebView 时所提出的常见问题提供解答，并提供示例和其他资源的链接。
Windows 命令行 | [新的控制台功能](https://devblogs.microsoft.com/commandline/new-experimental-console-features/)包括试验性的“终端”选项卡，以及有关滚动、光标形状和光标颜色的设置。 请在[面向开发人员的 Windows 命令行工具博客](https://devblogs.microsoft.com/commandline/)中了解详细信息。
Windows 社区工具包 | Windows 社区工具包 v5.1 提供动画、远程设备、图像裁剪和辅助功能的令人兴奋的更新。 </br> • 新的 [Lottie-Windows 库](https://docs.microsoft.com/windows/communitytoolkit/animations/lottie)利用 Windows.UI.Composition API 在 Windows 10 (1809) 上提供优质动画支持，并允许使用 [Bodymovin](https://aescripts.com/bodymovin/) JSON 文件或优化的代码生成类在 Windows 应用中播放内容。 请试用 Microsoft Store 中提供的新 [Lottie Viewer 应用](https://aka.ms/lottieviewer)，以测试动画并为 Windows 应用生成优化的代码。 </br> • 新的[远程设备选取器](https://docs.microsoft.com/windows/communitytoolkit/controls/remotedevicepicker)可让用户选择（附近的或可通过云访问的）设备，在该设备上启动应用，或者与远程设备上的应用服务通信。 </br> • 新的 [ImageCropper 控件](https://docs.microsoft.com/windows/communitytoolkit/controls/imagecropper)集成了裁剪功能，可用于选择个人资料图片或使用照片编辑工具。 </br> • 此外，已改进控件的辅助功能，并为 WPF 和 WinForms 提供了 [Microsoft.Toolkit.Win32](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32) 6.0 预览包更新；在[发行说明](https://github.com/windows-toolkit/WindowsCommunityToolkit/releases/tag/v5.1.0)中可以了解更多的功能。
Windows 机器学习 | 我们重新制作了 Windows AI 文档，已将其拆分为三个区域：Windows 机器学习 (WinML)、Windows 视觉技能和直接机器学习 (DirectML)。 查看新的[登陆页](https://docs.microsoft.com/windows/ai/) </br> • [*MLGen* 体验](https://docs.microsoft.com/windows/ai/mlgen)即将在 Visual Studio 中发生变化。 在 Windows 10 版本 1903 和更高版本中，*mlgen* 不再包含在 Windows 10 SDK 中。 如果使用的是 VS 2017，则应下载并安装 Visual Studio 扩展 [Windows 机器学习代码生成器 VS 2017](https://marketplace.visualstudio.com/items?itemName=WinML.mlgen)。 如果使用的是 Visual Studio 2019，则应安装 [Windows 机器学习代码生成器](https://marketplace.visualstudio.com/items?itemName=WinML.mlgenv2)扩展。 </br> • 此外，我们自豪地宣布推出新的支持以精简代码。 现在，开发人员可以使用一项称作“精简代码”的技术（可通过 [WinMLTools 转换器](https://docs.microsoft.com/windows/ai/convert-model-winmltools)使用）来减少其机器学习模型的磁盘占用空间。
WinRT 合并参考 | 我们已添加 [WinRT 类型系统](https://docs.microsoft.com/uwp/winrt-cref/winrt-type-system)和 [WinMD 文件](https://docs.microsoft.com/uwp/winrt-cref/winmd-files)的完整说明，以提供有关 WinRT API 结构定义的具体深入说明。
适用于 Linux 的 Windows 子系统 (WSL) | [WSL 的最新更新](https://devblogs.microsoft.com/commandline/whats-new-for-wsl-in-windows-10-version-1903/)包括使用文件资源管理器从 Windows 访问 Linux 文件的功能，以及 wsl.exe 和 wslconfig.exe 的一些新命令。
Windows 视觉技能 | [Windows 视觉技能](https://docs.microsoft.com/windows/ai/windows-vision-skills)是一组 API，可用于创建人脸识别等“技能”，然后将其打包成可供其他应用使用的 NuGet 包，甚至不需要包含机器学习模型。

## <a name="publish--monetize-windows-apps"></a>发布 Windows 应用并实现盈利

功能 | 描述
 :------ | :------
MSIX | [Windows 10 内部版本 1709 和 1803 中的 MSIX 支持](https://docs.microsoft.com/windows/msix/msix-1709-and-1803-support)介绍了 Windows 10 版本 1809 以前的版本所支持的 MSIX 功能。
MSIX 打包和部署 | 我们引入了多项[修改包相关的改进](https://docs.microsoft.com/windows/msix/modification-package-insider-preview-build-18312)，以便更轻松地在 MSIX 包中打包自定义项。 这些改进包括包清单中的新 **rescap6:ModificationPackage** 元素、将主包中的文件取代为修改包的功能，以及将基于文件系统的插件打包成 MSIX 修改包的功能。
MSIX 打包工具 | • 我们已添加[在远程计算机上执行转换的支持](https://docs.microsoft.com/windows/msix/packaging-tool/remote-conversion-setup)。 我们还引入了 [MSIX 打包工具预览体验计划](https://docs.microsoft.com/windows/msix/packaging-tool/insider-program)，让用户提前访问新的工具功能。 </br> • [1709 和更高版本中的 MSIX 包支持](https://docs.microsoft.com/windows/msix/packaging-tool/support-on-1709-and-later)提供了有关使用 MSIX 打包工具生成专门于 Windows 10 版本 1709 和 1803 的包的指导。 </br> • [Hyper-V Quick Create 中的 MSIX 打包环境](https://docs.microsoft.com/windows/msix/packaging-tool/quick-create-vm)介绍了如何为 MSIX 打包项目创建虚拟环境。 </br> • [捆绑 MSIX 包](https://docs.microsoft.com/windows/msix/packaging-tool/bundle-msix-packages)提供了有关使用 MSIX 打包工具创建捆绑包的说明。 </br> • [Windows 10 版本 1809 中的修改包](https://docs.microsoft.com/windows/msix/modification-package-1809-update)提供了有关使用 MSIX 打包工具和 MakeApp.exe 创建适用于 Windows 10 版本 1809 及更高版本的修改包的说明。
MSIX SDK | [使用 MSIX SDK 生成可跨平台使用的包](https://docs.microsoft.com/windows/msix/msix-sdk/sdk-guidance)，并了解如何指定要将包提取到的目标平台。

## <a name="microsoft-learn"></a>Microsoft Learn

Microsoft Learn 为 Microsoft 开发人员提供新的动手学习和培训机会。

* 如果你对学习如何开发 Windows 应用感兴趣，请查看[我们的新学习路径](https://docs.microsoft.com/learn/paths/develop-windows10-apps/)，其中详细介绍了平台、工具以及如何编写最初的几个应用。

* 想要了解如何将 UI 功能添加到 Windows 应用？ 了解如何[创建 UI](https://docs.microsoft.com/learn/modules/create-ui-for-windows-10-apps/)、[在 UI 中添加导航和媒体](https://docs.microsoft.com/learn/modules/enhance-ui-of-windows-10-app/)，或[实现数据绑定](https://docs.microsoft.com/learn/modules/implement-data-binding-in-windows-10-app/)。

* 如果你对 Web 开发感兴趣，请查看[使用 Visual Studio Code 开发 Web 应用程序](https://docs.microsoft.com/learn/modules/develop-web-apps-with-vs-code/)或[构建简单的网站](https://docs.microsoft.com/learn/modules/build-simple-website/)。

* 或者，请尽情浏览 [Microsoft Learn 中的所有 Windows 开发人员模块](https://docs.microsoft.com/learn/browse/?products=windows&resource_type=module)。

## <a name="videos"></a>视频

### <a name="progressive-web-apps"></a>渐进式 Web 应用

渐进式 Web 应用是网站，功能类似于各种浏览器和各种 Windows 10 设备上的本机应用。 [观看视频](https://youtu.be/ugAewC3308Y)了解详细信息，然后[查看文档](https://aka.ms/Windows-PWA)以开始。

### <a name="vs-code-series"></a>VS Code 系列

查看[有关 Visual Studio Code 的新视频系列](https://www.youtube.com/playlist?list=PLlrxD0HtieHjQX77y-0sWH9IZBTmv1tTx)来了解 VSCode 的概念、其用法和创建方法。

### <a name="mixed-reality-services"></a>混合现实服务

我们最近已宣布推出 HoloLens 2。 请查看[有关混合现实的视频系列](https://www.youtube.com/watch?v=pdB7Ukf3u0I&list=PLlrxD0HtieHjh2Nt2BhcluIZeQg0uBZST)了解最新信息，以及如何参与和开始开发。

### <a name="one-dev-question"></a>一个开发问题

在“一个开发问题”视频系列中，资深的 Microsoft 开发人员介绍了关于 Windows 开发、团队文化和发展历程的一系列问题。

* [Raymond Chen：Windows 开发和历史](https://www.youtube.com/playlist?list=PLWs4_NfqMtoxjy3LrIdf2oamq1coolpZ7)

* [Larry Osterman：Windows 开发和历史](https://www.youtube.com/playlist?list=PLWs4_NfqMtoyPUkYGpJU0RzvY6PBSEA4K)

* [Aaron Gustafson：渐进式 Web 应用](https://www.youtube.com/playlist?list=PLWs4_NfqMtoyPHoI-CIB71mEq-om6m35I)

* [Chris Heilmann：webhint 工具](https://www.youtube.com/playlist?list=PLWs4_NfqMtow00LM-vgyECAlMDxx84Q2v)
