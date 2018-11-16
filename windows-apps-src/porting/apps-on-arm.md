---
title: 基于 ARM 的 Windows 10
author: msatranjr
description: 本文概述了 ARM 上的体验和应用运行方式，存在哪些限制，以及可以在何处了解详细信息。
ms.author: misatran
ms.date: 02/15/2018
ms.topic: article
keywords: windows 10 s, 始终连接, ARM, ARM64, x86 模拟
ms.localizationpriority: medium
ms.openlocfilehash: 8f62a873e84f200a019bde23038ae10b21150072
ms.sourcegitcommit: e38b334edb82bf2b1474ba686990f4299b8f59c7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/15/2018
ms.locfileid: "6843884"
---
# <a name="windows-10-on-arm"></a>基于 ARM 的 Windows 10
起初，Windows 10（与 Windows 10 移动版不同）只能在采用 x86 和 x64 处理器的电脑上运行。 但现在，应用了 Fall Creators Update 的 Windows 10 桌面版（专业版和 S 版本）可在采用 ARM64 处理器的计算机上运行。 ARM CPU 体系结构的省电特性使这些电脑拥有全天的电池使用时间，并且支持移动数据网络。 这些电脑将提供出色的应用程序兼容性，让你在不进行任何修改的情况下运行现有的 x86 win32 应用程序。 例如 Adobe reader。 有关详细信息或演示，请观看[第 9 频道视频：始终连接的电脑](https://channel9.msdn.com/Events/Build/2017/P4171)。 

在本文中，我们使用术语 *ARM* 作为在 ARM64（通常也称为 *AArch64*）处理器上运行 Windows 10 桌面版的电脑的简写，  使用术语 *ARM32*（在其他文档中通常称为 *ARM*）作为 32 位 ARM 体系结构的简写。

## <a name="apps-and-experiences-on-arm"></a>ARM 上的应用和体验

### <a name="built-in-windows-10-experiences-apps-and-drivers"></a>内置 Windows 10 体验、应用和驱动程序
Edge、Cortana、“开始”菜单、资源管理器等内置 Windows 10 体验都是本机的，以 ARM64（或 ARM32）方式运行。 这还包括所有设备驱动程序，如图形、网络或硬盘。 这可确保你的设备以 Qualcomm Snapdragon 处理器的纯本机速度运行，从而获得最佳的用户体验和电池使用时间。

### <a name="universal-windows-platform-uwp-apps"></a>通用 Windows 平台 (UWP) 应用
基于 ARM 的 Windows 10 可运行 Microsoft Store 中的所有 x86 和 ARM32 [UWP 应用](../get-started/universal-application-platform-guide.md)。 ARM32 应用在没有任何模拟的情况下以本机方式运行，x86 应用则在模拟方式下运行。 如果你是 UWP 开发人员，请确保为你的应用提交 ARM 包，因为这将为设备提供最佳的用户体验。 有关详细信息，请参阅[应用包体系结构](../packaging/device-architecture.md)。

>[!IMPORTANT] 
> 当用户从 Microsoft Store 下载 UWP 应用时，将在 ARM64 设备上安装 ARM32 版本，除非只有 x86 版本可用。 有关体系结构的详细信息，请参阅[应用包体系结构](../packaging/device-architecture.md)。

### <a name="win32-apps"></a>Win32 应用
除了 UWP 应用以外，基于 ARM 的 Windows 10 还能够在不进行任何修改的情况下运行 x86 Win32 应用（例如 Adobe Reader），提供像任何电脑一样良好的性能和无缝的用户体验。 不必为 ARM 重新编译这些 x86 win32 应用，它们甚至不会意识到自己运行在 ARM 处理器上。 请注意，不支持 64 位 x64 Win32 应用，但绝大多数应用都有 x86 版本，因此从用户角度看，只需选择 32 位 x86 安装程序，就能在基于 ARM 的 Windows 电脑上运行。

## <a name="in-this-section"></a>本部分内容
|主题 | 说明 |
|-----|-----|
|[x86 模拟如何在 ARM 上运行](apps-on-arm-x86-emulation.md)|详细介绍如何在 ARM 上模拟 x86 应用。|
|[基于 ARM 的 x86 应用疑难解答](apps-on-arm-troubleshooting-x86.md)|在 ARM 上运行 x86 应用的常见问题以及如何解决这些问题。 |
|[基于 ARM 的 ARM32 应用疑难解答](apps-on-arm-troubleshooting-arm32.md)|在 ARM 上运行 ARM32 应用的常见问题以及如何解决这些问题。 |
|[ARM 上的程序兼容性疑难解答](apps-on-arm-program-compat-troubleshooter.md)|介绍当你的应用无法在 ARM 上正常工作时如何调整兼容性设置。 |

## <a name="related-topics"></a>相关主题
|主题 | 说明 |
|-----|-----|
|[使用 WDK 构建 ARM64 驱动程序](https://docs.microsoft.com/en-us/windows-hardware/drivers/develop/building-arm64-drivers)|有关构建 ARM64 驱动程序的说明。 |
| [调试基于 ARM 的 x86 应用](https://docs.microsoft.com/en-us/windows-hardware/drivers/debugger/debugging-arm64) | 调试基于 ARM 的 x86 应用的指南。 |
