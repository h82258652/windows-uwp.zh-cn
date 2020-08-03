---
title: 改进 Windows 10 上的开发工作流的技巧
description: 改进 Windows 10 上的开发工作流的技巧。
author: mattwojo
ms.author: mattwoj
manager: jken
ms.topic: article
ms.technology: windows-nodejs
keywords: Microsoft, Windows, 开发人员, 使用技巧, 性能, WSL
ms.localizationpriority: medium
ms.date: 07/24/2020
ms.openlocfilehash: 8c094e7871e9de4fdf7eca2e0e1b425af295f252
ms.sourcegitcommit: 5ba2524d237be82d3621551e48cac938fe81d2ea
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/28/2020
ms.locfileid: "87255011"
---
# <a name="tips-for-improving-performance-and-development-workflows"></a>改进性能和开发工作流的技巧

我们收集了一些技巧，希望可以帮助提高工作流的效率以及提供更愉悦的体验。 你还有其他技巧要分享吗？ 使用上方的“编辑”按钮提交拉取请求，或使用下方的“反馈”按钮提交问题，我们可能会将其添加到列表中。

## <a name="use-shortcuts-to-open-a-project-in-vs-code-or-windows-file-explorer"></a>使用快捷方式在 VS Code 或 Windows 文件资源管理器中打开项目

可以使用以下命令通过命令行在已打开的项目中启动 VS Code：`code .`；或者使用 Windows 或 WSL 分发版中的 `explorer.exe .`，使用 Windows 文件资源管理器通过命令行打开项目目录。 如果默认情况下不起作用，则可能需要将 VS Code 可执行文件添加到 PATH 环境变量中。 了解有关[从命令行启动](https://code.visualstudio.com/docs/editor/command-line#_launching-from-command-line)的详细信息。

![Windows 文件资源管理器屏幕截图](../images/wsl-file-explorer.png)

## <a name="use-the-credential-manager-to-your-streamline-authentication-process"></a>使用凭据管理器简化身份验证过程

如果使用 Git 进行版本控制和协作，则可以[设置 Git 凭据管理器](https://docs.microsoft.com/windows/wsl/tutorials/wsl-git#git-credential-manager-setup)以将令牌存储在 Windows 凭据管理器中，从而简化身份验证过程。 我们还建议[向项目添加 .gitignore 文件](https://docs.microsoft.com/windows/wsl/tutorials/wsl-git#adding-a-git-ignore-file)。

## <a name="use-wsl-for-testing-your-production-pipeline-before-deploying-to-the-cloud"></a>在部署到云之前，使用 WSL 测试生产管道

适用于 Linux 的 Windows 子系统可让开发人员按原样运行 GNU/Linux 环境 - 包括大多数命令行工具、实用工具和应用程序 - 且不会产生传统虚拟机或双启动设置开销。

WSL 面向开发人员受众，旨在用作内部开发流程的一部分。 假设何石要创建一个 CI/CD 管道（持续集成和持续交付），而且他想先在本地计算机（笔记本电脑）上测试它，然后再将它部署到云中。 何石可启用 WSL（和 WSL 2 来提高速度和性能），然后在本地（在笔记本电脑上）将正版 Linux Ubuntu 实例与所需的任何 Bash 命令和功能搭配使用。 在本地验证开发管道后，何石可将该 CI/CD 管道向上推送到云中（也就是 Azure 中），方法是将它放入 Docker 容器并将该容器推送到云实例，使其在生产就绪的 Ubuntu VM 上运行。

有关使用 WSL 的更多方法，请查看 [WSL 2 上的制表符和空格剧集](https://channel9.msdn.com/Shows/Tabs-vs-Spaces/WSL2-Code-faster-on-the-Windows-Subsystem-for-Linux)。

## <a name="improve-performance-speed-for-wsl-by-not-crossing-over-file-systems"></a>通过避免跨越文件系统提高 WSL 的性能速度

如果同时使用 Windows 和适用于 Linux 的 Windows 子系统，则安装了两个文件系统：NTSF (Windows) 和 WSL（Linux 分发版）。 为了提高性能，请确保项目文件与所使用的工具存储在同一系统中。 了解有关[选择正确的文件系统以提高性能](https://docs.microsoft.com/windows/wsl/compare-versions#use-the-linux-file-system-for-faster-performance)的详细信息。

## <a name="improve-build-speeds-by-adding-windows-defender-exclusions"></a>通过添加 Windows Defender 排除项提高生成速度

可以通过更新 Windows Defender 设置针对足够信任的项目文件夹或文件类型添加排除项，以避免对其进行安全威胁扫描，从而提高生成速度。 了解有关如何[更新 Windows Defender 设置以提高性能](https://docs.microsoft.com/windows/android/defender-settings)的详细信息。

![Windows Defender 屏幕截图](../images/windows-defender-exclusions.png)

## <a name="launch-all-your-command-lines-in-windows-terminal-at-once"></a>在 Windows 终端一次性启动所有命令行

* 可以使用 [Windows 终端命令行参数](https://docs.microsoft.com/windows/terminal/command-line-arguments?tabs=powershell#multiple-panes)在具有多个窗格的单个窗口中启动多个命令行（如 PowerShell、Ubuntu 和 Azure CLI）。 安装 [Windows 终端](https://docs.microsoft.com/windows/terminal/get-started)、[WSL/Ubuntu](https://docs.microsoft.com/windows/wsl/install-win10)和 [Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest) 后，请在 PowerShell 中输入以下命令，以打开包含全部三个命令行的新的多窗格窗口：

    ```powershell
    wt -p "Command Prompt" `; split-pane -p "Windows PowerShell" `; split-pane -H wsl.exe
    ```

## <a name="share-your-tips"></a>共享技巧

你是否有技巧可以帮助其他使用 Windows 的开发人员改进工作流？ 如果你希望我们添加关于特定主题的提示，请通过[提交拉取请求](https://github.com/MicrosoftDocs/windows-uwp/edit/docs/hub/dev-environment/overview.md)向页面添加使用技巧，或[提交问题](https://github.com/MicrosoftDocs/windows-uwp/issues/new?title=&body=%0A%0A%5BEnter%20feedback%20here%5D%0A%0A%0A---%0A%23%23%23%23%20Document%20Details%0A%0A%E2%9A%A0%20*Do%20not%20edit%20this%20section.%20It%20is%20required%20for%20docs.microsoft.com%20%E2%9E%9F%20GitHub%20issue%20linking.*%0A%0A*%20ID%3A%207779352b-7b4e-dad8-7c1b-b9aba2c5e561%0A*%20Version%20Independent%20ID%3A%20a5b81b80-87a1-b6e2-8936-baf6c1a0b9c5%0A*%20Content%3A%20%5BSet%20up%20your%20Windows%2010%20development%20environment%5D(https%3A%2F%2Fdocs.microsoft.com%2Fen-us%2Fwindows%2Fdev-environment%2Foverview)%0A*%20Content%20Source%3A%20%5Bhub%2Fdev-environment%2Foverview.md%5D(https%3A%2F%2Fgithub.com%2FMicrosoftDocs%2Fwindows-uwp%2Fblob%2Fdocs%2Fhub%2Fdev-environment%2Foverview.md)%0A*%20Product%3A%20**dev-environment**%0A*%20Technology%3A%20**windows-nodejs**)。

你是否希望我们解决某些与性能相关的问题？ 请在 [WinDev 问题存储库](https://github.com/microsoft/windev)中提交问题。

感谢开发人员。 我们一直在倾听并努力改善你的体验！
