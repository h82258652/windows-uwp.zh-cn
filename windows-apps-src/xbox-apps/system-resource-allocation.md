---
title: Xbox One 上 UWP 应用和游戏的系统资源
description: Xbox 上的 UWP 系统资源
area: Xbox
---

# Xbox One 上 UWP 应用和游戏的系统资源

Xbox One 上运行的 UWP 应用和游戏与系统和其他应用共享资源。 
因此，UWP 应用和游戏将有权访问以下资源：

* 在此预览版中，最大的可用内存为 448 MB。
    * 在将来版本中，最大的可用内存将为 1 GB。
    * 当从 Visual Studio 调试器中运行你的应用程序或游戏时，不会应用这些内存限制。 此限制仅在调试模式下处于未运行状态时才适用。

* 共享 2-4 CPU 内核具体取决于系统上运行的应用和游戏数量。

* 共享 45% 的 GPU 具体取决于系统上运行的应用和游戏数量。

* Xbox One 上的 UWP 支持 DirectX 11 功能级别 10。 DirectX 12 在此情况下不受支持。 

有关**应用程序开发**，请务必记住，相对于标准电脑，可用的资源可能会受到限制。

有关**游戏开发**，请务必记住 Xbox One（就像其他游戏控制台一样） 
是一个专门的硬件，它需要专门的基于硬件的开发工具来访问其全部潜能。 
如果你在需要访问 Xbox One 硬件最大潜能的游戏上运行， 
你可以注册 [ID@Xbox](http://www.xbox.com/en-us/Developers/id) 计划，以获取 Xbox One 开发工具包的访问权限，其中包括 DirectX 12 支持。


<!--HONumber=Mar16_HO5-->


