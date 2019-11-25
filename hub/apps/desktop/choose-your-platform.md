---
Description: 若要创建新的桌面应用，首先要做的决定是使用 Win32 和 COM API 还是 .NET。
ms.assetid: 82705644-F1F0-40F3-99B1-7A97BFB32831
title: 选择应用平台
ms.topic: article
ms.date: 11/04/2019
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: medium
keywords: windows win32, 桌面开发
ms.openlocfilehash: d0d87f8e4b6524471ff5e2ada9012a22641b06d7
ms.sourcegitcommit: ddf0137929945eddf01041a81aa4d26038e70f46
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/22/2019
ms.locfileid: "74392094"
---
# <a name="choose-your-app-platform"></a>选择应用平台

若要为 Windows 电脑创建新的桌面应用程序，首先要做的决定是使用哪个应用程序平台。 Windows 提供了四个主要的应用程序平台，每个平台都具有不同的优势：

* [通用 Windows 平台 (UWP)](#uwp)
* [WPF (.NET)](#wpf)
* [Windows 窗体 (.NET)](#windows-forms)
* [Win32](#win32)

借助所有这些应用程序平台，你可以创建在经典 Windows 桌面上运行的 Word、Excel 和 Photoshop 等桌面应用，并充分利用该环境的特定功能。 但是，其中一些平台有一些共同的特征，更适用于某些类型的应用程序：

* **UWP、WPF 和 Windows 窗体**。 这些平台使托管运行时环境（适用于 UWP 的 Windows 运行时，以及适用于 Windows 窗体和 WPF 的 .NET）具有许多优势，尤其是在开发人员的工作效率、复杂且可自定义的 UI 和应用程序安全性方面。 由于这些框架支持可视化设计器和 UI 标记以快速创建 UI，因此它们特别适用于业务线应用程序。

* **Win32 API**。 Win32 API（也称为 Windows API）是需要直接访问 Windows 和硬件的本机 C/C++ Windows 应用程序的原始平台。 它提供一流的开发体验，无需依赖于 .NET 和 WinRT 等托管运行时环境。 这使得 Win32 API 成为需要最高级别性能和直接访问系统硬件的应用程序的首选平台。

本文更详细地介绍了这些平台，并帮助确定适用于应用程序的最佳平台。 

> [!NOTE]
> 无论选择哪个应用平台，都可以使用通用 Windows 平台 (UWP) 的许多功能在 Windows 10 上的应用中提供新式体验。 例如，即使桌面应用是使用 WPF、Windows 窗体或 Win32 API 生成的，仍可使用 UWP 首次引入的许多功能，如 MSIX 包部署和 UWP XAML 控件。 有关详细信息，请参阅[桌面应用的现代化](modernize/index.md)。

## <a name="uwp"></a>UWP

UWP 是 Windows 10 应用程序和游戏的前沿平台。 它是一个高度可自定义的平台，使用 XAML 标记将 UI（展示）与代码（业务逻辑）分隔。 UWP 适用于需要复杂 UI、自定义样式和图形密集型方案的桌面应用程序。 UWP 还针对默认 UX 体验内置了对 [Fluent Design 系统](/windows/uwp/design/fluent-design-system/)的支持，并提供了对 [Windows 运行时 (WinRT) API](/windows/uwp/get-started/universal-application-platform-guide#how-the-universal-windows-platform-relates-to-windows-runtime-apis) 的访问。 通过采用 Fluent，UWP 可自动支持常见输入方式，如墨迹、触控、游戏板、键盘和鼠标。

UWP 不仅可以用于为 Windows 电脑创建桌面应用程序，同时，它也是 Xbox、HoloLens 和 Surface Hub 应用程序的唯一支持平台。 UWP 是我们最新的前沿应用程序平台。

有关 UWP 的详细信息，请参阅以下文章：

* [开始行动](/windows/uwp/get-started/)
* [设计和 UI](/windows/uwp/design/)
* [技术和功能](/windows/uwp/develop/)
* [API 参考](/uwp/)
* [示例](https://github.com/Microsoft/Windows-universal-samples)

## <a name="wpf"></a>WPF

WPF 是为托管型 Windows 应用程序而建立的平台，通过它可访问 .NET Core 或整个 .NET Framework，它还使用 XAML 标记将 UI 与代码分隔。 此平台旨在用于需要复杂 UI、自定义样式和图形密集型方案的桌面应用程序。 WPF 开发技能类似于 UWP 开发技能，因此从 WPF 迁移到 UWP 应用比从 Windows 窗体迁移更容易。

有关 WPF 的详细信息，请参阅以下文章：

* [入门 (WPF)](https://docs.microsoft.com/dotnet/framework/wpf/getting-started/)。
* [创建首个应用 (.NET Core)](/visualstudio/get-started/csharp/tutorial-wpf/)
* [创建首个应用 (.NET Framework)](/dotnet/framework/wpf/getting-started/walkthrough-my-first-wpf-desktop-application/)
* [将 WPF 应用迁移到 .NET Core](/dotnet/desktop-wpf/migration/convert-project-from-net-framework/)
* [API 参考 (.NET)](https://docs.microsoft.com/dotnet/api/index)
* [示例](https://github.com/Microsoft/WPF-Samples)

## <a name="windows-forms"></a>Windows 窗体

Windows 窗体是用于托管型 Windows 应用程序的原始平台，具有一个轻型 UI 模型和对 .NET Core 或整个 .NET Framework 的访问权限。 它擅长帮助开发人员快速开始构建应用程序，即使对于刚接触该平台的开发人员也是如此。 这是一种基于窗体的快速应用程序开发平台，其中包含大量内置的可视化和非可视化拖放控件。 Windows 窗体不使用 XAML，因此，如果决定以后将应用程序扩展到 UWP，则需要完全重写 UI。

有关 Windows 窗体的详细信息，请参阅以下文章：

* [Windows 窗体入门](https://docs.microsoft.com/dotnet/framework/winforms/getting-started-with-windows-forms)
* [创建第一个 Windows 窗体应用](/dotnet/framework/winforms/creating-a-new-windows-form)
* [教程：创建图片查看器](/visualstudio/ide/tutorial-1-create-a-picture-viewer?view=vs-2019)
* [API 参考 (.NET)](https://docs.microsoft.com/dotnet/api/index)
* [增强 Windows 窗体应用](/dotnet/framework/winforms/advanced/)

## <a name="win32"></a>Win32

与在托管运行时环境上（如 WinRT 和 .NET）相比，将 Win32 API 与 C++ 结合使用可以实现最高级别的性能和效率，方法是通过使用非托管代码对目标平台进行更多控制。 但是，对应用程序的执行进行这种级别的控制需要更加谨慎和更集中的注意力才能正确执行，同时，需牺牲开发效率以提高运行时性能。

以下是 Win32 API 和 C++ 提供的一些主要功能，使你能够生成高性能应用程序。

* 硬件级优化，包括对资源分配、对象生存期、数据布局、对齐、字节封装等的严格控制。
* 通过内部函数访问面向性能的指令集，如 SSE 和 AVX。
* 使用模板进行高效且类型安全的泛型编程。
* 高效且安全的容器和算法。
* DirectX，尤其是 Direct3D 和 DirectCompute（请注意，UWP 还提供 DirectX 互操作）。

有关详细信息，请参阅以下文章：

* [开始行动](/windows/win32/desktop-programming/)
* [创建首个 Win32 和 C++ 应用](/windows/win32/learnwin32/learn-to-program-for-windows/)
* [技术和功能](/windows/win32/desktop-app-technologies)
* [API 参考](/windows/win32/apiindex/windows-api-list/)
* [示例](https://github.com/Microsoft/Windows-classic-samples)

## <a name="platform-comparison-uwp-wpf-and-windows-forms"></a>平台比较：UWP、WPF 和 Windows 窗体

下表详细比较了 Windows 窗体、WPF 和 UWP 的各种特征。

| 功能或方案  |    UWP     |      WPF     |   Windows 窗体  |
|--------|--------|--------|--------|
| **支持的版本**      |  Windows 10   |  Windows 7 及更高版本 |  Windows 7 及更高版本  |
| **语言**      |   C\#、C++/WinRT、C++/CX、VB、JavaScript   |  C\#、C++/CLI (Managed Extensions for C++)、F\#、VB |  C\#、C++/CLI (Managed Extensions for C++)、F\#、VB   |
| **UI 运行时** |    本机（C++/WinRT 和 C++/CX）和托管 (.NET Native)  |  托管（.NET Framework 和 .NET Core 3） |   托管（.NET Framework 和 .NET Core 3）   |
| **开放源代码** | [是（仅限 Windows UI 库）](https://github.com/Microsoft/microsoft-ui-xaml)  |  [是（仅限 .NET Core）](https://github.com/dotnet/wpf) | [是（仅限 .NET Core）](https://github.com/dotnet/winforms)  |
| **支持 XAML** |   是   |  是  |   否   |
| **优势**  |  <ul><li>适用于 UI 的 XAML 标记</li><li>丰富且可自定义的 UX</li><li>现有代码库符合 .NET 标准</li><li>高 DPI 支持</li><li>支持跨 Windows 设备的多种输入类型（包括触控、笔、游戏板、鼠标和键盘）</li><li>支持 Xbox、HoloLens、IoT 或 Surface Hub</li><li>可选的密集（精简）UI</li><li>支持本机 C++</li><li>优化的电池使用时间</li><li>新式辅助功能支持（如屏幕阅读器）</li><li>丰富的文本数据功能（如内置拼写检查）</li><li>墨迹书写支持</li><li>通过应用程序容器实现安全执行（例如，对不受信任的内容进行沙盒处理）</li></ul>  |  <ul><li>适用于 UI 的 XAML 标记</li><li>丰富且可自定义的 UX</li><li>Microsoft 及合作伙伴提供的大量控件</li><li>密集 UI</li><li>支持 Windows 7</li><li>平台支持输入验证</li></ul> | <ul><li>快速开发应用程序</li><li>用于生成 UI 的 WYSIWYG 编辑器</li><li>Microsoft 及合作伙伴提供的大量控件</li><li>密集 UI</li><li>支持 Windows 7</li><li>键盘和鼠标输入</li></ul>          |
| **具有有限支持的方案** |  <ul><li>多窗口支持<sup>1</sup></li><li>平台支持输入验证<sup>1</sup></li><li>不支持 Windows 7</li><li>某些 UWP API 要求使用特定最低版本的 Windows 10</li><li>完整的平台支持和 shell 集成（例如，UWP 当前不支持系统托盘集成或对所有设备的完全访问权限）</li><li>直接访问磁盘上的所有文件</li><li>ADO.NET</li><li>使用非 .NET 标准或非 Windows 应用认证工具包兼容 API 的现有代码库类库</li><li>本地网络环回支持（也就是说，如果应用需要与 localhost 通信，而无需在目标设备上创建环回豁免）</li><li>密集型文件 I/O</li></ul>     |  <ul><li>高 DPI 支持<sup>2</sup></li><li>触摸输入<sup>2</sup></li></ul>  |  <ul><li>高 DPI 支持<sup>2</sup></li><li>触摸输入<sup>2</sup></li><li>可自定义的 UI</li><li>丰富的图形和用户体验（例如触控和动画）</li><li>丰富的视图和数据模型抽象</li></ul>    |   |

<sup>1</sup> 我们已经公开发布了一些功能，这些功能将在未来版本的 Windows 10 中处理此方案。

<sup>2</sup> 尽管该平台缺少针对此方案的一流 API 支持，但开发人员可通过工作区来支持此方案。

## <a name="other-app-platforms"></a>其他应用平台

### <a name="progressive-web-apps-pwas"></a>渐进式 Web 应用 (PWA)

PWA 使开发人员能够将网站代码进行打包，以便其能像应用程序一样在 Windows 10 电脑上安装和运行。 有关详细信息，请参阅[渐进式 Web 应用](https://docs.microsoft.com/microsoft-edge/progressive-web-apps/get-started)。

### <a name="xamarin"></a>Xamarin

使用 Xamarin 为 Windows 10 生成可在 iOS 和 Android 上运行的跨平台应用程序。 有关详细信息，请参阅 [Xamarin](https://docs.microsoft.com/xamarin/xamarin-forms/get-started/index)。
