---
author: Mtoepke
title: "Xbox One 上 UWP 应用和游戏的系统资源"
description: "Xbox 上的 UWP 系统资源"
area: Xbox
ms.sourcegitcommit: 6a34b0f657fc787eaa3be691b69a591cfdb2a669
ms.openlocfilehash: 79c47bbcf33b1493a8a961b800932ce6be021453

---

# Xbox One 上 UWP 应用和游戏的系统资源

Xbox One 上运行的 UWP 应用和游戏与系统和其他应用共享资源。 因此，UWP 应用和游戏将有权访问以下资源：

* 在此预览版中，前台运行的应用的最大可用内存为 1 GB。
    * 后台运行的应用的最大可用内存为 128 MB。
    * 超过这些内存要求的应用将遇到内存分配失败。 有关监视内存使用的详细信息，请参阅 [MemoryManager 类](https://msdn.microsoft.com/en-us/library/windows/apps/windows.system.memorymanager.aspx)参考。
    * **注意** &nbsp;&nbsp;当从 Visual Studio 调试器中运行你的应用程序或游戏时，不会应用这些内存限制。 此限制仅在调试模式下处于未运行状态时才适用。

* 共享 2-4 CPU 内核具体取决于系统上运行的应用和游戏数量。

* 共享 45% 的 GPU 具体取决于系统上运行的应用和游戏数量。

* Xbox One 上的 UWP 支持 DirectX 11 功能级别 10。 DirectX 12 在此情况下不受支持。 

对于**应用程序开发**，请务必记住，相对于标准电脑，可用的资源可能会受到限制。

对于**游戏开发**，请务必记住，Xbox One 与其他游戏主机一样，是一个专业化的硬件，它需要专门的基于硬件的开发工具来访问其全部潜能。 如果你正在开发需要最大限度发挥 Xbox One 硬件潜能的游戏，你可以注册 [ID@Xbox](http://www.xbox.com/en-us/Developers/id) 计划来获取 Xbox One 开发工具包的访问权限（包括 DirectX 12 支持）。



<!--HONumber=Jun16_HO4-->


