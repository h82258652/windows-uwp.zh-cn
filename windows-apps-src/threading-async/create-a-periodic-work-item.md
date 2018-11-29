---
ms.assetid: 1B077801-0A58-4A34-887C-F1E85E9A37B0
title: 创建定期工作项
description: 了解如何创建定期重复的工作项。
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, 定期工作项, 线程处理, 计时器
ms.localizationpriority: medium
ms.openlocfilehash: 92142bcf084b6504e4c694ca33d2dc8532f1acca
ms.sourcegitcommit: b5c9c18e70625ab770946b8243f3465ee1013184
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/28/2018
ms.locfileid: "7981837"
---
# <a name="create-a-periodic-work-item"></a>创建定期工作项


** 重要的 API **

-   [**CreatePeriodicTimer**](https://msdn.microsoft.com/library/windows/apps/Hh967915)
-   [**ThreadPoolTimer**](https://msdn.microsoft.com/library/windows/apps/BR230587)

了解如何创建定期重复的工作项。

## <a name="create-the-periodic-work-item"></a>创建定期工作项

使用 [**CreatePeriodicTimer**](https://msdn.microsoft.com/library/windows/apps/Hh967915) 方法创建定期工作项。 提供用于完成工作的 lambda，并使用 *period* 参数指定两次提交之间的间隔。 使用 [**TimeSpan**](https://msdn.microsoft.com/library/windows/apps/BR225996) 结构指定此期限。 每次在此期限到期时将重新提交工作项，因此请确保该期限足够长，以便完成工作。

[**CreateTimer**](https://msdn.microsoft.com/library/windows/apps/windows.system.threading.threadpooltimer.createtimer.aspx) 返回一个 [**ThreadPoolTimer**](https://msdn.microsoft.com/library/windows/apps/BR230587) 对象。 存储该对象，以防需要取消计时器。

> **注意**不要指定值为零 （或小于 1 微秒的任何值） 的时间间隔。 这将导致定期计时器像单次计时器一样操作。

> **注意** [**CoreDispatcher.RunAsync**](https://msdn.microsoft.com/library/windows/apps/Hh750317)可用于访问 UI 并显示工作项的进度。

以下示例创建每 60 秒运行一次的工作项：

> [!div class="tabbedCodeSnippets"]
> ```csharp
> TimeSpan period = TimeSpan.FromSeconds(60);
>
> ThreadPoolTimer PeriodicTimer = ThreadPoolTimer.CreatePeriodicTimer((source) =>
>     {
>         //
>         // TODO: Work
>         //
>         
>         //
>         // Update the UI thread by using the UI core dispatcher.
>         //
>         Dispatcher.RunAsync(CoreDispatcherPriority.High,
>             () =>
>             {
>                 //
>                 // UI components can be accessed within this scope.
>                 //
>
>             });
>
>     }, period);
> ```
> ``` cpp
> TimeSpan period;
> period.Duration = 60 * 10000000; // 10,000,000 ticks per second
>
> ThreadPoolTimer ^ PeriodicTimer = ThreadPoolTimer::CreatePeriodicTimer(
>         ref new TimerElapsedHandler([this](ThreadPoolTimer^ source)
>         {
>             //
>             // TODO: Work
>             //
>             
>             //
>             // Update the UI thread by using the UI core dispatcher.
>             //
>             Dispatcher->RunAsync(CoreDispatcherPriority::High,
>                 ref new DispatchedHandler([this]()
>                 {
>                     //
>                     // UI components can be accessed within this scope.
>                     //
>                         
>                 }));
>
>         }), period);
> ```

## <a name="handle-cancellation-of-the-periodic-work-item-optional"></a>处理定期工作项的取消（可选）

如果需要，可以使用 [**TimerDestroyedHandler**](https://msdn.microsoft.com/library/windows/apps/Hh967926) 处理定期计时器的取消。 使用 [**CreatePeriodicTimer**](https://msdn.microsoft.com/library/windows/apps/Hh967915) 重载以提供用于处理定期工作项取消的其他 lambda。

以下示例创建每 60 秒重复一次的定期工作项，并且它还提供一个取消处理程序：

> [!div class="tabbedCodeSnippets"]
> ``` csharp
> using Windows.System.Threading;
>
>     TimeSpan period = TimeSpan.FromSeconds(60);
>
>     ThreadPoolTimer PeriodicTimer = ThreadPoolTimer.CreatePeriodicTimer((source) =>
>     {
>         //
>         // TODO: Work
>         //
>         
>         //
>         // Update the UI thread by using the UI core dispatcher.
>         //
>         Dispatcher.RunAsync(CoreDispatcherPriority.High,
>             () =>
>             {
>                 //
>                 // UI components can be accessed within this scope.
>                 //
>
>             });
>     },
>     period,
>     (source) =>
>     {
>         //
>         // TODO: Handle periodic timer cancellation.
>         //
>
>         //
>         // Update the UI thread by using the UI core dispatcher.
>         //
>         Dispatcher->RunAsync(CoreDispatcherPriority.High,
>             ()=>
>             {
>                 //
>                 // UI components can be accessed within this scope.
>                 //                 
>
>                 // Periodic timer cancelled.
>
>             }));
>     });
> ```
> ``` cpp
> using namespace Windows::System::Threading;
> using namespace Windows::UI::Core;
>
> TimeSpan period;
> period.Duration = 60 * 10000000; // 10,000,000 ticks per second
>
> ThreadPoolTimer ^ PeriodicTimer = ThreadPoolTimer::CreatePeriodicTimer(
>         ref new TimerElapsedHandler([this](ThreadPoolTimer^ source)
>         {
>             //
>             // TODO: Work
>             //
>                 
>             //
>             // Update the UI thread by using the UI core dispatcher.
>             //
>             Dispatcher->RunAsync(CoreDispatcherPriority::High,
>                 ref new DispatchedHandler([this]()
>                 {
>                     //
>                     // UI components can be accessed within this scope.
>                     //
>
>                 }));
>
>         }),
>         period,
>         ref new TimerDestroyedHandler([&](ThreadPoolTimer ^ source)
>         {
>             //
>             // TODO: Handle periodic timer cancellation.
>             //
>
>             Dispatcher->RunAsync(CoreDispatcherPriority::High,
>                 ref new DispatchedHandler([&]()
>                 {
>                     //
>                     // UI components can be accessed within this scope.
>                     //
>
>                     // Periodic timer cancelled.
>
>                 }));
>         }));
> ```

## <a name="cancel-the-timer"></a>取消计时器

如有必要，调用 [**Cancel**](https://msdn.microsoft.com/library/windows/apps/windows.system.threading.threadpooltimer.cancel.aspx) 方法停止定期工作项重复运行。 如果取消定期计时器时正在运行工作项，则允许完成该工作项。 当定期工作项的所有实例完成时，请调用 [**TimerDestroyedHandler**](https://msdn.microsoft.com/library/windows/apps/Hh967926)（如已提供）。

> [!div class="tabbedCodeSnippets"]
> ``` csharp
> PeriodicTimer.Cancel();
> ```
> ``` cpp
> PeriodicTimer->Cancel();
> ```

## <a name="remarks"></a>备注

有关一次性计时器的信息，请参阅[使用计时器提交工作项](use-a-timer-to-submit-a-work-item.md)。

## <a name="related-topics"></a>相关主题

* [向线程池提交工作项](submit-a-work-item-to-the-thread-pool.md)
* [使用线程池的最佳实践](best-practices-for-using-the-thread-pool.md)
* [使用计时器提交工作项](use-a-timer-to-submit-a-work-item.md)
 
