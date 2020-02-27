---
title: 什么是 .NET TraceProcessing-.NET TraceProcessing
description: 在本概述中，了解 .NET TraceProcessing 是什么。
author: maiak
ms.author: maiak
ms.date: 02/23/2020
ms.topic: overview
ms.openlocfilehash: 1283a6b183673cbfb0d3d27290fc24ae6b020302
ms.sourcegitcommit: 4fdab7be28aca18cb3879fc205eb49edc4f9a96b
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/26/2020
ms.locfileid: "77629068"
---
# <a name="process-event-tracing-for-windows-etw-traces-in-net"></a>在 .NET 中处理 Windows 事件跟踪（ETW）跟踪

[Windows 事件跟踪（ETW）](https://docs.microsoft.com/windows/win32/etw/event-tracing-portal)是一个内置于 windows 操作系统的功能强大的跟踪收集系统。 Windows 深度集成了 ETW，包括有关系统行为的数据，包括针对上下文切换、内存分配、进程创建和退出等事件的内核。 ETW 中可用的系统范围数据使其适合用于端到端性能分析或其他问题，需要查看整个系统中多个组件之间的交互。

与文本日志记录不同，ETW 为自动数据处理提供了结构化事件。 Microsoft 在这些结构化事件（包括[Windows 性能分析器（WPA））](https://docs.microsoft.com/windows-hardware/test/wpt/windows-performance-analyzer)之上构建了强大的工具，该工具提供了一个图形界面，用于可视化和浏览 ETW 跟踪文件（.etl）中捕获的跟踪数据。

在 Microsoft 内部，我们经常使用 ETW 跟踪来度量 Windows 新版本的性能。 鉴于生成 Windows 工程系统的数据量，自动分析至关重要。 对于我们的自动化跟踪分析，我们经常C#使用和 .net，因此我们创建了[.net TraceProcessing API](https://www.nuget.org/packages/Microsoft.Windows.EventTracing.Processing.All)来访问许多类型的 ETW 跟踪数据。 Windows 性能分析器中还使用此技术来为它的多个表通电。

利用 .NET TraceProcessing NuGet 包，你可以使用 Microsoft 用于分析 Windows 的相同工具来分析你自己的应用程序和系统。

## <a name="next-steps"></a>后续步骤

在本概述中，已了解 .NET TraceProcessing 是什么。

下一步是[处理第一个跟踪](quickstart.md)。

## <a name="in-this-section"></a>本节内容

* [访问跟踪数据](tutorial.md)
* [扩展 TraceProcessor](extensibility.md)
* [加载符号](symbols.md)
* [使用流式处理](streaming.md)
* [示例](https://github.com/microsoft/eventtracing-processing-samples)
* [API 参考](reference.md)
