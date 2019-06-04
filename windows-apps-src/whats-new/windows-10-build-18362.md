---
title: 面向开发人员的 Windows 10 中的新增功能
description: Windows 10 生成 18362 和新开发人员工具提供工具、 功能和通用 Windows 平台为后盾的体验。
keywords: 是什么的新增功能新更新，更新功能，新的、 Windows 10 最新，开发人员，18362，可能会
ms.date: 04/19/2019
ms.topic: article
ms.localizationpriority: medium
ms.custom: 19H1
ms.openlocfilehash: 935d7f787d0cc23965c0fd51747b7687adb80a3f
ms.sourcegitcommit: a4fe508e62827a10471e2359e81e82132dc2ac5a
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/03/2019
ms.locfileid: "66468313"
---
# <a name="whats-new-in-windows-10-for-developers-build-18362"></a>对于开发人员，生成 18362，什么是 Windows 10 中的新增功能

Windows 10 生成 18362 (也称为 SDK 版本 1903年)，结合使用 Visual Studio 2019，提供了工具、 功能和体验，以使非同寻常的 Windows 应用。 只需在 Windows 10 上[安装工具和 SDK](https://go.microsoft.com/fwlink/?LinkId=821431)，你便可以随时[创建新的通用 Windows 应用](../get-started/create-uwp-apps.md)，或了解如何使用 [Windows 上的现有应用代码](../porting/index.md)。

该版本集合了 Windows 开发人员感兴趣的新增和改进功能及指南。 新的命名空间添加到 Windows SDK 的完整列表，请参阅[Windows 10 生成 18362 API 更改](windows-10-build-18362-api-diff.md)。 有关 Windows 10 突出功能的详细信息，请参阅 [Windows 10 中的酷炫功能](https://go.microsoft.com/fwlink/?LinkId=823181)。

## <a name="design--ui"></a>设计和 UI

功能 | 描述
:------ | :------
AnimatedVisualPlayer | [AnimatedVisualPlayer](https://docs.microsoft.com/uwp/api/microsoft.ui.xaml.controls.animatedvisualplayer) API 的宿主并控制播放的动画应用程序中的视觉对象。 此 API 用于控制和显示内容，如[Lottie](https://docs.microsoft.com/windows/communitytoolkit/animations/lottie)视觉对象，可用于呈现本机应用程序中的 Adobe 后果一览无余动画。
CompactDensity | 启用[简洁模式](../design/style/spacing.md)应用程序中启用广泛、 丰富信息的控件组。 这可以帮助解决浏览大量内容，最大化可见内容在页上，或帮助导航和交互时用户正在使用指针输入。
项 Repeater | [ItemsRepeater](../design/controls-and-patterns/items-repeater.md)控件可以生物用于向用户显示集合的自定义体验。 ItemsRepeater 不提供全面的最终用户体验或默认 UI。 相反，它是一个可用于创建您自己的唯一基于集合的体验和自定义控件的构造块。
教学提示 | 一个[教学提示](../design/controls-and-patterns/dialogs-and-flyouts/teaching-tip.md)是提供的上下文信息的半持久性和大量内容的弹出窗口。 有关通知，提醒，并讲授有关新的或重要的功能的用户，可以使用此控件。
用户界面发出命令 | 与[UWP 应用中发出命令](../design/controls-and-patterns/commanding.md)，使用[XamlUICommand](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.standarduicommand)并[StandardUICommand](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.standarduicommand)类 （以及 ICommand 接口） 来共享和管理跨不同的命令控件类型，而不考虑所使用的设备和输入类型。
Windows UI 库 | 最新官方版本的 Windows 用户界面库 – [WinUI 2.1](https://docs.microsoft.com/uwp/toolkits/winui/release-notes/winui-2.1) – 提供充满活力的新 XAML 控件，用于 Windows 应用程序。 WinUI 库 API 可以在较早的 Windows 10 版本上运行，所以你不需要添加版本检查或条件 XAML 以支持未使用最新版本的用户。
可视化层中的桌面应用程序 | 现在可以[使用桌面应用程序中的 UWP Visual 层 Api](../composition/visual-layer-in-desktop-apps.md)。 这些 Api 的图形、 效果和动画，提供高性能模式下重新训练 API，是跨 Windows 设备用户界面的基础。
Z 深度和阴影 | 使用[Z 深度和卷影](../design/layout/depth-shadow.md)若要在 UWP 应用中创建提升。 这项新增功能的可以使应用程序的 UI 更轻松地扫描，并更好地传达什么是让用户能够专注于重要。

## <a name="develop-windows-apps"></a>开发 Windows 应用

功能 | 描述
:------ | :------
反恶意软件扫描界面 (AMSI) | 了解[反恶意软件扫描界面 (AMSI) 如何帮助你防范恶意软件](https://docs.microsoft.com/windows/desktop/amsi/how-amsi-helps)，然后查看[示例代码](https://docs.microsoft.com/windows/desktop/amsi/dev-audience)若要了解如何在桌面应用程序中实现它。
C++/WinRT 2.0 | 版本 2.0 的C++/WinRT 已发布。 请查看[新增功能C++/WinRT](../cpp-and-winrt-apis/news.md)的所有新更改和添加将完整逐一介绍。
选择平台 | 在创建新的桌面应用程序感兴趣？ 请查看我们改进[选择你的平台](https://docs.microsoft.com/windows/desktop/choose-your-technology)页的详细的说明和 UWP、 WPF 和 Windows 窗体平台和 Win32 API 的详细信息的比较。
对话式代理 | [Windows.ApplicationModel.ConversationalAgent](https://docs.microsoft.com/uwp/api/windows.applicationmodel.conversationalagent)命名空间，可以添加 Windows 平台代理激活运行时 (AAR) 向 Windows 应用程序支持的任何数字协助。
云文件 API | *云文件 API* ，您可以[构建一个云同步引擎，支持占位符文件](https://docs.microsoft.com/windows/desktop/cfapi/build-a-cloud-file-sync-engine)。
直接 3D 12 | [Direct3D 12 中呈现传递](/windows/desktop/direct3d12/direct3d-12-render-passes)可以提高您的呈现器的性能如果它具有基于基于磁贴的延迟呈现 (TBDR) 以及其他技术。 方法可帮助您通过启用应用程序以更好地识别资源呈现排序要求和数据依赖关系提高 GPU 效率的呈现器。 这将减少内存进出关闭芯片内存。
直接机器学习 (DirectML) | [DirectML](https://docs.microsoft.com/windows/desktop/direct3d12/dml)是机器学习的低级别的硬件加速 API。 它具有熟悉 (本机C++，nano COM) 编程接口和 DirectX 12 的样式中的工作流。 您可以将机器学习集成到您的游戏、 引擎、 中间件、 后端或其他应用程序的推断工作负荷。 DirectML 受所有 DirectX 12 兼容硬件。
DirectX HLSL | [HLSL 着色器模型 6.4](https://docs.microsoft.com/windows/desktop/direct3dhlsl/hlsl-shader-model-6-4-features-for-direct3d-12) DirectML 用于提供新的机器学习内部函数。
驱动程序开发 | 新的音频、 照相机、 显示、 网络、 移动宽带、 打印、 传感器、 存储和 wifi 功能已添加的 Windows 驱动程序开发人员。 请查看[什么是驱动程序开发中的新增功能](https://docs.microsoft.com/windows-hardware/drivers/what-s-new-in-driver-development#whats-new-in-windows-10-version-1903-latest)的更多详细信息。
文件系统操作 | 这[最佳做法指南](../files/best-practices-for-writing-to-files.md)可以帮助你充分利用 Windows.Storage.FileIO 和 Windows.Storage.PathIO 类来执行文件系统 I/O 操作。
游戏板和遥控器交互 | 使用[游戏手柄和远程控制交互](../design/input/gamepad-and-remote-interactions.md)构建易于使用，并且可访问的交互体验。 这些交互，你的应用程序可以是一样直观且易于使用从两个英尺远，因为它是从 10 英尺远。
日语纪元更改 | 我们提供了[这些说明](../design/globalizing/japanese-era-change.md)展示如何以确保你的 Windows 应用程序已准备好日语纪元更改集上 2019 年 5 月 1 日，才能开始。 [此页也会出现在日语](https://docs.microsoft.com/ja-jp/windows/uwp/design/globalizing/japanese-era-change)。
WPF、 Windows 窗体和 WinUI 的开放源代码 | WPF、 Windows 窗体和 WinUI UX 框架现可在 GitHub 上的开放源代码发布内容。 有关详细信息和链接，请参阅[构建 Windows 应用博客](https://blogs.windows.com/buildingapps/2018/12/04/announcing-open-source-of-wpf-windows-forms-and-winui-at-microsoft-connect-2018/#OKZjJs1VVTrMMtkL.97)。
适用于 Xbox 渐进式 Web 应用 | 与[渐进式 Web 应用，适用于 Xbox One](https://docs.microsoft.com/microsoft-edge/progressive-web-apps/xbox-considerations)，可以扩展 web 应用程序并将其提供为 Xbox One 的应用程序通过 Microsoft Store 同时仍继续使用现有框架、 CDN 和服务器的后端。 大多数情况下，可以在相同的方式为 Windows for Xbox One 打包在 PWA。 本指南将引导你完成该过程中，并突出显示的主要差异。
Project Rome | 项目罗马 SDK 现已推出适用于 Android 和 iOS。 了解如何将图形通知与每个平台集成：[Android](https://docs.microsoft.com/windows/project-rome/notifications/how-to-guide-for-android)并[iOS](https://docs.microsoft.com/windows/project-rome/notifications/how-to-guide-for-ios)。
远程摄像头 | 使用到 DeviceWatcher 类[连接到远程摄像头](../audio-video-camera/connect-to-remote-cameras.md)，并从这些相机的帧读入您的 Windows 应用程序。
桌面应用程序 （XAML 群岛） 中的 UWP 控件 | 适用于 WPF、 Windows 窗体中的 UWP 控件承载的 Windows SDK 中的 Api 和C++Win32 桌面应用程序已不存在于开发者预览版。 有关详细信息，请参阅[桌面应用程序中的 UWP 控件](../xaml-platform/xaml-host-controls.md)。
Visual Studio 2019 | Visual Studio 2019 已发布，使用最新工具和服务对任何开发人员、 应用或平台。 请查看[什么是 Visual Studio 2019 中的新增功能](https://docs.microsoft.com/visualstudio/ide/whats-new-visual-studio-2019?view=vs-2019)若要了解最新版本，以及如何开始。
Win32 WebView | 我们[方面的常见问题](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/webview#frequently-asked-questions-faqs)使用 Microsoft Edge web 视图在桌面应用程序，以及指向示例和其他资源时提供常见问题的解答。
Windows 命令行 | [新的控制台功能](https://devblogs.microsoft.com/commandline/new-experimental-console-features/)包括实验性终端选项卡，并滚动、 光标形状和光标颜色设置。 了解详细信息[Windows 命令行工具适用于开发者博客](https://devblogs.microsoft.com/commandline/)。
Windows 社区工具包 | Windows 社区工具包 v5.1 提供对动画、 远程设备、 剪切、 映像和可访问性的令人兴奋的更新。 </br> • 新[Lottie Windows 库](https://docs.microsoft.com/windows/communitytoolkit/animations/lottie)通过利用 Windows.UI.Composition Api 中，Windows 10 (1809) 上提供高质量动画支持并允许进行的消耗[Bodymovin](https://aescripts.com/bodymovin/) JSON 文件或优化代码生成的 Windows 应用程序中播放的类。 尝试使用全新[Lottie 查看器应用](https://aka.ms/lottieviewer)从 Microsoft Store 以测试动画，并生成 Windows 应用的优化的代码。 </br> • 新[远程设备选取器](https://docs.microsoft.com/windows/communitytoolkit/controls/remotedevicepicker)允许用户选择一个设备 (proximally 或可访问的云)、 启动该设备上的应用或与远程设备上的应用服务进行通信。 </br> • 新[ImageCropper 控件](https://docs.microsoft.com/windows/communitytoolkit/controls/imagecropper)集成了裁剪功能选择个人资料图片或使用照片编辑工具。 </br> • 此外，已在控件上的辅助功能改进[Microsoft.Toolkit.Win32](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32) 6.0 预览包更新为 WPF 和 WinForms 和更多的功能，你可以在中了解[发行说明](https://github.com/windows-toolkit/WindowsCommunityToolkit/releases/tag/v5.1.0).
Windows 机器学习 | 我们已重新设计 Windows AI 文档，将它们拆分成三个区域：Windows 机器学习 (WinML)、 Windows 视觉技能和直接的机器学习 (DirectML)。 查看全新[登陆页面](https://docs.microsoft.com/windows/ai/) </br> • [ *MLGen*体验](https://docs.microsoft.com/windows/ai/mlgen)在 Visual Studio 中更改。 在 Windows 10，版本 1903年及更高版本， *mlgen*不再包含在 Windows 10 SDK。 如果你使用 VS 2017，而是应下载和安装 Visual Studio 扩展[Windows 机器学习代码生成器 VS 2017](https://marketplace.visualstudio.com/items?itemName=WinML.mlgen)。 如果使用的 Visual Studio 2019，则应安装[Windows 机器学习代码生成器](https://marketplace.visualstudio.com/items?itemName=WinML.mlgenv2)扩展。 </br> • 我们还自豪地公布推出新打包的权重的支持。 开发人员现在可以减少的磁盘占用其机器学习模型通过使用一种称为权重打包，可通过技术[WinMLTools 转换器](https://docs.microsoft.com/windows/ai/convert-model-winmltools)。
合并的 WinRT 引用 | 我们已添加的完整说明[WinRT 类型系统](https://docs.microsoft.com/uwp/winrt-cref/winrt-type-system)并[WinMD 文件](https://docs.microsoft.com/uwp/winrt-cref/winmd-files)，以提供有关结构的 WinRT Api 定义的特定深入说明。
适用于 Linux 的 Windows 子系统 (WSL) | [最新更新 WSL](https://devblogs.microsoft.com/commandline/whats-new-for-wsl-in-windows-10-version-1903/)包括能够从 Windows 使用文件资源管理器，以及一些新命令 wsl.exe 和 wslconfig.exe 访问 Linux 文件。
Windows 视觉技能 | [Windows 视觉技能](https://docs.microsoft.com/windows/ai/windows-vision-skills)是一组 Api，可用于创建"技能，"如面部识别，然后将它们打包为 NuGet 包，它可以使用其他应用，而无需包括机器学习模型。

## <a name="publish--monetize-windows-apps"></a>发布 Windows 应用并实现盈利

功能 | 描述
 :------ | :------
MSIX | [MSIX 支持在 Windows 10 版本 1709年和 1803年](https://docs.microsoft.com/windows/msix/msix-1709-and-1803-support)介绍在 Windows 10，版本 1809年之前的版本上支持哪些 MSIX 功能。
MSIX 打包和部署 | 我们引入了若干[改进与修改包](https://docs.microsoft.com/windows/msix/modification-package-insider-preview-build-18312)，使其易于包自定义项 MSIX 包中。 这些改进包括新**rescap6:ModificationPackage**基于插件的包清单，能够重写的文件中修改包中，主包和包文件系统的功能中的元素为 MSIX 修改包。
MSIX 打包工具 | • 我们添加了[支持在远程计算机上执行转换的转换](https://docs.microsoft.com/windows/msix/packaging-tool/remote-conversion-setup)。 我们还引入了[MSIX 打包工具预览体验计划](https://docs.microsoft.com/windows/msix/packaging-tool/insider-program)提供提前访问新工具功能。 </br> • [MSIX 程序包支持 1709年及更高版本](https://docs.microsoft.com/windows/msix/packaging-tool/support-on-1709-and-later)提供有关使用 MSIX 打包工具专门适用于 Windows 10，版本 1709年和 1803年中生成包的指南。 </br> • [MSIX 打包环境上的 HYPER-V 快速创建](https://docs.microsoft.com/windows/msix/packaging-tool/quick-create-vm)演示如何创建的虚拟环境的 MSIX 打包项目。 </br> •[捆绑 MSIX 包](https://docs.microsoft.com/windows/msix/packaging-tool/bundle-msix-packages)提供创建包捆绑包使用 MSIX 打包工具的说明。 </br> •[修改包在 Windows 10 版本 1809年](https://docs.microsoft.com/windows/msix/modification-package-1809-update)包含创建使用 MSIX 打包工具和 MakeApp.exe 1809 及更高版本的 Windows 10 版本的修改包的说明。
MSIX SDK | [使用 MSIX SDK 来构建跨平台使用的包](https://docs.microsoft.com/windows/msix/msix-sdk/sdk-guidance)，并了解如何指定想将包提取到的目标平台。

## <a name="microsoft-learn"></a>Microsoft Learn

Microsoft 了解到 Microsoft 开发人员提供新的动手学习和培训机会。

* 如果有兴趣学习如何开发 Windows 应用，请查看[我们新的学习路径](https://docs.microsoft.com/learn/paths/develop-windows10-apps/)平台、 工具，以及如何编写首个几个应用的全面介绍。

* 想要了解如何将 UI 功能添加到 Windows 应用？ 了解如何[创建的用户界面](https://docs.microsoft.com/learn/modules/create-ui-for-windows-10-apps/)，[添加到你的 UI 的导航和媒体](https://docs.microsoft.com/learn/modules/enhance-ui-of-windows-10-app/)，或[实现数据绑定](https://docs.microsoft.com/learn/modules/implement-data-binding-in-windows-10-app/)。

* 如果有兴趣 Web 开发，请查看[开发 web 应用程序使用 Visual Studio Code](https://docs.microsoft.com/learn/modules/develop-web-apps-with-vs-code/)或[构建简单的网站](https://docs.microsoft.com/learn/modules/build-simple-website/)。

* 或者，也可以浏览[Microsoft 了解上的所有 Windows 开发人员模块](https://docs.microsoft.com/learn/browse/?products=windows&resource_type=module)。

## <a name="videos"></a>视频

### <a name="progressive-web-apps"></a>渐进式 Web 应用

渐进式 Web 应用是功能上相当于本机应用程序跨不同的浏览器和各种 Windows 10 设备的 web 站点。 [观看视频](https://youtu.be/ugAewC3308Y)若要了解详细信息，然后[签出文档](https://aka.ms/Windows-PWA)若要开始。

### <a name="vs-code-series"></a>VS 代码系列

请查看我们[Visual Studio Code 上的新视频系列](https://www.youtube.com/playlist?list=PLlrxD0HtieHjQX77y-0sWH9IZBTmv1tTx)VSCode 是什么、 如何使用它，以及它的创建方式有关的信息。

### <a name="mixed-reality-services"></a>混合的现实服务

最近宣布 HoloLens 2。 请查看此[视频系列混合现实](https://www.youtube.com/watch?v=pdB7Ukf3u0I&list=PLlrxD0HtieHjh2Nt2BhcluIZeQg0uBZST)为最新信息，以及如何参与此计划并开始开发。

### <a name="one-dev-question"></a>一个适用于开发人员问题

在开发人员的一个问题视频系列中，资深 Microsoft 开发者介绍一系列的有关 Windows 开发、 团队区域性和历史记录的问题。

* [Raymond Chen 在 Windows 开发和历史记录](https://www.youtube.com/playlist?list=PLWs4_NfqMtoxjy3LrIdf2oamq1coolpZ7)

* [Larry Osterman 在 Windows 开发和历史记录](https://www.youtube.com/playlist?list=PLWs4_NfqMtoyPUkYGpJU0RzvY6PBSEA4K)

* [Aaron Gustafson 渐进式 Web 应用上](https://www.youtube.com/playlist?list=PLWs4_NfqMtoyPHoI-CIB71mEq-om6m35I)

* [Chris Heilmann webhint 工具](https://www.youtube.com/playlist?list=PLWs4_NfqMtow00LM-vgyECAlMDxx84Q2v)
