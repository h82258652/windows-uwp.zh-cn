---
author: TylerMSFT
ms.assetid: beac6333-655a-4bcf-9caf-bba15f715ea5
title: 线程和异步编程
description: 线程和异步编程可让你的应用在并行线程中以异步方式完成工作。
ms.author: twhitney
ms.date: 05/14/2018
ms.topic: article
keywords: windows 10, uwp, 异步, 线程, 线程
ms.localizationpriority: medium
ms.openlocfilehash: f01142695b676ebadea2f227cf5f8beb65ba6f9c
ms.sourcegitcommit: 6cc275f2151f78db40c11ace381ee2d35f0155f9
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/26/2018
ms.locfileid: "5570396"
---
# <a name="threading-and-async-programming"></a>线程处理和异步编程
线程和异步编程可让你的应用在并行线程中以异步方式完成工作。

应用可使用线程池在并行线程中异步完成工作。 线程池管理一组线程并使用队列向变得可用的线程分配工作项。 线程池与 Windows 运行时中提供的异步编程模式相似，因为它可用于完成扩展的工作而不会阻止 UI，但是线程池比异步编程模式提供更多的控制，并且你可以使用它并行完成多个工作项。 你可以使用线程池执行下列操作：

-   添加工作项，控制其优先级和取消工作项。

-   使用计时器和定期计时器安排工作项。

-   为关键工作项留出资源。

-   运行工作项以响应指定的事件和信号灯。

在管理线程方面，线程池更为有效，因为这样可以减少创建和销毁线程的开销。 这意味着它可以跨多个 CPU 核心优化线程，并且它可以在应用之间及在后台任务运行时平衡线程资源。 使用内置线程池是一种方便的方法，因为你只需将注意力集中到编写完成任务的代码上，而不是线程管理机制上。

| 主题                                                                                                          | 说明                         |
|----------------------------------------------------------------------------------------------------------------|-------------------------------------|
| [异步编程（UWP 应用）](asynchronous-programming-universal-windows-platform-apps.md)              | 本主题介绍通用 Windows 平台 (UWP) 中进行异步编程和它的表示形式在 C# 中，Microsoft Visual Basic.NET VisualC + + 组件扩展 (C + + CX)，和 JavaScript。 |
| [使用 C++/CX 进行异步编程（UWP 应用）](asynchronous-programming-in-cpp-universal-windows-platform-apps.md)| 本文介绍在 C++/CX 中，通过使用在 ppltasks.h 中的 <code>concurrency</code> 命名空间中定义的 <code>task</code> 类来使用异步方法的推荐方法。 |
| [使用线程池的最佳做法](best-practices-for-using-the-thread-pool.md)                         | 本主题介绍使用线程池的最佳实践。 |
| [使用 C# 或 Visual Basic 调用异步 API](call-asynchronous-apis-in-csharp-or-visual-basic.md)             | 通用 Windows 平台 (UWP) 包含许多异步 API，可确保应用在执行可能花费大量时间的任务时仍能保持响应。 本主题将介绍如何从采用 C# 或 Microsoft Visual Basic 的 UWP 使用异步方法。 |
| [创建定期工作项](create-a-periodic-work-item.md)                                                   | 了解如何创建定期重复的工作项。 |
| [向线程池提交工作项](submit-a-work-item-to-the-thread-pool.md)                               | 了解如何通过向线程池提交工作项，在单独的线程中完成工作。 |
| [使用计时器提交工作项](use-a-timer-to-submit-a-work-item.md)                                       | 了解如何创建在经过计时器时间后运行的工作项。 |
