---
title: 扩展性-.NET TraceProcessing
description: 在本教程中，了解如何扩展 .NET TraceProcessing。
author: maiak
ms.author: maiak
ms.date: 02/23/2020
ms.topic: overview
ms.openlocfilehash: bf5f7a7c1bb007b7f1a19508fa0ee7bbaf298654
ms.sourcegitcommit: 4fdab7be28aca18cb3879fc205eb49edc4f9a96b
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/26/2020
ms.locfileid: "77629138"
---
# <a name="extend-traceprocessor"></a>扩展 TraceProcessor

许多类型的跟踪数据在[TraceProcessor](https://docs.microsoft.com/dotnet/api/microsoft.windows.eventtracing.traceprocessor)中都具有内置支持，但是，如果你有想要分析的其他提供程序（包括你自己的自定义提供程序），则在处理过程中也可以从实时跟踪数据。

例如，下面是一种在跟踪中获取提供程序 Id 列表的简单方法：

```csharp
// Open a trace with TraceProcessor.Create() and call Run...

static void Run(ITraceProcessor trace)
{
    HashSet<Guid> providerIds = new HashSet<Guid>();
    trace.Use((e) => providerIds.Add(e.ProviderId));
    trace.Process();

    foreach (Guid providerId in providerIds)
    {
        Console.WriteLine(providerId);
    }
}
```

下面的示例演示简化的自定义数据源：

```csharp
// Open a trace with TraceProcessor.Create() and call Run...

static void Run(ITraceProcessor trace)
{
    CustomDataSource customDataSource = new CustomDataSource();
    trace.Use(customDataSource);

    trace.Process();

    Console.WriteLine(customDataSource.Count);
}

class CustomDataSource : IFilteredEventConsumer
{
    public IReadOnlyList<Guid> ProviderIds { get; } = new Guid[] { new Guid("your provider ID") };

    public int Count { get; private set; }

    public void Process(EventContext eventContext)
    {
        ++Count;
    }
}
```

## <a name="next-steps"></a>后续步骤

在本教程中，已学习如何扩展 TraceProcessor。

下一步是了解如何加载用于跟踪的[符号](symbols.md)。
