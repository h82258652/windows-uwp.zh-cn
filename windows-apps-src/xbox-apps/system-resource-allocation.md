---
author: Mtoepke
title: "Xbox One 上 UWP 应用和游戏的系统资源"
description: "Xbox 上的 UWP 系统资源"
ms.author: mtoepke
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.assetid: 12e87019-4315-424e-b73c-426d565deef9
ms.openlocfilehash: 81a437807332ff60a1401c00abdcff78e41a7496
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="system-resources-for-uwp-apps-and-games-on-xbox-one"></a>用于 Xbox One 上 UWP 应用和游戏的系统资源

Xbox One 上运行的 UWP 应用和游戏与系统和其他应用共享资源。 因此，UWP 应用和游戏将有权访问以下资源：

* 前台运行的应用最多可使用 1 GB 内存。
    * 后台运行的应用最多可使用 128 MB 内存。
    * 超过这些内存要求的应用将遇到内存分配失败。 有关监视内存使用情况的详细信息，请参阅 [MemoryManager 类](https://msdn.microsoft.com/library/windows/apps/windows.system.memorymanager.aspx)参考。
    
    > [!NOTE]
    > 当从 Visual Studio 调试器中运行你的应用程序或游戏时，不会应用这些内存限制。 此限制仅在调试模式下处于未运行状态时才适用。

* 共享 2-4 CPU 内核具体取决于系统上运行的应用和游戏数量。

* 共享 45% 的 GPU 具体取决于系统上运行的应用和游戏数量。

* Xbox One 上的 UWP 支持 DirectX 11 功能级别 10。 DirectX 12 目前不受支持。

* 所有应用必须面向 x64 体系结构以进行开发或提交到 Xbox 应用商店。  

对于**应用程序开发**，请务必记住，相对于标准电脑，可用的资源可能会受到限制。

对于**游戏开发**，请务必记住，Xbox One 与其他游戏主机一样，是一个专业化的硬件，它需要专门的基于硬件的开发工具来访问其全部潜能。 如果你正在开发需要最大限度发挥 Xbox One 硬件潜能的游戏，你可以注册 [ID@Xbox](http://www.xbox.com/Developers/id) 计划来获取 Xbox One 开发工具包的访问权限（包括 DirectX 12 支持）。

## <a name="see-also"></a>另请参阅
- [Xbox One 上的 UWP](index.md)
