---
title: 在 Windows 10 上设置开发环境
description: 帮助你在 Windows 上设置开发环境并安装首选工具和代码语言的指南。 无论你是否选择使用 Python、NodeJS、VS Code、Git、Bash、Linux 工具和命令、Android Studio，我们都会为你提供功能强大的新工具（例如 Windows 终端和 WSL）。
author: mattwojo
ms.author: mattwoj
manager: jken
ms.topic: article
ms.technology: windows-nodejs
keywords: 设置 windows, 开发环境, 开发工具, 开发路径, Microsoft, Windows, 开发人员, 使用技巧, 性能, WSL, 终端, nodejs, python
ms.localizationpriority: medium
ms.date: 07/24/2020
ms.openlocfilehash: e62ca938a23910290a8c63682fc2fde77ec0ea92
ms.sourcegitcommit: 1e06168ada5ce6013b1d07c428548f084464a286
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/28/2020
ms.locfileid: "87363719"
---
# <a name="set-up-your-development-environment-on-windows-10"></a>在 Windows 10 上设置开发环境

本指南将帮助你开始安装和设置在 Windows 或适用于 Linux 的 Windows 子系统上进行开发所需的语言和工具。

## <a name="development-paths"></a>开发路径

:::row:::
    :::column:::
       [![JavaScript/NodeJS](../images/nodejs-logo.png)](https://docs.microsoft.com/windows/nodejs)<br>
        **[NodeJS 入门](https://docs.microsoft.com/windows/nodejs)**<br>
        在 Windows 或适用于 Linux 的 Windows 子系统上安装 NodeJS 并设置开发环境。
    :::column-end:::
    :::column:::
       [![Python](../images/python-logo.png)](https://docs.microsoft.com/windows/python)<br>
        **[Python 入门](https://docs.microsoft.com/windows/python)**<br>
        在 Windows 或适用于 Linux 的 Windows 子系统上安装 Python 并设置开发环境。
    :::column-end:::
    :::column:::
       [![Android](../images/android-logo.png)](https://docs.microsoft.com/windows/android)<br>
        **[Android 入门](https://docs.microsoft.com/windows/android)**<br>
        安装 Android Studio，或选择 Xamarin、React 或 Cordova 等跨平台解决方案，然后在 Windows 上设置开发环境。
    :::column-end:::
    :::column:::
       [![Windows 桌面版](../images/windows-logo.png)](https://docs.microsoft.com/windows/apps/)<br>
        **[Windows 桌面入门](https://docs.microsoft.com/windows/apps/)**<br>
        开始使用 UWP、Win32、WPF、Windows Forms 生成适用于 Windows 10 的桌面应用，或者使用 MSIX 和 XAML Islands 更新和部署现有桌面应用。
    :::column-end:::
:::row-end:::

:::row:::
    :::column:::
       [![C/C++](../images/c-logo.png)](https://docs.microsoft.com/cpp/)<br>
        **[C++ 和 C 入门](https://docs.microsoft.com/cpp/)**<br>
        开始使用 C++、C 和程序集开发应用、服务和工具。
    :::column-end:::
    :::column:::
       [![C#](../images/csharp-logo.png)](https://docs.microsoft.com/dotnet/csharp/)<br>
        **[C# 入门](https://docs.microsoft.com/dotnet/csharp/)**<br>
        开始使用 C# 和 .NET Core 生成应用。
    :::column-end:::
    :::column:::
       [![适用于 Java 的 Azure](../images/java-logo.png)](https://docs.microsoft.com/azure/developer/java/)<br>
        **[Azure 上的 Java 入门](https://docs.microsoft.com/azure/developer/java/)**<br>
        开始使用这些面向 Java 开发人员的教程和工具生成面向云端的应用。
    :::column-end:::
    :::column:::
       [![PowerShell](../images/powershell.png)](https://docs.microsoft.com/powershell/)<br>
        **[PowerShell 入门](https://docs.microsoft.com/powershell/)**<br>
        开始使用 PowerShell（一种命令行 shell 和脚本语言）自动完成跨平台任务和管理配置。
    :::column-end:::
:::row-end:::

## <a name="tools-and-platforms"></a>工具和平台

:::row:::
    :::column:::
       [![WSL](../images/windows-linux-dev-env.png)](https://docs.microsoft.com/windows/wsl/)<br>
        **[适用于 Linux 的 Windows 子系统](https://docs.microsoft.com/windows/wsl/)**<br>
        使用与 Windows 完全集成的偏好 Linux 分发版（不再需要双引导）。<br>
        [安装 WSL](https://docs.microsoft.com/windows/wsl/install-win10)
    :::column-end:::
    :::column:::
       [![Windows 终端](../images/terminal.png)](https://docs.microsoft.com/windows/terminal/)<br>
        **[Windows 终端](https://docs.microsoft.com/windows/terminal/)**<br>
        自定义终端环境，以使用多个命令行 shell。
        <br>
        [安装终端](https://www.microsoft.com/p/windows-terminal/9n0dx20hk701?rtc=1&activetab=pivot:overviewtab)
    :::column-end:::
    :::column:::
       [![Windows 程序包管理器](../images/winget.png)](https://docs.microsoft.com/windows/package-manager/)<br>
        **[Windows 程序包管理器](https://docs.microsoft.com/windows/package-manager/)**<br>
        将综合程序包管理器 WinGet 与命令行配合使用，在 Windows 10 上安装应用程序。<br>
        [安装 WinGet（公共预览版）](https://docs.microsoft.com/windows/package-manager/winget/#install-winget)
    :::column-end:::
    :::column:::
       [![PowerToys](../images/powertoys.png)](https://github.com/microsoft/PowerToys)<br>
        **[Windows PowerToys](https://github.com/microsoft/PowerToys)**<br>
        通过这组高级用户实用程序，调整并简化 Windows 体验，以提高工作效率。<br>
        [安装 PowerToys（公共预览版）](https://github.com/microsoft/PowerToys#installing-and-running-microsoft-powertoys)
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
       [![VS Code](../images/Vscode.png)](https://code.visualstudio.com/docs)<br>
        **[VS Code](https://code.visualstudio.com/docs)**<br>
        轻量级源代码编辑器，具有对 JavaScript、TypeScript、Node.js 的内置支持；并且还是丰富的扩展（C++、C#、Java、Python、PHP、Go）和运行时（例如 .NET 和 Unity）生态系统。<br>
        [安装 VS Code](https://code.visualstudio.com/download)
    :::column-end:::
    :::column:::
       [![Visual Studio](../images/visualstudio.png)](https://docs.microsoft.com/visualstudio/windows/)<br>
        **[Visual Studio](https://docs.microsoft.com/visualstudio/windows/)**<br>
        集成开发环境，可用于编辑、调试、生成代码和发布应用，包括编译器、IntelliSense 代码完成以及更多功能。<br>
        [安装 Visual Studio](https://docs.microsoft.com/visualstudio/install/install-visual-studio)
    :::column-end:::
    :::column:::
       [![Azure](../images/Azure.png)](https://docs.microsoft.com/azure/guides/developer/azure-developer-guide)<br>
        **[Azure](https://docs.microsoft.com/azure/guides/developer/azure-developer-guide)**<br>
        用于托管现有应用并简化新开发的完整云平台。 Azure 服务集成了开发、测试、部署和管理应用所需的一切。<br>
        [设置 Azure 帐户](https://azure.microsoft.com/free/)
    :::column-end:::
    :::column:::
       [![.NET](../images/net.png)](https://dotnet.microsoft.com/)<br>
        **[.NET](https://docs.microsoft.com/dotnet/standard/get-started/)**<br>
        带有工具和库的开源开发平台，可用于生成任何类型的应用，包括 Web、移动、桌面、游戏、IoT、云和微服务。<br>
        [安装 .NET](https://dotnet.microsoft.com/download)
    :::column-end:::
:::row-end:::

<br>

## <a name="run-windows-and-linux"></a>运行 Windows 和 Linux

适用于 Linux 的 Windows 子系统 (WSL) 允许开发人员同时运行 Linux 操作系统和 Windows。 两者共享同一硬盘驱动器（可以访问对方的文件），剪贴板自然地支持二者之间的复制和粘贴，不需要双启动。 WSL 使你能够使用 BASH，并提供 Mac 用户最熟悉的环境。
- 在 [WSL 文档](https://docs.microsoft.com/windows/wsl)中或通过[第 9 频道的 WSL 视频](https://channel9.msdn.com/Search?term=wsl&lang-en=true)了解详细信息。

> [!VIDEO https://channel9.msdn.com/Blogs/One-Dev-Minute/What-can-I-do-with-WSL--One-Dev-Question/player?format=ny]

你还可以使用 Windows 终端，通过多个选项卡或多个窗格在同一窗口中打开所有喜欢的命令行工具（PowerShell、Windows 命令提示符、Ubuntu、Debian、Azure CLI、Oh-my-Zsh、Git Bash 或以上所有工具）。

- 在 [Windows 终端文档](https://docs.microsoft.com/windows/terminal)中或通过[第 9 频道的 WT 视频](https://channel9.msdn.com/Search?term=windows%20terminal&lang-en=true)了解详细信息。

> [!VIDEO https://channel9.msdn.com/Blogs/One-Dev-Minute/What-are-the-main-features-of-the-new-Terminal--One-Dev-Question/player?format=ny]

## <a name="transitioning-between-mac-and-windows"></a>在 Mac 和 Windows 之间转换

查看我们的[在 Mac 和 Windows（或适用于 Linux 的 Windows 子系统）之间进行转换的指南](https://docs.microsoft.com/windows/dev-environment/mac-to-windows)。 它可以帮助确定以下项之间的区别：

* [键盘快捷方式](https://docs.microsoft.com/windows/dev-environment/mac-to-windows#keyboard-shortcuts)
* [触控板快捷方式](https://docs.microsoft.com/windows/dev-environment/mac-to-windows#trackpad-shortcuts)
* [终端和 shell 工具](https://docs.microsoft.com/windows/dev-environment/mac-to-windows#terminal-and-shell)
* [应用和实用程序](https://docs.microsoft.com/windows/dev-environment/mac-to-windows#apps-and-utilities)

![Office 图像](../images/flashy-office3.png)

## <a name="additional-resources"></a>其他资源

* [改进工作流的技巧](./tips.md)
* [从 Mac 切换到 Windows 的开发人员的成功案例](./dev-stories.md)
* [热门教程、课程和代码示例](./tutorials.md)
* [Microsoft Game Stack 文档](https://docs.microsoft.com/gaming/)
