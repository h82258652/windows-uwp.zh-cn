---
title: Windows 10 内部版本 19041 中的新增功能
description: Windows 10 内部版本 18362 和新的开发人员工具提供由 Windows 10 支持的工具、功能和体验。
keywords: 新增功能, 新功能, Windows, Windows 10, 更新, 刷新, 功能, 新, 最新, 开发人员, 19041, 五月
ms.date: 05/12/2020
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: bb7630afd6cc69497494a2e86e6c5e3544acefec
ms.sourcegitcommit: f806d5f3b0c1e046c903d3388092c0e059d21858
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/22/2020
ms.locfileid: "83790989"
---
# <a name="whats-new-for-developers-in-windows-10-build-19041"></a>Windows 10 内部版本 19041 中面向开发人员的新增功能

Windows 10 内部版本 19041（也称为 SDK 版本 2004）与 Visual Studio 2019 及相关工具和功能结合，为你提供创建出色 Windows 应用所需的一切。 只需在 Windows 10 上[安装工具和 SDK](https://developer.microsoft.com/windows/downloads#_blank)，你便可以随时[创建新的通用 Windows 应用](../get-started/create-uwp-apps.md)，或了解如何使用 [Windows 上的现有应用代码](../porting/index.md)。

该版本集合了 Windows 开发人员感兴趣的新增和改进功能及指南。 有关已添加到 Windows SDK 的新命名空间的完整列表，请参阅 [Windows 10 内部版本 19041 API 更改](windows-10-build-19041-api-diff.md)。 有关 Windows 10 中亮点功能的详细信息，请参阅 [Windows 10 中的酷炫功能](https://developer.microsoft.com/windows/windows-10-for-developers)。

## <a name="windows-10-apps"></a>Windows 10 应用

功能 | 说明
:------ | :------
蓝牙音频播放 | [启用来自远程蓝牙连接设备的音频播放](../audio-video-camera/enable-remote-audio-playback.md)介绍如何使用 [AudioPlaybackConnection](/uwp/api/windows.media.audio.audioplaybackconnection) 启用蓝牙连接的远程设备以播放本地计算机上的音频，从而实现诸如将电脑配置为模拟蓝牙扬声器的功能以及允许用户从手机收听音频等方案。
C# 应用移植 | 我们已对将 C# 应用程序移植到 C++/WinRT 的过程进行了记录。 [将剪贴板示例从 C# 移植到 C++/WinRT](/windows/uwp/cpp-and-winrt-apis/clipboard-to-winrt-from-csharp) 与上下文相关，并且基于特定的实际移植体验。 与其关联的主题[从 C# 移动到 C++/WinRT](/windows/uwp/cpp-and-winrt-apis/move-to-winrt-from-csharp) 对移植中所涉及的技术细节和步骤进行了更为详尽的介绍。 
C++/WinRT | 在[最新改进功能/新增功能汇总](/windows/uwp/cpp-and-winrt-apis/news#rollup-of-recent-improvementsadditions-as-of-march-2020)中，了解有关 C++/WinRT 针对生成时和运行时的性能改进的更新（与 Visual C++ 编译器团队共同实现）。 </br> 对于 C++/WinRT，我们向以下这些主题添加了更多信息：[从 C++/CX 进行移植](/windows/uwp/cpp-and-winrt-apis/move-to-winrt-from-cx#boxing-and-unboxing)、[从 C# 进行移植](/windows/uwp/cpp-and-winrt-apis/move-to-winrt-from-csharp#boxing-and-unboxing)、[简单 C++/WinRT Windows UI 库示例](/windows/uwp/cpp-and-winrt-apis/simple-winui-example)、[并发](/windows/uwp/cpp-and-winrt-apis/concurrency)、[get_unknown()](/uwp/cpp-ref-for-winrt/get-unknown) 以及[使用 C++/WinRT 创建 XAML 自定义（模板化）控件](/windows/uwp/cpp-and-winrt-apis/xaml-cust-ctrl)。
DirectX | 对于多个以前版本的 Windows（从 Creators Update 到 Windows 10，版本 1903），我们引入了几个与 DirectX 相关的最新“新增功能”主题。 [DirectWrite 中的新增功能](/windows/win32/directwrite/what-s-new-in-directwrite-for-windows-8-consumer-preview)、[DXGI 1.6 改进功能](/windows/win32/direct3ddxgi/dxgi-1-6-improvements)和 [Direct3D 12 中的新增功能](/windows/win32/direct3d12/new-releases)。
DirectXMath | 我们发布了 21 个新的 DirectXMath 主题，其中涵盖两个矩阵结构及其成员函数和自由函数。 例如 [XMFLOAT3X4 结构](/windows/win32/api/directxmath/ns-directxmath-xmfloat3x4)。
Direct3D | [使用具有高动态范围显示和高级颜色的 DirectX](/windows/win32/direct3darticles/high-dynamic-range) 提供了 Windows 高动态范围应用的最佳做法列表。 </br> 新 [ID3D11On12Device2](/windows/win32/api/d3d11on12/nn-d3d11on12-id3d11on12device2) 接口及其方法，使你能够获取通过 Direct3D 11 API 所创建的资源，并在 Direct3D 12 中使用它们。
Direct3D 12 | 已添加 [Direct3D 12 Core 1.0 功能级别](/windows/win32/direct3d12/core-feature-levels)，供仅用于计算的设备使用。 </br> 已为 [ID3D12Debug3 接口](/windows/win32/api/d3d12sdklayers/nn-d3d12sdklayers-id3d12debug3)添加新主题。
Direct ML | 已将 18 个运算符添加到 DirectML，DirectML 是一个低级别硬件加速 API，WinML 基于此 API 进行生成。 例如 [DML_ACTIVATION_SHRINK_OPERATOR_DESC 结构](/windows/win32/api/directml/ns-directml-dml_activation_shrink_operator_desc)。
错误报告 | RoFailFastWithErrorContextInternal2 函数已添加到 Win32 中，这会引发一个异常，该异常可能包含其他错误上下文。
机器学习 | Windows 机器学习[现在支持 ONNX 版本 1.4 和 opset 9](/windows/ai/windows-ml/release-notes)。 </br>  使用 [CloseModelOnSessionCreation](https://docs.microsoft.com/uwp/api/windows.ai.machinelearning.learningmodelsessionoptions.closemodelonsessioncreation?view=winrt-19041) API，可以在不再需要某个学习模型时自动关闭该模型，从而节省内存。
WLAN | 添加了几个新的本机 WiFi 函数和结构，如 [WlanDeviceServiceCommand 函数](/windows/win32/api/wlanapi/nf-wlanapi-wlandeviceservicecommand)。
Wi-Fi 热点 2 | [通过网站预配 Wi-Fi 配置文件](/windows/win32/nativewifi/prov-wifi-profile-via-website)描述了 Wi-Fi 热点 2 的新功能。
Windows Holographic 互操作 | 添加了 [`windows.graphics.holographic.interop.h`](/windows/win32/api/windows.graphics.holographic.interop) 标头以及 17 个 Win32 API。 这些 API 用于在 Win32 和 Windows 运行时之间进行互操作。 虽然在 Windows 10 内部版本 18362 中添加了这些 API，但该标头是内部版本 19041 的新标头。
Windows 套接字 | 已对 Windows 套接字 2 SPI 内容进行了增强。 我们改进和扩充的众多主题中的一个示例是 [LPWSPEVENTSELECT 回调函数](/windows/win32/api/ws2spi/nc-ws2spi-lpwspeventselect)主题。
XAML 岛 - 基础知识 | 使用 XAML 岛在桌面 Windows 应用中托管 UWP XAML 控件。 了解如何[在 WPF 应用中托管标准 UWP 控件](/windows/apps/desktop/modernize/host-standard-control-with-xaml-islands)以及如何[在 C++ Win32 应用中托管标准 UWP 控件](/windows/apps/desktop/modernize/host-standard-control-with-xaml-islands-cpp)。
XAML 岛 - 自定义控件 | [Microsoft.Toolkit.Win32.UI.XamlApplication](https://www.nuget.org/packages/Microsoft.Toolkit.Win32.UI.XamlApplication) 和 [Microsoft.Toolkit.Win32.UI.SDK](https://www.nuget.org/packages/Microsoft.Toolkit.Win32.UI.SDK) NuGet 包使得在 .NET 和 C++ Win32 应用中托管自定义 UWP XAML 控件变得更加容易。 </br> 若要进行分步演练，请参阅[在 WPF 应用中托管自定义 UWP 控件](/windows/apps/desktop/modernize/host-custom-control-with-xaml-islands)和[在 C++ Win32 应用中托管自定义 UWP 控件](/windows/apps/desktop/modernize/host-custom-control-with-xaml-islands-cpp)。 </br> 最后，有关更复杂的 C++ Win32 方案的指南，请参阅 [XAML 岛的高级方案](/windows/apps/desktop/modernize/advanced-scenarios-xaml-islands-cpp)。

## <a name="build-with-windows"></a>使用 Windows 进行生成

功能 | 说明
:------ | :------
Windows 开发环境 | [Windows 开发环境](/windows/dev-environment/)文档提供了有关使用 Windows 跨各种平台进行开发所需的资源，有助于你实现可能具有的任何开发目标。
Windows 上的 Python | [Windows 上的 Python](/windows/python/) 部分为刚接触 Python 语言的开发人员以及希望使用 Windows 上提供的其他工具来优化其 Python 开发的开发人员提供信息。 了解如何为 [Web 开发](/windows/python/web-frameworks)和[数据库交互](/windows/python/databases)设置 Python 环境。
Windows 上的 NodeJS | [推荐用于 Node.js 开发环境的设置](/windows/nodejs/setup-on-wsl2)为在 Linux 服务器进行部署的高级开发人员提供了详细指南。 此外，还提供了[常见 Node.js Web 框架](/windows/nodejs/web-frameworks)、[数据库交互](/windows/nodejs/databases)和 [Docker 容器](/windows/nodejs/containers)的设置说明。
Mac 到 Windows | [更改开发环境指南](/windows/dev-environment/mac-to-windows)主要面向将其开发平台从 Mac 过渡到 Windows 的用户，并为类似的快捷方式和开发实用程序提供映射。
Windows 终端 | 面向命令行工具和 shell（如命令提示符、PowerShell 和适用于 Linux 的 Windows 子系统 (WSL)）用户的[新式终端应用程序](https://www.microsoft.com/p/windows-terminal/9n0dx20hk701?activetab=pivot:overviewtab)。 它的主要功能包括多个选项卡、窗格、Unicode 和 UTF-8 字符支持、GPU 加速文本呈现引擎，还可以用于创建你自己的主题并自定义文本、颜色、背景和快捷键绑定。
WSL 2 | 现在提供[新版本的适用于 Linux 的 Windows 子系统 (WSL)](/windows/wsl/wsl2-about)。 WSL 2 具有重新配置的体系结构，可以在 Windows 上运行实际的 Linux 内核，从而提高了文件系统的性能并增加了全系统调用兼容性。 这一新的体系结构改变了 Linux 二进制文件与 Windows 和计算机硬件进行交互的方式，但仍然提供与之前版本的 WSL 中相同的用户体验。 每个单独的 Linux 发行版都可以作为 WSL1 或 WSL2 发行版运行，可以并行运行，且可以随时进行更改。 </br> [安装 WSL 2](https://docs.microsoft.com/windows/wsl/wsl2-install) 以开始使用。 </br> 探索有关 [WSL 1 和 WSL 2 之间的用户体验更改](https://docs.microsoft.com/windows/wsl/wsl2-ux-changes)的进一步信息。 </br> 查看[有关 WSL 2 的常见问题解答](https://docs.microsoft.com/windows/wsl/wsl2-faq)。

## <a name="msix-packaging-and-deployment"></a>MSIX、打包和部署

功能 | 说明
:------ | :------
MSIX | 自上次发布 Windows 10 SDK 后，我们对 [MSIX 打包格式](/windows/msix/overview)进行了重要更新。 
包含服务的打包 | MSIX 和 MSIX 打包工具[现在支持包含服务的应用包](/windows/msix/packaging-tool/convert-an-installer-with-services)。
MSIX 包中的脚本 | 你可以[使用包支持框架 (PSF) 在 MSIX 应用包中运行脚本](/windows/msix/psf/run-scripts-with-package-support-framework)，这使得 IT 专业人员能够在使用 MSIX 将应用程序打包后，根据用户环境动态​​自定义应用程序。
强制实施包完整性 | 你现在可以通过使用程序包清单中的 [uap10:PackageIntegrity 元素](/uwp/schemas/appxpackage/uapmanifestschema/element-uap10-packageintegrity)对 MSIX 包的内容强制实施包完整性。 通过 MSIX 打包工具创建 MSIX 包时，也可以强制实施包完整性。
稀疏包 | 你可以通过生成稀疏包并将其注册到应用来[向未打包到 MSIX 包中的桌面应用授予包标识符](/windows/apps/desktop/modernize/grant-identity-to-nonpackaged-apps)。 此功能使得尚不能采用 MSIX 打包方式进行部署的桌面应用能够使用需要程序包标识符的 Windows 10 可扩展性功能。
托管应用 | 你现在可以[创建托管应用](/windows/uwp/launch-resume/hosted-apps)。 托管应用与父托管应用共享相同的可执行文件和定义，但其外观和行为类似于系统上的单独应用。 对于希望组件（如可执行文件或脚本文件）的行为类似于独立 Windows 10 应用，但该组件需要托管进程才能进行执行的应用场景，托管应用会很有用。 托管应用可以具有其自己的开始磁贴、标识，且可以与 Windows 10 功能（如后台任务、通知、磁贴和共享目标）深度集成。

## <a name="windows-ui-library-winui"></a>Windows UI 库 (WinUI)

功能 | 说明
:------ | :------
WinUI 2.4 | [WinUI 2.4](/uwp/toolkits/winui/release-notes/winui-2.4) 是 Windows UI 库的最新公共版本。 所有版本的 WinUI 都为 Windows 应用提供了广泛的官方 UI 控件，并且它们是作为独立于 Windows SDK 的 NuGet 包提供的，因此适用于早期版本的 Windows 10。 [按照这些说明](/uwp/toolkits/winui)安装 WinUI。
RadialGradientBrush | WinUI 2.4 中的新增功能，[RadialGradientBrush](../design/style/brushes.md#radial-gradient-brushes) 在椭圆内绘制，该椭圆由 Center、RadiusX 和 RadiusY 属性定义。 渐变的颜色开始于椭圆的中心之处，结束于半径之处。
ProgressRing | WinUI 2.4 中的新增功能，[ProgressRing 控件](../design/controls-and-patterns/progress-controls.md)用于模式交互，其中用户会受到阻止，直到 ProgressRing 消失。 如果操作要求在该操作完成之前挂起与应用的大多数交互，请使用此控件。
TabView | [TabView 控件](../design/controls-and-patterns/tab-view.md)的更新提供了对选项卡呈现方式的更多控制。 你可以设置未选定选项卡的宽度并仅显示一个图标以节省屏幕空间，还可以隐藏未选定选项卡上的“关闭”按钮，直到用户将鼠标悬停在该选项卡上时才进行显示。
TextBox 控件 | 如果启用了深色主题，则默认情况下，TextBox 系列控件的背景色在文本插入时仍保持为深色。 受影响的控件有 [TextBox](/uwp/api/windows.ui.xaml.controls.textbox)、[RichEditBox](/uwp/api/windows.ui.xaml.controls.richtextblock)、[PasswordBox](/uwp/api/windows.ui.xaml.controls.passwordbox)、[Editable ComboBox](/uwp/api/windows.ui.xaml.controls.combobox) 和 [AutoSuggestBox](/uwp/api/windows.ui.xaml.controls.autosuggestbox)。
NavigationView | [NavigationView 控件](/uwp/api/microsoft.ui.xaml.controls.navigationview)现在支持分层导航，并包括 Left、Top 和 LeftCompact 显示模式。 对于显示页面类别、标识带有相关子页面的页面，或在具有链接到许多其他页面的中心式页面的应用内使用，分层的 NavigationView 非常有用。
Windows UI 库 | XAML 控件库中提供了每个 WinUI 功能的示例。 可在 [Microsoft Store](https://www.microsoft.com/p/xaml-controls-gallery/9msvh128x2zt) 上下载它，或[在 Github 上查看源代码](https://github.com/Microsoft/Xaml-Controls-Gallery)。
以前的版本 | 自 Windows 10 SDK 的上一个主要版本发布以来，我们还发布了 [WinUI 2.3](/uwp/toolkits/winui/release-notes/winui-2.3) 和 [WinUI 2.2](/uwp/toolkits/winui/release-notes/winui-2.2)，这为 Windows 开发人员提供了更多新的 UI 功能。

## <a name="samples"></a>示例

以下示例应用已更新为面向 Windows 10 内部版本 19041。

* [远程会话（问答游戏）](https://github.com/microsoft/Windows-appsample-remote-system-sessions)
* [客户订单数据库](https://github.com/Microsoft/Windows-appsample-customers-orders-database)
* [RSS 阅读器](https://github.com/Microsoft/Windows-appsample-rssreader)
* [大理石迷宫](https://github.com/Microsoft/Windows-appsample-marble-maze)
* [照片编辑器](https://github.com/Microsoft/Windows-appsample-photo-editor)
* [Lunch Scheduler](https://github.com/Microsoft/Windows-appsample-lunch-scheduler)
* [Coloring Book](https://github.com/Microsoft/Windows-appsample-coloringbook)
* [色调光源控制器](https://github.com/Microsoft/Windows-appsample-huelightcontroller)
* [照片实验室](https://github.com/Microsoft/Windows-appsample-photo-lab)
* [系列说明](https://github.com/Microsoft/Windows-appsample-familynotes)

## <a name="videos"></a>视频

### <a name="windows-terminal-the-secret-to-command-line-happiness"></a>Windows 终端：快乐使用命令行的秘诀！

了解如何为工作流自定义 Windows 终端，并观看其运行中功能的演示。 [观看视频](https://www.youtube.com/watch?v=2dsnwlnNBzs)，然后[阅读文档](https://github.com/microsoft/terminal#terminal--console-overview)以了解详细信息。

### <a name="wsl2-code-faster-on-the-windows-subsystem-for-linux"></a>WSL2：在适用于 Linux 的 Windows 子系统上更快地进行编码

了解有关 WSL2（适用于 Linux 的 Windows 子系统新版本）以及为提高性能所进行的更改的全部信息。 [观看视频](https://www.youtube.com/watch?v=MrZolfGm8Zk)，然后[阅读文档](/windows/wsl/wsl2-about)以了解详细信息。

### <a name="msix-package-desktop-apps-for-windows-10-replace-outdated-installers"></a>MSIX：适用于 Windows 10 的包桌面应用。 替换已过期的安装程序。

了解 MSIX（用于安装 Windows 应用的包格式），包括如何使用 Visual Studio 打包现有代码以及如何部署和分配应用。 [观看视频](https://www.youtube.com/watch?v=yhOnClQrvBk)，然后[阅读文档](/windows/msix/)以了解详细信息。
