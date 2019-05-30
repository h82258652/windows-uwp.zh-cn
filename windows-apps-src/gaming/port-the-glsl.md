---
title: 移植 GLSL
description: 转到创建和配置缓冲区及着色器对象的代码之后，应该将这些着色器中的代码从 OpenGL ES 2.0 的 GL 着色器语言 (GLSL) 移植到 Direct3D 11 的高级着色器语言 (HLSL)。
ms.assetid: 0de06c51-8a34-dc68-6768-ea9f75dc57ee
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, 游戏, glsl, 移植
ms.localizationpriority: medium
ms.openlocfilehash: 210f98476a06b88e7d3d543006a6d4ec886cfd45
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/29/2019
ms.locfileid: "66368256"
---
# <a name="port-the-glsl"></a>移植 GLSL




**重要的 Api**

-   [HLSL 语义](https://docs.microsoft.com/windows/desktop/direct3dhlsl/dcl-usage---ps)
-   [着色器常量 (HLSL)](https://docs.microsoft.com/windows/desktop/direct3dhlsl/dx-graphics-hlsl-constants)

转到创建和配置缓冲区及着色器对象的代码之后，应该将这些着色器中的代码从 OpenGL ES 2.0 的 GL 着色器语言 (GLSL) 移植到 Direct3D 11 的高级着色器语言 (HLSL)。

在 OpenGL ES 2.0 着色器返回的数据之后执行，如使用内部函数**gl\_位置**， **gl\_FragColor**，或者**gl\_FragData\[n\]**  （其中 n 是特定呈现器目标的索引）。 在 Direct3D 中，没有特定的内部函数，着色器以其各自的 main() 函数的返回类型返回数据。

要在着色器阶段之间进行内插的数据（如顶点位置或法线）通过使用 **varying** 声明进行处理。 但是，Direct3D 没有此声明；在着色器阶段之间传递的所有数据都必须使用 [HLSL 语义](https://docs.microsoft.com/windows/desktop/direct3dhlsl/dcl-usage---ps)进行标记。 所选择的特定语义指示数据的用途及含义。 例如，将要在各个片段着色器之间进行插值的顶点数据声明为：

`float4 vertPos : POSITION;`

或

`float4 vertColor : COLOR;`

其中 POSITION 是用于标识顶点位置数据的语义。 POSITION 还是一个特殊情况，因为进行内插后，像素着色器不能对其进行访问。 因此，必须指定像素着色器的输入与 SV\_位置和内插的顶点数据将被放入该变量。

`float4 position : SV_POSITION;`

可以在着色器的主体 (main) 方法上声明语义。 像素着色器，SV\_目标\[n\]，指示呈现器目标，需要对正文方法。 (SV\_目标没有数字后缀默认情况下呈现目标索引为 0。)

另请注意顶点着色器所需输出 SV\_位置系统值语义。 该语义将顶点位置数据解析为坐标值，其中 x 介于 -1 和 1 之间，y 介于 -1 和 1 之间，z 除以原始齐次坐标 w 值 (z/w)，并且 w 为 1 除以原始的 w 值 (1/w)。 像素着色器使用 SV\_位置系统值语义来检索其中 x 是介于 0 和呈现器目标的宽度和 y 之间的像素位置在屏幕上，为 0 到呈现目标高度 （0.5 的每个偏移量）。 功能级别 9\_像素着色器无法从 SV 中读取 x\_位置值。

常量缓冲区必须使用 **cbuffer** 进行声明，并且必须与用于查找的特定起始寄存器相关联。

Direct3D 11:HLSL 常量缓冲区声明

``` syntax
cbuffer ModelViewProjectionConstantBuffer : register(b0)
{
  matrix mvp;
};
```

在此处，常量缓冲区使用寄存器 b0 来包含打包的缓冲区。 所有寄存器在窗体 b 中被都引用到\#。 有关常量缓冲区、寄存器以及数据打包的 HLSL 实现的详细信息，请阅读[着色器常量 (HLSL)](https://docs.microsoft.com/windows/desktop/direct3dhlsl/dx-graphics-hlsl-constants)。

<a name="instructions"></a>说明
------------
### <a name="step-1-port-the-vertex-shader"></a>第 1 步：端口顶点着色器

在我们的简单 OpenGL ES 2.0 示例中，顶点着色器具有三个输入：一个常量模型视图投影 4x4 矩阵和两个 4 坐标矢量。 这两个矢量包含顶点位置及其颜色。 着色器转换为角度来看坐标位置向量，并将其分配给 gl\_光栅化内部函数的位置。 还需要将顶点颜色复制到 varying 变量以在光栅化期间进行内插。

OpenGL ES 2.0:多维数据集对象 (GLSL) 的顶点着色器

``` syntax
uniform mat4 u_mvpMatrix; 
attribute vec4 a_position;
attribute vec4 a_color;
varying vec4 destColor;

void main()
{           
  gl_Position = u_mvpMatrix * a_position;
  destColor = a_color;
}
```

现在，在 Direct3D 中，在注册 b0 打包常量缓冲区中包含常量的模型-视图-投影矩阵和顶点位置和颜色专门使用适当的各自 HLSL 语义标记：位置和颜色。 由于我们的输入布局指示这两个顶点值的特定排列，因此你将创建一个结构来包含它们并在着色器主体函数 (main) 上将其声明为输入参数的类型。 （您还可以作为两个单独的参数，指定它们，但，可能会很麻烦。）此外指定输出类型为此阶段，其中包含的内插的位置和颜色，并将其声明为顶点着色器的正文函数的返回值。

Direct3D 11:多维数据集对象 (HLSL) 的顶点着色器

``` syntax
cbuffer ModelViewProjectionConstantBuffer : register(b0)
{
  matrix mvp;
};

// Per-vertex data used as input to the vertex shader.
struct VertexShaderInput
{
  float3 pos : POSITION;
  float3 color : COLOR;
};

// Per-vertex color data passed through the pixel shader.
struct PixelShaderInput
{
  float3 pos : SV_POSITION;
  float3 color : COLOR;
};

PixelShaderInput main(VertexShaderInput input)
{
  PixelShaderInput output;
  float4 pos = float4(input.pos, 1.0f); // add the w-coordinate

  pos = mul(mvp, projection);
  output.pos = pos;

  output.color = input.color;

  return output;
}
```

在光栅化期间填充输出数据类型 PixelShaderInput 并将其提供给片段（像素）着色器。

### <a name="step-2-port-the-fragment-shader"></a>步骤 2：端口片段着色器

GLSL 在我们的示例片段着色器是非常简单： 提供 gl\_FragColor 内部使用内插的颜色值。 OpenGL ES 2.0 会将其写入到默认的呈现目标。

OpenGL ES 2.0:多维数据集对象 (GLSL) 片段着色器

``` syntax
varying vec4 destColor;

void main()
{
  gl_FragColor = destColor;
} 
```

Direct3D 几乎同样简单。 唯一明显的区别是像素着色器的主体函数必须返回一个值。 由于颜色是 4 坐标 (RGBA) 浮点值，指示作为返回类型，float4，然后指定默认呈现器目标作为 SV\_目标系统值语义。

Direct3D 11:多维数据集对象 (HLSL) 像素着色器

``` syntax
struct PixelShaderInput
{
  float4 pos : SV_POSITION;
  float3 color : COLOR;
};


float4 main(PixelShaderInput input) : SV_TARGET
{
  return float4(input.color, 1.0f);
}
```

像素在位置处的颜色将写入到呈现目标。 现在，让我们在[绘制到屏幕](draw-to-the-screen.md)中看一看如何显示该呈现目标的内容！

## <a name="previous-step"></a>上一步


[移植顶点缓冲区和数据](port-the-vertex-buffers-and-data-config.md) 下一步
---------
[绘制到屏幕](draw-to-the-screen.md) 备注
-------
了解 HLSL 语义以及常量缓冲区的打包可以为你省去调试的麻烦，并且还可以提供优化机会。 如果您有机会，通读[变量语法 (HLSL)](https://docs.microsoft.com/windows/desktop/direct3dhlsl/dx-graphics-hlsl-variable-syntax)，[简介 Direct3D 11 中的缓冲区](https://docs.microsoft.com/windows/desktop/direct3d11/overviews-direct3d-11-resources-buffers-intro)，和[如何：创建常量缓冲区](https://docs.microsoft.com/windows/desktop/direct3d11/overviews-direct3d-11-resources-buffers-constant-how-to)。 如果没有机会通读这些内容，那么请记住下面这些有关语义和常量缓冲区的开始提示：

-   始终要仔细检查呈现器的 Direct3D 配置代码，以确保常量缓冲区的结构与 HLSL 中的 cbuffer struct 声明相匹配，并且确保两个声明之间的组件标量类型相匹配。
-   在呈现器的 C++ 代码中，在常量缓冲区声明中使用 [DirectXMath](https://docs.microsoft.com/windows/desktop/dxmath/directxmath-portal) 类型，以确保正确的数据打包。
-   有效使用常量缓冲区的最佳方法是，根据它们的更新频率将着色器变量组织成常量缓冲区。 例如，如果你有一些每个帧都进行更新的 uniform 数据，还有仅在相机移动时更新的其他 uniform 数据，则需考虑将该数据分为两个不同的常量缓冲区。
-   你忘记应用或应用错误的语义将是着色器编译 (FXC) 错误的最早来源。 一定要对它们进行仔细检查！ 本文可能有点乱，因为很多较旧的页面和示例都参考了 Direct3D 11 之前不同版本的 HLSL 语义。
-   确保你了解每个着色器的目标 Direct3D 功能级别。 功能的语义级别 9\_ \*区别于 11\_1。
-   SV\_位置坐标的值，其中 x 是 0 和呈现器目标宽度、 y 之间的关联后的内插位置数据是介于 0 和呈现器之间的语义解析目标高度，z 除以原始齐次坐标 w值 (z/w) 和 w 是 1 除以 w 的原始值 (1/w)。

## <a name="related-topics"></a>相关主题


[如何： 移植到 Direct3D 11 的简单 OpenGL ES 2.0 呈现器](port-a-simple-opengl-es-2-0-renderer-to-directx-11-1.md)

[端口的着色器对象](port-the-shader-config.md)

[顶点缓冲区和数据端口](port-the-vertex-buffers-and-data-config.md)

[在屏幕上绘制](draw-to-the-screen.md)

 

 




