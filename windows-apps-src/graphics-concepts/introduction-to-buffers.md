---
title: 缓冲区简介
description: 缓冲区资源是一系列完全类型化的数据，被分组到元素中。
ms.assetid: 494FDF57-0FBE-434C-B568-06F977B40263
keywords:
- 缓冲区简介
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: deeae0cc66a7e75da2e44c0d2aba2a9ed459b824
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2019
ms.locfileid: "57602572"
---
# <a name="introduction-to-buffers"></a>缓冲区简介


缓冲区资源是一系列完全类型化的数据，被分组到元素中。 缓冲区将存储数据，如*顶点缓冲区* 中的纹理坐标、*索引缓冲区* 中的索引、*常量缓冲区* 中的着色器常量数据、位置矢量、法向矢量或设备状态。

缓冲区元素由 1 到 4 个部分组成。 缓冲区元素可以包含打包数据值（如 R8G8B8A8 表面值）、单个 8 位整数或四个 32 位浮点值。

缓冲区是作为非结构化资源创建的。 由于是非结构化的，缓冲区不能包含任何 mipmap 级别，它不能在读取时被过滤，也不能被多重采样。

## <a name="span-idbuffertypesspanspan-idbuffertypesspanspan-idbuffertypesspanbuffer-types"></a><span id="Buffer_Types"></span><span id="buffer_types"></span><span id="BUFFER_TYPES"></span>缓冲区类型


Direct3D 11 支持以下缓冲区资源类型。

-   [顶点缓冲区](#vertex-buffer)
-   [索引缓冲区](#index-buffer)
-   [常量缓冲区](#shader-constant-buffer)

### <a name="span-idvertexbufferspanspan-idvertexbufferspanspan-idvertexbufferspanspan-idvertex-bufferspanvertex-buffer"></a><span id="Vertex_Buffer"></span><span id="vertex_buffer"></span><span id="VERTEX_BUFFER"></span><span id="vertex-buffer"></span>顶点缓冲区

顶点缓冲区包含用于定义几何图形的顶点数据。 顶点数据包括位置坐标、颜色数据、纹理坐标数据、法线数据等。

最简单的顶点缓冲区只包含位置数据。 它可进行可视化，如下图所示。

![包含位置数据的顶点缓冲区的图示](images/d3d10-resources-single-element-vb2.png)

更常见的情况是，顶点缓冲区包含完全指定 3D 顶点所需的所有数据。 示例可能是包含每顶点位置、法线和纹理坐标的顶点缓冲区。 此数据通常组织为每顶点元素集，如下图所示。

![包含位置、法线和纹理数据的顶点缓冲区的图示](images/d3d10-vertex-buffer-element.png)

该顶点缓冲区包含每个顶点的数据；每个顶点存储三个元素（位置、法线和纹理坐标）。 通常，使用 3 个 32 位浮点指定每个位置和法线，使用两个 32 位浮点指定纹理坐标。

要从顶点缓冲区访问数据，您需要知道要访问的顶点，以及以下额外的缓冲区参数：

-   Offset - 缓冲区起点到第一个顶点数据之间的字节数量。
-   BaseVertexLocation - 从偏移到相应的绘制调用使用的第一个顶点之间的字节数量。

在创建顶点缓冲区之前，您需要定义其布局。 创建 input-layout 对象后，将其绑定到[输入装配器 (IA) 阶段](input-assembler-stage--ia-.md)。

### <a name="span-idindexbufferspanspan-idindexbufferspanspan-idindexbufferspanspan-idindex-bufferspanindex-buffer"></a><span id="Index_Buffer"></span><span id="index_buffer"></span><span id="INDEX_BUFFER"></span><span id="index-buffer"></span>索引缓冲区

索引缓冲区包含到顶点缓冲区的整数偏移量，用于更高效地渲染基元。 索引缓冲区包含一组连续的 16 位或 32 位索引；每个索引用于标识顶点缓冲区中的一个顶点。 索引缓冲区可进行可视化，如下图所示。

![索引缓冲区的图示](images/d3d10-index-buffer.png)

使用以下参数查找存储在索引缓冲区中的连续索引：

-   Offset - 从索引缓冲区的基址开始的字节数。
-   StartIndexLocation - 用基址和偏移量指定第一个索引缓冲区元素。 开始位置表示要渲染的第一个索引。
-   IndexCount - 要渲染的索引的数量。

索引缓冲区开头 = 索引缓冲区基址 + 偏移量 （字节） + StartIndexLocation \* ElementSize （字节）;

在该计算公式中，ElementSize 是每个索引缓冲区元素的大小（两个或四个字节）。

### <a name="span-idshaderconstantbufferspanspan-idshaderconstantbufferspanspan-idshaderconstantbufferspanspan-idshader-constant-bufferspanconstant-buffer"></a><span id="Shader_Constant_Buffer"></span><span id="shader_constant_buffer"></span><span id="SHADER_CONSTANT_BUFFER"></span><span id="shader-constant-buffer"></span>常量缓冲区

常量缓冲区让你能够高效地向管道提供着色器常量数据。 你可以使用常量缓冲区存储流输出阶段的结果。 从概念上讲，常量缓冲区看起来就像单元素顶点缓冲区，如下图所示。

![着色器-常量缓冲区的图示](images/d3d10-shader-resource-buffer.png)

每个元素存储 1 到 4 个组件常量（由所存储数据的格式决定）。

常量缓冲区只能使用单个绑定标志，不能与任何其他绑定标志结合使用。

要从着色器读取着色器常量缓冲区，请使用 HLSL 加载函数。 每个着色器阶段允许最多 15 个着色器-常量缓冲区；每个缓冲区可包含最多 4096 个常量。

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>相关主题


[顶点和索引缓冲区](vertex-and-index-buffers.md)

 

 




