---
title: 游戏和 DirectX
description: 通用 Windows 平台 (UWP) 提供了创建、分配游戏以及通过游戏获益的新机会。 了解有关启动新游戏或移植现有游戏的信息。
ms.assetid: 4073b835-c900-4ff2-9fc5-da52f9432a1f
---

# 游戏和 DirectX


\[ 已针对 Windows 10 上的 UWP 应用更新。 有关 Windows 8.x 文章，请参阅[存档](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

通用 Windows 平台 (UWP) 提供了创建、发布游戏以及通过游戏获取收益的新机会。 了解有关启动新游戏或移植现有游戏的信息。

| 主题 | 说明 |
|---------------------------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [Windows 10 游戏开发指南](e2e.md) | 开发通用 UWP 游戏的资源和信息的端到端指南。 |
| [适用于通用 Windows 平台应用的游戏技术](game-development-platform-guide.md) | 在本指南中，你将了解有关可用于开发 UWP 游戏的技术的信息。 |
| [适用于游戏的项目模板和工具](prepare-your-dev-environment-for-windows-store-directx-game-development.md) | 介绍开始为 UWP 编写 DirectX 游戏所需的内容。 |
| [应用对象和 DirectX](about-the-metro-style-user-interface-and-directx.md) | 使用 DirectX 的 UWP 游戏不会使用许多 Windows UI 用户界面元素和对象。 相反，因为它们在 Windows 运行时堆栈中的较低级别上运行，所以它们必须以更加基本的方式与用户界面框架互操作：直接访问应用对象并与之互操作。 了解何时以及如何执行此互操作，以及作为 DirectX 开发人员，你可以如何在 UWP 应用的开发中高效使用此模型。 |
| [启动和恢复应用](launching-and-resuming-apps-directx-and-cpp.md) | 了解如何启动、暂停以及恢复 UWP DirectX 应用。 |
| [DirectX 游戏的 2D 图形](working-with-2d-graphics-in-your-directx-game.md) | 我们将讨论 2D 位图图形的使用和效果，以及如何在游戏中使用它们。 |
| [DirectX 游戏的基本 3D 图形](an-introduction-to-3d-graphics-with-directx.md) | 我们将介绍如何使用 DirectX 编程来实现 3D 图形的基本概念。 |
| [在 DirectX 游戏中加载资源](load-a-game-asset.md) | 大多数游戏在某些时间会从本地存储或其他一些数据流中加载资源（例如着色器、纹理、预先定义的网络或其他图形数据）。 下面，让我们看一看在 UWP 游戏中加载要使用的这些文件时你必须考虑的一个高级视图。 |
| [使用 DirectX 创建一款简单的 UWP 游戏](tutorial--create-your-first-metro-style-directx-game.md) | 在此教程集中，你将了解如何使用 DirectX 和 C++ 创建基本的 UWP 游戏。 我们将介绍游戏的所有主要部分，包括加载艺术和网格之类的资源，创建主游戏循环，实现简单的呈现管道以及添加声音和控件的过程。 |
| [开发 Marble Maze，一款使用 C++ 和 DirectX 的通用 Windows 平台游戏](developing-marble-maze-a-windows-store-game-in-cpp-and-directx.md) | 文档的这部分介绍如何使用 DirectX 和 Visual C++ 创建一个 3D UWP 游戏。 本文档介绍如何创建一个名为 Marble Maze 的 3D 游戏，它支持平板电脑等新外形规格，同时也适用于传统的台式机和笔记本电脑。 |
| [支持屏幕方向](supporting-screen-rotation-directx-and-cpp.md) | 下面我们将讨论在你的 UWP DirectX 应用中处理屏幕旋转的最佳做法，以便有效地使用 Windows 10 设备的图形硬件。 |
| [游戏音频](working-with-audio-in-your-directx-game.md) | 学习如何开发音乐和声音并将其融入你的 DirectX 游戏，以及如何处理音频信号以创建动态和有方位感的声音。 |
| [游戏的触摸控件](tutorial--adding-touch-controls-to-your-directx-game.md) | 学习如何将基本触摸控件添加到使用 DirectX 的 UWP C++ 游戏。 我们将介绍如何添加基于触摸的控件以便在 Direct3D 环境中移动固定平台相机，其中包括使用手指拖动或触笔改变相机视景。 |
| [游戏的移动观看控件](tutorial--adding-move-look-controls-to-your-directx-game.md) | 学习如何向你的 DirectX 游戏添加传统的鼠标和键盘移动观看控件（也称为鼠标观看控件）。 |
| [优化输入和呈现循环](optimize-performance-for-windows-store-direct3d-11-apps-with-coredispatcher.md) | 输入延迟可能会大大影响游戏体验，将其优化可使游戏更美观。 此外，适当的输入事件优化可延长电池寿命。 了解如何选择正确的 [CoreDispatcher](optimize-performance-for-windows-store-direct3d-11-apps-with-coredispatcher.md) 输入事件处理选项，以确保你的游戏尽可能流畅地处理输入。 |
| [交换链缩放和覆盖](multisampling--scaling--and-overlay-swap-chains.md) | 了解如何创建已缩放的交换链以提高在移动设备上的渲染速度，以及如何使用覆盖交换链（如果可用）来提高视觉质量。 |
| [利用 DXGI 1.3 交换链减少延迟](reduce-latency-with-dxgi-1-3-swap-chains.md) | 使用 DXGI 1.3 通过等待交换链发信号通知开始呈现新帧的正确时间来减少有效的帧延迟。 |
| [UWP 应用中的多重采样](multisampling--multi-sample-anti-aliasing--in-windows-store-apps.md) | 了解如何在使用 Direct3D 生成的 UWP 应用中使用多重采样。 |
| [在 Direct3D 11 中处理设备删除方案](handling-device-lost-scenarios.md) | 本主题介绍了图形适配器被删除或重新初始化时，如何重新创建 Direct3D 和 DXGI 的设备界面链。 |
| [游戏异步编程](asynchronous-programming-directx-and-cpp.md) | 本主题介绍在你对 DirectX 使用异步编程和线程处理时需要考虑的各种注意事项。 |
| [游戏网络](work-with-networking-in-your-directx-game.md) | 了解如何在你的 DirectX 游戏中开发和合并网络功能。 |
| [DirectX 和 XAML 互操作](directx-and-xaml-interop.md) | 你可以在 UWP 游戏中同时使用 Extensible Application Markup Language (XAML) 和 Microsoft DirectX。 |
| [打包游戏](package-your-windows-store-directx-game.md) | 较大的 UWP 游戏可能会轻易地膨胀得很大，尤其是那些支持具有特定于区域的资源的多语言游戏或具有可选的高清晰度资源的游戏。 在本主题中，了解如何使用应用包和应用程序包来自定义应用，使你的客户仅收到真正需要的资源。 |
| [游戏移植指南](porting-guides.md) | 提供将现有游戏移植到 Direct3D 11、UWP 和 Windows 10 的指南。 |
| [游戏编程资源](additional-directx-game-programming-resources.md) | 有关在 Windows 上进行游戏编程的详细信息，请查看以下资源。 |

 

> **注意**  
本文适用于编写通用 Windows 平台 (UWP) 应用的 Windows 10 开发人员。 如果你要针对 Windows 8.x 或 Windows Phone 8.x 进行开发， 请参阅[存档文档](http://go.microsoft.com/fwlink/p/?linkid=619132)。

 

若要充分利用游戏开发概述和教程，你应当熟悉以下主题：

-   Microsoft C++ 组件扩展 (C++/CX)。 这是对 Microsoft C++ 的更新，它合并了自动引用计数， 并且是使用 DirectX 11.1 或更高版本开发 UWP 游戏的语言。
-   基本图形编程术语。
-   基本的 Windows 编程概念。
-   基本熟悉 Direct3D 9 或 11 API。

 

 






<!--HONumber=Mar16_HO1-->


