---
title: 加载符号-.NET TraceProcessing
description: 在本教程中，了解如何在处理跟踪时加载符号。
author: maiak
ms.author: maiak
ms.date: 02/23/2020
ms.topic: tutorial
ms.openlocfilehash: a6954538159c6fffb3185aa8b3137af26e17b32f
ms.sourcegitcommit: 4fdab7be28aca18cb3879fc205eb49edc4f9a96b
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/26/2020
ms.locfileid: "77629088"
---
# <a name="use-symbols-in-net-traceprocessing"></a>在 .NET 中使用符号 TraceProcessing

[TraceProcessor](https://docs.microsoft.com/dotnet/api/microsoft.windows.eventtracing.traceprocessor)支持加载符号并从多个数据源获取堆栈。 下面的控制台应用程序查看 CPU 示例，并输出特定函数正在运行的估计持续时间（基于跟踪的 CPU 使用率统计采样）。

```csharp
using Microsoft.Windows.EventTracing;
using Microsoft.Windows.EventTracing.Cpu;
using Microsoft.Windows.EventTracing.Symbols;
using System;
using System.Collections.Generic;

class Program
{
    static void Main(string[] args)
    {
        if (args.Length != 3)
        {
            Console.Error.WriteLine("Usage: GetCpuSampleDuration.exe <trace.etl> <imageName> <functionName>");
            return;
        }

        string tracePath = args[0];
        string imageName = args[1];
        string functionName = args[2];
        Dictionary<string, Duration> matchDurationByCommandLine = new Dictionary<string, Duration>();

        using (ITraceProcessor trace = TraceProcessor.Create(tracePath))
        {
            IPendingResult<ISymbolDataSource> pendingSymbolData = trace.UseSymbols();
            IPendingResult<ICpuSampleDataSource> pendingCpuSamplingData = trace.UseCpuSamplingData();

            trace.Process();

            ISymbolDataSource symbolData = pendingSymbolData.Result;
            ICpuSampleDataSource cpuSamplingData = pendingCpuSamplingData.Result;

            symbolData.LoadSymbolsForConsoleAsync(SymCachePath.Automatic, SymbolPath.Automatic).GetAwaiter().GetResult();

            Console.WriteLine();
            IThreadStackPattern pattern = AnalyzerThreadStackPattern.Parse($"{imageName}!{functionName}");

            foreach (ICpuSample sample in cpuSamplingData.Samples)
            {
                if (sample.Stack != null && sample.Stack.Matches(pattern))
                {
                    string commandLine = sample.Process.CommandLine;

                    if (!matchDurationByCommandLine.ContainsKey(commandLine))
                    {
                        matchDurationByCommandLine.Add(commandLine, Duration.Zero);
                    }

                    matchDurationByCommandLine[commandLine] += sample.Weight;
                }
            }

            foreach (string commandLine in matchDurationByCommandLine.Keys)
            {
                Console.WriteLine($"{commandLine}: {matchDurationByCommandLine[commandLine]}");
            }
        }
    }
}
```

运行此程序时，将生成如下所示的输出：

```shell
C:\GetCpuSampleDuration\bin\Debug\> GetCpuSampleDuration.exe C:\boot.etl user32.dll LoadImageInternal
0.0% (0 of 1165; 0 loaded)
<snip>
100.0% (1165 of 1165; 791 loaded)
wininit.exe: 15.99 ms
C:\Windows\Explorer.EXE: 5 ms
winlogon.exe: 20.15 ms
"C:\Users\AdminUAC\AppData\Local\Microsoft\OneDrive\OneDrive.exe" /background: 2.09 ms
```

（输出详细信息将根据跟踪的不同而异）。

## <a name="symbols-format"></a>符号格式

在内部，TraceProcessor 使用[SymCache](https://docs.microsoft.com/windows-hardware/test/wpt/loading-symbols#symcache-path)格式，它是存储在 PDB 中的某些数据的缓存。 加载符号时，TraceProcessor 需要指定要用于这些 SymCache 文件（SymCache 路径）的位置，并且支持选择性地指定 SymbolPath 以访问 Pdb。 当提供了 SymbolPath 时，TraceProcessor 将根据需要创建 PDB 文件之外的 SymCache 文件，并且对相同数据的后续处理可以直接使用 SymCache 文件以获得更好的性能。

## <a name="next-steps"></a>后续步骤

在本教程中，已学习了如何在处理跟踪时加载符号。

下一步是了解如何[使用流式处理](streaming.md)来访问跟踪数据，而无需缓冲内存中的所有内容。
