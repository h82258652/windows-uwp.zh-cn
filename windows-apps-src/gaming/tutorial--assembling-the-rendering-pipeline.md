---
title: 呈现简介
description: 了解如何装配显示图形的呈现管道。 呈现简介。
ms.assetid: 1da3670b-2067-576f-da50-5eba2f88b3e6
ms.date: 10/24/2017
ms.topic: article
keywords: windows 10, uwp, 游戏, 呈现
ms.localizationpriority: medium
ms.openlocfilehash: 6724aedf898706dd4c5bf728616c918d64b2fb32
ms.sourcegitcommit: 89ff8ff88ef58f4fe6d3b1368fe94f62e59118ad
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/29/2018
ms.locfileid: "8202577"
---
# <a name="rendering-framework-i-intro-to-rendering"></a>呈现框架 I：呈现简介

我们在之前的主题中介绍了如何生成通用 Windows 平台 (UWP) 游戏，以及如何定义状态机来处理游戏流。 现在，应该来了解如何装配呈现框架了。 让我们来看看示例游戏如何呈现游戏场景使用 Direct3D11 （通常称为 DirectX 11）。

>[!Note]
>如果你尚未下载适用于此示例的最新游戏代码，请转到 [Direct3D 游戏示例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Simple3DGameDX)。 此示例是大型 UWP 功能示例集合的一部分。 有关如何下载示例的说明，请参阅[从 GitHub 获取 UWP 示例](https://docs.microsoft.com/windows/uwp/get-started/get-uwp-app-samples)。

Direct3D 11 包含一组 API，其提供访问高性能图形硬件的高级功能的访问权限，而该硬件可用于为游戏等图形密集应用程序创建 3D 图形。

在屏幕上呈现游戏图形基本上是在屏幕上呈现帧序列。 在每个帧中，你必须基于视图呈现场景中可见的对象。 

为呈现框架，必须将需要的场景信息传递到硬件，以便它可以显示在屏幕上。 如果你希望在屏幕上显示任何内容，则需要在游戏开始运行后即开始呈现。

## <a name="objective"></a>目标

若要设置基本呈现框架，以便为 UWP DirectX 游戏显示图形输出，你可以大致将其归纳为以下三个步骤。

 1. 与图形界面建立连接
 2. 创建绘制图形所需的资源
 3. 通过呈现帧显示图形

本文介绍如何呈现图形，包含步骤 1 和 3。

[呈现框架 II：游戏呈现](tutorial-game-rendering.md)包含步骤 2；如何设置呈现框架以及如何为呈现准备数据。

## <a name="get-started"></a>入门

在开始之前，你应该熟悉基本的图形和呈现概念。 如果你不熟悉 Direct3D 和呈现，请参阅[术语和概念](#terms-and-concepts)了解本文使用的图形和呈现术语的简要描述。

对于此游戏，__GameRenderer__ 类对象代表此示例游戏的呈现器。  它负责创建和维护用于生成游戏视觉效果的所有 Direct3D 11 和 Direct2D 对象。  它还保留对 __Simple3DGame__ 对象的引用，此对象用于检索要呈现的对象列表，以及提醒显示 (HUD) 的游戏状态。 

在教程的这个部分，我们将重点介绍在游戏中呈现 3D 对象。

## <a name="establish-a-connection-to-the-graphics-interface"></a>与图形界面建立连接

若要访问用于呈现的硬件，请参阅 [__App::Initialize__](tutorial--building-the-games-uwp-app-framework.md#appinitialize-method) 下的 UWP 框架文章。

__make\_shared function__（如[下方](#appinitialize-method)所示）用于创建 [__DX::DeviceResources__](#dxdeviceresources) 的 __shared\_ptr__，这还将提供对设备的访问权限。 

在 Direct3D 11 中，[设备](#device)用于分配和销毁对象、呈现原语，并通过图形驱动程序与显卡通信。

### <a name="appinitialize-method"></a>App::Initialize 方法

```cpp
void App::Initialize(
    CoreApplicationView^ applicationView
    )
{
    //...

    // At this point we have access to the device. 
    // We can create the device-dependent resources.
    m_deviceResources = std::make_shared<DX::DeviceResources>();
}
```

## <a name="display-the-graphics-by-rendering-the-frame"></a>通过呈现帧显示图形

游戏场景需要在游戏启动时呈现。 [__GameMain::Run__](#gameamainrun-method) 方法中的呈现开始说明（如下所示）。

这个简单的流是：
1. __更新__
2. __呈现__
3. __显示__

### <a name="gamemainrun-method"></a>GameMain::Run 方法

```cpp
// Comparing this to App::Run() in the DirectX 11 App template
void GameMain::Run()
{
    while (!m_windowClosed)
    {
        if (m_visible) // if the window is visible
        {
            // Added: Switching different game states since this game sample is more complex
            switch (m_updateState) 
            {

                // ...
                CoreWindow::GetForCurrentThread()->Dispatcher->ProcessEvents(CoreProcessEventsOption::ProcessAllIfPresent);
                // 1. Update
                // Also exist in DirectX 11 App template: Update loop to get the latest game changes
                Update();
                
                // 2. Render
                // Present also in DirectX 11 App template: Prepare to render
                m_renderer->Render();
                
                // 3. Present
                // Present also in DirectX 11 App template: Present the 
                // contents of the swap chain to the screen.
                m_deviceResources->Present(); 
                
                // Added: resetting a variable we've added to indicate that 
                // render has been done and no longer needed to render
                m_renderNeeded = false; 
            }
        }
        else
        {   
            // Present also in DirectX 11 App template
            CoreWindow::GetForCurrentThread()->Dispatcher->ProcessEvents(CoreProcessEventsOption::ProcessOneAndAllPending); 
        }
    }
    m_game->OnSuspending();  // exiting due to window close. Make sure to save state.
}
```

### <a name="update"></a>更新

请参阅[游戏流管理](tutorial-game-flow-management.md)一文，了解有关如何在 [__App::Update__ 和 __GameMain::Update__](tutorial-game-flow-management.md#appupdate-method) 方法中更新游戏状态的详细信息。

### <a name="render"></a>呈现

呈现通过调用 __GameMain::Run__ 中的 [__GameRenderer::Render__](#gamerendererrender-method) 方法实现。

如果[立体呈现](#stereo-rendering)已启用，则有两个呈现通道：一个用于右眼，一个用于左眼。 在每个呈现通道中，我们将呈现目标和[深度模板视图](#depth-stencil-view)绑定到设备。 我们还会清除之后的深度模板视图。

> [!Note]
> 立体呈现可以使用其他方法实现，如使用顶点实例或几何着色器的单通道立体声。 双呈现通道方法速度较慢，但更便于实现立体呈现。

当游戏存在并加载资源后，更新[投影矩阵](#projection-transform-matrix)，每个呈现通道一次。 每个视图的对象略有不同。 接下来，设置[图形呈现管道](#rendering-pipeline)。 

> [!Note]
> 请参阅[创建和加载 DirectX 图形资源](tutorial-game-rendering.md#create-and-load-directx-graphic-resources)了解有关如何加载资源的详细信息。

在此游戏示例中，呈现器的用途是使用所有对象的标准顶点布局。 这简化了着色器的设计并允许在着色器之间进行轻松更改，不依赖于对象的几何结构。

#### <a name="gamerendererrender-method"></a>GameRenderer::Render 方法

设置 Direct3D 上下文以使用输入顶点布局。 输入布局对象介绍了如何将顶点缓冲区数据传输到[呈现管道](#rendering-pipeline)。 

接下来，我们设置 Direct3D 上下文以使用之前定义的[常量缓冲区](#constant-buffers)，其用于[顶点着色器](#vertex-shaders-and-pixel-shaders)管道阶段和[像素着色器](#vertex-shaders-and-pixel-shaders)管道阶段。 

> [!Note]
> 请参阅[呈现框架 II：游戏呈现](tutorial-game-rendering.md)了解有关常量缓冲区定义的详细信息。

由于为管道中的所有着色器使用相同的常量缓冲区输入布局和集，因此每一帧设置一次。

```cpp
void GameRenderer::Render()
{
    bool stereoEnabled = m_deviceResources->GetStereoState();

    auto d3dContext = m_deviceResources->GetD3DDeviceContext();
    auto d2dContext = m_deviceResources->GetD2DDeviceContext();

    int renderingPasses = 1;
    if (stereoEnabled)
    {
        renderingPasses = 2;
    }

    for (int i = 0; i < renderingPasses; i++)
    {
        // Iterate through the number of rendering passes to be completed.
        // 2 rendering passes if stereo is enabled
        if (i > 0)
        {
            // Doing the Right Eye View.
            ID3D11RenderTargetView *const targets[1] = { m_deviceResources->GetBackBufferRenderTargetViewRight() };

            // Resets render targets to the screen.
            // OMSetRenderTargets binds 2 things to the device.
            // 1. Binds one render target atomically to the device.
            // 2. Binds the depth-stencil view, as returned by the GetDepthStencilView method, to the device.
            // For more info, go to: https://msdn.microsoft.com/library/windows/desktop/ff476464.aspx

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
            
            // d2d -- Discussed later
            d2dContext->SetTarget(m_deviceResources->GetD2DTargetBitmapRight());
        }
        else
        {
            // Doing the Mono or Left Eye View.
            // As compared to the right eye:
            // m_deviceResources->GetBackBufferRenderTargetView instead of GetBackBufferRenderTargetViewRight
            ID3D11RenderTargetView *const targets[1] = { m_deviceResources->GetBackBufferRenderTargetView() }; 
            
            // Same as the Right Eye View.
            d3dContext->OMSetRenderTargets(1, targets, m_deviceResources->GetDepthStencilView());
            d3dContext->ClearDepthStencilView(m_deviceResources->GetDepthStencilView(), D3D11_CLEAR_DEPTH, 1.0f, 0);
            
            // d2d -- Discussed later under Adding UI
            d2dContext->SetTarget(m_deviceResources->GetD2DTargetBitmap()); 
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
                            m_game->GameCamera()->LeftEyeProjection() :
                            m_game->GameCamera()->RightEyeProjection()
                            ),
                        XMMatrixTranspose(XMLoadFloat4x4(&orientation))
                        )
                    );

                d3dContext->UpdateSubresource(
                    m_constantBufferChangeOnResize.Get(),
                    0,
                    nullptr,
                    &changesOnResize,
                    0,
                    0
                    );
            }

            // Update variables that change once per frame.
            ConstantBufferChangesEveryFrame constantBufferChangesEveryFrame;
            XMStoreFloat4x4(
                &constantBufferChangesEveryFrame.view,
                XMMatrixTranspose(m_game->GameCamera()->View())
                );
            d3dContext->UpdateSubresource(
                m_constantBufferChangesEveryFrame.Get(),
                0,
                nullptr,
                &constantBufferChangesEveryFrame,
                0,
                0
                );

            // Setup the graphics pipeline. This sample uses the same InputLayout and set of
            // constant buffers for all shaders, so they only need to be set once per frame.
            // For more info about the graphics or rendering pipeline, 
            // go to https://msdn.microsoft.com/library/windows/desktop/ff476882.aspx

            // IASetInputLayout binds an input-layout object to the input-assembler (IA) stage. 
            // Input-layout objects describe how vertex buffer data is streamed into the IA pipeline stage.
            // Set up the Direct3D context to use this vertex layout.
            // For more info, go to: https://msdn.microsoft.com/library/windows/desktop/ff476454.aspx
            d3dContext->IASetInputLayout(m_vertexLayout.Get());

            // VSSetConstantBuffers sets the constant buffers used by the vertex shader pipeline stage.
            // Set up the Direct3D context to use these constant buffers.
            // For more info, go to: https://msdn.microsoft.com/library/windows/desktop/ff476491.aspx

            d3dContext->VSSetConstantBuffers(0, 1, m_constantBufferNeverChanges.GetAddressOf());
            d3dContext->VSSetConstantBuffers(1, 1, m_constantBufferChangeOnResize.GetAddressOf());
            d3dContext->VSSetConstantBuffers(2, 1, m_constantBufferChangesEveryFrame.GetAddressOf());
            d3dContext->VSSetConstantBuffers(3, 1, m_constantBufferChangesEveryPrim.GetAddressOf());

            // Sets the constant buffers used by the pixel shader pipeline stage. 
            // For more info, go to: https://msdn.microsoft.com/library/windows/desktop/ff476470.aspx

            d3dContext->PSSetConstantBuffers(2, 1, m_constantBufferChangesEveryFrame.GetAddressOf());
            d3dContext->PSSetConstantBuffers(3, 1, m_constantBufferChangesEveryPrim.GetAddressOf());
            d3dContext->PSSetSamplers(0, 1, m_samplerLinear.GetAddressOf());

            auto objects = m_game->RenderObjects();
            for (auto object = objects.begin(); object != objects.end(); object++)
            {
                // The 3D object render method handles the rendering.
                // For more info, see Primitive rendering below.
                (*object)->Render(d3dContext, m_constantBufferChangesEveryPrim.Get()); /
            }
        }
        else
        {
            const float ClearColor[4] = {0.5f, 0.5f, 0.8f, 1.0f};

            // Only need to clear the background when not rendering the full 3D scene since
            // the 3D world is a fully enclosed box and the dynamics prevents the camera from
            // moving outside this space.
            if (i > 0)
            {
                // Doing the Right Eye View.
                d3dContext->ClearRenderTargetView(m_deviceResources->GetBackBufferRenderTargetViewRight(), ClearColor);
            }
            else
            {
                // Doing the Mono or Left Eye View.
                d3dContext->ClearRenderTargetView(m_deviceResources->GetBackBufferRenderTargetView(), ClearColor);
            }
        }

        // Start of 2D rendering
        d3dContext->BeginEventInt(L"D2D BeginDraw", 1);
        d2dContext->BeginDraw();

        // ...
    }
}
```

### <a name="primitive-rendering"></a>基元呈现

呈现场景时，将循环所有需要呈现的对象。 对每个对象（基元）重复以下步骤。

* 使用模型的[世界转换矩阵](#world-transform-matrix)和材料信息来更新常量缓冲区 (__m\_constantBufferChangesEveryPrim__)。
* __M\_constantBufferChangesEveryPrim__ 包含每个对象的参数。  它包括世界转换矩阵的对象，以及材料属性，如用于光线计算的颜色和反射指数。
* 设置 Direct3D 上下文以使用将传输到[呈现管道](#rendering-pipeline)输入装配器 (IA) 阶段的网格对象数据的输入顶点布局
* 设置 Direct3D 上下文以在 IA 阶段使用[索引缓冲区](#index-buffer)。 提供基元信息：类型、数据顺序。
* 提交绘图调用来绘制索引的非实例化基元。 __GameObject::Render__ 方法使用特定于给定基元的数据更新基元[常量缓冲区](#constant-buffer-or-shader-constant-buffer)。 这将导致在上下文中调用 __DrawIndexed__，以绘制每个基元的几何图形。 尤其是，此绘图调用会随着常量缓冲区数据的参数化将命令和数据编入图形处理单元 (GPU) 的队列。 每个绘图调用对每个顶点执行一次[顶点着色器](#vertex-shaders-and-pixel-shaders)，然后对基元中每个三角形的每个像素执行一次[像素着色器](#vertex-shaders-and-pixel-shaders)。 纹理是像素着色器用于执行呈现的状态部分。

多个常量缓冲区的原因：
    * 该游戏使用多个常量缓冲区，但每个基元只需更新一次这些缓冲区。 如之前所述，常量缓冲区就像对每个基元的着色器的输入。 一些数据是静态数据 (__m\_constantBufferNeverChanges__)；一些数据是 (__m\_constantBufferChangesEveryFrame__) 帧上的常量，如相机的位置；还有一些数据特定于基元，如其颜色和纹理 (__m\_constantBufferChangesEveryPrim__)
    * 游戏[呈现器](#renderer)将这些输入分别放入不同的常量缓冲区，以优化 CPU 和 GPU 使用的内存带宽。 此方法还有助于最大程度地减少 GPU 需要跟踪的数据量。 GPU 有一个很大的命令队列，游戏每次调用 __Draw__ 时，该命令将随与之关联的数据一起排队。 当游戏更新基元常量缓冲区并发出下一个 __Draw__ 命令时，图形驱动程序会将此下一个命令和关联的数据添加到队列。 如果游戏绘制 100 个基元，它可能在队列中有 100 个常量缓冲区数据的副本。 为了最大程度地减少游戏发送到 GPU 的数据量，游戏使用仅包含每个基元更新的单独基元常量缓冲区。

#### <a name="gameobjectrender-method"></a>GameObject::Render 方法

```cpp
void GameObject::Render(
    _In_ ID3D11DeviceContext *context,
    _In_ ID3D11Buffer *primitiveConstantBuffer
    )
{
    if (!m\_active || (m\_mesh == nullptr) || (m_normalMaterial == nullptr))
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

#### MeshObject::Render method

void MeshObject::Render(\_In\_ ID3D11DeviceContext *context)
{
    // PNTVertex is a struct. stride provides us the size required for all the mesh data
    // struct PNTVertex
    //{
    //  DirectX::XMFLOAT3 position;
    //  DirectX::XMFLOAT3 normal;
    //  DirectX::XMFLOAT2 textureCoordinate;
    //};
    uint32 stride = sizeof(PNTVertex);
    uint32 offset = 0;

    // Similar to the main render loop.
    // Input-layout objects describe how vertex buffer data is streamed into the IA pipeline stage.
    context->IASetVertexBuffers(0, 1, m_vertexBuffer.GetAddressOf(), &stride, &offset);

    // IASetIndexBuffer binds an index buffer to the input-assembler stage.
    // For more info, go to: https://msdn.microsoft.com/library/windows/desktop/ff476453.aspx
    context->IASetIndexBuffer(m_indexBuffer.Get(), DXGI_FORMAT_R16_UINT, 0);

    // Binds information about the primitive type, and data order that describes input data for the input assembler stage.
    // For more info, go to: https://msdn.microsoft.com/library/windows/desktop/ff476455.aspx
    context->IASetPrimitiveTopology(D3D11_PRIMITIVE_TOPOLOGY_TRIANGLELIST);

    // Draw indexed, non-instanced primitives. A draw API submits work to the rendering pipeline.
    // For more info, go to: https://msdn.microsoft.com/library/windows/desktop/ff476409.aspx
    context->DrawIndexed(m_indexCount, 0, 0);
}
```

### <a name="present"></a>显示

我们调用 __DX::DeviceResources::Present__ 方法放置已放入缓冲区的内容并显示这些内容。

我们为用于向用户显示帧的缓冲区集合使用术语交换链。 应用程序每次提供要显示的新帧时，交换链中的第一个缓冲区将替代已显示的缓冲区。 此过程称为交换或翻转。 有关详细信息，请参阅[交换链](../graphics-concepts/swap-chains.md)。

* __IDXGISwapChain1__ 接口的 __Present__ 方法指示 [DXGI](#dxgi) 阻止直到垂直同步 (VSync)，并将应用程序置于睡眠状态直到下一次 VSync。 这将确保不会浪费呈现永远不会显示到屏幕的帧的任何周期。
* __ID3D11DeviceContext3__ 接口的 __DiscardView__ 方法丢弃[呈现器目标](#render-target)的内容。 仅当现有内容将完全被覆盖时此操作才有效。 如果使用了异常或滚动矩形，应删除此调用。
* 使用相同的 __DiscardView__ 方法，丢弃[深度模板](#depth-stencil)的内容。
* 如果[设备](#device)被删除，__HandleDeviceLost__ 方法将用来管理此场景。 如果设备由于断开连接或驱动程序升级而被删除，则必须重新创建所有设备资源。 有关详细信息，请参阅[在 Direct3D 11 中处理设备删除方案](handling-device-lost-scenarios.md)。

> [!Tip]
> 若要实现流畅的帧速率，必须确保呈现帧的工作量适合 VSync 之间的时间。

```cpp
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
    m_d3dContext->DiscardView(m_d3dRenderTargetView.Get());

    // Discard the contents of the depth-stencil.
    m_d3dContext->DiscardView(m_d3dDepthStencilView.Get());

    // If the device was removed either by a disconnection or a driver upgrade, we 
    // must recreate all device resources.
    // For more info about how to handle a device lost scenario, go to:
    // https://docs.microsoft.com/windows/uwp/gaming/handling-device-lost-scenarios
    if (hr == DXGI_ERROR_DEVICE_REMOVED || hr == DXGI_ERROR_DEVICE_RESET)
    {
        HandleDeviceLost();
    }
    else
    {
        DX::ThrowIfFailed(hr);
    }
}
```

## <a name="next-steps"></a>后续步骤

本文介绍图形如何在屏幕上呈现，并提供了所使用的一些呈现术语的简短描述。 通过[呈现框架 II：游戏呈现](tutorial-game-rendering.md)文章了解有关呈现的详细信息，并了解如何在呈现之前准备所需的数据。

## <a name="terms-and-concepts"></a>术语和概念

### <a name="simple-game-scene"></a>简单的游戏场景

简单的游戏场景由具有若干光源的几个对象构成。

对象的形状由空间内的一组 X、Y、Z 坐标定义。 游戏世界中的实际呈现位置可以通过将转换矩阵应用于位置 X、Y、Z 坐标来确定。 它可能还有一组纹理坐标 - U 和 V，其指定材料如何应用到对象。 这定义对象的的图面属性，并使你能够看到对象是像网球一样的粗糙图面还是像保龄球一样光滑的图面。

场景和对象信息被呈现框架用于按帧重新创建场景，使其在显示器上栩栩如生。

### <a name="rendering-pipeline"></a>呈现管道

呈现管道是一个过程，在此过程中 3D 场景信息转换为在屏幕上显示的图像。 在 Direct3D 11 中，此管道是可编程的。 你可以调整各个阶段来支持你的呈现需求。 具有常见着色器核心的阶段可通过使用 HLSL 编程语言进行编程。 它也称为图形呈现管道或管道。

为了帮助你创建此管道，你需要熟悉：
* [HLSL](#HLSL)。 对于 UWP DirectX 游戏，我们建议使用 HLSL 着色器模型 5.1 及更高版本。
* [着色器](#Shaders)
* [顶点着色器和像素着色器](#vertext-shaders-pixel-shaders)
* [着色器阶段](#shader-stages)
* [各着色器文件格式](#various-shader-file-formats)

有关详细信息，请参阅[了解 Direct3D 11 呈现管道](https://msdn.microsoft.com/library/windows/desktop/dn643746.aspx)和[图形管道](https://msdn.microsoft.com/library/windows/desktop/ff476882.aspx)。

#### <a name="hlsl"></a>HLSL

HLSL 是适用于 DirectX 的高级着色语言。 使用 HLSL，可以创建用于 Direct3D 管道的类似于 C 的可编程着色器。 有关详细信息，请参阅 [HLSL](https://msdn.microsoft.com/library/windows/desktop/bb509561.aspx)。

#### <a name="shaders"></a>着色器

着色器可以看作是一套指令，确定对象在呈现时所显示的图面。 使用 HLSL 编程的着色器称为 HLSL 着色。 [HLSL])(#hlsl) 着色器的源代码文件具有 .hlsl 文件扩展名。 这些着色器可以在创作或运行时编译，并在运行时设置到相应的管道阶段；经过编译的着色器对象具有 .cso 文件扩展名。

Direct3D 9 着色器可以使用着色器模型 1、着色器模型 2 和着色器模型 3 设计；Direct3D 10 着色器只能基于着色器模型 4 设计。 Direct3D 11 着色器可以基于着色器模型 5 设计。 Direct3D 11.3 和 Direct3D 12 可以基于着色器模型 5.1 设计，Direct3D 12 还可以基于着色器模型 6 设计。

#### <a name="vertex-shaders-and-pixel-shaders"></a>顶点着色器和像素着色器

数据作为基原流进入图形管道并由各个着色器（如顶点着色器和像素着色器）处理。 

顶点着色器处理顶点，通常执行诸如转换、换肤以及照明之类的操作。  像素着色器支持丰富的着色技术，如每像素照明和后处理。 它将常变量、纹理数据、内插的每顶点值和其他数据组合起来以生成每像素输出。 

#### <a name="shader-stages"></a>着色器阶段

定义的用于处理此基元流的各个着色器的序列在呈现管道中称为着色器阶段。 实际阶段取决于 Direct3D 的版本，但通常包括顶点、几何图形和像素阶段。 还有其他一些阶段，如用于分割的外壳着色器和域着色器、计算着色器。 所有这些阶段均可使用 [HLSL])(#hlsl) 完全编程。 有关详细信息，请参阅[图形管道](https://msdn.microsoft.com/library/windows/desktop/ff476882.aspx)。

#### <a name="various-shader-file-formats"></a>各着色器文件格式

着色器代码文件扩展名：
    * 扩展名为 .hlsl 的文件保留 [HLSL])(#hlsl) 源代码。
    * 扩展名为 .cso 的文件保留经过编译的着色器对象。
    * 扩展名为 .h 的文件是头文件，但在着色器代码上下文中，此头文件定义保留着色器数据的字节数组。
    * 扩展名为 .hlsli 的文件包含常量缓冲区的格式。 在游戏示例中，该文件为 __着色器__>__ConstantBuffers.hlsli__。

> [!Note]
> 你将通过在运行时加载 .cso 文件或在可执行代码中添加 .h 文件来嵌入此着色器。 但不能同时添加相同的着色器。

### <a name="deeper-understanding-of-directx"></a>更深入地了解 DirectX

Direct3D 11 是一组 API，可帮助我们为图形密集应用程序（如游戏）创建图形，这时我们需要一个优质的显卡来处理密集计算。 此部分简要说明 Direct3D 11 图形编程概念：资源、子资源、设备和设备上下文。

#### <a name="resource"></a>资源

对于新手，你可以将资源（也称为设备资源）视为有关如何呈现对象（如纹理、位置、颜色）的信息。 资源向管道提供数据，并定义在场景中呈现的内容。 资源可以从游戏媒体加载或在运行时动态创建。

资源实际上是内存中可由 Direct3D [管道](#rendering-pipeline)访问的区域。 为了使管道能够高效地访问内存，提供给管道的数据（如输入几何图形、着色器资源和纹理）必须存储在资源中。 所有 Direct3D 资源都存储在两种类型的资源中：缓冲区或纹理。 每个管道阶段最多可以有 128 个活动资源。 有关详细信息，请参阅[资源](../graphics-concepts/resources.md)。

#### <a name="subresource"></a>子资源

术语子资源指资源的子集。 Direct3D 可以参考整个资源或参考资源的子集。 有关详细信息，请参阅[子资源](../graphics-concepts/resource-types.md#subresources)。

#### <a name="depth-stencil"></a>深度模板

深度模板资源包含用于保留深度和模板信息的格式和缓冲区。 其使用纹理资源创建。 有关如何创建深度模板资源的详细信息，请参阅[配置深度模板功能](https://msdn.microsoft.com/library/windows/desktop/bb205074.aspx)。 我们通过使用 [ID3D11DepthStencilView](https://msdn.microsoft.com/library/windows/desktop/ff476377.aspx) 界面实现的深度模板视图访问深度模板资源。

深度信息告知我们在视图中呈现哪些多边形区域（而不是隐藏）。 模板信息告知我们会屏蔽哪些像素。 可以使用它来产生特殊效果，因为它确定是否绘制某个像素；将位设置为 1 或 0。 

有关详细信息，请参阅：[深度模板视图](../graphics-concepts/depth-stencil-view--dsv-.md)、[深度缓冲区](../graphics-concepts/depth-buffers.md)和[模板缓冲区](../graphics-concepts/stencil-buffers.md)。

#### <a name="render-target"></a>呈现器目标

呈现器目标是我们可以在呈现传递末尾写入的资源。 它通常使用 [ID3D11Device::CreateRenderTargetView](https://msdn.microsoft.com/library/windows/desktop/ff476517.aspx) 方法、将交换链后台缓冲区（也是一种资源）用作输入参数来创建。 

每个呈现器目标还应有一个相应的深度模板视图，因为当我们在使用呈现目标前使用 [OMSetRenderTargets](https://msdn.microsoft.com/library/windows/desktop/ff476464.aspx) 设置它时，它也需要深度模板视图。 我们通过使用 [ID3D11RenderTargetView](https://msdn.microsoft.com/library/windows/desktop/ff476582.aspx) 界面实现的呈现器目标视图访问呈现器目标资源。 

#### <a name="device"></a>设备

对于不熟悉 Direct3D 11 的用户，你可以将设备设想为用于分配和销毁对象、呈现基元，并通过图形驱动程序与显卡通信的方式。 

更准确地说，Direct3D 设备是 Direct3D 的渲染组件。 设备封装并存储渲染状态，执行转换和照明操作，并将图像光栅化到表面。 有关详细信息，请参阅[设备](../graphics-concepts/devices.md)

设备由 [ID3D11Device](https://msdn.microsoft.com/library/windows/desktop/ff476379.aspx) 界面表示。 换言之，ID3D11Device 界面表示虚拟显示适配器，用于创建设备所拥有的资源。 

请注意，有不同版本的 ID3D11Device，[ID3D11Device5](https://msdn.microsoft.com/library/windows/desktop/mt492478.aspx) 是最新版本，向 ID3D11Device4 中添加了新方法。 有关 Direct3D 如何与基础硬件通信的详细信息，请参阅 [Windows 设备驱动程序 (WDDM) 体系结构](https://docs.microsoft.com/windows-hardware/drivers/display/windows-vista-and-later-display-driver-model-architecture)。

每个应用程序都必须有至少一台设备，大多数应用程序只能创建一台设备。 通过调用 __D3D11CreateDevice__ 或 __D3D11CreateDeviceAndSwapChain__ 并使用 D3D\_DRIVER\_TYPE 标记指定驱动程序类型，为在计算机上安装的一个硬件驱动程序创建设备。 每台设备可以使用一个或多个设备上下文，具体取决于所需的功能。 有关详细信息，请参阅 [D3D11CreateDevice 函数](https://msdn.microsoft.com/library/windows/desktop/ff476082.aspx)。

#### <a name="device-context"></a>设备上下文

设备上下文用来设置[管道](#rendering-pipeline)状态以及使用[设备](#device)所拥有的[资源](#resource)来生成呈现命令。 

Direct3D 11 实现两种类型的设备上下文，一个用于立即呈现，另一个用于延迟呈现；两个上下文均使用 [ID3D11DeviceContext](https://msdn.microsoft.com/library/windows/desktop/ff476385.aspx) 界面表示。  

__ID3D11DeviceContext__ 界面具有不同版本；__ID3D11DeviceContext4__ 向 __ID3D11DeviceContext3__ 中的界面添加新方法。

注意：__ID3D11DeviceContext4__ 在 Windows 10 创意者更新中推出，是最新版的 __ID3D11DeviceContext__ 界面。 面向 Windows 10 创意者更新的应用程序应该使用此界面，而不是早期版本。 有关详细信息，请参阅 [ID3D11DeviceContext4](https://msdn.microsoft.com/library/windows/desktop/mt492481.aspx)。

#### <a name="dxdeviceresources"></a>DX::DeviceResources

__DX::DeviceResources__ 类位于 __DeviceResources.cpp__/__.h__文件中，控制所有 DirectX 设备资源。 在示例游戏项目和 DirectX 11 应用模板项目中，这些文件在 __Commons__ 文件夹中。 在 Visual Studio 2015 或更高版本中创建新的 DirectX 11 应用模板项目时，你可以获取这些文件的最新版本。

### <a name="buffer"></a>缓冲区

缓冲区资源是一系列完全类型化的数据，被分组到元素中。 你可以使用缓冲区来存储各类数据，包括位置矢量、法向矢量、顶点缓冲区中的纹理坐标、索引缓冲区中的索引或设备状态。 缓冲区元素可以包含打包数据值（如 R8G8B8A8 表面值）、单个 8 位整数或四个 32 位浮点值。

有三种类型的缓冲区：顶点缓冲区、索引缓冲区和常量缓冲区。

#### <a name="vertex-buffer"></a>顶点缓冲区

包含用于定义几何图形的顶点数据。 顶点数据包括位置坐标、颜色数据、纹理坐标数据、法线数据等。 

#### <a name="index-buffer"></a>索引缓冲区

包含到顶点缓冲区的整数偏移量，用于更高效地渲染基元。 索引缓冲区包含一组连续的 16 位或 32 位索引；每个索引用于标识顶点缓冲区中的一个顶点。

#### <a name="constant-buffer-or-shader-constant-buffer"></a>常量缓冲区或着色器常量缓冲区

让你能够高效地向管道提供着色器数据。 你可以将常量缓冲区用作为每个基元运行以及存储呈现管道的流输出阶段结果的着色器的输入。 从概念上讲，常量缓冲区看起来就像单元素顶点缓冲区。

#### <a name="design-and-implementation-of-buffers"></a>缓冲区的设计和实现

你可以基于数据类型设计缓冲区，例如，就像在我们的游戏示例中，一个缓冲区为静态数据创建，另一个为属于帧上的常量的数据创建，还有一个为特定于基元的数据创建。

所有缓冲区类型由 __ID3D11Buffer__ 界面封装，你可以通过调用 __ID3D11Device::CreateBuffer__ 创建缓冲区资源。 但缓冲区必须绑定到管道后才能访问。 缓冲区可以同时绑定到多个管道阶段来用于读取。 缓冲区还可以绑定到单个管道阶段来用于写入；但是，不能为读取和写入同时绑定相同的缓冲区。

将缓冲区绑定到：
    * 通过调用 __ID3D11DeviceContext__ 方法（如 __ID3D11DeviceContext::IASetVertexBuffers__ 和 __ID3D11DeviceContext::IASetIndexBuffer__）的输入装配器阶段
    * 通过调用 __ID3D11DeviceContext::SOSetTargets__ 的流输出阶段
    * 通过调用着色器方法（如 __ID3D11DeviceContext::VSSetConstantBuffers__）的着色器阶段

有关详细信息，请参阅 [Direct3D 11 中的缓冲区简介](https://msdn.microsoft.com/library/windows/desktop/ff476898.aspx)。

### <a name="dxgi"></a>DXGI

Microsoft DirectX 图形基础结构 (DXGI) 是一个新子系统中引入的 WindowsVista 封装的一些低级别任务所需的 Direct3D 10、 10.1、 11 和 11.1。 在多线程应用程序中使用 DXGI 时需要格外小心，以确保不会发生死锁。 有关详细信息，请参阅 [DirectX 图形基础结构 (DXGI)：最佳做法-多线程](https://msdn.microsoft.com/library/windows/desktop/ee417025.aspx#multithreading_and_dxgi)

### <a name="feature-level"></a>功能级别

功能级别是 Direct3D 11 中引入的概念，用于处理新的或现有计算机中的各类视频卡。 功能级别是明确定义的图形处理单元 (GPU) 功能的集合。 

每个视频卡根据所安装的 GPU 来实现特定级别的 DirectX 功能。 在以前版本的 Microsoft Direct3D 中，你可以找到视频卡实现的 Direct3D 版本，然后相应地对应用程序编程。 

使用功能级别，在创建设备时，你可以尝试为想要请求的功能级别创建设备。 如果设备创建成功，该功能级别将存在，如果失败，硬件将不支持该功能级别。 你可以尝试在更低的功能级别重新创建设备，也可以选择退出应用程序。 例如，12\_0 功能级别需要 Direct3D 11.3 或 Direct3D 12，以及着色器模型 5.1。 有关详细信息，请参阅 [Direct3D 功能级别：各功能级别概述](https://msdn.microsoft.com/library/windows/desktop/ff476876.aspx#Overview)。

使用功能级别，你可以开发适用于 Direct3D9、 Microsoft Direct3D10 或 Direct3D11，应用程序，然后在 9、 10 或 11 硬件 （有某些例外） 上运行。 有关详细信息，请参阅 [Direct3D 功能级别](https://msdn.microsoft.com/library/windows/desktop/ff476876.aspx)。

### <a name="stereo-rendering"></a>立体呈现

立体呈现用于增强深度的视觉效果。 它使用两个图像，一个从左眼、另一个从右眼来在显示屏幕上显示场景。 

通过数学方式，我们应用常规单一投影矩阵的立体投影矩阵（稍微向右或向左水平偏移）来实现此目的。

我们通过两个呈现通道来在此示例游戏中实现立体呈现：
* 绑定到右侧的呈现器目标，应用右投影，然后绘制基元对象。
* 绑定到左侧的呈现器目标，应用右投影，然后绘制基元对象。

### <a name="camera-and-coordinate-space"></a>相机和坐标空间

该游戏有现成的代码用于更新本身坐标系中的世界（有时称为世界空间或场景空间）。 所有对象（包括相机）都在此空间定位和确定方向。 有关详细信息，请参阅[坐标系统](../graphics-concepts/coordinate-systems.md)。

顶点着色器使用以下算法执行从模型坐标到设备坐标的转换（其中 V 是一个矢量，M 是一个矩阵）。

V(device) = V(model) x M(model-to-world) x M(world-to-view) x M(view-to-device).

其中： 
* M(model-to-world) 是模型坐标到世界坐标的转换矩阵，也称为[世界转换矩阵](#world-transform-matrix)。 这由基元提供。
* M(world-to-view) 是世界坐标到视图坐标的转换矩阵，也称为[视图转换矩阵](#view-transform-matrix)。
    * 这由相机的视图矩阵提供。 它由相机的位置和观看矢量（从相机直接进入场景的“观看”矢量和与其垂直向上的“仰望”矢量）定义
    * 在示例游戏中，__m\_viewMatrix__ 是视图转换矩阵，使用 __Camera::SetViewParams__ 计算 
* M(view-to-device) 是视图坐标到设备坐标的转换矩阵，也称为[投影转换矩阵](#projection-transform-matrix)
    * 这由相机的投影提供。 它提供该空间在最终场景中有多少实际可见的信息。 视区 (FoV)、纵横比和剪裁平面定义投影转换矩阵。
    * 在示例游戏中，__m\_projectionMatrix__ 定义到投影坐标的转换，它使用 __Camera::SetProjParams__ 计算（对于立体投影，使用两个投影矩阵：每只眼睛的视角各一个）。 

VertexShader.hlsl 中的着色器代码随这些矢量和矩阵从常量缓冲区加载，并对每个顶点执行此转换。

### <a name="coordinate-transformation"></a>坐标转换

Direct3D 使用三个转换来将 3D 模型坐标更改为像素坐标（屏幕空间）。 这些转换是世界转换、视图转换和投影转换。 有关详细信息，请转到[转换概述](../graphics-concepts/transform-overview.md)。

#### <a name="world-transform-matrix"></a>世界转换矩阵

世界转换将模型空间（在此空间内，顶点是相对于模型的本地原点定义的）的坐标更改为世界空间（在此空间中，顶点是相对场景中所有对象共同的原点定义的）。 本质上，世界转换将模型放入世界空间中；然后是它的名称。 有关详细信息，请参阅[世界转换](../graphics-concepts/world-transform.md)。

####  <a name="view-transform-matrix"></a>视图转换矩阵

视图转换在世界空间中定位查看器，并将顶点转换为相机空间。 在相机空间中，相机或查看器位于原点，并面向正 z 方向。 有关详细信息，请转到[视图转换](../graphics-concepts/view-transform.md)。

####  <a name="projection-transform-matrix"></a>投影转换矩阵

投影转换可将视锥转换为长方体形状。 视锥是场景中相对于视区的相机放置的 3D 体。 视区是将 3D 场景投影到其中的 2D 矩形。 有关详细信息，请参阅[视区和剪切](../graphics-concepts/viewports-and-clipping.md)。

由于视锥的近端小于远端，这将产生拉伸靠近相机的对象的效果；这是透视应用于场景的方式。 因此，越靠近玩家的对象看起来越大；越远的则越小。

在数学上，投影转换是一个通常同时是缩放和透视投影的矩阵。 它像相机镜头一样工作。 有关详细信息，请参阅[投影转换](../graphics-concepts/projection-transform.md)。

### <a name="sampler-state"></a>取样器状态

取样器状态确定如何使用纹理寻址模式、筛选和详细级别对纹理数据采样。 每次从纹理读取纹理像素或纹素时完成采样。

纹理包含一组纹素或纹理像素。 每个纹素的位置由 (u,v) 表示，其中 u 是宽度，v 是高度，并根据纹理宽度和高度与 0-1 对应。 生成的纹理坐标用于在对纹理采样时对纹素寻址。

当纹理坐标小于 0 或大于 1 时，纹理寻址模式定义纹理坐标如何寻址纹素位置。 例如，在使用 __TextureAddressMode.Clamp__ 时，任何 0-1 范围以外的坐标在采样之前都将被限定为最大值为 1，最小值为 0。

如果纹理对于多边形太大或太小，将筛选适合该空间的纹理。 放大筛选器放大纹理、缩小筛选器缩小纹理以适应更小的区域。 纹理放大为生成更模糊图像的一个或多个地址重复示例纹素。 纹理缩小更加复杂，因为它需要将多个纹素值合并为一个值。 这可能导致失真或锯齿形边缘，具体取决于纹理数据。 最受欢迎的缩小方法是使用 mipmap。 Mipmap 是多级纹理。 每个级别的大小都比上一级小二次方，一直到 1x1 纹理。 使用缩小时，游戏选择最接近呈现时所需大小的 mipmap 级别。 

### <a name="basicloader"></a>BasicLoader

__BasicLoader__ 是一个简单的加载器类，它为从磁盘上的文件加载着色器、纹理和网格提供支持。 它提供同步和异步方法。 在此游戏示例中，BasicLoader.h/.cpp 文件位于 __Commons__ 文件夹中。

有关详细信息，请参阅[基本加载器](complete-code-for-basicloader.md)。