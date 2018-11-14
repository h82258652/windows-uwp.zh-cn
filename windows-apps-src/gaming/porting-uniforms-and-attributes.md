---
author: mtoepke
title: 将 OpenGL ES 2.0 缓冲区、uniform、顶点移植到 Direct3D
description: 在从 OpenGL ES 2.0 移植到 Direct3D 11 的过程中，必须更改用于在应用和着色器程序之间传递数据的语法和 API 行为。
ms.assetid: 9b215874-6549-80c5-cc70-c97b571c74fe
ms.author: mtoepke
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, 游戏, opengl, direct3d, 缓冲区, uniform, 顶点属性
ms.localizationpriority: medium
ms.openlocfilehash: bc0192eb4b89ef91bc895a96e46cd39524f24c44
ms.sourcegitcommit: 4d88adfaf544a3dab05f4660e2f59bbe60311c00
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/13/2018
ms.locfileid: "6467342"
---
# <a name="compare-opengl-es-20-buffers-uniforms-and-vertex-attributes-to-direct3d"></a>将 OpenGL ES 2.0 缓冲区、uniform 和顶点属性与 Direct3D 进行比较




**重要的 API**

-   [**ID3D11Device1::CreateBuffer**](https://msdn.microsoft.com/library/windows/desktop/hh404575)
-   [**ID3D11Device1::CreateInputLayout**](https://msdn.microsoft.com/library/windows/desktop/ff476512)
-   [**ID3D11DeviceContext1::IASetInputLayout**](https://msdn.microsoft.com/library/windows/desktop/ff476454)

在从 OpenGL ES 2.0 移植到 Direct3D 11 的过程中，必须更改用于在应用和着色器程序之间传递数据的语法和 API 行为。

在 OpenGL ES 2.0 中，采用四种方法往返于着色器程序传递数据：对于常量数据，采用 uniform 的形式；对于顶点数据，采用属性的形式；对于其他资源数据（如纹理），采用缓冲区对象的形式。 在 Direct3D 11 中，这些内容大概映射到常量缓冲区、顶点缓冲区以及子资源。 尽管这些方法都非常肤浅，但是其处理方式却有很大不同。

下面是基本映射。

| OpenGL ES 2.0             | Direct3D 11                                                                                                                                                                         |
|---------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| uniform                   | 常量缓冲区 (**cbuffer**) 字段。                                                                                                                                                |
| 属性                 | 顶点缓冲区元素字段，由一个输入布局指定并且标记有特定的 HLSL 语法。                                                                                |
| 缓冲区对象             | 缓冲区；有关常规用途的缓冲区定义，请参阅 [**D3D11\_SUBRESOURCE\_DATA**](https://msdn.microsoft.com/library/windows/desktop/ff476220) 和 [**D3D11\_BUFFER\_DESC**](https://msdn.microsoft.com/library/windows/desktop/ff476092)。 |
| 帧缓冲区对象 (FBO) | 呈现目标；请参阅具有 [**ID3D11Texture2D**](https://msdn.microsoft.com/library/windows/desktop/ff476635) 的 [**ID3D11RenderTargetView**](https://msdn.microsoft.com/library/windows/desktop/ff476582)。                                       |
| 后台缓冲区               | 具有“后台缓冲区”图面的交换链；请参阅附加了 [**IDXGISurface1**](https://msdn.microsoft.com/library/windows/desktop/ff471343) 的 [**IDXGISwapChain1**](https://msdn.microsoft.com/library/windows/desktop/hh404631)。                       |

 

## <a name="port-buffers"></a>移植缓冲区


在 OpenGL ES 2.0 中，创建和绑定任何种类缓冲区的过程通常遵循此模式。

-   调用 glGenBuffers 以生成一个或多个缓冲区并将句柄返回给它们。
-   调用 glBindBuffer 以定义缓冲区的布局，如 GL\_ELEMENT\_ARRAY\_BUFFER。
-   调用 glBufferData 用特定布局中的特定数据（如顶点结构、索引数据或颜色数据）填充缓冲区。

最常见的缓冲区种类是顶点缓冲区，它至少包含一些坐标系中顶点的位置。 典型用法是，用包含位置坐标、顶点位置的法向矢量、顶点位置的切线矢量以及纹理查找 (uv) 坐标的结构来表示顶点。 缓冲区包含这些顶点按照一定顺序的连续列表（如三角形列表、条状列表或扇形列表），它们共同表示在场景中可见的多边形。 （在 Direct3D 11 以及 OpenGL ES 2.0 中，让每个 draw 调用拥有多个顶点缓冲区会导致效率降低。）

下面是使用 OpenGL ES 2.0 创建的顶点缓冲区和索引缓冲区的示例：

OpenGL ES 2.0：创建和填充顶点缓冲区和索引缓冲区。

``` syntax
glGenBuffers(1, &renderer->vertexBuffer);
glBindBuffer(GL_ARRAY_BUFFER, renderer->vertexBuffer);
glBufferData(GL_ARRAY_BUFFER, sizeof(Vertex) * CUBE_VERTICES, renderer->vertices, GL_STATIC_DRAW);

glGenBuffers(1, &renderer->indexBuffer);
glBindBuffer(GL_ELEMENT_ARRAY_BUFFER, renderer->indexBuffer);
glBufferData(GL_ELEMENT_ARRAY_BUFFER, sizeof(int) * CUBE_INDICES, renderer->vertexIndices, GL_STATIC_DRAW);
```

其他缓冲区包括像素缓冲区和映射，如纹理。 着色器管道可以呈现到纹理缓冲区（pixmap）中或呈现缓冲区对象并在将来的着色器通道中使用这些缓冲区。 在最简单的情况下，调用流程为：

-   调用 glGenFramebuffers 生成帧缓冲区对象。
-   调用 glBindFramebuffer 绑定帧缓冲区对象以便进行写入。
-   调用 glFramebufferTexture2D 绘制到指定的纹理映射中。

在 Direct3D 11 中，缓冲区数据元素被视为“子资源”，范围可以从各个顶点数据元素到 MIP 映射纹理。

-   使用缓冲区数据元素的配置填充 [**D3D11\_SUBRESOURCE\_DATA**](https://msdn.microsoft.com/library/windows/desktop/ff476220) 结构。
-   使用缓冲区中各个元素的大小以及缓冲区类型填充 [**D3D11\_BUFFER\_DESC**](https://msdn.microsoft.com/library/windows/desktop/ff476092) 结构。
-   使用这两个结构调用 [**ID3D11Device1::CreateBuffer**](https://msdn.microsoft.com/library/windows/desktop/hh404575)。

Direct3D 11：创建和填充顶点缓冲区和索引缓冲区。

``` syntax
D3D11_SUBRESOURCE_DATA vertexBufferData = {0};
vertexBufferData.pSysMem = cubeVertices;
vertexBufferData.SysMemPitch = 0;
vertexBufferData.SysMemSlicePitch = 0;
CD3D11_BUFFER_DESC vertexBufferDesc(sizeof(cubeVertices), D3D11_BIND_VERTEX_BUFFER);

m_d3dDevice->CreateBuffer(
  &vertexBufferDesc,
  &vertexBufferData,
  &m_vertexBuffer);

m_indexCount = ARRAYSIZE(cubeIndices);

D3D11_SUBRESOURCE_DATA indexBufferData = {0};
indexBufferData.pSysMem = cubeIndices;
indexBufferData.SysMemPitch = 0;
indexBufferData.SysMemSlicePitch = 0;
CD3D11_BUFFER_DESC indexBufferDesc(sizeof(cubeIndices), D3D11_BIND_INDEX_BUFFER);

m_d3dDevice->CreateBuffer(
  &indexBufferDesc,
  &indexBufferData,
  &m_indexBuffer);
    
```

可写入的像素缓冲区或映射（如帧缓冲区）可以创建为 [**ID3D11Texture2D**](https://msdn.microsoft.com/library/windows/desktop/ff476635) 对象。 可将这些内容作为资源绑定到 [**ID3D11RenderTargetView**](https://msdn.microsoft.com/library/windows/desktop/ff476582) 或 [**ID3D11ShaderResourceView**](https://msdn.microsoft.com/library/windows/desktop/ff476628)，绘制到其中之后，可以使用关联的交换链进行显示或者传递到着色器。

Direct3D 11：创建帧缓冲区对象。

``` syntax
ComPtr<ID3D11RenderTargetView> m_d3dRenderTargetViewWin;
// ...
ComPtr<ID3D11Texture2D> frameBuffer;

m_swapChainCoreWindow->GetBuffer(0, IID_PPV_ARGS(&frameBuffer));
m_d3dDevice->CreateRenderTargetView(
  frameBuffer.Get(),
  nullptr,
  &m_d3dRenderTargetViewWin);
```

## <a name="change-uniforms-and-uniform-buffer-objects-to-direct3d-constant-buffers"></a>将 uniform 和 uniform 缓冲区对象更改为 Direct3D 常量缓冲区


在 Open GL ES 2.0 中，uniform 是向各个着色器程序提供常量数据的机制。 着色器不能改变此数据。

设置 uniform 通常涉及向其中一个 glUniform\* 方法提供 GPU 中的上传位置以及指向应用内存中数据的指针。 执行 glUniform\* 方法之后，uniform 数据位于 GPU 内存中并且声明该 uniform 的着色器可以对其进行访问。 你应该确保采用这样的方式来打包数据，以便着色器可以基于着色器中的 uniform 声明解释该数据（使用兼容的类型）。

OpenGL ES 2.0 创建 uniform 并向其中上载数据

``` syntax
renderer->mvpLoc = glGetUniformLocation(renderer->programObject, "u_mvpMatrix");

// ...

glUniformMatrix4fv(renderer->mvpLoc, 1, GL_FALSE, (GLfloat*) &renderer->mvpMatrix.m[0][0]);
```

在着色器的 GLSL 中，对应的 uniform 声明如下所示：

Open GL ES 2.0：GLSL uniform 声明

``` syntax
uniform mat4 u_mvpMatrix;
```

Direct3D 将 uniform 数据指定为“常量缓冲区”，与 uniform 一样，它包含提供给各个着色器的常量数据。 与 uniform 缓冲区一样，应采用同样的方法将内存中的常量缓冲区数据打包以使着色器能够解释该数据，这一点非常重要。 使用 DirectXMath 类型（如 [**XMFLOAT4**](https://msdn.microsoft.com/library/windows/desktop/ee419608)），而不是平台类型（如 **float\*** 或 **float\[4\]**）确保数据元素正确对齐。

常量缓冲区必须在 GPU 上拥有一个关联的 GPU 寄存器，用于参考该数据。 按照缓冲区布局的指示将数据打包到寄存器位置。

Direct3D 11：创建常量缓冲区并向其中上载数据

``` syntax
struct ModelViewProjectionConstantBuffer
{
     DirectX::XMFLOAT4X4 mvp;
};

// ...

ModelViewProjectionConstantBuffer   m_constantBufferData;

// ...

XMStoreFloat4x4(&m_constantBufferData.mvp, mvpMatrix);

CD3D11_BUFFER_DESC constantBufferDesc(sizeof(ModelViewProjectionConstantBuffer), D3D11_BIND_CONSTANT_BUFFER);
m_d3dDevice->CreateBuffer(
  &constantBufferDesc,
  nullptr,
  &m_constantBuffer);
```

在着色器的 HLSL 中，对应的常量缓冲区声明如下所示：

Direct3D 11：常量缓冲区 HLSL 声明

``` syntax
cbuffer ModelViewProjectionConstantBuffer : register(b0)
{
  matrix mvp;
};
```

请注意，必须为每个常量缓冲区声明一个寄存器。 不同的 Direct3D 功能级别可用的最大寄存器数有所不同，因此不要超过最低目标功能级别对应的最大数量。

## <a name="port-vertex-attributes-to-a-direct3d-input-layouts-and-hlsl-semantics"></a>将顶点属性移植到 Direct3D 输入布局和 HLSL 语义


由于着色器管道可以修改顶点数据，因此，OpenGL ES 2.0 要求你将数据指定为“attribute”而不是“uniform”。 （这在更高版本 OpenGL 和 GLSL 中已更改。）特定于顶点的数据（如顶点位置、法线、切线以及颜色值）作为属性值提供给着色器。 这些属性值与顶点数据中每个元素的特定偏移相对应；例如，第一个属性可以指向单个顶点的位置组件，第二个属性可以指向法线等。

将顶点缓冲区数据从主内存移动到 GPU 的基本过程如下所示：

-   使用 glBindBuffer 上载顶点数据。
-   使用 glGetAttribLocation 获取 GPU 上属性的位置。 针对顶点数据元素中的每个属性调用它。
-   调用 glVertexAttribPointer 可在一个顶点数据元素内设置正确的属性大小和偏移。 针对每个属性执行该操作。
-   使用 glEnableVertexAttribArray 启用顶点数据布局信息。

OpenGL ES 2.0：将顶点缓冲区数据上载到着色器属性

``` syntax
glBindBuffer(GL_ELEMENT_ARRAY_BUFFER, renderer->vertexBuffer);
loc = glGetAttribLocation(renderer->programObject, "a_position");

glVertexAttribPointer(loc, 3, GL_FLOAT, GL_FALSE, 
  sizeof(Vertex), 0);
loc = glGetAttribLocation(renderer->programObject, "a_color");
glEnableVertexAttribArray(loc);

glVertexAttribPointer(loc, 4, GL_FLOAT, GL_FALSE, 
  sizeof(Vertex), (GLvoid*) (sizeof(float) * 3));
glEnableVertexAttribArray(loc);
```

现在，在你的顶点着色器中，使用在对 glGetAttribLocation 的调用中定义的相同名称声明属性。

OpenGL ES 2.0：在 GLSL 中声明属性

``` syntax
attribute vec4 a_position;
attribute vec4 a_color;                     
```

在某些方面，同样的过程适用于 Direct3D。 在输入缓冲区（包括顶点缓冲区以及相应的索引缓冲区）中提供顶点数据而不是属性。 但是，由于 Direct3D 没有“attribute”声明，因此你必须指定一个输入布局（该布局声明顶点缓冲区中数据元素的单个组件）以及指示顶点着色器解释这些组件的位置和方法的 HLSL 语义。 HLSL 语义要求你使用特定字符串（它会告知着色器引擎关于组件的用途）定义每个组件的用法。 例如，顶点位置数据标记为 POSITION，法线数据标记为 NORMAL，而顶点颜色数据标记为 COLOR。 （其他着色器阶段也要求特定语义，并且根据着色器阶段的不同，这些语义也有不同的解释。）有关 HLSL 语义的详细信息，请阅读[移植着色器管道](change-your-shader-loading-code.md)和 [HLSL 语义](https://msdn.microsoft.com/library/windows/desktop/bb205574)。

设置顶点缓冲区和索引缓冲区以及设置输入布局的过程统称为 Direct3D 图形管道的“输入程序集”(IA) 阶段。

Direct3D 11：配置输入程序集阶段

``` syntax
// Set up the IA stage corresponding to the current draw operation.
UINT stride = sizeof(VertexPositionColor);
UINT offset = 0;
m_d3dContext->IASetVertexBuffers(
        0,
        1,
        m_vertexBuffer.GetAddressOf(),
        &stride,
        &offset);

m_d3dContext->IASetIndexBuffer(
        m_indexBuffer.Get(),
        DXGI_FORMAT_R16_UINT,
        0);

m_d3dContext->IASetPrimitiveTopology(D3D11_PRIMITIVE_TOPOLOGY_TRIANGLELIST);
m_d3dContext->IASetInputLayout(m_inputLayout.Get());
```

通过声明顶点数据元素的格式以及用于每个组件的语义来声明输入布局并将其与顶点着色器关联。 采用所创建的 D3D11\_INPUT\_ELEMENT\_DESC 描述的顶点元素数据布局必须与相应的结构布局相对应。 你在此处为包含两个组件的顶点数据创建一个布局：

-   顶点位置坐标在主内存中表示为 XMFLOAT3，它是由 (x, y, z) 坐标的 3 个 32 位浮点值构成的已对齐数组。
-   顶点颜色值表示为 XMFLOAT4，它是由颜色 (RGBA) 的 4 个 32 位浮点值构成的已对齐数组。

为每个组件分配一个语义以及一个格式类型。 然后，将描述传递给 [**ID3D11Device1::CreateInputLayout**](https://msdn.microsoft.com/library/windows/desktop/ff476512)。 如果在呈现方法期间设置输入程序集，则会在调用 [**ID3D11DeviceContext1::IASetInputLayout**](https://msdn.microsoft.com/library/windows/desktop/ff476454) 时使用输入布局。

Direct3D 11：使用特定语义描述输入布局

``` syntax
ComPtr<ID3D11InputLayout> m_inputLayout;

// ...

const D3D11_INPUT_ELEMENT_DESC vertexDesc[] = 
{
  { "POSITION", 0, DXGI_FORMAT_R32G32B32_FLOAT, 0, 0,  D3D11_INPUT_PER_VERTEX_DATA, 0 },
  { "COLOR",    0, DXGI_FORMAT_R32G32B32_FLOAT, 0, 12, D3D11_INPUT_PER_VERTEX_DATA, 0 },
};

m_d3dDevice->CreateInputLayout(
  vertexDesc,
  ARRAYSIZE(vertexDesc),
  fileData->Data,
  fileData->Length,
  &m_inputLayout);

// ...
// When we start the drawing process...

m_d3dContext->IASetInputLayout(m_inputLayout.Get());
```

最后，确保着色器可以通过声明输入来理解输入数据。 将使用在布局中分配的语义来选择 GPU 内存中的正确位置。

Direct3D 11：使用 HLSL 语义声明着色器输入数据

``` syntax
struct VertexShaderInput
{
  float3 pos : POSITION;
  float3 color : COLOR;
};
```

 

 




