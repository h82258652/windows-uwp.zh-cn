---
title: 图形诊断工具
description: 了解如何在 Visual Studio 中获取和使用图形诊断功能，包括图形调试、图形框架分析和 GPU 使用率。
ms.assetid: 629ea462-18ed-a333-07e9-cc87ea2dcd93
ms.date: 11/20/2019
ms.topic: article
keywords: windows 10, uwp, 游戏, 图形, 诊断, 工具, directx
ms.localizationpriority: medium
ms.openlocfilehash: 14711a356f00ac027bf990554c90547017b9cc4d
ms.sourcegitcommit: f464e5157ca22cfe31075f2f1219b8227587bf03
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/21/2019
ms.locfileid: "74263100"
---
# <a name="graphics-diagnostics-tools"></a>图形诊断工具

图形诊断工具可从 Windows 10 中获得，这是一项可选功能。 若要使用图形诊断功能（在运行时和 Visual Studio 中提供）来开发 DirectX 应用或游戏，请安装可选的图形工具功能。

1. 请参阅 "**设置**" > **应用**" > **应用 & 功能/可选功能**"。
2. 如果 "**已安装的功能**" 下列出了**图形工具**，则已完成。 否则，单击 "**添加功能**"。
3. 搜索和/或选择 "**图形工具**"，然后单击 "**安装**"。

图形诊断功能包括在 DirectX 运行时中创建 Direct3D 调试设备（通过 Direct3D SDK 层）的功能以及“图形调试”、“框架分析”和“GPU 使用情况”。

-   图形调试使你可以跟踪你的应用正在进行的 Direct3D 调用。 然后，你可以重播这些调用、检查参数、使用着色器进行调试和实验， 并可视化图形资源以诊断呈现问题。 你可以在 Windows 电脑、模拟器或设备上拍摄日志，然后在不同的硬件上播放它们。
-   Visual Studio 中的图形帧分析在图形调试日志上运行，并收集 Direct3D 绘图调用的基线计时。 然后，它通过修改各种图形设置来执行一组试验，并生成计时结果表。 你可以使用此数据了解应用中的图形性能问题，并且可以查看各种实验的结果，以确定性能改善的机会。
-   Visual Studio 中的 GPU 使用情况使你可以实时监视 GPU 使用。 它收集和分析 CPU 和 GPU 正在处理的工作负荷的计时数据，以便确定任何瓶颈的位置。

## <a name="related-topics"></a>相关主题

[Visual Studio 中的图形诊断概述](/visualstudio/debugger/overview-of-visual-studio-graphics-diagnostics?view=vs-2015)