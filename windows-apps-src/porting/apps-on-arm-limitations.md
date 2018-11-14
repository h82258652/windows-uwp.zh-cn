---
title: ARM 上的应用和体验限制
author: msatranjr
description: 适用于无法在 ARM 上正常工作的应用的疑难解答步骤。
ms.author: misatran
ms.date: 02/15/2018
ms.topic: article
keywords: windows 10 s, 始终连接, 限制, 基于 ARM 的 Windows 10
ms.localizationpriority: medium
redirect_url: https://docs.microsoft.com/en-us/windows/uwp/porting/apps-on-arm-troubleshooting-x86
ms.openlocfilehash: 24afc8a876b976f21d0f4ebd5892ceef7c403018
ms.sourcegitcommit: 4d88adfaf544a3dab05f4660e2f59bbe60311c00
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/13/2018
ms.locfileid: "6459492"
---
# <a name="limitations-of-apps-and-experiences-on-arm"></a>ARM 上的应用和体验限制
基于 ARM 的 Windows 10 有以下必要限制：

- **只支持 ARM64 驱动程序**。 与所有体系结构一样，内核模式驱动程序、[用户模式驱动程序框架 (UMDF)](https://docs.microsoft.com/en-us/windows-hardware/drivers/wdf/overview-of-the-umdf) 驱动程序和打印驱动程序必须进行编译，以匹配操作系统的体系结构。 虽然 ARM 操作系统有模拟 x86 用户模式应用的功能，但目前尚未模拟为其他体系结构（如 x64 或 x86）实现的驱动程序，因此在该平台上不受支持。 任何使用自定义驱动程序的应用都需要移植到 ARM64。 在有限的情况下，应用可以在模拟环境下以 x86 方式运行，但应用的驱动程序部分必须移植到 ARM64。 有关为 ARM64 编译驱动程序的详细信息，请参阅[使用 WDK 构建 ARM64 驱动程序](https://review.docs.microsoft.com/en-us/windows-hardware/drivers/develop/building-arm64-drivers?branch=rs4-arm64)。

- **不支持 x64 应用**。 基于 ARM 的 Windows 10 不支持 x64 应用模拟。

- **某些游戏无法正常运行**。 使用低于 1.1 版本的 OpenGL 或需要硬件加速 OpenGL 的游戏和应用无法正常运行。 此外，该平台不支持依赖“反作弊”驱动程序的游戏。

- **自定义 Windows 体验的应用可能无法正常工作**。 本机操作系统组件无法加载非本机组件。 此类应用示例通常包括输入法编辑器 (IME)、辅助技术、云存储应用等。 IME 和辅助技术通常会将其大部分应用功能连接到输入堆栈中。 云存储应用通常会使用 shell 扩展（例如，“资源管理器”中的图标和右键单击菜单添加项）；这些 shell 扩展可能会失败，如果失败没有得到妥善处理，应用本身可能根本无法工作。

- **假设所有基于 ARM 的设备都运行 Windows 移动版本的应用可能无法正常工作**。 做出这种假设的应用可能会以错误的方向显示，存在意外的 UI 布局或绘制；如果事先未测试协定可用性就直接尝试调用只适用于移动版的 API，则根本无法启动。

- **ARM 不支持 Windows 虚拟机监控程序平台**。 在 ARM 设备上使用 Hyper-V 运行任何虚拟机都将失败。

下表列出了一些常见问题，并提供了有关如何解决它们的建议。

|问题|解决方案|
|-----|--------|
| 你的应用依赖于不是为 ARM 设计的驱动程序。 | 将你的 x86 驱动程序重新编译为 ARM64 驱动程序。 请参阅[使用 WDK 构建 ARM64 驱动程序](https://docs.microsoft.com/en-us/windows-hardware/drivers/develop/building-arm64-drivers)。 |
| 你的应用只适用于 x64。 | 如果你的应用是为 Microsoft Store 开发的，请提交应用的 ARM 版本。 有关详细信息，请参阅[应用包体系结构](../packaging/device-architecture.md)。 如果你是 Win32 开发人员，请分发应用的 x86 版本。 |
| 你的应用使用了高于 1.1 版本的 OpenGL，或者需要硬件加速的 OpenGL。 | 使用 DirectX 9、DirectX 10、DirectX 11 和 DirectX 12 的 x86 应用将可在 ARM 上正常运行。 有关详细信息，请参阅 [DirectX 图形和游戏](https://msdn.microsoft.com/en-us/library/windows/desktop/ee663274(v=vs.85).aspx)。 |
| 你的 x86 应用无法按预期工作。 | 按照 [ARM 上的程序兼容性疑难解答](apps-on-arm-program-compat-troubleshooter.md)中的指南尝试使用兼容性疑难解答。 有关其他一些疑难解答步骤，请参阅[基于 ARM 的 x86 应用疑难解答](apps-on-arm-troubleshooting-x86.md)。 |
| 你的 x86 应用没有检测到它正在 ARM 上运行。 | 使用 [IsWow64Process2](https://msdn.microsoft.com/en-us/library/windows/desktop/mt804318(v=vs.85).aspx) 确定你的应用是否在 ARM 上运行。 |
| 你的 UWP ARM32 应用无法按预期工作。 | 请参阅[基于 ARM 的 ARM32 应用疑难解答](apps-on-arm-troubleshooting-arm32.md)，了解如何让你的应用在 ARM 上正常工作。 |
