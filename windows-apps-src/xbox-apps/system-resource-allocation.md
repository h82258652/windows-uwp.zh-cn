---
title: Xbox One 上 UWP 应用和游戏的系统资源
description: Xbox 上的 UWP 系统资源
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.assetid: 12e87019-4315-424e-b73c-426d565deef9
ms.localizationpriority: medium
ms.openlocfilehash: f2aad1b4d6abf9a05af9b9a938188c69d9cae7c8
ms.sourcegitcommit: d7613c791107f74b6a3dc12a372d9de916c0454b
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/05/2018
ms.locfileid: "8752651"
---
# <a name="system-resources-for-uwp-apps-and-games-on-xbox-one"></a>用于 Xbox One 上 UWP 应用和游戏的系统资源

Xbox One 上运行的 UWP 应用与系统和其他应用共享资源。 Xbox One 上的 UWP 可以使用的资源取决于你提交为应用还是提交为 Xbox Live 创意者计划游戏。

* 在前台运行时的最大可用内存：
    * 应用：1 GB
    * 游戏：5 GB

后台运行的应用最多可使用 128 MB 内存。 背景模式仅适用于并发应用程序，如后台音乐播放器。  游戏会暂停，并在后台终止。

超过限制将导致内存分配故障。 有关监视内存使用情况的详细信息，请参阅 [MemoryManager 类](https://msdn.microsoft.com/library/windows/apps/windows.system.memorymanager.aspx)参考。
    
    > [!NOTE]
    > When running your app or game from the Visual Studio debugger, these memory constraints do not apply. This limit is only applicable when not running in debugging mode.

* CPU
    * 应用：共享 2-4 CPU 内核具体取决于系统上运行的应用和游戏数量。
    * 游戏：4 个独占 CPU 内核，2 个共享 CPU 内核。

* GPU
    * 应用：共享 45% 的 GPU 具体取决于系统上运行的应用和游戏数量。
    * 游戏：对可用 GPU 周期的完全访问权限。

* DirectX 支持
    * 应用：DirectX 11 功能级别 10。
    * 游戏：DirectX 12 以及 DirectX 11 功能级别 10。

* 所有应用和游戏必须面向 x64 体系结构以进行开发或提交到 Xbox 应用商店。  

对于**应用程序开发**，与标准电脑相比，可用资源可能会受到限制，并因系统上运行的应用和游戏数而有所变化。

对于**游戏开发**，Xbox One 与其他游戏主机一样，是一个专业化的硬件，它需要专门的基于硬件的开发工具来访问其全部潜能。 如果你正在开发需要最大限度发挥 Xbox One 硬件潜能的游戏，可以考虑注册 [ID@Xbox](http://www.xbox.com/Developers/id) 计划来获取 Xbox One 开发工具包的访问权限。


有关 Xbox One 上的 UWP 应用的系统资源的详细信息，请观看此视频的第一部分。
</br>
</br>
<iframe src="https://mva.microsoft.com/en-US/training-courses-embed/developing-xbox-one-applications-16860/Video-What-s-Unique--vk0fOPf9C_2006218965" width="636" height="480" allowFullScreen frameBorder="0"></iframe>

## <a name="see-also"></a>另请参阅
- [Xbox One 上的 UWP](index.md)
- [Xbox Live 创意者计划入门](../xbox-live/get-started-with-creators/get-started-with-xbox-live-creators.md)
- [DirectX 和 Xbox One 上的 UWP](https://blogs.msdn.microsoft.com/chuckw/2017/12/15/directx-and-uwp-on-xbox-one/)

