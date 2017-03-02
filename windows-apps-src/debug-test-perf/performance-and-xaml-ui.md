---
author: mcleblanc
ms.assetid: 64F7FC51-E8AC-4098-9C5F-0172E4724B5C
title: "性能"
description: "用户希望他们的应用保持响应状态、感觉自然，并且不会耗尽电池。"
ms.author: markl
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
translationtype: Human Translation
ms.sourcegitcommit: c6b64cff1bbebc8ba69bc6e03d34b69f85e798fc
ms.openlocfilehash: 38a78b8af1555bdb4409c967bd27e5967b5c40aa
ms.lasthandoff: 02/07/2017

---
# <a name="performance"></a>性能

\[ 已针对 Windows 10 上的 UWP 应用更新。 有关 Windows 8.x 文章，请参阅[存档](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

用户希望他们的应用保持响应性、感觉自然，并且不会耗尽电池。 从技术上讲，性能是非功能要求，但将性能视为一项功能将有助于你满足用户的期望。 指定目标与衡量是关键因素。 确定性能关键型方案是什么；定义良好的性能意味着什么。 然后及早衡量，并在项目的整个生命周期中频繁衡量，以确保达到你的目标。 本部分将向你介绍如何组织你的性能工作流、修复动画故障和帧速率问题以及调整你的启动时间、页面导航时间和内存使用情况。

我们已证明可以显著提高性能的步骤是只需移植你的应用以面向 Windows 10 即可（如果你尚未如此操作）。 多项 XAML 优化（例如，[{x:Bind}](https://msdn.microsoft.com/library/windows/apps/Mt204783)）仅在 Windows 10 应用中可用。 请参阅[将应用移植到 Windows 10](https://msdn.microsoft.com/library/windows/apps/Mt238321) 以及 //build/ 会话[移动到通用 Windows 平台](http://channel9.msdn.com/Events/Build/2015/3-741)。

| 主题 | 描述 |
|-------|-------------|
| [规划性能](planning-and-measuring-performance.md) | 用户希望他们的应用保持响应状态、使用感觉自然，并且不会耗尽电池。 从技术上讲，性能是非功能要求，但将性能视为一项功能将有助于你满足用户的期望。 指定目标与衡量是关键因素。 确定性能关键型方案是什么；定义良好的性能意味着什么。 然后及早衡量，并在项目的整个生命周期中频繁衡量，以确保达到你的目标。 |
| [优化后台活动](optimize-background-activity.md) | 创建 UWP 应用，这些应用与系统一起以电池高效方式使用后台任务。 |
| [ListView 和 GridView UI 优化](optimize-gridview-and-listview.md) | 通过 UI 虚拟化、元素减少和项目的进度更新来改进 [<strong>GridView</strong>](https://msdn.microsoft.com/library/windows/apps/BR242705) 性能和启动时间。 |
| [ListView 和 GridView 数据虚拟化](listview-and-gridview-data-optimization.md) | 通过数据虚拟化改进 [<strong>GridView</strong>](https://msdn.microsoft.com/library/windows/apps/BR242705) 性能和启动时间。 |
| [提高垃圾回收性能](improve-garbage-collection-performance.md) | 使用 C# 和 Visual Basic 编写的通用 Windows 平台 (UWP) 应用从 .NET 垃圾回收器获取自动内存管理。 本部分汇总了 UWP 应用中的 .NET 垃圾回收器的行为和性能最佳实践。 |
| [保持 UI 线程有响应](keep-the-ui-thread-responsive.md) | 用户期望应用在执行计算时保持响应，无论计算机的类型如何都是如此。 这对于不同的应用具有不同含义。 对于某些应用，这意味着要提供更加真实的物理结构、从磁盘或 Web 更快地加载数据、快速显示复杂的场景和在页面之间导航、瞬间查找方向，或者快速处理数据。 无论计算属于何种类型，用户都希望其应用对输入做出反应，并消除在应用在&quot;思考&quot;时看起来毫无响应的情形。 |
| [优化 XAML 标记](optimize-xaml-loading.md) | 在内存中分析构造对象的 XAML 标记对于复杂的 UI 而言非常耗时。 你可以采取以下措施为你的应用提高 XAML 标记分析和加载时间效率以及内存效率。 | 
| [优化 XAML 布局](optimize-your-xaml-layout.md) | 布局可能是 XAML 应用中最耗费资源的部分，无论在 CPU 使用率还是内存开销方面。 以下是可为提高 XAML 应用的布局性能而采取的一些简单步骤。 | 
| [MVVM 和语言性能提示](mvvm-performance-tips.md) | 本主题将讨论一些与选择软件设计模式和编程语言相关的性能注意事项。 |
| [应用的启动性能的最佳做法](best-practices-for-your-app-s-startup-performance.md) | 通过用户改进处理启动和激活的方式，创建启动时间得到优化的 UWP 应用。 |
| [优化动画、媒体和图像](optimize-animations-and-media.md) | 创建具备流畅动画、高帧速率和高性能媒体捕获与播放的通用 Windows 平台 (UWP) 应用。 |
| [优化暂停/恢复](optimize-suspend-resume.md) | 创建 UWP 应用，以简化其生存期系统的使用过程，从而在暂停或终止后高效地恢复。 |
| [优化文件访问](optimize-file-access.md) | 创建可高效访问文件系统的 UWP 应用，避免因磁盘延迟和内存/CPU 周期而产生的性能问题。 |
| [Windows 运行时组件和优化的互操作](windows-runtime-components-and-optimizing-interop.md) | 创建使用 UWP 组件的 UWP 应用并在本机和托管类型之间进行互操作，同时避免互操作性能问题。 |
| [用于分析和性能的工具](tools-for-profiling-and-performance.md) | Microsoft 提供了多种工具来帮助你改进 UWP 应用的性能。|


