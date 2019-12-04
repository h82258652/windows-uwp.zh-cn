---
description: 用于调试和测试应用如何与进程周期管理配合使用的工具和技术。
title: 测试和调试进程生存期管理
ms.date: 04/08/2019
ms.topic: article
keywords: windows 10, uwp
ms.assetid: 8ac6d127-3475-4512-896d-80d1e1d66ccd
ms.localizationpriority: medium
ms.openlocfilehash: 6912d7faa3a86dade13b60eac5654aef8a52173d
ms.sourcegitcommit: ae9c1646398bb5a4a888437628eca09ae06e6076
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/03/2019
ms.locfileid: "74735012"
---
# <a name="testing-and-debugging-tools-for-process-lifetime-management-plm"></a>进程周期管理 (PLM) 的测试和调试工具

UWP 应用与传统桌面应用程序的关键差异之一是 UWP 标题位于受进程周期管理 (PLM) 管制的应用容器中。 运行时中转服务可以在所有平台上暂停、恢复或终止 UWP 应用，在你测试或调试处理它们的代码时，可以使用某些专用工具来强制执行这些转换。

## <a name="features-in-visual-studio-2015"></a>Visual Studio 2015 中的功能

Visual Studio 2015 中的内置调试器可帮助你调查使用 UWP 独占功能时的潜在问题。 你可以使用**生命周期事件**工具栏将应用程序强制转换为不同的 PLM 状态，该工具栏会在你运行和调试标题时变为可见。

![生命周期事件工具栏](images/gs-debug-uwp-apps-001.png)

## <a name="the-plmdebug-tool"></a>PLMDebug 工具

PLMDebug.exe 是一种命令行工具，可允许你控制应用程序包的 PLM 状态，该工具作为 Windows SDK 的一部分提供。 安装后，该工具默认位于 *C:\Program Files (x86)\Windows Kits\10\Debuggers\x64* 中。

PLMDebug 还可允许你为任何安装的应用包禁用 PLM，这对于某些调试器来说是必需的。 禁用 PLM 可防止运行时中转服务在你有机会进行调试前终止应用。 若要禁用 PLM，请使用 **/enableDebug** 开关，后跟 UWP 应用的*完整程序包名称*（短名称、程序包系列名称或程序包的 AUMID 将不起作用）：

```cmd
plmdebug /enableDebug [PackageFullName]
```

从 Visual Studio 部署 UWP 应用后，完整程序包名称将显示在输出窗口中。 或者，你还可以通过在 PowerShell 控制台中运行 **Get-AppxPackage** 来检索完整程序包名称。

![运行 Get-AppxPackage](images/gs-debug-uwp-apps-003.png)

你可以指定将在应用包激活时自动启动的调试器的绝对路径。 如果你希望使用 Visual Studio 执行此操作，你将需要将 VSJITDebugger.exe 指定为调试器。 但是，VSJITDebugger.exe 需要你指定“-p”开关，以及 UWP 应用的进程 ID (PID)。 由于不可能事先知道你的 UWP 应用的 PID，此方案无法开箱即用。

可通过编写标识游戏进程的脚本或工具来绕过此限制，然后 shell 将运行 VSJITDebugger.exe，从而传入 UWP 应用的 PID。 以下 C# 代码示例演示完成此操作的简单方法。

```cs
using System.Diagnostics;

namespace VSJITLauncher
{
    class Program
    {
        static void Main(string[] args)
        {
            // Name of UWP process, which can be retrieved via Task Manager.
            Process[] processes = Process.GetProcessesByName(args[0]);

            // Get PID of most recent instance
            // Note the highest PID is arbitrary. Windows may recycle or wrap the PID at any time.
            int highestId = 0;
            foreach (Process detectedProcess in processes)
            {
                if (detectedProcess.Id > highestId)
                    highestId = detectedProcess.Id;
            }

            // Launch VSJITDebugger.exe, which resides in C:\Windows\System32
            ProcessStartInfo startInfo = new ProcessStartInfo("vsjitdebugger.exe", "-p " + highestId);
            startInfo.UseShellExecute = true;

            Process process = new Process();
            process.StartInfo = startInfo;
            process.Start();
        }
    }
}
```

将此方法与 PLMDebug 结合使用的示例用法：

```cmd
plmdebug /enableDebug 279f7062-ce35-40e8-a69f-cc22c08e0bb8_1.0.0.0_x86__c6sq6kwgxxfcg "\"C:\VSJITLauncher.exe\" Game"
```

其中 `Game` 是进程名称，`279f7062-ce35-40e8-a69f-cc22c08e0bb8_1.0.0.0_x86__c6sq6kwgxxfcg` 是示例 UWP 应用包的完整程序包名称。

请注意，对 **/enableDebug** 的每次调用之后必须使用 **/disableDebug** 开关耦合到另一个 PLMDebug 调用。 此外，调试器的路径必须是绝对路径（不支持相对路径）。

## <a name="related-topics"></a>相关主题

- [部署和调试 UWP 应用](deploying-and-debugging-uwp-apps.md)
- [调试、测试和性能](index.md)
