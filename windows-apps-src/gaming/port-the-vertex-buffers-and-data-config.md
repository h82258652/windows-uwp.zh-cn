---
title: 移植顶点缓冲区和数据
description: 在此步骤中，你将定义将包含网格的顶点缓冲区以及允许着色器按照指定的顺序遍历顶点的索引缓冲区。
ms.assetid: 9a8138a5-0797-8532-6c00-58b907197a25
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, 游戏, 移植, 顶点缓冲区, 数据, direct3d
ms.localizationpriority: medium
ms.openlocfilehash: 4c961a8852fb1e03e4e86209f62bda821b980f8c
ms.sourcegitcommit: b5c9c18e70625ab770946b8243f3465ee1013184
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/28/2018
ms.locfileid: "7964737"
---
# <a name="port-the-vertex-buffers-and-data"></a>移植顶点缓冲区和数据




**重要的 API**

-   [**ID3DDevice::CreateBuffer**](https://msdn.microsoft.com/library/windows/desktop/ff476501)
-   [**ID3DDeviceContext::IASetVertexBuffers**](https://msdn.microsoft.com/library/windows/desktop/ff476456)
-   [**ID3D11DeviceContext::IASetIndexBuffer**](https://msdn.microsoft.com/library/windows/desktop/bb173588)

在此步骤中，你将定义将包含网格的顶点缓冲区以及允许着色器按照指定的顺序遍历顶点的索引缓冲区。

此时，让我们检查一下所使用的立方体网格的硬编码模型。 这两个表示都将顶点组织为三角形列表（而不是条带或其他更有效的三角形布局）。 这两个表示中的所有顶点还都具有关联的索引和颜色值。 本主题中的大部分 Direct3D 代码都参考了 Direct3D 项目中定义的变量和对象。

下面是由 OpenGL ES 2.0 进行处理的立方体。 在此示例实现中，每个顶点都是 7 位浮点值：3 个位置坐标，之后是 4 个 RGBA 颜色值。

```cpp
#define CUBE_INDICES 36
#define CUBE_VERTICES 8

GLfloat cubeVertsAndColors[] = 
{
  -0.5f, -0.5f,  0.5f, 0.0f, 0.0f, 1.0f, 1.0f,
  -0.5f, -0.5f, -0.5f, 0.0f, 0.0f, 0.0f, 1.0f,
  -0.5f,  0.5f,  0.5f, 0.0f, 1.0f, 1.0f, 1.0f,
  -0.5f,  0.5f, -0.5f, 0.0f, 1.0f, 0.0f, 1.0f,
  0.5f, -0.5f,  0.5f, 1.0f, 0.0f, 1.0f, 1.0f,
  0.5f, -0.5f, -0.5f, 1.0f, 0.0f, 0.0f, 1.0f,  
  0.5f,  0.5f,  0.5f, 1.0f, 1.0f, 1.0f, 1.0f,
  0.5f,  0.5f, -0.5f, 1.0f, 1.0f, 0.0f, 1.0f
};

GLuint cubeIndices[] = 
{
  0, 1, 2, // -x
  1, 3, 2,

  4, 6, 5, // +x
  6, 7, 5,

  0, 5, 1, // -y
  5, 6, 1,

  2, 6, 3, // +y
  6, 7, 3,

  0, 4, 2, // +z
  4, 6, 2,

  1, 7, 3, // -z
  5, 7, 1
};
```

下面是由 Direct3D 11 进行处理的同一个立方体。

```cpp
VertexPositionColor cubeVerticesAndColors[] = 
// struct format is position, color
{
  {XMFLOAT3(-0.5f, -0.5f, -0.5f), XMFLOAT3(0.0f, 0.0f, 0.0f)},
  {XMFLOAT3(-0.5f, -0.5f,  0.5f), XMFLOAT3(0.0f, 0.0f, 1.0f)},
  {XMFLOAT3(-0.5f,  0.5f, -0.5f), XMFLOAT3(0.0f, 1.0f, 0.0f)},
  {XMFLOAT3(-0.5f,  0.5f,  0.5f), XMFLOAT3(0.0f, 1.0f, 1.0f)},
  {XMFLOAT3( 0.5f, -0.5f, -0.5f), XMFLOAT3(1.0f, 0.0f, 0.0f)},
  {XMFLOAT3( 0.5f, -0.5f,  0.5f), XMFLOAT3(1.0f, 0.0f, 1.0f)},
  {XMFLOAT3( 0.5f,  0.5f, -0.5f), XMFLOAT3(1.0f, 1.0f, 0.0f)},
  {XMFLOAT3( 0.5f,  0.5f,  0.5f), XMFLOAT3(1.0f, 1.0f, 1.0f)},
};
        
unsigned short cubeIndices[] = 
{
  0, 2, 1, // -x
  1, 2, 3,

  4, 5, 6, // +x
  5, 7, 6,

  0, 1, 5, // -y
  0, 5, 4,

  2, 6, 7, // +y
  2, 7, 3,

  0, 4, 6, // -z
  0, 6, 2,

  1, 3, 7, // +z
  1, 7, 5
};
```

查看该代码，你会注意到，OpenGL ES 2.0 代码中的立方体采用右手坐标系加以表示，而特定于 Direct3D 的代码采用左手坐标系表加以表示。 导入你自己的网格数据时，必须将你的模型的 z 轴坐标反向并相应地更改每个网格的索引以便根据坐标系的更改遍历三角形。

假定我们已经成功地将立方体网格从右手 OpenGL ES 2.0 坐标系移动到左手 Direct3D 坐标系，下面我们介绍一下如何在这两种模型中加载立方体数据以便进行处理。

## <a name="instructions"></a>说明

### <a name="step-1-create-an-input-layout"></a>步骤 1：创建输入布局

在 OpenGL ES 2.0 中，你的顶点数据以属性的形式提供，属性将提供给着色器对象并由着色器对象进行读取。 通常你会将包含在着色器的 GLSL 中使用的属性名称的字符串提供给着色器程序对象，并得到一个你可以提供给着色器的内存位置。 在此示例中，顶点缓冲区对象包含一个自定义的矢量结构列表，该列表是按照如下方式进行定义并设置格式的：

OpenGL ES 2.0：配置包含每个顶点信息的属性。

``` syntax
typedef struct 
{
  GLfloat pos[3];        
  GLfloat rgba[4];
} Vertex;
```

在 OpenGL ES 2.0 中，输入布局是隐式的；你获取一个常规用途的 GL\_ELEMENT\_ARRAY\_BUFFER 并提供步幅和偏移，以便顶点着色器可以在上传数据之后解释该数据。 使用 **glVertexAttribPointer** 呈现哪些属性映射到每个顶点数据块的哪个部分之前，通知着色器。

在 Direct3D 中，必须提供输入布局以描述在创建缓冲区时（而不是绘制几何图形之前）顶点缓冲区中顶点数据的结构。 为此，使用一个与内存中各个顶点的数据布局相对应的输入布局。 精确地指定此内容非常重要！

你在此处以 [**D3D11\_INPUT\_ELEMENT\_DESC**](https://msdn.microsoft.com/library/windows/desktop/ff476180) 结构数组的形式创建一个输入描述。

Direct3D：定义输入布局描述。

``` syntax
struct VertexPositionColor
{
  DirectX::XMFLOAT3 pos;
  DirectX::XMFLOAT3 color;
};

// ...

const D3D11_INPUT_ELEMENT_DESC vertexDesc[] = 
{
  { "POSITION", 0, DXGI_FORMAT_R32G32B32_FLOAT, 0, 0,  D3D11_INPUT_PER_VERTEX_DATA, 0 },
  { "COLOR",    0, DXGI_FORMAT_R32G32B32_FLOAT, 0, 12, D3D11_INPUT_PER_VERTEX_DATA, 0 },
};

```

该输入描述将顶点定义为两个 3 位坐标矢量对：一个三维矢量用于存储模型坐标中顶点的位置，另一个三维矢量用于存储与顶点关联的 RGB 颜色值。 在本例中，使用 3x32 位浮点格式，我们采用代码将其中的元素表示为 `XMFLOAT3(X.Xf, X.Xf, X.Xf)`。 处理将用于着色器的数据时，都应该使用来自 [DirectXMath](https://msdn.microsoft.com/library/windows/desktop/ee415574) 库的类型，因为它可以确保使该数据进行正确的打包和对齐。 （例如，对于矢量数据，使用 [**XMFLOAT3**](https://msdn.microsoft.com/library/windows/desktop/ee419475) 或 [**XMFLOAT4**](https://msdn.microsoft.com/library/windows/desktop/ee419608)；对于矩阵，使用 [**XMFLOAT4X4**](https://msdn.microsoft.com/library/windows/desktop/ee419621)）。

有关所有可能的格式类型的列表，请参阅 [**DXGI\_FORMAT**](https://msdn.microsoft.com/library/windows/desktop/bb173059)。

定义了每个顶点输入布局后，可以创建布局对象。 在以下代码中，我们将其写入 **m\_inputLayout**，它是一个类型为 **ComPtr** 的变量（指向类型为 [**ID3D11InputLayout**](https://msdn.microsoft.com/library/windows/desktop/ff476575) 的对象）。 **fileData** 包含上一步[移植着色器](port-the-shader-config.md)中已编译好的顶点着色器对象。

Direct3D：创建顶点缓冲区使用的输入布局。

``` syntax
Microsoft::WRL::ComPtr<ID3D11InputLayout>      m_inputLayout;

// ...

m_d3dDevice->CreateInputLayout(
  vertexDesc,
  ARRAYSIZE(vertexDesc),
  fileData->Data,
  fileShaderData->Length,
  &m_inputLayout
);
```

我们已经定义了输入布局。 现在，我们将创建一个使用该布局的缓冲区并将其与立方体网格数据一起加载。

### <a name="step-2-create-and-load-the-vertex-buffers"></a>步骤 2：创建和加载顶点缓冲区

在 OpenGL ES 2.0 中，创建一对缓冲区，一个用于位置数据，一个用于颜色数据。 （也可以创建一个包含两者和单个缓冲区的结构。）绑定每个缓冲区并向其中写入位置和颜色数据。 然后，在呈现函数期间，再次绑定缓冲区并为着色器提供缓冲区中数据的格式，以便它可以正确解释该数据。

OpenGL ES 2.0：绑定顶点缓冲区

``` syntax
// upload the data for the vertex position buffer
glGenBuffers(1, &renderer->vertexBuffer);    
glBindBuffer(GL_ARRAY_BUFFER, renderer->vertexBuffer);
glBufferData(GL_ARRAY_BUFFER, sizeof(VERTEX) * CUBE_VERTICES, renderer->vertices, GL_STATIC_DRAW);   
```

在 Direct3D 中，着色器可以访问的缓冲区表示为 [**D3D11\_SUBRESOURCE\_DATA**](https://msdn.microsoft.com/library/windows/desktop/ff476220) 结构。 若要将该缓冲区的位置绑定到着色器对象，你需要使用 [**ID3DDevice::CreateBuffer**](https://msdn.microsoft.com/library/windows/desktop/ff476501) 为每个缓冲区创建一个 CD3D11_BUFFER_DESC 结构，然后通过调用特定于缓冲区类型的 set 方法（如 [**ID3DDeviceContext::IASetVertexBuffers**](https://msdn.microsoft.com/library/windows/desktop/ff476456)）设置 Direct3D 设备上下文的缓冲区。

设置缓冲区时，必须从缓冲区的开头设置步幅（单个顶点的数据元素大小）以及偏移（顶点数据数组实际开始的位置）。

注意，我们将 **vertexIndices** 数组的指针分配给 [**D3D11\_SUBRESOURCE\_DATA**](https://msdn.microsoft.com/library/windows/desktop/ff476220) 结构的 **pSysMem** 字段。 如果该操作不正确，你的网格将遭到破坏或为空！

Direct3D：创建和设置顶点缓冲区

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
        
// ...

UINT stride = sizeof(VertexPositionColor);
UINT offset = 0;
m_d3dContext->IASetVertexBuffers(
  0,
  1,
  m_vertexBuffer.GetAddressOf(),
  &stride,
  &offset);
```

### <a name="step-3-create-and-load-the-index-buffer"></a>步骤 3：创建和加载索引缓冲区

索引缓冲区是允许顶点着色器查找各个顶点的有效方法。 尽管不需要，但我们还是在该示例呈现器中使用它们。 同 OpenGL ES 2.0 中的顶点缓冲区一样，创建一个索引缓冲区并作为常规用途缓冲区绑定，将你之前创建的顶点索引复制到该缓冲区中。

当你准备好绘图时，你需要再次绑定顶点缓冲区和索引缓冲区，并且调用 **glDrawElements**。

OpenGL ES 2.0：向绘图调用发送索引顺序。

``` syntax
GLuint indexBuffer;

// ...

glGenBuffers(1, &renderer->indexBuffer);    
glBindBuffer(GL_ELEMENT_ARRAY_BUFFER, renderer->indexBuffer);   
glBufferData(GL_ELEMENT_ARRAY_BUFFER, 
  sizeof(GLuint) * CUBE_INDICES, 
  renderer->vertexIndices, 
  GL_STATIC_DRAW);

// ...
// Drawing function

// Bind the index buffer
glBindBuffer(GL_ELEMENT_ARRAY_BUFFER, renderer->indexBuffer);
glDrawElements (GL_TRIANGLES, renderer->numIndices, GL_UNSIGNED_INT, 0);
```

对于 Direct3D，虽然有点说教，但这个过程有点类似。 提供索引缓冲区作为配置 Direct3D 时创建的 [**ID3D11DeviceContext**](https://msdn.microsoft.com/library/windows/desktop/ff476385) 的 Direct3D 子资源。 通过使用为索引数组配置的子资源调用 [**ID3D11DeviceContext::IASetIndexBuffer**](https://msdn.microsoft.com/library/windows/desktop/bb173588) 来完成该操作，如下所示。 （再次注意，将 **cubeIndices** 数组的指针分配给 [**D3D11\_SUBRESOURCE\_DATA**](https://msdn.microsoft.com/library/windows/desktop/ff476220) 结构的 **pSysMem** 字段。）

Direct3D：创建索引缓冲区。

``` syntax
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

// ...

m_d3dContext->IASetIndexBuffer(
  m_indexBuffer.Get(),
  DXGI_FORMAT_R16_UINT,
  0);
```

之后，你将通过调用 [**ID3D11DeviceContext::DrawIndexed**](https://msdn.microsoft.com/library/windows/desktop/ff476409)（对于无索引顶点，则通过调用 [**ID3D11DeviceContext::Draw**](https://msdn.microsoft.com/library/windows/desktop/ff476407)）绘制三角形，如下所示。 （有关详细信息，请向前跳至[绘制到屏幕](draw-to-the-screen.md)。）

Direct3D：绘制索引的顶点。

``` syntax
m_d3dContext->IASetPrimitiveTopology(D3D11_PRIMITIVE_TOPOLOGY_TRIANGLELIST);
m_d3dContext->IASetInputLayout(m_inputLayout.Get());

// ...

m_d3dContext->DrawIndexed(
  m_indexCount,
  0,
  0);
```

## <a name="previous-step"></a>上一步


[移植着色器对象](port-the-shader-config.md)

## <a name="next-step"></a>下一步

[移植 GLSL](port-the-glsl.md)

## <a name="remarks"></a>备注

当构造你的 Direct3D 时，将在 [**ID3D11Device**](https://msdn.microsoft.com/library/windows/desktop/ff476379) 上调用方法的代码分离成一个当需要重新创建设备资源时调用的方法。 （在 Direct3D 项目模板中，该代码位于呈现器对象的 **CreateDeviceResource** 方法中。） 另一方面，更新设备上下文 ([**ID3D11DeviceContext**](https://msdn.microsoft.com/library/windows/desktop/ff476385)) 的代码位于 **Render** 方法中，因为这是你实际构造着色器阶段以及绑定数据的位置。

## <a name="related-topics"></a>相关主题


* [如何：将简单的 OpenGL ES 2.0 呈现器移植到 Direct3D 11](port-a-simple-opengl-es-2-0-renderer-to-directx-11-1.md)
* [移植着色器对象](port-the-shader-config.md)
* [移植顶点缓冲区和数据](port-the-vertex-buffers-and-data-config.md)
* [移植 GLSL](port-the-glsl.md)

 

 




