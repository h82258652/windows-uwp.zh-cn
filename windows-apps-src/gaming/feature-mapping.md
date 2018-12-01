---
title: 将 DirectX 9 功能映射到 DirectX 11 API
description: 了解 Direct3D 9 游戏使用的功能如何转换到 Direct3D 11 和通用 Windows 平台 (UWP)。
ms.assetid: 3aa8a114-4e47-ae0a-9447-88ba324377b8
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, 游戏, directx 9, directx 11, 移植
ms.localizationpriority: medium
ms.openlocfilehash: 56bb86706795e773d21e45263f640f9fc0aa596a
ms.sourcegitcommit: d2517e522cacc5240f7dffd5bc1eaa278e3f7768
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/30/2018
ms.locfileid: "8338557"
---
# <a name="map-directx-9-features-to-directx-11-apis"></a>将 DirectX 9 功能映射到 DirectX 11 API



**小结**

-   [规划 DirectX 移植](plan-your-directx-port.md)
-   [从 Direct3D 9 到 Direct3D 11 的重要更改](understand-direct3d-11-1-concepts.md)
-   功能映射


了解 Direct3D 9 游戏使用的功能如何转换到 Direct3D 11 和通用 Windows 平台 (UWP)。

## <a name="mapping-direct3d-9-to-directx-11-apis"></a>将 Direct3D 9 映射到 DirectX 11 API


[Direct3D](https://msdn.microsoft.com/library/windows/desktop/hh309466) 仍然是 DirectX 图形的基础，但是 API 自从 DirectX 9 起发生了更改。

-   Microsoft DirectX 图形基础结构 (DXGI) 用于设置图形适配器。 使用 [DXGI](https://msdn.microsoft.com/library/windows/desktop/hh404534) 来选择缓冲区格式、创建交换链、呈现帧以及创建共享资源。 请参阅 [DXGI 概述](https://msdn.microsoft.com/library/windows/desktop/bb205075)。
-   Direct3D 设备上下文用于设置管道状态以及生成呈现命令。 我们的大多数示例都使用即时上下文直接呈现到设备；Direct3D 11 还支持使用延迟上下文的多线程呈现。 请参阅 [Direct3D 11 中的设备简介](https://msdn.microsoft.com/library/windows/desktop/ff476880)。
-   一些功能已经被弃用，最值得一提的就是固定函数管道。 请参阅[已弃用的功能](https://msdn.microsoft.com/library/windows/desktop/cc308047)。

有关 Direct3D 11 功能的完整列表，请参阅 [Direct3D 11 功能](https://msdn.microsoft.com/library/windows/desktop/ff476342)和 [Direct3D 11 功能](https://msdn.microsoft.com/library/windows/desktop/hh404562)。

## <a name="moving-from-direct2d-9-to-direct2d-11"></a>从 Direct2D 9 移动到 Direct2D 11


[Direct2D (Windows)](https://msdn.microsoft.com/library/windows/desktop/dd370990) 仍然是 DirectX 图形和 Windows 的重要组成部分。 你仍然可以使用 Direct2D 来绘制 2D 游戏，以及在 Direct3D 上面绘制覆盖 (HUD)。

Direct2D 在 Direct3D 上运行，因此可以使用任一 API 实现 2D 游戏。 例如，使用 Direct3D 实现的 2D 游戏可以使用正交投影、设置 Z 值以控制基元的绘制顺序，以及使用像素着色器添加特殊效果。

由于 Direct2D 是基于 Direct3D 的，因此它也使用 DXGI 和设备上下文。 请参阅 [Direct2D API 概述](https://msdn.microsoft.com/library/windows/desktop/dd317121)。

[DirectWrite](https://msdn.microsoft.com/library/windows/desktop/dd368038) API 增加了对使用 Direct2D 设置文本格式的支持。 请参阅 [DirectWrite 简介](https://msdn.microsoft.com/library/windows/desktop/dd371554)。

## <a name="replace-deprecated-helper-libraries"></a>替换已弃用的帮助程序库


D3DX 和 DXUT 已弃用，并且不能由 UWP 游戏使用。 这些帮助程序库为诸如纹理加载和网格加载之类的任务提供了资源。

-   [从 Direct3D 9 到 UWP 的简单移植](walkthrough--simple-port-from-direct3d-9-to-11-1.md)操作实例演示了如何设置窗口、初始化 Direct3D 以及执行基本的 3D 呈现。
-   [使用 DirectX 编写简单的 UWP 游戏](tutorial--create-your-first-uwp-directx-game.md)演示了常见游戏编程任务，包括图形、加载文件、UI、控件以及声音。
-   [DirectX 工具包](http://go.microsoft.com/fwlink/p/?LinkID=248929)社区项目提供用于 Direct3D 11 和 UWP 应用的 帮助程序类。

## <a name="move-shader-programs-from-fx-to-hlsl"></a>将 FX 中的着色器程序移动到 HLSL


对于 UWP 来说，D3DX 实用工具库（D3DX 9、D3DX 10 和 D3DX 11）（包括“效果”库）已被弃用。 UWP 的所有 DirectX 游戏都使用 [HLSL](https://msdn.microsoft.com/library/windows/desktop/bb509561)（不使用“效果”库）驱动图形管道。

Visual Studio 仍然在后台使用 FXC 编译着色器对象。 将提前编译 UWP 游戏着色器。 在运行时加载字节码，然后 在相应的呈现传递过程中将每个着色器资源绑定到图形管道。 应将着色器移动到它们自己的 .HLSL 文件中，且应采用 C++ 代码实现呈现技术。

若要快速浏览加载着色器资源，请参阅[从 Direct3D 9 到 UWP 的简单移植](walkthrough--simple-port-from-direct3d-9-to-11-1.md)。

Direct3D 11 引入了着色器模型 5，它需要 Direct3D 功能级别 11\_0（或更高功能级别）。 请参阅 [Direct3D 11 的 HLSL 着色器模型 5 功能](https://msdn.microsoft.com/library/windows/desktop/ff471419)。

## <a name="replace-xnamath-and-d3dxmath"></a>替换 XNAMath 和 D3DXMath


应该将使用 XNAMath（或 D3DXMath）的代码迁移到 [DirectXMath](https://msdn.microsoft.com/library/windows/desktop/hh437833)。 DirectXMath 包括可以跨 x86、x64 和 ARM 进行移植的类型。 请参阅 [XNA Math 库中的代码迁移](https://msdn.microsoft.com/library/windows/desktop/ee418730)。

请注意，DirectXMath 浮点类型便于与着色器配合使用。 例如，[**XMFLOAT4**](https://msdn.microsoft.com/library/windows/desktop/ee419608) 和 [**XMFLOAT4X4**](https://msdn.microsoft.com/library/windows/desktop/ee419621) 便于对齐常量缓冲区的数据。

## <a name="replace-directsound-with-xaudio2-and-background-audio"></a>将 DirectSound 替换为 XAudio2（以及背景音频）


UWP 不支持 DirectSound：

-   使用 [XAudio2](https://msdn.microsoft.com/library/windows/desktop/hh405049) 向游戏中添加声音效果。

##  <a name="replace-directinput-with-xinput-and-uwp-apis"></a>将 DirectInput 替换为 XInput 和 UWP API


UWP 不支持 DirectInput：

-   为鼠标、键盘和触摸输入使用 [**CoreWindow**](https://msdn.microsoft.com/library/windows/apps/br208225) 输入事件回调。
-   使用 [XInput](https://msdn.microsoft.com/library/windows/desktop/ee417001) 1.4 以获得游戏控制器支持（以及游戏控制器耳机支持）。 如果使用桌面和 UWP 的共享代码库，请参阅 [XInput 版本](https://msdn.microsoft.com/library/windows/desktop/hh405051)以获取有关向后兼容的信息。
-   如果你的游戏需要使用应用栏，请注册 [**EdgeGesture**](https://msdn.microsoft.com/library/windows/apps/hh701600) 事件。

## <a name="use-microsoft-media-foundation-instead-of-directshow"></a>使用 Microsoft Media Foundation 代替 DirectShow


DirectShow 不再是 DirectX API（或 Windows API）的一部分。 [Microsoft 媒体基础](https://msdn.microsoft.com/library/windows/desktop/ms694197)使用共享图面向 Direct3D 提供视频内容。 请参阅 [Direct3D 11 视频 API](https://msdn.microsoft.com/library/windows/desktop/hh447677)。

## <a name="replace-directplay-with-networking-code"></a>将 DirectPlay 替换为网络代码


Microsoft DirectPlay 已被弃用。 如果游戏使用网络服务，则需要提供符合 UWP 要求的网络代码。 使用以下 API：

-   [适用于 UWP 应用的 Win32 和 COM（网络）(Windows)](https://msdn.microsoft.com/library/windows/apps/br205759)
-   [**Windows.Networking 命名空间 (Windows)**](https://msdn.microsoft.com/library/windows/apps/br207124)
-   [**Windows.Networking.Sockets 命名空间 (Windows)**](https://msdn.microsoft.com/library/windows/apps/br226960)
-   [**Windows.Networking.Connectivity 命名空间 (Windows)**](https://msdn.microsoft.com/library/windows/apps/br207308)
-   [**Windows.ApplicationModel.Background 命名空间 (Windows)**](https://msdn.microsoft.com/library/windows/apps/br224847)

以下文章将帮助你在应用包清单中添加网络功能并声明对网络的支持。

-   [使用套接字进行连接（使用 C#/VB/C++ 和 XAML 的 UWP 应用）(Windows)](https://msdn.microsoft.com/library/windows/apps/xaml/hh452976)
-   [使用 WebSockets 进行连接（使用 C#/VB/C++ 和 XAML 的 UWP 应用）(Windows)](https://msdn.microsoft.com/library/windows/apps/xaml/hh994396)
-   [连接到 Web 服务（使用 C#/VB/C++ 和 XAML 的 UWP 应用）(Windows)](https://msdn.microsoft.com/library/windows/apps/xaml/hh761504)
-   [网络基础知识](https://msdn.microsoft.com/library/windows/apps/mt280233)

请注意，所有 UWP 应用（包括游戏）都使用特定类型的后台任务，以便在应用暂停时保持连接。 如果你的游戏需要在暂停时保持连接状态，请参阅[网络基础知识](https://msdn.microsoft.com/library/windows/apps/mt280233)。

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
<td align="left"><p><a href="https://msdn.microsoft.com/library/windows/desktop/bb174336">IDirect3DDevice9</a></p></td>
<td align="left"><p><a href="https://msdn.microsoft.com/library/windows/desktop/dn280493">ID3D11Device2</a></p>
<p><a href="https://msdn.microsoft.com/library/windows/desktop/dn280498">ID3D11DeviceContext2</a></p>
<p><a href="https://msdn.microsoft.com/library/windows/desktop/ff476882">图形管道</a>中描述了各个图形管道阶段。</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="https://msdn.microsoft.com/library/windows/desktop/bb174300">IDirect3D9</a></p></td>
<td align="left"><p><a href="https://msdn.microsoft.com/library/windows/desktop/hh404556">IDXGIFactory2</a></p>
<p><a href="https://msdn.microsoft.com/library/windows/desktop/hh404537">IDXGIAdapter2</a></p>
<p><a href="https://msdn.microsoft.com/library/windows/desktop/dn280345">IDXGIDevice3</a></p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="https://msdn.microsoft.com/library/windows/desktop/bb174423">IDirect3DDevice9::Present</a></p></td>
<td align="left"><p><a href="https://msdn.microsoft.com/library/windows/desktop/hh446797">IDXGISwapChain1::Present1</a></p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="https://msdn.microsoft.com/library/windows/desktop/bb174472">IDirect3DDevice9::TestCooperativeLevel</a></p></td>
<td align="left"><p>调用设置了 DXGI_PRESENT_TEST 标志的 <a href="https://msdn.microsoft.com/library/windows/desktop/hh446797">IDXGISwapChain1::Present1</a>。</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="https://msdn.microsoft.com/library/windows/desktop/bb174322">IDirect3DBaseTexture9</a></p>
<p><a href="https://msdn.microsoft.com/library/windows/desktop/bb205909">IDirect3DTexture9</a></p>
<p><a href="https://msdn.microsoft.com/library/windows/desktop/bb174329">IDirect3DCubeTexture9</a></p>
<p><a href="https://msdn.microsoft.com/library/windows/desktop/bb205941">IDirect3DVolumeTexture9</a></p>
<p><a href="https://msdn.microsoft.com/library/windows/desktop/bb205865">IDirect3DIndexBuffer9</a></p>
<p><a href="https://msdn.microsoft.com/library/windows/desktop/bb205915">IDirect3DVertexBuffer9</a></p></td>
<td align="left"><p><a href="https://msdn.microsoft.com/library/windows/desktop/ff476351">ID3D11Buffer</a></p>
<p><a href="https://msdn.microsoft.com/library/windows/desktop/ff476633">ID3D11Texture1D</a></p>
<p><a href="https://msdn.microsoft.com/library/windows/desktop/ff476635">ID3D11Texture2D</a></p>
<p><a href="https://msdn.microsoft.com/library/windows/desktop/ff476637">ID3D11Texture3D</a></p>
<p><a href="https://msdn.microsoft.com/library/windows/desktop/ff476628">ID3D11ShaderResourceView</a></p>
<p><a href="https://msdn.microsoft.com/library/windows/desktop/ff476582">ID3D11RenderTargetView</a></p>
<p><a href="https://msdn.microsoft.com/library/windows/desktop/ff476377">ID3D11DepthStencilView</a></p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="https://msdn.microsoft.com/library/windows/desktop/bb205922">IDirect3DVertexShader9</a></p>
<p><a href="https://msdn.microsoft.com/library/windows/desktop/bb205869">IDirect3DPixelShader9</a></p></td>
<td align="left"><p><a href="https://msdn.microsoft.com/library/windows/desktop/ff476641">ID3D11VertexShader</a></p>
<p><a href="https://msdn.microsoft.com/library/windows/desktop/ff476576">ID3D11PixelShader</a></p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="https://msdn.microsoft.com/library/windows/desktop/bb205919">IDirect3DVertexDeclaration9</a></p></td>
<td align="left"><p><a href="https://msdn.microsoft.com/library/windows/desktop/ff476575">ID3D11InputLayout</a></p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="https://msdn.microsoft.com/library/windows/desktop/bb205805">IDirect3DDevice9::SetRenderState</a></p>
<p><a href="https://msdn.microsoft.com/library/windows/desktop/bb205806">IDirect3DDevice9::SetSamplerState</a></p></td>
<td align="left"><p><a href="https://msdn.microsoft.com/library/windows/desktop/hh404571">ID3D11BlendState1</a></p>
<p><a href="https://msdn.microsoft.com/library/windows/desktop/ff476375">ID3D11DepthStencilState</a></p>
<p><a href="https://msdn.microsoft.com/library/windows/desktop/hh446828">ID3D11RasterizerState1</a></p>
<p><a href="https://msdn.microsoft.com/library/windows/desktop/ff476588">ID3D11SamplerState</a></p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="https://msdn.microsoft.com/library/windows/desktop/bb174369">IDirect3DDevice9::DrawIndexedPrimitive</a></p>
<p><a href="https://msdn.microsoft.com/library/windows/desktop/bb174371">IDirect3DDevice9::DrawPrimitive</a></p></td>
<td align="left"><p><a href="https://msdn.microsoft.com/library/windows/desktop/ff476407">ID3D11DeviceContext::Draw</a></p>
<p><a href="https://msdn.microsoft.com/library/windows/desktop/ff476409">ID3D11DeviceContext::DrawIndexed</a></p>
<p><a href="https://msdn.microsoft.com/library/windows/desktop/bb173566">ID3D11DeviceContext::DrawIndexedInstanced</a></p>
<p><a href="https://msdn.microsoft.com/library/windows/desktop/bb173567">ID3D11DeviceContext::DrawInstanced</a></p>
<p><a href="https://msdn.microsoft.com/library/windows/desktop/bb173590">ID3D11DeviceContext::IASetPrimitiveTopology</a></p>
<p><a href="https://msdn.microsoft.com/library/windows/desktop/bb173564">ID3D11DeviceContext::DrawAuto</a></p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="https://msdn.microsoft.com/library/windows/desktop/bb174350">IDirect3DDevice9::BeginScene</a></p>
<p><a href="https://msdn.microsoft.com/library/windows/desktop/bb174375">IDirect3DDevice9::EndScene</a></p>
<p><a href="https://msdn.microsoft.com/library/windows/desktop/bb174372">IDirect3DDevice9::DrawPrimitiveUP</a></p>
<p><a href="https://msdn.microsoft.com/library/windows/desktop/bb174370">IDirect3DDevice9::DrawIndexedPrimitiveUP</a></p></td>
<td align="left"><p>无直接等效项</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="https://msdn.microsoft.com/library/windows/desktop/bb174470">IDirect3DDevice9::ShowCursor</a></p>
<p><a href="https://msdn.microsoft.com/library/windows/desktop/bb174429">IDirect3DDevice9::SetCursorPosition</a></p>
<p><a href="https://msdn.microsoft.com/library/windows/desktop/bb174430">IDirect3DDevice9::SetCursorProperties</a></p></td>
<td align="left"><p>使用标准光标 API。</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="https://msdn.microsoft.com/library/windows/desktop/bb174425">IDirect3DDevice9::Reset</a></p></td>
<td align="left"><p>LOST 设备和 POOL_MANAGED 不再存在。 <a href="https://msdn.microsoft.com/library/windows/desktop/hh446797">IDXGISwapChain1::Present1</a> 可能失败，并带有 <a href="https://msdn.microsoft.com/library/windows/desktop/bb509553">DXGI_ERROR_DEVICE_REMOVED</a> 返回值。</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="https://msdn.microsoft.com/library/windows/desktop/bb174373">IDirect3DDevice9:DrawRectPatch</a></p>
<p><a href="https://msdn.microsoft.com/library/windows/desktop/bb174374">IDirect3DDevice9:DrawTriPatch</a></p>
<p><a href="https://msdn.microsoft.com/library/windows/desktop/bb174421">IDirect3DDevice9:LightEnable</a></p>
<p><a href="https://msdn.microsoft.com/library/windows/desktop/bb174422">IDirect3DDevice9:MultiplyTransform</a></p>
<p><a href="https://msdn.microsoft.com/library/windows/desktop/bb205798">IDirect3DDevice9:SetLight</a></p>
<p><a href="https://msdn.microsoft.com/library/windows/desktop/bb174437">IDirect3DDevice9:SetMaterial</a></p>
<p><a href="https://msdn.microsoft.com/library/windows/desktop/bb174438">IDirect3DDevice9:SetNPatchMode</a></p>
<p><a href="https://msdn.microsoft.com/library/windows/desktop/bb174463">IDirect3DDevice9:SetTransform</a></p>
<p><a href="https://msdn.microsoft.com/library/windows/desktop/bb174433">IDirect3DDevice9:SetFVF</a></p>
<p><a href="https://msdn.microsoft.com/library/windows/desktop/bb174462">IDirect3DDevice9:SetTextureStageState</a></p></td>
<td align="left"><p>固定函数管道已被弃用。</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="https://msdn.microsoft.com/library/windows/desktop/bb174308">IDirect3DDevice9:CheckDepthStencilMatch</a></p>
<p><a href="https://msdn.microsoft.com/library/windows/desktop/bb174309">IDirect3DDevice9:CheckDeviceFormat</a></p>
<p><a href="https://msdn.microsoft.com/library/windows/desktop/bb174320">IDirect3DDevice9:GetDeviceCaps</a></p>
<p><a href="https://msdn.microsoft.com/library/windows/desktop/bb205859">IDirect3DDevice9:ValidateDevice</a></p></td>
<td align="left"><p>功能位已被替换为功能级别。 对于任何给定的功能级别，只有几个格式和功能用例是可选的。 这些可以使用 <a href="https://msdn.microsoft.com/library/windows/desktop/ff476497">ID3D11Device::CheckFeatureSupport</a> 和 <a href="https://msdn.microsoft.com/library/windows/desktop/bb173536">ID3D11Device::CheckFormatSupport</a> 进行检查。</p></td>
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
<strong>注意</strong>使用器.r 重排将红色复制到其他组件以获取 Direct3D 9 行为的着色器中。
</div>
<div>
 
</div></td>
</tr>
<tr class="even">
<td align="left"><p>D3DFMT_A8L8</p></td>
<td align="left"><p>DXGI_FORMAT_R8G8_UNORM</p>
<div class="alert">
<strong>注意</strong>使用着色器中的重排.rrrg 复制红色并将绿色移动到 alpha 组件以获取 Direct3D 9 行为。
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
<strong>注意</strong>在 Direct3D 9 中，数据被放大了 255.0 f 倍，但可以在着色器中对此进行处理。
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
<strong>注意</strong>在 Direct3D 9 中，数据被放大了 255.0 f 倍，但可以在着色器中对此进行处理。
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
<strong>注意</strong>来看，DXT1 和 DXT2 是相同的从 API/硬件的角度。 唯一的差别是是否使用了预乘 alpha，这可以通过应用程序来跟踪，并且不需要单独的格式。
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
<strong>注意</strong>来看，DXT3 和 DXT4 是相同的从 API/硬件的角度。 唯一的差别是是否使用了预乘 alpha，这可以通过应用程序来跟踪，并且不需要单独的格式。
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
<strong>注意</strong>使用器.r 重排将红色复制到其他组件以获取 D3D9 行为的着色器中。
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
<strong>注意</strong>着色器获取 UINT 值，但如果 Direct3D 9 样式的整型需要浮点数 （0.0 f、 1.0 f...255.f)，UINT 只可以转换为 float32 着色器中。
</div>
<div>
 
</div></td>
</tr>
<tr class="odd">
<td align="left"><p>D3DDECLTYPE_SHORT2</p></td>
<td align="left"><p>DXGI_FORMAT_R16G16_SINT</p>
<div class="alert">
<strong>注意</strong>着色器获取 SINT 值，但如果需要 Direct3D 9 样式整型浮点数，则只需将 SINT 转换为 float32 着色器中。
</div>
<div>
 
</div></td>
</tr>
<tr class="even">
<td align="left"><p>D3DDECLTYPE_SHORT4</p></td>
<td align="left"><p>DXGI_FORMAT_R16G16B16A16_SINT</p>
<div class="alert">
<strong>注意</strong>着色器获取 SINT 值，但如果需要 Direct3D 9 样式整型浮点数，则只需将 SINT 转换为 float32 着色器中。
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
<strong>注意</strong>要求功能级别 10.0 或更高版本
</div>
<div>
 
</div></td>
</tr>
<tr class="odd">
<td align="left"><p>FourCC 'ATI2'</p></td>
<td align="left"><p>DXGI_FORMAT_BC5_UNORM</p>
<div class="alert">
<strong>注意</strong>要求功能级别 10.0 或更高版本
</div>
<div>
 
</div></td>
</tr>
</tbody>
</table>

 

 

 




