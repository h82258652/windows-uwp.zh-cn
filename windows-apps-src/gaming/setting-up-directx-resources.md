---
author: mtoepke
title: 设置 DirectX 资源和显示图像
description: 下面我们将为你介绍如何创建 Direct3D 设备、交换链和呈现目标视图，以及如何向屏幕显示呈现的图像。
ms.assetid: d54d96fe-3522-4acb-35f4-bb11c3a5b064
ms.author: mtoepke
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, uwp, 游戏, directx, 资源, 图像
ms.localizationpriority: medium
ms.openlocfilehash: 24fd038bdd447491da43e5d5803445d00147ba2d
ms.sourcegitcommit: 753e0a7160a88830d9908b446ef0907cc71c64e7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/29/2018
ms.locfileid: "5746998"
---
# <a name="set-up-directx-resources-and-display-an-image"></a>设置 DirectX 资源和显示图像



下面我们将为你介绍如何创建 Direct3D 设备、交换链和呈现目标视图，以及如何向屏幕显示呈现的图像。

**目标：** 在 C++ 通用 Windows 平台 (UWP) 应用中设置 DirectX 资源并显示纯色。

## <a name="prerequisites"></a>先决条件


我们假定你熟悉 C++。 你还需要具有图形编程概念方面的基本经验。

**完成所需时间：** 20 分钟。

## <a name="instructions"></a>说明

### <a name="1-declaring-direct3d-interface-variables-with-comptr"></a>1. 使用 ComPtr 声明 Direct3D 接口变量

我们使用 Windows 运行时 C++ 模板库 (WRL) 中的 ComPtr [智能指针](https://msdn.microsoft.com/library/windows/apps/hh279674.aspx)模板来声明 Direct3D 接口变量，以便可以采用防异常的方式管理这些变量的生存时间。 然后，使用这些变量访问 [**ComPtr class**](https://msdn.microsoft.com/library/windows/apps/br244983.aspx) 及其成员。 例如：

```cpp
    ComPtr<ID3D11RenderTargetView> m_renderTargetView;
    m_d3dDeviceContext->OMSetRenderTargets(
        1,
        m_renderTargetView.GetAddressOf(),
        nullptr // Use no depth stencil.
        );
```

如果使用 ComPtr 声明 [**ID3D11RenderTargetView**](https://msdn.microsoft.com/library/windows/desktop/ff476582)，则可以接着使用 ComPtr 的 **GetAddressOf** 方法来获取指向 **ID3D11RenderTargetView** (\*\*ID3D11RenderTargetView) 的指针的地址以传递给 [**ID3D11DeviceContext::OMSetRenderTargets**](https://msdn.microsoft.com/library/windows/desktop/ff476464)。 **OMSetRenderTargets** 会将呈现器目标绑定到[输出合并阶段](https://msdn.microsoft.com/library/windows/desktop/bb205120)，以将呈现器目标指定为输出目标。

在启动示例应用之后，该示例应用将初始化并加载，然后就可以运行了。

### <a name="2-creating-the-direct3d-device"></a>2. 创建 Direct3D 设备

若要使用 Direct3D API 来呈现场景，必须首先创建一个代表显示适配器的 Direct3D 设备。 要创建 Direct3D 设备，需要调用 [**D3D11CreateDevice**](https://msdn.microsoft.com/library/windows/desktop/ff476082) 函数。 在 [**D3D\_FEATURE\_LEVEL**](https://msdn.microsoft.com/library/windows/desktop/ff476329) 值的数组中指定级别 9.1 至 11.1。 Direct3D 按顺序检查数组并返回支持的最高功能级别。 因此，为了获取可用的最高级功能级别，我们将 **D3D\_FEATURE\_LEVEL** 数组条目按从高到低的顺序列出。 将 [**D3D11\_CREATE\_DEVICE\_BGRA\_SUPPORT**](https://msdn.microsoft.com/library/windows/desktop/ff476107#D3D11_CREATE_DEVICE_BGRA_SUPPORT) 标记传递到 *Flags* 参数，使 Direct3D 资源与 Direct2D 交互操作。 如果使用调试版本，则还需传递 [**D3D11\_CREATE\_DEVICE\_DEBUG**](https://msdn.microsoft.com/library/windows/desktop/ff476107#D3D11_CREATE_DEVICE_DEBUG) 标记。 有关调试应用的详细信息，请参阅[使用调试层来调试应用](https://msdn.microsoft.com/library/windows/desktop/jj200584)。

通过查询从 [**D3D11CreateDevice**](https://msdn.microsoft.com/library/windows/desktop/ff476082) 返回的 Direct3D 11 设备和设备上下文来获取 Direct3D 11.1 设备 ([**ID3D11Device1**](https://msdn.microsoft.com/library/windows/desktop/hh404575)) 和设备上下文 ([**ID3D11DeviceContext1**](https://msdn.microsoft.com/library/windows/desktop/hh404598))。

```cpp
        // First, create the Direct3D device.

        // This flag is required in order to enable compatibility with Direct2D.
        UINT creationFlags = D3D11_CREATE_DEVICE_BGRA_SUPPORT;

#if defined(_DEBUG)
        // If the project is in a debug build, enable debugging via SDK Layers with this flag.
        creationFlags |= D3D11_CREATE_DEVICE_DEBUG;
#endif

        // This array defines the ordering of feature levels that D3D should attempt to create.
        D3D_FEATURE_LEVEL featureLevels[] =
        {
            D3D_FEATURE_LEVEL_11_1,
            D3D_FEATURE_LEVEL_11_0,
            D3D_FEATURE_LEVEL_10_1,
            D3D_FEATURE_LEVEL_10_0,
            D3D_FEATURE_LEVEL_9_3,
            D3D_FEATURE_LEVEL_9_1
        };

        ComPtr<ID3D11Device> d3dDevice;
        ComPtr<ID3D11DeviceContext> d3dDeviceContext;
        DX::ThrowIfFailed(
            D3D11CreateDevice(
                nullptr,                    // Specify nullptr to use the default adapter.
                D3D_DRIVER_TYPE_HARDWARE,
                nullptr,                    // leave as nullptr if hardware is used
                creationFlags,              // optionally set debug and Direct2D compatibility flags
                featureLevels,
                ARRAYSIZE(featureLevels),
                D3D11_SDK_VERSION,          // always set this to D3D11_SDK_VERSION
                &d3dDevice,
                nullptr,
                &d3dDeviceContext
                )
            );

        // Retrieve the Direct3D 11.1 interfaces.
        DX::ThrowIfFailed(
            d3dDevice.As(&m_d3dDevice)
            );

        DX::ThrowIfFailed(
            d3dDeviceContext.As(&m_d3dDeviceContext)
            );
```

### <a name="3-creating-the-swap-chain"></a>3. 创建交换链

接下来，创建设备用于呈现和显示的交换链。 声明并初始化 [**DXGI\_SWAP\_CHAIN\_DESC1**](https://msdn.microsoft.com/library/windows/desktop/hh404528) 结构来描述该交换链。 接着，将该交换链设置为翻转模型（即，在 **SwapEffect** 成员中设置 [**DXGI\_SWAP\_EFFECT\_FLIP\_SEQUENTIAL**](https://msdn.microsoft.com/library/windows/desktop/bb173077#DXGI_SWAP_EFFECT_FLIP_SEQUENTIAL) 值的交换链）并将 **Format** 成员设置为 [**DXGI\_FORMAT\_B8G8R8A8\_UNORM**](https://msdn.microsoft.com/library/windows/desktop/bb173059#DXGI_FORMAT_B8G8R8A8_UNORM)。 将 **SampleDesc** 成员指定的 [**DXGI\_SAMPLE\_DESC**](https://msdn.microsoft.com/library/windows/desktop/bb173072) 结构的 **Count** 成员设置为 1，并将 **DXGI\_SAMPLE\_DESC** 的 **Quality** 成员设置为 0，因为翻转模型不支持多重采样抗锯齿 (MSAA)。 将 **BufferCount** 成员设置为 2，让交换链可以使用前台缓冲区来向显示设备显示，并使用后台缓冲区充当呈现器目标。

通过查询 Direct3D 11.1 设备来获取基本 DXGI 设备。 为了最大程度降低电耗（对于笔记本电脑和平板电脑等使用电池供电的设备，这样做很重要），将 1 作为 DXGI 可排队的后台缓冲区帧的最大数量来调用 [**IDXGIDevice1::SetMaximumFrameLatency**](https://msdn.microsoft.com/library/windows/desktop/ff471334) 方法。 这样可确保仅在垂直空白之后才呈现应用。

最后，要创建交换链，需要从 DXGI 设备获取父工厂。 调用 [**IDXGIDevice::GetAdapter**](https://msdn.microsoft.com/library/windows/desktop/bb174531) 来获取设备的适配器，然后在适配器上调用 [**IDXGIObject::GetParent**](https://msdn.microsoft.com/library/windows/desktop/bb174542) 来获取父工厂 ([**IDXGIFactory2**](https://msdn.microsoft.com/library/windows/desktop/hh404556))。 要创建交换链，需要使用交换链描述符和应用的核心窗口来调用 [**IDXGIFactory2::CreateSwapChainForCoreWindow**](https://msdn.microsoft.com/library/windows/desktop/hh404559)。

```cpp
            // If the swap chain does not exist, create it.
            DXGI_SWAP_CHAIN_DESC1 swapChainDesc = {0};

            swapChainDesc.Stereo = false;
            swapChainDesc.BufferUsage = DXGI_USAGE_RENDER_TARGET_OUTPUT;
            swapChainDesc.Scaling = DXGI_SCALING_NONE;
            swapChainDesc.Flags = 0;

            // Use automatic sizing.
            swapChainDesc.Width = 0;
            swapChainDesc.Height = 0;

            // This is the most common swap chain format.
            swapChainDesc.Format = DXGI_FORMAT_B8G8R8A8_UNORM;

            // Don't use multi-sampling.
            swapChainDesc.SampleDesc.Count = 1;
            swapChainDesc.SampleDesc.Quality = 0;

            // Use two buffers to enable the flip effect.
            swapChainDesc.BufferCount = 2;

            // We recommend using this swap effect for all applications.
            swapChainDesc.SwapEffect = DXGI_SWAP_EFFECT_FLIP_SEQUENTIAL;


            // Once the swap chain description is configured, it must be
            // created on the same adapter as the existing D3D Device.

            // First, retrieve the underlying DXGI Device from the D3D Device.
            ComPtr<IDXGIDevice2> dxgiDevice;
            DX::ThrowIfFailed(
                m_d3dDevice.As(&dxgiDevice)
                );

            // Ensure that DXGI does not queue more than one frame at a time. This both reduces
            // latency and ensures that the application will only render after each VSync, minimizing
            // power consumption.
            DX::ThrowIfFailed(
                dxgiDevice->SetMaximumFrameLatency(1)
                );

            // Next, get the parent factory from the DXGI Device.
            ComPtr<IDXGIAdapter> dxgiAdapter;
            DX::ThrowIfFailed(
                dxgiDevice->GetAdapter(&dxgiAdapter)
                );

            ComPtr<IDXGIFactory2> dxgiFactory;
            DX::ThrowIfFailed(
                dxgiAdapter->GetParent(IID_PPV_ARGS(&dxgiFactory))
                );

            // Finally, create the swap chain.
            CoreWindow^ window = m_window.Get();
            DX::ThrowIfFailed(
                dxgiFactory->CreateSwapChainForCoreWindow(
                    m_d3dDevice.Get(),
                    reinterpret_cast<IUnknown*>(window),
                    &swapChainDesc,
                    nullptr, // Allow on all displays.
                    &m_swapChain
                    )
                );
```

### <a name="4-creating-the-render-target-view"></a>4. 创建呈现器目标视图

要将图形呈现到窗口，需要创建一个呈现器目标视图。 调用 [**IDXGISwapChain::GetBuffer**](https://msdn.microsoft.com/library/windows/desktop/bb174570) 来获取交换链接的后台缓冲区，以便在创建呈现器目标视图时使用。 将后台缓冲区指定为一个 2D 纹理 ([**ID3D11Texture2D**](https://msdn.microsoft.com/library/windows/desktop/ff476635))。 要创建呈现器目标视图，需要使用交换链的后台缓冲区调用 [**ID3D11Device::CreateRenderTargetView**](https://msdn.microsoft.com/library/windows/desktop/ff476517)。 通过将视区 ([**D3D11\_VIEWPORT**](https://msdn.microsoft.com/library/windows/desktop/ff476260)) 指定为交换链的后台缓冲区的最大大小，指定绘制到整个核心窗口。 在对 [**ID3D11DeviceContext::RSSetViewports**](https://msdn.microsoft.com/library/windows/desktop/ff476480) 的调用中使用视区将视区绑定到管道的[光栅化阶段](https://msdn.microsoft.com/library/windows/desktop/bb205125)。 光栅化阶段将向量信息转换为光栅图像。 此情况下不要求转换，因为我们只是显示纯色。

```cpp
        // Once the swap chain is created, create a render target view.  This will
        // allow Direct3D to render graphics to the window.

        ComPtr<ID3D11Texture2D> backBuffer;
        DX::ThrowIfFailed(
            m_swapChain->GetBuffer(0, IID_PPV_ARGS(&backBuffer))
            );

        DX::ThrowIfFailed(
            m_d3dDevice->CreateRenderTargetView(
                backBuffer.Get(),
                nullptr,
                &m_renderTargetView
                )
            );


        // After the render target view is created, specify that the viewport,
        // which describes what portion of the window to draw to, should cover
        // the entire window.

        D3D11_TEXTURE2D_DESC backBufferDesc = {0};
        backBuffer->GetDesc(&backBufferDesc);

        D3D11_VIEWPORT viewport;
        viewport.TopLeftX = 0.0f;
        viewport.TopLeftY = 0.0f;
        viewport.Width = static_cast<float>(backBufferDesc.Width);
        viewport.Height = static_cast<float>(backBufferDesc.Height);
        viewport.MinDepth = D3D11_MIN_DEPTH;
        viewport.MaxDepth = D3D11_MAX_DEPTH;

        m_d3dDeviceContext->RSSetViewports(1, &viewport);
```

### <a name="5-presenting-the-rendered-image"></a>5. 显示呈现的图像

我们将进入一个不断呈现和显示场景的无限循环。

在此循环中，我们调用：

1.  [**ID3D11DeviceContext::OMSetRenderTargets**](https://msdn.microsoft.com/library/windows/desktop/ff476464) 以将呈现器目标指定为输出目标。
2.  [**ID3D11DeviceContext::ClearRenderTargetView**](https://msdn.microsoft.com/library/windows/desktop/ff476388) 以将呈现目标清除为纯色。
3.  [**IDXGISwapChain::Present**](https://msdn.microsoft.com/library/windows/desktop/bb174576) 以向窗口显示呈现的图像。

由于我们之前将最大帧延迟设置为 1，因此，Windows 通常会将呈现循环减慢至屏幕刷新速率，通常大约为 60 Hz。 Windows 通过在应用调用 [**Present**](https://msdn.microsoft.com/library/windows/desktop/bb174576) 时使应用进入睡眠状态来减慢呈现循环。 在刷新屏幕之前，Windows 会将应用保持为睡眠状态。

```cpp
        // Enter the render loop.  Note that UWP apps should never exit.
        while (true)
        {
            // Process events incoming to the window.
            m_window->Dispatcher->ProcessEvents(CoreProcessEventsOption::ProcessAllIfPresent);

            // Specify the render target we created as the output target.
            m_d3dDeviceContext->OMSetRenderTargets(
                1,
                m_renderTargetView.GetAddressOf(),
                nullptr // Use no depth stencil.
                );

            // Clear the render target to a solid color.
            const float clearColor[4] = { 0.071f, 0.04f, 0.561f, 1.0f };
            m_d3dDeviceContext->ClearRenderTargetView(
                m_renderTargetView.Get(),
                clearColor
                );

            // Present the rendered image to the window.  Because the maximum frame latency is set to 1,
            // the render loop will generally be throttled to the screen refresh rate, typically around
            // 60 Hz, by sleeping the application on Present until the screen is refreshed.
            DX::ThrowIfFailed(
                m_swapChain->Present(1, 0)
                );
        }
```

### <a name="6-resizing-the-app-window-and-the-swap-chains-buffer"></a>6. 调整应用窗口和交换链的缓冲区的大小

如果应用窗口大小发生改变，则应用必须调整交换链的缓冲区的大小，重新创建呈现器目标视图，然后显示大小调整后的呈现图像。 要调整交换链缓冲区的大小，需要调用 [**IDXGISwapChain::ResizeBuffers**](https://msdn.microsoft.com/library/windows/desktop/bb174577)。 在此调用中，保持缓冲区的数量和缓冲区的格式不变（将 *BufferCount* 参数设置为 2，并将 *NewFormat* 参数设置为 [**DXGI\_FORMAT\_B8G8R8A8\_UNORM**](https://msdn.microsoft.com/library/windows/desktop/bb173059#DXGI_FORMAT_B8G8R8A8_UNORM)）。 使交换链的后台缓冲区的大小与调整后的窗口大小相同。 在调整了交换链的缓冲区的大小之后，创建新的呈现器目标并显示新呈现的图像（与初始化应用时类似）。

```cpp
            // If the swap chain already exists, resize it.
            DX::ThrowIfFailed(
                m_swapChain->ResizeBuffers(
                    2,
                    0,
                    0,
                    DXGI_FORMAT_B8G8R8A8_UNORM,
                    0
                    )
                );
```

## <a name="summary-and-next-steps"></a>摘要和后续步骤


我们创建了一个 Direct3D 设备、交换链和呈现器目标视图，并向显示器显示了呈现的图像。

接下来，我们还要在屏幕上绘制一个三角形。

[创建着色器和绘制基元](creating-shaders-and-drawing-primitives.md)

 

 




