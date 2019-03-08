---
title: 通用 Windows 平台 (UWP) 应用中的多重采样
description: 了解如何在使用 Direct3D 生成的通用 Windows 平台 (UWP) 应用中使用多重采样。
ms.assetid: 1cd482b8-32ff-1eb0-4c91-83eb52f08484
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, 游戏, 多重采样, direct3d
ms.localizationpriority: medium
ms.openlocfilehash: 0c1634af8589a97f5070ff85909fe12ab16bf8d6
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2019
ms.locfileid: "57610852"
---
# <a name="span-iddevgamingmultisamplingmulti-sampleantialiasinginwindowsstoreappsspan-multisampling-in-universal-windows-platform-uwp-apps"></a><span id="dev_gaming.multisampling__multi-sample_anti_aliasing__in_windows_store_apps"></span> 多重采样中通用 Windows 平台 (UWP) 应用程序



了解如何在使用 Direct3D 生成的通用 Windows 平台 (UWP) 应用中使用多重采样。 多重采样（也称为多重采样抗锯齿）是一种图形技术，用于减少锯齿边缘的显示。 它的工作方式是绘制比最终渲染目标中包含的实际像素更多的像素，然后取平均值以保留某些像素的“局部”边缘的显示。 有关多重采样在 Direct3D 中的实际工作方式的详细说明，请参阅[多重采样抗锯齿光栅化规则](https://msdn.microsoft.com/library/windows/desktop/cc627092#Multisample)。

## <a name="multisampling-and-the-flip-model-swap-chain"></a>多重采样和翻转模型交换链


使用 DirectX 的 UWP 应用必须使用翻转模型交换链。 翻转模型交换链不直接支持多重采样，但仍然可以使用不同方式应用多重采样，方法是将场景渲染到多重采样的渲染目标视图，然后在呈现前将多重采样的渲染目标解析到后台缓冲区。 本文介绍将多重采样添加到 UWP 应用所需的步骤。

### <a name="how-to-use-multisampling"></a>如何使用多重采样

Direct3D 功能级别保证支持特定的最小样本计数功能，并保证某些支持多重采样的缓冲区格式可用。 图形设备支持的格式和样本计数通常多于最低要求。 通过检查具有特定 DXGI 格式的多重采样支持功能，然后检查可以与每个支持的格式一起使用的样本计数，你可以在运行时确定多重采样支持。

1.  调用 [**ID3D11Device::CheckFeatureSupport**](https://msdn.microsoft.com/library/windows/desktop/ff476497) 以了解哪些 DXGI 格式可以用于多重采用。 提供游戏可以使用的呈现目标格式。 呈现器目标和解析目标必须使用相同的格式，因此请查看两个[ **D3D11\_格式\_支持\_MULTISAMPLE\_RENDERTARGET** ](https://msdn.microsoft.com/library/windows/desktop/ff476134)并**D3D11\_格式\_支持\_MULTISAMPLE\_解决**。

    **功能级别 9:  **尽管功能级别 9 的设备[保证支持多级采样呈现器目标格式](https://msdn.microsoft.com/library/windows/desktop/ff471324#MultiSample_RenderTarget)，支持多重采样解析目标不能保证。 因此，在尝试使用本主题所述的多重采样技术之前，此检查是必要的。

    下面的代码检查所有 dxgi 多级取样支持\_格式值：

    ```cpp
    // Determine the format support for multisampling.
    for (UINT i = 1; i < DXGI_FORMAT_MAX; i++)
    {
        DXGI_FORMAT inFormat = safe_cast<DXGI_FORMAT>(i);
        UINT formatSupport = 0;
        HRESULT hr = m_d3dDevice->CheckFormatSupport(inFormat, &formatSupport);

        if ((formatSupport & D3D11_FORMAT_SUPPORT_MULTISAMPLE_RESOLVE) &&
            (formatSupport & D3D11_FORMAT_SUPPORT_MULTISAMPLE_RENDERTARGET)
            )
        {
            m_supportInfo->SetFormatSupport(i, true);
        }
        else
        {
            m_supportInfo->SetFormatSupport(i, false);
        }
    }
    ```

2.  对于每个支持的格式，通过调用 [**ID3D11Device::CheckMultisampleQualityLevels**](https://msdn.microsoft.com/library/windows/desktop/ff476499) 查询样本计数支持。

    以下代码将检查受支持的 DXGI 格式的样本大小支持：

    ```cpp
    // Find available sample sizes for each supported format.
    for (unsigned int i = 0; i < DXGI_FORMAT_MAX; i++)
    {
        for (unsigned int j = 1; j < MAX_SAMPLES_CHECK; j++)
        {
            UINT numQualityFlags;

            HRESULT test = m_d3dDevice->CheckMultisampleQualityLevels(
                (DXGI_FORMAT) i,
                j,
                &numQualityFlags
                );

            if (SUCCEEDED(test) && (numQualityFlags > 0))
            {
                m_supportInfo->SetSampleSize(i, j, 1);
                m_supportInfo->SetQualityFlagsAt(i, j, numQualityFlags);
            }
        }
    }
    ```

    > **请注意**  使用[ **ID3D11Device2::CheckMultisampleQualityLevels1** ](https://msdn.microsoft.com/library/windows/desktop/dn280494)改为如果您需要检查多重采样支持的平铺的资源缓冲区。

     

3.  创建具有所需样本计数的缓冲区和呈现目标视图。 使用相同的 DXGI\_格式、 宽度和高度为交换链，但指定抽样计数大于 1，并使用多级采样的纹理维度 (**D3D11\_RTV\_维度\_TEXTURE2DMS**为例)。 如果必要，你可以使用为多重采样优化的新设置重新创建交换链。

    以下代码将创建多重采样渲染目标：

    ```cpp
    float widthMulti = m_d3dRenderTargetSize.Width;
    float heightMulti = m_d3dRenderTargetSize.Height;

    D3D11_TEXTURE2D_DESC offScreenSurfaceDesc;
    ZeroMemory(&offScreenSurfaceDesc, sizeof(D3D11_TEXTURE2D_DESC));

    offScreenSurfaceDesc.Format = DXGI_FORMAT_B8G8R8A8_UNORM;
    offScreenSurfaceDesc.Width = static_cast<UINT>(widthMulti);
    offScreenSurfaceDesc.Height = static_cast<UINT>(heightMulti);
    offScreenSurfaceDesc.BindFlags = D3D11_BIND_RENDER_TARGET;
    offScreenSurfaceDesc.MipLevels = 1;
    offScreenSurfaceDesc.ArraySize = 1;
    offScreenSurfaceDesc.SampleDesc.Count = m_sampleSize;
    offScreenSurfaceDesc.SampleDesc.Quality = m_qualityFlags;

    // Create a surface that's multisampled.
    DX::ThrowIfFailed(
        m_d3dDevice->CreateTexture2D(
        &offScreenSurfaceDesc,
        nullptr,
        &m_offScreenSurface)
        );

    // Create a render target view. 
    CD3D11_RENDER_TARGET_VIEW_DESC renderTargetViewDesc(D3D11_RTV_DIMENSION_TEXTURE2DMS);
    DX::ThrowIfFailed(
        m_d3dDevice->CreateRenderTargetView(
        m_offScreenSurface.Get(),
        &renderTargetViewDesc,
        &m_d3dRenderTargetView
        )
        );
    ```

4.  深度缓冲区必须具有相同的宽度、高度、样本计数和纹理大小以匹配多重采样渲染目标。

    以下代码将创建多重采样深度缓冲区：

    ```cpp
    // Create a depth stencil view for use with 3D rendering if needed.
    CD3D11_TEXTURE2D_DESC depthStencilDesc(
        DXGI_FORMAT_D24_UNORM_S8_UINT,
        static_cast<UINT>(widthMulti),
        static_cast<UINT>(heightMulti),
        1, // This depth stencil view has only one texture.
        1, // Use a single mipmap level.
        D3D11_BIND_DEPTH_STENCIL,
        D3D11_USAGE_DEFAULT,
        0,
        m_sampleSize,
        m_qualityFlags
        );

    ComPtr<ID3D11Texture2D> depthStencil;
    DX::ThrowIfFailed(
        m_d3dDevice->CreateTexture2D(
        &depthStencilDesc,
        nullptr,
        &depthStencil
        )
        );

    CD3D11_DEPTH_STENCIL_VIEW_DESC depthStencilViewDesc(D3D11_DSV_DIMENSION_TEXTURE2DMS);
    DX::ThrowIfFailed(
        m_d3dDevice->CreateDepthStencilView(
        depthStencil.Get(),
        &depthStencilViewDesc,
        &m_d3dDepthStencilView
        )
        );
    ```

5.  现在是创建视口的最佳时间，因为视口宽度和高度必须也匹配渲染目标。

    以下代码将创建视口：

    ```cpp
    // Set the 3D rendering viewport to target the entire window.
    m_screenViewport = CD3D11_VIEWPORT(
        0.0f,
        0.0f,
        widthMulti / m_scalingFactor,
        heightMulti / m_scalingFactor
        );

    m_d3dContext->RSSetViewports(1, &m_screenViewport);
    ```

6.  将每个帧渲染到多重采样渲染目标。 完成渲染后，先调用 [**ID3D11DeviceContext::ResolveSubresource**](https://msdn.microsoft.com/library/windows/desktop/ff476474)，然后再呈现帧。 此操作可指导 Direct3D 执行多重采样操作、计算每个像素值以供显示，并将结果放置在后台缓冲区中。 然后，后台缓冲区可包含最终抗锯齿图像并可供呈现。

    以下代码在呈现帧之前将解析子资源：

    ```cpp
    if (m_sampleSize > 1)
    {
        unsigned int sub = D3D11CalcSubresource(0, 0, 1);

        m_d3dContext->ResolveSubresource(
            m_backBuffer.Get(),
            sub,
            m_offScreenSurface.Get(),
            sub,
            DXGI_FORMAT_B8G8R8A8_UNORM
            );
    }

    // The first argument instructs DXGI to block until VSync, putting the application
    // to sleep until the next VSync. This ensures that we don't waste any cycles rendering
    // frames that will never be displayed to the screen.
    hr = m_swapChain->Present(1, 0);
    ```

 

 




