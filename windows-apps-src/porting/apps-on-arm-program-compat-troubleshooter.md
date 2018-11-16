---
title: ARM 上的程序兼容性疑难解答
author: msatranjr
description: 介绍当你的应用无法在 ARM 上正常工作时如何调整兼容性设置
ms.author: misatran
ms.date: 02/15/2018
ms.topic: article
keywords: windows 10 s, 始终连接, 兼容性疑难解答, 基于 ARM 的 windows
ms.localizationpriority: medium
ms.openlocfilehash: 4765ad324e90167c7279c9245bccd840bce1163d
ms.sourcegitcommit: 9f8010fe67bb3372db1840de9f0be36097ed6258
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/16/2018
ms.locfileid: "7103601"
---
# <a name="program-compatibility-troubleshooter-on-arm"></a>ARM 上的程序兼容性疑难解答
模拟以支持 x86 应用是为基于 ARM64 的 Windows 10 创建的一项新功能。 有时，模拟可以执行优化，但无法提供最佳体验。 你可以使用程序兼容性疑难解答切换 x86 应用的模拟设置，从而减少默认优化并有可能提高兼容性。

## <a name="start-the-program-compatibility-troubleshooter"></a>启动程序兼容性疑难解答
以在任何 Windows 10 电脑上相同的方式手动启动[程序兼容性疑难解答](https://support.microsoft.com/en-us/help/15078/windows-make-older-programs-compatible)：右键单击可执行 (.exe) 文件，然后选择**疑难解答兼容性**。 将出现下面的屏幕。

![兼容性疑难解答选项屏幕截图](images/arm/Capture4.png)

如果单击**疑难解答程序**，你将看到以下选项。

![兼容性疑难解答选项屏幕截图](images/arm/Capture5.png)

所有选项都启用适用并可在所有 Windows 10 台式电脑上应用的设置。 此外，第一、第二和第四个选项还应用[禁用应用程序缓存](#disable-app-cache) 和[禁用混合执行模式](#disable-hybrid-exec-mode) 模拟设置。

## <a name="toggling-emulation-settings"></a>切换模拟设置
> [!WARNING]
> 更改模拟设置可能会导致应用程序意外崩溃或根本无法启动。

你可以右键单击可执行文件并选择**属性**来切换模拟设置。

在 ARM 上，**兼容性**选项卡中提供了标题为**基于 ARM 的 Windows 10** 的部分。单击 **Change emulation settings** 启动下图所示的第二个窗口。

![更改模拟设置屏幕截图](images/arm/Capture.png)

该窗口提供了两种修改模拟设置的方法。 你可以选择预定义的一组模拟设置，也可以单击 **Use advanced settings** 选项启用并选择单个设置。

分组模拟设置可降低性能优化，有利于提高质量。 以下是你可以选择的一些分组设置。

![更改模拟设置屏幕截图 2](images/arm/Capture2.png)

选择 **Use advanced settings** 后可以选择单个设置，如下表中所述。

| 模拟设置 | 结果 |
| ----------------- | ----------- |
| <p id="disable-app-cache">禁用应用程序缓存</p> | 操作系统将缓存编译后的代码块，以减少后续执行时的模拟开销。 该设置要求模拟器在运行时重新编译所有应用代码。 |
| <p id="disable-hybrid-exec-mode">禁用混合执行模式</p> | 编译后的混合可移植可执行文件 (CHPE) 二进制文件是与 x86 兼容的二进制文件，其中包含本机 ARM64 代码以提高性能，但这可能与某些应用不兼容。 该设置强制使用只包含 x86 指令的二进制文件。 |
| 严格模式自修改代码支持 | 启用该选项可确保在模拟中正确支持任何自修改代码。 默认的模拟器行为涵盖了最常见的自修改代码方案。 启用该选项会显著降低执行期间自修改代码的性能。 |
| 禁用 RWX 页面性能优化 | 该优化可提高可读、可写和可执行 (RWX) 页上的代码性能，但可能与某些应用不兼容。 |

你也可以选择多核设置，如下所示。

![多核设置屏幕截图](images/arm/Capture3.png)

这些设置将更改模拟期间应用中的内存屏障（用于在核心间同步内存访问）数量。 默认模式为 **Fast**，**strict** 和 **very strict** 选项将增加屏障数量。 这会降低应用运行速度，但可减少应用出错风险。 **单一核心**选项将移除内存屏障并强制所有应用线程在单个核心上运行。

如果更改特定设置可解决你的问题，请通过电子邮件向 *woafeedback@microsoft.com* 发送详细说明，以便我们整合你的反馈。