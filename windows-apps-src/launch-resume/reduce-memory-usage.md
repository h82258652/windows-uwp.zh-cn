---
author: TylerMSFT
ms.assetid: 3a3ea86e-fa47-46ee-9e2e-f59644c0d1db
description: 本文介绍了如何在将应用移动到后台时减少内存。
title: 在应用移动到后台状态时减少内存使用量
ms.author: twhitney
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 983eb3e69e170054fcc7bc87a5f2035cd0541753
ms.sourcegitcommit: 0ab8f6fac53a6811f977ddc24de039c46c9db0ad
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/15/2018
ms.locfileid: "1655489"
---
# <a name="free-memory-when-your-app-moves-to-the-background"></a>在将应用移动到后台时释放内存

本文介绍了如何在将应用移至后台状态时减少应用使用的内存量，以便它不会暂停和终止。

## <a name="new-background-events"></a>新的后台事件

Windows 10 版本 1607 引入了两个新的应用程序生命周期事件：[**EnteredBackground**](https://msdn.microsoft.com/library/windows/apps/Windows.ApplicationModel.Core.CoreApplication.EnteredBackground) 和 [**LeavingBackground**](https://msdn.microsoft.com/library/windows/apps/Windows.ApplicationModel.Core.CoreApplication.LeavingBackground)。 借助这些事件，应用将知道进入和退出后台的时间。

当应用进入后台时，由系统强制执行的内存约束可能会更改。 使用这些事件检查当前的内存占用，以便保持在限制以下，这样当应用在后台运行时，它将不会暂停和终止。

### <a name="events-for-controlling-your-apps-memory-usage"></a>控制应用内存使用量的事件

[MemoryManager.AppMemoryUsageLimitChanging](https://msdn.microsoft.com/library/windows/apps/windows.system.memorymanager.appmemoryusagelimitchanging.aspx) 仅在应用可以使用的总内存限制更改时引发。 例如，当应用进入后台，Xbox 上的内存限制从 1024MB 更改为 128MB 时。  
为防止平台暂停或终止应用，这是需要处理的最重要的事件。

当应用的内存占用增加到了 [AppMemoryUsageLevel](https://msdn.microsoft.com/library/windows/apps/windows.system.appmemoryusagelevel.aspx) 枚举中的较高值时，将引发 [MemoryManager.AppMemoryUsageIncreased](https://msdn.microsoft.com/library/windows/apps/windows.system.memorymanager.appmemoryusageincreased.aspx)。 例如，从**低**到**中**。 处理此事件是可选项，但建议这样做，因为应用程序仍负责保持在限制以下。

当应用的内存占用降低到了 **AppMemoryUsageLevel** 枚举中的较低值时，将引发 [MemoryManager.AppMemoryUsageDecreased](https://msdn.microsoft.com/library/windows/apps/windows.system.memorymanager.appmemoryusagedecreased.aspx)。 例如，从**高**到**低**。 处理此事件是可选项，但指示应用程序可以根据需要分配额外的内存。

## <a name="handle-the-transition-between-foreground-and-background"></a>处理前台和后台之间的转换

当应用从前台移至后台时，将引发 [**EnteredBackground**](https://msdn.microsoft.com/library/windows/apps/Windows.ApplicationModel.Core.CoreApplication.EnteredBackground) 事件。 当应用返回到前台时，将引发 [**LeavingBackground**](https://msdn.microsoft.com/library/windows/apps/Windows.ApplicationModel.Core.CoreApplication.LeavingBackground) 事件。 可以在创建应用时为这些事件注册处理程序。 在默认项目模板中，这将在 App.xaml.cs 的 **App** 类构造函数中完成。

由于在后台运行会减少允许应用保留的内存资源，因此还应该注册 [**AppMemoryUsageIncreased**](https://msdn.microsoft.com/library/windows/apps/Windows.System.MemoryManager.AppMemoryUsageIncreased) 和 [**AppMemoryUsageLimitChanging**](https://msdn.microsoft.com/library/windows/apps/Windows.System.MemoryManager.AppMemoryUsageLimitChanging) 事件，这两个事件可用于检查应用的当前内存使用量和当前限制。 这些事件的处理程序如以下示例中所示。 有关 UWP 应用的应用程序生命周期的详细信息，请参阅[应用生命周期](..//launch-resume/app-lifecycle.md)。

[!code-cs[RegisterEvents](./code/ReduceMemory/cs/App.xaml.cs#SnippetRegisterEvents)]

当引发 [**EnteredBackground**](https://msdn.microsoft.com/library/windows/apps/Windows.ApplicationModel.Core.CoreApplication.EnteredBackground) 事件时，请设置跟踪变量以指示当前正在后台运行。 当你编写代码来减少内存使用量时，这将非常有用。

[!code-cs[EnteredBackground](./code/ReduceMemory/cs/App.xaml.cs#SnippetEnteredBackground)]

当应用过渡到后台时，系统会降低该应用的内存限制，以确保当前的前台应用具有足够的资源来提供响应迅速的用户体验

[**AppMemoryUsageLimitChanging**](https://msdn.microsoft.com/library/windows/apps/Windows.System.MemoryManager.AppMemoryUsageLimitChanging) 事件处理程序使应用可以知道其分配的内存已减少，同时在传递给该处理程序的事件参数中提供新限制。 将 [**MemoryManager.AppMemoryUsage**](https://msdn.microsoft.com/library/windows/apps/Windows.System.MemoryManager.AppMemoryUsage) 属性（提供应用的当前使用量）与事件参数的 [**NewLimit**](https://msdn.microsoft.com/library/windows/apps/Windows.System.AppMemoryUsageLimitChangingEventArgs.NewLimit) 属性（指定新限制）比较。 如果内存使用量超过该限制，则需要减少内存使用量。

在此示例中，是在帮助程序方法 **ReduceMemoryUsage** 中编写此代码，此方法将在后文中进行定义。

[!code-cs[MemoryUsageLimitChanging](./code/ReduceMemory/cs/App.xaml.cs#SnippetMemoryUsageLimitChanging)]

> [!NOTE]
> 某些设备配置会允许应用程序在超出新内存限制的情况下继续运行，直到系统遇到资源压力，而有些设备则不允许。 尤其是在 Xbox 上，如果应用在 2 秒内未将内存使用降低至新限制以下，应用将暂停或终止。 这意味着，通过使用此事件在引发该事件的 2 秒内将资源使用量降低至限制下，就可以在大部分设备上实现最佳体验。

当应用首次过渡到后台时，尽管应用内存使用量当前可能低于后台应用的内存限制，但它的内存占用可能会随着时间的推移而增加，开始接近限制。 处理程序 [**AppMemoryUsageIncreased**](https://msdn.microsoft.com/library/windows/apps/Windows.System.MemoryManager.AppMemoryUsageIncreased) 使你有机会检查使用量增加时的当前使用量，并释放内存（如果需要）。

查看 [**AppMemoryUsageLevel**](https://msdn.microsoft.com/library/windows/apps/Windows.System.AppMemoryUsageLevel) 是否为 **High** 或 **OverLimit**，如果是，请降低内存使用量。 在此示例中，这将由帮助程序方法 **ReduceMemoryUsage** 处理。 还可以订阅 [**AppMemoryUsageDecreased**](https://msdn.microsoft.com/library/windows/apps/Windows.System.MemoryManager.AppMemoryUsageDecreased) 事件，查看应用是否低于限制，如果是，你便知道可以分配其他资源。

[!code-cs[MemoryUsageIncreased](./code/ReduceMemory/cs/App.xaml.cs#SnippetMemoryUsageIncreased)]

**ReduceMemoryUsage** 是一个帮助程序方法，当应用超过在后台运行的使用量限制时，可实现该方法来释放内存。 释放内存的方法视应用的具体情况而定，但用于释放内存的一个建议方法是释放 UI 以及与应用视图关联的其他资源。 为此，请确保正在后台状态下运行，然后将应用窗口的 [**Content**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Window.Content) 属性设置为 `null`、注销 UI 事件处理程序，并删除可能具有的对页面的任何其他引用。 如果未能注销 UI 事件处理程序和清除可能具有的对页面的任何其他引用，将阻止释放页面资源。 然后，调用 **GC.Collect** 立即回收释放的内存。 通常，不用强制执行垃圾回收，因为系统会自行处理此操作。 在此特定情况下，我们会减少用于此应用程序的内存量，因为它会进入后台，进而降低系统为回收内存而决定终止应用的可能性。

[!code-cs[UnloadViewContent](./code/ReduceMemory/cs/App.xaml.cs#SnippetUnloadViewContent)]

当窗口内容收集完成时，每个框架都开始其断开连接处理。 如果窗口内容下的可视化对象树中存在页面，这些页面将开始引发其 Unloaded 事件。 除非已删除对页面的所有引用，否则无法从内存中彻底清除它们。 在 Unloaded 回调中，执行以下操作以确保快速释放内存：
* 清除页面中任何较大的数据结构，并将它们设置为 `null`。
* 注销页面内具有回调方法的所有事件处理程序。 确保在页面的 Loaded 事件处理程序期间注册这些回调。 当 UI 已完成重建，并且页面已添加到可视化对象树时，将引发 Loaded 事件。
* 在 Unloaded 回调的末尾调用 `GC.Collect`，以便快速对刚设为 `null` 的任何大数据结构进行垃圾回收。 同样，通常不用强制进行垃圾回收，因为系统会自行处理该操作。 在此特定情况下，我们会减少用于此应用程序的内存量，因为它会进入后台，进而降低系统为回收内存而决定终止应用的可能性。

[!code-cs[MainPageUnloaded](./code/ReduceMemory/cs/App.xaml.cs#SnippetMainPageUnloaded)]

在 [**LeavingBackground**](https://msdn.microsoft.com/library/windows/apps/Windows.ApplicationModel.Core.CoreApplication.LeavingBackground) 事件处理程序中，设置跟踪变量 (`isInBackgroundMode`) 以指示应用已不在后台运行。 接下来，查看当前窗口的 [**Content**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Window.Content) 是否为 `null`，如果已释放应用视图来清除内存（在后台运行时），它将为 null。 如果窗口内容为 `null`，请重新生成应用视图。 在此示例中，使用帮助程序方法 **CreateRootFrame** 创建窗口内容。

[!code-cs[LeavingBackground](./code/ReduceMemory/cs/App.xaml.cs#SnippetLeavingBackground)]

**CreateRootFrame** 帮助程序方法用于重新创建应用的视图内容。 此方法中的代码几乎默认项目模板中提供的 [**OnLaunched**](https://msdn.microsoft.com/library/windows/apps/br242335) 处理程序代码完全相同。 一个区别是：**Launching** 处理程序确定 [**LaunchActivatedEventArgs**](https://msdn.microsoft.com/library/windows/apps/Windows.ApplicationModel.Activation.LaunchActivatedEventArgs) 的 [**PreviousExecutionState**](https://msdn.microsoft.com/library/windows/apps/Windows.ApplicationModel.Activation.LaunchActivatedEventArgs.PreviousExecutionState) 属性中的先前执行状态，而 **CreateRootFrame** 方法只是获取作为参数传入的先前执行状态。 若要最大程度地减少重复代码，可以重构默认的 **Launching** 事件处理程序代码以调用 **CreateRootFrame**。

[!code-cs[CreateRootFrame](./code/ReduceMemory/cs/App.xaml.cs#SnippetCreateRootFrame)]

## <a name="guidelines"></a>指南

### <a name="moving-from-the-foreground-to-the-background"></a>从前台移动到后台

当应用从前台移动到后台时，系统将代表应用工作，来释放在后台不需要使用的资源。 例如，UI 框架会刷新缓存的纹理，并且视频子系统会代表应用释放分配的内存。 但是，应用将仍然需要仔细监视其内存使用量，以避免被系统暂停或终止。

当应用从前台移动到后台时，它将先获取 **EnteredBackground** 事件，然后获取 **AppMemoryUsageLimitChanging** 事件。

- **使用** **EnteredBackground** 事件，以释放所知道的应用（在后台运行时）不需要的 UI 资源。 例如，可以释放某首歌曲的封面画面图像。
- **使用** **AppMemoryUsageLimitChanging** 事件，以确保应用使用比新后台限制更少的内存。 如果不是，请确保释放资源。 如果不这样做，根据设备特定的策略，应用可能会暂停或终止。
- 当 **AppMemoryUsageLimitChanging** 事件引发时，如果应用超出新的内存限制，**请**手动调用垃圾回收器。
- **使用** **AppMemoryUsageIncreased** 事件，以在应用在后台运行时继续监视应用的内存使用量（如果预计会出现变化）。 如果 **AppMemoryUsageLevel** 为 **High** 或 **OverLimit**，请确保释放资源。
- 作为一种性能优化，请**考虑**在 **AppMemoryUsageLimitChanging** 事件处理程序中释放 UI 资源，而不是在 **EnteredBackground** 处理程序中释放。 使用 **EnteredBackground/LeavingBackground** 事件处理程序中设定的布尔值，来跟踪应用是在后台还是在前台运行。 然后在 **AppMemoryUsageLimitChanging** 事件处理程序中，如果 **AppMemoryUsage** 超出限制并且应用在后台运行（基于布尔值），则可以释放 UI 资源。
- **不要**在 **EnteredBackground** 事件中执行长时间运行的操作，因为可能会导致用户感觉应用程序之间的过渡较慢。

### <a name="moving-from-the-background-to-the-foreground"></a>从后台移动到前台

当应用从后台移动到前台时，应用将先获取 **AppMemoryUsageLimitChanging** 事件，然后获取 **LeavingBackground** 事件。

- **使用** **LeavingBackground** 事件，重新创建应用在进入后台时丢弃的 UI 资源。

## <a name="related-topics"></a>相关主题

* [后台媒体播放示例](http://go.microsoft.com/fwlink/p/?LinkId=800141) - 介绍了如何在将应用移动到后台状态时释放内存。
* [诊断工具](https://blogs.msdn.microsoft.com/visualstudioalm/2015/01/16/diagnostic-tools-debugger-window-in-visual-studio-2015/) - 使用诊断工具，可以观察垃圾回收事件，并验证应用是否按预期方式释放内存。
