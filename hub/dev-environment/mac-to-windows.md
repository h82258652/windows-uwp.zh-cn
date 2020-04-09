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
ms.openlocfilehash: 8c23fa3e6791a3cd78d259b40e68606a30fd9395
ms.sourcegitcommit: c660def841abc742600fbcf6ed98e1f4f7beb8cc
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/24/2020
ms.locfileid: "80218437"
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

## <a name="terminal-and-shell"></a>终端和 Shell

Windows 针对 Mac 的终端模拟器提供了多个替代选项。

1. Windows 命令行

Windows 命令行会接受 DOS 命令，是 Windows 上最常用的命令行工具。 若要打开它，请执行以下操作：按 WindowsKey+R 打开“运行”框，然后键入 cmd 并单击“确定”     。 若要打开管理员命令行，请键入 cmd  ，然后按 Ctrl+Shift+Enter  。

2. PowerShell

[PowerShell](https://docs.microsoft.com/powershell/scripting/overview?view=powershell-6) 是根据 .NET 构建的基于任务的命令行 Shell 和脚本语言。 PowerShell 可帮助系统管理员和高级用户快速自动执行管理操作系统的任务。 换句话说，它是一个功能非常强大的命令行，尤其受系统管理员喜欢。

顺便说一下，PowerShell [也可用于 Mac](https://docs.microsoft.com/powershell/scripting/install/installing-powershell-core-on-macos?view=powershell-6)。

3. 适用于 Linux 的 Windows 子系统 (WSL)

通过 WSL 可以在 Windows 中运行 Linux shell。 这意味着，你可以运行 bash 或其他 shell，具体取决于选择和安装的特定 Linux 发行版  。 使用 WSL 可提供 Mac 用户最熟悉的环境类型。 例如，你可以使用 ls 列出当前目录中的文件，而不是像在 Windows 命令行中使用 dir   。 若要了解如何安装和使用 WSL，请参阅[适用于 Linux 的 Windows 子系统安装指南 (Windows 10)](https://docs.microsoft.com/windows/wsl/install-win10)。

4. Windows 终端（预览）

Windows 终端是一个结合命令行工具和多个来源的 shell 的应用程序，包括传统的 Windows 命令行、PowerShell 以及适用于 Linux 的 Windows 子系统。 尽管仍处于预览版，但它已经包含多个实用的功能，例如支持多个选项卡、拆分窗格、自定义主题和样式和完整的 Unicode 支持。 可以从 [Windows 10 上的 Microsoft Store](https://www.microsoft.com/en-us/p/windows-terminal-preview/9n0dx20hk701?activetab=pivot:overviewtab) 安装 Windows 终端。

## <a name="apps-and-utilities"></a>应用和实用程序

 **应用** | **Mac** | **Windows** |
|---------------|--------------------|---------------------|
| 设置和首选项 | 系统首选项 | Settings |
| 任务管理器 | 活动监视器 | 任务管理器 |
| 磁盘格式化 | 磁盘实用程序 | 磁盘管理 |
| 文本编辑 | TextEdit | 记事本 |
| 事件查看 | 控制台 | 事件查看器 |
| 查找文件/应用 | Command+空格 | Windows 键 |
