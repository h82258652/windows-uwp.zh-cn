---
author: mtoepke
title: "DirectX 11 移植常见问题"
description: "有关将游戏移植到通用 Windows 平台 (UWP) 的常见问题的解答。"
ms.assetid: 79c3b4c0-86eb-5019-97bb-5feee5667a2d
ms.author: mtoepke
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "windows 10, uwp, 游戏, directx 11"
translationtype: Human Translation
ms.sourcegitcommit: c6b64cff1bbebc8ba69bc6e03d34b69f85e798fc
ms.openlocfilehash: 7dda21925e31785e0ce7c3dfc72ba173b8686743
ms.lasthandoff: 02/07/2017

---

# <a name="directx-11-porting-faq"></a>DirectX 11 移植常见问题


\[ 已针对 Windows 10 上的 UWP 应用更新。 有关 Windows 8.x 文章，请参阅[存档](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


有关将游戏移植到通用 Windows 平台 (UWP) 的常见问题的解答。

## <a name="is-porting-my-game-going-to-be-a-set-of-search-and-replace-operations-on-api-methods-or-do-i-need-to-plan-for-a-more-thoughtful-porting-process"></a>移植游戏是否涉及对 API 方法进行的一系列搜索替换操作，或者是否需要规划更周全的移植过程？


Direct3D 11 是对 Direct3D 9 的重大升级。 其中包含了多种范式转变，包括为虚拟化图形适配器及其上下文以及设备资源多态性的新层提供单独的 API。 你的游戏仍然可以采用本质上相同的方式使用图形硬件，但是你需要了解这个新的 Direct3D 11 API 体系结构，并且需要更新你的图形代码的每个部分以使用正确的 API 组件。 请参阅[移植概念和注意事项](porting-considerations.md)。

## <a name="what-is-the-new-device-context-for-am-i-supposed-to-replace-my-direct3d-9-device-with-the-direct3d-11-device-the-device-context-or-both"></a>新的设备上下文的作用是什么？ 我应该将我的 Direct3D 9 设备替换为 Direct3D 11 设备、设备上下文，还是替换为这两者？


现在，Direct3D 设备用于在视频内存中创建资源，而设备上下文用于设置管道状态以及生成呈现命令。 有关详细信息，请参阅 [Direct3D 9 之后的版本中最重要的更改是什么？](understand-direct3d-11-1-concepts.md)

##  <a name="do-i-have-to-update-my-game-timer-for-uwp"></a>必须针对 UWP 更新游戏计时器吗？


[**QueryPerformanceCounter**](https://msdn.microsoft.com/library/windows/desktop/ms644904) 以及 [**QueryPerformanceFrequency**](https://msdn.microsoft.com/library/windows/desktop/ms644905) 仍然是为 UWP 应用实现游戏计时器的最佳方法。

你应该注意计时器和 UWP 应用生命周期的细微差别。 暂停/恢复与玩家重新启动桌面游戏有所不同，因为你的游戏将 从该游戏的上次播放时间点及时恢复一个快照。 如果经过了很长时间（例如，几周），那么某些游戏计时器实现可能无法正常工作。 当游戏恢复时，你可以使用应用生命周期事件来重置你的计时器。

仍然使用 RDTSC 指令的游戏需要进行升级。 请参阅[游戏计时和多核处理器](https://msdn.microsoft.com/library/windows/desktop/ee417693)。

## <a name="my-game-code-is-based-on-d3dx-and-dxut-is-there-anything-available-that-can-help-me-migrate-my-code"></a>我的游戏代码基于 D3DX 和 DXUT。 是否有可以帮助我迁移代码的内容？


[DirectX 工具包 (DirectXTK)](http://go.microsoft.com/fwlink/p/?LinkID=248929) 社区项目提供用于 Direct3D 11 的帮助程序类。

##  <a name="how-do-i-maintain-code-paths-for-the-desktop-and-the-windows-store"></a>如何保留桌面和 Windows 应用商店的代码路径？


Chuck Walbourn 编写的标题为[游戏编码技术的双重用途](http://go.microsoft.com/fwlink/p/?LinkID=286210)的文章系列提供有关在桌面和 Windows 应用商店代码路径之间共享代码的指南。

##  <a name="how-do-i-load-image-resources-in-my-directx-uwp-app"></a>如何在 DirectX UWP应用中加载图像资源？


有两个可用于加载图像的 API 路径：

-   内容管道将图像转换为用作 Direct3D 纹理资源的 DDS 文件。 请参阅[在游戏或应用中使用 3D 资源](https://msdn.microsoft.com/library/windows/apps/hh972446.aspx)。
-   [Windows 图像处理组件](https://msdn.microsoft.com/library/windows/desktop/ee719902)可用于加载各种格式的图像，并且可用于 Direct2D 位图以及 Direct3D 纹理资源。

还可以使用 [DirectXTK](http://go.microsoft.com/fwlink/p/?LinkID=248929) 或 [DirectXTex](http://go.microsoft.com/fwlink/p/?LinkID=248926) 中的 DDSTextureLoader 和 WICTextureLoader。

## <a name="where-is-the-directx-sdk"></a>DirectX SDK 在哪里？


DirectX SDK 作为 Windows SDK 的一部分包含在内。 与 Windows SDK 分离的最新的 DirectX SDK 已于 2010 年 6 月发布。 Direct3D 示例以及其余的 Windows 应用示例都位于“代码库”中。

## <a name="what-about-directx-redistributables"></a>DirectX 可再分发内容如何？


Windows SDK 中的大多数组件都已经包含在支持的操作系统版本中，或者没有 DLL 组件（如 DirectXMath）。 你的游戏现在可以使用 UWP 应用可以使用的所有 Direct3D API 组件，所以无需重新分发它们。

Win32 桌面应用程序仍然使用 DirectSetup，因此如果你还要升级游戏的桌面版本， 请参阅[适用于游戏开发人员的 Direct3D 11 部署](https://msdn.microsoft.com/library/windows/desktop/ee416644)。

## <a name="is-there-any-way-i-can-update-my-desktop-code-to-directx-11-before-moving-away-from-effects"></a>离开“效果”之前，有什么方法可以将我的桌面代码更新到 DirectX 11？


请参阅 [Direct3D 11 更新的效果](http://go.microsoft.com/fwlink/p/?LinkId=271568)。 Effects 11 有助于消除对传统 DirectX SDK 标头的依赖性，它的用途是帮助移植，并且只能用于桌面应用。

##  <a name="is-there-a-path-for-porting-my-directx-8-game-to-uwp"></a>是否存在将 DirectX 8 游戏移植到 UWP 的路径？


存在：

-   请阅读[转换到 Direct3D 9](https://msdn.microsoft.com/library/windows/desktop/bb204851)。
-   确保游戏没有固定管道残存 - 请参阅[已弃用的功能](https://msdn.microsoft.com/library/windows/desktop/cc308047)。
-   然后，获取 DirectX 9 移植路径：[从 D3D 9 移植到 UWP](walkthrough--simple-port-from-direct3d-9-to-11-1.md)。

##  <a name="can-i-port-my-directx-10-or-11-game-to-uwp"></a>是否可以将 DirectX 10 或 DirectX 11 游戏移植到 UWP？


DirectX 10.x 和 DirectX 11 桌面游戏可轻松移植到 UWP。 请参阅 [迁移到 Direct3D 11](https://msdn.microsoft.com/library/windows/desktop/ff476190)。

## <a name="how-do-i-choose-the-right-display-device-in-a-multi-monitor-system"></a>如何在多监视器系统中选择正确的显示设备？


用户可以选择用于显示应用的监视器。 通过调用 [**D3D11CreateDevice**](https://msdn.microsoft.com/library/windows/desktop/ff476082)（将第一个参数设置为 **nullptr**）让 Windows 提供正确的适配器。 然后，获取该设备的 [**IDXGIDevice interface**](https://msdn.microsoft.com/library/windows/desktop/bb174527)，调用 [**GetAdapter**](https://msdn.microsoft.com/library/windows/desktop/bb174531) 并使用 DXGI 适配器来创建交换链。

## <a name="how-do-i-turn-on-antialiasing"></a>如何打开抗锯齿？


创建 Direct3D 设备时会启用抗锯齿（多重采样）。 通过调用 [**CheckMultisampleQualityLevels**](https://msdn.microsoft.com/library/windows/desktop/ff476499) 来枚举多重采样支持，然后在调用 [**CreateSurface**](https://msdn.microsoft.com/library/windows/desktop/bb174530) 时在 [**DXGI\_SAMPLE\_DESC structure**](https://msdn.microsoft.com/library/windows/desktop/bb173072) 中设置多重采样选项。

## <a name="my-game-renders-using-multithreading-andor-deferred-rendering-what-do-i-need-to-know-for-direct3d-11"></a>我的游戏使用多线程和/或延迟呈现来进行呈现。 对于 Direct3D 11，我需要了解哪些内容？


请访问 [Direct3D 11 中的多线程简介](https://msdn.microsoft.com/library/windows/desktop/ff476891)来开始操作。 有关主要差别的列表，请参阅 [Direct3D 版本之间的线程差别](https://msdn.microsoft.com/library/windows/desktop/ff476890)。 请注意，延迟呈现使用设备的*延迟上下文*而不是*即时上下文*。

## <a name="where-can-i-read-more-about-the-programmable-pipeline-since-direct3d-9"></a>我在哪里可以了解有关 Direct3D 9 之后的可编程管道的详细信息？


请访问以下主题：

-   [HLSL 编程指南](https://msdn.microsoft.com/library/windows/desktop/bb509635)
-   [Direct3D 10 常见问题](https://msdn.microsoft.com/library/windows/desktop/ee416643)

## <a name="what-should-i-use-instead-of-the-x-file-format-for-my-models"></a>我应该对我的模型使用哪种文件格式来替代 .x 文件格式？


尽管我们还没有正式替换 .x 文件格式，但是很多示例都采用了 SDKMesh 格式。 Visual Studio 还有一个[内容管道](https://msdn.microsoft.com/library/windows/apps/hh972446.aspx)，该管道将多种流行的格式编译成可以使用 Visual Studio 3D 初学者工具包中的代码加载或可以使用 [DirectXTK](http://go.microsoft.com/fwlink/p/?LinkID=248929) 加载的 CMO 文件。

## <a name="how-do-i-debug-my-shaders"></a>如何调试着色器？


Microsoft Visual Studio 2015 包含针对 DirectX 图形的诊断工具。 请参阅[调试 DirectX 图形](https://msdn.microsoft.com/library/windows/apps/hh315751.aspx)。

##  <a name="what-is-the-direct3d-11-equivalent-for-x-function"></a>*x* 函数的 Direct3D 11 等同项是什么？


请参阅“将 DirectX 9 功能映射到 DirectX 11 API”中提供的[函数映射](feature-mapping.md#function-mapping)。

##  <a name="what-is-the-dxgiformat-equivalent-of-y-surface-format"></a>*y* 图面格式的 DXGI\_FORMAT 等同项是什么？


请参阅“将 DirectX 9 功能映射到 DirectX 11 API”中提供的[图面格式映射](feature-mapping.md#surface-format-mapping)。

 

 





