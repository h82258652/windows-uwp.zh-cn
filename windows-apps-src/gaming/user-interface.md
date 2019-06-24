---
title: DirectX 游戏项目模板
description: 了解用于创建通用 Windows 平台 (UWP) 和 DirectX 游戏的模板。
ms.assetid: 41b6cd76-5c9a-e2b7-ef6f-bfbf6ef7331d
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, 游戏, directx, 模板
ms.localizationpriority: medium
ms.openlocfilehash: 668a41a69c2b7dab338d251d95e23e801fa85cf6
ms.sourcegitcommit: 6f32604876ed480e8238c86101366a8d106c7d4e
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/21/2019
ms.locfileid: "67321144"
---
# <a name="directx-game-project-templates"></a>DirectX 游戏项目模板



DirectX 和通用 Windows 平台 (UWP) 模板使你可以快速创建一个项目来作为游戏的起始点。

## <a name="prerequisites"></a>先决条件


要创建该项目，你需要执行以下操作：

-   [下载 Microsoft Visual Studio 2015](https://visualstudio.microsoft.com/vs/)。 Visual Studio 2015 具有图形编程，如调试工具的工具。 有关 DirectX 图形和游戏功能及工具的概述，请参阅 [用于 DirectX 游戏开发的 Visual Studio 工具](set-up-visual-studio-for-game-development.md)。

## <a name="choosing-a-template"></a>选择模板


Visual Studio 2015 包括三个 DirectX 和 UWP 模板：

-   DirectX 11 应用（通用 Windows）- DirectX 11 应用（通用 Windows）模板创建向使用 DirectX 11 的应用窗口直接呈现的 UWP 项目。
-   DirectX 12 应用（通用 Windows）- DirectX 12 应用（通用 Windows）模板创建向使用 DirectX 12 的应用窗口直接呈现的UWP 项目。
-   DirectX 11 和 XAML 应用（通用 Windows）- DirectX 11 和 XAML 应用（通用 Windows）模板创建在使用 DirectX 11 的 XAML 控件内呈现的 UWP 项目。 此模板使用 [**SwapChainPanel**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.SwapChainPanel)，因此你可以使用 XAML UI 控件。 这可以使添加用户界面元素更容易，但使用 XAML 模板可能会导致 性能降低。

所选模板取决于性能以及要使用的 技术。

## <a name="template-structure"></a>模板结构


DirectX 通用 Windows 模板包含以下文件：

-   pch.h and pch.cpp - 预编译的头支持。
-   Package.appxmanifest - 应用部署包的属性。
-   \*.pfx 的证书为应用程序。
-   外部依赖项 - 指向外部文件和项目 use.s 的链接
-   \*Main.h 和\*Main.cpp-用于管理应用程序资产、 更新应用程序状态和呈现帧的方法。
-   App.h 和 App.cpp - 应用程序的主入口点。 将应用与 Windows shell 连接并处理应用程序生命周期事件。 这些文件仅在 DirectX 11 应用（通用 Windows） 和 DirectX 12 应用（通用 Windows）模板中显示。
-   App.xaml、App.xaml.cpp 和 App.xaml.h - 应用程序的主入口点。 将应用与 Windows shell 连接并处理应用程序生命周期事件。 这些文件仅在 DirectX 11 和 XAML 应用（通用 Windows）模板中显示。
-   DirectXPage.xaml、DirectXPage.xaml.cpp 和 DirectXPage.xaml.h - 承载 DirectX SwapChainPanel 的页面。 这些文件仅在 DirectX 11 和 XAML 应用（通用 Windows）模板中显示。
-   内容
    -   Sample3DSceneRenderer.h 和 Sample3DSceneRenderer.cpp - 实例化基本呈现管道的示例呈现器。
    -   SampleFpsTextRenderer.h and SampleFpsTextRenderer.cpp - 使用 Direct2D 和 DirectWrite 在屏幕右下角显示当前 FPS 值的呈现器。 这些文件仅在 DirectX 11 应用（通用 Windows） 和 DirectX 11 和 XAML 应用（通用 Windows）模板中显示。
    -   SamplePixelShader.hlsl - 像素着色器的简单示例。
    -   SampleVertexShader.hlsl - 顶点着色器的简单示例。
    -   ShaderStructures.h - 用于向示例顶点着色器发送日期的结构。
-   通用
    -   StepTimer.h - 用于动画和模拟计时的帮助程序类。
    -   DirectXHelper.h - 杂项帮助程序函数。
    -   DeviceResources.h 和 Device Resources.cpp - 为拥有 DeviceResources 的应用程序提供一个界面，以在丢失或创建设备时收到通知。
    -   d3dx12.h - 包含 D3DX12 实用工具库。 此文件仅在 DirectX 12 应用（通用 Windows）中显示。
-   资源 - 由应用程序使用的徽标和初始屏幕图像。

## <a name="next-steps"></a>后续步骤


现在你拥有了一个起点，增强了构建游戏开发的知识和 Microsoft Store 游戏的开发技能。

如果你要移植现有游戏，请参阅下列主题。

-   [从 OpenGL ES 2.0 移植到 Direct3D 11.1](port-from-opengl-es-2-0-to-directx-11-1.md)
-   [Directx 9 移植到通用 Windows 平台](porting-your-directx-9-game-to-windows-store.md)

如果你要创建新的 DirectX 游戏，请参阅下列主题。

-   [使用 DirectX 创建简单的 UWP 游戏](tutorial--create-your-first-uwp-directx-game.md)
-   [开发中通用 Windows 平台游戏 Marble MazeC++和 DirectX](developing-marble-maze-a-windows-store-game-in-cpp-and-directx.md)