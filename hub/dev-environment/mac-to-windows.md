---
title: 从 Mac (Unix) 转到 Windows 的帮助
description: 本指南可帮助你从 Mac (Unix) 转换到 Windows 开发环境，具体内容包括快捷键映射和 Mac 和 Windows 之间概念差异概述。
author: mattwojo
ms.author: mattwoj
manager: jken
ms.topic: article
ms.technology: windows-nodejs
keywords: Mac 转到 Windows, 快捷键映射, 从 Unix 转到 Windows, 从 Mac 转到 Windows, 从 MacBook 转到 Surface 的帮助, Macintosh 用户如何使用 Windows, 从 Macintosh 切换到 Windows, 开发环境更改帮助, Mac OS X 转到 Windows, 从 Mac 转到电脑的帮助
ms.localizationpriority: medium
ms.date: 09/19/2019
ms.openlocfilehash: fa137ab51f0bb53e2907fa319d79ed77eb7ed655
ms.sourcegitcommit: 1e06168ada5ce6013b1d07c428548f084464a286
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/28/2020
ms.locfileid: "87363706"
---
# <a name="guide-for-changing-your-dev-environment-from-mac-to-windows"></a>将开发环境从 Mac 改为 Windows 的指南

以下提示和控件等效项应有助于在 Mac 和 Windows（或 WSL/Linux）开发环境之间转换。

对于应用开发，与 Xcode 最相近的等效项是 [Visual Studio](https://visualstudio.microsoft.com)。 如果不需要那么大的近似度，可以使用 [Visual Studio for Mac](https://visualstudio.microsoft.com/vs/mac/)。 如需进行跨平台源代码编辑（和获取大量插件），[Visual Studio Code](https://code.visualstudio.com/?wt.mc_id=DX_841432) 是最常用的选择。

## <a name="keyboard-shortcuts"></a>键盘快捷方式

| **操作** | **Mac** | **Windows** |
|---------------|--------------------|---------------------|
| 复制 | Command+C | Ctrl+C |
| 剪切 | Command+X | Ctrl+X |
| 粘贴 | Command+V | Ctrl+V |
| 撤消 | Command+Z | Ctrl+Z |
| 保存 | Command+S | Ctrl+S |
| 打开 | Command+O | Ctrl+O |
| 锁定计算机 | Command+Control+Q | WindowsKey+L |
| 显示桌面 | Command+F3 | WindowsKey+D |
| 打开文件浏览器 | Command+N | WindowsKey+E |
| 窗口最小化 | Command+M | WindowsKey+M |
| 搜索 | Command+空格 | WindowsKey |
| 关闭活动窗口 | Command+W | Control+W |
| 切换当前任务 | Command+Tab | Alt+Tab |
| 将窗口最大化至全屏 | Control+Command+F | WindowsKey+Up |
| 保存屏幕（截图） | Command+Shift+3 | WindowsKey+Shift+S |
| 保存窗口 | Command+Shift+4 | WindowsKey+Shift+S |
| 查看项信息或属性 | Command+I | Alt+Enter |
 | 选择全部项 | Command+A | Ctrl+A |
| 选择列表中的多个项（非连续） | 使用 Command，然后单击每个项 | 使用 Control 键，然后单击每个项 |
| 键入特殊字符 | Option + 字符键 | Alt + 字符键|

## <a name="trackpad-shortcuts"></a>触控板快捷方式

注意：其中一些快捷方式需要“Precision Trackpad”，如 Surface 设备和其他第三方笔记本电脑上的触控板。

 **操作** | **Mac** | **Windows** |
|---------------|--------------------|---------------------|
| 滚动 | 双指垂直轻扫 | 双指垂直轻扫 |
| 缩放 | 双指向内和向外收缩 | 双指向内和向外收缩 |
| 在视图间向前和向后轻扫 | 双指侧向轻扫 | 双指侧向轻扫 |
| 切换虚拟工作区 | 四指侧向轻扫 | 四指侧向轻扫 |
| 显示当前打开的应用 | 四指向上轻扫 | 三指向上轻扫 |
| 切换应用 | N/A | 三指侧向缓慢轻扫 |
| 转到桌面 | 四指向外滑动 | 三指向下轻扫 |
| 打开 Cortana/操作中心 | 双指从右侧开始滑动 | 三指点击 |
| 打开额外信息 | 三指点击 | N/A |
|显示启动板/启动应用 | 四指收缩 | 四指点击 |

注意：这两个平台上均可配置触控板选项。

## <a name="command-line-shells-and-terminals"></a>命令行 shell 和终端

Windows 支持多个命令行 shell 和终端，它们的工作方式有时与 Mac 的 BASH shell 和终端模拟器应用（例如 Terminal 和 iTerm）稍有不同。

### <a name="windows-shells"></a>Windows shell

Windows 有两个主要的命令行 shell：

1. **[PowerShell](https://docs.microsoft.com/powershell/scripting/overview?view=powershell-7)** - PowerShell 是一个跨平台的任务自动化和配置管理框架，由基于 .NET 构建的命令行 shell 和脚本语言组成。 使用 PowerShell，管理员、开发人员和高级用户可以快速控制和自动完成以下任务：管理复杂流程，以及环境和运行环境的操作系统的各个方面。 PowerShell [完全开放源代码](https://github.com/powershell/powershell)，并且由于它是跨平台的，因此也同样[适用于 Mac 和 Linux](https://docs.microsoft.com/powershell/scripting/install/installing-powershell?view=powershell-7)。

    **Mac 和 Linux BASH shell 用户**：PowerShell 还支持许多你已经熟悉的命令别名。 例如：
    - 使用 `ls` 列出当前目录的内容
    - 使用 `mv` 移动文件
    - 使用 `cd <path>` 移动到新目录

    PowerShell 和 BASH 中的某些命令和参数是不同的。 要了解详细信息，请在 PowerShell 中输入 [`get-help`](https://docs.microsoft.com/powershell/scripting/learn/ps101/02-help-system?view=powershell-7) 或在文档中查看[兼容性别名](https://docs.microsoft.com/powershell/scripting/samples/appendix-1---compatibility-aliases?view=powershell-7)。

    要以管理员身份运行 PowerShell，请在 Windows“开始”菜单中输入“PowerShell”，然后选择“以管理员身份运行”。

2. **Windows 命令行 (Cmd)** ：Windows 仍然提供传统的命令提示符（和控制台 - 见下文），以提供与当前和旧版 MS-DOS-compatible 命令和批处理文件的兼容性。 Cmd 在运行现有/旧批处理文件或命令行操作时很有用，但一般情况下，建议用户了解和使用 PowerShell，因为 Cmd 现在正处于维护中，并且将来不会收到任何改进或新功能。

### <a name="linux-shells"></a>Linux shell

现在可以安装适用于 Linux 的 Windows 子系统 (WSL) 来支持在 Windows 中运行 Linux shell。 这意味着你可以使用所选的任意特定 Linux 发行版，运行 Windows 内集成的 bash。 使用 WSL 可提供 Mac 用户最熟悉的环境类型。 例如，你可以使用 ls 列出当前目录中的文件，而不是像在传统 Windows Cmd Shell 中那样使用 dir 。 若要了解如何安装和使用 WSL，请参阅[适用于 Linux 的 Windows 子系统安装指南 (Windows 10)](https://docs.microsoft.com/windows/wsl/install-win10)。 可以使用 WSL 在 Windows 上安装的 Linux 发行版包括：

1. [Ubuntu 20.04 LTS](https://www.microsoft.com/store/apps/9n6svws3rx71)
2. [Kali Linux](https://www.microsoft.com/store/apps/9PKR34TNCV07)
3. [Debian GNU/Linux](https://www.microsoft.com/store/apps/9MSVKQC78PK6)
4. [openSUSE Leap 15.1](https://www.microsoft.com/store/apps/9NJFZK00FGKV)
5. [SUSE Linux Enterprise Server 15 SP1](https://www.microsoft.com/store/apps/9PN498VPMF3Z)

这只是其中一部分。 可在 [WSL 安装文档](https://docs.microsoft.com/windows/wsl/install-win10#install-your-linux-distribution-of-choice)中查找详细信息，并直接从 [Microsoft Store](https://www.microsoft.com/search/shop/apps?q=linux&category=Developer+tools) 安装它们。

## <a name="windows-terminals"></a>Windows 终端

除了许多第三方产品/服务外，Microsoft 还提供了两个“终端”- GUI 应用程序（用于访问命令行 shell 和应用程序）。

1. **[Windows 终端](https://docs.microsoft.com/windows/terminal/)** ：Windows 终端是一种高度可配置的全新现代化命令行终端应用程序，它提供了超高性能、低延迟的命令行用户体验、多个选项卡、拆分窗口窗格、自定义主题和样式、针对不同 shell 或命令行应用的多个“配置文件”，并使用户有大量机会可以配置命令行用户体验的许多方面并对其进行个性化设置。

    可以使用 Windows 终端打开连接到 PowerShell、WSL shell（例如 Ubuntu 或 Debian）、传统 Windows 命令提示符或任何其他命令行应用（例如 SSH、Azure CLI、Git Bash）的选项卡。

2. **[控制台](https://docs.microsoft.com/windows/console/)** ：在 Mac 和 Linux 上，用户通常启动其首选的终端应用程序，然后创建并连接到用户的默认 shell（例如 BASH）。

    然而，由于历史原因，Windows 用户通常会启动其 shell，而 Windows 会自动启动并连接 GUI 控制台应用。

    尽管仍可以直接启动 shell 并使用旧版 Windows 控制台，但强烈建议用户安装并使用 Windows 终端来体验最佳、最快、最高效的命令行体验。

## <a name="apps-and-utilities"></a>应用和实用程序

 **应用** | **Mac** | **Windows** |
|---------------|--------------------|---------------------|
| 设置和首选项 | 系统首选项 | Settings |
| 任务管理器 | 活动监视器 | 任务管理器 |
| 磁盘格式化 | 磁盘实用程序 | 磁盘管理 |
| 文本编辑 | TextEdit | 记事本 |
| 事件查看 | 控制台 | 事件查看器 |
| 查找文件/应用 | Command+空格 | Windows 键 |
