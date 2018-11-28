---
title: 转换呈现框架
description: 介绍如何将简单的呈现框架从 Direct3D 9 转换到 Direct3D 11，包括如何移植几何图形缓冲区、如何编译和加载 HLSL 着色器程序以及如何在 Direct3D 11 中实现呈现链。
ms.assetid: f6ca1147-9bb8-719a-9a2c-b7ee3e34bd18
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, uwp, 游戏, 呈现框架, 转换, direct3d 9, direct3d 11
ms.localizationpriority: medium
ms.openlocfilehash: aba723a5ee2443664d6d640adc124b991ff0da7e
ms.sourcegitcommit: b11f305dbf7649c4b68550b666487c77ea30d98f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/28/2018
ms.locfileid: "7855036"
---
# <a name="convert-the-rendering-framework"></a>转换呈现框架



**摘要**

-   [第 1 部分：初始化 Direct3D 11](simple-port-from-direct3d-9-to-11-1-part-1--initializing-direct3d.md)
-   第 2 部分：转换呈现框架
-   [第 3 部分：移植游戏循环](simple-port-from-direct3d-9-to-11-1-part-3--viewport-and-game-loop.md)


介绍如何将简单的呈现框架从 Direct3D 9 转换到 Direct3D 11，包括如何移植几何图形缓冲区、如何编译和加载 HLSL 着色器程序以及如何在 Direct3D 11 中实现呈现链。 [将简单的 Direct3D 9 应用移植到 DirectX 11 和通用 Windows 平台 (UWP)](walkthrough--simple-port-from-direct3d-9-to-11-1.md) 演练的第 2 部分。

## <a name="convert-effects-to-hlsl-shaders"></a>将效果转换到 HLSL 着色器


以下示例是针对传统“效果” API 编写的简单的 D3DX 技术，用于硬件顶点转换和传递颜色数据。

Direct3D 9 着色器代码

```cpp
// Global variables
matrix g_mWorld;        // world matrix for object
matrix g_View;          // view matrix
matrix g_Projection;    // projection matrix

// Shader pipeline structures
struct VS_OUTPUT
{
    float4 Position   : POSITION;   // vertex position
    float4 Color      : COLOR0;     // vertex diffuse color
};

struct PS_OUTPUT
{
    float4 RGBColor : COLOR0;  // Pixel color    
};

// Vertex shader
VS_OUTPUT RenderSceneVS(float3 vPos : POSITION, 
                        float3 vColor : COLOR0)
{
    VS_OUTPUT Output;
    
    float4 pos = float4(vPos, 1.0f);

    // Transform the position from object space to homogeneous projection space
    pos = mul(pos, g_mWorld);
    pos = mul(pos, g_View);
    pos = mul(pos, g_Projection);

    Output.Position = pos;
    
    // Just pass through the color data
    Output.Color = float4(vColor, 1.0f);
    
    return Output;
}

// Pixel shader
PS_OUTPUT RenderScenePS(VS_OUTPUT In) 
{ 
    PS_OUTPUT Output;

    Output.RGBColor = In.Color;

    return Output;
}

// Technique
technique RenderSceneSimple
{
    pass P0
    {          
        VertexShader = compile vs_2_0 RenderSceneVS();
        PixelShader  = compile ps_2_0 RenderScenePS(); 
    }
}
```

在 Direct3D 11 中，我们仍然可以使用我们的 HLSL 着色器。 我们将每个着色器放置到它自己的 HLSL 文件中以便 Visual Studio 将它们编译成单独的文件，之后我们会将它们作为单独的 Direct3D 资源加载。 我们将目标级别设置为[着色器模型 4 级别 9\_1 (/4\_0\_level\_9\_1)](https://msdn.microsoft.com/library/windows/desktop/ff476876)，因为这些着色器是针对 DirectX 9.1 GPU 编写的。

定义了输入布局之后，我们要确保它表示我们用来在系统内存和 GPU 内存中存储每个顶点数据的相同数据结构。 同样，顶点着色器的输出应该与用作像素着色器输入的结构相匹配。 在 C++ 中，将数据从一个函数传递到另一个函数时的规则并不相同；你可以忽略位于结构结尾部分的未使用的变量。 但不能重新排列顺序，并且不能跳过数据结构中间的内容。

> **注意** Direct3D 9 中为顶点着色器绑定到像素着色器的规则比更宽松 Direct3D 11 中的规则。 Direct3D 9 排列比较灵活，但效率低。

 

你的 HLSL 文件可以使用较旧的语法作为着色器语义 - 例如，使用 COLOR 而不是 SV\_TARGET。 如果是这样，那么你将需要启用 HLSL 兼容模式（/Gec 编译器选项）或将着色器[语义](https://msdn.microsoft.com/library/windows/desktop/bb509647)更新为当前语法。 该示例中的顶点着色器已经使用当前语法进行了更新。

下面是我们的硬件转换顶点着色器，这次是在它自己的文件中定义的。

> **注意**需要使用顶点着色器来输出 SV\_POSITION 系统值语义。 该语义将顶点位置数据解析为坐标值，其中 x 介于 -1 和 1 之间，y 介于 -1 和 1 之间，z 除以原始齐次坐标 w 值 (z/w) ，并且 w 为 1 除以原始的 w 值 (1/w)。

 

HLSL 顶点着色器（功能级别 9.1）

```cpp
cbuffer ModelViewProjectionConstantBuffer : register(b0)
{
    matrix mWorld;       // world matrix for object
    matrix View;        // view matrix
    matrix Projection;  // projection matrix
};

struct VS_INPUT
{
    float3 vPos   : POSITION;
    float3 vColor : COLOR0;
};

struct VS_OUTPUT
{
    float4 Position : SV_POSITION; // Vertex shaders must output SV_POSITION
    float4 Color    : COLOR0;
};

VS_OUTPUT main(VS_INPUT input) // main is the default function name
{
    VS_OUTPUT Output;

    float4 pos = float4(input.vPos, 1.0f);

    // Transform the position from object space to homogeneous projection space
    pos = mul(pos, mWorld);
    pos = mul(pos, View);
    pos = mul(pos, Projection);
    Output.Position = pos;

    // Just pass through the color data
    Output.Color = float4(input.vColor, 1.0f);

    return Output;
}
```

这是我们使用传递像素着色器所全部需要的。 尽管我们称它为传递，但实际上是获取每个像素的透视校正插值颜色数据。 请注意，我们的像素着色器会根据 API 的需要将 SV\_TARGET 系统值语义应用于颜色值输出。

> **注意**着色器级别 9 \_x 像素着色器无法从 SV\_POSITION 系统值语义读取。 模型 4.0（或更高版本）像素着色器可以使用 SV\_POSITION 来检索屏幕上的像素位置，其中 x 介于 0 和呈现目标宽度之间，y 介于 0 和呈现目标高度（每个偏移 0.5）之间。

 

大多数像素着色器都比传递着色器复杂得多；请注意，较高的 Direct3D 功能级别所允许的每个着色器程序的计算量也更大。

HLSL 像素着色器（功能级别 9.1）

```cpp
struct PS_INPUT
{
    float4 Position : SV_POSITION;  // interpolated vertex position (system value)
    float4 Color    : COLOR0;       // interpolated diffuse color
};

struct PS_OUTPUT
{
    float4 RGBColor : SV_TARGET;  // pixel color (your PS computes this system value)
};

PS_OUTPUT main(PS_INPUT In)
{
    PS_OUTPUT Output;

    Output.RGBColor = In.Color;

    return Output;
}
```

## <a name="compile-and-load-shaders"></a>编译和加载着色器


Direct3D 9 游戏通常使用“效果”库作为实现可编程管道的便捷方法。 可以使用 [**D3DXCreateEffectFromFile function**](https://msdn.microsoft.com/library/windows/desktop/bb172768) 方法在运行时编译效果。

在 Direct3D 9 中加载效果

```cpp
// Turn off preshader optimization to keep calculations on the GPU
DWORD dwShaderFlags = D3DXSHADER_NO_PRESHADER;

// Only enable debug info when compiling for a debug target
#if defined (DEBUG) || defined (_DEBUG)
dwShaderFlags |= D3DXSHADER_DEBUG;
#endif

D3DXCreateEffectFromFile(
    m_pd3dDevice,
    L"CubeShaders.fx",
    NULL,
    NULL,
    dwShaderFlags,
    NULL,
    &m_pEffect,
    NULL
    );
```

Direct3D 11 将着色器程序用作二进制资源。 构建项目时会编译着色器，然后作为资源进行处理。 因此，我们的示例会将着色器 字节码加载到系统内存中，使用 Direct3D 设备接口为每个着色器创建 Direct3D 资源，并在设置每个帧时指向 Direct3D 着色器资源。

在 Direct3D 11 中加载着色器资源

```cpp
// BasicReaderWriter is a tested file loader used in SDK samples.
BasicReaderWriter^ readerWriter = ref new BasicReaderWriter();


// Load vertex shader:
Platform::Array<byte>^ vertexShaderData =
    readerWriter->ReadData("CubeVertexShader.cso");

// This call allocates a device resource, validates the vertex shader 
// with the device feature level, and stores the vertex shader bits in 
// graphics memory.
m_d3dDevice->CreateVertexShader(
    vertexShaderData->Data,
    vertexShaderData->Length,
    nullptr,
    &m_vertexShader
    );
```

若要在编译的应用程序包中包含着色器字节码，只需将 HLSL 文件添加到 Visual Studio 项目。 Visual Studio 将使用[效果编译器工具](https://msdn.microsoft.com/library/windows/desktop/bb232919) (FXC) 将 HLSL 文件编译到编译的着色器对象（.CSO 文件）中，并将其包含在应用包中。

> **注意**请确保为 HLSL 编译器设置正确的目标功能级别： 右键单击 HLSL 源文件，在 Visual Studio 中的，选择属性并更改下的**着色器模型**设置**HLSL 编译器-&gt;常规**。 当你的应用创建 Direct3D 着色器资源时，Direct3D 会针对硬件功能检查此属性。

 

![hlsl 着色器属性](images/hlslshaderpropertiesmenu.png)![hlsl 着色器类型](images/hlslshadertypeproperties.png)

这是创建输入布局的最佳位置，它对应于 Direct3D 9 中的顶点流声明。 每个顶点数据结构需要与顶点着色器使用的数据结构相匹配；在 Direct3D 11 中，我们对输入布局拥有更多控制；我们可以定义数组大小、浮点矢量的位长度，并可为顶点着色器指定语义。 我们创建一个 [**D3D11\_INPUT\_ELEMENT\_DESC**](https://msdn.microsoft.com/library/windows/desktop/ff476180) 结构并使用该结构来告知 Direct3D 每个顶点数据的内容。 我们一直等到加载完顶点着色器之后才定义输入布局，因为 API 要针对顶点着色器资源对输入布局进行验证。 如果输入布局不兼容，则 Direct3D 会引发异常。

每个顶点数据必须采用兼容的类型存储在系统内存中。 DirectXMath 数据类型可能有所帮助；例如，DXGI\_FORMAT\_R32G32B32\_FLOAT 对应于 [**XMFLOAT3**](https://msdn.microsoft.com/library/windows/desktop/ee419475)。

> **注意**常量缓冲区使用一个固定的一次对齐四个浮点数的输入的布局。 建议对常量缓冲区数据使用 [**XMFLOAT4**](https://msdn.microsoft.com/library/windows/desktop/ee419608)（及其派生对象）。

 

在 Direct3D 11 中设置输入布局

```cpp
// Create input layout:
const D3D11_INPUT_ELEMENT_DESC vertexDesc[] = 
{
    { "POSITION", 0, DXGI_FORMAT_R32G32B32_FLOAT,
        0, 0,  D3D11_INPUT_PER_VERTEX_DATA, 0 },

    { "COLOR",    0, DXGI_FORMAT_R32G32B32_FLOAT, 
        0, 12, D3D11_INPUT_PER_VERTEX_DATA, 0 },
};
```

## <a name="create-geometry-resources"></a>创建几何图形资源


在 Direct3D 9 中，我们通过在 Direct3D 设备上创建缓冲区、锁定内存并将数据从 CPU 内存复制到 GPU 内存来存储几何图形资源。

Direct3D 9

```cpp
// Create vertex buffer:
VOID* pVertices;

// In Direct3D 9 we create the buffer, lock it, and copy the data from 
// system memory to graphics memory.
m_pd3dDevice->CreateVertexBuffer(
    sizeof(CubeVertices),
    0,
    D3DFVF_XYZ | D3DFVF_DIFFUSE,
    D3DPOOL_MANAGED,
    &pVertexBuffer,
    NULL);

pVertexBuffer->Lock(
    0,
    sizeof(CubeVertices),
    &pVertices,
    0);

memcpy(pVertices, CubeVertices, sizeof(CubeVertices));
pVertexBuffer->Unlock();
```

DirectX 11 遵循较简单的流程。 API 自动将数据从系统内存复制到 GPU。 我们可以使用 COM 智能指针，以使得编程更加简单。

DirectX 11

```cpp
D3D11_SUBRESOURCE_DATA vertexBufferData = {0};
vertexBufferData.pSysMem = CubeVertices;
vertexBufferData.SysMemPitch = 0;
vertexBufferData.SysMemSlicePitch = 0;
CD3D11_BUFFER_DESC vertexBufferDesc(
    sizeof(CubeVertices),
    D3D11_BIND_VERTEX_BUFFER);
  
// This call allocates a device resource for the vertex buffer and copies
// in the data.
m_d3dDevice->CreateBuffer(
    &vertexBufferDesc,
    &vertexBufferData,
    &m_vertexBuffer
    );
```

## <a name="implement-the-rendering-chain"></a>实现呈现链


Direct3D 9 游戏通常使用一个基于效果的呈现链。 这种类型的呈现链设置效果对象，为其提供所需的资源并让其呈现每个通道。

Direct3D 9 呈现链

```cpp
// Clear the render target and the z-buffer.
m_pd3dDevice->Clear(
    0, NULL,
    D3DCLEAR_TARGET | D3DCLEAR_ZBUFFER,
    D3DCOLOR_ARGB(0, 45, 50, 170),
    1.0f, 0
    );

// Set the effect technique
m_pEffect->SetTechnique("RenderSceneSimple");

// Rotate the cube 1 degree per frame.
D3DXMATRIX world;
D3DXMatrixRotationY(&world, D3DXToRadian(m_frameCount++));


// Set the matrices up using traditional functions.
m_pEffect->SetMatrix("g_mWorld", &world);
m_pEffect->SetMatrix("g_View", &m_view);
m_pEffect->SetMatrix("g_Projection", &m_projection);

// Render the scene using the Effects library.
if(SUCCEEDED(m_pd3dDevice->BeginScene()))
{
    // Begin rendering effect passes.
    UINT passes = 0;
    m_pEffect->Begin(&passes, 0);
    
    for (UINT i = 0; i < passes; i++)
    {
        m_pEffect->BeginPass(i);
        
        // Send vertex data to the pipeline.
        m_pd3dDevice->SetFVF(D3DFVF_XYZ | D3DFVF_DIFFUSE);
        m_pd3dDevice->SetStreamSource(
            0, pVertexBuffer,
            0, sizeof(VertexPositionColor)
            );
        m_pd3dDevice->SetIndices(pIndexBuffer);
        
        // Draw the cube.
        m_pd3dDevice->DrawIndexedPrimitive(
            D3DPT_TRIANGLELIST,
            0, 0, 8, 0, 12
            );
        m_pEffect->EndPass();
    }
    m_pEffect->End();
    
    // End drawing.
    m_pd3dDevice->EndScene();
}

// Present frame:
// Show the frame on the primary surface.
m_pd3dDevice->Present(NULL, NULL, NULL, NULL);
```

DirectX 11 呈现链仍然执行相同的任务，但需要采用不同的方式实现呈现通道。 不再将具体内容放到 FX 文件中，并使得呈现技术对于我们的 C++ 代码来说或多或少有些不透明，我们将采用 C++ 设置我们的全部呈现。

我们的呈现链如下所示。 我们需要提供在加载顶点着色器之后我们创建的输入布局、提供每个着色器对象，以及为每个要使用的着色器指定常量缓冲区。 该示例并未包含多个呈现通道，但如果它这样做，我们会针对每个通道实现类似的呈现链，从而根据需要更改设置。

Direct3D 11 呈现链

```cpp
// Clear the back buffer.
const float midnightBlue[] = { 0.098f, 0.098f, 0.439f, 1.000f };
m_d3dContext->ClearRenderTargetView(
    m_renderTargetView.Get(),
    midnightBlue
    );

// Set the render target. This starts the drawing operation.
m_d3dContext->OMSetRenderTargets(
    1,  // number of render target views for this drawing operation.
    m_renderTargetView.GetAddressOf(),
    nullptr
    );


// Rotate the cube 1 degree per frame.
XMStoreFloat4x4(
    &m_constantBufferData.model, 
    XMMatrixTranspose(XMMatrixRotationY(m_frameCount++ * XM_PI / 180.f))
    );

// Copy the updated constant buffer from system memory to video memory.
m_d3dContext->UpdateSubresource(
    m_constantBuffer.Get(),
    0,      // update the 0th subresource
    NULL,   // use the whole destination
    &m_constantBufferData,
    0,      // default pitch
    0       // default pitch
    );


// Send vertex data to the Input Assembler stage.
UINT stride = sizeof(VertexPositionColor);
UINT offset = 0;

m_d3dContext->IASetVertexBuffers(
    0,  // start with the first vertex buffer
    1,  // one vertex buffer
    m_vertexBuffer.GetAddressOf(),
    &stride,
    &offset
    );

m_d3dContext->IASetIndexBuffer(
    m_indexBuffer.Get(),
    DXGI_FORMAT_R16_UINT,
    0   // no offset
    );

m_d3dContext->IASetPrimitiveTopology(D3D11_PRIMITIVE_TOPOLOGY_TRIANGLELIST);
m_d3dContext->IASetInputLayout(m_inputLayout.Get());


// Set the vertex shader.
m_d3dContext->VSSetShader(
    m_vertexShader.Get(),
    nullptr,
    0
    );

// Set the vertex shader constant buffer data.
m_d3dContext->VSSetConstantBuffers(
    0,  // register 0
    1,  // one constant buffer
    m_constantBuffer.GetAddressOf()
    );


// Set the pixel shader.
m_d3dContext->PSSetShader(
    m_pixelShader.Get(),
    nullptr,
    0
    );


// Draw the cube.
m_d3dContext->DrawIndexed(
    m_indexCount,
    0,  // start with index 0
    0   // start with vertex 0
    );
```

交换链是图形基础结构的一部分，因此我们使用我们的 DXGI 交换链来呈现完成的帧。 DXGI 会阻止该调用，直到下一个 vsync 为止；然后它将返回，并且我们的游戏循环可以继续进行下一次迭代。

使用 DirectX 11 将帧呈现到屏幕

```cpp
m_swapChain->Present(1, 0);
```

将从在 [**IFrameworkView::Run**](https://msdn.microsoft.com/library/windows/apps/hh700505) 方法中实现的游戏循环调用我们刚刚创建的呈现链。 [第 3 部分：视口和游戏循环](simple-port-from-direct3d-9-to-11-1-part-3--viewport-and-game-loop.md)中对此进行了介绍。

 

 




