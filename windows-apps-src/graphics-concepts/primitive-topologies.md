---
title: 基元拓扑
description: Direct3D 支持多种基元拓扑，后者定义管道（如点列表、线列表和三角形带）将如何解释和呈现顶点。
ms.assetid: 7AA5A4A2-0B7C-431D-B597-684D58C02BA5
keywords:
- 基元拓扑
author: michaelfromredmond
ms.author: mithom
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
ms.localizationpriority: medium
ms.openlocfilehash: cf14efc4c4ec29c98ebebff91493623d55267db5
ms.sourcegitcommit: 897a111e8fc5d38d483800288ad01c523e924ef4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/13/2018
ms.locfileid: "1044606"
---
# <a name="primitive-topologies"></a>基元拓扑


Direct3D 支持多种基元拓扑，后者定义管道（如点列表、线列表和三角形带）将如何解释和呈现顶点。

## <a name="span-idprimitivetypesspanspan-idprimitivetypesspanspan-idprimitivetypesspanbasic-primitive-topologies"></a><span id="Primitive_Types"></span><span id="primitive_types"></span><span id="PRIMITIVE_TYPES"></span>基本基元拓扑


支持以下基本基元拓扑（或基元类型）：

-   [点列表](point-lists.md)
-   [行列表](line-lists.md)
-   [线条带](line-strips.md)
-   [三角形列表](triangle-lists.md)
-   [三角形带](triangle-strips.md)

有关每个基元类型的可视化，请参阅[缠绕方向和前导顶点位置](#winding-direction-and-leading-vertex-positions)中本主题后面的图示。

[输入装配器 (IA) 阶段](input-assembler-stage--ia-.md)从顶点和索引缓冲区读取数据，将数据装配到这些基元中，然后将数据发送到剩余的管道阶段。

## <a name="span-idprimitiveadjacencyspanspan-idprimitiveadjacencyspanspan-idprimitiveadjacencyspanprimitive-adjacency"></a><span id="Primitive_Adjacency"></span><span id="primitive_adjacency"></span><span id="PRIMITIVE_ADJACENCY"></span>基元邻近度


所有 Direct3D 基元类型（点列表除外）以两个版本提供：带邻近度的基元类型和不带邻近度的基元类型。 带邻近度的基元包含一些周围的顶点，而不带邻近度的基元仅包含目标基元的顶点。 例如，线列表基元具有包含邻近度的对应线列表基元。

相邻基元旨在提供有关你的几何图形的更多信息，只能通过几何着色器查看。 邻近度对使用剪影检测、阴影卷挤压等技术的几何着色器很有用。

例如，假设你想要绘制带邻近度的三角形列表。 包含 36 个顶点的三角形列表（带邻近度）将产生 6 个已完成的基元。 带邻近度的基元（线条带除外）包含的顶点数正好是不带邻近度的等效基元的顶点数的两倍，其中每个多出的顶点都是相邻顶点。

## <a name="span-idwindingdirectionandleadingvertexpositionsspanspan-idwindingdirectionandleadingvertexpositionsspanspan-idwindingdirectionandleadingvertexpositionsspanspan-idwinding-direction-and-leading-vertex-positionsspanwinding-direction-and-leading-vertex-positions"></a><span id="Winding_Direction_and_Leading_Vertex_Positions"></span><span id="winding_direction_and_leading_vertex_positions"></span><span id="WINDING_DIRECTION_AND_LEADING_VERTEX_POSITIONS"></span><span id="winding-direction-and-leading-vertex-positions"></span>缠绕方向和前导顶点位置


如下图中所示，前导顶点是基元中的第一个非相邻顶点。 基元类型可以定义多个前导顶点，只要每个顶点都用于不同的基元。

-   对于带邻近度的三角形带，前导顶点是 0、2、4、6，依此类推。
-   对于带邻近度的线条带，前导顶点是 1、2、3，依此类推。
-   另一方面，相邻基元没有前导顶点。

下图显示了输入装配器可生成的所有基元类型的顶点顺序。

![基元类型的顶点顺序的图示](images/d3d10-primitive-topologies.png)

下表描述了上图中的符号。

| 符号                                                                                   | 名称              | 描述                                                                         |
|------------------------------------------------------------------------------------------|-------------------|-------------------------------------------------------------------------------------|
| ![顶点的符号](images/d3d10-primitive-topologies-vertex.png)                     | 顶点            | 3D 空间中的点。                                                                |
| ![缠绕方向的符号](images/d3d10-primitive-topologies-winding-direction.png) | 缠绕方向 | 装配基元时的顶点顺序。 可以是顺时针或逆时针。 |
| ![前导顶点的符号](images/d3d10-primitive-topologies-leading-vertex.png)       | 前导顶点    | 包含每常量数据的基元中的第一个非相邻顶点。       |

 

## <a name="span-idgeneratingmultiplestripsspanspan-idgeneratingmultiplestripsspanspan-idgeneratingmultiplestripsspangenerating-multiple-strips"></a><span id="Generating_Multiple_Strips"></span><span id="generating_multiple_strips"></span><span id="GENERATING_MULTIPLE_STRIPS"></span>生成多个条带


你可以通过条带切割生成多个条带。 你可以通过显式调用 [RestartStrip](https://msdn.microsoft.com/library/windows/desktop/bb509660) HLSL 函数或通过将特殊索引值插入索引缓冲区来执行条带切割。 此值为 –1，对于 32 位指数为 0xffffffff，对于 16 位指数为 0xffff。

索引 –1 表示显式“切割”或“重启”当前条带。 上一个索引完成上一个基元或条带，下一个索引启动新基元或条带。

有关生成多个条带的详细信息，请参阅[几何着色器 (GS) 阶段](geometry-shader-stage--gs-.md)。

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>相关主题


[输入装配器 (IA) 阶段](input-assembler-stage--ia-.md)

[图形管道](graphics-pipeline.md)

 

 




