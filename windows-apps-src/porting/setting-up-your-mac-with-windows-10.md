---
author: stevewhims
description: 使用当前的 Mac 计算机开发 Windows 应用。
title: 在 Mac 上设置 Windows 10
ms.assetid: 6D520610-5DE0-476E-A792-AA57E002D309
ms.author: stwhi
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 661c324fbe7a80a6ff150da06536879a25c0c0c2
ms.sourcegitcommit: e814a13978f33654d8e995584f4b047cb53e0aef
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/05/2018
ms.locfileid: "6041783"
---
# <a name="setting-up-your-mac-with-windows-10"></a>在 Mac 上设置 Windows 10


使用当前的 Mac 计算机开发 Windows 应用。

## <a name="run-windows-on-your-mac-and-use-visual-studio"></a>在 Mac 上运行 Windows 并使用 Visual Studio

准备好开始开发通用 Windows 应用，却没有一台随手可用的电脑？ 没关系 - 你可以使用 Mac！ 与 Apple Boot Camp、 Oracle VirtualBox、 VMware Fusion 和 Parallels Desktop 等热门第三方解决方案，可以在 Apple 计算机上安装 windows 10 和 Microsoft Visual Studio。

**注意**你将需要在磁盘或 U 盘上的 windows 10 可启动映像。 如果你是 MSDN 订户，可以从 MSDN 订户下载中心下载安装映像。 如果你不是订户，可以从[Microsoft Store](http://apps.microsoft.com/windows/app)购买安装程序。 也可以从[此位置](http://go.microsoft.com/fwlink/?LinkId=623906)下载安装程序，这在你已运行 Windows 并希望升级时很有用。

运行 Windows 后，然后可以从[开发人员下载 windows 10](https://developer.microsoft.com/en-us/windows/downloads)安装最新版本的 Visual Studio 并开始编写应用 ！

**注意**如果你打算使用 Visual Studio 设备仿真程序，你**必须**安装 64 位 (x64) 版本的 windows 10 专业版或更好。 遗憾的是，某些较旧的 Mac 电脑无法运行 64 位 Windows。 请在此 [Apple 支持页面](http://go.microsoft.com/fwlink/p/?LinkID=397959)上与 Apple 联系以核实你的硬件是否兼容。

## <a name="apple-boot-camp"></a>Apple Boot Camp

每个新的 Mac 上预安装 Boot Camp Assistant 应用，并启动它将指导你完成安装 windows 10 的过程。 你只需要一个 Windows 的副本（来自上面列出的源）和至少 30 GB 的可用磁盘空间。 安装后，你可以选择启动到 Mac OSX 或 Windows 10 中。 有关详细信息，请参阅 Apple 的 [Boot Camp 说明页面](http://go.microsoft.com/fwlink/?LinkId=623912)。

## <a name="parallels-desktop"></a>Parallels Desktop

使用 Parallels Desktop 11，可以将包括 Visual Studio 和 Cortana 在内的 Windows 应用与现有的 Mac 应用程序并列运行。 专业版可用于开发人员，它具有一些额外功能，其中包括改进的调试以及对 Docker 和 Jenkins 的支持。 有关详细信息和免费试用版，请参阅 [Parallels Desktop](http://go.microsoft.com/fwlink/p/?LinkId=281827)。

## <a name="vmware-fusion"></a>VMWare Fusion

来自 VMWare 的 Fusion 8 可以使你在 Mac 桌面上直接运行 Visual Studio。 专业版可用于为开发人员提供一些更高级的功能，如 vSphere 支持 有关详细信息和免费试用版，请参阅 [VMWare Fusion](http://go.microsoft.com/fwlink/p/?LinkId=281826)。

## <a name="oracle-virtualbox"></a>Oracle VirtualBox

VirtualBox 是一款用于在计算机上运行虚拟机的免费应用程序，它支持在 Mac 上运行 Windows。 它是只提供基本服务的选项，但是价格很具有吸引力。 有关详细信息，请参阅 [VirtualBox](http://go.microsoft.com/fwlink/p/?LinkId=280599)。

