---
title: 将 DirectX 9 功能映射到 DirectX 11 API
description: 了解 Direct3D 9 游戏使用的功能如何转换到 Direct3D 11 和通用 Windows 平台 (UWP)。
ms.assetid: 3aa8a114-4e47-ae0a-9447-88ba324377b8
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, 游戏, directx 9, directx 11, 移植
ms.localizationpriority: medium
ms.openlocfilehash: c5c86ad836c94d990f5728ce9f7cfe49c754ce19
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/20/2019
ms.locfileid: "74259527"
---
# <a name="map-directx-9-features-to-directx-11-apis"></a>将 DirectX 9 功能映射到 DirectX 11 API



**摘要**

-   [规划 DirectX 端口](plan-your-directx-port.md)
-   [从 Direct3D 9 到 Direct3D 11 的重要更改](understand-direct3d-11-1-concepts.md)
-   功能映射


了解 Direct3D 9 游戏使用的功能如何转换到 Direct3D 11 和通用 Windows 平台 (UWP)。

## <a name="mapping-direct3d-9-to-directx-11-apis"></a>将 Direct3D 9 映射到 DirectX 11 API


[Direct3D](https://docs.microsoft.com/windows/desktop/direct3d) 仍然是 DirectX 图形的基础，但是 API 自从 DirectX 9 起发生了更改。

-   Microsoft DirectX 图形基础结构 (DXGI) 用于设置图形适配器。 使用 [DXGI](https://docs.microsoft.com/windows/desktop/direct3ddxgi/dx-graphics-dxgi) 来选择缓冲区格式、创建交换链、呈现帧以及创建共享资源。 请参阅 [DXGI 概述](https://docs.microsoft.com/windows/desktop/direct3ddxgi/d3d10-graphics-programming-guide-dxgi)。
-   Direct3D 设备上下文用于设置管道状态以及生成呈现命令。 我们的大多数示例都使用即时上下文直接呈现到设备；Direct3D 11 还支持使用延迟上下文的多线程呈现。 请参阅 [Direct3D 11 中的设备简介](https://docs.microsoft.com/windows/desktop/direct3d11/overviews-direct3d-11-devices-intro)。
-   一些功能已经被弃用，最值得一提的就是固定函数管道。 请参阅[已弃用的功能](https://docs.microsoft.com/windows/desktop/direct3d10/d3d10-graphics-programming-guide-api-features-deprecated)。

有关 Direct3D 11 功能的完整列表，请参阅 [Direct3D 11 功能](https://docs.microsoft.com/windows/desktop/direct3d11/direct3d-11-features)和 [Direct3D 11 功能](https://docs.microsoft.com/windows/desktop/direct3d11/direct3d-11-1-features)。

## <a name="moving-from-direct2d-9-to-direct2d-11"></a>从 Direct2D 9 移动到 Direct2D 11


[Direct2D (Windows)](https://docs.microsoft.com/windows/desktop/Direct2D/direct2d-portal) 仍然是 DirectX 图形和 Windows 的重要组成部分。 你仍然可以使用 Direct2D 来绘制 2D 游戏，以及在 Direct3D 上面绘制覆盖 (HUD)。

Direct2D 在 Direct3D 上运行，因此可以使用任一 API 实现 2D 游戏。 例如，使用 Direct3D 实现的 2D 游戏可以使用正交投影、设置 Z 值以控制基元的绘制顺序，以及使用像素着色器添加特殊效果。

由于 Direct2D 是基于 Direct3D 的，因此它也使用 DXGI 和设备上下文。 请参阅 [Direct2D API 概述](https://docs.microsoft.com/windows/desktop/Direct2D/the-direct2d-api)。

[DirectWrite](https://docs.microsoft.com/windows/desktop/DirectWrite/direct-write-portal) API 增加了对使用 Direct2D 设置文本格式的支持。 请参阅 [DirectWrite 简介](https://docs.microsoft.com/windows/desktop/DirectWrite/introducing-directwrite)。

## <a name="replace-deprecated-helper-libraries"></a>替换已弃用的帮助程序库


D3DX 和 DXUT 已弃用，并且不能由 UWP 游戏使用。 这些帮助程序库为诸如纹理加载和网格加载之类的任务提供了资源。

-   [从 Direct3D 9 到 UWP 的简单移植](walkthrough--simple-port-from-direct3d-9-to-11-1.md)操作实例演示了如何设置窗口、初始化 Direct3D 以及执行基本的 3D 呈现。
-   [使用 DirectX 编写简单的 UWP 游戏](tutorial--create-your-first-uwp-directx-game.md)演示了常见游戏编程任务，包括图形、加载文件、UI、控件以及声音。
-   [DirectX 工具包](https://github.com/Microsoft/DirectXTK)社区项目提供用于 Direct3D 11 和 UWP 应用的 帮助程序类。

## <a name="move-shader-programs-from-fx-to-hlsl"></a>将 FX 中的着色器程序移动到 HLSL


对于 UWP 来说，D3DX 实用工具库（D3DX 9、D3DX 10 和 D3DX 11）（包括“效果”库）已被弃用。 UWP 的所有 DirectX 游戏都使用 [HLSL](https://docs.microsoft.com/windows/desktop/direct3dhlsl/dx-graphics-hlsl)（不使用“效果”库）驱动图形管道。

Visual Studio 仍然在后台使用 FXC 编译着色器对象。 将提前编译 UWP 游戏着色器。 在运行时加载字节码，然后 在相应的呈现传递过程中将每个着色器资源绑定到图形管道。 应将着色器移动到它们自己的 .HLSL 文件中，且应采用 C++ 代码实现呈现技术。

若要快速浏览加载着色器资源，请参阅[从 Direct3D 9 到 UWP 的简单移植](walkthrough--simple-port-from-direct3d-9-to-11-1.md)。

Direct3D 11 引入了着色器模型5，它需要 Direct3D 功能级别 11\_0 （或更高版本）。 请参阅 [Direct3D 11 的 HLSL 着色器模型 5 功能](https://docs.microsoft.com/windows/desktop/direct3dhlsl/overviews-direct3d-11-hlsl)。

## <a name="replace-xnamath-and-d3dxmath"></a>替换 XNAMath 和 D3DXMath


应该将使用 XNAMath（或 D3DXMath）的代码迁移到 [DirectXMath](https://docs.microsoft.com/windows/desktop/dxmath/directxmath-portal)。 DirectXMath 包括可以跨 x86、x64 和 ARM 进行移植的类型。 请参阅 [XNA Math 库中的代码迁移](https://docs.microsoft.com/windows/desktop/dxmath/pg-xnamath-migration)。

请注意，DirectXMath 浮点类型便于与着色器配合使用。 例如，[**XMFLOAT4**](https://docs.microsoft.com/windows/desktop/api/directxmath/ns-directxmath-xmfloat4) 和 [**XMFLOAT4X4**](https://docs.microsoft.com/windows/desktop/api/directxmath/ns-directxmath-xmfloat4x4) 便于对齐常量缓冲区的数据。

## <a name="replace-directsound-with-xaudio2-and-background-audio"></a>将 DirectSound 替换为 XAudio2（以及背景音频）


UWP 不支持 DirectSound：

-   使用 [XAudio2](https://docs.microsoft.com/windows/desktop/xaudio2/xaudio2-apis-portal) 向游戏中添加声音效果。

##  <a name="replace-directinput-with-xinput-and-uwp-apis"></a>将 DirectInput 替换为 XInput 和 UWP API


UWP 不支持 DirectInput：

-   为鼠标、键盘和触摸输入使用 [**CoreWindow**](https://docs.microsoft.com/uwp/api/Windows.UI.Core.CoreWindow) 输入事件回调。
-   使用 [XInput](https://docs.microsoft.com/windows/desktop/xinput/getting-started-with-xinput) 1.4 以获得游戏控制器支持（以及游戏控制器耳机支持）。 如果使用桌面和 UWP 的共享代码库，请参阅 [XInput 版本](https://docs.microsoft.com/windows/desktop/xinput/xinput-versions)以获取有关向后兼容的信息。
-   如果你的游戏需要使用应用栏，请注册 [**EdgeGesture**](https://docs.microsoft.com/uwp/api/Windows.UI.Input.EdgeGesture) 事件。

## <a name="use-microsoft-media-foundation-instead-of-directshow"></a>使用 Microsoft Media Foundation 代替 DirectShow


DirectShow 不再是 DirectX API（或 Windows API）的一部分。 [Microsoft 媒体基础](https://docs.microsoft.com/windows/desktop/medfound/microsoft-media-foundation-sdk)使用共享图面向 Direct3D 提供视频内容。 请参阅 [Direct3D 11 视频 API](https://docs.microsoft.com/windows/desktop/medfound/direct3d-11-video-apis)。

## <a name="replace-directplay-with-networking-code"></a>将 DirectPlay 替换为网络代码


Microsoft DirectPlay 已被弃用。 如果游戏使用网络服务，则需要提供符合 UWP 要求的网络代码。 使用以下 API：

-   [UWP 应用的 Win32 和 COM （网络）（Windows）](https://docs.microsoft.com/uwp/win32-and-com/win32-and-com-for-uwp-apps)
-   [**Windows 网络命名空间（Windows）** ](https://docs.microsoft.com/uwp/api/Windows.Networking)
-   [**Windows 网络 Socket 命名空间（Windows）** ](https://docs.microsoft.com/uwp/api/Windows.Networking.Sockets)
-   [**Windows 网络连接命名空间（Windows）** ](https://docs.microsoft.com/uwp/api/Windows.Networking.Connectivity)
-   [**Windows.applicationmodel.resources.core 命名空间（Windows）** ](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background)

以下文章将帮助你在应用包清单中添加网络功能并声明对网络的支持。

-   [使用套接字进行连接（使用C#/VB/C++和 XAML 的 UWP 应用）（Windows）](https://docs.microsoft.com/previous-versions/windows/apps/hh452976(v=win.10))
-   [使用 Websocket 连接（使用C#/VB/C++和 XAML 的 UWP 应用）（Windows）](https://docs.microsoft.com/previous-versions/windows/apps/hh994396(v=win.10))
-   [连接到 web 服务（使用C#/VB/C++和 XAML 的 UWP 应用）（Windows）](https://docs.microsoft.com/previous-versions/windows/apps/hh761504(v=win.10))
-   [网络基础知识](https://docs.microsoft.com/windows/uwp/networking/networking-basics)

请注意，所有 UWP 应用（包括游戏）都使用特定类型的后台任务，以便在应用暂停时保持连接。 如果你的游戏需要在暂停时保持连接状态，请参阅[网络基础知识](https://docs.microsoft.com/windows/uwp/networking/networking-basics)。

## <a name="function-mapping"></a>功能映射


使用下表可帮助你将 Direct3D 9 中的代码转换到 Direct3D 11。 该表还有助于区分设备和设备上下文。

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Direct3D9</th>
<th align="left">Direct3D 11 等效内容</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p><a href="https://docs.microsoft.com/windows/desktop/api/d3d9helper/nn-d3d9helper-idirect3ddevice9">IDirect3DDevice9</a></p></td>
<td align="left"><p><a href="https://docs.microsoft.com/windows/desktop/api/d3d11_2/nn-d3d11_2-id3d11device2">ID3D11Device2</a></p>
<p><a href="https://docs.microsoft.com/windows/desktop/api/d3d11_2/nn-d3d11_2-id3d11devicecontext2">ID3D11DeviceContext2</a></p>
<p><a href="https://docs.microsoft.com/windows/desktop/direct3d11/overviews-direct3d-11-graphics-pipeline">图形管道</a>中描述了各个图形管道阶段。</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="https://docs.microsoft.com/windows/desktop/api/d3d9helper/nn-d3d9helper-idirect3d9">IDirect3D9</a></p></td>
<td align="left"><p><a href="https://docs.microsoft.com/windows/desktop/api/dxgi1_2/nn-dxgi1_2-idxgifactory2">IDXGIFactory2</a></p>
<p><a href="https://docs.microsoft.com/windows/desktop/api/dxgi1_2/nn-dxgi1_2-idxgiadapter2">IDXGIAdapter2</a></p>
<p><a href="https://docs.microsoft.com/windows/desktop/api/dxgi1_3/nn-dxgi1_3-idxgidevice3">IDXGIDevice3</a></p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="https://docs.microsoft.com/windows/desktop/api/d3d9/nf-d3d9-idirect3ddevice9-present">IDirect3DDevice9：:P 重发</a></p></td>
<td align="left"><p><a href="https://docs.microsoft.com/windows/desktop/api/dxgi1_2/nf-dxgi1_2-idxgiswapchain1-present1">IDXGISwapChain1：:P resent1</a></p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="https://docs.microsoft.com/windows/desktop/api/d3d9helper/nf-d3d9helper-idirect3ddevice9-testcooperativelevel">IDirect3DDevice9::TestCooperativeLevel</a></p></td>
<td align="left"><p>调用设置了 DXGI_PRESENT_TEST 标志的 <a href="https://docs.microsoft.com/windows/desktop/api/dxgi1_2/nf-dxgi1_2-idxgiswapchain1-present1">IDXGISwapChain1::Present1</a>。</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="https://docs.microsoft.com/windows/desktop/api/d3d9helper/nn-d3d9helper-idirect3dbasetexture9">IDirect3DBaseTexture9</a></p>
<p><a href="https://docs.microsoft.com/windows/desktop/api/d3d9helper/nn-d3d9helper-idirect3dtexture9">IDirect3DTexture9</a></p>
<p><a href="https://docs.microsoft.com/windows/desktop/api/d3d9helper/nn-d3d9helper-idirect3dcubetexture9">IDirect3DCubeTexture9</a></p>
<p><a href="https://docs.microsoft.com/windows/desktop/api/d3d9helper/nn-d3d9helper-idirect3dvolumetexture9">IDirect3DVolumeTexture9</a></p>
<p><a href="https://docs.microsoft.com/windows/desktop/api/d3d9helper/nn-d3d9helper-idirect3dindexbuffer9">IDirect3DIndexBuffer9</a></p>
<p><a href="https://docs.microsoft.com/windows/desktop/api/d3d9helper/nn-d3d9helper-idirect3dvertexbuffer9">IDirect3DVertexBuffer9</a></p></td>
<td align="left"><p><a href="https://docs.microsoft.com/windows/desktop/api/d3d11/nn-d3d11-id3d11buffer">ID3D11Buffer</a></p>
<p><a href="https://docs.microsoft.com/windows/desktop/api/d3d11/nn-d3d11-id3d11texture1d">ID3D11Texture1D</a></p>
<p><a href="https://docs.microsoft.com/windows/desktop/api/d3d11/nn-d3d11-id3d11texture2d">ID3D11Texture2D</a></p>
<p><a href="https://docs.microsoft.com/windows/desktop/api/d3d11/nn-d3d11-id3d11texture3d">ID3D11Texture3D</a></p>
<p><a href="https://docs.microsoft.com/windows/desktop/api/d3d11/nn-d3d11-id3d11shaderresourceview">ID3D11ShaderResourceView</a></p>
<p><a href="https://docs.microsoft.com/windows/desktop/api/d3d11/nn-d3d11-id3d11rendertargetview">ID3D11RenderTargetView</a></p>
<p><a href="https://docs.microsoft.com/windows/desktop/api/d3d11/nn-d3d11-id3d11depthstencilview">ID3D11DepthStencilView</a></p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="https://docs.microsoft.com/windows/desktop/api/d3d9helper/nn-d3d9helper-idirect3dvertexshader9">IDirect3DVertexShader9</a></p>
<p><a href="https://docs.microsoft.com/windows/desktop/api/d3d9helper/nn-d3d9helper-idirect3dpixelshader9">IDirect3DPixelShader9</a></p></td>
<td align="left"><p><a href="https://docs.microsoft.com/windows/desktop/api/d3d11/nn-d3d11-id3d11vertexshader">ID3D11VertexShader</a></p>
<p><a href="https://docs.microsoft.com/windows/desktop/api/d3d11/nn-d3d11-id3d11pixelshader">ID3D11PixelShader</a></p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="https://docs.microsoft.com/windows/desktop/api/d3d9helper/nn-d3d9helper-idirect3dvertexdeclaration9">IDirect3DVertexDeclaration9</a></p></td>
<td align="left"><p><a href="https://docs.microsoft.com/windows/desktop/api/d3d11/nn-d3d11-id3d11inputlayout">ID3D11InputLayout</a></p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="https://docs.microsoft.com/windows/desktop/direct3d9/id3dxeffectstatemanager--setrenderstate">IDirect3DDevice9::SetRenderState</a></p>
<p><a href="https://docs.microsoft.com/windows/desktop/direct3d9/id3dxeffectstatemanager--setsamplerstate">IDirect3DDevice9::SetSamplerState</a></p></td>
<td align="left"><p><a href="https://docs.microsoft.com/windows/desktop/api/d3d11_1/nn-d3d11_1-id3d11blendstate1">ID3D11BlendState1</a></p>
<p><a href="https://docs.microsoft.com/windows/desktop/api/d3d11/nn-d3d11-id3d11depthstencilstate">ID3D11DepthStencilState</a></p>
<p><a href="https://docs.microsoft.com/windows/desktop/api/d3d11_1/nn-d3d11_1-id3d11rasterizerstate1">ID3D11RasterizerState1</a></p>
<p><a href="https://docs.microsoft.com/windows/desktop/api/d3d11/nn-d3d11-id3d11samplerstate">ID3D11SamplerState</a></p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="https://docs.microsoft.com/windows/desktop/api/d3d9/nf-d3d9-idirect3ddevice9-drawindexedprimitive">IDirect3DDevice9：:D rawIndexedPrimitive</a></p>
<p><a href="https://docs.microsoft.com/windows/desktop/api/d3d9/nf-d3d9-idirect3ddevice9-drawprimitive">IDirect3DDevice9：:D rawPrimitive</a></p></td>
<td align="left"><p><a href="https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-draw">ID3D11DeviceContext：:D raw</a></p>
<p><a href="https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-drawindexed">ID3D11DeviceContext：:D rawIndexed</a></p>
<p><a href="https://docs.microsoft.com/windows/desktop/api/d3d10/nf-d3d10-id3d10device-drawindexedinstanced">ID3D11DeviceContext：:D rawIndexedInstanced</a></p>
<p><a href="https://docs.microsoft.com/windows/desktop/api/d3d10/nf-d3d10-id3d10device-drawinstanced">ID3D11DeviceContext：:D rawInstanced</a></p>
<p><a href="https://docs.microsoft.com/windows/desktop/api/d3d10/nf-d3d10-id3d10device-iasetprimitivetopology">ID3D11DeviceContext：： IASetPrimitiveTopology</a></p>
<p><a href="https://docs.microsoft.com/windows/desktop/api/d3d10/nf-d3d10-id3d10device-drawauto">ID3D11DeviceContext：:D rawAuto</a></p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="https://docs.microsoft.com/windows/desktop/api/d3d9/nf-d3d9-idirect3ddevice9-beginscene">IDirect3DDevice9::BeginScene</a></p>
<p><a href="https://docs.microsoft.com/windows/desktop/api/d3d9/nf-d3d9-idirect3ddevice9-endscene">IDirect3DDevice9::EndScene</a></p>
<p><a href="https://docs.microsoft.com/windows/desktop/api/d3d9/nf-d3d9-idirect3ddevice9-drawprimitiveup">IDirect3DDevice9：:D rawPrimitiveUP</a></p>
<p><a href="https://docs.microsoft.com/windows/desktop/api/d3d9/nf-d3d9-idirect3ddevice9-drawindexedprimitiveup">IDirect3DDevice9：:D rawIndexedPrimitiveUP</a></p></td>
<td align="left"><p>无直接等效项</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="https://docs.microsoft.com/windows/desktop/api/d3d9helper/nf-d3d9helper-idirect3ddevice9-showcursor">IDirect3DDevice9：： ShowCursor</a></p>
<p><a href="https://docs.microsoft.com/windows/desktop/api/d3d9/nf-d3d9-idirect3ddevice9-setcursorposition">IDirect3DDevice9::SetCursorPosition</a></p>
<p><a href="https://docs.microsoft.com/windows/desktop/api/d3d9/nf-d3d9-idirect3ddevice9-setcursorproperties">IDirect3DDevice9::SetCursorProperties</a></p></td>
<td align="left"><p>使用标准光标 API。</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="https://docs.microsoft.com/windows/desktop/api/d3d9/nf-d3d9-idirect3ddevice9-reset">IDirect3DDevice9：： Reset</a></p></td>
<td align="left"><p>LOST 设备和 POOL_MANAGED 不再存在。 <a href="https://docs.microsoft.com/windows/desktop/api/dxgi1_2/nf-dxgi1_2-idxgiswapchain1-present1">IDXGISwapChain1::Present1</a> 可能失败，并带有 <a href="https://docs.microsoft.com/windows/desktop/direct3ddxgi/dxgi-error">DXGI_ERROR_DEVICE_REMOVED</a> 返回值。</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="https://docs.microsoft.com/windows/desktop/api/d3d9/nf-d3d9-idirect3ddevice9-drawrectpatch">IDirect3DDevice9:DrawRectPatch</a></p>
<p><a href="https://docs.microsoft.com/windows/desktop/api/d3d9/nf-d3d9-idirect3ddevice9-drawtripatch">IDirect3DDevice9:DrawTriPatch</a></p>
<p><a href="https://docs.microsoft.com/windows/desktop/api/d3d9/nf-d3d9-idirect3ddevice9-lightenable">IDirect3DDevice9:LightEnable</a></p>
<p><a href="https://docs.microsoft.com/windows/desktop/api/d3d9/nf-d3d9-idirect3ddevice9-multiplytransform">IDirect3DDevice9:MultiplyTransform</a></p>
<p><a href="https://docs.microsoft.com/windows/desktop/direct3d9/id3dxeffectstatemanager--setlight">IDirect3DDevice9:SetLight</a></p>
<p><a href="https://docs.microsoft.com/windows/desktop/api/d3d9helper/nf-d3d9helper-idirect3ddevice9-setmaterial">IDirect3DDevice9:SetMaterial</a></p>
<p><a href="https://docs.microsoft.com/windows/desktop/api/d3d9helper/nf-d3d9helper-idirect3ddevice9-setnpatchmode">IDirect3DDevice9:SetNPatchMode</a></p>
<p><a href="https://docs.microsoft.com/windows/desktop/api/d3d9helper/nf-d3d9helper-idirect3ddevice9-settransform">IDirect3DDevice9:SetTransform</a></p>
<p><a href="https://docs.microsoft.com/windows/desktop/api/d3d9/nf-d3d9-idirect3ddevice9-setfvf">IDirect3DDevice9:SetFVF</a></p>
<p><a href="https://docs.microsoft.com/windows/desktop/api/d3d9helper/nf-d3d9helper-idirect3ddevice9-settexturestagestate">IDirect3DDevice9:SetTextureStageState</a></p></td>
<td align="left"><p>固定函数管道已被弃用。</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="https://docs.microsoft.com/windows/desktop/api/d3d9/nf-d3d9-idirect3d9-checkdepthstencilmatch">IDirect3DDevice9:CheckDepthStencilMatch</a></p>
<p><a href="https://docs.microsoft.com/windows/desktop/api/d3d9/nf-d3d9-idirect3d9-checkdeviceformat">IDirect3DDevice9:CheckDeviceFormat</a></p>
<p><a href="https://docs.microsoft.com/windows/desktop/api/d3d9/nf-d3d9-idirect3d9-getdevicecaps">IDirect3DDevice9:GetDeviceCaps</a></p>
<p><a href="https://docs.microsoft.com/windows/desktop/api/d3d9/nf-d3d9-idirect3ddevice9-validatedevice">IDirect3DDevice9:ValidateDevice</a></p></td>
<td align="left"><p>功能位已被替换为功能级别。 对于任何给定的功能级别，只有几个格式和功能用例是可选的。 这些可以使用 <a href="https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-id3d11device-checkfeaturesupport">ID3D11Device::CheckFeatureSupport</a> 和 <a href="https://docs.microsoft.com/windows/desktop/api/d3d10/nf-d3d10-id3d10device-checkformatsupport">ID3D11Device::CheckFormatSupport</a> 进行检查。</p></td>
</tr>
</tbody>
</table>

 

## <a name="surface-format-mapping"></a>图面格式映射


使用下表将 Direct3D 9 格式转换为 DXGI 格式。

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Direct3D 9 格式</th>
<th align="left">Direct3D 11 格式</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p>D3DFMT_UNKNOWN</p></td>
<td align="left"><p>DXGI_FORMAT_UNKNOWN</p></td>
</tr>
<tr class="even">
<td align="left"><p>D3DFMT_R8G8B8</p></td>
<td align="left"><p>不可用</p></td>
</tr>
<tr class="odd">
<td align="left"><p>D3DFMT_A8R8G8B8</p></td>
<td align="left"><p>DXGI_FORMAT_B8G8R8A8_UNORM</p>
<p>DXGI_FORMAT_B8G8R8A8_UNORM_SRGB</p></td>
</tr>
<tr class="even">
<td align="left"><p>D3DFMT_X8R8G8B8</p></td>
<td align="left"><p>DXGI_FORMAT_B8G8R8X8_UNORM</p>
<p>DXGI_FORMAT_B8G8R8X8_UNORM_SRGB</p></td>
</tr>
<tr class="odd">
<td align="left"><p>D3DFMT_R5G6B5</p></td>
<td align="left"><p>DXGI_FORMAT_B5G6R5_UNORM</p></td>
</tr>
<tr class="even">
<td align="left"><p>D3DFMT_X1R5G5B5</p></td>
<td align="left"><p>不可用</p></td>
</tr>
<tr class="odd">
<td align="left"><p>D3DFMT_A1R5G5B5</p></td>
<td align="left"><p>DXGI_FORMAT_B5G5R5A1_UNORM</p></td>
</tr>
<tr class="even">
<td align="left"><p>D3DFMT_A4R4G4B4</p></td>
<td align="left"><p>DXGI_FORMAT_B4G4R4A4_UNORM</p></td>
</tr>
<tr class="odd">
<td align="left"><p>D3DFMT_R3G3B2</p></td>
<td align="left"><p>不可用</p></td>
</tr>
<tr class="even">
<td align="left"><p>D3DFMT_A8</p></td>
<td align="left"><p>DXGI_FORMAT_A8_UNORM</p></td>
</tr>
<tr class="odd">
<td align="left"><p>D3DFMT_A8R3G3B2</p></td>
<td align="left"><p>不可用</p></td>
</tr>
<tr class="even">
<td align="left"><p>D3DFMT_X4R4G4B4</p></td>
<td align="left"><p>不可用</p></td>
</tr>
<tr class="odd">
<td align="left"><p>D3DFMT_A2B10G10R10</p></td>
<td align="left"><p>DXGI_FORMAT_R10G10B10A2</p></td>
</tr>
<tr class="even">
<td align="left"><p>D3DFMT_A8B8G8R8</p></td>
<td align="left"><p>DXGI_FORMAT_R8G8B8A8_UNORM</p>
<p>DXGI_FORMAT_R8G8B8A8_UNORM_SRGB</p></td>
</tr>
<tr class="odd">
<td align="left"><p>D3DFMT_X8B8G8R8</p></td>
<td align="left"><p>不可用</p></td>
</tr>
<tr class="even">
<td align="left"><p>D3DFMT_G16R16</p></td>
<td align="left"><p>DXGI_FORMAT_R16G16_UNORM</p></td>
</tr>
<tr class="odd">
<td align="left"><p>D3DFMT_A2R10G10B10</p></td>
<td align="left"><p>不可用</p></td>
</tr>
<tr class="even">
<td align="left"><p>D3DFMT_A16B16G16R16</p></td>
<td align="left"><p>DXGI_FORMAT_R16G16B16A16_UNORM</p></td>
</tr>
<tr class="odd">
<td align="left"><p>D3DFMT_A8P8</p></td>
<td align="left"><p>不可用</p></td>
</tr>
<tr class="even">
<td align="left"><p>D3DFMT_P8</p></td>
<td align="left"><p>不可用</p></td>
</tr>
<tr class="odd">
<td align="left"><p>D3DFMT_L8</p></td>
<td align="left"><p>DXGI_FORMAT_R8_UNORM</p>
<div class="alert">
<strong>请注意</strong>   在着色器中使用 swizzle，以将红色复制到其他组件，以获取 Direct3D 9 行为。
</div>
<div>
 
</div></td>
</tr>
<tr class="even">
<td align="left"><p>D3DFMT_A8L8</p></td>
<td align="left"><p>DXGI_FORMAT_R8G8_UNORM</p>
<div class="alert">
<strong>请注意</strong>   在着色器中使用 rrrg 重复红色，并将绿色移动到 alpha 组件以获取 Direct3D 9 行为。
</div>
<div>
 
</div></td>
</tr>
<tr class="odd">
<td align="left"><p>D3DFMT_A4L4</p></td>
<td align="left"><p>不可用</p></td>
</tr>
<tr class="even">
<td align="left"><p>D3DFMT_V8U8</p></td>
<td align="left"><p>DXGI_FORMAT_R8G8_SNORM</p></td>
</tr>
<tr class="odd">
<td align="left"><p>D3DFMT_L6V5U5</p></td>
<td align="left"><p>不可用</p></td>
</tr>
<tr class="even">
<td align="left"><p>D3DFMT_X8L8V8U8</p></td>
<td align="left"><p>不可用</p></td>
</tr>
<tr class="odd">
<td align="left"><p>D3DFMT_Q8W8V8U8</p></td>
<td align="left"><p>DXGI_FORMAT_R8G8B8A8_SNORM</p></td>
</tr>
<tr class="even">
<td align="left"><p>D3DFMT_V16U16</p></td>
<td align="left"><p>DXGI_FORMAT_R16G16_SNORM</p></td>
</tr>
<tr class="odd">
<td align="left"><p>D3DFMT_W11V11U10</p></td>
<td align="left"><p>不可用</p></td>
</tr>
<tr class="even">
<td align="left"><p>D3DFMT_A2W10V10U10</p></td>
<td align="left"><p>不可用</p></td>
</tr>
<tr class="odd">
<td align="left"><p>D3DFMT_UYVY</p></td>
<td align="left"><p>不可用</p></td>
</tr>
<tr class="even">
<td align="left"><p>D3DFMT_R8G8_B8G8</p></td>
<td align="left"><p>DXGI_FORMAT_G8R8_G8B8_UNORM</p>
<div class="alert">
<strong>请注意</strong>，Direct3D 9 中的   数据通过 255.0 f 向上扩展，但这可以在着色器中进行处理。
</div>
<div>
 
</div></td>
</tr>
<tr class="odd">
<td align="left"><p>D3DFMT_YUY2</p></td>
<td align="left"><p>不可用</p></td>
</tr>
<tr class="even">
<td align="left"><p>D3DFMT_G8R8_G8B8</p></td>
<td align="left"><p>DXGI_FORMAT_R8G8_B8G8_UNORM</p>
<div class="alert">
<strong>请注意</strong>，Direct3D 9 中的   数据通过 255.0 f 向上扩展，但这可以在着色器中进行处理。
</div>
<div>
 
</div></td>
</tr>
<tr class="odd">
<td align="left"><p>D3DFMT_DXT1</p></td>
<td align="left"><p>DXGI_FORMAT_BC1_UNORM &amp; DXGI_FORMAT_BC1_UNORM_SRGB</p></td>
</tr>
<tr class="even">
<td align="left"><p>D3DFMT_DXT2</p></td>
<td align="left"><p>DXGI_FORMAT_BC1_UNORM &amp; DXGI_FORMAT_BC1_UNORM_SRGB</p>
<div class="alert">
<strong>请注意</strong>，   DXT1 和 DXT2 与 API/硬件的透视是相同的。 唯一的差别是是否使用了预乘 alpha，这可以通过应用程序来跟踪，并且不需要单独的格式。
</div>
<div>
 
</div></td>
</tr>
<tr class="odd">
<td align="left"><p>D3DFMT_DXT3</p></td>
<td align="left"><p>DXGI_FORMAT_BC2_UNORM &amp; DXGI_FORMAT_BC2_UNORM_SRGB</p></td>
</tr>
<tr class="even">
<td align="left"><p>D3DFMT_DXT4</p></td>
<td align="left"><p>DXGI_FORMAT_BC2_UNORM &amp; DXGI_FORMAT_BC2_UNORM_SRGB</p>
<div class="alert">
<strong>请注意</strong>，   DXT3 和 DXT4 与 API/硬件的透视是相同的。 唯一的差别是是否使用了预乘 alpha，这可以通过应用程序来跟踪，并且不需要单独的格式。
</div>
<div>
 
</div></td>
</tr>
<tr class="odd">
<td align="left"><p>D3DFMT_DXT5</p></td>
<td align="left"><p>DXGI_FORMAT_BC3_UNORM &amp; DXGI_FORMAT_BC3_UNORM_SRGB</p></td>
</tr>
<tr class="even">
<td align="left"><p>D3DFMT_D16 &amp; D3DFMT_D16_LOCKABLE</p></td>
<td align="left"><p>DXGI_FORMAT_D16_UNORM</p></td>
</tr>
<tr class="odd">
<td align="left"><p>D3DFMT_D32</p></td>
<td align="left"><p>不可用</p></td>
</tr>
<tr class="even">
<td align="left"><p>D3DFMT_D15S1</p></td>
<td align="left"><p>不可用</p></td>
</tr>
<tr class="odd">
<td align="left"><p>D3DFMT_D24S8</p></td>
<td align="left"><p>不可用</p></td>
</tr>
<tr class="even">
<td align="left"><p>D3DFMT_D24X8</p></td>
<td align="left"><p>不可用</p></td>
</tr>
<tr class="odd">
<td align="left"><p>D3DFMT_D24X4S4</p></td>
<td align="left"><p>不可用</p></td>
</tr>
<tr class="even">
<td align="left"><p>D3DFMT_D16</p></td>
<td align="left"><p>DXGI_FORMAT_D16_UNORM</p></td>
</tr>
<tr class="odd">
<td align="left"><p>D3DFMT_D32F_LOCKABLE</p></td>
<td align="left"><p>DXGI_FORMAT_D32_FLOAT</p></td>
</tr>
<tr class="even">
<td align="left"><p>D3DFMT_D24FS8</p></td>
<td align="left"><p>不可用</p></td>
</tr>
<tr class="odd">
<td align="left"><p>D3DFMT_S1D15</p></td>
<td align="left"><p>不可用</p></td>
</tr>
<tr class="even">
<td align="left"><p>D3DFMT_S8D24</p></td>
<td align="left"><p>DXGI_FORMAT_D24_UNORM_S8_UINT</p></td>
</tr>
<tr class="odd">
<td align="left"><p>D3DFMT_X8D24</p></td>
<td align="left"><p>不可用</p></td>
</tr>
<tr class="even">
<td align="left"><p>D3DFMT_X4S4D24</p></td>
<td align="left"><p>不可用</p></td>
</tr>
<tr class="odd">
<td align="left"><p>D3DFMT_L16</p></td>
<td align="left"><p>DXGI_FORMAT_R16_UNORM</p>
<div class="alert">
<strong>请注意</strong>   在着色器中使用 swizzle，以将红色复制到其他组件，以获取 D3D9 行为。
</div>
<div>
 
</div></td>
</tr>
<tr class="even">
<td align="left"><p>D3DFMT_INDEX16</p></td>
<td align="left"><p>DXGI_FORMAT_R16_UINT</p></td>
</tr>
<tr class="odd">
<td align="left"><p>D3DFMT_INDEX32</p></td>
<td align="left"><p>DXGI_FORMAT_R32_UINT</p></td>
</tr>
<tr class="even">
<td align="left"><p>D3DFMT_Q16W16V16U16</p></td>
<td align="left"><p>DXGI_FORMAT_R16G16B16A16_SNORM</p></td>
</tr>
<tr class="odd">
<td align="left"><p>D3DFMT_MULTI2_ARGB8</p></td>
<td align="left"><p>不可用</p></td>
</tr>
<tr class="even">
<td align="left"><p>D3DFMT_R16F</p></td>
<td align="left"><p>DXGI_FORMAT_R16_FLOAT</p></td>
</tr>
<tr class="odd">
<td align="left"><p>D3DFMT_G16R16F</p></td>
<td align="left"><p>DXGI_FORMAT_R16G16_FLOAT</p></td>
</tr>
<tr class="even">
<td align="left"><p>D3DFMT_A16B16G16R16F</p></td>
<td align="left"><p>DXGI_FORMAT_R16G16B16A16_FLOAT</p></td>
</tr>
<tr class="odd">
<td align="left"><p>D3DFMT_R32F</p></td>
<td align="left"><p>DXGI_FORMAT_R32_FLOAT</p></td>
</tr>
<tr class="even">
<td align="left"><p>D3DFMT_G32R32F</p></td>
<td align="left"><p>DXGI_FORMAT_R32G32_FLOAT</p></td>
</tr>
<tr class="odd">
<td align="left"><p>D3DFMT_A32B32G32R32F</p></td>
<td align="left"><p>DXGI_FORMAT_R32G32B32A32_FLOAT</p></td>
</tr>
<tr class="even">
<td align="left"><p>D3DFMT_CxV8U8</p></td>
<td align="left"><p>不可用</p></td>
</tr>
<tr class="odd">
<td align="left"><p>D3DDECLTYPE_FLOAT1</p></td>
<td align="left"><p>DXGI_FORMAT_R32_FLOAT</p></td>
</tr>
<tr class="even">
<td align="left"><p>D3DDECLTYPE_FLOAT2</p></td>
<td align="left"><p>DXGI_FORMAT_R32G32_FLOAT</p></td>
</tr>
<tr class="odd">
<td align="left"><p>D3DDECLTYPE_FLOAT3</p></td>
<td align="left"><p>DXGI_FORMAT_R32G32B32_FLOAT</p></td>
</tr>
<tr class="even">
<td align="left"><p>D3DDECLTYPE_FLOAT4</p></td>
<td align="left"><p>DXGI_FORMAT_R32G32B32A32_FLOAT</p></td>
</tr>
<tr class="odd">
<td align="left"><p>D3DDECLTYPED3DCOLOR</p></td>
<td align="left"><p>不可用</p></td>
</tr>
<tr class="even">
<td align="left"><p>D3DDECLTYPE_UBYTE4</p></td>
<td align="left"><p>DXGI_FORMAT_R8G8B8A8_UINT</p>
<div class="alert">
<strong>请注意</strong>   着色器获取 UINT 值，但如果需要 Direct3D 9 样式整数浮点值（0.0 f，1.0 f .。。255. f），UINT 只能在着色器中转换为 float32。
</div>
<div>
 
</div></td>
</tr>
<tr class="odd">
<td align="left"><p>D3DDECLTYPE_SHORT2</p></td>
<td align="left"><p>DXGI_FORMAT_R16G16_SINT</p>
<div class="alert">
<strong>请注意</strong>   着色器获取圣数值，但如果需要 Direct3D 9 样式整数浮点值，则可以只在着色器中将圣马丁转换为 float32。
</div>
<div>
 
</div></td>
</tr>
<tr class="even">
<td align="left"><p>D3DDECLTYPE_SHORT4</p></td>
<td align="left"><p>DXGI_FORMAT_R16G16B16A16_SINT</p>
<div class="alert">
<strong>请注意</strong>   着色器获取圣数值，但如果需要 Direct3D 9 样式整数浮点值，则可以只在着色器中将圣马丁转换为 float32。
</div>
<div>
 
</div></td>
</tr>
<tr class="odd">
<td align="left"><p>D3DDECLTYPE_UBYTE4N</p></td>
<td align="left"><p>DXGI_FORMAT_R8G8B8A8_UNORM</p></td>
</tr>
<tr class="even">
<td align="left"><p>D3DDECLTYPE_SHORT2N</p></td>
<td align="left"><p>DXGI_FORMAT_R16G16_SNORM</p></td>
</tr>
<tr class="odd">
<td align="left"><p>D3DDECLTYPE_SHORT4N</p></td>
<td align="left"><p>DXGI_FORMAT_R16G16B16A16_SNORM</p></td>
</tr>
<tr class="even">
<td align="left"><p>D3DDECLTYPE_USHORT2N</p></td>
<td align="left"><p>DXGI_FORMAT_R16G16_UNORM</p></td>
</tr>
<tr class="odd">
<td align="left"><p>D3DDECLTYPE_USHORT4N</p></td>
<td align="left"><p>DXGI_FORMAT_R16G16B16A16_UNORM</p></td>
</tr>
<tr class="even">
<td align="left"><p>D3DDECLTYPE_UDEC3</p></td>
<td align="left"><p>不可用</p></td>
</tr>
<tr class="odd">
<td align="left"><p>D3DDECLTYPE_DEC3N</p></td>
<td align="left"><p>不可用</p></td>
</tr>
<tr class="even">
<td align="left"><p>D3DDECLTYPE_FLOAT16_2</p></td>
<td align="left"><p>DXGI_FORMAT_R16G16_FLOAT</p></td>
</tr>
<tr class="odd">
<td align="left"><p>D3DDECLTYPE_FLOAT16_4</p></td>
<td align="left"><p>DXGI_FORMAT_R16G16B16A16_FLOAT</p></td>
</tr>
<tr class="even">
<td align="left"><p>FourCC 'ATI1'</p></td>
<td align="left"><p>DXGI_FORMAT_BC4_UNORM</p>
<div class="alert">
<strong>请注意</strong>   需要功能级别10.0 或更高版本
</div>
<div>
 
</div></td>
</tr>
<tr class="odd">
<td align="left"><p>FourCC 'ATI2'</p></td>
<td align="left"><p>DXGI_FORMAT_BC5_UNORM</p>
<div class="alert">
<strong>请注意</strong>   需要功能级别10.0 或更高版本
</div>
<div>
 
</div></td>
</tr>
</tbody>
</table>

 

 

 




