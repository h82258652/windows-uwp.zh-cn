---
description: 本文概述了适用于 Windows 应用的 Visual Studio 项目和项模板。
title: 适用于 Windows 应用的 Visual Studio 项目和项模板
ms.date: 07/02/2020
ms.topic: article
keywords: windows 10, uwp, windows 窗体, wpf, xaml 岛
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: high
ms.openlocfilehash: 30f190b43b9156a92cdaf2ad533ededfc79c3a74
ms.sourcegitcommit: c1226b6b9ec5ed008a75a3d92abb0e50471bb988
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/20/2020
ms.locfileid: "86493933"
---
# <a name="visual-studio-project-and-item-templates-for-windows-apps"></a>适用于 Windows 应用的 Visual Studio 项目和项模板

Visual Studio 2019 提供很多项目和项模板，可帮助你使用 C\# 或 C++ 生成适用于 Windows 10 设备的应用。 本主题介绍了这些模板，并可帮助你选择一种适用于你的场景的模板。

* 项目模板包括项目文件、代码文件和其他资产，可配置它们以生成应用或可由应用加载和使用的组件。
* 项模板是包含常用代码和 XAML 的项目文件，可以将其添加到项目中以减少开发时间。 例如，你可以使用项模板将新的窗口、页面或控件添加到应用。

## <a name="winui-templates"></a>WinUI 模板

[Windows UI 库 (WinUI)](../winui/index.md) 是新式的本机用户界面 (UI) 平台，适用于跨桌面（.NET 和本机 Win32）和 UWP 应用平台的 Windows 应用。 [WinUI 3](../winui/winui3/index.md)（目前作为开发者预览版提供）是 WinUI 的最新主要版本，它将 WinUI 转换为适用于桌面 Windows 应用的完整 UX 框架。

WinUI 3 包括用于 Visual Studio 2019 的 VSIX 包，此包提供项目和项模板，可帮助你使用基于 WinUI 的界面开始生成应用。 有关 WinUI 3 VSIX 包及其提供的项目模板的详细信息，请参阅[本节](../winui/winui3/index.md#install-winui-3-preview-2)。

> [!IMPORTANT]
> WinUI 3（包括相关的 Visual Studio 模板）当前以开发者预览版提供，用于早期评估并从开发人员社区收集反馈。 目前不应将其用于生产应用。

## <a name="uwp-templates"></a>UWP 模板

Visual Studio 提供了各种项目模板，用于生成使用 C# 或 C++ 的 UWP 应用。 要使用这些项目模板，在你安装 Visual Studio 时，“通用 Windows 平台开发”工作负载必须包括在内。 对于 C++ 项目模板，用于“通用 Windows 平台开发”工作负载的“C++ (v142) 通用 Windows 平台工具”可选组件也必须包括在内 。

### <a name="project-templates-for-c-and-uwp"></a>适用于 C# 和 UWP 的项目模板

要在 Visual Studio 中创建新项目时访问UWP C# 项目模板，请将语言筛选为“C#”，将平台筛选为“Windows”并将项目类型筛选为“UWP”  。

![UWP C# 项目模板](images/uwp-projects-csharp.png)

可以使用这些项目模板来创建 C# UWP 应用。

| 模板 | 说明 |
|----------|-------------|
| 空白应用(通用 Windows) | 创建 UWP 应用。 生成的项目包括一个基础页面，该页面派生自可用于开始生成 UI 的 [Windows.UI.Xaml.Controls.Page](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.page) 类。 |
| 单元测试应用(通用 Windows) | 采用 C# 为 UWP 应用创建一个单元测试项目。 有关详细信息，请参阅 [单元测试 C# 代码](https://docs.microsoft.com/visualstudio/test/unit-testing-visual-csharp-code-in-a-store-app)。 |

可以使用这些项目模板来生成 C# UWP 应用的各个部分。

| 模板 | 说明 |
|----------|-------------|
| 类库(通用 Windows) | 使用 C# 创建一个托管类库 (DLL)，使其可由以托管代码编写的其他 UWP 应用使用。 |
| Windows 运行时组件(通用 Windows) | 使用 C# 创建一个 [Windows 运行时组件](https://docs.microsoft.com/windows/uwp/winrt-components/)，使其可由任何 UWP 应用使用，而不管该应用是使用哪种编程语言编写的。 |
| 可选代码包(通用 Windows) | 使用可执行 C# 代码创建一个可由应用加载的可选包。 有关详细信息，请参阅[包含可执行代码的可选包](https://docs.microsoft.com/windows/msix/package/optional-packages-with-executable-code)。  |

### <a name="project-templates-for-c-and-uwp"></a>适用于 C++ 和 UWP 的项目模板

可以使用两种不同的技术来生成 C++ UWP 应用：

* 推荐的技术是 [C++/WinRT](https://docs.microsoft.com/windows/uwp/cpp-and-winrt-apis/)。 这是一种 C++ 语言投影，完全在头文件中实现，旨在提供对新式 WinRT API 的一流访问。
* 或者，你可以使用较旧版 [C++/CX](https://docs.microsoft.com/cpp/cppcx/visual-c-language-reference-c-cx) 扩展集。 C++/CX 仍受支持，但我们建议你改用 C++/WinRT。

要在 Visual Studio 中创建新项目时访问UWP C++ 项目模板，请将语言筛选为“C++”，将平台筛选为“Windows”并将项目类型筛选为“UWP”  。 

> [!NOTE]
> 默认情况下，Visual Studio 中的“通用 Windows 平台开发”工作负载仅提供对 C++/CX 项目模板的访问。 要访问 C++/WinRT 项目模板，必须安装 [C++/WinRT VSIX](https://docs.microsoft.com/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package) 包。

![UWP C++ 项目模板](images/uwp-projects-cpp.png)

可以使用这些项目模板来创建 C++ UWP 应用。

| 模板 | 说明 |
|----------|-------------|
| 空白应用 (C++/WinRT) | 使用 XAML 用户界面创建 C++/WinRT UWP 应用。 生成的项目包括一个基础页面，该页面派生自可用于开始生成 UI 的 [Windows.UI.Xaml.Controls.Page](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.page) 类。 |
| 核心应用 (C++/WinRT) | 创建一个使用 [CoreApplication](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Core.CoreApplication) 与各种 UI 框架（而不是 XAML 用户界面）集成的 C++/WinRT UWP 应用。 有关演示如何使用该项目模板来创建使用 DirectX 的简单游戏的演练，请参阅[创建使用 DirectX 的简单 UWP 游戏](https://docs.microsoft.com/windows/uwp/gaming/tutorial--create-your-first-uwp-directx-game)。 |
| 空白应用(通用 Windows - C++/CX) | 使用 XAML 用户界面创建 C++/WinRT UWP 应用。 生成的项目包括一个基础页面，该页面派生自 WinUI 库中可用于开始生成 UI 的 [Windows.UI.Xaml.Controls.Page](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.page) 类。 |
| DirectX 11 和 XAML 应用(通用 Windows - C++/CX) | 创建一个使用 DirectX 11 的 UWP 应用和一个 SwapChainPanel，以便可以使用 XAML UI 控件。 有关详细信息，请参阅 [DirectX 游戏项目模板](https://docs.microsoft.com/windows/uwp/gaming/user-interface)。 |
| DirectX 11 应用(通用 Windows - C++/CX) | 创建使用 DirectX 11 的 UWP 应用。 有关详细信息，请参阅 [DirectX 游戏项目模板](https://docs.microsoft.com/windows/uwp/gaming/user-interface)。 |
| DirectX 12 应用(通用 Windows - C++/CX) | 创建使用 DirectX 12 的 UWP 应用。 有关详细信息，请参阅 [DirectX 游戏项目模板](https://docs.microsoft.com/windows/uwp/gaming/user-interface)。 |
| 单元测试应用(通用 Windows - C++/CX) | 采用 C++/CX 为 UWP 应用创建一个单元测试项目。 有关详细信息，请参阅[如何测试 C++ UWP DLL](https://docs.microsoft.com/visualstudio/test/unit-testing-a-visual-cpp-dll-for-store-apps)。 |

可以使用这些项目模板来生成 C++ UWP 应用的各个部分。

| 模板 | 说明 |
|----------|-------------|
| Windows 运行时组件 (C++/WinRT) | 使用 C++/WinRT 创建一个 [Windows 运行时组件](https://docs.microsoft.com/windows/uwp/winrt-components/)，使其可由任何 UWP 应用使用，而不管该应用是使用哪种编程语言编写的。 |
| Windows 运行时组件(通用 Windows) | 使用 C++/CX 创建一个 [Windows 运行时组件](https://docs.microsoft.com/windows/uwp/winrt-components/)，使其可由任何 UWP 应用使用，而不管该应用是使用哪种编程语言编写的。 |
| DLL (通用 Windows) | 一个使用 C++/CX 创建动态链接库 (DLL) 的项目，可以在 UWP 应用中使用。 有关详细信息，请参阅 [DLL (C++/CX)](https://docs.microsoft.com/cpp/cppcx/dlls-c-cx)。 |
| 静态库(通用 Windows) | 一个使用 C++/CX 创建静态库 (LIB) 的项目，可以在 UWP 应用中使用。 有关详细信息，请参见[静态库 (C++/CX)](https://docs.microsoft.com/cpp/cppcx/static-libraries-c-cx)。 |

## <a name="cwin32-templates"></a>C++/Win32 模板

Visual Studio 提供了各种项目模板，用于生成使用本机 C++ 的桌面 Windows 应用以及直接访问 Win32 API。 要使用这些项目模板，在你安装 Visual Studio 时，“使用 C++ 进行桌面开发”工作负载必须包括在内。 此工作负载包括用于生成桌面应用、控制台应用和库的项目模板。

### <a name="project-templates-for-desktop-apps"></a>适用于桌面应用的项目模板

在 Visual Studio 中创建新项目时，要访问经典桌面应用的 C++ 项目模板，请将语言筛选为“C++”，将平台筛选为“Windows”并将项目类型筛选为“桌面”  。

![本机 C++ 应用项目模板](images/desktop-app-projects-cpp.png)

| 模板 | 说明 |
|----------|----------|
| Windows 桌面应用程序 | 使用 C++ 创建经典 Windows 桌面应用。 有关详细信息，请参阅[演练：创建传统的 Windows 桌面应用程序](https://docs.microsoft.com/cpp/windows/walkthrough-creating-windows-desktop-applications-cpp)。 |
| Windows 桌面向导 | 提供可用于创建以下一种类型的项目的分步向导：经典 Windows 桌面应用、控制台应用、动态链接库 (DLL) 或静态库。 有关详细信息，请参阅 [Windows 桌面向导](https://docs.microsoft.com/cpp/windows/windows-desktop-wizard)和[演练：创建传统的 Windows 桌面应用程序](https://docs.microsoft.com/cpp/windows/walkthrough-creating-windows-desktop-applications-cpp)。         |
| Windows 应用程序打包项目 | 创建可用于将桌面应用生成到 [MSIX 包](https://docs.microsoft.com/windows/msix/overview)的项目。 这提供了一种新式部署体验、通过包扩展与 Windows 10 功能集成的功能以及更多其他功能。 有关详细信息，请参阅 [Windows 应用程序打包项目](https://docs.microsoft.com/windows/msix/desktop/desktop-to-uwp-packaging-dot-net)。  |

### <a name="project-templates-for-console-apps"></a>适用于控制台应用的项目模板

要访问适用于控制台应用的 C++ 项目模板，请将语言筛选为“C++”，将平台筛选为“Windows”并将项目类型筛选为“控制台”。  。

![本机 C++ 控制台项目模板](images/desktop-console-projects-cpp.png)

| 模板 | 说明 |
|----------|----------|
| Windows 控制台应用程序 (C++/WinRT) | 在不使用用户界面的情况下创建一个 [C++/WinRT](https://docs.microsoft.com/windows/uwp/cpp-and-winrt-apis/) 控制台应用。 有关详细信息，请参阅 [C++/WinRT 快速入门](https://docs.microsoft.com/windows/uwp/cpp-and-winrt-apis/get-started#a-cwinrt-quick-start)。 此项目模板需要 [C++ /WinRT VSIX](https://docs.microsoft.com/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package)。  |
| 控制台应用 | 在不使用用户界面的情况下创建一个控制台应用。 有关详细信息，请参阅[演练：创建标准的 C++ 程序](https://docs.microsoft.com/cpp/windows/walkthrough-creating-a-standard-cpp-program-cpp)。 |
| 空项目 | 用于创建应用程序、库或 DLL 的空项目。 必须添加所需的任何代码或资源。 |

### <a name="project-templates-for-libraries"></a>适用于库的项目模板

要访问适用于库的 C++ 项目模板，请将语言筛选为“C++”，将平台筛选为“Windows”并将项目类型筛选为“库”。  。

![本机 C++ 库项目模板](images/desktop-library-projects-cpp.png)

| 模板 | 说明 |
|----------|----------|
| 动态链接库 (DLL) | 创建动态链接库 (DLL) 的项目。 有关详细信息，请参阅[演练：创建和使用动态链接库](https://docs.microsoft.com/cpp/build/walkthrough-creating-and-using-a-dynamic-link-library-cpp)。 |
| 静态库 | 创建静态库 (LIB) 的项目。 有关详细信息，请参阅[演练：创建和使用静态库](https://docs.microsoft.com/cpp/build/walkthrough-creating-and-using-a-static-library-cpp)。 |

### <a name="item-templates-for-native-c-and-win32"></a>适用于本机 C++ 和 Win32 的项模板

C++ 项目模板包括许多项模板，可使用它们来执行任务，例如向项目中添加新文件和资源。 有关完整列表，请参阅[使用 Visual C++ 添加新项模板](https://docs.microsoft.com/cpp/build/reference/using-visual-cpp-add-new-item-templates)。

## <a name="net-templates"></a>.NET 模板

Visual Studio 提供了各种项目模板，用于生成使用 .NET 和 C# 的桌面 Windows 应用。 要使用这些项目模板，在你安装 Visual Studio 时，“.NET 桌面开发”工作负载必须包括在内。

要在 Visual Studio 中创建新项目时访问 .NET C# 项目模板，请将语言筛选为“C#”，将平台筛选为“Windows”并将项目类型筛选为“桌面”  。

![.NET C# 项目模板](images/dotnet-projects-csharp.png)

可以使用这些项目模板创建使用 C# 和 .NET 的应用。

| 模板 | 说明 |
|----------|----------|
| WPF 应用 (.NET Core) | 创建一个以 [.NET Core](https://docs.microsoft.com/dotnet/core/) 为目标的 [WPF](https://docs.microsoft.com/dotnet/framework/wpf/) 应用。 有关此项目模板的演练，请参阅[创建 WPF 应用程序](https://docs.microsoft.com/visualstudio/get-started/csharp/tutorial-wpf)。 |
| WPF 应用(.NET Framework) | 创建一个以 [.NET Framework](https://docs.microsoft.com/dotnet/framework/) 为目标的 [WPF](https://docs.microsoft.com/dotnet/framework/wpf/) 应用。 有关此项目模板的演练，请参阅[教程：创建你的第一个 WPF 应用程序](https://docs.microsoft.com/dotnet/framework/wpf/getting-started/walkthrough-my-first-wpf-desktop-application)。 |
| Windows 窗体应用 (.NET Core) | 创建一个以 [.NET Core](https://docs.microsoft.com/dotnet/core/) 为目标的 [Windows 窗体](https://docs.microsoft.com/dotnet/framework/winforms/)应用。  |
| Windows 窗体应用 (.NET Framework) | 创建一个以 [.NET Framework](https://docs.microsoft.com/dotnet/framework/) 为目标的 [Windows 窗体](https://docs.microsoft.com/dotnet/framework/winforms/)应用。 有关此项目模板的演练，请参阅[使用 C# 在 Visual Studio 中创建 Windows 窗体应用](https://docs.microsoft.com/visualstudio/ide/create-csharp-winform-visual-studio)。 |
| Windows 应用程序打包项目 | 创建可用于将 WPF 或 Windows 窗体应用生成到 [MSIX 包](https://docs.microsoft.com/windows/msix/overview)的项目。 这提供了一种新式部署体验、通过包扩展与 Windows 10 功能集成的功能以及更多其他功能。 有关详细信息，请参阅 [Windows 应用程序打包项目](https://docs.microsoft.com/windows/msix/desktop/desktop-to-uwp-packaging-dot-net)。 |
