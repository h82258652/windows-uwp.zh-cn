---
title: 细化器 (TS) 阶段
description: 细化器 (TS) 阶段创建代表几何图形修补程序的域的采样模式，并生成连接这些样本的一组较小对象（三角形、点或线）。
ms.assetid: 2F006F3D-5A04-4B3F-8BC7-55567EFCFA6C
keywords:
- 细化器 (TS) 阶段
author: michaelfromredmond
ms.author: mithom
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 1d57c60e8cba9be75e936c55800bac93f8df3e30
ms.sourcegitcommit: 086001cffaf436e6e4324761d59bcc5e598c15ea
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/27/2018
ms.locfileid: "5698988"
---
# <a name="tessellator-ts-stage"></a>细化器 (TS) 阶段


细化器 (TS) 阶段创建代表几何图形修补程序的域的采样模式，并生成连接这些样本的一组较小对象（三角形、点或线）。

## <a name="span-idpurposeandusesspanspan-idpurposeandusesspanspan-idpurposeandusesspanpurpose-and-uses"></a><span id="Purpose_and_uses"></span><span id="purpose_and_uses"></span><span id="PURPOSE_AND_USES"></span>用途和用法


下图重点显示了 Direct3D 图形管道的阶段。

![重点显示外壳着色器、细化器和域着色器阶段的 direct3d 11 管道图](images/d3d11-pipeline-stages-tessellation.png)

下表显示分割阶段的进度。

![分割进度图](images/tess-prog.png)

进度从低画质细分图面开始。 进度接下来使用连接这些样本的相应几何图形修补程序、域样本和三角形重点显示输入修补程序。 最后，进度重点显示与这些样本对应的顶点。

Direct3D 运行时支持实现分割的三个阶段，通过分割可将低画质细分图面转换为 GPU 上的高画质基元。 分割将高阶图面平铺（或分裂）成合适的结构以进行渲染。

各分割阶段一起将高阶图面（保留了模型简易度和效率）转换为多个三角形，从而在 Direct3D 图形管道中进行精确渲染。

分割使用 GPU 根据构建于四边形修补程序、三角形修补程序或等值线的图面计算更加精细的图面。 为了接近高阶图面，每个修补程序会通过分割因子细分为三角形、点或线。 Direct3D 图形管道利用三个管道阶段实施分割：

-   [外壳着色器 (HS) 阶段](hull-shader-stage--hs-.md) - 可编程的着色器阶段，可生成与各个输入修补程序（四边形、三角形或线）对应的几何修补程序（和修补程序常数）。
-   细化器 (TS) 阶段 - 固定功能管道阶段，创建代表几何图形修补程序的域的采样模式，并生成连接这些样本的一组较小对象（三角形、点或线）。
-   [域着色器 (DS) 阶段](domain-shader-stage--ds-.md) - 可编程的着色器阶段，计算与各个域样本对应的顶点位置。

通过在硬件中实施分割，图形管道可评估更低画质（多边形数量更少）模型并以更高画质进行渲染。 在可完成软件分割时，由硬件实施的分割可生成大量的视觉细节（包括位移映射的支持），无需为模型大小添加视觉细节，也不会影响刷新率。

分割的优点：

-   分割能够节约大量的成本和带宽，进而让应用程序从低分辨率的模型渲染更高画质的图面。 在 Direct3D 图形管道中实施的分割技术还支持位移映射，这能够产生丰富的图面细节。
-   分割支持可扩展的渲染技术，例如持续或视点相关精细程度（无需停机即可计算）。
-   分割能以较低频率执行高成本计算（在较低画质模型上计算），进而改善性能。 其中包括使用融合变形或变形目标实现逼真动画的混合计算，或者碰撞检测或柔体动力学的物理计算。

Direct3D 图形管道在软件中实施分割，能够将 CPU 的工作负担转移至 GPU。 如果应用程序实施大量的变形目标和/或更加复杂的蒙皮/变形模型，这将能够显著改善性能。

细化器是由一种固定功能阶段，通过将[外壳着色器](hull-shader-stage--hs-.md)绑定至管道进行初始化。 （请参阅[如何：初始化细化器阶段](https://msdn.microsoft.com/library/windows/desktop/ff476341)）。 细化器阶段旨在将域（四边形、三角形或线）分割为很多较小对象（三角形、点或线）。 细化器能够在标准化（零到一）协调系统中平铺规范域。 例如，四边形域细化为单位正方形。

### <a name="span-idphasesinthetessellatortsstagespanspan-idphasesinthetessellatortsstagespanspan-idphasesinthetessellatortsstagespanphases-in-the-tessellator-ts-stage"></a><span id="Phases_in_the_Tessellator__TS__stage"></span><span id="phases_in_the_tessellator__ts__stage"></span><span id="PHASES_IN_THE_TESSELLATOR__TS__STAGE"></span>细化器 (TS) 阶段中的各个阶段

细化器 (TS) 阶段的运行有两个阶段：

-   第一阶段使用 32 位浮点算法处理分割因子、修复制圆问题、处理极小因子、减少并合并因子。
-   第二阶段根据所选分区类型，生成点或拓扑列表。 这是细化器阶段的核心任务，通过定点算法使用 16 位分数。 定点算法可实现硬件加速，同时维持可接受的精度。 例如，对于 64 米宽修补程序，该精度可实现 2 毫米分辨率的点。

    | 分区类型 | 范围                       |
    |----------------------|-----------------------------|
    | 分数\_奇数      | \[1...63\]                  |
    | 分数\_偶数     | 分割因子范围：\[2..64\] |
    | 整数              | 分割因子范围：\[1..64\] |
    | 幂 2                 | 分割因子范围：\[1..64\] |

     

分割通过两个可编程的着色器阶段实施：[外壳着色器](hull-shader-stage--hs-.md)和[域着色器](domain-shader-stage--ds-.md)。 这些着色器阶段通过在着色器模型 5 中定义的 HLSL 代码进行编程。 着色器目标为：hs\_5\_0 和 ds\_5\_0。 标题创建着色器，然后从在着色器绑定至管道时传递至运行时的编译着色器提取硬件代码。

### <a name="span-idenablingdisablingtessellationspanspan-idenablingdisablingtessellationspanspan-idenablingdisablingtessellationspanenablingdisabling-tessellation"></a><span id="Enabling_disabling_tessellation"></span><span id="enabling_disabling_tessellation"></span><span id="ENABLING_DISABLING_TESSELLATION"></span>启用/禁用分割

通过创建外壳着色器并将其绑定至外壳着色器阶段（这会自动设置细化器阶段），进而实现分割。 要从细化修补程序生成最终顶点位置，你还需要创建[域着色器](domain-shader-stage--ds-.md)并将其绑定至域着色器阶段。 启用分割后，输入汇编程序 (IA) 阶段的数据输入必须为修补程序数据。 输入汇编程序拓扑必须为修补程序常数拓扑。

要禁用分割，则将外壳着色器和域着色器设为**空**。 [几何着色器 (GS) 阶段](geometry-shader-stage--gs-.md)和[流输出 (SO) 阶段](stream-output-stage--so-.md)均不会读取外壳着色器输出控制点或修补程序数据。

## <a name="span-idinputspanspan-idinputspanspan-idinputspaninput"></a><span id="Input"></span><span id="input"></span><span id="INPUT"></span>输入


通过使用分割因子（指定域的细化精细度）以及从外壳着色器阶段传入的分区类型（指定用于修补程序切片的算法），细化器在每个修补程序运行一次。

## <a name="span-idoutputspanspan-idoutputspanspan-idoutputspanoutput"></a><span id="Output"></span><span id="output"></span><span id="OUTPUT"></span>输出


细化器向域着色器阶段输出 uv（以及可选的 w）坐标和图面拓扑。

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>相关主题


[图形管道](graphics-pipeline.md)

 

 




