---
title: 呈现简介
description: 了解如何开发呈现管道以显示图形。 呈现简介。
ms.assetid: 1da3670b-2067-576f-da50-5eba2f88b3e6
ms.date: 10/24/2017
ms.topic: article
keywords: windows 10, uwp, 游戏, 呈现
ms.localizationpriority: medium
ms.openlocfilehash: d1abb324c5e9e16babbbf8d3650adc39cb995137
ms.sourcegitcommit: 20969781aca50738792631f4b68326f9171a3980
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/26/2020
ms.locfileid: "85409636"
---
# <a name="rendering-framework-i-intro-to-rendering"></a>呈现框架 I：呈现简介

> [!NOTE]
> 本主题是[使用 DirectX 教程系列创建简单通用 Windows 平台（UWP）游戏](tutorial--create-your-first-uwp-directx-game.md)的一部分。 该链接上的主题设置了序列的上下文。

到目前为止，我们已经介绍了如何构建通用 Windows 平台（UWP）游戏，以及如何定义一台状态机来处理游戏的流动。 现在，可以学习如何开发呈现框架了。 让我们看看示例游戏如何使用 Direct3D 11 呈现游戏场景。

Direct3D 11 包含一组 Api，可提供对高性能图形硬件高级功能的访问，这些功能可用于为图形密集型应用程序（如游戏）创建3D 图形。

在屏幕上渲染游戏图形，这基本上就是在屏幕上渲染一系列的帧。 在每个帧中，你必须基于视图呈现场景中可见的对象。

为呈现框架，必须将需要的场景信息传递到硬件，以便它可以显示在屏幕上。 如果你希望在屏幕上显示任何内容，则需要在游戏开始运行后即开始呈现。

## <a name="objectives"></a>目标

设置基本渲染框架以显示 UWP DirectX 游戏的图形输出。 你可以将其分为三个步骤。

1. 建立与图形接口的连接。
2. 创建绘制图形所需的资源。
3. 通过呈现框架来显示图形。

本主题介绍如何呈现图形，涵盖步骤1和3。

[渲染框架 II：游戏渲染](tutorial-game-rendering.md)介绍了步骤 2 &mdash; 如何设置渲染框架，以及如何在进行渲染之前准备数据。

## <a name="get-started"></a>入门

最好先熟悉基本的图形和渲染概念。 如果你不熟悉 Direct3D 和呈示，请参阅有关本主题中使用的图形和呈现术语的简短说明的[术语和概念](#terms-and-concepts)。

对于此游戏， **GameRenderer**类表示此示例游戏的呈现器。 它负责创建和维护用于生成游戏视觉效果的所有 Direct3D 11 和 Direct2D 对象。 它还保留对**Simple3DGame**对象的引用，该对象用于检索要呈现的对象列表，以及用于打印头显示的游戏的状态（HUD）。

在教程的这个部分，我们将重点介绍在游戏中呈现 3D 对象。

## <a name="establish-a-connection-to-the-graphics-interface"></a>与图形界面建立连接

有关访问用于呈现的硬件的信息，请参阅[定义游戏的 UWP 应用框架](tutorial--building-the-games-uwp-app-framework.md#the-appinitialize-method)主题。

### <a name="the-appinitialize-method"></a>App：： Initialize 方法

如下所示， **std：： make_shared**函数用于创建对 DX 的**Shared_ptr** **：:D eviceresources**，这也提供对设备的访问。

在 Direct3D 11 中，[设备](#device)用于分配和销毁对象、呈现原语，并通过图形驱动程序与显卡通信。

```cppwinrt
void Initialize(CoreApplicationView const& applicationView)
{
    ...

    // At this point we have access to the device. 
    // We can create the device-dependent resources.
    m_deviceResources = std::make_shared<DX::DeviceResources>();
}
```

## <a name="display-the-graphics-by-rendering-the-frame"></a>通过呈现帧显示图形

游戏场景需要在游戏启动时呈现。 [**GameMain：： Run**](#gamemainrun-method)方法中的呈现开始说明如下所示。

简单的流程就是这样。

1. **更新**
2. **Render**
3. **存在**

### <a name="gamemainrun-method"></a>GameMain::Run 方法

```cppwinrt
void GameMain::Run()
{
    while (!m_windowClosed)
    {
        if (m_visible) // if the window is visible
        {
            switch (m_updateState)
            {
            ...
            default:
                CoreWindow::GetForCurrentThread().Dispatcher().ProcessEvents(CoreProcessEventsOption::ProcessAllIfPresent);
                Update();
                m_renderer->Render();
                m_deviceResources->Present();
                m_renderNeeded = false;
            }
        }
        else
        {
            CoreWindow::GetForCurrentThread().Dispatcher().ProcessEvents(CoreProcessEventsOption::ProcessOneAndAllPending);
        }
    }
    m_game->OnSuspending();  // Exiting due to window close, so save state.
}
```

### <a name="update"></a>更新

有关如何在[**GameMain：： Update**](tutorial-game-flow-management.md#the-gamemainupdate-method)方法中更新游戏状态的详细信息，请参阅[游戏流管理](tutorial-game-flow-management.md)主题。

### <a name="render"></a>呈现

呈现是通过调用**GameMain：： Run**中的[**GameRenderer：： Render**](#gamerendererrender-method)方法实现的。

如果启用了[立体声渲染](#stereo-rendering)，则有两个渲染刀路， &mdash; 一个用于向左的眼睛，一个用于向右。 在每个呈现通道中，我们将呈现目标和深度模板视图绑定到设备。 我们还会清除之后的深度模板视图。

> [!NOTE]
> 立体呈现可以使用其他方法实现，如使用顶点实例或几何着色器的单通道立体声。 双呈现方法是一种更慢但更方便的方法来实现立体声呈现。

游戏运行并加载资源后，会更新[投影矩阵](#projection-transform-matrix)，每次呈现通过。 每个视图的对象略有不同。 接下来，我们设置[图形渲染管道](#rendering-pipeline)。 

> [!NOTE]
> 请参阅[创建和加载 DirectX 图形资源](tutorial-game-rendering.md#create-and-load-directx-graphic-resources)了解有关如何加载资源的详细信息。

在此示例游戏中，呈现器旨在在所有对象中使用标准顶点布局。 这简化了着色器设计，并允许在着色器之间轻松更改，而与对象的几何图形无关。

#### <a name="gamerendererrender-method"></a>GameRenderer::Render 方法

将 Direct3D 上下文设置为使用输入顶点布局。 输入布局对象介绍了如何将顶点缓冲区数据传输到[呈现管道](#rendering-pipeline)。 

接下来，我们将 Direct3D 上下文设置为使用前面定义的常量缓冲区，这些缓冲区由[顶点着色器](#vertex-shaders-and-pixel-shaders)管道阶段和[像素着色器](#vertex-shaders-and-pixel-shaders)管道阶段使用。 

> [!NOTE]
> 请参阅[呈现框架 II：游戏呈现](tutorial-game-rendering.md)了解有关常量缓冲区定义的详细信息。

由于为管道中的所有着色器使用相同的常量缓冲区输入布局和集，因此每一帧设置一次。

```cppwinrt
void GameRenderer::Render()
{
    bool stereoEnabled{ m_deviceResources->GetStereoState() };

    auto d3dContext{ m_deviceResources->GetD3DDeviceContext() };
    auto d2dContext{ m_deviceResources->GetD2DDeviceContext() };

    int renderingPasses = 1;
    if (stereoEnabled)
    {
        renderingPasses = 2;
    }

    for (int i = 0; i < renderingPasses; i++)
    {
        // Iterate through the number of rendering passes to be completed.
        // 2 rendering passes if stereo is enabled.
        if (i > 0)
        {
            // Doing the Right Eye View.
            ID3D11RenderTargetView* const targets[1] = { m_deviceResources->GetBackBufferRenderTargetViewRight() };

            // Resets render targets to the screen.
            // OMSetRenderTargets binds 2 things to the device.
            // 1. Binds one render target atomically to the device.
            // 2. Binds the depth-stencil view, as returned by the GetDepthStencilView method, to the device.
            // For more info, see
            // https://docs.microsoft.com/windows/win32/api/d3d11/nf-d3d11-id3d11devicecontext-omsetrendertargets

            d3dContext->OMSetRenderTargets(1, targets, m_deviceResources->GetDepthStencilView());

            // Clears the depth stencil view.
            // A depth stencil view contains the format and buffer to hold depth and stencil info.
            // For more info about depth stencil view, go to: 
            // https://docs.microsoft.com/windows/uwp/graphics-concepts/depth-stencil-view--dsv-
            // A depth buffer is used to store depth information to control which areas of 
            // polygons are rendered rather than hidden from view. To learn more about a depth buffer,
            // go to: https://docs.microsoft.com/windows/uwp/graphics-concepts/depth-buffers
            // A stencil buffer is used to mask pixels in an image, to produce special effects. 
            // The mask determines whether a pixel is drawn or not,
            // by setting the bit to a 1 or 0. To learn more about a stencil buffer,
            // go to: https://docs.microsoft.com/windows/uwp/graphics-concepts/stencil-buffers

            d3dContext->ClearDepthStencilView(m_deviceResources->GetDepthStencilView(), D3D11_CLEAR_DEPTH, 1.0f, 0);

            // Direct2D -- discussed later
            d2dContext->SetTarget(m_deviceResources->GetD2DTargetBitmapRight());
        }
        else
        {
            // Doing the Mono or Left Eye View.
            // As compared to the right eye:
            // m_deviceResources->GetBackBufferRenderTargetView instead of GetBackBufferRenderTargetViewRight
            ID3D11RenderTargetView* const targets[1] = { m_deviceResources->GetBackBufferRenderTargetView() };

            // Same as the Right Eye View.
            d3dContext->OMSetRenderTargets(1, targets, m_deviceResources->GetDepthStencilView());
            d3dContext->ClearDepthStencilView(m_deviceResources->GetDepthStencilView(), D3D11_CLEAR_DEPTH, 1.0f, 0);

            // d2d -- Discussed later under Adding UI
            d2dContext->SetTarget(m_deviceResources->GetD2DTargetBitmap());
        }

        const float clearColor[4] = { 0.5f, 0.5f, 0.8f, 1.0f };

        // Only need to clear the background when not rendering the full 3D scene since
        // the 3D world is a fully enclosed box and the dynamics prevents the camera from
        // moving outside this space.
        if (i > 0)
        {
            // Doing the Right Eye View.
            d3dContext->ClearRenderTargetView(m_deviceResources->GetBackBufferRenderTargetViewRight(), clearColor);
        }
        else
        {
            // Doing the Mono or Left Eye View.
            d3dContext->ClearRenderTargetView(m_deviceResources->GetBackBufferRenderTargetView(), clearColor);
        }

        // Render the scene objects
        if (m_game != nullptr && m_gameResourcesLoaded && m_levelResourcesLoaded)
        {
            // This section is only used after the game state has been initialized and all device
            // resources needed for the game have been created and associated with the game objects.
            if (stereoEnabled)
            {
                // When doing stereo, it is necessary to update the projection matrix once per rendering pass.

                auto orientation = m_deviceResources->GetOrientationTransform3D();

                ConstantBufferChangeOnResize changesOnResize;
                // Apply either a left or right eye projection, which is an offset from the middle
                XMStoreFloat4x4(
                    &changesOnResize.projection,
                    XMMatrixMultiply(
                        XMMatrixTranspose(
                            i == 0 ?
                            m_game->GameCamera().LeftEyeProjection() :
                            m_game->GameCamera().RightEyeProjection()
                            ),
                        XMMatrixTranspose(XMLoadFloat4x4(&orientation))
                        )
                    );

                d3dContext->UpdateSubresource(
                    m_constantBufferChangeOnResize.get(),
                    0,
                    nullptr,
                    &changesOnResize,
                    0,
                    0
                    );
            }

            // Update variables that change once per frame.
            ConstantBufferChangesEveryFrame constantBufferChangesEveryFrameValue;
            XMStoreFloat4x4(
                &constantBufferChangesEveryFrameValue.view,
                XMMatrixTranspose(m_game->GameCamera().View())
                );
            d3dContext->UpdateSubresource(
                m_constantBufferChangesEveryFrame.get(),
                0,
                nullptr,
                &constantBufferChangesEveryFrameValue,
                0,
                0
                );

            // Set up the graphics pipeline. This sample uses the same InputLayout and set of
            // constant buffers for all shaders, so they only need to be set once per frame.
            // For more info about the graphics or rendering pipeline, see
            // https://docs.microsoft.com/windows/win32/direct3d11/overviews-direct3d-11-graphics-pipeline

            // IASetInputLayout binds an input-layout object to the input-assembler (IA) stage. 
            // Input-layout objects describe how vertex buffer data is streamed into the IA pipeline stage.
            // Set up the Direct3D context to use this vertex layout. For more info, see
            // https://docs.microsoft.com/windows/win32/api/d3d11/nf-d3d11-id3d11devicecontext-iasetinputlayout
            d3dContext->IASetInputLayout(m_vertexLayout.get());

            // VSSetConstantBuffers sets the constant buffers used by the vertex shader pipeline stage.
            // Set up the Direct3D context to use these constant buffers. For more info, see
            // https://docs.microsoft.com/windows/win32/api/d3d11/nf-d3d11-id3d11devicecontext-vssetconstantbuffers

            ID3D11Buffer* constantBufferNeverChanges{ m_constantBufferNeverChanges.get() };
            d3dContext->VSSetConstantBuffers(0, 1, &constantBufferNeverChanges);
            ID3D11Buffer* constantBufferChangeOnResize{ m_constantBufferChangeOnResize.get() };
            d3dContext->VSSetConstantBuffers(1, 1, &constantBufferChangeOnResize);
            ID3D11Buffer* constantBufferChangesEveryFrame{ m_constantBufferChangesEveryFrame.get() };
            d3dContext->VSSetConstantBuffers(2, 1, &constantBufferChangesEveryFrame);
            ID3D11Buffer* constantBufferChangesEveryPrim{ m_constantBufferChangesEveryPrim.get() };
            d3dContext->VSSetConstantBuffers(3, 1, &constantBufferChangesEveryPrim);

            // Sets the constant buffers used by the pixel shader pipeline stage. 
            // For more info, see
            // https://docs.microsoft.com/windows/win32/api/d3d11/nf-d3d11-id3d11devicecontext-pssetconstantbuffers

            d3dContext->PSSetConstantBuffers(2, 1, &constantBufferChangesEveryFrame);
            d3dContext->PSSetConstantBuffers(3, 1, &constantBufferChangesEveryPrim);
            ID3D11SamplerState* samplerLinear{ m_samplerLinear.get() };
            d3dContext->PSSetSamplers(0, 1, &samplerLinear);

            for (auto&& object : m_game->RenderObjects())
            {
                // The 3D object render method handles the rendering.
                // For more info, see Primitive rendering below.
                object->Render(d3dContext, m_constantBufferChangesEveryPrim.get());
            }
        }

        // Start of 2D rendering
        ...
    }
}
```

### <a name="primitive-rendering"></a>基元呈现

呈现场景时，将循环所有需要呈现的对象。 对每个对象（基元）重复以下步骤。

- 用模型的[世界变换矩阵](#world-transform-matrix)和材料信息更新常量缓冲区（**m_constantBufferChangesEveryPrim**）。
- **M_constantBufferChangesEveryPrim**包含每个对象的参数。 它包括对象到世界转换矩阵以及材料属性（如光照计算的颜色和反射指数）。
- 将 Direct3D 上下文设置为使用要流式传输到[呈现管道](#rendering-pipeline)的输入汇编程序（IA）阶段的网格对象数据的输入顶点布局。
- 将 Direct3D 上下文设置为在 IA 阶段使用[索引缓冲区](#index-buffer)。 提供基元信息：类型、数据顺序。
- 提交绘图调用来绘制索引的非实例化基元。 **GameObject::Render** 方法使用特定于给定基元的数据更新基元[常量缓冲区](#constant-buffer-or-shader-constant-buffer)。 这将导致在上下文中调用 **DrawIndexed**，以绘制每个基元的几何图形。 尤其是，此绘图调用会随着常量缓冲区数据的参数化将命令和数据编入图形处理单元 (GPU) 的队列。 每个绘图调用对每个顶点执行一次顶点着色器，然后对基元中每个三角形的每个像素执行一次[像素着色器](#vertex-shaders-and-pixel-shaders)。 纹理是像素着色器用于执行呈现的状态部分。

下面是使用多个常量缓冲区的原因。

- 游戏使用多个常量缓冲区，但每个基元只需更新一次这些缓冲区。 如之前所述，常量缓冲区就像对每个基元的着色器的输入。 某些数据是静态的（**m_constantBufferNeverChanges**）;某些数据在帧（**m_constantBufferChangesEveryFrame**）上是常量，如相机的位置;某些数据特定于基元，例如其颜色和纹理（**m_constantBufferChangesEveryPrim**）。
- 示例呈现器将这些输入分别放入不同的常量缓冲区，以优化 CPU 和 GPU 使用的内存带宽。 此方法还有助于最大程度地减少 GPU 需要跟踪的数据量。 GPU 有一个很大的命令队列，游戏每次调用 **Draw** 时，该命令将随与之关联的数据一起排队。 当游戏更新基元常量缓冲区并发出下一个 **Draw** 命令时，图形驱动程序会将此下一个命令和关联的数据添加到队列。 如果游戏绘制 100 个基元，它可能在队列中有 100 个常量缓冲区数据的副本。 为了最大程度地减少游戏发送到 GPU 的数据量，游戏使用仅包含每个基元更新的单独基元常量缓冲区。

#### <a name="gameobjectrender-method"></a>GameObject::Render 方法

```cppwinrt
void GameObject::Render(
    _In_ ID3D11DeviceContext* context,
    _In_ ID3D11Buffer* primitiveConstantBuffer
    )
{
    if (!m_active || (m_mesh == nullptr) || (m_normalMaterial == nullptr))
    {
        return;
    }

    ConstantBufferChangesEveryPrim constantBuffer;

    // Put the model matrix info into a constant buffer, in world matrix.
    XMStoreFloat4x4(
        &constantBuffer.worldMatrix,
        XMMatrixTranspose(ModelMatrix())
        );

    // Check to see which material to use on the object.
    // If a collision (a hit) is detected, GameObject::Render checks the current context, which 
    // indicates whether the target has been hit by an ammo sphere. If the target has been hit, 
    // this method applies a hit material, which reverses the colors of the rings of the target to 
    // indicate a successful hit to the player. Otherwise, it applies the default material 
    // with the same method. In both cases, it sets the material by calling Material::RenderSetup, 
    // which sets the appropriate constants into the constant buffer. Then, it calls 
    // ID3D11DeviceContext::PSSetShaderResources to set the corresponding texture resource for the 
    // pixel shader, and ID3D11DeviceContext::VSSetShader and ID3D11DeviceContext::PSSetShader 
    // to set the vertex shader and pixel shader objects themselves, respectively.

    if (m_hit && m_hitMaterial != nullptr)
    {
        m_hitMaterial->RenderSetup(context, &constantBuffer);
    }
    else
    {
        m_normalMaterial->RenderSetup(context, &constantBuffer);
    }

    // Update the primitive constant buffer with the object model's info.
    context->UpdateSubresource(primitiveConstantBuffer, 0, nullptr, &constantBuffer, 0, 0);

    // Render the mesh.
    // See MeshObject::Render method below.
    m_mesh->Render(context);
}
```

#### <a name="meshobjectrender-method"></a>MeshObject：： Render 方法

```cppwinrt
void MeshObject::Render(_In_ ID3D11DeviceContext* context)
{
    // PNTVertex is a struct. stride provides us the size required for all the mesh data
    // struct PNTVertex
    //{
    //  DirectX::XMFLOAT3 position;
    //  DirectX::XMFLOAT3 normal;
    //  DirectX::XMFLOAT2 textureCoordinate;
    //};
    uint32_t stride{ sizeof(PNTVertex) };
    uint32_t offset{ 0 };

    // Similar to the main render loop.
    // Input-layout objects describe how vertex buffer data is streamed into the IA pipeline stage.
    ID3D11Buffer* vertexBuffer{ m_vertexBuffer.get() };
    context->IASetVertexBuffers(0, 1, &vertexBuffer, &stride, &offset);

    // IASetIndexBuffer binds an index buffer to the input-assembler stage.
    // For more info, see
    // https://docs.microsoft.com/windows/win32/api/d3d11/nf-d3d11-id3d11devicecontext-iasetindexbuffer.
    context->IASetIndexBuffer(m_indexBuffer.get(), DXGI_FORMAT_R16_UINT, 0);

    // Binds information about the primitive type, and data order that describes input data for the input assembler stage.
    // For more info, see
    // https://docs.microsoft.com/windows/win32/api/d3d11/nf-d3d11-id3d11devicecontext-iasetprimitivetopology.
    context->IASetPrimitiveTopology(D3D11_PRIMITIVE_TOPOLOGY_TRIANGLELIST);

    // Draw indexed, non-instanced primitives. A draw API submits work to the rendering pipeline.
    // For more info, see
    // https://docs.microsoft.com/windows/win32/api/d3d11/nf-d3d11-id3d11devicecontext-drawindexed.
    context->DrawIndexed(m_indexCount, 0, 0);
}
```

### <a name="deviceresourcespresent-method"></a>DeviceResources：:P 重发方法

我们调用**DeviceResources：:P 重发**方法，以显示我们放入缓冲区中的内容。

我们为用于向用户显示帧的缓冲区集合使用术语交换链。 应用程序每次提供要显示的新帧时，交换链中的第一个缓冲区将替代已显示的缓冲区。 此过程称为交换或翻转。 有关详细信息，请参阅[交换链](../graphics-concepts/swap-chains.md)。

- **IDXGISwapChain1**接口的**当前**方法指示[DXGI](#dxgi)在垂直同步（VSync）发生之前一直阻止，使应用程序进入睡眠状态，直到下一个 VSync 为止。 这可以确保不会浪费任何将永远不会显示在屏幕上的循环呈现帧。
- **ID3D11DeviceContext3**接口的**DiscardView**方法会丢弃[呈现器目标](#render-target)的内容。 仅当现有内容将完全被覆盖时此操作才有效。 如果使用脏或 scroll rect，则应删除此调用。
* 使用相同的 **DiscardView** 方法，丢弃[深度模板](#depth-stencil)的内容。
* **HandleDeviceLost**方法用于管理要删除的[设备](#device)的方案。 如果通过断开连接或驱动程序升级删除了设备，则必须重新创建所有设备资源。 有关详细信息，请参阅[在 Direct3D 11 中处理设备删除方案](handling-device-lost-scenarios.md)。

> [!TIP]
> 若要实现平滑帧速率，必须确保渲染帧的工作量适合于 VSyncs 之间的时间。

```cppwinrt
// Present the contents of the swap chain to the screen.
void DX::DeviceResources::Present()
{
    // The first argument instructs DXGI to block until VSync, putting the application
    // to sleep until the next VSync. This ensures we don't waste any cycles rendering
    // frames that will never be displayed to the screen.
    HRESULT hr = m_swapChain->Present(1, 0);

    // Discard the contents of the render target.
    // This is a valid operation only when the existing contents will be entirely
    // overwritten. If dirty or scroll rects are used, this call should be removed.
    m_d3dContext->DiscardView(m_d3dRenderTargetView.get());

    // Discard the contents of the depth stencil.
    m_d3dContext->DiscardView(m_d3dDepthStencilView.get());

    // If the device was removed either by a disconnection or a driver upgrade, we 
    // must recreate all device resources.
    if (hr == DXGI_ERROR_DEVICE_REMOVED || hr == DXGI_ERROR_DEVICE_RESET)
    {
        HandleDeviceLost();
    }
    else
    {
        winrt::check_hresult(hr);
    }
}
```

## <a name="next-steps"></a>后续步骤

本主题说明了图形在显示器上的呈现方式，并提供了一些使用的渲染词的简短说明（见下）。 详细了解渲染[框架 II：游戏渲染](tutorial-game-rendering.md)主题，并了解如何在呈现之前准备所需数据。

## <a name="terms-and-concepts"></a>术语和概念

### <a name="simple-game-scene"></a>简单的游戏场景

简单的游戏场景由具有若干光源的几个对象构成。

对象的形状由空间内的一组 X、Y、Z 坐标定义。 游戏世界中的实际呈现位置可以通过将转换矩阵应用于位置 X、Y、Z 坐标来确定。 它还可能具有一组纹理坐标 &mdash; 和 V， &mdash; 它们指定如何将材料应用于对象。 这将定义对象的表面属性，并使您能够查看某个对象是否具有粗糙表面（如网球球）或光滑光滑表面（如保龄球球）。

呈现框架使用场景和对象信息按帧重新创建场景框架，使其在显示器上处于活动状态。

### <a name="rendering-pipeline"></a>呈现管道

呈现管道是将三维场景信息转换为屏幕上显示的图像的过程。 在 Direct3D 11 中，此管道是可编程的。 你可以调整各个阶段来支持你的呈现需求。 具有常见着色器核心的阶段可通过使用 HLSL 编程语言进行编程。 它也称为*图形呈现管道*，或只是*管道*。

若要帮助创建此管道，需要熟悉这些详细信息。

- [HLSL](#hlsl)。 对于 UWP DirectX 游戏，我们建议使用 HLSL 着色器模型 5.1 及更高版本。
- [着色](#shaders)器。
- [顶点着色器和像素着色](#vertex-shaders-and-pixel-shaders)器。
- [着色器阶段](#shader-stages)。
- [各种着色器文件格式](#various-shader-file-formats)。

有关详细信息，请参阅[了解 Direct3D 11 呈现管道](/windows/desktop/direct3dgetstarted/understand-the-directx-11-2-graphics-pipeline)和[图形管道](/windows/desktop/direct3d11/overviews-direct3d-11-graphics-pipeline)。

#### <a name="hlsl"></a>HLSL

HLSL 是用于 DirectX 的高级着色器语言。 使用 HLSL，可以为 Direct3D 管道创建类似于 C 的可编程着色器。 有关详细信息，请参阅 [HLSL](/windows/desktop/direct3dhlsl/dx-graphics-hlsl)。

#### <a name="shaders"></a>着色器

着色器可以被视为一组说明，用于确定如何在呈现时显示对象的表面。 使用 HLSL 编程的着色器称为 HLSL 着色。 [HLSL] （#hlsl）着色器的源代码文件具有 `.hlsl` 文件扩展名。 这些着色器可以在生成时或运行时编译，并在运行时设置为相应的管道阶段。 已编译的着色器对象具有 `.cso` 文件扩展名。

Direct3D 9 着色器可以使用着色器模型1、着色器模型2和着色器模型3设计;Direct3D 10 着色器只能在着色器模型4上设计。 Direct3D 11 着色器可以基于着色器模型 5 设计。 Direct3D 11.3 和 Direct3D 12 可以基于着色器模型 5.1 设计，Direct3D 12 还可以基于着色器模型 6 设计。

#### <a name="vertex-shaders-and-pixel-shaders"></a>顶点着色器和像素着色器

数据将图形管道作为基元流进入，并由各种着色器（例如顶点着色器和像素着色器）进行处理。 

顶点着色器处理顶点，通常执行诸如转换、换肤以及照明之类的操作。 像素着色器支持丰富的着色技术，如每像素照明和后处理。 它将常变量、纹理数据、内插的每顶点值和其他数据组合起来以生成每像素输出。 

#### <a name="shader-stages"></a>着色器阶段

定义的用于处理此基元流的各个着色器的序列在呈现管道中称为着色器阶段。 实际阶段取决于 Direct3D 的版本，但通常包括顶点、几何图形和像素阶段。 还有其他一些阶段，如用于分割的外壳着色器和域着色器、计算着色器。 所有这些阶段都是使用[HLSL](#hlsl)完全可编程的。 有关详细信息，请参阅[图形管道](/windows/desktop/direct3d11/overviews-direct3d-11-graphics-pipeline)。

#### <a name="various-shader-file-formats"></a>各着色器文件格式

下面是着色器代码文件扩展。

- `.hlsl`扩展名为 [HLSL] （#hlsl）源代码的文件。
- 扩展名为的文件 `.cso` 保存已编译的着色器对象。
- 扩展名为的文件 `.h` 是标头文件，但在着色器代码上下文中，此标头文件定义了一个包含着色器数据的字节数组。
- 扩展名为的文件 `.hlsli` 包含常量缓冲区的格式。 在示例游戏中，该文件为**着色**  >  **ConstantBuffers. .hlsli**。

> [!NOTE]
> 您可以通过 `.cso` 在运行时加载文件或通过 `.h` 在可执行代码中添加文件来嵌入着色器。 但对于同一着色器，不会同时使用这两种方法。

### <a name="deeper-understanding-of-directx"></a>更深入地了解 DirectX

Direct3D 11 是一组 Api，可帮助我们为图形密集型应用程序（如游戏）创建图形，在这些应用程序中，我们希望使用合适的图形卡来处理密集型计算。 此部分简要说明 Direct3D 11 图形编程概念：资源、子资源、设备和设备上下文。

#### <a name="resource"></a>资源

可以将资源（也称为设备资源）视为有关如何呈现对象的信息，如纹理、位置或颜色。 资源将数据提供给管道，并定义场景中呈现的内容。 可以从游戏媒体加载资源，也可以在运行时动态创建资源。

资源实际上是内存中可由 Direct3D [管道](#rendering-pipeline)访问的区域。 为了使管道能够高效地访问内存，提供给管道的数据（如输入几何图形、着色器资源和纹理）必须存储在资源中。 所有 Direct3D 资源都存储在两种类型的资源中：缓冲区或纹理。 每个管道阶段最多可以有 128 个活动资源。 有关详细信息，请参阅[资源](../graphics-concepts/resources.md)。

#### <a name="subresource"></a>子资源

术语子资源指资源的子集。 Direct3D 可以引用整个资源，也可以引用资源的子集。 有关详细信息，请参阅[子资源](../graphics-concepts/resource-types.md#subresources)。

#### <a name="depth-stencil"></a>深度模板

深度模板资源包含用于保留深度和模板信息的格式和缓冲区。 其使用纹理资源创建。 有关如何创建深度模板资源的详细信息，请参阅[配置深度模板功能](/windows/desktop/direct3d11/d3d10-graphics-programming-guide-depth-stencil)。 我们通过使用 [ID3D11DepthStencilView](/windows/desktop/api/d3d11/nn-d3d11-id3d11depthstencilview) 界面实现的深度模板视图访问深度模板资源。

深度信息告诉我们多边形的哪些区域位于其他区域，以便我们可以确定哪些是隐藏的。 模板信息告知我们会屏蔽哪些像素。 可以使用它来产生特殊效果，因为它确定是否绘制某个像素；将位设置为 1 或 0。 

有关详细信息，请参阅[深度模具视图](../graphics-concepts/depth-stencil-view--dsv-.md)、[深度缓冲区](../graphics-concepts/depth-buffers.md)和[模具缓冲区](../graphics-concepts/stencil-buffers.md)。

#### <a name="render-target"></a>呈现器目标

呈现器目标是我们可以在呈现传递末尾写入的资源。 它通常使用 [ID3D11Device::CreateRenderTargetView](/windows/desktop/api/d3d11/nf-d3d11-id3d11device-createrendertargetview) 方法、将交换链后台缓冲区（也是一种资源）用作输入参数来创建。 

每个呈现器目标还应有一个相应的深度模板视图，因为当我们在使用呈现目标前使用 [OMSetRenderTargets](/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-omsetrendertargets) 设置它时，它也需要深度模板视图。 我们通过使用 [ID3D11RenderTargetView](/windows/desktop/api/d3d11/nn-d3d11-id3d11rendertargetview) 界面实现的呈现器目标视图访问呈现器目标资源。 

#### <a name="device"></a>设备

您可以想像一台设备来分配和销毁对象，呈现基元，并通过图形驱动程序与图形卡通信。 

更准确地说，Direct3D 设备是 Direct3D 的渲染组件。 设备封装并存储渲染状态，执行转换和照明操作，并将图像光栅化到表面。 有关详细信息，请参阅[设备](../graphics-concepts/devices.md)

设备由[**ID3D11Device**](/windows/desktop/api/d3d11/nn-d3d11-id3d11device)接口表示。 换句话说， **ID3D11Device**接口表示虚拟显示适配器，用于创建设备所拥有的资源。 

ID3D11Device 有不同的版本。 [**ID3D11Device5**](/windows/desktop/api/d3d11_4/nn-d3d11_4-id3d11device5)是最新版本，并将新方法添加到**ID3D11Device4**中的新方法。 有关 Direct3D 如何与基础硬件通信的详细信息，请参阅 [Windows 设备驱动程序 (WDDM) 体系结构](/windows-hardware/drivers/display/windows-vista-and-later-display-driver-model-architecture)。

每个应用程序都必须至少有一个设备;大多数应用程序仅创建一个。 通过调用**D3D11CreateDevice**或**D3D11CreateDeviceAndSwapChain**并指定带有**D3D_DRIVER_TYPE**标志的驱动程序类型，为安装在计算机上的某个硬件驱动程序创建设备。 每台设备可以使用一个或多个设备上下文，具体取决于所需的功能。 有关详细信息，请参阅 [D3D11CreateDevice 函数](/windows/desktop/api/d3d11/nf-d3d11-d3d11createdevice)。

#### <a name="device-context"></a>设备上下文

设备上下文用于设置[管道](#rendering-pipeline)状态，并使用[设备](#device)拥有的[资源](#resource)生成呈现命令。 

Direct3D 11 实现两种类型的设备上下文，一个用于立即呈现，另一个用于延迟呈现；两个上下文均使用 [ID3D11DeviceContext](/windows/desktop/api/d3d11/nn-d3d11-id3d11devicecontext) 界面表示。 

**ID3D11DeviceContext** 界面具有不同版本；**ID3D11DeviceContext4** 向 **ID3D11DeviceContext3** 中的界面添加新方法。

Windows 10 创意者更新中引入了**ID3D11DeviceContext4** ，是**ID3D11DeviceContext**接口的最新版本。 面向 Windows 10 创意者更新及更高版本的应用程序应使用此接口而不是早期版本。 有关详细信息，请参阅 [ID3D11DeviceContext4](/windows/desktop/api/d3d11_3/nn-d3d11_3-id3d11devicecontext4)。

#### <a name="dxdeviceresources"></a>DX::DeviceResources

**DX：:D eviceresources**类在**DeviceResources** / **.h**文件中，并控制所有 DirectX 设备资源。

### <a name="buffer"></a>Buffer

缓冲区资源是一系列完全类型化的数据，被分组到元素中。 你可以使用缓冲区来存储各类数据，包括位置矢量、法向矢量、顶点缓冲区中的纹理坐标、索引缓冲区中的索引或设备状态。 Buffer 元素可以包含打包的数据值（如**R8G8B8A8** surface 值）、单个8位整数或 4 32 位浮点值。

有三种类型的可用缓冲区：顶点缓冲区、索引缓冲区和常量缓冲区。

#### <a name="vertex-buffer"></a>顶点缓冲区

包含用于定义几何图形的顶点数据。 顶点数据包括位置坐标、颜色数据、纹理坐标数据、法线数据等。 

#### <a name="index-buffer"></a>索引缓冲区

包含到顶点缓冲区的整数偏移量，用于更高效地渲染基元。 索引缓冲区包含一组连续的 16 位或 32 位索引；每个索引用于标识顶点缓冲区中的一个顶点。

#### <a name="constant-buffer-or-shader-constant-buffer"></a>常量缓冲区或着色器常量缓冲区

让你能够高效地向管道提供着色器数据。 你可以将常量缓冲区用作为每个基元运行以及存储呈现管道的流输出阶段结果的着色器的输入。 从概念上讲，常量缓冲区看起来就像单元素顶点缓冲区。

#### <a name="design-and-implementation-of-buffers"></a>缓冲区的设计和实现

您可以基于数据类型设计缓冲区，例如，在我们的示例游戏中，为静态数据创建一个缓冲区，为静态数据创建一个缓冲区，对特定于基元的数据使用另一个缓冲区。

所有缓冲区类型由 **ID3D11Buffer** 界面封装，你可以通过调用 **ID3D11Device::CreateBuffer** 创建缓冲区资源。 但缓冲区必须绑定到管道后才能访问。 缓冲区可以同时绑定到多个管道阶段来用于读取。 缓冲区还可以绑定到单个管道阶段以进行写入;但是，同一缓冲区不能同时绑定到读取和写入。

可以通过这些方式绑定缓冲区。

- 通过调用**ID3D11DeviceContext**方法（如**ID3D11DeviceContext：： IASetVertexBuffers**和**ID3D11DeviceContext：： IASetIndexBuffer**）到输入汇编程序阶段。
- 到流输出阶段，方法是调用**ID3D11DeviceContext：： SOSetTargets**。
- 到着色器阶段，方法是调用着色器方法，如**ID3D11DeviceContext：： VSSetConstantBuffers**。

有关详细信息，请参阅 [Direct3D 11 中的缓冲区简介](/windows/desktop/direct3d11/overviews-direct3d-11-resources-buffers-intro)。

### <a name="dxgi"></a>DXGI

Microsoft DirectX 图形基础结构（DXGI）是一个子系统，它封装了 Direct3D 所需的一些低级别任务。 在多线程应用程序中使用 DXGI 时必须特别注意，以确保不会发生死锁。 有关详细信息，请参阅[多线程和 DXGI](/windows/win32/direct3darticles/dxgi-best-practices#multithreading-and-dxgi)

### <a name="feature-level"></a>功能级别

功能级别是 Direct3D 11 中引入的概念，用于处理新的或现有计算机中的各类视频卡。 功能级别是一组定义完善的图形处理单元（GPU）功能。 

每个视频卡根据所安装的 GPU 来实现特定级别的 DirectX 功能。 在以前版本的 Microsoft Direct3D 中，你可以找到视频卡实现的 Direct3D 版本，然后相应地对应用程序编程。 

使用功能级别，在创建设备时，你可以尝试为想要请求的功能级别创建设备。 如果设备创建成功，该功能级别将存在，如果失败，硬件将不支持该功能级别。 可以尝试在较低的功能级别重新创建设备，也可以选择退出应用程序。 例如，12_0 功能级别需要 Direct3D 11.3 或 Direct3D 12 以及着色器模型5.1。 有关详细信息，请参阅 [Direct3D 功能级别：各功能级别概述](/windows/desktop/direct3d11/overviews-direct3d-11-devices-downlevel-intro)。

使用功能级别，你可以开发适用于 Direct3D 9、Microsoft Direct3D 10 或 Direct3D 11 的应用程序，然后在 9、10 或 11 硬件上运行应用程序（除一些例外情况）。 有关详细信息，请参阅 [Direct3D 功能级别](/windows/desktop/direct3d11/overviews-direct3d-11-devices-downlevel-intro)。

### <a name="stereo-rendering"></a>立体呈现

立体呈现用于增强深度的视觉效果。 它使用两个图像，一个从左眼、另一个从右眼来在显示屏幕上显示场景。 

通过数学方式，我们应用常规单一投影矩阵的立体投影矩阵（稍微向右或向左水平偏移）来实现此目的。

我们执行了两个渲染阶段来实现此示例游戏中的立体声渲染。

- 绑定到右侧的呈现器目标，应用右投影，然后绘制基元对象。
- 绑定到左侧的呈现器目标，应用右投影，然后绘制基元对象。

### <a name="camera-and-coordinate-space"></a>相机和坐标空间

该游戏有现成的代码用于更新本身坐标系中的世界（有时称为世界空间或场景空间）。 所有对象（包括相机）都在此空间定位和确定方向。 有关详细信息，请参阅[坐标系统](../graphics-concepts/coordinate-systems.md)。

顶点着色器使用以下算法执行从模型坐标到设备坐标的转换（其中 V 是一个矢量，M 是一个矩阵）。

`V(device) = V(model) x M(model-to-world) x M(world-to-view) x M(view-to-device)`

- `M(model-to-world)`是模型坐标到世界坐标（也称为[世界变换矩阵](#world-transform-matrix)）的变换矩阵。 这由基元提供。
- `M(world-to-view)`是用于全局坐标的变换矩阵，它也称为[视图转换矩阵](#view-transform-matrix)。
  - 这由相机的视图矩阵提供。 它由相机的位置定义，以及外观向量（从相机直接指向场景*的外观向量*，以及与之垂直的*查找*向量）。
  - 在示例游戏中， **m_viewMatrix**是视图变换矩阵，并使用**摄像：： SetViewParams**进行计算。
- `M(view-to-device)`是视图坐标到设备坐标的变换矩阵，也称为[投影转换矩阵](#projection-transform-matrix)。
  - 这由相机的投影提供。 它提供有关在最终场景中实际可见空间的信息。 视图的字段（FoV）、纵横比和剪辑平面定义投影转换矩阵。
  - 在示例游戏中， **m_projectionMatrix**定义转换为投影坐标，并使用**摄像：： SetProjParams**进行计算（对于立体声投影，为 &mdash; 每个眼睛的视图使用两个投影矩阵）。

中的着色器代码 `VertexShader.hlsl` 将与常量缓冲区中的向量和矩阵一起加载，并对每个顶点执行此转换。

### <a name="coordinate-transformation"></a>坐标转换

Direct3D 使用三个转换来将 3D 模型坐标更改为像素坐标（屏幕空间）。 这些转换是世界转换、视图转换和投影转换。 有关详细信息，请参阅[转换概述](../graphics-concepts/transform-overview.md)。

#### <a name="world-transform-matrix"></a>世界转换矩阵

世界转换将模型空间（在此空间内，顶点是相对于模型的本地原点定义的）的坐标更改为世界空间（在此空间中，顶点是相对场景中所有对象共同的原点定义的）。 本质上，世界转换将模型放入世界空间中；然后是它的名称。 有关详细信息，请参阅[世界转换](../graphics-concepts/world-transform.md)。

#### <a name="view-transform-matrix"></a>视图转换矩阵

视图转换在世界空间中定位查看器，并将顶点转换为相机空间。 在相机空间中，相机或查看器位于原点，并面向正 z 方向。 有关详细信息，请转到[视图转换](../graphics-concepts/view-transform.md)。

####  <a name="projection-transform-matrix"></a>投影转换矩阵

投影转换可将视锥转换为长方体形状。 视锥是场景中相对于视区的相机放置的 3D 体。 视区是将 3D 场景投影到其中的 2D 矩形。 有关详细信息，请参阅[视区和剪切](../graphics-concepts/viewports-and-clipping.md)。

由于视锥的近端小于远端，这将产生拉伸靠近相机的对象的效果；这是透视应用于场景的方式。 因此，靠近播放机的对象显示得更大;更远离的对象显得更小。

从数学上来说，投影转换是一种通常是刻度和透视投影的矩阵。 它像相机镜头一样工作。 有关详细信息，请参阅[投影转换](../graphics-concepts/projection-transform.md)。

### <a name="sampler-state"></a>取样器状态

取样器状态确定如何使用纹理寻址模式、筛选和详细级别对纹理数据采样。 每次从纹理中读取纹理像素（或纹素）时，都将执行采样。

纹理包含纹素的数组。 每个纹素的位置由表示 `(u,v)` ，其中 `u` 是宽度， `v` 是高度，并基于纹理宽度和高度在0和1之间进行映射。 生成的纹理坐标用于在对纹理采样时对纹素寻址。

当纹理坐标小于 0 或大于 1 时，纹理寻址模式定义纹理坐标如何寻址纹素位置。 例如，在使用 **TextureAddressMode.Clamp** 时，任何 0-1 范围以外的坐标在采样之前都将被限定为最大值为 1，最小值为 0。

如果纹理太大或太小而无法用于多边形，则会对纹理进行筛选以适合空间。 放大筛选器放大纹理、缩小筛选器缩小纹理以适应更小的区域。 纹理放大为生成更模糊图像的一个或多个地址重复示例纹素。 纹理缩减更复杂，因为它需要将多个纹素值组合为单个值。 这可能导致失真或锯齿形边缘，具体取决于纹理数据。 最受欢迎的缩小方法是使用 mipmap。 Mipmap 是多级纹理。 每个级别的大小为2的幂，小于上一级别到1x1 纹理。 使用缩小时，游戏选择最接近呈现时所需大小的 mipmap 级别。 

### <a name="the-basicloader-class"></a>BasicLoader 类

**BasicLoader** 是一个简单的加载器类，它为从磁盘上的文件加载着色器、纹理和网格提供支持。 它提供同步和异步方法。 在此示例游戏中， `BasicLoader.h/.cpp` 文件位于 "**实用工具**" 文件夹中。

有关详细信息，请参阅[基本加载器](complete-code-for-basicloader.md)。