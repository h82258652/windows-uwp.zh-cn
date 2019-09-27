---
Description: 若要创建新的桌面应用程序，你所做的第一次决定是使用 Win32 和 COM API 还是 .NET。
ms.assetid: 82705644-F1F0-40F3-99B1-7A97BFB32831
title: 选择应用平台
ms.topic: article
ms.date: 03/18/2019
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: medium
ms.openlocfilehash: ff32e20c42f613bd9ac3dba9eada2cced0baa64c
ms.sourcegitcommit: f34deba1d4460d85ed08fe9648999fe03ff6a3dd
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/26/2019
ms.locfileid: "71316994"
---
# <a name="choose-your-app-platform"></a>选择应用平台

如果要为 Windows Pc 创建新的桌面应用程序，你所做的第一次决定是使用哪个应用程序平台。 Windows 提供四个主要应用程序平台，每个平台都有不同的优势：

* [通用 Windows 平台 (UWP)](#uwp)
* [WPF （.NET）](#wpf)
* [Windows 窗体（.NET）](#windows-forms)
* [Win32](#win32)

所有这些应用程序平台都允许你创建桌面应用（如 Word、Excel 和 Photoshop），这些应用在经典 Windows 桌面上运行并充分利用该环境的特定功能。 但是，其中一些平台共享某些特征，更适用于某些类型的应用程序：

* **UWP、WPF 和 Windows 窗体**。 这些平台提供了托管运行时环境（UWP 的 Windows 运行时，以及适用于 Windows 窗体和 WPF 的 .NET），其中包含许多优点，尤其是在开发人员的工作效率、复杂的可自定义的 UI 和应用程序安全性方面。 由于这些框架支持可视化设计器和 UI 标记以便快速创建 UI，因此它们特别适用于业务线应用程序。

* **Win32 API**。 Win32 API （也称为 Windows API）是需要直接访问 Windows 和硬件的本机 CC++ /Windows 应用程序的原始平台。 它提供一流的开发体验，而不取决于 .NET 和 WinRT 等托管运行时环境。 这使得 Win32 API 适用于需要最高级别的性能和直接访问系统硬件的应用程序的平台。

本文更详细地介绍了这些平台，并帮助你确定适用于你的应用程序的最佳平台。 

> [!NOTE]
> 无论选择哪个应用平台，都可以使用通用 Windows 平台（UWP）的许多功能在 Windows 10 上的应用程序中提供新式体验。 例如，即使桌面应用是使用 WPF、Windows 窗体或 Win32 API 生成的，仍可使用 UWP 首次引入的许多功能，如 .MSIX 包部署和 UWP XAML 控件。 有关详细信息，请参阅[使桌面应用实现现代化](modernize/index.md)。

## <a name="uwp"></a>UWP

UWP 是适用于 Windows 10 应用程序和游戏的最前沿平台。 它是一个高度可自定义的平台，它使用 XAML 标记从代码（业务逻辑）分隔 UX （表示）。 UWP 适用于需要复杂的 UI、样式自定义和图形密集型方案的桌面应用程序。 UWP 还为默认 UX 体验提供对[熟知设计系统](/windows/uwp/design/fluent-design-system/)的内置支持，并提供对[Windows 运行时（WinRT） api](/windows/uwp/get-started/universal-application-platform-guide#how-the-universal-windows-platform-relates-to-windows-runtime-apis)的访问。 通过采用熟知，UWP 自动支持常见输入法，如墨迹、触摸、游戏板、键盘和鼠标。

您不仅可以使用 UWP 为 Windows Pc 创建桌面应用程序，还可以使用 UWP 作为 Xbox、HoloLens 和 Surface Hub 应用程序所支持的平台。 UWP 是我们最新的领先应用程序平台。

有关 UWP 的详细信息，请参阅[Windows 10 应用入门](/windows/uwp/get-started/)。

## <a name="wpf"></a>WPF

WPF 是为托管 Windows 应用程序提供的、具有对完整 .NET Framework 的访问权限的已建立平台，它还使用 XAML 标记将 UX 与代码区分开来。 此平台适用于需要复杂的 UI、样式自定义和图形密集型方案的桌面应用程序。 WPF 开发技能类似于 UWP 开发技能，因此从 WPF 迁移到 UWP 应用比从 Windows 窗体迁移更容易。

有关 WPF 的详细信息，请参阅[入门（WPF）](https://docs.microsoft.com/dotnet/framework/wpf/getting-started/)。

## <a name="windows-forms"></a>Windows 窗体

Windows 窗体是使用轻型 UI 模型的托管 Windows 应用程序的原始平台，并可访问完整 .NET Framework。 它 transact-sql 使开发人员能够快速开始构建应用程序，即使是面向平台的新开发人员。 这是一种基于窗体的快速应用程序开发平台，其中包含大量内置的可视化和非可视化拖放控件。 Windows 窗体不使用 XAML，因此，稍后决定将应用程序扩展到 UWP 需要完全重新编写 UI。

有关 Windows 窗体的详细信息，请参阅[Windows 窗体](https://docs.microsoft.com/dotnet/framework/winforms/getting-started-with-windows-forms)入门。

## <a name="platform-comparison-uwp-wpf-and-windows-forms"></a>平台比较：UWP、WPF 和 Windows 窗体

下表详细说明了 Windows 窗体、WPF 和 UWP 的各种特征。

| 功能或方案  |    UWP     |      WPF     |   Windows 窗体  |
|--------|--------|--------|--------|
| **支持的版本**      |  Windows 10   |  Windows 7 及更高版本 |  Windows 7 及更高版本  |
| **语言**      |   C\#， C++/WinRT， C++/cx，VB，JavaScript   |  C\#、 C++/cli （托管扩展C++）、F\#、VB |  C\#、 C++/cli （托管扩展C++）、F\#、VB   |
| **UI 运行时** |    本机（C++/WinRT 和C++/cx）和托管（.NET Native）  |  托管（.NET Framework 和 .NET Core 3） |   托管（.NET Framework 和 .NET Core 3）   |
| **开放源** | [是（仅 Windows UI 库）](https://github.com/Microsoft/microsoft-ui-xaml)  |  [是（仅限 .NET Core）](https://github.com/dotnet/wpf) | [是（仅限 .NET Core）](https://github.com/dotnet/winforms)  |
| **支持 XAML** |   是   |  是  |   否   |
| **长度**  |  <ul><li>UI 的 XAML 标记</li><li>丰富且可自定义的 UX</li><li>现有的基本代码 .NET Standard 符合</li><li>高 DPI 支持</li><li>跨 Windows 设备（包括触摸、笔、游戏板、鼠标和键盘）支持多个输入类型</li><li>支持 Xbox、HoloLens、IoT 或 Surface Hub</li><li>支持本机C++</li><li>优化电池寿命</li><li>新式辅助功能支持（如屏幕阅读器）</li><li>丰富的文本数据功能（例如内置拼写检查）</li><li>墨迹支持</li><li>通过应用程序容器实现安全执行（例如，对不受信任的内容进行沙盒处理）</li></ul>  |  <ul><li>UI 的 XAML 标记</li><li>丰富且可自定义的 UX</li><li>Microsoft 和合作伙伴提供的大量控件集合</li><li>密集 UI</li><li>支持 Windows 7</li><li>输入验证的平台支持</li></ul> | <ul><li>快速应用程序开发</li><li>用于生成 UI 的 WYSIWYG 编辑器</li><li>Microsoft 和合作伙伴提供的大量控件集合</li><li>密集 UI</li><li>支持 Windows 7</li><li>键盘和鼠标输入</li></ul>          |
| **支持有限的方案** |  <ul><li>密集 UI （创建密集 UI 需要自定义样式）<sup>1</sup></li><li>多窗口支持<sup>1</sup></li><li>输入验证的平台支持<sup>1</sup></li><li>不支持 Windows 7</li><li>某些 UWP Api 需要 Windows 10 的特定最低版本</li><li>完全平台支持和 shell 集成（例如，UWP 当前不支持对所有设备进行系统托盘集成或完全访问）</li><li>直接访问磁盘上的所有文件</li><li>ADO.NET</li><li>使用 non-.NET 标准或非 Windows 应用程序认证包兼容 Api 的现有代码库类库</li><li>本地网络环回支持（也就是说，如果你的应用程序需要与 localhost 通信，而不在目标设备上创建环回豁免）</li><li>密集型文件 i/o</li></ul>     |  <ul><li>高 DPI 支持<sup>2</sup></li><li>触摸输入<sup>2</sup></li></ul>  |  <ul><li>高 DPI 支持<sup>2</sup></li><li>触摸输入<sup>2</sup></li><li>自定义 UI</li><li>丰富的图形和用户体验（例如触摸和动画）</li><li>丰富的视图和数据模型抽象</li></ul>    |   |

<sup>1</sup>我们已经公开发布了一些功能，这些功能将在未来版本的 Windows 10 中处理此方案。

<sup>2</sup>虽然该平台缺少针对此方案的第一类 API 支持，但开发人员可通过解决方法来支持此方案。

## <a name="win32"></a>Win32

通过使用 Win32 API， C++可以通过对具有非托管代码的目标平台进行更多的控制，从而实现更高级别的性能和效率，而不能像 WinRT 和 .net 这样的托管运行时环境一样实现其效率。 但是，对应用程序的执行执行这种级别的控制需要更多的小心和注意事项，并为运行时性能提高开发效率。

下面是一些可用于构建高性能应用程序C++的 Win32 API 和提供内容的要点。

-   硬件级优化，包括对资源分配、对象生存期、数据布局、对齐、字节封装等的严格控制。
-   通过内部函数访问面向性能的指令集，如 SSE 和 AVX。
-   使用模板的有效的类型安全泛型编程。
-   高效和安全的容器和算法。
-   DirectX，特别是 Direct3D 和 DirectCompute （请注意，UWP 还提供 DirectX 互操作性）。
-   C++打造.

有关详细信息，请参阅使用 Win32 API 和[桌面应用技术](/windows/desktop/desktop-app-technologies)[的桌面 Windows 应用入门](/windows/desktop/desktop-programming)。

### <a name="win32-and-c-for-traditional-desktop-applications"></a>Win32 和C++传统桌面应用程序

当在中C++编写桌面应用程序时，您可以为 UI 或同时支持非 Windows 平台的第三方应用程序框架宿主选择 WIN32 或 MFC。

-   **Win32**这是 Windows 平台的基于处理程序的 C 语言 API，包括但不限于 UI 功能，如窗口化、绘图和 UI 控件。 由于它是基于句柄的低级别 C 语言 API，因此，它是创建新式 UI 密集型应用程序的一种不常见选择。 不过，它提供了与 Windows 平台进行交互所需的基本 Api，适用于具有简单 UI 要求的应用程序，或者只是想让 Windows UI 尽可能地保持不变（例如游戏）的应用程序。
-   **MFC （Microsoft 基础类库）：** 这是自1992以来为 Windows 开发人员提供服务的古老应用程序框架和 UI 库。 它是基于处理C++程序的 C 语言 Win32 API 的精简包装器，为许多预定义的窗口、公共控件和其他 windows 对象提供面向对象的接口。 尽管 .NET 生态系统中的许多新式 UI 框架都超过 MFC，但对于创建 Windows 桌面应用程序的许多C++开发人员来说，它仍是选择的本机 ui 框架。
-   **第三方应用程序框架：** 由于C++可以在各种平台上运行，而不会绑定到 Windows 或 .net 运行时，因此第三方开发了新的C++应用程序和 UI 框架，以方便使用丰富的用户界面开发跨平台应用程序。 其中一些框架提供自己的外观 & 感觉，而其他框架（如 wxWidgets 或 Qt）使用或模拟平台的本机控制集。 使用这些库，可以在 Windows 或其他平台上运行的应用程序版本（如 OSX 或 Linux）之间共享几乎所有应用程序的源代码。

## <a name="other-app-platforms"></a>其他应用平台

### <a name="progressive-web-apps-pwas"></a>渐进式 Web 应用（Pwa）

Pwa 让开发人员打包其网站代码，使其能够在 Windows 10 电脑上像应用程序一样安装并运行。 有关详细信息，请参阅[渐进式 Web Apps](https://docs.microsoft.com/microsoft-edge/progressive-web-apps/get-started)。

### <a name="xamarin"></a>Xamarin

使用 Xamarin 生成适用于 Windows 10 的跨平台应用程序，这些应用程序也可以在 iOS 和 Android 上运行。 有关详细信息，请参阅[Xamarin](https://docs.microsoft.com/xamarin/xamarin-forms/get-started/index)。
