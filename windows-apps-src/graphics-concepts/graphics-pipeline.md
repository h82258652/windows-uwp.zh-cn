---
title: "图形管道"
description: "Direct3D 图形管道旨在为实时游戏应用程序生成图形。 数据通过各个可配置或可编程的阶段从输入流到输出。"
ms.assetid: C9519AD0-5425-48BD-9FF4-AED8959CA4AD
keywords:
- "图形管道"
- "管道阶段"
author: PeterTurcan
ms.author: pettur
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
ms.openlocfilehash: af583deb51b93bffc05c4371466fa8412bb0476d
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="graphics-pipeline"></a>图形管道


Direct3D 图形管道旨在为实时游戏应用程序生成图形。 数据通过各个可配置或可编程的阶段从输入流到输出。

所有阶段都可以使用 Direct3D API 进行配置。 具有常见着色器核心（圆角矩形块）的阶段可通过使用 [HLSL](https://msdn.microsoft.com/library/windows/desktop/bb509561) 编程语言进行编程。 这使得管道具有非常高的灵活性和适应性。

最常用的是顶点着色器 (VS) 阶段和像素着色器 (PS) 阶段。 如果你甚至不提供这些着色器阶段，则使用默认的无操作、传递式顶点和像素着色器。

![direct3d 11 可编程管道中的数据流图示](images/d3d11-pipeline-stages.jpg)

## <a name="input-assembler-stage"></a>输入装配器阶段

 

| | |
|-|-|
|用途|[输入装配器 (IA) 阶段](input-assembler-stage--ia-.md)向管道提供基元和邻接数据，例如三角形、线和点，包括语义 ID，以减少对尚未处理基元的处理，从而提高着色器的效率。|
|输入|基元数据（三角形、线和/或点），来自内存中的用户填充缓冲区。 和可能的邻接数据。 三角形对于每个三角形将是 3 个顶点，并且对于每个三角形邻接数据可能是 3 个顶点。|
|输出|具有附加的系统生成值的基元（例如基元 ID、实例 ID 或顶点 ID）。|
| | |

<!---
<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left">Purpose</td>
<td align="left">The [Input Assembler (IA) stage](input-assembler-stage--ia-.md) supplies primitive and adjacency data to the pipeline, such as triangles, lines and points, including semantics IDs to help make shaders more efficient by reducing processing to primitives that haven't already been processed.</td>
</tr>
<tr class="even">
<td align="left">Input</td>
<td align="left"> Primitive data (triangles, lines, and/or points), from user-filled buffers in memory. And possibly adjacency data. A triangle would be 3 vertices for each triangle and possibly 3 vertices for adjacency data per triangle.  </td>
</tr>
<tr class="odd">
<td align="left">Output</td>
<td align="left">Primitives with attached system-generated values (such as a primitive ID, an instance ID, or a vertex ID). </td>
</tr>
</tbody>
</table>
 -->



## <a name="vertex-shader-stage"></a>顶点着色器阶段
 

| | |
|-|-|
|用途|[顶点着色器 (VS) 阶段](vertex-shader-stage--vs-.md)处理顶点，通常执行诸如转换、换肤以及照明之类的操作。 顶点着色器获取一个输入顶点并生成一个输出顶点。 单个每顶点操作，如转换、换肤、变形和每顶点照明。|
|输入|具有 VertexID 和 InstanceID 系统生成值的单个顶点。 每个顶点着色器输入顶点可以由 16 个 32 位矢量（每个矢量最多有 4 个分量）组成。|
|输出|单个顶点。 每个输出顶点可以由多达 16 个 32 位 4 分量矢量组成。|
| | |

<!---
<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left">Purpose</td>
<td align="left">The [Vertex Shader (VS) stage](vertex-shader-stage--vs-.md) processes vertices, typically performing operations such as transformations, skinning, and lighting. A vertex shader takes a single input vertex and produces a single output vertex. Individual per-vertex operations, such as:
<ul>
<li>Transformations</li>
<li>Skinning</li>
<li>Morphing</li>
<li>Per-vertex lighting</li>
</ul></td>
</tr>
<tr class="even">
<td align="left">Input</td>
<td align="left">A single vertex, with VertexID and InstanceID system-generated values. Each vertex shader input vertex can be comprised of up to 16 32-bit vectors (up to 4 components each).</td>
</tr>
<tr class="odd">
<td align="left">Output</td>
<td align="left">A single vertex. Each output vertex can be comprised of as many as 16 32-bit 4-component vectors.</td>
</tr>
</tbody>
</table>
-->
 

## <a name="hull-shader-stage"></a>外壳着色器阶段
 

| | |
|-|-|
|用途|[外壳着色器 (HS) 阶段](hull-shader-stage--hs-.md)是细分阶段之一，其有效地将模型的单个表面分解为许多三角形。 每个修补程序调用一次外壳着色器，它将定义低阶表面的输入控制点转换为构成修补程序的控制点。 它还执行一些按每个修补程序进行的计算，以便为细化器 (TS) 阶段和域着色器 (DS) 阶段提供数据。|
|输入|在 1 到 32 个输入控制点之间，它们共同定义低阶表面。|
|输出|在 1 到 32 个输出控制点之间，它们共同构成修补程序。 外壳着色器声明细化器 (TS) 阶段的状态，包括控制点的数量、修补程序面的类型以及在细分时使用的分区类型。|
| | |

<!---
<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left">Purpose</td>
<td align="left">The [Hull Shader (HS) stage](hull-shader-stage--hs-.md) is one of the tessellation stages, which efficiently break up a single surface of a model into many triangles. A hull shader is invoked once per patch, and it transforms input control points that define a low-order surface into control points that make up a patch. It also does some per-patch calculations to provide data for the Tessellator (TS) stage and the Domain Shader (DS) stage.</td>
</tr>
<tr class="even">
<td align="left">Input</td>
<td align="left">Between 1 and 32 input control points, which together define a low-order surface. </td>
</tr>
<tr class="odd">
<td align="left">Output</td>
<td align="left">Between 1 and 32 output control points, which together make up a patch. The hull shader declares the state for the Tessellator (TS) stage, including the number of control points, the type of patch face, and the type of partitioning to use when tessellating. </td>
</tr>
</tbody>
</table>
-->

## <a name="tessellator-stage"></a>细化器阶段
 

| | |
|-|-|
|用途|[细化器 (TS) 阶段](tessellator-stage--ts-.md)创建代表几何图形修补程序的域的采样模式，并生成连接这些样本的一组较小对象（三角形、点或线）。|
|输入|在每个使用从外壳着色器阶段传入的细化因素（指定域被细分的细微程度）和分区的类型（指定用于分割修补程序的算法）的修补程序上执行一次细化器。 |
|输出|细化器将 uv（和可选 w）坐标和表面拓扑输出到域着色器阶段。|
| | |

<!---
<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left">Purpose</td>
<td align="left">The [Tessellator (TS) stage](tessellator-stage--ts-.md) creates a sampling pattern of the domain that represents the geometry patch and generates a set of smaller objects (triangles, points, or lines) that connect these samples.   </td>
</tr>
<tr class="even">
<td align="left">Input</td>
<td align="left"> The tessellator operates once per patch using the tessellation factors (which specify how finely the domain will be tessellated) and the type of partitioning (which specifies the algorithm used to slice up a patch) that are passed in from the hull-shader stage. </td>
</tr>
<tr class="odd">
<td align="left">Output</td>
<td align="left">The tessellator outputs uv (and optionally w) coordinates and the surface topology to the domain-shader stage.     </td>
</tr>
</tbody>
</table>
 -->

## <a name="domain-shader-stage"></a>域着色器阶段
 

| | |
|-|-|
|用途|[域着色器 (DS) 阶段](domain-shader-stage--ds-.md)计算输出修补程序中细分点的顶点位置；它计算与每个域样本对应的顶点位置。 对每个细化器阶段输出点运行一次域着色器，并且该着色器具有对外壳着色器输出修补程序、输出修补程序常量以及细化器阶段输出 UV 坐标的只读权限。|
|输入|域着色器使用[外壳着色器 (HS) 阶段](hull-shader-stage--hs-.md)的输出控制点。 外壳着色器输出包括：控制点、修补程序常数数据和细化因素（例如，细化因素可以包括固定函数细化器使用的值以及原始值（例如在被整数细化舍入前），这有助于加快几何过渡）。 对[细化器 (TS) 阶段](tessellator-stage--ts-.md)的每个输出坐标调用一次域着色器。|
|输出|域着色器 (DS) 阶段输出输出修补程序中细分点的顶点位置。|
| | |

<!---
<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left">Purpose</td>
<td align="left">The [Domain Shader (DS) stage](domain-shader-stage--ds-.md) calculates the vertex position of a subdivided point in the output patch; it calculates the vertex position that corresponds to each domain sample. A domain shader is run once per tessellator stage output point and has read-only access to the hull shader output patch and output patch constants, and the tessellator stage output UV coordinates.</td>
</tr>
<tr class="even">
<td align="left">Input</td>
<td align="left"><ul>
<li>A domain shader consumes output control points from the [Hull Shader (HS) stage](hull-shader-stage--hs-.md). The hull shader outputs include:
<ul>
<li>Control points.</li>
<li>Patch constant data.</li>
<li>Tessellation factors. The tessellation factors can include the values used by the fixed-function tessellator as well as the raw values (before rounding by integer tessellation, for example), which facilitates geomorphing, for example.</li>
</ul></li>
<li>A domain shader is invoked once per output coordinate from the [Tessellator (TS) stage](tessellator-stage--ts-.md).</li>
</ul></td>
</tr>
<tr class="odd">
<td align="left">Output</td>
<td align="left">The Domain Shader (DS) stage outputs the vertex position of a subdivided point in the output patch.</td>
</tr>
</tbody>
</table>
-->
 

## <a name="geometry-shader-stage"></a>几何着色器阶段
 

| | |
|-|-|
|用途|[几何着色器 (GS) 阶段](geometry-shader-stage--gs-.md)处理整个基元：三角形、线和点，以及它们的相邻顶点。 它支持几何放大和解扩。 它对于点精灵扩展、动态粒子系统、皮毛/鳍生成、阴影卷生成、单通道渲染到 Cubemap、每基元材料交换和每基元材料设置等算法很有用 - 包括将重心坐标生成为基元数据，使得像素着色器可以执行定制属性内插。 |
|输入|与在单个顶点上操作的顶点着色器不同，几何着色器的输入是完整基元的顶点（三角形为三个顶点，线为两个顶点或点为单个顶点）。|
|输出|几何着色器 (GS) 阶段能够输出形成单个选定拓扑的多个顶点。 可用的几何着色器输出拓扑有 <strong>tristrip</strong>、<strong>linestrip</strong> 和 <strong>pointlist</strong>。 在几何着色器的任何调用中，发出的基元的数目可以自由地变化，但是必须静态地声明可发出的顶点的最大数目。 从几何着色器调用发出的带长度可以是任意的，并且可以通过 [RestartStrip](https://msdn.microsoft.com/library/windows/desktop/bb509660) HLSL 函数创建新带。|
| | |

<!---
<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left">Purpose</td>
<td align="left">The [Geometry Shader (GS) stage](geometry-shader-stage--gs-.md) processes entire primitives: triangles, lines, and points, along with their adjacent vertices. It is useful for algorithms including Point Sprite Expansion, Dynamic Particle Systems, and Shadow Volume Generation. It supports geometry amplification and de-amplification.
<ul>
<li>Point Sprite Expansion</li>
<li>Dynamic Particle Systems</li>
<li>Fur/Fin Generation</li>
<li>Shadow Volume Generation</li>
<li>Single Pass Render-to-Cubemap</li>
<li>Per-Primitive Material Swapping</li>
<li>Per-Primitive Material Setup, including generation of barycentric coordinates as primitive data so that a pixel shader can perform custom attribute interpolation.</li>
</ul></td>
</tr>
<tr class="even">
<td align="left">Input</td>
<td align="left">Unlike vertex shaders, which operate on a single vertex, the geometry shader's inputs are the vertices for a full primitive (three vertices for triangles, two vertices for lines, or single vertex for point).</td>
</tr>
<tr class="odd">
<td align="left">Output</td>
<td align="left">The Geometry Shader (GS) stage is capable of outputting multiple vertices forming a single selected topology. Available geometry shader output topologies are <strong>tristrip</strong>, <strong>linestrip</strong>, and <strong>pointlist</strong>. The number of primitives emitted can vary freely within any invocation of the geometry shader, though the maximum number of vertices that could be emitted must be declared statically. Strip lengths emitted from a geometry shader invocation can be arbitrary, and new strips can be created via the [RestartStrip](https://msdn.microsoft.com/library/windows/desktop/bb509660) HLSL function.</td>
</tr>
</tbody>
</table>
-->
 

## <a name="stream-output-stage"></a>流输出阶段
 

| | |
|-|-|
|用途|[流输出 (SO) 阶段](stream-output-stage--so-.md)连续地将顶点数据从前一活动阶段输出（或流入）到内存中的一个或多个缓冲区。 流出到内存的数据可以作为输入数据再次循环回到管道，或者从 CPU 读回。|
|输入|来自前一管道阶段的顶点数据。|
|输出|流输出 (SO) 阶段连续地将来自前一活动阶段的顶点数据（例如几何着色器 (GS) 阶段）输出（或流入）到内存中的一个或多个缓冲区。 如果几何着色器 (GS) 阶段是非活动的，并且流输出 (SO) 阶段是活动的，它会连续地将来自域着色器 (DS) 阶段的顶点数据输出到内存中的缓冲区（或者如果 DS 也是非活动的，它将使用来自顶点着色器 (VS) 阶段的数据）。|
| | |

<!---
<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left">Purpose</td>
<td align="left">The [Stream Output (SO) stage](stream-output-stage--so-.md) continuously outputs (or streams) vertex data from the previous active stage to one or more buffers in memory. Data streamed out to memory can be recirculated back into the pipeline as input data, or read-back from the CPU.</td>
</tr>
<tr class="even">
<td align="left">Input</td>
<td align="left">Vertex data from a previous pipeline stage.   </td>
</tr>
<tr class="odd">
<td align="left">Output</td>
<td align="left">The Stream Output (SO) stage continuously outputs (or streams) vertex data from the previous active stage, such as the Geometry Shader (GS) stage, to one or more buffers in memory. If the Geometry Shader (GS) stage is inactive, and the Stream Output (SO) stage is active, it continuously outputs vertex data from the Domain Shader (DS) stage to buffers in memory (or if the DS is also inactive, from the Vertex Shader (VS) stage).</td>
</tr>
</tbody>
</table>
-->

## <a name="rasterizer-stage"></a>光栅器阶段
 

| | |
|-|-|
|用途|[光栅器 (RS) 阶段](rasterizer-stage--rs-.md)剪辑未在视图中的基元，为像素着色器 (PS) 阶段准备基元并确定如何调用像素着色器。 将矢量信息（由形状或基元构成）转换为光栅图像（由像素构成）以便显示实时 3D 图形。|
|输入|假定进入光栅器阶段的顶点 (x,y,z,w) 将处于齐性裁剪空间。 在此坐标空间里，X 轴指向右，Y 轴指向上，Z 轴指向与摄像机相反的方向。|
|输出|需要渲染的实际像素。 包括一些用于由像素着色器进行内插的顶点属性。|
| | |

<!---
<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left">Purpose</td>
<td align="left">The [Rasterizer (RS) stage](rasterizer-stage--rs-.md) clips primitives that aren't in view, prepares primitives for the Pixel Shader (PS) stage, and determines how to invoke pixel shaders. Converts vector information (composed of shapes or primitives) into a raster image (composed of pixels) for the purpose of displaying real-time 3D graphics.</td>
</tr>
<tr class="even">
<td align="left">Input</td>
<td align="left">Vertices (x,y,z,w) coming into the Rasterizer stage are assumed to be in homogeneous clip-space. In this coordinate space the X axis points right, Y points up and Z points away from camera.</td>
</tr>
<tr class="odd">
<td align="left">Output</td>
<td align="left">The actual pixels that need to be rendered. Includes some vertex attributes for use in interpolation by the pixel Shader.</td>
</tr>
</tbody>
</table>
 -->

## <a name="pixel-shader-stage"></a>像素着色器阶段
 

| | |
|-|-|
|用途|[像素着色器 (PS) 阶段](pixel-shader-stage--ps-.md)接收基元的插值数据并生成每像素数据，如颜色。|
|输入|如果管道在配置时未使用几何着色器，像素着色器限制为 16 个 32 位 4 分量输入。 否则，像素着色器可能占用多达 32 个 32 位 4 分量输入。 像素着色器输入数据包含顶点属性（可以在使用或不使用透视校正的情况下内插），或者也可以被视为每基元常量。 像素着色器输入根据声明的内插模式从正在光栅化的基元的顶点属性内插。 如果基元在光栅化之前被剪裁，内插模式在剪裁过程中也受支持。 |
|输出|像素着色器可输出多达 8 个 32 位 4 分量颜色，如果像素被弃用，则不会生成颜色。 像素着色器输出寄存器分量必须先声明才能使用；允许每个寄存器使用一个独特的输出写入掩码。|
| | |

<!---
<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left">Purpose</td>
<td align="left">The [Pixel Shader (PS) stage](pixel-shader-stage--ps-.md) receives interpolated data for a primitive and generates per-pixel data such as color.</td>
</tr>
<tr class="even">
<td align="left">Input</td>
<td align="left"><p>When the pipeline is configured without a geometry shader, a pixel shader is limited to 16, 32-bit, 4-component inputs. Otherwise, a pixel shader can take up to 32, 32-bit, 4-component inputs.</p>
<p>Pixel shader input data includes vertex attributes (that can be interpolated with or without perspective correction) or can be treated as per-primitive constants. Pixel shader inputs are interpolated from the vertex attributes of the primitive being rasterized, based on the interpolation mode declared. If a primitive gets clipped before rasterization, the interpolation mode is honored during the clipping process as well.</p></td>
</tr>
<tr class="odd">
<td align="left">Output</td>
<td align="left"><p>A pixel shader can output up to 8, 32-bit, 4-component colors, or no color if the pixel is discarded. Pixel shader output register components must be declared before they can be used; each register is allowed a distinct output-write mask.</p></td>
</tr>
</tbody>
</table>
-->
 

## <a name="output-merger-stage"></a>输出合并阶段
 

| | |
|-|-|
|用途|[输出合并 (OM) 阶段](output-merger-stage--om-.md)将各种类型的输出数据（像素着色器值、深度和模具信息）与渲染目标的内容以及深度/模具缓冲区组合在一起，以生成最终的管道结果。|
|输入|输出合并输入是管道状态、像素着色器生成的像素数据、渲染目标的内容和深度/模具缓冲区的内容。|
|输出|最终渲染的像素颜色。|
| | |


<!---
<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left">Purpose</td>
<td align="left">The [Output Merger (OM) stage](output-merger-stage--om-.md) combines various types of output data (pixel shader values, depth and stencil information) with the contents of the render target and depth/stencil buffers to generate the final pipeline result.</td>
</tr>
<tr class="even">
<td align="left">Input</td>
<td align="left"><ul>
<li>Pipeline state</li>
<li>The pixel data generated by the pixel shaders</li>
<li>The contents of the render targets</li>
<li>The contents of the depth/stencil buffers.</li>
</ul></td>
</tr>
<tr class="odd">
<td align="left">Output</td>
<td align="left">The final rendered pixel color.</td>
</tr>
</tbody>
</table>


| | |
|-|-|
|Purpose|xxxx|
|Input|yyyy|
|Output|zzzz|
| | |
-->

## <a name="related-topics"></a>相关主题


[Direct3D 图形学习指南](index.md)

[计算管道](compute-pipeline.md)

 

 
