---
author: mtoepke
title: 创建着色器和绘制基元
description: 在此处，我们将向你介绍如何使用 HLSL 源文件来编译和创建着色器，这些着色器可用于在显示器上绘制基元。
ms.assetid: 91113bbe-96c9-4ef9-6482-39f1ff1a70f4
ms.author: mtoepke
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, 游戏, 着色器, 基元, directx
ms.localizationpriority: medium
ms.openlocfilehash: 475a69837796b0b64be27c96f10b42d5b61390c1
ms.sourcegitcommit: 6cc275f2151f78db40c11ace381ee2d35f0155f9
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/25/2018
ms.locfileid: "5546722"
---
# <a name="create-shaders-and-drawing-primitives"></a>创建着色器和绘制基元



我们将在此向你介绍如何使用 HLSL 源文件来编译和创建着色器，这些着色器可用于在屏幕上绘制基元。

我们将使用顶点着色器和像素着色器来创建并绘制一个黄色的三角形。 在创建了 Direct3D 设备、交换链和呈现器目标视图之后，我们将从磁盘上的二进制着色器对象文件中读取数据。

**目标：** 创建着色器并绘制基元。

## <a name="prerequisites"></a>先决条件


我们假定你熟悉 C++。 你还需要具有图形编程概念方面的基本经验。

我们还假定你已阅读[快速入门：设置 DirectX 资源并显示图像](setting-up-directx-resources.md)。

**完成所需时间：** 20 分钟。

## <a name="instructions"></a>说明

### <a name="1-compiling-hlsl-source-files"></a>1. 编译 HLSL 源文件

Microsoft Visual Studio 使用 [fxc.exe](https://msdn.microsoft.com/library/windows/desktop/bb232919) HLSL 代码编译器将 .hlsl 源文件（SimpleVertexShader.hlsl 和 SimplePixelShader.hlsl）编译为 .cso 二进制着色器对象文件（SimpleVertexShader.cso 和 SimplePixelShader.cso）。 有关 HLSL 代码编译器的详细信息，请参阅效果编译器工具。 有关编译着色器代码的详细信息，请参阅[编译着色器](https://msdn.microsoft.com/library/windows/desktop/bb509633)。

下面是 SimpleVertexShader.hlsl 中的代码：

```hlsl
struct VertexShaderInput
{
    DirectX::XMFLOAT2 pos : POSITION;
};

struct PixelShaderInput
{
    float4 pos : SV_POSITION;
};

PixelShaderInput SimpleVertexShader(VertexShaderInput input)
{
    PixelShaderInput vertexShaderOutput;

    // For this lesson, set the vertex depth value to 0.5, so it is guaranteed to be drawn.
    vertexShaderOutput.pos = float4(input.pos, 0.5f, 1.0f);

    return vertexShaderOutput;
}
```

下面是 SimplePixelShader.hlsl 中的代码：

```hlsl
struct PixelShaderInput
{
    float4 pos : SV_POSITION;
};

float4 SimplePixelShader(PixelShaderInput input) : SV_TARGET
{
    // Draw the entire triangle yellow.
    return float4(1.0f, 1.0f, 0.0f, 1.0f);
}
```

### <a name="2-reading-data-from-disk"></a>2. 从磁盘读取数据

我们使用 DirectX 11 应用（通用 Windows）模板中的 DirectXHelper.h DX::ReadDataAsync 函数以异步方式从磁盘上的文件中读取数据。

### <a name="3-creating-vertex-and-pixel-shaders"></a>3. 创建顶点着色器和像素着色器

从 SimpleVertexShader.cso 文件读取数据，并将该数据分配给 *vertexShaderBytecode* 字节数组。 使用字节数组调用 [**ID3D11Device::CreateVertexShader**](https://msdn.microsoft.com/library/windows/desktop/ff476524) 来创建顶点着色器 ([**ID3D11VertexShader**](https://msdn.microsoft.com/library/windows/desktop/ff476641))。 在 SimpleVertexShader.hlsl 源中将顶点深度值设置为 0.5，以保证绘制出三角形。 填充 [**D3D11\_INPUT\_ELEMENT\_DESC**](https://msdn.microsoft.com/library/windows/desktop/ff476180) 结构数组来描述顶点着色器代码的布局，然后调用 [**ID3D11Device::CreateInputLayout**](https://msdn.microsoft.com/library/windows/desktop/ff476512) 来创建布局。 该数组仅有一个定义顶点位置的布局元素。 从 SimplePixelShader.cso 文件读取数据，并将该数据分配给 *pixelShaderBytecode* 字节数组。 使用字节数组调用 [**ID3D11Device::CreatePixelShader**](https://msdn.microsoft.com/library/windows/desktop/ff476513) 来创建像素着色器 ([**ID3D11PixelShader**](https://msdn.microsoft.com/library/windows/desktop/ff476576))。 在 SimplePixelShader.hlsl 源中将像素值设置为 (1,1,1,1)，使三角形变为黄色。 你可以通过更改此值来更改颜色。

创建用来定义简单三角形的顶点缓冲区和索引缓冲区。 为此，首先定义该三角形，然后使用三角形定义描述顶点缓冲区和索引缓冲区（[**D3D11\_BUFFER\_DESC**](https://msdn.microsoft.com/library/windows/desktop/ff476092) 和 [**D3D11\_SUBRESOURCE\_DATA**](https://msdn.microsoft.com/library/windows/desktop/ff476220)），最后为每个缓冲区均调用一次 [**ID3D11Device::CreateBuffer**](https://msdn.microsoft.com/library/windows/desktop/ff476501)。

```cpp
        auto loadVSTask = DX::ReadDataAsync(L"SimpleVertexShader.cso");
        auto loadPSTask = DX::ReadDataAsync(L"SimplePixelShader.cso");
        
        // Load the raw vertex shader bytecode from disk and create a vertex shader with it.
        auto createVSTask = loadVSTask.then([this](const std::vector<byte>& vertexShaderBytecode) {


          ComPtr<ID3D11VertexShader> vertexShader;
          DX::ThrowIfFailed(
              m_d3dDevice->CreateVertexShader(
                  vertexShaderBytecode->Data,
                  vertexShaderBytecode->Length,
                  nullptr,
                  &vertexShader
                  )
              );

          // Create an input layout that matches the layout defined in the vertex shader code.
          // For this lesson, this is simply a DirectX::XMFLOAT2 vector defining the vertex position.
          const D3D11_INPUT_ELEMENT_DESC basicVertexLayoutDesc[] =
          {
              { "POSITION", 0, DXGI_FORMAT_R32G32_FLOAT, 0, 0, D3D11_INPUT_PER_VERTEX_DATA, 0 },
          };

          ComPtr<ID3D11InputLayout> inputLayout;
          DX::ThrowIfFailed(
              m_d3dDevice->CreateInputLayout(
                  basicVertexLayoutDesc,
                  ARRAYSIZE(basicVertexLayoutDesc),
                  vertexShaderBytecode->Data,
                  vertexShaderBytecode->Length,
                  &inputLayout
                  )
              );
        });
        
        // Load the raw pixel shader bytecode from disk and create a pixel shader with it.
        auto createPSTask = loadPSTask.then([this](const std::vector<byte>& pixelShaderBytecode) {
          ComPtr<ID3D11PixelShader> pixelShader;
          DX::ThrowIfFailed(
              m_d3dDevice->CreatePixelShader(
                  pixelShaderBytecode->Data,
                  pixelShaderBytecode->Length,
                  nullptr,
                  &pixelShader
                  )
              );
        });

        // Create vertex and index buffers that define a simple triangle.
        auto createTriangleTask = (createPSTask && createVSTask).then([this] () {

          DirectX::XMFLOAT2 triangleVertices[] =
          {
              float2(-0.5f, -0.5f),
              float2( 0.0f,  0.5f),
              float2( 0.5f, -0.5f),
          };

          unsigned short triangleIndices[] =
          {
              0, 1, 2,
          };

          D3D11_BUFFER_DESC vertexBufferDesc = {0};
          vertexBufferDesc.ByteWidth = sizeof(float2) * ARRAYSIZE(triangleVertices);
          vertexBufferDesc.Usage = D3D11_USAGE_DEFAULT;
          vertexBufferDesc.BindFlags = D3D11_BIND_VERTEX_BUFFER;
          vertexBufferDesc.CPUAccessFlags = 0;
          vertexBufferDesc.MiscFlags = 0;
          vertexBufferDesc.StructureByteStride = 0;

          D3D11_SUBRESOURCE_DATA vertexBufferData;
          vertexBufferData.pSysMem = triangleVertices;
          vertexBufferData.SysMemPitch = 0;
          vertexBufferData.SysMemSlicePitch = 0;

          ComPtr<ID3D11Buffer> vertexBuffer;
          DX::ThrowIfFailed(
              m_d3dDevice->CreateBuffer(
                  &vertexBufferDesc,
                  &vertexBufferData,
                  &vertexBuffer
                  )
              );

          D3D11_BUFFER_DESC indexBufferDesc;
          indexBufferDesc.ByteWidth = sizeof(unsigned short) * ARRAYSIZE(triangleIndices);
          indexBufferDesc.Usage = D3D11_USAGE_DEFAULT;
          indexBufferDesc.BindFlags = D3D11_BIND_INDEX_BUFFER;
          indexBufferDesc.CPUAccessFlags = 0;
          indexBufferDesc.MiscFlags = 0;
          indexBufferDesc.StructureByteStride = 0;

          D3D11_SUBRESOURCE_DATA indexBufferData;
          indexBufferData.pSysMem = triangleIndices;
          indexBufferData.SysMemPitch = 0;
          indexBufferData.SysMemSlicePitch = 0;

          ComPtr<ID3D11Buffer> indexBuffer;
          DX::ThrowIfFailed(
              m_d3dDevice->CreateBuffer(
                  &indexBufferDesc,
                  &indexBufferData,
                  &indexBuffer
                  )
              );
        });
```

使用顶点着色器和像素着色器、顶点着色器布局以及顶点缓冲区和索引缓冲区来缓制一个黄色三角形。

### <a name="4-drawing-the-triangle-and-presenting-the-rendered-image"></a>4. 绘制三角形并显示呈现的图像

我们将进入一个不断呈现和显示场景的无限循环。 调用 [**ID3D11DeviceContext::OMSetRenderTargets**](https://msdn.microsoft.com/library/windows/desktop/ff476464) 以将呈现器目标指定为输出目标。 调用 [**ID3D11DeviceContext::ClearRenderTargetView**](https://msdn.microsoft.com/library/windows/desktop/ff476388)，通过 { 0.071f, 0.04f, 0.561f, 1.0f } 将呈现器目标清空为纯蓝色。

在该无限循环中，我们需要在蓝色图面上绘制一个黄色三角形。

**绘制黄色三角形的步骤**

1.  首先，调用 [**ID3D11DeviceContext::IASetInputLayout**](https://msdn.microsoft.com/library/windows/desktop/ff476454) 来描述如何将顶点缓冲区数据流传输到输入程序集阶段。
2.  接着，调用 [**ID3D11DeviceContext::IASetVertexBuffers**](https://msdn.microsoft.com/library/windows/desktop/ff476456) 和 [**ID3D11DeviceContext::IASetIndexBuffer**](https://msdn.microsoft.com/library/windows/desktop/ff476453) 将顶点缓冲区和索引缓冲区绑定到输入程序集阶段。
3.  接着，调用 [**ID3D11DeviceContext::IASetPrimitiveTopology**](https://msdn.microsoft.com/library/windows/desktop/ff476455)，使用 [**D3D11\_PRIMITIVE\_TOPOLOGY\_TRIANGLESTRIP**](https://msdn.microsoft.com/library/windows/desktop/ff476189#D3D11_PRIMITIVE_TOPOLOGY_TRIANGLESTRIP) 值指定由输入程序集阶段将顶点数据解释为三角形带。
4.  接着，调用 [**ID3D11DeviceContext::VSSetShader**](https://msdn.microsoft.com/library/windows/desktop/ff476493) 以使用顶点着色器代码初始化顶点着色器阶段，并调用 [**ID3D11DeviceContext::PSSetShader**](https://msdn.microsoft.com/library/windows/desktop/ff476472) 以使用像素着色器代码初始化像素着色器阶段。
5.  最后，调用 [**ID3D11DeviceContext::DrawIndexed**](https://msdn.microsoft.com/library/windows/desktop/ff476409) 绘制三角形并将其提交给呈现管道。

调用 [**IDXGISwapChain::Present**](https://msdn.microsoft.com/library/windows/desktop/bb174576) 以向窗口显示呈现的图像。

```cpp
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

            m_d3dDeviceContext->IASetInputLayout(inputLayout.Get());

            // Set the vertex and index buffers, and specify the way they define geometry.
            UINT stride = sizeof(float2);
            UINT offset = 0;
            m_d3dDeviceContext->IASetVertexBuffers(
                0,
                1,
                vertexBuffer.GetAddressOf(),
                &stride,
                &offset
                );

            m_d3dDeviceContext->IASetIndexBuffer(
                indexBuffer.Get(),
                DXGI_FORMAT_R16_UINT,
                0
                );

            m_d3dDeviceContext->IASetPrimitiveTopology(D3D11_PRIMITIVE_TOPOLOGY_TRIANGLELIST);

            // Set the vertex and pixel shader stage state.
            m_d3dDeviceContext->VSSetShader(
                vertexShader.Get(),
                nullptr,
                0
                );

            m_d3dDeviceContext->PSSetShader(
                pixelShader.Get(),
                nullptr,
                0
                );

            // Draw the cube.
            m_d3dDeviceContext->DrawIndexed(
                ARRAYSIZE(triangleIndices),
                0,
                0
                );

            // Present the rendered image to the window.  Because the maximum frame latency is set to 1,
            // the render loop will generally be throttled to the screen refresh rate, typically around
            // 60 Hz, by sleeping the application on Present until the screen is refreshed.
            DX::ThrowIfFailed(
                m_swapChain->Present(1, 0)
                );
```

## <a name="summary-and-next-steps"></a>摘要和后续步骤


我们已使用顶点着色器和像素着色器创建并绘制出一个黄色的三角形。

接下来，我们需要创建一个环行 3D 立方体并向其应用照明效果。

[对基元使用深度和效果](using-depth-and-effects-on-primitives.md)

 

 




