---
author: mtoepke
title: "将阴影映射呈现到深度缓冲区"
description: "从光线的角度呈现，以创建一个表示阴影卷的二维深度映射。"
ms.assetid: 7f3d0208-c379-8871-cc48-027047c6c2d0
translationtype: Human Translation
ms.sourcegitcommit: 6530fa257ea3735453a97eb5d916524e750e62fc
ms.openlocfilehash: 337aa63ee30b05da51d5b224cb0013519e11504d

---

# 将阴影映射呈现到深度缓冲区


\[ 已针对 Windows 10 上的 UWP 应用更新。 有关 Windows 8.x 文章，请参阅[存档](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


从光线的角度呈现，以创建一个表示阴影卷的二维深度映射。 深度映射会掩盖将在阴影中呈现的空间。 [操作实例：使用 Direct3D 11 中的深度缓冲区实现阴影卷](implementing-depth-buffers-for-shadow-mapping.md)的第 2 部分。

## 清除深度缓冲区


呈现到深度缓冲区之前，始终清除深度缓冲区。

```cpp
context->ClearRenderTargetView(m_deviceResources->GetBackBufferRenderTargetView(), DirectX::Colors::CornflowerBlue);
context->ClearDepthStencilView(m_shadowDepthView.Get(), D3D11_CLEAR_DEPTH | D3D11_CLEAR_STENCIL, 1.0f, 0);
```

## 将阴影映射呈现到深度缓冲区


对于阴影呈现通道，指定深度缓冲区，但不指定呈现目标。

指定光线视区、顶点着色器，并设置光线空间常量缓冲区。 为该通道使用正面剔除以优化放置在阴影缓冲区中的深度值。

请注意，在大多数设备上，你可以为像素着色器指定 nullptr（或者完全跳过指定像素着色器）。 但一些驱动程序可能会在像素着色器集为 null 的 Direct3D 设备上调用 draw 时引发异常。 为了避免发生此异常，你可以为阴影呈现通道设置最低像素着色器。 扔掉该着色器的输出；它可以在每个像素上调用 [**discard**](https://msdn.microsoft.com/library/windows/desktop/bb943995)。

呈现可以投影的对象，但不要打扰没有投影的呈现几何图形（就像房间中的地板，或者为了优化而从阴影通道中删除的对象）。

```cpp
void ShadowSceneRenderer::RenderShadowMap()
{
    auto context = m_deviceResources->GetD3DDeviceContext();

    // Render all the objects in the scene that can cast shadows onto themselves or onto other objects.

    // Only bind the ID3D11DepthStencilView for output.
    context->OMSetRenderTargets(
        0,
        nullptr,
        m_shadowDepthView.Get()
        );

    // Note that starting with the second frame, the previous call will display
    // warnings in VS debug output about forcing an unbind of the pixel shader
    // resource. This warning can be safely ignored when using shadow buffers
    // as demonstrated in this sample.

    // Set rendering state.
    context->RSSetState(m_shadowRenderState.Get());
    context->RSSetViewports(1, &m_shadowViewport);

    // Each vertex is one instance of the VertexPositionTexNormColor struct.
    UINT stride = sizeof(VertexPositionTexNormColor);
    UINT offset = 0;
    context->IASetVertexBuffers(
        0,
        1,
        m_vertexBuffer.GetAddressOf(),
        &stride,
        &offset
        );

    context->IASetIndexBuffer(
        m_indexBuffer.Get(),
        DXGI_FORMAT_R16_UINT, // Each index is one 16-bit unsigned integer (short).
        0
        );

    context->IASetPrimitiveTopology(D3D11_PRIMITIVE_TOPOLOGY_TRIANGLELIST);
    context->IASetInputLayout(m_inputLayout.Get());

    // Attach our vertex shader.
    context->VSSetShader(
        m_simpleVertexShader.Get(),
        nullptr,
        0
        );

    // Send the constant buffers to the Graphics device.
    context->VSSetConstantBuffers(
        0,
        1,
        m_lightViewProjectionBuffer.GetAddressOf()
        );

    context->VSSetConstantBuffers(
        1,
        1,
        m_rotatedModelBuffer.GetAddressOf()
        );

    // In some configurations, it's possible to avoid setting a pixel shader
    // (or set PS to nullptr). Not all drivers are tolerant of this, so to be
    // safe set a minimal shader here.
    //
    // Direct3D will discard output from this shader because the render target
    // view is unbound.
    context->PSSetShader(
        m_textureShader.Get(),
        nullptr,
        0
        );

    // Draw the objects.
    context->DrawIndexed(
        m_indexCountCube,
        0,
        0
        );
}
```

**优化视锥：**确保你的实现计算一个严密的视锥，以便在深度缓冲区之外获得最大精度。 有关阴影技术的更多提示，请参阅[改进阴影深度映射的常用技术](https://msdn.microsoft.com/library/windows/desktop/ee416324)。

## 阴影通道的顶点着色器


使用简化版的顶点着色器在光线空间中仅呈现顶点位置。 不要包含任何照明法线、二次转换等。

```cpp
PixelShaderInput main(VertexShaderInput input)
{
    PixelShaderInput output;
    float4 pos = float4(input.pos, 1.0f);

    // Transform the vertex position into projected space.
    pos = mul(pos, model);
    pos = mul(pos, view);
    pos = mul(pos, projection);
    output.pos = pos;

    return output;
}
```

在本演练的下一部分中，了解如何通过[使用深度测试进行呈现](render-the-scene-with-depth-testing.md)来添加阴影。

 

 







<!--HONumber=Aug16_HO3-->


