---
title: 使用流式处理-.NET TraceProcessing
description: 在本教程中，了解如何使用流立即访问跟踪数据并使用较少的内存。
author: maiak
ms.author: maiak
ms.date: 02/23/2020
ms.topic: tutorial
ms.openlocfilehash: e04f306a6a5c03d1f502b9cfb6c2cbb737e0098f
ms.sourcegitcommit: 4fdab7be28aca18cb3879fc205eb49edc4f9a96b
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/26/2020
ms.locfileid: "77629076"
---
# <a name="use-streaming-with-traceprocessor"></a>通过 TraceProcessor 使用流式处理

默认情况下，TraceProcessor 通过在处理跟踪时将数据加载到内存中来访问数据。 这种缓冲方法很容易使用，但内存使用量可能会占用大量资源。

TraceProcessor 还提供了 trace。UseStreaming （），它支持以流式处理方式访问多种类型的跟踪数据（在从跟踪文件中读取数据时处理数据，而不是在内存中缓冲数据）。 例如，syscall 跟踪可能会很大，并且在跟踪中缓冲整个 syscall 列表可能非常昂贵。

## <a name="accessing-buffered-data"></a>访问缓冲数据

下面的代码演示如何通过跟踪以正常的缓冲方式访问 syscall 数据。UseSyscalls():

```csharp
using Microsoft.Windows.EventTracing;
using Microsoft.Windows.EventTracing.Processes;
using Microsoft.Windows.EventTracing.Syscalls;
using System;
using System.Collections.Generic;

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
            IPendingResult<ISyscallDataSource> pendingSyscallData = trace.UseSyscalls();

            trace.Process();

            ISyscallDataSource syscallData = pendingSyscallData.Result;

            Dictionary<IProcess, int> syscallsPerCommandLine = new Dictionary<IProcess, int>();

            foreach (ISyscall syscall in syscallData.Syscalls)
            {
                IProcess process = syscall.Thread?.Process;

                if (process == null)
                {
                    continue;
                }

                if (!syscallsPerCommandLine.ContainsKey(process))
                {
                    syscallsPerCommandLine.Add(process, 0);
                }

                ++syscallsPerCommandLine[process];
            }

            Console.WriteLine("Process Command Line: Syscalls Count");

            foreach (IProcess process in syscallsPerCommandLine.Keys)
            {
                Console.WriteLine($"{process.CommandLine}: {syscallsPerCommandLine[process]}");
            }
        }
    }
}
```

## <a name="accessing-streaming-data"></a>访问流数据

使用大的 syscall 跟踪，尝试在内存中缓冲 syscall 数据可能会非常昂贵，甚至可能不可能。 下面的代码演示如何以流式处理方式访问相同的 syscall 数据，并替换 trace。带有 trace 的 UseSyscalls （）。UseStreaming().UseSyscalls():

```csharp
using Microsoft.Windows.EventTracing;
using Microsoft.Windows.EventTracing.Processes;
using Microsoft.Windows.EventTracing.Syscalls;
using System;
using System.Collections.Generic;

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
            IPendingResult<IThreadDataSource> pendingThreadData = trace.UseThreads();

            Dictionary<IProcess, int> syscallsPerCommandLine = new Dictionary<IProcess, int>();

            trace.UseStreaming().UseSyscalls(ConsumerSchedule.SecondPass, context =>
            {
                Syscall syscall = context.Data;
                IProcess process = syscall.GetThread(pendingThreadData.Result)?.Process;

                if (process == null)
                {
                    return;
                }

                if (!syscallsPerCommandLine.ContainsKey(process))
                {
                    syscallsPerCommandLine.Add(process, 0);
                }

                ++syscallsPerCommandLine[process];
            });

            trace.Process();

            Console.WriteLine("Process Command Line: Syscalls Count");

            foreach (IProcess process in syscallsPerCommandLine.Keys)
            {
                Console.WriteLine($"{process.CommandLine}: {syscallsPerCommandLine[process]}");
            }
        }
    }
}
```

## <a name="how-streaming-works"></a>流式处理的工作原理

默认情况下，在第一次遍历跟踪期间提供所有流式处理数据，而来自其他源的缓冲数据则不可用。 上面的示例演示了如何将流式处理与缓冲组合在一起，并在流式传输 syscall 数据之前缓冲线程数据。 因此，跟踪必须读取两次–一次用于获取缓冲线程数据，第二次使用缓冲线程数据访问流式处理 syscall 数据。 为了以这种方式合并流式处理和缓冲，此示例将 SecondPass 传递到 ConsumerSchedule。UseStreaming().UseSyscalls （），这会导致在第二次遍历跟踪时发生 syscall 处理。 通过在第二个传递中运行，syscall 回调可以从 trace 访问挂起的结果。处理每个 syscall 时的 UseThreads （）。 如果没有此可选参数，则 syscall 流式处理会在第一次遍历跟踪（只有一个传递）和跟踪的挂起结果时运行。UseThreads （）尚不可用。 在这种情况下，回调仍可以从 syscall 访问 ThreadId，但它不能访问线程的进程（因为处理链接数据的线程是通过可能尚未处理的其他事件提供的）。

缓冲与流式处理之间的一些主要差异：

1. 缓冲将返回[IPendingResult&lt;t&gt;](https://docs.microsoft.com/dotnet/api/microsoft.windows.eventtracing.ipendingresult-1)，并仅在处理跟踪之前提供其保留的结果。 处理跟踪后，可以使用 foreach 和 LINQ 等技术来枚举结果。
2. 流式处理返回 void，而改用回调参数。 每个项都可用时，它会调用回调一次。 由于未对数据进行缓冲处理，因此不会出现要使用 foreach 或 LINQ 枚举的结果列表–流式处理回调需要缓冲要保存的任何数据部分，以便在处理完成后使用。
3. 用于处理缓冲数据的代码会在调用 trace 后出现。进程（）（如果可用）。
4. 用于处理流式处理数据的代码将出现在对跟踪的调用之前。进程（），作为对跟踪的回调。UseStreaming .。。（）方法。
5. 流式处理使用者可以选择仅处理部分流，并通过调用上下文来取消将来的回调。取消（）。 缓冲使用者始终提供一个完整的缓冲列表。

## <a name="correlated-streaming-data"></a>相关流式处理数据

有时跟踪数据会出现在一系列事件中-例如，syscall 是通过单独的 enter 和 exit 事件记录的，但这两个事件的组合数据可能更有用。 方法跟踪。UseStreaming().UseSyscalls （）将这两个事件中的数据关联起来，并在该对变为可用时将其提供。 可以通过跟踪使用几种类型的相关数据。UseStreaming():

| 代码                                        | 说明                                                                                                                                     |
|---------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------|
| 轨迹.UseStreaming().UseContextSwitchData() | 流相关的上下文切换数据（从 compact 和非压缩事件开始，与原始非压缩事件相比，SwitchInThreadIds）。 |
| 轨迹.UseStreaming().UseScheduledTasks()    | 流相关计划任务数据。                                                                                                         |
| 轨迹.UseStreaming().UseSyscalls()          | 流相关系统调用数据。                                                                                                            |
| 轨迹.UseStreaming().UseWindowInFocus()     | 流相关窗口-焦点数据。                                                                                                        |

## <a name="standalone-streaming-events"></a>独立流式处理事件

此外，跟踪。UseStreaming （）为多个不同的独立事件类型提供分析的事件：

| 代码                                               | 说明                                     |
|----------------------------------------------------|-------------------------------------------------|
| 轨迹.UseStreaming().UseLastBranchRecordEvents()   | 流分析了最后一个分支记录（LBR）事件。 |
| 轨迹.UseStreaming().UseReadyThreadEvents()        | 流分析的就绪线程事件。             |
| 轨迹.UseStreaming().UseThreadCreateEvents()       | 流分析的线程创建事件。            |
| 轨迹.UseStreaming().UseThreadExitEvents()         | 流分析的线程退出事件。              |
| 轨迹.UseStreaming().UseThreadRundownStartEvents() | 流分析的线程断开开始事件。     |
| 轨迹.UseStreaming().UseThreadRundownStopEvents()  | 流分析的线程断开停止事件。      |
| 轨迹.UseStreaming().UseThreadSetNameEvents()      | 流分析的线程集名称事件。          |

## <a name="underlying-streaming-events-for-correlated-data"></a>相关数据的基础流式处理事件

最后跟踪。UseStreaming （）还提供了用于关联以上列表中的数据的基础事件。 这些基础事件包括：

| 代码                                                        | 说明                                                                                | 包含在其中                                 |
|-------------------------------------------------------------|--------------------------------------------------------------------------------------------|---------------------------------------------|
| 轨迹.UseStreaming().UseCompactContextSwitchEvents()        | 流分析了精简上下文切换事件。                                              | 轨迹.UseStreaming().UseContextSwitchData() |
| 轨迹.UseStreaming().UseContextSwitchEvents()               | 流分析的上下文切换事件。 在某些情况下，SwitchInThreadIds 可能不准确。 | 轨迹.UseStreaming().UseContextSwitchData() |
| 轨迹.UseStreaming().UseFocusChangeEvents()                 | 流分析的窗口焦点更改事件。                                                 | 轨迹.UseStreaming().UseWindowInFocus()     |
| 轨迹.UseStreaming().UseScheduledTaskStartEvents()          | 流已分析计划的任务启动事件。                                                | 轨迹.UseStreaming().UseScheduledTasks()    |
| 轨迹.UseStreaming().UseScheduledTaskStopEvents()           | 流已分析计划的任务停止事件。                                                 | 轨迹.UseStreaming().UseScheduledTasks()    |
| 轨迹.UseStreaming().UseScheduledTaskTriggerEvents()        | 流已分析计划任务触发器事件。                                              | 轨迹.UseStreaming().UseScheduledTasks()    |
| 轨迹.UseStreaming().UseSessionLayerSetActiveWindowEvents() | 流分析的会话层集活动窗口事件。                                     | 轨迹.UseStreaming().UseWindowInFocus()     |
| 轨迹.UseStreaming().UseSyscallEnterEvents()                | 流分析的 syscall 输入事件。                                                       | 轨迹.UseStreaming().UseSyscalls()          |
| 轨迹.UseStreaming().UseSyscallExitEvents()                 | 流分析了 syscall 出口事件。                                                        | 轨迹.UseStreaming().UseSyscalls()          |

## <a name="next-steps"></a>后续步骤

在本教程中，您学习了如何使用流式传输立即访问跟踪数据并使用较少的内存。

下一步是从跟踪查看所需数据。 查看一些[示例](https://github.com/microsoft/eventtracing-processing-samples)。 请注意，并非所有跟踪都包含所有支持的数据类型。
