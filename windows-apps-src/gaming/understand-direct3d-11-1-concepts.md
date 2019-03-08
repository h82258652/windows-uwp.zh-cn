---
title: 从 Direct3D 9 到 Direct3D 11 的重要更改
description: 本主题介绍 DirectX 9 和 DirectX 11 之间更高级别的差异。
ms.assetid: 35a9e388-b25e-2aac-0534-577b15dae364
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, 游戏, directx, direct3d 9, direct3d 11, 更改
ms.localizationpriority: medium
ms.openlocfilehash: ecdd8591efb3920d2cfe333aa8ec02c65c1a1465
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2019
ms.locfileid: "57638962"
---
# <a name="important-changes-from-direct3d-9-to-direct3d-11"></a>从 Direct3D 9 到 Direct3D 11 的重要更改



**摘要**

-   [规划您的 DirectX 端口](plan-your-directx-port.md)
-   从 Direct3D 9 到 Direct3D 11 的重要更改
-   [功能映射](feature-mapping.md)


本主题介绍 DirectX 9 和 DirectX 11 之间更高级别的差异。

从根本上说，Direct3D 11 与 Direct3D 9 是同类型的 API - 一种到图形硬件的低级别虚拟化接口。 它还允许你在各种硬件实现上执行图形绘制操作。 自 Direct3D 9 起，图形 API 的布局已更改，设备上下文的概念已经扩展，并且已添加一个 API 专用于图形基础结构。 Direct3D 设备上存储的资源具有一种新的数据多态性机制，称为资源视图。

## <a name="core-api-functions"></a>核心 API 函数


在 Direct3D 9 中，你必须先创建一个到 Direct3D API 的接口，然后才能开始使用它。 在 Direct3D 11 通用 Windows 平台 (UWP) 游戏中，可调用 名为 [**D3D11CreateDevice**](https://msdn.microsoft.com/library/windows/desktop/ff476082) 的静态函数，创建设备和设备上下文。

## <a name="devices-and-device-context"></a>设备和设备上下文


Direct3D 11 设备表示一个虚拟化的图形适配器。 它用于在视频内存中创建资源，例如：将纹理上载到 GPU、在纹理资源和交换链上创建视图，以及创建纹理采样器。 有关 Direct3D 11 设备接口用途的完整列表，请参见 [**ID3D11Device**](https://msdn.microsoft.com/library/windows/desktop/ff476379) 和 [**ID3D11Device1**](https://msdn.microsoft.com/library/windows/desktop/hh404575)。

Direct3D 11 设备上下文用于设置管道状态，并生成呈现命令。 例如，Direct3D 11 呈现链使用设备上下文来设置呈现链并绘制场景（请参见下文）。 设备上下文用于访问（映射）Direct3D 设备资源使用的视频内存，它也可用于更新子资源数据（例如常量缓冲区数据）。 有关 Direct3D 11 设备上下文用途的完整列表，请参见 [**ID3D11DeviceContext**](https://msdn.microsoft.com/library/windows/desktop/ff476385) 和 [**ID3D11DeviceContext1**](https://msdn.microsoft.com/library/windows/desktop/hh404598)。 请注意，我们的大多数示例均使用即时上下文直接呈现给设备，但 Direct3D 11 还支持递延的设备上下文，这主要用于多线程处理。

在 Direct3D 11 中，设备句柄和设备上下文句柄都通过调用 [**D3D11CreateDevice**](https://msdn.microsoft.com/library/windows/desktop/ff476082) 获取。 此方法也是你在图形适配器支持的 Direct3D 功能级别上请求一组特定硬件功能并检索信息的位置。 有关设备、设备上下文和线程处理注意事项的详细信息，请参阅 [Direct3D 11 中的设备简介](https://msdn.microsoft.com/library/windows/desktop/ff476880)。

## <a name="device-infrastructure-frame-buffers-and-render-target-views"></a>设备基础结构、帧缓存和呈现目标视图


在 Direct3D 11 中，设备适配器和硬件配置均使用 [**IDXGIAdapter**](https://msdn.microsoft.com/library/windows/desktop/bb174523) 和 [**IDXGIDevice1**](https://msdn.microsoft.com/library/windows/desktop/hh404543) COM 接口通过 DirectX 图形基础结构 (DXGI) API 进行设置。 缓冲区和其他窗口资源（可见或位于屏幕外）使用特定 DXGI 接口创建和配置，[**IDXGIFactory2**](https://msdn.microsoft.com/library/windows/desktop/hh404556) 出厂模式实现获取帧缓冲区之类的 DXGI 资源。 由于 DXGI 拥有交换链，因此 DXGI 接口用于将帧呈现到屏幕上：请参见 [**IDXGISwapChain1**](https://msdn.microsoft.com/library/windows/desktop/hh404631)。

使用 [**IDXGIFactory2**](https://msdn.microsoft.com/library/windows/desktop/hh404556) 创建与你的游戏兼容的交换链。 你需要为核心窗口或为复合（XAML 互操作）创建一个交换链，而不是为 HWND 创建一个交换链。

## <a name="device-resources-and-resource-views"></a>设备资源和资源视图


对于名为视图的视频内存资源，Direct3D 11 支持一种额外的多态性。 从本质上说，如果你有一个 Direct3D 9 对象供获取纹理，则你现在拥有两个对象：纹理资源（保留数据）和资源视图（指示该视图用于呈现的方式）。 基于资源的视图使该资源能够用于特定目的。 例如，将一个 2D 纹理资源创建为 [**ID3D11Texture2D**](https://msdn.microsoft.com/library/windows/desktop/ff476635)，然后在对其创建一个着色器资源视图 ([**ID3D11ShaderResourceView**](https://msdn.microsoft.com/library/windows/desktop/ff476628))，以便该资源在着色器中可用作纹理。 还可以在同一 2D 纹理资源上创建一个呈现目标视图 ([**ID3D11RenderTargetView**](https://msdn.microsoft.com/library/windows/desktop/ff476582))，以便该资源可用作绘图表面。 在另一个示例中，通过对一个纹理资源使用 2 个不同的视图以 2 种不同的像素格式表示相同的像素数据。

底层资源必须使用与创建所用的视图类型兼容的属性创建。 例如，如果[ **ID3D11RenderTargetView** ](https://msdn.microsoft.com/library/windows/desktop/ff476582)应用到图面上，使用创建该图面[ **D3D11\_绑定\_呈现\_目标**](https://msdn.microsoft.com/library/windows/desktop/ff476085)标志。 在图面还必须具有与呈现兼容的 DXGI 表面格式 (请参阅[ **DXGI\_格式**](https://msdn.microsoft.com/library/windows/desktop/bb173059))。

用于呈现的大部分资源都继承自[**ID3D11Resource**](https://msdn.microsoft.com/library/windows/desktop/ff476584) 接口，而此接口又继承自 [**ID3D11DeviceChild**](https://msdn.microsoft.com/library/windows/desktop/ff476380)。 顶点缓冲区、索引缓冲区、常量缓冲区以及着色器均为 Direct3D 11 资源。 输入布局和采样器状态直接继承自 [**ID3D11DeviceChild**](https://msdn.microsoft.com/library/windows/desktop/ff476380)。

资源视图使用 DXGI\_格式的枚举值，以指示的像素格式。 不是每个 D3DFMT 支持作为 DXGI\_格式。 例如，没有 24bpp RGB 格式中没有等效于 D3DFMT DXGI\_R8G8B8。 也有不为每个 RGB 格式 BGR 等效项 (DXGI\_格式\_R10G10B10A2\_UNORM 相当于 D3DFMT\_A2B10G10R10，但没有直接等效项到 D3DFMT\_A2R10G10B10)。 在生成时，你应该计划将任何采用这些传统格式的内容转换为支持的格式。 有关完整列表的 DXGI 格式，请参阅[ **DXGI\_格式**](https://msdn.microsoft.com/library/windows/desktop/bb173059)枚举。

在呈现场景之前创建 Direct3D 设备资源（和资源视图）。 设备上下文用于设置呈现链，如下所述。

## <a name="device-context-and-the-rendering-chain"></a>设备上下文和呈现链


在 Direct3D 9 和 Direct3D 10.x 中，有一个管理资源创建、状态和绘图的 Direct3D 设备对象。 在 Direct3D 11 中，该 Direct3D 设备接口仍管理资源创建，但所有状态和绘图操作均使用 Direct3D 设备上下文处理。 下面是如何使用设备上下文（[**ID3D11DeviceContext1**](https://msdn.microsoft.com/library/windows/desktop/hh404598) 接口）设置呈现链的示例：

-   设置和清除呈现目标视图（以及深度模板视图）
-   为输入装配器阶段（IA 阶段）设置顶点缓冲区、索引缓冲区和输入布局
-   将顶点和像素着色器绑定到管道
-   将常量缓冲区绑定到着色器
-   将纹理视图和采样器绑定到像素着色器
-   绘制场景

调用其中一个 [**ID3D11DeviceContext::Draw**](https://msdn.microsoft.com/library/windows/desktop/ff476407) 方法时，会在呈现目标视图上绘制场景。 当你完成所有绘图时，DXGI 适配器将用于通过调用 [**IDXGISwapChain1::Present1**](https://msdn.microsoft.com/library/windows/desktop/hh446797) 呈现完整的帧。

## <a name="state-management"></a>状态管理


Direct3D 9 通过使用 SetRenderState、SetSamplerState 和 SetTextureStageState 方法设置的大量单独切换管理状态设置。 由于 Direct3D 11 不支持传统的固定函数管道，因此 SetTextureStageState 替换为写入像素着色器 (PS)。 Direct3D 9 状态块没有等效对象。 Direct3D 11 通过使用 4 种状态对象管理状态，从而提供了一种简化的方式来对呈现状态进行分组。

例如，而不是使用 SetRenderState 与 D3DRS\_ZENABLE，则与此创建 DepthStencilState 对象和其他相关状态设置，并使用它来更改在呈现时的状态。

将 Direct3D 9 应用程序移植到状态对象时，请注意，你的各种状态组合表示为不可变的状态对象。 它们应一次性创建，并且只要有效，就应重复使用。

## <a name="direct3d-feature-levels"></a>Direct3D 功能级别


Direct3D 具有一种新机制来确定名为功能级别的硬件支持。 通过允许你请求一组明确定义的 GPU 功能，功能级别简化了指出图形适配器可执行操作的任务。 例如，9\_1 功能级别实现 Direct3D 9 图形适配器，包括着色器提供的功能模型 2.x。 由于 9\_1 为最低功能级别，有望获得所有设备都支持顶点着色器和像素着色器已由 Direct3D 9 可编程的着色器模型支持的相同阶段。

你的游戏将使用 [**D3D11CreateDevice**](https://msdn.microsoft.com/library/windows/desktop/ff476082) 来创建 Direct3D 设备和设备上下文。 调用该函数时，你将提供游戏可支持的功能级别列表。 将从该列表返回受支持的最高功能级别。 例如，如果您的游戏可以使用 BC4/BC5 纹理 （DirectX 10 硬件的功能），应包括至少 9\_1 和 10\_的受支持的功能级别的列表中为 0。 如果游戏在 DirectX 9 的硬件上运行，并且 BC4/BC5 纹理不能使用，然后[ **D3D11CreateDevice** ](https://msdn.microsoft.com/library/windows/desktop/ff476082)将返回 9\_1。 然后，你的游戏可回退到不同的纹理格式（以及更小的纹理）。

如果你决定扩展 Direct3D 9 游戏以支持更高的 Direct3D 功能级别，则最好先完成现有 Direct3D 9 图形代码的移植。 之后，你可以让游戏在 Direct3D 11 中工作，通过增强的图形可以比较容易地增加额外的呈现路径。

有关功能级别支持的详细解释，请参见 [Direct3D 功能级别](https://msdn.microsoft.com/library/windows/desktop/ff476876)。 有关 Direct3D 11 功能的完整列表， 请参阅 [Direct3D 11 功能](https://msdn.microsoft.com/library/windows/desktop/ff476342)和 [Direct3D 11.1 功能](https://msdn.microsoft.com/library/windows/desktop/hh404562)。

## <a name="feature-levels-and-the-programmable-pipeline"></a>功能级别和可编程管道


从 Direct3D 9 开始，硬件在不断发展，一些新的可选阶段已添加到可编程图形管道。 你拥有的适用于图形管道的选项集因 Direct3D 功能级别而不同。 功能级别 10.0 包括具有可选流输出的几何着色器阶段，以在 GPU 上实现多通道呈现。 功能级别 11\_0 包括外壳着色器和用于硬件分割域着色器。 功能级别 11\_0 还包括 DirectCompute 着色器的完全支持，而 10.x 仅包括的功能级别的支持有限的 DirectCompute 格式。

使用对应于 Direct3D 功能级别的着色器配置文件以 HLSL 编写所有着色器。 着色器配置文件是向上兼容，因此在编译使用 vs 的 HLSL 着色器\_4\_0\_级别\_9\_1 或 ps\_4\_0\_级别\_9\_1 将在所有设备上工作。 着色器配置文件不是下层兼容，因此使用 vs 着色器编译\_4\_1 将仅作用于功能级别为 10\_1，11\_0 或 11\_1 的设备。

Direct3D 9 使用 SetVertexShaderConstant 和 SetPixelShaderConstant 为采用共享数组的着色器管理常量。 Direct3D 11 使用常量缓冲区，这是顶点缓冲区或索引缓冲区之类的资源。 常量缓冲区旨在有效地进行更新。 无需将所有的着色器常量组织为一个全局数组，你可以将常量组织成逻辑分组，并通过一个或多个常量缓冲区进行管理。 当你将 Direct3D 9 游戏移植到 Direct3D 11 时，请计划组织常量缓冲区，以便你可以相应地更新它们。 例如，对不是将每个帧更新到一个单独常量缓冲区的着色器常量进行分组，这样你就不必经常将数据上载到图形适配器以及更加动态的着色器常量。

> **请注意**  最 Direct3D 9 应用程序广泛使用了着色器，但偶尔会混合使用旧的固定函数行为。 请注意，Direct3D 11 仅使用可编程着色模型。 Direct3D 9 的传统固定函数功能已弃用。

 

 

 




