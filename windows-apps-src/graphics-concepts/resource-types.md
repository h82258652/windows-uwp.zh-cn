---
title: 资源类型
description: 不同类型的资源具有不同的布局（或内存占用量）。
ms.assetid: BCDDF227-1837-44DA-ABD4-E39BCFF2B8EF
keywords:
- 资源类型
author: michaelfromredmond
ms.author: mithom
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 3c23cc07c84e9a77b36c812c6f273f518e98838e
ms.sourcegitcommit: 144f5f127fc4fbd852f2f6780ef26054192d68fc
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/03/2018
ms.locfileid: "5992051"
---
# <a name="resource-types"></a>资源类型


不同类型的资源具有不同的布局（或内存占用）。 Direct3D 使用的所有资源派生自两种基础资源类型：[缓冲区](#buffer-resources)和[纹理](#texture-resources)。 缓冲区是原始数据（元素）的集合；纹理是纹素（纹理元素）的集合。

可通过两种方式完全指定资源的布局（或内存占用量）：

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">项目</th>
<th align="left">描述</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p><span id="Typed"></span><span id="typed"></span><span id="TYPED"></span>类型化</p></td>
<td align="left"><p>在创建资源时完全指定类型。</p></td>
</tr>
<tr class="even">
<td align="left"><p><span id="Typeless"></span><span id="typeless"></span><span id="TYPELESS"></span>无类型</p></td>
<td align="left"><p>在资源绑定到管道时完全指定类型。</p></td>
</tr>
</tbody>
</table>

 

## <a name="span-idbufferresourcesspanspan-idbufferresourcesspanspan-idbufferresourcesspanspan-idbuffer-resourcesspanbuffer-resources"></a><span id="Buffer_Resources"></span><span id="buffer_resources"></span><span id="BUFFER_RESOURCES"></span><span id="buffer-resources"></span>缓冲区资源


缓冲区资源是完全类型化数据的集合；在内部，缓冲区包含多个元素。 一个元素由 1 到 4 个组件构成。 元素数据类型的示例包括：一个紧缩数据值（如 R8G8B8A8）、一个 8 位整数、4 个 32 位浮点值。 这些数据类型用于存储数据（例如，位置矢量、标准矢量、顶点缓冲区中的纹理坐标、索引缓冲区中的索引）或设备状态。

缓冲区是作为非结构化资源创建的。 由于缓冲区是非结构化的，因此它无法包含任何 mipmap 级别、在读取时不会筛选且无法进行多重采样。

### <a name="span-idbuffertypesspanspan-idbuffertypesspanspan-idbuffertypesspanbuffer-types"></a><span id="Buffer_Types"></span><span id="buffer_types"></span><span id="BUFFER_TYPES"></span>缓冲区类型

-   [顶点缓冲区](#vertex-buffer)
-   [索引缓冲区](#index-buffer)
-   [常量缓冲区](#shader-constant-buffer)

### <a name="span-idvertexbufferspanspan-idvertexbufferspanspan-idvertexbufferspanspan-idvertex-bufferspanvertex-buffer"></a><span id="Vertex_Buffer"></span><span id="vertex_buffer"></span><span id="VERTEX_BUFFER"></span><span id="vertex-buffer"></span>顶点缓冲区

缓冲区是元素的集合；顶点缓冲区包含每顶点数据。 最简单的示例是包含一种类型的数据（如位置数据）的顶点缓冲区。 它可进行可视化，如下图所示。

![包含位置数据的顶点缓冲区的图示](images/d3d10-resources-single-element-vb2.png)

更常见的情况是，顶点缓冲区包含完全指定 3D 顶点所需的所有数据。 示例可能是包含每顶点位置、法线和纹理坐标的顶点缓冲区。 此数据通常组织为每顶点元素集，如下图所示。

![包含位置、法线和纹理数据的顶点缓冲区的图示](images/d3d10-vertex-buffer-element.png)

此顶点缓冲区包含 8 个顶点的每顶点数据；每个顶点存储 3 个元素（位置、法线和纹理坐标）。 通常，使用 3 个 32 位浮点指定每个位置和法线，使用两个 32 位浮点指定纹理坐标。

要访问顶点缓冲区中的数据，你需要知道要访问的顶点以及其他缓冲区参数：

-   *Offset* - 缓冲区起点到第一个顶点数据之间的字节数。
-   *BaseVertexLocation* - 从偏移到相应的绘图调用所使用的第一个顶点之间的字节数。

在创建顶点缓冲区之前，你需要通过创建输入布局对象来定义其布局。 创建输入布局对象后，将它绑定到输入装配器 (IA) 阶段。

### <a name="span-idindexbufferspanspan-idindexbufferspanspan-idindexbufferspanspan-idindex-bufferspanindex-buffer"></a><span id="Index_Buffer"></span><span id="index_buffer"></span><span id="INDEX_BUFFER"></span><span id="index-buffer"></span>索引缓冲区

索引缓冲区包含一组连续的 16 位或 32 位索引；每个索引用于标识顶点缓冲区中的一个顶点。 将一个索引缓冲区与一个或多个顶点缓冲区配合使用来为 IA 阶段提供数据的过程称为建立索引。 索引缓冲区可进行可视化，如下图所示。

![索引缓冲区的图示](images/d3d10-index-buffer.png)

使用以下参数查找存储在索引缓冲区中的连续索引：

-   *Offset* - 从缓冲区起点到第一个索引之间的字节数。
-   *StartIndexLocation* - 从偏移到相应的绘图调用所使用的第一个顶点之间的字节数。
-   *IndexCount* - 要呈现的索引的数量。

索引缓冲区可通过使用带-剪切索引分隔多个直线带或三角形带（[基元拓扑](primitive-topologies.md)）来将它们连接在一起。 带-剪切索引允许使用单一绘图调用来绘制多个直线带或三角形带。 带-剪切索引是索引的最大可能值（对于 16 位索引，为 0xffff；对于 32 位索引，为 0xffffffff）。 带-剪切索引将重置已索引基元中的缠绕顺序，并且可用于消除使在三角形带中保持正确的缠绕顺序所需的三角形退化的需求。 下图显示带-剪切索引的示例。

![带-剪切索引的图示](images/d3d10-ia-strips-cut-value.png)

### <a name="span-idshaderconstantbufferspanspan-idshaderconstantbufferspanspan-idshaderconstantbufferspanspan-idshader-constant-bufferspanconstant-buffer"></a><span id="Shader_Constant_Buffer"></span><span id="shader_constant_buffer"></span><span id="SHADER_CONSTANT_BUFFER"></span><span id="shader-constant-buffer"></span>常量缓冲区

Direct3D 具有一个提供着色器常量的缓冲区，此缓冲区称为“着色器-常量缓冲区”或简称为“常量缓冲区”。 从概念上看，它看起来像一个单元素顶点缓冲区，如下图所示。

![着色器-常量缓冲区的图示](images/d3d10-shader-resource-buffer.png)

每个元素存储 1 到 4 个组件常量（由所存储数据的格式决定）。

常量缓冲区允许将着色器常量组合在一起并同时提交，而不是逐个调用来单独提交每个常量，这减少了更新着色器常量所需的带宽。

着色器将继续直接按变量名读取常量缓冲区中的变量，读取方式与读取常量缓冲区之外的变量的方式相同。

每个着色器阶段允许最多 15 个着色器-常量缓冲区；每个缓冲区可包含最多 4096 个常量。

使用常量缓冲区存储流-输出阶段的结果。

有关在着色器中声明常量缓冲区的示例，请参阅[着色器常量 (DirectX HLSL)](https://msdn.microsoft.com/library/windows/desktop/bb509581)。

## <a name="span-idtextureresourcesspanspan-idtextureresourcesspanspan-idtextureresourcesspanspan-idtexture-resourcesspantexture-resources"></a><span id="Texture_Resources"></span><span id="texture_resources"></span><span id="TEXTURE_RESOURCES"></span><span id="texture-resources"></span>纹理资源


纹理资源是用于存储纹素的数据的结构化集合。 与缓冲区不同，在着色器单元读取纹理时，纹理采样器可筛选纹理。 纹理的类型将影响筛选纹理的方式。 纹素表示可由管道读取的或可写入管道的纹理的最小单位。 每个纹理包含 1 到 4 个组件，这些组件采用一种 DXGI 格式排列（请参阅 [**DXGI\_FORMAT**](https://msdn.microsoft.com/library/windows/desktop/bb173059)）。

纹理作为结构化资源创建，以使其大小众所周知。 但是，只要在将纹理绑定到管道时使用视图完全指定类型，在创建资源时，每个纹理可以是类型化的，也可以是无类型的。

-   [纹理类型](#texture-types)
-   [子资源](#subresources)
-   [强与弱类型化](#typed)

### <a name="span-idtexturetypesspanspan-idtexturetypesspanspan-idtexturetypesspanspan-idtexture-typesspantexture-types"></a><span id="Texture_Types"></span><span id="texture_types"></span><span id="TEXTURE_TYPES"></span><span id="texture-types"></span>纹理类型

有几种类型的纹理：1D、2D、3D，其中每种类型均可在有或没有 mipmap 的情况下创建。 Direct3D 还支持纹理数组和多重采样纹理。

-   [1D 纹理](#texture1d-resource)
-   [1D 纹理数组](#texture1d-array-resource)
-   [2D 纹理和 2D 纹理数组](#texture2d-resource)
-   [3D 纹理](#texture3d-resource)

### <a name="span-idtexture1dresourcespanspan-idtexture1dresourcespanspan-idtexture1dresourcespanspan-idtexture1d-resourcespan1d-texture"></a><span id="Texture1D_Resource"></span><span id="texture1d_resource"></span><span id="TEXTURE1D_RESOURCE"></span><span id="texture1d-resource"></span>1D 纹理

最简单形式的 1D 纹理包含可使用单个纹理坐标寻址的纹理数据；它可以可视化为纹素数组，如下图所示。

![1D 纹理的图示](images/d3d10-1d-texture.png)

每个纹素均包含大量颜色分量，具体取决于已存储数据的格式。 再复杂一些的话，你可以创建具有 mipmap 级别的 1D 纹理，如下图所示。

![具有 mipmap 级别的 1D 纹理的图示](images/d3d10-resource-texture1d.png)

mipmap 级别是一个比它上面的级别小二次幂的纹理。 最高级别包含最多细节，每个后续级别递减；对于 1D mipmap，最小级别包含一个纹理。 不同的级别由名为 LOD（细节级别）的索引标识；在呈现未靠近相机的几何图形时，你可使用 LOD 访问较小的纹理。

### <a name="span-idtexture1darrayresourcespanspan-idtexture1darrayresourcespanspan-idtexture1darrayresourcespanspan-idtexture1d-array-resourcespan1d-texture-array"></a><span id="Texture1D_Array_Resource"></span><span id="texture1d_array_resource"></span><span id="TEXTURE1D_ARRAY_RESOURCE"></span><span id="texture1d-array-resource"></span>1D 纹理数组

Direct3D 10 也具有一个适用于纹理数组的新数据结构。 从概念上说，1D 纹理数组与下图类似。

![1D 纹理数组的图示](images/d3d10-resource-texture1darray.png)

此纹理数组包含三个纹理。 三个纹理都是宽度为 5（即第一层中的元素数）的纹理。 每个纹理还包含一个 3 层 mipmap。

Direct3D 中的所有纹理数组都是同类纹理数组；这意味着，纹理数组中的每个纹理必须具有相同的数据格式和大小（包括纹理宽度和 mipmap 级别数）。 你可以创建具有不同大小的纹理数组，前提是每个数组中的所有纹理大小相同。

### <a name="span-idtexture2dresourcespanspan-idtexture2dresourcespanspan-idtexture2dresourcespanspan-idtexture2d-resourcespan2d-texture-and-2d-texture-array"></a><span id="Texture2D_Resource"></span><span id="texture2d_resource"></span><span id="TEXTURE2D_RESOURCE"></span><span id="texture2d-resource"></span>2D 纹理和 2D 纹理数组

Texture2D 资源包含 2D 纹素网格。 每个纹素均可通过 u, v 矢量进行寻址。 由于它是纹理资源，因此可包含 mipmap 级别和子资源。 完全填充的 2D 纹理资源如下图所示。

![2D 纹理资源的图示](images/d3d10-resource-texture2d.png)

此纹理资源包含一个具有 3 个 mipmap 级别的 3x5 纹理。

Texture2DArray 资源是一个同类 2D 纹理数组；也就是说，每个纹理具有相同的数据格式和尺寸（包括 mipmap 级别）。 它具有与 1D 纹理数组类似的布局，只不过这些纹理现在包含 2D 数据，因此与下图所示类似。

![2D 纹理资源数组的图示](images/d3d10-resource-texture2darray.png)

此纹理数组包含三个纹理；每个纹理均为具有两个 mipmap 级别的 3x5 纹理。

### <a name="span-idtexture2darrayresourceasatexturecubespanspan-idtexture2darrayresourceasatexturecubespanspan-idtexture2darrayresourceasatexturecubespanusing-a-texture2darray-as-a-texture-cube"></a><span id="Texture2DArray_Resource_as_a_Texture_Cube"></span><span id="texture2darray_resource_as_a_texture_cube"></span><span id="TEXTURE2DARRAY_RESOURCE_AS_A_TEXTURE_CUBE"></span>使用 Texture2DArray 作为纹理立方体

纹理立方体是一个包含 6 个纹理的 2D 纹理数组，其中每个纹理均表示立方体的一个面。 完全填充的纹理立方体如下图所示。

![表示纹理立方体的 2D 纹理资源数组的图示](images/d3d10-resource-texturecube.png)

使用立方体纹理视图将包含 6 个纹理的 2D 纹理数组绑定到管道后，可以使用立方体贴图内部函数在着色器中读取该数组。 使用从纹理立方体的中心指向外的 3D 矢量从着色器对纹理立方体进行寻址。

### <a name="span-idtexture3dresourcespanspan-idtexture3dresourcespanspan-idtexture3dresourcespanspan-idtexture3d-resourcespan3d-texture"></a><span id="Texture3D_Resource"></span><span id="texture3d_resource"></span><span id="TEXTURE3D_RESOURCE"></span><span id="texture3d-resource"></span>3D 纹理

Texture3D 资源（也称作“体纹理”）包含纹素的 3D 体。 由于它是纹理资源，因此可能包含 mipmap 级别。 完全填充的 3D 纹理如下图所示。

![3D 纹理资源的图示](images/d3d10-resource-texture3d.png)

当 3D 纹理 mipmap 切片作为呈现目标输出绑定时（具有呈现-目标视图），3D 纹理的行为与具有 n 个切片的 2D 纹理数组的相同。 从几何图形-着色器阶段中选择特定的呈现切片。

不存在 3D 纹理数组的概念；因此，3D 纹理子资源是单个 mipmap 级别。

### <a name="span-idsubresourcesspanspan-idsubresourcesspanspan-idsubresourcesspansubresources"></a><span id="Subresources"></span><span id="subresources"></span><span id="SUBRESOURCES"></span>子资源

Direct3D API 引用整个资源或多个资源子集。 为了明确资源的各个部分，Direct3D 创造了*子资源*一词，意指资源的子集。

缓冲区被定义为一个子资源。 纹理会略复杂，因为存在多种不同的纹理类型（1D、2D 等），其中部分类型支持 mipmap 级别和/或纹理数组。 从最简单的示例开始，1D 纹理被定义为单个子资源，如下图所示。

![1D 纹理的图示](images/d3d10-1d-texture.png)

这意味着，构成 1D 纹理的纹素数组包含在单个子资源中。

如果您展开包含三个 mipmap 级别的 1D 纹理，则可对其进行可视化，如下所示。

![具有 mipmap 级别的 1D 纹理的图示](images/d3d10-resource-texture1d.png)

将此视为一个由 3 个子纹理构成的纹理。 每个子纹理将计为一个子资源，因此，此 1D 纹理包含 3 个子资源。 对于单个纹理，可使用细节级别 (LOD) 为子纹理（或子资源）建立索引。 在使用纹理数组时，访问特殊子纹理需要 LOD 和特殊纹理。 或者，API 会将这两类信息组合为一个从零开始的子资源索引，如此处所示。

![从零开始的子资源索引的图示](images/d3d10-resource-texture1darray-sub-indexing.png)

### <a name="span-idselectingsubresourcesspanspan-idselectingsubresourcesspanspan-idselectingsubresourcesspanselecting-subresources"></a><span id="Selecting_Subresources"></span><span id="selecting_subresources"></span><span id="SELECTING_SUBRESOURCES"></span>选择子资源

一些 API 访问整个资源，其他 API 访问一部分资源。 访问一部分资源的 API 通常使用视图描述来指定要访问的子资源。

这些数字阐述了在访问纹理数组时视图描述使用的术语。

### <a name="span-idarrayslicespanspan-idarrayslicespanspan-idarrayslicespanarray-slice"></a><span id="Array_Slice"></span><span id="array_slice"></span><span id="ARRAY_SLICE"></span>数组切片

假设一个纹理数组，每个纹理包含 mipmap，则一个数组切片（用白色矩形表示）将包括一个纹理及其所有子纹理，如下图所示。

![数组切片的图示](images/d3d10-resource-array-slice.png)

### <a name="span-idmipslicespanspan-idmipslicespanspan-idmipslicespanmip-slice"></a><span id="Mip_Slice"></span><span id="mip_slice"></span><span id="MIP_SLICE"></span>Mip 切片

mip 切片（用白色矩形表示）为数组中的每个纹理包含一个 mipmap 级别，如下图所示。

![mip 切片的图示](images/d3d10-resource-mip-slice.png)

### <a name="span-idselectingasinglesubresourcespanspan-idselectingasinglesubresourcespanspan-idselectingasinglesubresourcespanselecting-a-single-subresource"></a><span id="Selecting_a_Single_Subresource"></span><span id="selecting_a_single_subresource"></span><span id="SELECTING_A_SINGLE_SUBRESOURCE"></span>选择单个子资源

你可使用这两种切片选择单个子资源，如下图所示。

![使用数组切片和 mip 切片选择子资源的图示](images/d3d10-resource-subresources-1.png)

### <a name="span-idselectingmultiplesubresourcesspanspan-idselectingmultiplesubresourcesspanspan-idselectingmultiplesubresourcesspanselecting-multiple-subresources"></a><span id="Selecting_Multiple_Subresources"></span><span id="selecting_multiple_subresources"></span><span id="SELECTING_MULTIPLE_SUBRESOURCES"></span>选择多个子资源

你也可使用这两种包含 mipmap 级别数和/或纹理数的切片来选择多个子资源。

![选择多个子资源的图示](images/d3d10-resource-subresources-2.png)

无论你使用哪种纹理类型，是否有 mipmap 或纹理数组，通常都会提供 Helper 函数来计算特殊子资源的索引。

### <a name="span-idtypedspanspan-idtypedspanspan-idtypedspanstrong-vs-weak-typing"></a><span id="Typed"></span><span id="typed"></span><span id="TYPED"></span>强与弱类型化

创建完全类型化的资源会将资源限制为创建时采用的格式。 这使运行时能够优化访问，尤其是在创建的资源带有指示其无法由应用程序映射的标志时。 无法使用视图机制解释使用特定类型创建的资源。

在无类型资源中，首次创建资源时数据类型是未知的。 应用程序必须从可用的无类型格式中选择。 你必须指定要分配的内存大小以及运行时是否需要在 mipmap 中生成子纹理。

不过，准确的数据格式（无论内存将解释为整数、浮点值还是无符号整数等）直到使用视图将资源绑定到管道时才能确定。 由于纹理格式在纹理绑定到管道之前仍保持灵活性，因此资源被称为弱类型化的存储。 弱类型化的存储具有以下优点：它可以重复使用或重新解释（以另一种格式），前提是新格式的组件位与旧格式的位计数匹配。

单个资源可绑定到多个管道阶段，前提是每个资源具有唯一视图，这样可以完全限定每个位置的格式。 例如，使用无类型格式创建的资源可在管道中的不同位置同时用作 FLOAT 格式和 UINT 格式。

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>相关主题


[资源](resources.md)
