---
author: mtoepke
title: GLSL 到 HLSL 参考
description: 将图形体系结构从 OpenGL ES 2.0 移植到 Direct3D 11 以便创建适用于通用 Windows 平台 (UWP) 的游戏时，需要将 OpenGL 着色器语言 (GLSL) 代码移植到 Microsoft 高级着色器语言 (HLSL) 代码。
ms.assetid: 979d19f6-ef0c-64e4-89c2-a31e1c7b7692
ms.author: mtoepke
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, glsl, hlsl, opengl, directx, 着色器
ms.localizationpriority: medium
ms.openlocfilehash: 30c925f9ebb07d578147dfba373fdeb3baa364fe
ms.sourcegitcommit: 753e0a7160a88830d9908b446ef0907cc71c64e7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/29/2018
ms.locfileid: "5758002"
---
# <a name="glsl-to-hlsl-reference"></a>GLSL 到 HLSL 参考



[将图形体系结构从 OpenGL ES 2.0 移植到 Direct3D 11](port-from-opengl-es-2-0-to-directx-11-1.md) 以便为通用 Windows 平台 (UWP) 创建游戏时，需要将 OpenGL 着色器语言 (GLSL) 代码移植到 Microsoft 高级着色器语言 (HLSL) 代码。 此处所谈到的 GLSL 兼容 OpenGL ES 2.0；HLSL 兼容 Direct3D 11。 有关 Direct3D 11 和之前版本的 Direct3D 之间差别的信息，请参阅[功能映射](feature-mapping.md)。

-   [比较 OpenGL ES 2.0 与 Direct3D 11](#comparing-opengl-es-20-with-direct3d-11)
-   [将 GLSL 变量移植到 HLSL](#porting-glsl-variables-to-hlsl)
-   [将 GLSL 类型移植到 HLSL](#porting-glsl-types-to-hlsl)
-   [将 GLSL 预定义的全局变量移植到 HLSL](#porting-glsl-pre-defined-global-variables-to-hlsl)
-   [将 GLSL 变量移植到 HLSL 的示例](#examples-of-porting-glsl-variables-to-hlsl)
    -   [GLSL 中的 uniform、attribute 和 varying](#uniform-attribute-and-varying-in-glsl)
    -   [HLSL 中的常量缓冲区和数据传输](#constant-buffers-and-data-transfers-in-hlsl)
-   [将 OpenGL 呈现代码移植到 Direct3D 的示例](#examples-of-porting-opengl-rendering-code-to-direct3d)
-   [相关主题](#related-topics)

## <a name="comparing-opengl-es-20-with-direct3d-11"></a>比较 OpenGL ES 2.0 与 Direct3D 11


OpenGL ES 2.0 和 Direct3D 11 有很多相似之处。 它们都有相似的呈现管道和图形功能。 但是 Direct3D 11 是呈现实现和 API，而不是规范；OpenGL ES 2.0 是呈现规范和 API，而不是实现。 Direct3D 11 和 OpenGL ES 2.0 通常会在以下方面有所不同：

| OpenGL ES 2.0                                                                                         | Direct3D 11                                                                                                            |
|-------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------|
| 硬件和操作系统规范与供应商提供的实现无关             | 在 Windows 平台上硬件抽象和认证的 Microsoft 实现                                |
| 对硬件多样性进行了抽象，运行时管理大多数资源                                     | 直接访问硬件布局；应用可以管理资源和处理                                              |
| 通过第三方库（例如，Simple DirectMedia Layer (SDL)）提供更高级的模块 | 更高级的模块（如 Direct2D）构建在低级模块上，以简化 Windows 应用的开发             |
| 通过扩展名来区分硬件供应商                                                         | Microsoft 采用常规方法向 API 中添加可选功能，以便这些功能不会特定于任何特定的硬件供应商 |

 

GLSL 和 HLSL 通常会在以下方面有所不同：

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">GLSL</th>
<th align="left">HLSL</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left">注重过程，以步骤为中心（如 C）</td>
<td align="left">面向对象，以数据为中心（如 C++）</td>
</tr>
<tr class="even">
<td align="left">着色器编译被集成到了图形 API 中</td>
<td align="left">HLSL 编译器<a href="https://msdn.microsoft.com/library/windows/desktop/bb509633">将着色器编译为</a>中间二进制表示，然后 Direct3D 将其传递给驱动程序。
<div class="alert">
<strong>注意</strong>此二进制表示与硬件无关。 通常在应用生成时对其进行编译，而不是在应用运行时编译。
</div>
<div>
 
</div></td>
</tr>
<tr class="odd">
<td align="left"><a href="#porting-glsl-variables-to-hlsl">可变</a>存储修饰符</td>
<td align="left">通过输入布局声明进行常量缓冲区和数据传输</td>
</tr>
<tr class="even">
<td align="left"><p><a href="#porting-glsl-types-to-hlsl">类型</a></p>
<p>典型的矢量类型：vec2/3/4</p>
<p>lowp、mediump、highp</p></td>
<td align="left"><p>典型的矢量类型：float2/3/4</p>
<p>min10float、min16float</p></td>
</tr>
<tr class="odd">
<td align="left">texture2D [Function]</td>
<td align="left"><a href="https://msdn.microsoft.com/library/windows/desktop/bb509695">texture.Sample</a> [datatype.Function]</td>
</tr>
<tr class="even">
<td align="left">sampler2D [datatype]</td>
<td align="left"><a href="https://msdn.microsoft.com/library/windows/desktop/ff471525">Texture2D</a> [datatype]</td>
</tr>
<tr class="odd">
<td align="left">行主序矩阵（默认设置）</td>
<td align="left">列主序矩阵（默认设置）
<div class="alert">
<strong>注意</strong>使用<strong>row_major</strong>类型修饰符来更改一个变量的布局。 有关详细信息，请参阅<a href="https://msdn.microsoft.com/library/windows/desktop/bb509706">变量语法</a>。 还可以指定编译器标志或 pragma 来更改全局默认设置。
</div>
<div>
 
</div></td>
</tr>
<tr class="even">
<td align="left">片段着色器</td>
<td align="left">像素着色器</td>
</tr>
</tbody>
</table>

 

> **注意**HLSL 让纹理和采样器作为两个不同的对象。 在 GLSL（如 Direct3D 9）中，纹理绑定是采样器状态的一部分。

 

在 GLSL 中，将大多数 OpenGL 状态呈现为预定义的全局变量。 例如，使用 GLSL，**gl\_Position** 变量可用于指定顶点位置，而 **gl\_FragColor** 变量可用于指定片段颜色。 在 HLSL 中，将 Direct3D 状态从应用代码显式传递到着色器。 例如，对于 Direct3D 和 HLSL，顶点着色器的输入必须与顶点缓冲区中的数据格式相匹配，并且应用代码中常量缓冲区的结构必须与着色器代码中常量缓冲区 ([cbuffer](https://msdn.microsoft.com/library/windows/desktop/bb509581)) 的结构相匹配。

## <a name="porting-glsl-variables-to-hlsl"></a>将 GLSL 变量移植到 HLSL


在 GLSL 中，将修饰符（限定符）应用于全局着色器变量声明，以为该变量提供一个你的着色器中的特定行为。 在 HLSL 中，不需要这些修饰符，因为你使用传递给着色器的参数以及从着色器返回的参数定义了着色器流。

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">GLSL 变量行为</th>
<th align="left">HLSL 等效内容</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p><strong>uniform</strong></p>
<p>将 uniform 变量从应用代码传递到顶点着色器和分段着色器或传递到两者。 必须在使用这些着色器绘制任何三角形之前，设置所有 uniform 的值，以便它们的值在绘制三角形网格的整个过程中保持不变。 这些值都是 uniform。 一些 uniform 是针对整个帧设置的，另一些 uniform 唯一对应于一个特定的顶点像素着色器对。</p>
<p>uniform 变量是每个多边形的变量。</p></td>
<td align="left"><p>使用常量缓冲区。</p>
<p>请参阅<a href="https://msdn.microsoft.com/library/windows/desktop/ff476896">如何：创建常量缓冲区</a>和<a href="https://msdn.microsoft.com/library/windows/desktop/bb509581">着色器常量</a>。</p></td>
</tr>
<tr class="even">
<td align="left"><p><strong>varying</strong></p>
<p>在顶点着色器内初始化一个 varying 变量，并将其传递到片段着色器中具有相同名称的 varying 变量。 由于顶点着色器仅设置每个顶点上的 varying 变量的值，因此光栅器会插入这些值（采用透视校正的方式），以生成每个要传递到片段着色器中的片段值。 这些变量在各个三角形之间有所不同。</p></td>
<td align="left">使用从顶点着色器返回的结构作为像素着色器的输入。 确保语义值相匹配。</td>
</tr>
<tr class="odd">
<td align="left"><p><strong>属性</strong></p>
<p>attribute 只是你从应用代码传递到顶点着色器的顶点描述的一部分。 与 uniform 不同，你为每个顶点设置每个 attribute 的值，但却允许每个顶点拥有不同的值。 attribute 变量是每个顶点的变量。</p></td>
<td align="left"><p>在 Direct3D 应用代码中定义顶点缓冲区并将其与顶点着色器中定义的顶点输入相匹配。 也可以定义索引缓冲区。 请参阅<a href="https://msdn.microsoft.com/library/windows/desktop/ff476899">如何：创建顶点缓冲区</a>和<a href="https://msdn.microsoft.com/library/windows/desktop/ff476897">如何：创建索引缓冲区</a>。</p>
<p>在 Direct3D 应用代码中创建输入布局并将语义值与顶点输入中的值相匹配。 请参阅<a href="https://msdn.microsoft.com/library/windows/desktop/bb205117#Create_the_Input_Layout">创建输入布局</a>。</p></td>
</tr>
<tr class="even">
<td align="left"><p><strong>const</strong></p>
<p>常量编译到着色器中，从不更改。</p></td>
<td align="left">使用 <strong>static const</strong>。 <strong>static</strong> 表示未向常量缓冲区显示该值，<strong>const</strong> 表示着色器无法更改该值。 因此，在编译时我们根据它的初始值来了解该值。</td>
</tr>
</tbody>
</table>

 

在 GLSL 中，没有修饰符的变量只是普通的全局变量，它们是每个着色器的私有变量。

当将数据传递到纹理（在 HLSL 中为 [Texture2D](https://msdn.microsoft.com/library/windows/desktop/ff471525)）及其关联的采样器 （在 HLSL 中为 [SamplerState](https://msdn.microsoft.com/library/windows/desktop/bb509644)）时，通常会在像素着色器中将它们声明为全局变量。

## <a name="porting-glsl-types-to-hlsl"></a>将 GLSL 类型移植到 HLSL


使用该表将 GLSL 类型移植到 HLSL。

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">GLSL 类型</th>
<th align="left">HLSL 类型</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left">标量类型：float、int、bool</td>
<td align="left"><p>标量类型：float、int、bool</p>
<p>also、uint、double</p>
<p>有关详细信息，请参阅<a href="https://msdn.microsoft.com/library/windows/desktop/bb509646">标量类型</a>。</p></td>
</tr>
<tr class="even">
<td align="left"><p>矢量类型</p>
<ul>
<li>浮点矢量：vec2、vec3、vec4</li>
<li>布尔矢量：bvec2、bvec3、bvec4</li>
<li>有符号整数矢量：ivec2、ivec3、ivec4</li>
</ul></td>
<td align="left"><p>矢量类型</p>
<ul>
<li>float2、float3、float4 和 float1</li>
<li>bool2、bool3、bool4 和 bool1</li>
<li>int2、int3、int4 和 int1</li>
<li><p>这些类型也都有类似于 float、bool 和 int 的矢量扩展：</p>
<ul>
<li>uint</li>
<li>min10float、min16float</li>
<li>min12int、min16int</li>
<li>min16uint</li>
</ul></li>
</ul>
<p>有关详细信息，请参阅<a href="https://msdn.microsoft.com/library/windows/desktop/bb509707">矢量类型</a>和<a href="https://msdn.microsoft.com/library/windows/desktop/bb509568">关键字</a>。</p>
<p>vector 是定义为 float4 的 also 类型 (typedef vector &lt;float, 4&gt; vector;)。 有关详细信息，请参阅<a href="https://msdn.microsoft.com/library/windows/desktop/bb509702">用户定义的类型</a>。</p></td>
</tr>
<tr class="odd">
<td align="left"><p>矩阵类型</p>
<ul>
<li>mat2: 2x2 浮点矩阵</li>
<li>mat3: 3x3 浮点矩阵</li>
<li>mat4: 4x4 浮点矩阵</li>
</ul></td>
<td align="left"><p>矩阵类型</p>
<ul>
<li>float2x2</li>
<li>float3x3</li>
<li>float4x4</li>
<li>also、float1x1、float1x2、float1x3、float1x4、float2x1、float2x3、float2x4、float3x1、float3x2、float3x4、float4x1、float4x2、float4x3</li>
<li><p>这些类型也都有类似于 float 的矩阵扩展：</p>
<ul>
<li>int、uint、bool</li>
<li>min10float、min16float</li>
<li>min12int、min16int</li>
<li>min16uint</li>
</ul></li>
</ul>
<p>也可以使用<a href="https://msdn.microsoft.com/library/windows/desktop/bb509623">矩阵类型</a>来定义矩阵。</p>
<p>例如：matrix &lt;float, 2, 2&gt; fMatrix = {0.0f, 0.1, 2.1f, 2.2f};</p>
<p>matrix 是定义为 float4x4 的 also 类型 (typedef matrix &lt;float, 4, 4&gt; matrix;)。 有关详细信息，请参阅<a href="https://msdn.microsoft.com/library/windows/desktop/bb509702">用户定义的类型</a>。</p></td>
</tr>
<tr class="even">
<td align="left"><p>float、int、sampler 的精度限定符</p>
<ul>
<li><p>highp</p>
<p>该限定符提供最低精度要求，该要求大于 min16float 提供的要求，但小于完整的 32 位浮点。 HLSL 中的等效内容为：</p>
<p>highp float -&gt; float</p>
<p>highp int -&gt; int</p></li>
<li><p>mediump</p>
<p>该限定符应用于 float 和 int，它等效于 HLSL 中的 min16float 和 min12int。 最低 10 位尾数，与 min10float 不同。</p></li>
<li><p>lowp</p>
<p>该限定符应用于 float，它提供的浮点范围为 -2 到 2。 等效于 HLSL 中的 min10float。</p></li>
</ul></td>
<td align="left"><p>精度类型</p>
<ul>
<li>min16float：最低 16 位浮点值</li>
<li><p>min10float</p>
<p>最低固定点有符号 2.8 位值（2 位整数和 8 位小数部分）。 8 位小数部分可以包括 1，而非排除 1，目的是为它提供完整的包含范围 -2 到 2。</p></li>
<li>min16int：最低 16 位有符号整数</li>
<li><p>min12int：最低 12 位有符号整数</p>
<p>该类型用于 10Level9（<a href="https://msdn.microsoft.com/library/windows/desktop/ff476876">9_x 功能级别</a>），其中整数由浮点数来表示。 这是在使用 16 位浮点数模拟整数时可以获得的精度。</p></li>
<li>min16uint：最低 16 位无符号整数</li>
</ul>
<p>有关详细信息，请参阅<a href="https://msdn.microsoft.com/library/windows/desktop/bb509646">标量类型</a>和<a href="https://msdn.microsoft.com/library/windows/desktop/hh968108">使用 HLSL 最低精度</a>。</p></td>
</tr>
<tr class="odd">
<td align="left">sampler2D</td>
<td align="left"><a href="https://msdn.microsoft.com/library/windows/desktop/ff471525">Texture2D</a></td>
</tr>
<tr class="even">
<td align="left">samplerCube</td>
<td align="left"><a href="https://msdn.microsoft.com/library/windows/desktop/bb509700">TextureCube</a></td>
</tr>
</tbody>
</table>

 

## <a name="porting-glsl-pre-defined-global-variables-to-hlsl"></a>将 GLSL 预定义的全局变量移植到 HLSL


使用该表将 GLSL 预定义的全局变量移植到 HLSL。

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">GLSL 预定义的全局变量</th>
<th align="left">HLSL 语义</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p><strong>gl_Position</strong></p>
<p>该变量为 <strong>vec4</strong> 类型。</p>
<p>顶点位置</p>
<p>例如 - gl_Position = position;</p></td>
<td align="left"><p>SV_Position</p>
<p>在 Direct3D 9 中为 POSITION</p>
<p>该语义为 <strong>float4</strong> 类型。</p>
<p>顶点着色器输出</p>
<p>顶点位置</p>
<p>例如 - float4 vPosition : SV_Position;</p></td>
</tr>
<tr class="even">
<td align="left"><p><strong>gl_PointSize</strong></p>
<p>该变量为 <strong>float</strong> 类型。</p>
<p>点大小</p></td>
<td align="left"><p>PSIZE</p>
<p>除非你的目标是 Direct3D 9，否则没有意义</p>
<p>该语义为 <strong>float</strong> 类型。</p>
<p>顶点着色器输出</p>
<p>点大小</p></td>
</tr>
<tr class="odd">
<td align="left"><p><strong>gl_FragColor</strong></p>
<p>该变量为 <strong>vec4</strong> 类型。</p>
<p>片段颜色</p>
<p>例如 - gl_FragColor = vec4(colorVarying, 1.0);</p></td>
<td align="left"><p>SV_Target</p>
<p>在 Direct3D 9 中为 COLOR</p>
<p>该语义为 <strong>float4</strong> 类型。</p>
<p>像素着色器输出</p>
<p>像素颜色</p>
<p>例如 - float4 Color[4] : SV_Target;</p></td>
</tr>
<tr class="even">
<td align="left"><p><strong>gl_FragData[n]</strong></p>
<p>该变量为 <strong>vec4</strong> 类型。</p>
<p>颜色附件 n 的片段颜色</p></td>
<td align="left"><p>SV_Target[n]</p>
<p>该语义为 <strong>float4</strong> 类型。</p>
<p>n 呈现目标中存储的像素着色器输出值，其中 0 &lt;= n &lt;= 7。</p></td>
</tr>
<tr class="odd">
<td align="left"><p><strong>gl_FragCoord</strong></p>
<p>该变量为 <strong>vec4</strong> 类型。</p>
<p>帧缓冲区中的片段位置</p></td>
<td align="left"><p>SV_Position</p>
<p>在 Direct3D 9 中不可用</p>
<p>该语义为 <strong>float4</strong> 类型。</p>
<p>像素着色器输入</p>
<p>屏幕空间坐标</p>
<p>例如 - float4 screenSpace : SV_Position</p></td>
</tr>
<tr class="even">
<td align="left"><p><strong>gl_FrontFacing</strong></p>
<p>该变量为 <strong>bool</strong> 类型。</p>
<p>确定片段是否属于正面基元。</p></td>
<td align="left"><p>SV_IsFrontFace</p>
<p>在 Direct3D 9 中为 VFACE</p>
<p>SV_IsFrontFace 为 <strong>bool</strong> 类型。</p>
<p>VFACE 为 <strong>float</strong> 类型。</p>
<p>像素着色器输入</p>
<p>基元朝向</p></td>
</tr>
<tr class="odd">
<td align="left"><p><strong>gl_PointCoord</strong></p>
<p>该变量为 <strong>vec2</strong> 类型。</p>
<p>某个点中的片段位置（仅限点光栅化）</p></td>
<td align="left"><p>SV_Position</p>
<p>在 Direct3D 9 中为 VPOS</p>
<p>SV_Position 为 <strong>float4</strong> 类型。</p>
<p>VPOS 为 <strong>float2</strong> 类型。</p>
<p>像素着色器输入</p>
<p>屏幕空间中的像素或示例位置</p>
<p>例如 - float4 pos : SV_Position</p></td>
</tr>
<tr class="even">
<td align="left"><p><strong>gl_FragDepth</strong></p>
<p>该变量为 <strong>float</strong> 类型。</p>
<p>深度缓冲区数据</p></td>
<td align="left"><p>SV_Depth</p>
<p>在 Direct3D 9 中为 DEPTH</p>
<p>SV_Depth 为 <strong>float</strong> 类型。</p>
<p>像素着色器输出</p>
<p>深度缓冲区数据</p></td>
</tr>
</tbody>
</table>

 

使用语义指定位置、颜色等作为顶点着色器输入和像素着色器输入。 必须将输入布局中的语义值与顶点着色器输入相匹配。 例如，请参阅[将 GLSL 变量移植到 HLSL 的示例](#examples-of-porting-glsl-variables-to-hlsl)。 有关 HLSL 语义的详细信息，请参阅[语义](https://msdn.microsoft.com/library/windows/desktop/bb509647)。

## <a name="examples-of-porting-glsl-variables-to-hlsl"></a>将 GLSL 变量移植到 HLSL 的示例


下面我们介绍在 OpenGL/GLSL 代码中使用 GLSL 变量的示例，然后介绍在 Direct3D/HLSL 代码中的等效示例。

### <a name="uniform-attribute-and-varying-in-glsl"></a>GLSL 中的 uniform、attribute 和 varying

OpenGL 应用代码

``` syntax
// Uniform values can be set in app code and then processed in the shader code.
uniform mat4 model;
uniform mat4 view;
uniform mat4 projection;

// Incoming position of vertex
attribute vec4 position;
 
// Incoming color for the vertex
attribute vec3 color;
 
// The varying variable tells the shader pipeline to pass it  
// on to the fragment shader.
varying vec3 colorVarying;
```

GLSL 顶点着色器代码

``` syntax
//The shader entry point is the main method.
void main()
{
colorVarying = color; //Use the varying variable to pass the color to the fragment shader
gl_Position = position; //Copy the position to the gl_Position pre-defined global variable
}
```

GLSL 片段着色器代码

``` syntax
void main()
{
//Pad the colorVarying vec3 with a 1.0 for alpha to create a vec4 color
//and assign that color to the gl_FragColor pre-defined global variable
//This color then becomes the fragment's color.
gl_FragColor = vec4(colorVarying, 1.0);
}
```

### <a name="constant-buffers-and-data-transfers-in-hlsl"></a>HLSL 中的常量缓冲区和数据传输

下面是如何将数据传递到 HLSL 顶点着色器，然后流动到像素着色器的示例。 在你的应用代码中，定义一个顶点缓冲区和一个常量缓冲区。 然后，在你的顶点着色器代码中，将常量缓冲区定义为 [cbuffer](https://msdn.microsoft.com/library/windows/desktop/bb509581) 并存储每个顶点数据以及像素着色器输入数据。 这里我们使用名为 **VertexShaderInput** 和 **PixelShaderInput** 的结构。

Direct3D 应用代码

```cpp
struct ConstantBuffer
{
    XMFLOAT4X4 model;
    XMFLOAT4X4 view;
    XMFLOAT4X4 projection;
};
struct SimpleCubeVertex
{
    XMFLOAT3 pos;   // position
    XMFLOAT3 color; // color
};

 // Create an input layout that matches the layout defined in the vertex shader code.
 const D3D11_INPUT_ELEMENT_DESC basicVertexLayoutDesc[] =
 {
     { "POSITION", 0, DXGI_FORMAT_R32G32B32_FLOAT, 0,  0, D3D11_INPUT_PER_VERTEX_DATA, 0 },
     { "COLOR",    0, DXGI_FORMAT_R32G32B32_FLOAT, 0, 12, D3D11_INPUT_PER_VERTEX_DATA, 0 },
 };

// Create vertex and index buffers that define a geometry.
```

HLSL 顶点着色器代码

``` syntax
cbuffer ModelViewProjectionCB : register( b0 )
{
    matrix model; 
    matrix view;
    matrix projection;
};
// The POSITION and COLOR semantics must match the semantics in the input layout Direct3D app code.
struct VertexShaderInput
{
    float3 pos : POSITION; // Incoming position of vertex 
    float3 color : COLOR; // Incoming color for the vertex
};

struct PixelShaderInput
{
    float4 pos : SV_Position; // Copy the vertex position.
    float4 color : COLOR; // Pass the color to the pixel shader.
};

PixelShaderInput main(VertexShaderInput input)
{
    PixelShaderInput vertexShaderOutput;

    // shader source code

    return vertexShaderOutput;
}
```

HLSL 像素着色器代码

``` syntax
// Collect input from the vertex shader. 
// The COLOR semantic must match the semantic in the vertex shader code.
struct PixelShaderInput
{
    float4 pos : SV_Position;
    float4 color : COLOR; // Color for the pixel
};

// Set the pixel color value for the renter target. 
float4 main(PixelShaderInput input) : SV_Target
{
    return input.color;
}
```

## <a name="examples-of-porting-opengl-rendering-code-to-direct3d"></a>将 OpenGL 呈现代码移植到 Direct3D 的示例


下面我们介绍一个在 OpenGL ES 2.0 代码中进行呈现的示例，然后介绍在 Direct3D 11 代码中的等效示例。

OpenGL 呈现代码

``` syntax
// Bind shaders to the pipeline. 
// Both vertex shader and fragment shader are in a program.
glUseProgram(m_shader->getProgram());
 
// Input asssembly 
// Get the position and color attributes of the vertex.

m_positionLocation = glGetAttribLocation(m_shader->getProgram(), "position");
glEnableVertexAttribArray(m_positionLocation);

m_colorLocation = glGetAttribColor(m_shader->getProgram(), "color");
glEnableVertexAttribArray(m_colorLocation);
 
// Bind the vertex buffer object to the input assembler.
glBindBuffer(GL_ARRAY_BUFFER, m_geometryBuffer);
glVertexAttribPointer(m_positionLocation, 4, GL_FLOAT, GL_FALSE, 0, NULL);
glBindBuffer(GL_ARRAY_BUFFER, m_colorBuffer);
glVertexAttribPointer(m_colorLocation, 3, GL_FLOAT, GL_FALSE, 0, NULL);
 
// Draw a triangle with 3 vertices.
glDrawArray(GL_TRIANGLES, 0, 3);
```

Direct3D 呈现代码

```cpp
// Bind the vertex shader and pixel shader to the pipeline.
m_d3dDeviceContext->VSSetShader(vertexShader.Get(),nullptr,0);
m_d3dDeviceContext->PSSetShader(pixelShader.Get(),nullptr,0);
 
// Declare the inputs that the shaders expect.
m_d3dDeviceContext->IASetInputLayout(inputLayout.Get());
m_d3dDeviceContext->IASetVertexBuffers(0, 1, vertexBuffer.GetAddressOf(), &stride, &offset);

// Set the primitive's topology.
m_d3dDeviceContext->IASetPrimitiveTopology(D3D11_PRIMITIVE_TOPOLOGY_TRIANGLELIST);

// Draw a triangle with 3 vertices. triangleVertices is an array of 3 vertices.
m_d3dDeviceContext->Draw(ARRAYSIZE(triangleVertices),0);
```

## <a name="related-topics"></a>相关主题


* [从 OpenGL ES 2.0 移植到 Direct3D 11](port-from-opengl-es-2-0-to-directx-11-1.md)

 

 




