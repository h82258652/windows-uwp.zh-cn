---
author: TylerMSFT
ms.assetid: 95CF7F3D-9E3A-40AC-A083-D8A375272181
title: "使用线程池的最佳做法"
description: "本主题介绍了使用线程池的最佳做法。"
ms.author: twhitney
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "windows 10, uwp, 线程, 线程池"
translationtype: Human Translation
ms.sourcegitcommit: c6b64cff1bbebc8ba69bc6e03d34b69f85e798fc
ms.openlocfilehash: d3b45da6a11bab926812682c209207bbbb436bf1
ms.lasthandoff: 02/07/2017

---
# <a name="best-practices-for-using-the-thread-pool"></a>使用线程池的最佳做法

\[ 已针对 Windows 10 上的 UWP 应用进行更新。 有关 Windows 8.x 文章，请参阅[存档](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


本主题介绍了使用线程池的最佳做法。

## <a name="dos"></a>应做事项


-   使用线程池在应用中执行并行工作。

-   使用工作项实现扩展任务，而不阻止 UI 线程。

-   创建生存时间较短的独立工作项。 工作项异步运行，可以从队列中以任何顺序将它们提交到池中。

-   使用 [**Windows.UI.Core.CoreDispatcher**](https://msdn.microsoft.com/library/windows/apps/BR208211) 调度对 UI 线程的更新。

-   使用 [**ThreadPoolTimer.CreateTimer**](https://msdn.microsoft.com/library/windows/apps/Hh967921) 而不是 **Sleep** 函数。

-   使用线程池，而不是创建自己的线程管理系统。 线程池运行在具有高级功能的操作系统级别，并且优化为根据进程和整个系统内的设备资源和活动来动态缩放。

-   在 C++ 中，确保工作项代理使用敏捷线程模型（默认情况下，C++ 代理是敏捷的）。

-   如果无法忍受资源分配在使用时失败，请使用预分配的工作项。

## <a name="donts"></a>禁止事项


-   不要创建 *period* 值为&lt;1 毫秒（包括 0）的定期计时器。 这样将使工作项像单次计时器一样操作。

-   不要提交需要花费比 *period* 参数指定的时间量更长的时间才能完成的定期工作项。

-   不要尝试从后台任务调度的工作项发送 UI 更新（非 Toast 和通知）。 相反，使用后台任务进度和完成处理程序（例如 [**IBackgroundTaskInstance.Progress**](https://msdn.microsoft.com/library/windows/apps/BR224800)）。

-   当使用的工作项处理程序使用 **async** 关键字时，请注意，在执行处理程序中的所有代码之前，线程池工作项可能会设置为完成状态。 在工作项已设置为完成状态后，可能会执行处理程序中 **await** 关键字之后的代码。

-   不要在未重新初始化的情况下尝试运行预分配的工作项多次。 [创建定期工作项](create-a-periodic-work-item.md)

## <a name="related-topics"></a>相关主题


* [创建定期工作项](create-a-periodic-work-item.md)
* [向线程池提交工作项](submit-a-work-item-to-the-thread-pool.md)
* [使用计时器提交工作项](use-a-timer-to-submit-a-work-item.md)

