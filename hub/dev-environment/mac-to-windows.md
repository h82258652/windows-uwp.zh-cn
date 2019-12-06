---
title: 帮助从 Mac （Unix）迁移到 Windows
description: 本指南可帮助你从 Mac （Unix）转换到 Windows 开发环境，包括快捷键映射和 Mac 和 Windows 之间不同概念的简要概述。
author: mattwojo
ms.author: mattwoj
manager: jken
ms.topic: article
ms.technology: windows-nodejs
keywords: Mac 到 Windows，快捷键映射，从 Unix 迁移到 windows，从 Mac 过渡到 Windows，帮助从 MacBook 移动到表面，如何将 Windows 用于 Macintosh 用户，如何将 Windows 切换为 Windows，帮助更改开发环境 Mac OS X，将其从 Macintosh 切换到 Windows，帮助从 Mac 移动到 PC
ms.localizationpriority: medium
ms.date: 09/19/2019
ms.openlocfilehash: a4e71143730184db094df2a7e8f1416cbaf244c4
ms.sourcegitcommit: f5bb4e35d1373b982259e61547b3b1765da0e78c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/06/2019
ms.locfileid: "74881271"
---
# <a name="guide-for-changing-your-dev-environment-from-mac-to-windows"></a>将开发环境从 Mac 更改为 Windows 的指南

以下提示和控制等效项应有助于在 Mac 和 Windows （或 WSL/Linux）开发环境之间过渡。

对于应用开发，最接近 Xcode 的等效项将是[Visual Studio](https://visualstudio.microsoft.com)。 如果您觉得需要返回，还有一个[Visual Studio for Mac](https://visualstudio.microsoft.com/vs/mac/)版本。 对于跨平台源代码编辑（和大量插件） [Visual Studio Code](https://code.visualstudio.com/?wt.mc_id=DX_841432)是最常用的选项。

## <a name="keyboard-shortcuts"></a>键盘快捷方式

| **运算** | **Mac** | **Windows** |
|---------------|--------------------|---------------------|
| “复制” | Command + C | Ctrl+C |
| Cut | Command + X | Ctrl+X |
| 粘帖 | Command + V | Ctrl+V |
| 撤销 | Command + Z | Ctrl+Z |
| “保存” | Command + S | Ctrl+S |
| 打开 | Command + O | Ctrl+O |
| 锁定计算机 | Command + Control + Q | WindowsKey + L |
| 显示桌面 | Command + F3 | WindowsKey + D |
| 最小化窗口 | Command + M | WindowsKey + M |
| “搜索” | 命令 + Space | WindowsKey |
| 关闭活动窗口 | Command + W | 控制 + W |
| 切换当前任务 | Command + Tab | Alt+Tab |
| 保存屏幕（屏幕快照） | Command + Shift + 3 | WindowsKey + Shift + S |
| 保存窗口 | Command + Shift + 4 | WindowsKey + Shift + S |
| 查看项信息或属性 | Command + I | Alt+Enter |
 | 选择全部项 | Command + A | Ctrl+A |
| 在列表中选择多个项目（非连续） | 命令，然后单击每个项目 | Control，然后单击每个项目 |
| 键入特殊字符 | 选项 + 字符键 | Alt + 字符键|

## <a name="trackpad-shortcuts"></a>触控板快捷方式

注意：其中一些快捷方式需要 "Precision 触控板"，例如触控板 on Surface 设备和其他第三方便携式计算机。

 **运算** | **Mac** | **Windows** |
|---------------|--------------------|---------------------|
| 滚动 | 双指垂直滑动 | 双指垂直滑动 |
| 缩放 | 两个推诿扯皮 | 两个推诿扯皮 |
| 在视图之间向后轻扫 | 双指侧向刷 | 双指侧向刷 |
| 切换虚拟工作区 | 四指侧向滑动 | 四指侧向滑动 |
| 显示当前打开的应用 | 四指向上轻扫 | 三个手指向上轻扫 |
| 在应用之间切换 | N/A | 缓慢的三指侧向滑动 |
| 中转到桌面 | 向外展开四个手指 | 向下轻扫的三指 |
| 打开 Cortana/操作中心 | 从右侧滑动两指 | 三指点击 |
| 打开额外信息 | 三指点击 | N/A |
|显示快速启动板/启动应用 | 带四指的挤压 | 用四个手指点击 |

注意：可在两个平台上配置触控板选项。

## <a name="terminal-and-shell"></a>终端和 Shell

Windows 提供了几种用于 Mac 的终端模拟器的替代方法。

1. Windows 命令行

Windows 命令行将接受 DOS 命令，是最常用的 Windows 命令行工具。 若要打开它：按**WindowsKey + R**打开 "**运行**" 框，键入**Cmd** ，然后单击 **"确定"** 。 若要打开管理员命令行，请键入**cmd** ，然后按**Ctrl + Shift + enter**。

2. PowerShell

[Powershell](https://docs.microsoft.com/powershell/scripting/overview?view=powershell-6)是一种基于任务的命令行外壳和脚本语言，它是在 .net 上构建的。 PowerShell 可帮助系统管理员和高级用户快速自动执行管理操作系统的任务 "。 换句话说，它是一个功能非常强大的命令行，特别是系统管理员。

顺便说一下，PowerShell[还适用于 Mac](https://docs.microsoft.com/powershell/scripting/install/installing-powershell-core-on-macos?view=powershell-6)。

3. 适用于 Linux 的 Windows 子系统 (WSL)

WSL 允许在 Windows 中运行 Linux shell。 这意味着，你可以运行*bash** 或其他 shell，具体取决于所安装的选项和特定的 Linux 发行版。 使用 WSL 可提供 Mac 用户最熟悉的环境类型。 例如，**你将在**当前目录中列出文件，**而不是**在 Windows 命令行中列出文件。 若要了解 instaling 和使用 WSL，请参阅[适用于 windows 10 的适用于 Linux 的 Windows 子系统安装指南](https://docs.microsoft.com/windows/wsl/install-win10)。

## <a name="apps-and-utilities"></a>应用和实用程序

 **应用** | **Mac** | **Windows** |
|---------------|--------------------|---------------------|
| 设置和首选项 | 系统首选项 | “设置” |
| 任务管理器 | 活动监视器 | “任务管理器” |
| 磁盘格式 | 磁盘实用工具 | 磁盘管理 |
| 文本编辑 | TextEdit | 记事本 |
| 事件查看 | 主机 | 事件查看器 |
| 查找文件/应用 | 命令 + Space | Windows 键 |
