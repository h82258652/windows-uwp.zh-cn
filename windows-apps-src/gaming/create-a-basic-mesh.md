---
创建和显示基本网格
3D 通用 Windows 平台游戏通常使用多边形来表示游戏中的对象和图面。
ms.assetid: bfe0ed5b-63d8-935b-a25b-378b36982b7d
---

# 创建和显示基本网格


\[ 已针对 Windows 10 上的 UWP 应用更新。 有关 Windows 8.x 文章，请参阅[存档](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

3D 通用 Windows 平台游戏通常使用多边形来表示游戏中的对象和图面。 构成这些多边形对象和图面的结构的顶点列表称为网格。 在这里，我们为立方体对象创建一个基本网格并为其提供用于呈现和显示的着色器管道。

> **重要提示** 此处包含的示例代码使用在 DirectXMath.h 中声明的类型（如 DirectX::XMFLOAT3 和 DirectX::XMFLOAT4X4）和内联方法。 如果你剪切并粘贴该代码，请在你的项目中使用 \#include <DirectXMath.h>。

 

## 你需要了解的内容


### 技术

-   [Direct3D](https://msdn.microsoft.com/library/windows/desktop/hh769064)

### 先决条件

-   线性代数和三维坐标系的基础知识
-   Visual Studio 2015 Direct3D 模板

## 说明

### 步骤 1：为模型构造网格

在大多数游戏中，游戏对象的网格都从包含特定顶点数据的文件加载。 这些顶点的排序与应用相关，但它们通常被序列化为带或扇形。 顶点数据可以来自任何软件源，也可以手动创建。 游戏采用顶点着色器可以高效处理的方式解释数据。

在我们的示例中，我们对一个立方体使用简单的网格。 该立方体，与管道中处于该阶段的任何对象网格一样，都使用其自己的坐标系来表示。 顶点着色器获取它的坐标，通过应用提供的变换矩阵在同类坐标系中返回最终的 2-D 视图投影。

为立方体定义网格。 （或从文件加载。 由你来调用！）

```cpp
SimpleCubeVertex cubeVertices[] =
{
    { DirectX::XMFLOAT3(-0.5f, 0.5f, -0.5f), DirectX::XMFLOAT3(0.0f, 1.0f, 0.0f) }, // +Y (top face)
    { DirectX::XMFLOAT3( 0.5f, 0.5f, -0.5f), DirectX::XMFLOAT3(1.0f, 1.0f, 0.0f) },
    { DirectX::XMFLOAT3( 0.5f, 0.5f,  0.5f), DirectX::XMFLOAT3(1.0f, 1.0f, 1.0f) },
    { DirectX::XMFLOAT3(-0.5f, 0.5f,  0.5f), DirectX::XMFLOAT3(0.0f, 1.0f, 1.0f) },

    { DirectX::XMFLOAT3(-0.5f, -0.5f,  0.5f), DirectX::XMFLOAT3(0.0f, 0.0f, 1.0f) }, // -Y (bottom face)
    { DirectX::XMFLOAT3( 0.5f, -0.5f,  0.5f), DirectX::XMFLOAT3(1.0f, 0.0f, 1.0f) },
    { DirectX::XMFLOAT3( 0.5f, -0.5f, -0.5f), DirectX::XMFLOAT3(1.0f, 0.0f, 0.0f) },
    { DirectX::XMFLOAT3(-0.5f, -0.5f, -0.5f), DirectX::XMFLOAT3(0.0f, 0.0f, 0.0f) },
};
```

立方体的坐标系将立方体的中心放置在原点，y 轴使用左手坐标系从上指向下。 坐标值表示为介于 -1 和 1 之间的 32 位浮点值。

在每对括号中，第二个 DirectX::XMFLOAT3 值组将与顶点关联的颜色指定为 RGB 值。 例如，(-0.5, 0.5, -0.5) 处的第一个顶点为完全绿色（G 值设置为 1.0，“R”和“B”值设置为 0）。

因此，你拥有 8 个顶点，每个顶点都具有特定的颜色。 在我们的示例中，每个顶点/颜色对都是顶点的完整数据。 当你指定我们的顶点缓冲区时，必须记住这个特定的布局。 我们将此输入布局提供给顶点着色器以便它可以理解你的顶点数据。

### 步骤 2：设置输入布局

现在，你在内存中已拥有顶点。 但你的图形设备拥有其自己的内存，并且你使用 Direct3D 来访问该内存。 若要使你的顶点数据进入图形设备以便进行处理，你需要扫除障碍，因为你必须声明顶点数据的布局方式，以便图形设备从你的游戏获取顶点数据时图形设备可以对其进行解释。 若要执行该操作，需要使用 [**ID3D11InputLayout**](https://msdn.microsoft.com/library/windows/desktop/ff476575)。

为顶点缓冲区声明和设置输入布局。

```cpp
const D3D11_INPUT_ELEMENT_DESC basicVertexLayoutDesc[] =
{
    { "POSITION", 0, DXGI_FORMAT_R32G32B32_FLOAT, 0,  0, D3D11_INPUT_PER_VERTEX_DATA, 0 },
    { "COLOR",    0, DXGI_FORMAT_R32G32B32_FLOAT, 0, 12, D3D11_INPUT_PER_VERTEX_DATA, 0 },
};

ComPtr<ID3D11InputLayout> inputLayout;
m_d3dDevice->CreateInputLayout(
                basicVertexLayoutDesc,
                ARRAYSIZE(basicVertexLayoutDesc),
                vertexShaderBytecode->Data,
                vertexShaderBytecode->Length,
                &inputLayout)
);
```

在该代码中，为顶点指定布局，具体地说就是指定顶点列表中每个元素包含的数据。 此处在 **basicVertexLayoutDesc** 中，你指定两个数据分量：

-   **POSITION**：这是提供给着色器的位置数据的 HLSL 语义。 在该代码中，它是 DirectX::XMFLOAT3，更具体地说，是一个具有 3 个与 3D 坐标 (x, y, z) 相对应的 32 位浮点值的结构。 如果你支持同类的“w”坐标，则还可以使用 float4，在这种情况下，需要指定 DXGI\_FORMAT\_R32G32B32A32\_FLOAT。 使用 DirectX::XMFLOAT3 还是使用 float4 由你的游戏的具体需求来决定。 只需确保网格的顶点数据与你使用的格式正确对应！

    在对象坐标空间中，每个坐标值都表示为介于 -1 和 1 之间的浮点值。 当顶点着色器完成时，转换后的顶点位于同类（透视校正）视图投影空间中。

    你严格地指出 “枚举值指示 RGB，而不是 XYZ！”。 好眼力！ 在同时使用颜色数据和坐标数据的情况下，通常使用 3 个或 4 个分量值，那么为何不对它们使用相同的格式呢？ HLSL 语义（非格式名称）指示着色器处理数据的方式。

-   **COLOR**：这是颜色数据的 HLSL 语义。 与 **POSITION** 一样，它也包含 3 个 32 位浮点值 (DirectX::XMFLOAT3)。 每个值都包含一个颜色分量：红色 (r)、蓝色 (b) 或绿色 (g)，都表示为介于 0 和 1 之间的浮点数。

    **COLOR** 值通常在着色器管道结尾处以 4 个分量的 RGBA 值的形式返回。 对于该示例，在所有像素的着色器管道中，你将“A”alpha 值设置为 1.0（最大不透明度）。

有关格式的完整列表，请参阅 [**DXGI\_FORMAT**](https://msdn.microsoft.com/library/windows/desktop/bb173059)。 有关 HLSL 语义的完整列表，请参阅[语义](https://msdn.microsoft.com/library/windows/desktop/bb509647)。

在 Direct3D 设备上，调用 [**ID3D11Device::CreateInputLayout**](https://msdn.microsoft.com/library/windows/desktop/ff476512) 并创建输入布局。 现在，你需要创建一个可以实际包含数据的缓冲区！

### 步骤 3：填充顶点缓冲区

顶点缓冲区包含网格中每个三角形的顶点列表。 每个顶点都必须在该列表中唯一。 在我们的示例中，立方体有 8 个顶点。 顶点着色器在图形设备上运行并从顶点缓冲区中读取，并且它根据你在上一步中指定的输入布局来解释数据。

在下面的示例中，为缓冲区提供一个描述和一个子资源，它们会告知 Direct3D 有关顶点数据的物理映射以及如何在图形设备的内存中对其进行处理的大量信息。 这是必需的，因为你使用的常规 [**ID3D11Buffer**](https://msdn.microsoft.com/library/windows/desktop/ff476351) 可能会包含所有内容！ 提供了 [**D3D11\_BUFFER\_DESC**](https://msdn.microsoft.com/library/windows/desktop/ff476092) 和 [**D3D11\_SUBRESOURCE\_DATA**](https://msdn.microsoft.com/library/windows/desktop/ff476220) 结构，目的是确保 Direct3D 理解缓冲区的物理内存布局，其中包括缓冲区中每个顶点元素的大小以及顶点列表的最大大小。 你也可以在此处控制对缓冲区内存的访问以及遍历的方式，但这有点超出本教程的范围。

配置缓冲区之后，调用 [**ID3D11Device::CreateBuffer**](https://msdn.microsoft.com/library/windows/desktop/ff476501) 来实际创建缓冲区。 很明显，如果你拥有多个对象，则为每个唯一的模型创建缓冲区。

声明并创建顶点缓冲区。

```cpp
D3D11_BUFFER_DESC vertexBufferDesc = {0};
vertexBufferDesc.ByteWidth = sizeof(SimpleCubeVertex) * ARRAYSIZE(cubeVertices);
vertexBufferDesc.Usage = D3D11_USAGE_DEFAULT;
vertexBufferDesc.BindFlags = D3D11_BIND_VERTEX_BUFFER;
vertexBufferDesc.CPUAccessFlags = 0;
vertexBufferDesc.MiscFlags = 0;
vertexBufferDesc.StructureByteStride = 0;

D3D11_SUBRESOURCE_DATA vertexBufferData;
vertexBufferData.pSysMem = cubeVertices;
vertexBufferData.SysMemPitch = 0;
vertexBufferData.SysMemSlicePitch = 0;

ComPtr<ID3D11Buffer> vertexBuffer;
m_d3dDevice->CreateBuffer(
                &vertexBufferDesc,
                &vertexBufferData,
                &vertexBuffer);
```

加载顶点。 但处理这些顶点有什么顺序吗？ 当你为顶点提供索引列表时进行处理时，这些索引的顺序即为顶点着色器处理它们的顺序。

### 步骤 4：填充索引缓冲区

现在，你为每个顶点提供一个索引列表。 这些索引与顶点缓冲区中顶点的位置相对应，从 0 开始。 为了帮助你看到这种情况，考虑为网格中的每个唯一顶点都分配一个唯一的数字，如分配一个 ID。 这个 ID 是顶点缓冲区中顶点的整数位置。

![一个包含八个带编号的顶点的立方体](images/cube-mesh-1.png)

在我们的示例立方体中，你拥有 8 个顶点，它们为侧面创建了 6 个四边形。 将四边形分割成三角形，使用 8 个顶点总共组成 12 个三角形。 每个三角形有 3 个顶点，那么在我们的索引缓冲区中便拥有 36 个项目。 在我们的示例中，该索引模式称为三角形列表，当你设置基元拓扑时你将其指示到 Direct3D 作为 **D3D11\_PRIMITIVE\_TOPOLOGY\_TRIANGLELIST**。

这可能是列出索引的最低效方式，因为当三角形共享点和面时有很多冗余。 例如，当在菱形中时三角形共享一个边，你却为四个顶点列出了 6 个索引，如下所示：

![构造菱形时的索引顺序](images/rhombus-surface-1.png)

-   三角形 1：\[0, 1, 2\]
-   三角形 2：\[0, 2, 3\]

在带状或扇形拓扑中，按照在遍历期间消除很多冗余边的方式对顶点进行排序（如图中从索引 0 到索引 2 的边）。对于大型网格，这会大大减少运行顶点着色器的次数，从而显著提高性能。 但是，我们将保持它的简单性并且继续使用三角形列表。

将顶点缓冲区的索引声明为一个简单的三角形列表拓扑。

```cpp
unsigned short cubeIndices[] =
{   0, 1, 2,
    0, 2, 3,

    4, 5, 6,
    4, 6, 7,

    3, 2, 5,
    3, 5, 4,

    2, 1, 6,
    2, 6, 5,

    1, 7, 6,
    1, 0, 7,

    0, 3, 4,
    0, 4, 7 };
```

当你只有 8 个顶点时，缓冲区中的 36 个索引元素非常多余！ 如果你选择消除一些冗余并使用不同的顶点列表类型，如某一带状或扇形区域，当你为 [**ID3D11DeviceContext::IASetPrimitiveTopology**](https://msdn.microsoft.com/library/windows/desktop/ff476455) 方法提供一个特定的 [**D3D11\_PRIMITIVE\_TOPOLOGY**](https://msdn.microsoft.com/library/windows/desktop/ff476189) 值时，必须指定该类型。

有关不同索引列表技术的详细信息，请参阅[基元拓扑](https://msdn.microsoft.com/library/windows/desktop/bb205124)。

### 步骤 5：为你的转换矩阵创建一个恒定的缓冲区

开始处理顶点之前，你需要提供运行时将应用于（相乘）每个顶点的转换矩阵。 对于大多数 3-D 游戏，包含其中三个：

-   将对象（模型）坐标系转换为总体世界坐标系的 4x4 矩阵。
-   将世界坐标系转换为相机（视图）坐标系的 4x4 矩阵。
-   将相机坐标系转换为 2-D 视图投影坐标系的 4x4 矩阵。

这些矩阵传递给*恒定缓冲区*中的着色器。 恒定缓冲区是在执行着色器管道的下次传递过程中保持不变的内存区域，着色器可以从 HLSL 代码中直接对其进行访问。 每个恒定缓冲区定义两次：第一次在游戏的 C++ 代码中，一次（至少）在着色器代码的类似于 C 的 HLSL 语法中。 这两个声明都必须与类型和数据对齐直接对应。 引入很容易，但当着色器使用 HLSL 声明解释 C++ 中声明的数据时很难发现错误，并且类型不匹配或者数据对齐被禁用！

恒定缓冲区不会被 HLSL 更改。 你可以在游戏更新特定数据时更改它们。 通常，游戏设备会创建 4 类恒定缓冲区：一种类型用于每个帧的更新；一种类型用于每个模型/对象的更新；一种类型用于每个游戏状态刷新的更新；一种类型用于整个游戏的生存时间内从不更改的数据。

在该示例中，我们只有一个从不更改的恒定缓冲区：三个矩阵的 DirectX::XMFLOAT4X4 数据。

> **注意** 此处呈现的示例代码使用列主序矩阵。 可以通过在 HLSL 中使用 **row\_major** 关键字并且确保源矩阵数据也是行主序来使用行主序矩阵。 DirectXMath 使用行主序矩阵，并且可以直接用于使用 **row\_major** 关键字定义的 HLSL 矩阵。

 

为用于转换每个顶点的三个矩阵声明并创建一个恒定缓冲区。

```cpp
struct ConstantBuffer
{
    DirectX::XMFLOAT4X4 model;
    DirectX::XMFLOAT4X4 view;
    DirectX::XMFLOAT4X4 projection;
};
ComPtr<ID3D11Buffer> m_constantBuffer;
ConstantBuffer m_constantBufferData;

// ...

// Create a constant buffer for passing model, view, and projection matrices
// to the vertex shader.  This allows us to rotate the cube and apply
// a perspective projection to it.

D3D11_BUFFER_DESC constantBufferDesc = {0};
constantBufferDesc.ByteWidth = sizeof(m_constantBufferData);
constantBufferDesc.Usage = D3D11_USAGE_DEFAULT;
constantBufferDesc.BindFlags = D3D11_BIND_CONSTANT_BUFFER;
constantBufferDesc.CPUAccessFlags = 0;
constantBufferDesc.MiscFlags = 0;
constantBufferDesc.StructureByteStride = 0;
m_d3dDevice->CreateBuffer(
                &constantBufferDesc,
                nullptr,
                &m_constantBuffer
             );

m_constantBufferData.model = DirectX::XMFLOAT4X4( // Identity matrix, since you are not animating the object
            1.0f, 0.0f, 0.0f, 0.0f,
            0.0f, 1.0f, 0.0f, 0.0f,
            0.0f, 0.0f, 1.0f, 0.0f,
            0.0f, 0.0f, 0.0f, 1.0f);

);
// Specify the view (camera) transform corresponding to a camera position of
// X = 0, Y = 1, Z = 2.  

m_constantBufferData.view = DirectX::XMFLOAT4X4(
            -1.00000000f, 0.00000000f,  0.00000000f,  0.00000000f,
             0.00000000f, 0.89442718f,  0.44721359f,  0.00000000f,
             0.00000000f, 0.44721359f, -0.89442718f, -2.23606800f,
             0.00000000f, 0.00000000f,  0.00000000f,  1.00000000f);
```

> **注意** 通常当设置特定于设备的资源时声明投影矩阵，因为与它相乘的结果必须与当前二维视口大小参数（通常与显示器的像素高度和宽度相对应）匹配。 如果这些内容发生改变，则必须相应地缩放 x 和 y 坐标值。

 

```cpp
// Finally, update the constant buffer perspective projection parameters
// to account for the size of the application window.  In this sample,
// the parameters are fixed to a 70-degree field of view, with a depth
// range of 0.01 to 100.  

float xScale = 1.42814801f;
float yScale = 1.42814801f;
if (backBufferDesc.Width > backBufferDesc.Height)
{
    xScale = yScale *
                static_cast<float>(backBufferDesc.Height) /
                static_cast<float>(backBufferDesc.Width);
}
else
{
    yScale = xScale *
                static_cast<float>(backBufferDesc.Width) /
                static_cast<float>(backBufferDesc.Height);
}
m_constantBufferData.projection = DirectX::XMFLOAT4X4(
            xScale, 0.0f,    0.0f,  0.0f,
            0.0f,   yScale,  0.0f,  0.0f,
            0.0f,   0.0f,   -1.0f, -0.01f,
            0.0f,   0.0f,   -1.0f,  0.0f
            );
```

当你位于此处时，在 [ID3D11DeviceContext](https://msdn.microsoft.com/library/windows/desktop/ff476149) 上设置顶点和索引缓冲区，以及你使用的拓扑。

```cpp
// Set the vertex and index buffers, and specify the way they define geometry.
UINT stride = sizeof(SimpleCubeVertex);
UINT offset = 0;
m_d3dDeviceContext->IASetVertexBuffers(
                0,
                1,
                vertexBuffer.GetAddressOf(),
                &stride,
                &offset);

m_d3dDeviceContext->IASetIndexBuffer(
                indexBuffer.Get(),
                DXGI_FORMAT_R16_UINT,
                0);

 m_d3dDeviceContext->IASetPrimitiveTopology(D3D11_PRIMITIVE_TOPOLOGY_TRIANGLELIST);
```

非常好！ 输入程序集已完成。 所有内容都可用于呈现。 让我们开始使用顶点着色器。

### 步骤 6：使用顶点着色器处理网格

现在，你已经拥有一个顶点缓冲区（它的顶点定义网格）以及定义处理顶点顺序的索引缓冲区，将它们发送到顶点着色器。 以编译的高级着色器语言表示的顶点着色器代码为顶点缓冲区中的每个顶点运行一次，从而允许你执行每个顶点的转换。 最终结果通常是一个 2-D 投影。

（加载顶点着色器了吗？ 如果没有，请查阅[如何在 DirectX 游戏中加载资源](load-a-game-asset.md)。）

在这里，你创建顶点着色器...

``` syntax
// Set the vertex and pixel shader stage state.
m_d3dDeviceContext->VSSetShader(
                vertexShader.Get(),
                nullptr,
                0);
```

...并设置恒定缓冲区。

``` syntax
m_d3dDeviceContext->VSSetConstantBuffers(
                0,
                1,
                m_constantBuffer.GetAddressOf());
```

下面是处理从对象坐标到世界坐标，然后到 2-D 视图投影坐标系的转换的顶点着色器代码。 你也可以应用一些简单的每个顶点照明来使它们更漂亮。 该操作需要在顶点着色器的 HLSL 文件（在该示例中为 SimplerVertexShader.hlsl）中进行。

``` syntax
cbuffer simpleConstantBuffer : register( b0 )
{
    matrix model;
    matrix view;
    matrix projection;
};

struct VertexShaderInput
{
    DirectX::XMFLOAT3 pos : POSITION;
    DirectX::XMFLOAT3 color : COLOR;
};

struct PixelShaderInput
{
    float4 pos : SV_POSITION;
    float4 color : COLOR;
};

PixelShaderInput SimpleVertexShader(VertexShaderInput input)
{
    PixelShaderInput vertexShaderOutput;
    float4 pos = float4(input.pos, 1.0f);

    // Transform the vertex position into projection space.
    pos = mul(pos, model);
    pos = mul(pos, view);
    pos = mul(pos, projection);
    vertexShaderOutput.pos = pos;

    // Pass the vertex color through to the pixel shader.
    vertexShaderOutput.color = float4(input.color, 1.0f);

    return vertexShaderOutput;
}
```

看到顶部的 **cbuffer** 了吗？ 它是与我们前面在 C++ 代码中声明的恒定缓冲区类似的 HLSL 恒定缓冲区。 看到 **VertexShaderInputstruct** 了吗？ 为什么它看起来就像是你的输入布局和顶点数据声明！ 重要的是，C++ 代码中的恒定缓冲区和顶点数据声明要与 HLSL 代码中的声明相匹配，这包括符号、类型以及数据对齐。

**PixelShaderInput** 指定顶点着色器的 main 函数返回的数据的布局。 当你完成处理某个顶点时，你将返回 2-D 投影空间中的某个顶点位置以及用于每个顶点照明的颜色。 图形卡使用着色器的数据输出来计算当在管道的下一阶段中运行像素着色器时必须着色的“分段”（可能的像素）。

### 步骤 7：通过像素着色器传递网格

通常，在在图形管道的这个阶段，需要在项目的可见投影面上执行每像素操作。 （人们喜欢纹理。）但出于示例的目的，你只是在该阶段传递它。

首先，让我们创建像素着色器的一个实例。 像素着色器将针对场景的 2-D 投影中的每个像素运行，从而为该像素分配一种颜色。 在这种情况下，我们直接传递顶点着色器返回的像素的颜色。

设置像素着色器。

``` syntax
m_d3dDeviceContext->PSSetShader( pixelShader.Get(), nullptr, 0 );
```

在 HLSL 中定义一个传递像素着色器。

``` syntax
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

将该代码放置在不同于顶点着色器 HLSL 的 HLSL 文件（如 SimplePixelShader.hlsl）中。 该代码为视口中的每个可见像素运行一次（你正在绘制到的屏幕部分的内存中表示），在这种情况下，映射到整个屏幕。 现在，你的图形管道已完全定义！

### 步骤 8：对网格进行光栅化并显示

让我们运行管道。 该操作非常简单：调用 [**ID3D11DeviceContext::DrawIndexed**](https://msdn.microsoft.com/library/windows/desktop/bb173565)。

绘制该立方体！

```cpp
// Draw the cube.
m_d3dDeviceContext->DrawIndexed( ARRAYSIZE(cubeIndices), 0, 0 );
            
```

在图形卡内，按照索引缓冲区中指定的顺序处理每个顶点。 代码已执行顶点着色器并定义 2-D 分段之后，调用像素着色器并对三角形进行着色。

现在，将立方体放在屏幕上。

将该帧缓冲区呈现到显示器。

```cpp
// Present the rendered image to the window.  Because the maximum frame latency is set to 1,
// the render loop is generally  throttled to the screen refresh rate, typically around
// 60 Hz, by sleeping the app on Present until the screen is refreshed.

m_swapChain->Present(1, 0);
```

已完成！ 若要查看充满模型的场景，请使用多个顶点和索引缓冲区，并且不同的模型类型可以拥有不同的着色器。 请记住，每个模型都有其自己的坐标系，你需要使用在恒定缓冲区中定义的矩阵将其转换为共享的世界坐标系。

## 备注

本主题介绍创建以及显示你自己创建的简单几何体。 有关从文件中加载更复杂的几何图形以及将其转换为特定于示例的顶点缓冲区对象 (.vbo) 格式的详细信息，请参阅[如何在 DirectX 游戏中加载资源](load-a-game-asset.md)。

> **注意**  
本文适用于编写通用 Windows 平台 (UWP) 应用的 Windows 10 开发人员。 如果你要针对 Windows 8.x 或 Windows Phone 8.x 进行开发，请参阅[存档文档](http://go.microsoft.com/fwlink/p/?linkid=619132)。

 

## 相关主题


* [如何在 DirectX 游戏中加载资源](load-a-game-asset.md)

 

 






<!--HONumber=Mar16_HO1-->


