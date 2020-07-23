---
title: 设置 Windows 10 开发环境
description: 帮助设置和优化 Windows 开发环境的指南。 我们将帮助你开始安装使用 Windows 或适用于 Linux 的 Windows 子系统进行开发所需的语言和工具。
author: mattwojo
ms.author: mattwoj
manager: jken
ms.topic: article
ms.technology: windows-nodejs
keywords: ''
ms.localizationpriority: medium
ms.date: 07/01/2020
ROBOTS: NOINDEX
ms.openlocfilehash: 237c3e8f58e41007840cf72aa1fd65efdc5763a5
ms.sourcegitcommit: c1226b6b9ec5ed008a75a3d92abb0e50471bb988
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/20/2020
ms.locfileid: "86493702"
---
# <a name="set-up-your-windows-10-development-environment"></a>设置 Windows 10 开发环境

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
        **[Windows 入门](https://docs.microsoft.com/windows/apps/)**<br>
        开始使用 UWP、Win32、WPF、Windows Forms 生成适用于 Windows 10 的桌面应用，或者使用 MSIX 和 XAML Islands 更新和部署现有桌面应用。
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
        [安装 WinGet](https://docs.microsoft.com/windows/package-manager/winget/#install-winget)
    :::column-end:::
    :::column:::
       [![PowerToys](../images/powertoys.png)](https://github.com/microsoft/PowerToys)<br>
        **[Windows PowerToys](https://github.com/microsoft/PowerToys)**<br>
        通过这组高级用户实用程序，调整并简化 Windows 体验，以提高工作效率。<br>
        [安装 PowerToys](https://github.com/microsoft/PowerToys#installing-and-running-microsoft-powertoys)
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

---

<br>

![填充图像](../images/flashy-office.png)

## <a name="tips-for-improving-your-workflow"></a>工作流改进技巧

我们收集了一些技巧，希望可以帮助提高工作流的效率以及提供更愉悦的体验。 你还有其他技巧要分享吗？ 使用上方的“编辑”按钮提交拉取请求，或使用下方的“反馈”按钮提交问题，我们可能会将其添加到列表中。

* 适用于 Linux 的 Windows 子系统旨在用作内部开发循环的一部分。 我们建议的示例工作流为创建 CI/CD 管道，使用 WSL 2 在 Windows 计算机上安装 Ubuntu，并使用实际的 Linux 实例进行本地开发。 在验证一切正常运行后，可以将该 CI/CD 管道向上推送到云中，方法是将其放入 Docker 容器中并将该容器推送到云实例，使其在生产就绪的 Ubuntu VM 上运行。 有关使用 WSL 的更多方法，请查看 [WSL 2 上的制表符和空格剧集](https://channel9.msdn.com/Shows/Tabs-vs-Spaces/WSL2-Code-faster-on-the-Windows-Subsystem-for-Linux)。

* 如果同时使用 Windows 和适用于 Linux 的 Windows 子系统，则安装了两个文件系统：NTSF (Windows) 和 WSL（Linux 分发版）。 为了提高性能，请确保项目文件与所使用的工具存储在同一系统中。 了解有关[选择正确的文件系统以提高性能](https://docs.microsoft.com/windows/wsl/compare-versions#use-the-linux-file-system-for-faster-performance)的详细信息。

* 可以通过更新 Windows Defender 设置针对足够信任的项目文件夹或文件类型添加排除项，以避免对其进行安全威胁扫描，从而提高生成速度。 了解有关如何[更新 Windows Defender 设置以提高性能](https://docs.microsoft.com/windows/android/defender-settings)的详细信息。

![Windows Defender 屏幕截图](../images/windows-defender-exclusions.png)

* 可以使用以下命令通过命令行在已打开的项目中启动 VS Code：`code .`；或者使用 Windows 或 WSL 分发版中的 `explorer.exe .`，使用 Windows 文件资源管理器通过命令行打开项目目录。 如果默认情况下不起作用，则可能需要将 VS Code 可执行文件添加到 PATH 环境变量中。 了解有关[从命令行启动](https://code.visualstudio.com/docs/editor/command-line#_launching-from-command-line)的详细信息。

![Windows 文件资源管理器屏幕截图](../images/wsl-file-explorer.png)

* 如果使用 Git 进行版本控制和协作，则可以[设置 Git 凭据管理器](https://docs.microsoft.com/windows/wsl/tutorials/wsl-git#git-credential-manager-setup)以将令牌存储在 Windows 凭据管理器中，从而简化身份验证过程。 我们还建议[向项目添加 .gitignore 文件](https://docs.microsoft.com/windows/wsl/tutorials/wsl-git#adding-a-git-ignore-file)。

* 可以使用 [Windows 终端命令行参数](https://docs.microsoft.com/windows/terminal/command-line-arguments?tabs=powershell#multiple-panes)在具有多个窗格的单个窗口中启动多个命令行（如 PowerShell、Ubuntu 和 Azure CLI）。 安装 [Windows 终端](https://docs.microsoft.com/windows/terminal/get-started)、[WSL/Ubuntu](https://docs.microsoft.com/windows/wsl/install-win10)和 [Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest) 后，请在 PowerShell 中输入以下命令，以打开包含全部三个命令行的新的多窗格窗口：

    ```powershell
    wt -p "Command Prompt" `; split-pane -p "Windows PowerShell" `; split-pane -H wsl.exe
    ```

![填充图像](../images/flashy-office2.png)

## <a name="transitioning-between-mac-and-windows"></a>在 Mac 和 Windows 之间转换

查看我们的[在 Mac 和 Windows（或适用于 Linux 的 Windows 子系统）之间进行转换的指南](https://docs.microsoft.com/windows/dev-environment/mac-to-windows)。 它可以帮助确定以下项之间的区别：

* [键盘快捷方式](https://docs.microsoft.com/windows/dev-environment/mac-to-windows#keyboard-shortcuts)
* [触控板快捷方式](https://docs.microsoft.com/windows/dev-environment/mac-to-windows#trackpad-shortcuts)
* [终端和 shell 工具](https://docs.microsoft.com/windows/dev-environment/mac-to-windows#terminal-and-shell)
* [应用和实用程序](https://docs.microsoft.com/windows/dev-environment/mac-to-windows#apps-and-utilities)

## <a name="stories-from-developers-who-have-switched"></a>来自已切换的开发人员的成功案例

我们认为，了解其他开发人员在 Mac 和 Windows 开发环境之间切换的经验可能会有所帮助。 大多数开发人员发现该过程相当简单，并很高兴他们仍然可以使用自己喜欢的 Linux 和开源工具，同时还可以拥有 Windows 生产力工具的集成访问权限，例如 [Microsoft Office](https://www.microsoft.com/microsoft-365/products-apps-services)、[Outlook](https://www.microsoft.com/microsoft-365/outlook/email-and-calendar-software-microsoft-outlook) 和 [Teams](https://www.microsoft.com/microsoft-365/microsoft-teams/group-chat-software)。 以下是我们发现的一些文章和博客条目：

* Ken Wang，[“Think Different — Software Developer Switching from Mac to Windows”](https://medium.com/@kenwang_57215/software-developer-switching-from-mac-to-windows-66773d331910)（转变思维方式 - 从 Mac 切换到 Windows 的软件开发人员）
* Owen Williams，[“The state of switching to Windows from Mac in 2019”](https://char.gd/blog/2019/the-state-of-switching-to-windows-from-mac-in-2019)（2019 年从 Mac 切换到 Windows 的现状）
* Brent Rose，[“What Happened When I Switched From Mac to Windows”](https://www.wired.com/story/rant-switching-from-mac-to-windows/)（我从 Mac 切换到 Windows 时发生了什么）
* Jack Franklin，[“Using Windows 10 and WSL for frontend web development”](https://www.jackfranklin.co.uk/blog/frontend-development-with-windows-10/)（使用 Windows 10 和 WSL 进行前端 Web 开发）
* Aaron Schlesinger，[“Coming from a Mac to Windows & WSL 2”](https://arschles.com/blog/coming-from-a-mac-to-windows-wsl-2/)（从 Mac 到 Windows 和 WSL 2）
* David Heinemeier Hansson，[“Back to windows after twenty years”](https://m.signalvnoise.com/back-to-windows-after-twenty-years/)（二十年后重拾 Windows）
* Ray Elenteny，[“Why I returned to Windows”](https://dzone.com/articles/why-i-returned-to-windows)（我为什么重拾 Windows）

## <a name="tutorials-courses-and-code-samples"></a>教程、课程和代码示例

我们在下面列出了一些教程、课程和代码示例，以帮助你着手处理一些常见的工作方案。

* [使用 React 和 Azure Cosmos DB 创建 MongoDB 应用](https://docs.microsoft.com/azure/cosmos-db/tutorial-develop-mongodb-react)

* [生成具有拖放功能的 Android 双屏应用](https://docs.microsoft.com/dual-screen/android/samples)

* [使用 Xamarin.Forms 生成待办事项列表跨平台应用](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/todo/)

* [生成利用 Google Play Services 演示 Google Maps API 的 Xamarin.Android 应用](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/todo/)

* [在 Azure 应用服务中使用 PostgreSQL 部署 Python (Django) Web 应用](https://docs.microsoft.com/azure/app-service/containers/tutorial-python-postgresql-app?tabs=bash)

* [使用 Blazor 生成第一个 ASP.Net Core Web 应用](https://docs.microsoft.com/aspnet/core/tutorials/build-your-first-blazor-app?view=aspnetcore-3.1)

* [使用 Microsoft Graph 生成 Java 应用](https://docs.microsoft.com/graph/tutorials/java)

* [使用 Azure AD V2 从 WPF 应用程序调用 ASP.NET Core Web API](https://docs.microsoft.com/samples/azure-samples/active-directory-dotnet-native-aspnetcore-v2/calling-an-aspnet-core-web-api-from-a-wpf-application-using-azure-ad-v2/?view=aspnetcore-3.1)

* [创建和部署云本机 ASP.NET Core 微服务](https://docs.microsoft.com/learn/modules/microservices-aspnet-core/?view=aspnetcore-3.1)

* [探索 Microsoft Learn 上的免费在线课程](https://docs.microsoft.com/learn/browse/)

![填充图像](../images/flashy-office3.png)

## <a name="additional-resources"></a>其他资源

* [Microsoft Edge Web 浏览器文档](https://docs.microsoft.com/microsoft-edge/)
* [尝试使用 WebHint 改进网站](https://webhint.io/)
* [Microsoft Game Stack 文档](https://docs.microsoft.com/gaming/)
