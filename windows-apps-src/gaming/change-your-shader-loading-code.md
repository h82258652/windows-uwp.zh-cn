---
title: 将 OpenGL ES 2.0 着色器管道与 Direct3D 进行比较
description: 从概念上来说，Direct3D 11 着色器管道与 OpenGL ES 2.0 中的着色器管道非常相似。
ms.assetid: 3678a264-e3f9-72d2-be91-f79cd6f7c4ca
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, 游戏, opengl, direct3d, 着色器管道
ms.localizationpriority: medium
ms.openlocfilehash: 7a35102fed9993ca37afa1d1f47850427235ed49
ms.sourcegitcommit: cbd900f350569a3901086a44b2d5007bb6fb7bed
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/11/2019
ms.locfileid: "72276296"
---
# <a name="compare-the-opengl-es-20-shader-pipeline-to-direct3d"></a>将 OpenGL ES 2.0 着色器管道与 Direct3D 进行比较




**重要的 API**

-   [输入-汇编程序阶段](https://docs.microsoft.com/windows/desktop/direct3d11/d3d10-graphics-programming-guide-input-assembler-stage)
-   [顶点-着色器阶段](https://docs.microsoft.com/previous-versions/bb205146(v=vs.85))
-   [像素着色器阶段](https://docs.microsoft.com/previous-versions/bb205146(v=vs.85))

从概念上来说，Direct3D 11 着色器管道与 OpenGL ES 2.0 中的着色器管道非常相似。 但是，就 API 设计而言，用于创建和管理着色器阶段的主要组件是两个主要接口 [**ID3D11Device1**](https://docs.microsoft.com/windows/desktop/api/d3d11_1/nn-d3d11_1-id3d11device1) 和 [**ID3D11DeviceContext1**](https://docs.microsoft.com/windows/desktop/api/d3d11_1/nn-d3d11_1-id3d11devicecontext1) 的一部分。 本主题尝试在这些接口中将常用的 OpenGL ES 2.0 着色器管道 API 模式映射到 Direct3D 11 同等模式。

## <a name="reviewing-the-direct3d-11-shader-pipeline"></a>查看 Direct3D 11 着色器管道


着色器对象是使用 [**ID3D11Device1**](https://docs.microsoft.com/windows/desktop/api/d3d11_1/nn-d3d11_1-id3d11device1) 接口上的方法创建的，如 [**ID3D11Device1::CreateVertexShader**](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-id3d11device-createvertexshader) 和 [**ID3D11Device1::CreatePixelShader**](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-id3d11device-createpixelshader)。

Direct3D 11 图形管道由 [**ID3D11DeviceContext1**](https://docs.microsoft.com/windows/desktop/api/d3d11_1/nn-d3d11_1-id3d11devicecontext1) 接口的实例管理，并且包含以下阶段：

-   [输入装配器阶段](https://docs.microsoft.com/windows/desktop/direct3d11/d3d10-graphics-programming-guide-input-assembler-stage)。 输入装配器阶段为管道提供数据（三角形、直线和点）。 支持此阶段的[**ID3D11DeviceContext1**](https://docs.microsoft.com/windows/desktop/api/d3d11_1/nn-d3d11_1-id3d11devicecontext1)方法以 "IA" 为前缀。
-   [顶点着色器阶段](https://docs.microsoft.com/previous-versions/bb205146(v=vs.85)) - 顶点着色器阶段处理顶点，通常执行诸如转换、换肤以及照明之类的操作。 顶点着色器始终获取一个输入顶点并生成一个输出顶点。 支持此阶段的[**ID3D11DeviceContext1**](https://docs.microsoft.com/windows/desktop/api/d3d11_1/nn-d3d11_1-id3d11devicecontext1)方法以 "VS" 为前缀。
-   [流输出阶段](https://docs.microsoft.com/windows/desktop/direct3d11/d3d10-graphics-programming-guide-output-stream-stage) - 流输出阶段将管道中的基元数据沿着到光栅器的路线流到内存中。 数据可以流出和/或传递到光栅器中。 流出到内存的数据可以作为输入数据再次循环回到管道，或者从 CPU 读回。 支持此阶段的[**ID3D11DeviceContext1**](https://docs.microsoft.com/windows/desktop/api/d3d11_1/nn-d3d11_1-id3d11devicecontext1)方法以 "SO" 为前缀。
-   [光栅器阶段](https://docs.microsoft.com/windows/desktop/direct3d11/d3d10-graphics-programming-guide-rasterizer-stage) - 光栅器剪辑基元，为像素着色器准备基元并确定如何调用像素着色器。 可以通过以下方式禁用光栅化：指示管道没有像素着色器（将像素着色器阶段设置为 NULL， [**ID3D11DeviceContext：:P ssetshader**](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-pssetshader)），禁用深度和模具测试（将 DepthEnable 和 StencilEnable 设置为 FALSE [**D3D11 @ no__t-4DEPTH @ no__t-5STENCIL @ no__t-6DESC**](https://docs.microsoft.com/windows/desktop/api/d3d11/ns-d3d11-d3d11_depth_stencil_desc)）。 禁用后，与光栅化有关的管道计数器将无法更新。
-   [像素着色器阶段](https://docs.microsoft.com/previous-versions/bb205146(v=vs.85)) - 像素着色器阶段接收基元的插值数据并生成每像素数据，如颜色。 支持此阶段的[**ID3D11DeviceContext1**](https://docs.microsoft.com/windows/desktop/api/d3d11_1/nn-d3d11_1-id3d11devicecontext1)方法以 "PS" 为前缀。
-   [输出合并器阶段](https://docs.microsoft.com/windows/desktop/direct3d11/d3d10-graphics-programming-guide-output-merger-stage) - 输出合并器阶段将各种类型的输出数据（像素着色器值、深度和模具信息）与呈现器目标的内容以及深度/模具缓冲区组合在一起，以生成最终的管道结果。 支持此阶段的[**ID3D11DeviceContext1**](https://docs.microsoft.com/windows/desktop/api/d3d11_1/nn-d3d11_1-id3d11devicecontext1)方法以 "OM" 为前缀。

（还可以使用几何着色器、凸着色器、tesselators 和域着色器的阶段，但由于它们在 OpenGL ES 2.0 中没有物，因此我们不会在此处讨论它们。）有关这些阶段的方法的完整列表，请参阅[**ID3D11DeviceContext**](https://docs.microsoft.com/windows/desktop/api/d3d11/nn-d3d11-id3d11devicecontext)和[**ID3D11DeviceContext1**](https://docs.microsoft.com/windows/desktop/api/d3d11_1/nn-d3d11_1-id3d11devicecontext1)参考页。 **ID3D11DeviceContext1** 扩展了 Direct3D 11 的 **ID3D11DeviceContext**。

## <a name="creating-a-shader"></a>创建着色器


在 Direct3D 中，在编译和加载着色器资源之前不会创建着色器资源，而是在加载 HLSLis 时创建该资源。 因此，不存在对 glCreateShader 的直接类似函数，这将创建特定类型的已初始化着色器资源（如总帐 @ no__t-0VERTEX @ no__t-1SHADER 或总帐 @ no__t-2FRAGMENT @ no__t-3SHADER）。 着色器是在使用特定的函数（如 [**ID3D11Device1::CreateVertexShader**](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-id3d11device-createvertexshader) 和 [**ID3D11Device1::CreatePixelShader**](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-id3d11device-createpixelshader)）加载 HLSL 之后创建的，并且采用类型和编译的 HLSL 作为参数。

| OpenGL ES 2.0  | Direct3D 11                                                                                                                                                                                                                                                             |
|----------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| glCreateShader | 成功加载编译的着色器对象并将这些对象作为缓冲区传递给 CSO 之后调用 [**ID3D11Device1::CreateVertexShader**](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-id3d11device-createvertexshader) 和 [**ID3D11Device1::CreatePixelShader**](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-id3d11device-createpixelshader)。 |

 

## <a name="compiling-a-shader"></a>编译着色器


Direct3D 着色器必须预编译为通用 Windows 平台（UWP）应用中的已编译的着色器对象（cso）文件，并使用其中一个 Windows 运行时文件 Api 进行加载。 （桌面应用程序可以在运行时从文本文件或字符串编译着色器。）CSO 文件是从 Microsoft Visual Studio 项目中包含的任何 hlsl 文件生成的，并保留相同的名称，但仅使用 CSO 文件扩展名。 请确保交付你的程序包时附带这些文件！

| OpenGL ES 2.0                          | Direct3D 11                                                                                                                                                                   |
|----------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| glCompileShader                        | N/A。 将着色器编译为 Visual Studio 中的 .cso 文件，并将这些文件包含在你的程序包中。                                                                                     |
| 对编译状态使用 glGetShaderiv | N/A。 如果编译中存在错误，请从 Visual Studio 的 FX 编译器 (FXC) 中查看编译输出。 如果编译成功，则会创建一个相应的 CSO 文件。 |

 

## <a name="loading-a-shader"></a>加载着色器


如创建着色器的部分中所述，Direct3D 11 会在相应的 CSO 文件加载到缓冲区中并传递到下表中的其中一个方法时创建着色器。

| OpenGL ES 2.0 | Direct3D 11                                                                                                                                                                                                                           |
|---------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| ShaderSource  | 在成功加载编译的着色器对象之后调用 [**ID3D11Device1::CreateVertexShader**](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-id3d11device-createvertexshader) 和 [**ID3D11Device1::CreatePixelShader**](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-id3d11device-createpixelshader)。 |

 

## <a name="setting-up-the-pipeline"></a>设置管道


OpenGL ES 2.0 具有“着色器程序”对象，该对象包含多个用于执行的着色器。 各个着色器都与该着色器程序对象相连接。 但是，在 Direct3D 11 中，你可以直接使用呈现上下文 ([**ID3D11DeviceContext1**](https://docs.microsoft.com/windows/desktop/api/d3d11_1/nn-d3d11_1-id3d11devicecontext1)) 并在其上创建着色器。

| OpenGL ES 2.0   | Direct3D 11                                                                                   |
|-----------------|-----------------------------------------------------------------------------------------------|
| glCreateProgram | N/A。 Direct3D 11 不使用着色器程序对象抽象。                          |
| glLinkProgram   | N/A。 Direct3D 11 不使用着色器程序对象抽象。                          |
| glUseProgram    | N/A。 Direct3D 11 不使用着色器程序对象抽象。                          |
| glGetProgramiv  | 使用你创建的 [**ID3D11DeviceContext1**](https://docs.microsoft.com/windows/desktop/api/d3d11_1/nn-d3d11_1-id3d11devicecontext1) 参考。 |

 

使用静态的 [**D3D11CreateDevice**](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-d3d11createdevice) 方法创建 [**ID3D11DeviceContext1**](https://docs.microsoft.com/windows/desktop/api/d3d11_1/nn-d3d11_1-id3d11devicecontext1) 和 [**ID3D11Device1**](https://docs.microsoft.com/windows/desktop/api/d3d11_2/nn-d3d11_2-id3d11device2) 的实例。

``` syntax
Microsoft::WRL::ComPtr<ID3D11Device1>          m_d3dDevice;
Microsoft::WRL::ComPtr<ID3D11DeviceContext1>  m_d3dContext;

// ...

D3D11CreateDevice(
  nullptr, // Specify nullptr to use the default adapter.
  D3D_DRIVER_TYPE_HARDWARE,
  nullptr,
  creationFlags, // Set set debug and Direct2D compatibility flags.
  featureLevels, // List of feature levels this app can support.
  ARRAYSIZE(featureLevels),
  D3D11_SDK_VERSION, // Always set this to D3D11_SDK_VERSION for UWP apps.
  &device, // Returns the Direct3D device created.
  &m_featureLevel, // Returns feature level of device created.
  &m_d3dContext // Returns the device's immediate context.
);
```

## <a name="setting-the-viewports"></a>设置视区


在 Direct3D 11 中设置视口与在 OpenGL ES 2.0 中设置视口的方法非常相似。 在 Direct3D 11 中，使用配置的[**CD3D11 @ no__t-4VIEWPORT**](https://docs.microsoft.com/previous-versions/windows/desktop/legacy/jj151722(v=vs.85))调用[**ID3D11DeviceContext：： RSSetViewports**](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-rssetviewports) 。

Direct3D 11：设置视区。

``` syntax
CD3D11_VIEWPORT viewport(
        0.0f,
        0.0f,
        m_d3dRenderTargetSize.Width,
        m_d3dRenderTargetSize.Height
        );
m_d3dContext->RSSetViewports(1, &viewport);
```

| OpenGL ES 2.0 | Direct3D 11                                                                                                                                  |
|---------------|----------------------------------------------------------------------------------------------------------------------------------------------|
| glViewport    | [**CD3D11 @ no__t-2VIEWPORT**](https://docs.microsoft.com/previous-versions/windows/desktop/legacy/jj151722(v=vs.85))， [ **ID3D11DeviceContext：： RSSetViewports**](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-rssetviewports) |

 

## <a name="configuring-the-vertex-shaders"></a>配置顶点着色器


在 Direct3D 11 中配置顶点着色器是在加载该着色器时完成的。 使用 [**ID3D11DeviceContext1::VSSetConstantBuffers1**](https://docs.microsoft.com/windows/desktop/api/d3d11_1/nf-d3d11_1-id3d11devicecontext1-vssetconstantbuffers1) 将 uniform 作为常量缓冲区传递。

| OpenGL ES 2.0                    | Direct3D 11                                                                                               |
|----------------------------------|-----------------------------------------------------------------------------------------------------------|
| glAttachShader                   | [**ID3D11Device1::CreateVertexShader**](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-id3d11device-createvertexshader)                       |
| glGetShaderiv、glGetShaderSource | [**ID3D11DeviceContext1::VSGetShader**](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-vsgetshader)                       |
| glGetUniformfv、glGetUniformiv   | [**ID3D11DeviceContext1：： VSGetConstantBuffers1**](https://docs.microsoft.com/windows/desktop/api/d3d11_1/nf-d3d11_1-id3d11devicecontext1-vsgetconstantbuffers1)。 |

 

## <a name="configuring-the-pixel-shaders"></a>配置像素着色器


在 Direct3D 11 中配置像素着色器是在加载该着色器时完成的。 使用 [**ID3D11DeviceContext1::PSSetConstantBuffers1**](https://docs.microsoft.com/windows/desktop/api/d3d11_1/nf-d3d11_1-id3d11devicecontext1-pssetconstantbuffers1) 将 Uniform 作为常量缓冲区传递。

| OpenGL ES 2.0                    | Direct3D 11                                                                                               |
|----------------------------------|-----------------------------------------------------------------------------------------------------------|
| glAttachShader                   | [**ID3D11Device1::CreatePixelShader**](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-id3d11device-createpixelshader)                         |
| glGetShaderiv、glGetShaderSource | [**ID3D11DeviceContext1：:P SGetShader**](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-psgetshader)                       |
| glGetUniformfv、glGetUniformiv   | [**ID3D11DeviceContext1：:P sgetconstantbuffers1**](https://docs.microsoft.com/windows/desktop/api/d3d11_1/nf-d3d11_1-id3d11devicecontext1-psgetconstantbuffers1)。 |

 

## <a name="generating-the-final-results"></a>生成最终结果


管道完成后，会在后台缓冲区中绘制着色器各个阶段的结果。 在 Direct3D 11 中，正如使用 Open GL ES 2.0 时一样，这涉及调用一个绘制命令以在后台缓冲区中将结果输出为颜色图，然后将这个后台缓冲区发送到显示器。

| OpenGL ES 2.0  | Direct3D 11                                                                                                                                                                                                                                         |
|----------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| glDrawElements | [**ID3D11DeviceContext1：:D raw**](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-draw)， [**ID3D11DeviceContext1：:D rawindexed**](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-drawindexed) （或其他[**上的**](https://docs.microsoft.com/windows/desktop/api/d3d11/nn-d3d11-id3d11devicecontext)方法）。 |
| eglSwapBuffers | [**IDXGISwapChain1::Present1**](https://docs.microsoft.com/windows/desktop/api/dxgi1_2/nf-dxgi1_2-idxgiswapchain1-present1)                                                                                                                                                                              |

 

## <a name="porting-glsl-to-hlsl"></a>将 GLSL 移植到 HLSL


除了复杂的类型支持和一些整体语法之外，GLSL 和 HLSL 没有太大的不同。 很多开发人员发现将常用的 OpenGL ES 2.0 指令和定义的别名设置为其 HLSL 同等内容最便于在两者之间进行移植。 请注意，Direct3D 使用着色器模型版本来表示图形接口所支持的 HLSL 的功能集；OpenGL 有一个不同版本的 HLSL 规范。 下表给出了其他版本中为 Direct3D 11 和 OpenGL ES 2.0 定义着色器语言功能集的大概思路。

| 着色器语言           | GLSL 功能版本                                                                                                                                                                                                      | Direct3D 着色器模型 |
|---------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-----------------------|
| Direct3D 11 HLSL          | ~4.30。                                                                                                                                                                                                                    | SM 5.0                |
| GLSL ES for OpenGL ES 2.0 | 1.40。 GLSL ES for OpenGL ES 2.0 的旧实现可能会使用 1.10 至 1.30。 通过 glGetString （GL @ no__t-0SHADING @ no__t-1LANGUAGE @ no__t-2VERSION）或 glGetString （底纹 @ no__t-3LANGUAGE @ no__t-4VERSION）检查原始代码，以确定它。 | ~SM 2.0               |

 

有关这两个着色器语言之间的差别以及常用语法映射的详细信息，请阅读 [GLSL 到 HLSL 参考](glsl-to-hlsl-reference.md)。

## <a name="porting-the-opengl-intrinsics-to-hlsl-semantics"></a>将 OpenGL 内部函数移植到 HLSL 语义


Direct3D 11 HLSL 语义是用来标识在应用和着色器程序之间传递的值的字符串，如 uniform 或属性名称。 尽管它们可以是任何类型的字符串，但最好使用指示用法的字符串，如 POSITION 或 COLOR。 在构造常量缓冲区或缓冲区输入布局时分配这些语义。 也可以为语义中附加一个介于 0 和 7 之间的数字，以便你可以为相似的值使用不同的寄存器。 例如：COLOR0、COLOR1、COLOR2 。

前缀为 "SV @ no__t-0" 的语义是由着色器程序写入的系统值语义;应用本身（在 CPU 上运行）无法修改它们。 通常，它们的值为图形管道中其他着色器阶段的输入或输出，或者完全由 GPU 生成。

此外，当 SV @ no__t-0 语义用于指定着色器阶段的输入或输出时，它们具有不同的行为。 例如，SV @ no__t-0POSITION （output）包含在顶点着色器阶段转换的顶点数据，SV @ no__t-1POSITION （input）包含在光栅化过程中插值的像素位置值。

下面是常用的 OpenGL ES 2.0 着色器内部函数的几个映射：

| OpenGL 系统值 | 使用此 HLSL 语义                                                                                                                                                   |
|---------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 总帐 @ no__t-0Position        | POSITION(n) 针对顶点缓冲区数据。 SV @ no__t-0POSITION 提供像素着色器的像素位置，并且应用程序不能写入。                                        |
| 总帐 @ no__t-0Normal          | NORMAL(n) 针对由顶点缓冲区提供的普通数据。                                                                                                                 |
| 总帐 @ no__t-0TexCoord @ no__t-1n @ no__t-2   | TEXCOORD(n) 针对提供给着色器的纹理 UV（在某些 OpenGL 文档中为 ST）坐标数据。                                                                       |
| 总帐 @ no__t-0FragColor       | COLOR(n) 针对提供给着色器的 RGBA 颜色数据。 请注意，处理方式与坐标数据相同；语义只是帮助你确定它是颜色数据。 |
| 总帐 @ no__t-0FragData @ no__t-1n @ no__t-2   | SV @ no__t-0Target @ no__t-1n @ no__t-2，将像素着色器写入目标纹理或其他像素缓冲区。                                                                               |

 

编写语义代码的方法并不同于在 OpenGL ES 2.0 中使用内部函数。 在 OpenGL 中，你可以直接访问很多内部函数，而无需进行任何配置或声明；在 Direct3D 中，必须在特定的常量缓冲区中声明一个字段才能使用某个特殊的语义，或者你将其声明为着色器的 **main()** 方法的返回值。

下面是常量缓冲区定义中使用的语义示例：

```cpp
struct VertexShaderInput
{
  float3 pos : POSITION;
  float3 color : COLOR0;
};

// The position is interpolated to the pixel value by the system. The per-vertex color data is also interpolated and passed through the pixel shader. 
struct PixelShaderInput
{
  float4 pos : SV_POSITION;
  float3 color : COLOR0;
};
```

该代码定义一对简单的常量缓冲区

下面是用来定义片段着色器返回值的语义示例：

```cpp
// A pass-through for the (interpolated) color data.
float4 main(PixelShaderInput input) : SV_TARGET
{
  return float4(input.color,1.0f);
}
```

在这种情况下，SV @ no__t-0TARGET 是呈现器目标的位置，在着色器完成执行时，像素颜色（定义为具有四个浮点值的矢量）将写入该目标。

有关 Direct3D 的语义使用的详细信息，请阅读 [HLSL 语义](https://docs.microsoft.com/windows/desktop/direct3dhlsl/dx-graphics-hlsl-semantics)。

 

 




