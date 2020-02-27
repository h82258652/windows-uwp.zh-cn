---
title: 访问跟踪数据-.NET TraceProcessing
description: 在本教程中，了解如何使用 TraceProcessor 访问跟踪数据。
author: maiak
ms.author: maiak
ms.date: 02/23/2020
ms.topic: tutorial
ms.openlocfilehash: 170a8c3084e180714a319d67dca2b6a5756ea474
ms.sourcegitcommit: 4fdab7be28aca18cb3879fc205eb49edc4f9a96b
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/26/2020
ms.locfileid: "77629107"
---
# <a name="access-trace-data"></a>访问跟踪数据

具有以下包 ID 的[NuGet](https://www.nuget.org/packages/Microsoft.Windows.EventTracing.Processing.All)中提供 .net TraceProcessing：

EventTracing。全部。

此包允许您访问跟踪文件中的数据。 如果还没有跟踪文件，可以使用[Windows 性能记录器](https://docs.microsoft.com/windows-hardware/test/wpt/start-a-recording)来创建一个。

下面的示例控制台应用程序演示如何访问跟踪中包含的所有进程的命令行：

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

## <a name="using-traceprocessor"></a>使用 TraceProcessor

若要处理跟踪，请调用[TraceProcessor](https://docs.microsoft.com/dotnet/api/microsoft.windows.eventtracing.traceprocessor.create)。 核心接口为[ITraceProcessor](https://docs.microsoft.com/dotnet/api/microsoft.windows.eventtracing.itraceprocessor)，使用此接口涉及以下模式：

1. 首先，告诉处理器要从跟踪使用哪些数据
2. 其次，处理跟踪;与
3. 最后，访问结果。

告诉处理器前面需要的数据类型，这意味着无需花费时间处理大量可能的跟踪数据。 相反， [TraceProcessor](https://docs.microsoft.com/dotnet/api/microsoft.windows.eventtracing.traceprocessor)只是提供您请求的特定类型的数据所需的工作。

## <a name="recommended-project-settings"></a>推荐的项目设置

建议将以下几个项目设置用于 TraceProcessor：

1. 建议以64位运行 exe。

    默认情况下，新C#的 .NET Framework 控制台应用程序的 Visual Studio 默认值为 "已检查32位的任何 CPU"。 .NET Core 的默认值可能已具有建议的设置。

    跟踪处理可能会占用大量内存，尤其是对于较大的跟踪，我们建议在使用 TraceProcessor 的 exe 中将平台目标更改为 x64 （或取消选中首选32位）。 若要更改这些设置，请参阅项目属性下的 "生成" 选项卡。 若要更改所有配置的这些设置，请确保将 "配置" 下拉列表设置为 "所有配置"，而不是默认的当前配置。

2. 建议将 NuGet 用于更新样式的 PackageReference 模式，而不是使用较旧的包. 配置模式。

    若要更改新项目的默认值，请参阅工具、NuGet 包管理器、包管理器设置、包管理、默认包管理格式。

## <a name="built-in-data-sources"></a>内置数据源

.Etl 文件可以在跟踪中捕获多种类型的数据。 请注意，在 .etl 文件中的数据取决于捕获跟踪时启用的提供程序。 以下列表显示了可从 TraceProcessor 获取的跟踪数据类型：

| 代码                                      | 说明                                                                                                                | 相关的 WPA 项                                                    |
|-------------------------------------------|----------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------|
| 轨迹.UseClassicEvents()                  | 从跟踪提供经典 ETW 事件，该跟踪不包括架构信息。                                         | 一般事件表（事件类型为经典或 WPP 时）             |
| 轨迹.UseConnectedStandbyData()           | 提供有关系统进入和退出连接待机的跟踪数据。                                        | CS 摘要表                                                     |
| 轨迹.UseCpuIdleStates()                  | 提供有关 CPU C 状态的跟踪数据。                                                                             | CPU 空闲状态表（当类型为实际值时）                          |
| 轨迹.UseCpuSamplingData()                | 根据指令指针的定期采样，提供有关 CPU 使用率的跟踪数据。                          | CPU 使用情况（采样）表                                            |
| 轨迹.UseCpuSchedulingData()              | 提供有关 CPU 线程计划的跟踪的数据，包括上下文切换和就绪线程事件。                | CPU 使用情况（精确）表                                            |
| 轨迹.UseDevicePowerData()                | 提供有关设备 D 状态的跟踪数据。                                                                          | 设备 DState 表                                                  |
| 轨迹.UseDirectXData()                    | 提供有关 DirectX 活动的跟踪数据。                                                                         | GPU 利用率表                                                |
| traceUseDiskIOData()                      | 提供有关磁盘 i/o 活动的跟踪数据。                                                                        | 磁盘使用情况表                                                     |
| 轨迹.UseEnergyEstimationData()           | 提供有关能源估算引擎中的每个进程能源使用量估算的跟踪数据。                         | 能源估算引擎摘要（按进程）表                  |
| 轨迹.UseEnergyMeterData()                | 提供有关从能源计量接口（EMI）测量能源使用情况的跟踪数据。                                  | 能源估算引擎（通过 Emi）表                              |
| 轨迹.UseFileIOData()                     | 提供有关文件 i/o 活动的跟踪数据。                                                                        | 文件 i/o 表                                                       |
| 轨迹.UseGenericEvents()                  | 提供跟踪中的 TraceLogging 事件。                                                                  | 一般事件表（事件类型为 "事件类型" 或 "TraceLogging" 时） |
| 轨迹.UseHandles()                        | 提供有关活动内核句柄的跟踪的部分数据。                                                            | 处理表                                                        |
| 轨迹.UseHardFaults()                     | 提供有关硬页面错误的跟踪数据。                                                                         | 硬故障表                                                    |
| 轨迹.UseHeapSnapshots()                  | 提供有关进程堆使用情况的跟踪数据。                                                                       | 堆快照表                                                  |
| 轨迹.UseHypercalls()                     | 提供有关在跟踪期间发生的 Hyper-v 虚拟化的数据。                                                        |                                                                      |
| 轨迹.UseImageSections()                  | 提供有关图像部分的跟踪数据。                                                                 | "CPU 使用率（采样）" 表的 "节名称" 列                 |
| 轨迹.UseInterruptHandlingData()          | 提供有关中断服务例程（ISR）和延迟过程调用（DPC）活动的跟踪数据。               | DPC/ISR 表                                                        |
| 轨迹.UseMarks()                          | 提供跟踪中的标记（标记的时间戳）。                                                                      | 标记表                                                          |
| 轨迹.UseMemoryUtilizationData()          | 提供有关整个系统内存使用率的跟踪数据。                                                          | 内存使用率表                                             |
| 轨迹.UseMetadata()                       | 提供可用的跟踪元数据，无需进一步处理。                                                              | 系统配置、跟踪和常规                             |
| 轨迹.UsePlatformIdleStates()             | 提供有关系统的目标和实际平台空闲状态的跟踪数据。                                   | 平台空闲状态表                                            |
| 轨迹.UsePoolAllocations()                | 提供有关内核池内存使用情况的跟踪数据。                                                                 | 池摘要表                                                   |
| 轨迹.UsePowerConfigurationData()         | 提供有关系统电源配置的跟踪数据。                                                               | 系统配置，电源设置                                 |
| 轨迹.UsePowerDependencyCoordinatorData() | 提供有关活动电源依赖关系协调器阶段的跟踪数据。                                               | 通知阶段摘要表                                     |
| 轨迹.UseProcesses()                      | 提供有关在跟踪期间处于活动状态的进程以及它们的映像和 Pdb 的数据。                                      | 进程表;Images 表;符号中心                           |
| 轨迹.UseProcessorCounters()              | 提供有关处理器计数器监视器（PCM）中处理器性能计数器值的跟踪数据。                |                                                                      |
| 轨迹.UseProcessorFrequencyData()         | 提供有关处理器运行频率的跟踪数据。                                                    | 处理器频率表（当类型为实际值时）                      |
| 轨迹.UseProcessorProfileData()           | 提供有关活动处理器电源配置文件的跟踪数据。                                                       | 处理器配置文件表                                             |
| 轨迹.UseProcessorParkingData()           | 提供有关已暂停或已离开哪些处理器的跟踪数据。                                                 | 处理器停车状态表                                        |
| 轨迹.UseProcessorParkingLimits()         | 提供有关已允许的最大已离开处理器数的跟踪数据。                                        | 核心停车帽状态表                                         |
| 轨迹.UseProcessorQualityOfServiceData()  | 提供有关每个处理器的服务级别质量的跟踪数据。                                          | 处理器 Qos 类表                                            |
| 轨迹.UseProcessorThrottlingData()        | 提供有关处理器最大频率限制的跟踪数据。                                                   | 处理器约束表                                          |
| 轨迹.UseReadyBootData()                  | 从 "就绪启动" 中的 "启动预提取" 活动的跟踪中提供数据。                                                | 就绪启动事件表                                              |
| 轨迹.UseReferenceSetData()               | 提供有关每个进程使用的虚拟内存页的跟踪数据。                                             | 引用集表                                                  |
| 轨迹.UseRegionsOfInterest()              | 提供在 xml 配置文件中指定的跟踪的感兴趣间隔区域。                       | 感兴趣的区域                                            |
| 轨迹.UseRegistryData()                   | 提供有关跟踪期间注册表活动的数据。                                                                      | 注册表表                                                       |
| 轨迹.UseResidentSetData()                | 为驻留在物理内存中的每个进程提供有关虚拟内存页的跟踪数据。       | 常驻设置表                                                   |
| 轨迹.UseRundownData()                    | 提供有关在发生跟踪断开数据收集期间的时间间隔的跟踪数据。                            | 图形时间线中的阴影区域                                 |
| 轨迹.UseScheduledTasks()                 | 提供有关在跟踪期间运行的计划任务的数据。                                                               | 计划的任务表                                                |
| 轨迹.UseServices()                       | 提供有关在跟踪期间处于活动状态或已捕获其状态的服务的数据。                                  | Services 表;系统配置，服务                       |
| 轨迹.UseStacks()                         | 提供有关在跟踪期间记录的堆栈的数据。                                                                        |                                                                      |
| 轨迹.UseStackEvents()                    | 提供有关在跟踪过程中记录的与堆栈相关的事件的数据。                                                 | 堆栈表                                                         |
| 轨迹.UseStackTags()                      | 提供将堆栈从跟踪分组为 XML 配置文件中指定的堆栈标记的映射器。               | 列（如堆栈标记和堆栈）                     |
| 轨迹.UseSymbols()                        | 提供加载跟踪符号的功能。                                                                          | 配置符号路径;加载符号                                 |
| 轨迹.UseSyscalls()                       | 提供跟踪过程中发生的 syscall 的相关数据。                                                                 | Syscall 表                                                       |
| 轨迹.UseSystemMetadata()                 | 从跟踪提供一般的系统范围的元数据。                                                                       | 系统配置                                                 |
| 轨迹.UseSystemPowerSourceData()          | 提供有关活动系统电源（AC vs DC）的跟踪数据。                                                | 系统电源表                                            |
| 轨迹.UseSystemSleepData()                | 提供有关整体系统电源状态的跟踪数据。                                                               | Power 转换表                                               |
| 轨迹.UseTargetCpuIdleStates()            | 提供有关目标 CPU C 状态的跟踪数据。                                                                      | CPU 空闲状态表（当类型为目标时）                          |
| 轨迹.UseTargetProcessorFrequencyData()   | 提供有关目标处理器频率的跟踪数据。                                                             | 处理器频率表（当类型为 Target 时）                      |
| 轨迹.UseThreads()                        | 提供有关在跟踪期间活动的线程的数据。                                                                         | 线程生存期表                                               |
| 轨迹.UseTraceStatistics()                | 提供有关跟踪中的事件的统计信息。                                                                           | 系统配置，跟踪统计信息                               |
| 轨迹.UseUtcData()                        | 使用通用遥测客户端（UTC）从有关 Microsoft 遥测活动的跟踪中提供数据。                      | Utc 表                                                            |
| 轨迹.UseWindowInFocus()                  | 提供有关焦点中活动 UI 窗口的更改的跟踪数据。                                                 | 焦点表中的窗口                                                |
| 轨迹.UseWindowsTracePreprocessorEvents() | 提供跟踪中的 Windows 软件跟踪预处理器（WPP）事件。                                                    | WPP 跟踪表;一般事件表（事件类型为 WPP 时）       |
| 轨迹.UseWinINetData()                    | 通过 Windows Internet （WinINet）从有关 internet 活动的跟踪中提供数据。                                         | 下载详细信息表                                               |
| 轨迹.UseWorkingSetData()                 | 提供有关每个进程或内核类别的工作集中的虚拟内存页的跟踪数据。 | 虚拟内存快照表                                       |

另请参阅[ITraceSource](https://docs.microsoft.com/dotnet/api/microsoft.windows.eventtracing.itracesource)上的扩展方法以获取所有可用的跟踪数据，或检查 "跟踪" 中的可用方法。 由 IntelliSense 显示。

## <a name="next-steps"></a>后续步骤

在本概述中，你学习了如何使用 TraceProcessor 以及它可以访问的内置数据源访问跟踪数据。

下一步是了解如何[扩展](extensibility.md)TraceProcessor 以访问自定义跟踪数据。
