---
author: mtoepke
title: "图形诊断工具"
description: "了解如何在 Visual Studio 中获取和使用图形诊断功能，包括图形调试、图形框架分析和 GPU 使用率。"
ms.assetid: 629ea462-18ed-a333-07e9-cc87ea2dcd93
translationtype: Human Translation
ms.sourcegitcommit: 6530fa257ea3735453a97eb5d916524e750e62fc
ms.openlocfilehash: f266cb50893fd37162f21be169d6daf6e37c6bb9

---

# 图形诊断工具


\[ 已针对 Windows 10 上的 UWP 应用更新。 有关 Windows 8.x 的文章，请参阅[存档](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

对于 Windows 10，图形诊断工具现已作为 Windows 内的可选功能提供。 若要使用在运行时和 Visual Studio 中提供的图形诊断功能来开发 DirectX 应用或游戏，请安装可选“图形工具”功能：

1.  转到**“设置”**、依次选择**“系统”**和**“可选功能”**，然后单击**“添加功能”**。 转到“设置”****、依次选择“系统”****、“应用和功能”****和“管理可选功能”****， 然后单击“添加功能”****。
2.  在**“添加功能”**列表中，单击**“图形工具”**。

图形诊断功能包括在 DirectX 运行时中创建 Direct3D 调试设备（通过 Direct3D SDK 层）的功能以及图形调试、框架分析和 GPU 使用情况。

-   图形调试使你可以跟踪你的应用正在进行的 Direct3D 调用。 然后，你可以重播这些调用、检查参数、使用着色器进行调试和实验， 并可视化图形资源以诊断呈现问题。 可以在 Windows 电脑、模拟器或设备上获取日志，并且该日志可以在不同的硬件上播放。
-   Visual Studio 中的图形框架分析在图形调试日志上运行，并收集 Direct3D 绘图调用的基线计时。 然后它通过修改各种图形设置来执行一组实验，并生成计时结果的表格。 你可以使用此数据了解应用中的图形性能问题，并且可以查看各种实验的结果，以确定性能改善的机会。
-   Visual Studio 中的 GPU 使用情况使你可以实时监视 GPU 使用。 它收集和分析 CPU 和 GPU 正在处理的工作负载的计时数据，以便你可以确定哪里存在瓶颈。

## 相关主题


[Visual Studio 中的图形诊断概述](http://go.microsoft.com/fwlink/p/?LinkID=526382)

 

 







<!--HONumber=Jun16_HO4-->


