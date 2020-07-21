---
title: 快速入门处理 trace-.NET TraceProcessing
description: 在本快速入门教程中，了解如何访问 ETW 跟踪中的数据。
author: maiak
ms.author: maiak
ms.date: 02/20/2020
ms.topic: quickstart
ms.openlocfilehash: 162646baff9b2d08f6fc0ea4862802216cff9619
ms.sourcegitcommit: 4fdab7be28aca18cb3879fc205eb49edc4f9a96b
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/26/2020
ms.locfileid: "77629098"
---
# <a name="quickstart-process-your-first-trace"></a>快速入门：处理第一个跟踪

尝试使用 TraceProcessor 访问 Windows 事件跟踪（ETW）跟踪中的数据。 TraceProcessor 使你可以将 ETW 跟踪数据作为 .NET 对象进行访问。

在此快速入门中，你将了解如何执行以下操作：

1. 安装 TraceProcessing NuGet 包。
2. 创建 TraceProcessor。
3. 使用 TraceProcessor 可访问跟踪中包含的进程命令行。

## <a name="prerequisites"></a>先决条件

Visual Studio 2019

## <a name="install-the-traceprocessing-nuget-package"></a>安装 TraceProcessing NuGet 包

具有以下包 ID 的[NuGet](https://www.nuget.org/packages/Microsoft.Windows.EventTracing.Processing.All)中提供 .net TraceProcessing：

EventTracing。全部。

可以在控制台应用程序中使用此包来列出 ETW 跟踪（.etl 文件）中包含的进程命令行。

1. 创建新的 .NET Core 控制台应用。 在 Visual Studio 中，依次选择 "文件"、"新建"、"项目 ..."，然后选择 "控制台应用C#（.net Core）" 模板。

    输入项目名称，例如 "TraceProcessorQuickstart"，然后选择 "创建"。

2. 在解决方案资源管理器中，右键单击 "依赖关系"，然后选择 "管理 NuGet 包 ..."并切换到 "浏览" 选项卡。

3. 在搜索框中，输入 "EventTracing" 和 "搜索"。

    选择包含该名称的 NuGet 包上的 "安装"，并关闭 "NuGet" 窗口。

## <a name="create-a-traceprocessor"></a>创建 TraceProcessor

1. 将 Program.cs 更改为以下内容：

    ```csharp
    using Microsoft.Windows.EventTracing;
    using Microsoft.Windows.EventTracing.Processes;
    using System;

    class Program
    {
        static void Main(string[] args)
        {
            if (args.Length != 1)
            {
                Console.Error.WriteLine("Usage: <trace.etl>");
                return;
            }

            using (ITraceProcessor trace = TraceProcessor.Create(args[0]))
            {
                // TODO: call trace.Use...

                trace.Process();

                Console.WriteLine("TODO: Access data from the trace");
            }
        }
    }
    ```

2. 提供运行项目时要使用的跟踪名称。

    在解决方案资源管理器中，右键单击该项目，然后选择 "属性"。 切换到 "调试" 选项卡，在 "应用程序参数" 中输入跟踪（.etl 文件）的路径。

    如果还没有跟踪文件，可以使用[Windows 性能记录器](https://docs.microsoft.com/windows-hardware/test/wpt/start-a-recording)来创建一个。

3. 运行该应用程序。

    选择 "调试"、"开始执行（不调试）" 以运行你的代码。

## <a name="use-traceprocessor-to-access-process-command-lines-contained-in-the-trace"></a>使用 TraceProcessor 访问跟踪中包含的进程命令行

1. 将 Program.cs 更改为以下内容：

    ```csharp
    using Microsoft.Windows.EventTracing;
    using Microsoft.Windows.EventTracing.Processes;
    using System;

    class Program
    {
        static void Main(string[] args)
        {
            if (args.Length != 1)
            {
                Console.Error.WriteLine("Usage: <trace.etl>");
                return;
            }

            using (ITraceProcessor trace = TraceProcessor.Create(args[0]))
            {
                IPendingResult<IProcessDataSource> pendingProcessData = trace.UseProcesses();

                trace.Process();

                IProcessDataSource processData = pendingProcessData.Result;

                foreach (IProcess process in processData.Processes)
                {
                    Console.WriteLine(process.CommandLine);
                }
            }
        }
    }
    ```

2. 再次运行该应用程序。

    此时，在记录跟踪时，应会看到所有正在执行的进程的列表命令行。

## <a name="next-steps"></a>后续步骤

在本快速入门中，你创建了一个控制台应用程序（已安装 TraceProcessor），并使用它来访问 ETW 跟踪中的进程命令行。 现在，你有一个访问跟踪数据的应用程序。

进程信息只是在您的应用程序可以访问的 ETW 跟踪中存储的许多类型的数据中的一种。

下一步是[详细了解 TraceProcessor](tutorial.md)以及它可以访问的其他数据源。
