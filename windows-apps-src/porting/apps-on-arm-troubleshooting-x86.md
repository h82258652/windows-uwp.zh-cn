---
title: x86 桌面应用疑难解答
description: 在 ARM 上运行 x86 应用的常见问题以及如何解决这些问题。
ms.author: misatran
ms.date: 05/09/2018
ms.topic: article
keywords: windows 10 s, 始终连接, ARM 上的 x86 模拟, 疑难解答
ms.localizationpriority: medium
ms.openlocfilehash: 01ef13f6f27b45a4cc41244e4ebed0a54804fc8e
ms.sourcegitcommit: ed0304b8a214c03b8aab74b8ef12c9f82b8e3c5f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/19/2018
ms.locfileid: "7300092"
---
# <a name="troubleshooting-x86-desktop-apps"></a>x86 桌面应用疑难解答
>[!IMPORTANT]
> ARM64 SDK 现已作为 Visual Studio 15.8 Preview 1 的组成部分提供。 我们建议你将应用重新编译到 ARM64 以便应用以纯本机速度运行。 有关更多信息，请参阅博客文章[针对 ARM 开发的 Visual Studio 早期预览版 Windows 10 支持](https://blogs.windows.com/buildingapps/2018/05/08/visual-studio-support-for-windows-10-on-arm-development/)。

如果 x86 桌面应用无法像在 x86 计算机上那样正常工作，请参阅以下指南以帮助你排除故障。

|问题|解决方案|
|-----|--------|
| 你的应用依赖于不是为 ARM 设计的驱动程序。 | 将你的 x86 驱动程序重新编译为 ARM64 驱动程序。 请参阅[使用 WDK 构建 ARM64 驱动程序](https://docs.microsoft.com/en-us/windows-hardware/drivers/develop/building-arm64-drivers)。 |
| 你的应用只适用于 x64。 | 如果你的应用是为 Microsoft Store 开发的，请提交应用的 ARM 版本。 有关详细信息，请参阅[应用包体系结构](../packaging/device-architecture.md)。 如果你是 Win32 开发人员，我们建议将你的应用重新编译到 ARM64。 有关更多信息，请参阅[针对 ARM 开发的 Visual Studio 早期预览版 Windows 10 支持](https://blogs.windows.com/buildingapps/2018/05/08/visual-studio-support-for-windows-10-on-arm-development/)。 |
| 你的应用使用了高于 1.1 版本的 OpenGL，或者需要硬件加速的 OpenGL。 | 如果可用，请使用应用的 DirectX 模式。 使用 DirectX 9、DirectX 10、DirectX 11 和 DirectX 12 的 x86 应用将可在 ARM 上正常运行。 有关详细信息，请参阅 [DirectX 图形和游戏](https://msdn.microsoft.com/en-us/library/windows/desktop/ee663274(v=vs.85).aspx)。 |
| 你的 x86 应用无法按预期工作。 | 按照 [ARM 上的程序兼容性疑难解答](apps-on-arm-program-compat-troubleshooter.md)中的指南尝试使用兼容性疑难解答。 有关其他一些疑难解答步骤，请参阅[基于 ARM 的 x86 应用疑难解答](apps-on-arm-troubleshooting-x86.md)。 |

## <a name="best-practices-for-wow"></a>WOW 最佳实践
当应用发现它在 WOW 下运行并假定它位于 x64 系统上时，会出现一个常见问题。 做出上述假设后，该应用可能会执行以下操作：

- 尝试安装应用的 x64 版本，这在 ARM 上不受支持。
- 检查本机注册表视图下的其他软件。
- 假定可以使用 64 位的 .NET Framework。

通常，当应用确定在 WOW 下运行时，其不应该对主机系统做出假设。 尽可能避免与操作系统的本机组件进行交互。

应用可以将注册表项放在本机注册表视图下，或者根据 WOW 的存在执行功能。 原来的 **IsWow64Process** 只指示应用是否运行在 x64 计算机上。 现在，应用应该使用 [IsWow64Process2](https://msdn.microsoft.com/en-us/library/windows/desktop/mt804318(v=vs.85).aspx) 确定自己是否运行在带有 WOW 支持的系统上。 

## <a name="drivers"></a>驱动程序 
所有内核模式驱动程序、[用户模式驱动程序框架 (UMDF)](https://docs.microsoft.com/windows-hardware/drivers/wdf/overview-of-the-umdf) 驱动程序和打印驱动程序都必须进行编译，以匹配操作系统的体系结构。 如果 x86 应用具有驱动程序，则必须为 ARM64 重新编译该驱动程序。 x86 应用也许能够在模拟环境中良好运行，但必须为 ARM64 重新编译其驱动程序，并且依赖该驱动程序的任何应用体验都将无法使用。 有关为 ARM64 编译驱动程序的详细信息，请参阅[使用 WDK 构建 ARM64 驱动程序](https://docs.microsoft.com/windows-hardware/drivers/develop/building-arm64-drivers)。

## <a name="shell-extensions"></a>Shell 扩展 
尝试连接 Windows 组件或将 DLL 加载到 Windows 进程中的应用需要重新编译这些 DLL，以匹配系统的体系结构，即 ARM64。 输入法编辑器 (IME)、辅助技术、shell 扩展应用（例如，在资源管理器中显示云存储图标或右键单击上下文菜单）通常会使用这种方法。 若要了解如何将应用或 DLL 重新编译到 ARM64，请参阅博客文章[针对 ARM 开发的 Visual Studio 早期预览版 Windows 10 支持](https://blogs.windows.com/buildingapps/2018/05/08/visual-studio-support-for-windows-10-on-arm-development/)。 

## <a name="debugging"></a>调试
要更深入地调查应用的行为，请参阅[在 ARM 上进行调试](https://docs.microsoft.com/en-us/windows-hardware/drivers/debugger/debugging-arm64)，以详细了解在 ARM 上进行调试的工具和策略。

## <a name="virtual-machines"></a>虚拟机
Qualcomm Snapdragon 835 移动电脑平台不支持 Windows 虚拟机监控程序平台。 因此，使用 Hyper-V 运行虚拟机将失败。 我们将继续在未来的 Qualcomm 芯片组上投资这些技术。 