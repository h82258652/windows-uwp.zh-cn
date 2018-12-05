---
ms.assetid: AAE467F9-B3C7-4366-99A2-8A880E5692BE
title: 使用计时器提交工作项
description: 了解如何创建在经过计时器时间后运行的工作项。
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, 计时器, 线程
ms.localizationpriority: medium
ms.openlocfilehash: 2537bad82fc4a17b964f5871ab6ae1434c417f66
ms.sourcegitcommit: d7613c791107f74b6a3dc12a372d9de916c0454b
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/05/2018
ms.locfileid: "8741003"
---
# <a name="use-a-timer-to-submit-a-work-item"></a>使用计时器提交工作项


** 重要的 API **

-   [**Windows.UI.Core 命名空间**](https://msdn.microsoft.com/library/windows/apps/BR208383)
-   [**Windows.System.Threading 命名空间**](https://msdn.microsoft.com/library/windows/apps/BR229642)

了解如何创建在经过计时器时间后运行的工作项。

## <a name="create-a-single-shot-timer"></a>创建单次计时器

使用 [**CreateTimer**](https://msdn.microsoft.com/library/windows/apps/Hh967921) 方法为工作项创建计时器。 提供用于完成工作的 lambda，并使用 *delay* 参数指定线程池在可将工作项分配给可用线程之前等待的时间。 使用 [**TimeSpan**](https://msdn.microsoft.com/library/windows/apps/BR225996) 结构指定延迟。

> **注意** [**CoreDispatcher.RunAsync**](https://msdn.microsoft.com/library/windows/apps/Hh750317)可用于访问 UI 并显示工作项的进度。

以下示例创建三分钟后运行的工作项：

> [!div class="tabbedCodeSnippets"]
> ``` csharp
> TimeSpan delay = TimeSpan.FromMinutes(3);
>             
> ThreadPoolTimer DelayTimer = ThreadPoolTimer.CreateTimer(
>     (source) =>
>     {
>         //
>         // TODO: Work
>         //
>         
>         //
>         // Update the UI thread by using the UI core dispatcher.
>         //
>         Dispatcher.RunAsync(
>             CoreDispatcherPriority.High,
>             () =>
>             {
>                 //
>                 // UI components can be accessed within this scope.
>                 //
>
>             });
>
>     }, delay);
> ```
> ``` cpp
> TimeSpan delay;
> delay.Duration = 3 * 60 * 10000000; // 10,000,000 ticks per second
>
> ThreadPoolTimer ^ DelayTimer = ThreadPoolTimer::CreateTimer(
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
>                     ExampleUIUpdateMethod("Timer completed.");
>
>                 }));
>
>         }), delay);
> ```

## <a name="provide-a-completion-handler"></a>提供完成处理程序

如果需要，使用 [**TimerDestroyedHandler**](https://msdn.microsoft.com/library/windows/apps/Hh967926) 处理工作项的取消和完成。 使用 [**CreateTimer**](https://msdn.microsoft.com/library/windows/apps/Hh967921) 重载以提供其他 lambda。 它在计时器被取消或工作项完成时运行。

以下示例创建提交工作项的计时器，并在工作项完成或计时器被取消时调用方法：

> [!div class="tabbedCodeSnippets"]
> ``` csharp
> TimeSpan delay = TimeSpan.FromMinutes(3);
>             
> bool completed = false;
>
> ThreadPoolTimer DelayTimer = ThreadPoolTimer.CreateTimer(
>     (source) =>
>     {
>         //
>         // TODO: Work
>         //
>
>         //
>         // Update the UI thread by using the UI core dispatcher.
>         //
>         Dispatcher.RunAsync(
>                 CoreDispatcherPriority.High,
>                 () =>
>                 {
>                     //
>                     // UI components can be accessed within this scope.
>                     //
>
>                 });
>
>         completed = true;
>     },
>     delay,
>     (source) =>
>     {
>         //
>         // TODO: Handle work cancellation/completion.
>         //
>
>
>         //
>         // Update the UI thread by using the UI core dispatcher.
>         //
>         Dispatcher.RunAsync(
>             CoreDispatcherPriority.High,
>             () =>
>             {
>                 //
>                 // UI components can be accessed within this scope.
>                 //
>
>                 if (completed)
>                 {
>                     // Timer completed.
>                 }
>                 else
>                 {
>                     // Timer cancelled.
>                 }
>
>             });
>     });
> ```
> ``` cpp
> TimeSpan delay;
> delay.Duration = 3 * 60 * 10000000; // 10,000,000 ticks per second
>
> completed = false;
>
> ThreadPoolTimer ^ DelayTimer = ThreadPoolTimer::CreateTimer(
>         ref new TimerElapsedHandler([&](ThreadPoolTimer ^ source)
>         {
>             //
>             // TODO: Work
>             //
>
>             //
>             // Update the UI thread by using the UI core dispatcher.
>             //
>             Dispatcher->RunAsync(CoreDispatcherPriority::High,
>                 ref new DispatchedHandler([&]()
>                 {
>                     //
>                     // UI components can be accessed within this scope.
>                     //
>
>                 }));
>
>             completed = true;
>
>         }),
>         delay,
>         ref new TimerDestroyedHandler([&](ThreadPoolTimer ^ source)
>         {
>             //
>             // TODO: Handle work cancellation/completion.
>             //
>
>             Dispatcher->RunAsync(CoreDispatcherPriority::High,
>                 ref new DispatchedHandler([&]()
>                 {
>                     //
>                     // Update the UI thread by using the UI core dispatcher.
>                     //
>
>                     if (completed)
>                     {
>                         // Timer completed.
>                     }
>                     else
>                     {
>                         // Timer cancelled.
>                     }
>
>                 }));
>         }));
> ```

## <a name="cancel-the-timer"></a>取消计时器

如果计时器仍在倒计时，但是已不再需要工作项，调用 [**Cancel**](https://msdn.microsoft.com/library/windows/apps/BR230588)。 计时器会取消，而工作项不会提交到线程池。

> [!div class="tabbedCodeSnippets"]
> ``` csharp
> DelayTimer.Cancel();
> ```
> ``` cpp
> DelayTimer->Cancel();
> ```

## <a name="remarks"></a>备注

通用 Windows 平台 (UWP) 应用无法使用 **Thread.Sleep**，因为它会阻止 UI 线程。 你可以改为使用 [**ThreadPoolTimer**](https://msdn.microsoft.com/library/windows/apps/BR230587) 创建工作项，这将延迟工作项完成的任务，但不会阻止 UI 线程。

如需演示工作项、计时器工作项和定期工作项的完整代码示例，请参阅[线程池示例](http://go.microsoft.com/fwlink/p/?linkid=255387)。 此代码示例最初为 Windows8.1 编写，但该代码可在 windows 10 中重复使用。

有关重复计时器的信息，请参阅[创建定期工作项](create-a-periodic-work-item.md)。

## <a name="related-topics"></a>相关主题

* [向线程池提交工作项](submit-a-work-item-to-the-thread-pool.md)
* [使用线程池的最佳实践](best-practices-for-using-the-thread-pool.md)
* [使用计时器提交工作项](use-a-timer-to-submit-a-work-item.md)
 

 
