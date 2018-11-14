---
title: 几何着色器 (GS) 阶段
description: 几何着色器 (GS) 阶段处理整个基元、三角形、线和点，以及它们的相邻顶点。
ms.assetid: 8A1350DD-B006-488F-9DAF-14CD2483BA4E
keywords:
- 几何着色器 (GS) 阶段
author: michaelfromredmond
ms.author: mithom
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: c4659ee4200915a7cc82f46c90f0e53965f322d5
ms.sourcegitcommit: 4d88adfaf544a3dab05f4660e2f59bbe60311c00
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/13/2018
ms.locfileid: "6467828"
---
# <a name="geometry-shader-gs-stage"></a>几何着色器 (GS) 阶段


几何着色器 (GS) 阶段处理整个基元：三角形、线和点，以及它们的相邻顶点。 它对于点精灵扩展、动态粒子系统和阴影卷生成等算法非常有用。 它支持几何放大和解扩。

## <a name="span-idpurposeandusesspanspan-idpurposeandusesspanspan-idpurposeandusesspanpurpose-and-uses"></a><span id="Purpose_and_uses"></span><span id="purpose_and_uses"></span><span id="PURPOSE_AND_USES"></span>用途和用法


几何着色器阶段处理整个基元：三角形（最多 3 个相邻顶点的 3 个顶点）、线（最多 2 个相邻顶点的 2 个顶点）和点（1 个顶点）。

![三角形和具有邻近顶点的线的图示](images/d3d10-gs.png)

几何着色器还支持有限的几何放大和解扩。 在给定输入基元的情况下，几何着色器可以丢弃基元，或者发出一个或多个新的基元。

几何着色器 (GS) 阶段是一个可编程着色器阶段；它在[图形管道](graphics-pipeline.md)图中显示为圆块。 此着色器阶段将公开自己的独特功能，该功能基于着色器模型（请参阅[常用着色器核心](https://msdn.microsoft.com/library/windows/desktop/bb509580)）进行构建。

几何着色器阶段非常适合以下算法：

-   点精灵扩展
-   动态粒子系统
-   皮毛/晶粒生成
-   阴影卷生成
-   单程渲染到 Cubemap
-   每基元材料交换
-   每基元材料设置 - 此功能包括将重心坐标生成为基元数据，使得像素着色器可以执行自定义属性内插。

## <a name="span-idinputspanspan-idinputspanspan-idinputspaninput"></a><span id="Input"></span><span id="input"></span><span id="INPUT"></span>输入


几何着色器阶段运行应用程序指定的着色器代码，其中整个基元作为输入，并能够在输出上生成顶点。 与在单个顶点上操作的顶点着色器不同，几何着色器的输入是完整基元的顶点（三角形为三个顶点，线为两个顶点或点为单个顶点）。 几何着色器还可以引入边缘相邻基元的顶点数据作为输入（三角形增加三个顶点，线增加两个顶点）。

几何着色器阶段还可以使用由[输入装配器 (IA) 阶段](input-assembler-stage--ia-.md)自动生成的 **SV\_PrimitiveID** 系统生成值。 这使得获取或计算每基元数据成为可能（如果需要）。

当几何着色器处于活动状态时，每个在管道中传递的或之前在管道中生成的基元都将调用一次几何着色器。 将几何着色器的每次调用都视为输入调用基元的数据，无论它是单点、单线还是单个三角形。 在管道中较早的三角形带将导致对带中每个单独三角形的几何着色器的调用（就像带被展开为三角形列表一样）。 单独基元中每个顶点的所有输入数据都可用（即，三角形的 3 个顶点），加上相邻的顶点数据（如果适用和可用）。

常见顶点缩写：

|     |                 |
|-----|-----------------|
| TV  | 三角形顶点 |
| LV  | 线顶点     |
| AV  | 相邻顶点 |

 

## <a name="span-idoutputspanspan-idoutputspanspan-idoutputspanoutput"></a><span id="Output"></span><span id="output"></span><span id="OUTPUT"></span>输出


几何着色器 (GS) 阶段能够输出形成单个选定拓扑的多个顶点。 可用的几何着色器输出拓扑有 **tristrip**、**linestrip** 和 **pointlist**。 在几何着色器的任何调用中，发出的基元的数目可以自由地变化，但是必须静态地声明可发出的顶点的最大数目。 从几何着色器调用发出的带长度可以是任意的，并且可以通过 [RestartStrip](https://msdn.microsoft.com/library/windows/desktop/bb509660) HLSL 函数创建新带。

几何着色器实例的执行与其他调用是不可分割的，除了添加到流的数据是串行的。 几何着色器的给定调用的输出独立于其他调用（尽管遵循排序）。 生成三角形条的几何着色器将在每次调用时启动一个新的带。

几何着色器输出可经由流输出阶段馈送到光栅器阶段和/或内存中的顶点缓冲区。 馈送到内存的输出可扩展到单个点/线/三角形列表（与将传递到光栅器的列表一致）。

几何着色器通过将顶点附加到输出流对象来一次输出一个顶点。 流的拓扑由固定声明确定，选择 **TriangleStream**、**LineStream** 和 **PointStream** 作为 GS 阶段的输出。

有三种类型的流对象可用：**TriangleStream**、**LineStream** 和 **PointStream**，它们都是模板对象。 输出的拓扑由它们各自的对象类型确定，而附加到流的顶点的格式由模板类型确定。

当几何着色器输出被识别为系统解释值（例如，**SV\_RenderTargetArrayIndex** 或 **SV\_Position**）时，除了能够将数据本身传递到下一个着色器阶段以用于输入以外，硬件还会查看此数据并执行某些取决于值的行为。 当几何着色器的这种数据输出在每基元基础上对硬件有意义时（例如 **SV\_RenderTargetArrayIndex** 或 **SV\_ViewportArrayIndex**），而不是在每顶点基础上（例如 **SV\_ClipDistance\[n\]** 或 **SV\_Position**），则每基元数据取自针对基元发出的前导顶点。

如果几何着色器结束并且基元不完整，则几何着色器可以生成部分完成的基元。 不完整的基元被静默丢弃。 这类似于 IA 处理部分完成的基元的方式。

几何着色器可以执行加载和纹理采样操作，其中不需要屏幕空间派生对象（**samplelevel**、**samplecmplevelzero**、**samplegrad**）。

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>相关主题


[图形管道](graphics-pipeline.md)

 

 




