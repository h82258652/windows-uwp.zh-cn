---
description: 使用当前的 Mac 计算机开发 Windows 应用。
title: 在 Mac 上设置 Windows 10
ms.assetid: 6D520610-5DE0-476E-A792-AA57E002D309
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 55165e0369c6bda64c19dc384c5c2addf224b8ba
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/20/2019
ms.locfileid: "74259117"
---
# <a name="setting-up-your-mac-with-windows-10"></a>在 Mac 上设置 Windows 10


使用当前的 Mac 计算机开发 Windows 应用。

## <a name="run-windows-on-your-mac-and-use-visual-studio"></a>在 Mac 上运行 Windows 并使用 Visual Studio

准备好开始开发通用 Windows 应用，却没有一台随手可用的电脑？ 没关系 - 你可以使用 Mac！ 借助 Apple Boot Camp、Oracle VirtualBox、VMware 合成和同类桌面等常见的第三方解决方案，你可以在 Apple 计算机上安装 Windows 10 和 Microsoft Visual Studio。

**请注意**  你将需要在磁盘或 u 盘上使用 Windows 10 可启动映像。 如果你是 MSDN 订户，可以从 MSDN 订户下载中心下载安装映像。 如果您不是订阅者，则可以从[Microsoft Store](https://www.microsoft.com/store/apps)购买安装程序。 也可以从[此位置](https://www.microsoft.com/software-download/windows10)下载安装程序，这在你已运行 Windows 并希望升级时很有用。

运行 Windows 后，可以从[适用于 windows 10 的开发人员下载](https://developer.microsoft.com/en-us/windows/downloads)安装最新版本的 Visual Studio 并开始编写应用！

**请注意**  如果计划使用 Visual Studio 设备仿真程序，则**必须**安装64位（X64）版本的 Windows 10 专业版或更高版本。 遗憾的是，某些较旧的 Mac 电脑无法运行 64 位 Windows。 请在此 [Apple 支持页面](https://support.apple.com/kb/HT5634)上与 Apple 联系以核实你的硬件是否兼容。

## <a name="apple-boot-camp"></a>Apple Boot Camp

每个最近的 Mac 上都预安装了 Boot Camp Assistant 应用，启动它将引导你完成安装 Windows 10 的过程。 你只需要一个 Windows 的副本（来自上面列出的源）和至少 30 GB 的可用磁盘空间。 安装后，你可以选择启动到 Mac OSX 或 Windows 10 中。 有关详细信息，请参阅 Apple 的 [Boot Camp 说明页面](https://support.apple.com/HT201468)。

## <a name="parallels-desktop"></a>Parallels Desktop

使用 Parallels Desktop 11，可以将包括 Visual Studio 和 Cortana 在内的 Windows 应用与现有的 Mac 应用程序并列运行。 专业版可用于开发人员，它具有一些额外功能，其中包括改进的调试以及对 Docker 和 Jenkins 的支持。 有关详细信息和免费试用版，请参阅 [Parallels Desktop](https://www.parallels.com/download/desktop/)。

## <a name="vmware-fusion"></a>VMWare Fusion

来自 VMWare 的 Fusion 8 可以使你在 Mac 桌面上直接运行 Visual Studio。 专业版可用于为开发人员提供一些更高级的功能，如 vSphere 支持 有关详细信息和免费试用版，请参阅 [VMWare Fusion](http://www.vmware.com/products/fusion/)。

## <a name="oracle-virtualbox"></a>Oracle VirtualBox

VirtualBox 是一款用于在计算机上运行虚拟机的免费应用程序，它支持在 Mac 上运行 Windows。 它是只提供基本服务的选项，但是价格很具有吸引力。 有关详细信息，请参阅 [VirtualBox](https://www.virtualbox.org/wiki/Downloads)。

