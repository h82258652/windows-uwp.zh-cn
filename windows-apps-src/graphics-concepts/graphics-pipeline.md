---
title: 图形管道
description: Direct3D 图形管道旨在为实时游戏应用程序生成图形。 数据通过各个可配置或可编程的阶段从输入流到输出。
ms.assetid: C9519AD0-5425-48BD-9FF4-AED8959CA4AD
keywords:
- 图形管道
- 管道阶段
author: michaelfromredmond
ms.author: mithom
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
ms.localizationpriority: medium
ms.openlocfilehash: 6cb50af5facdbf4271c9911f0e1f536dead0b8b8
ms.sourcegitcommit: 897a111e8fc5d38d483800288ad01c523e924ef4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/13/2018
ms.locfileid: "1044906"
---
# <a name="graphics-pipeline"></a>图形管道


Direct3D 图形管道旨在为实时游戏应用程序生成图形。 数据通过各个可配置或可编程的阶段从输入流到输出。

所有阶段都可以使用 Direct3D API 进行配置。 具有常见着色器核心（圆角矩形块）的阶段可通过使用 [HLSL](https://msdn.microsoft.com/library/windows/desktop/bb509561) 编程语言进行编程。 这使得管道具有非常高的灵活性和适应性。

最常用的是顶点着色器 (VS) 阶段和像素着色器 (PS) 阶段。 如果你甚至不提供这些着色器阶段，则使用默认的无操作、传递式顶点和像素着色器。

![direct3d 11 可编程管道中的数据流图示](images/d3d11-pipeline-stages.jpg)

## <a name="input-assembler-stage"></a>输入装配器阶段

|-|-| |用途 |[输入汇编 (IA) 阶段](input-assembler-stage--ia-.md)供应品基元和相邻数据到管道，如三角形、 行和点，其中包括语义 Id，以帮助确保着色器通过减少为没有已被基元处理效率处理。 ||输入 |基元中的数据 （三角形、 行和/或磅为单位），在内存中的用户填充缓冲区。 和可能的邻接数据。 一个三角形应为 3 的每个三角形的顶点和可能 3 相邻数据，每个三角形的顶点。 ||输出 |使用附加系统生成的值 （如基元 ID、 实例 ID 或顶点 ID） 的基元。 |

## <a name="vertex-shader-stage"></a>顶点着色器阶段

|-|-| |用途 |[顶点着色 (VS) 阶段](vertex-shader-stage--vs-.md)处理顶点，通常执行的操作，例如转换，皮，以及照明。 顶点着色器获取一个输入顶点并生成一个输出顶点。 每个顶点单个操作，如转换、 模式、 Morphing 和每个顶点的照明。 ||输入 |单个顶点，与 VertexID 和 InstanceID 系统生成的值。 每个顶点着色输入的顶点可以由达 16 位向量 （最多 4 组件每个） 组成。 ||输出 |单个顶点。 每个输出顶点可以组成多达 16 位 4 组件向量。 |
 
## <a name="hull-shader-stage"></a>外壳着色器阶段
 
|-|-| |用途 |[轮廓着色 (HS) 阶段](hull-shader-stage--hs-.md)是有效地拆分为多个三角形模型的单一曲面分割阶段之一。 每个修补程序调用一次外壳着色器，它将定义低阶表面的输入控制点转换为构成修补程序的控制点。 它还将执行一些每个修补程序计算的 Tessellator (TS) 阶段和域着色 (DS) 阶段提供数据。 ||输入 |1 和 32 输入的控制点，之间的共同定义低序位面。 ||输出 |1 到 32 输出控件点之间的共同组成在一个修补程序。 轮廓着色声明状态为 Tessellator (TS) 讲台中，包括控制点、 修补程序面临的类型和分区 tessellating 时要使用的类型的数目。 |

## <a name="tessellator-stage"></a>细化器阶段

|-|-| |用途 |[Tessellator (TS) 阶段](tessellator-stage--ts-.md)创建表示 geometry 修补程序，并生成一组连接这些示例的较小对象 （三角形、 点或行） 的域的采样阵列。 ||输入 |Tessellator 使用 （的指定域将 tessellated 细微程度） 分割因素和的类型的分区 （用于指定用于设置在一个修补程序切片的算法） 从轮廓着色阶段中传递的修补程序每运行一次。 | |输出 |Tessellator 输出 uv （和 （可选) w） 坐标和表面拓扑至域着色阶段。 |

## <a name="domain-shader-stage"></a>域着色器阶段

|-|-| |用途 |[域着色 (DS) 阶段](domain-shader-stage--ds-.md)计算中输出修补程序; 的细分点顶点位置它计算对应于每个域示例的顶点位置。 域着色运行一次，每 tessellator 阶段输出点具有只读访问轮廓着色器输出修补程序和输出修补程序常量和 tessellator 阶段输出 UV 坐标。 ||输入 |域着色使用输出控制点[轮廓着色 (HS) 容器](hull-shader-stage--hs-.md)中。 外壳着色器输出包括：控制点、修补程序常数数据和细化因素（例如，细化因素可以包括固定函数细化器使用的值以及原始值（例如在被整数细化舍入前），这有助于加快几何过渡）。 输出坐标[Tessellator (TS) 容器](tessellator-stage--ts-.md)中每一次调用域着色。 ||输出 |域着色 (DS) 阶段输出中输出修补程序的细分点顶点位置。 |

## <a name="geometry-shader-stage"></a>几何着色器阶段

|-|-| |用途 |[Geometry 着色 (GS) 阶段](geometry-shader-stage--gs-.md)处理整个基元： 三角形、 线条和点，以及其相邻的顶点。 它支持几何放大和解扩。 它对于点精灵扩展、动态粒子系统、皮毛/鳍生成、阴影卷生成、单通道渲染到 Cubemap、每基元材料交换和每基元材料设置等算法很有用 - 包括将重心坐标生成为基元数据，使得像素着色器可以执行定制属性内插。 | |输入 |与顶点着色，运行在单个顶点，不同的 geometry 着色输入是完整基元 （三个三角形的顶点、 两个顶点的线条或单个顶点点） 的顶点。 ||输出 |能够输出形成单个选定的拓扑的多个顶点的几何着色 (GS) 阶段。 可用的几何着色器输出拓扑有 <strong>tristrip</strong>、<strong>linestrip</strong> 和 <strong>pointlist</strong>。 在几何着色器的任何调用中，发出的基元的数目可以自由地变化，但是必须静态地声明可发出的顶点的最大数目。 条长度从几何着色器调用发出可以是任意的并且可以通过[RestartStrip](https://msdn.microsoft.com/library/windows/desktop/bb509660) HLSL 函数创建新的条纹。 |

## <a name="stream-output-stage"></a>流输出阶段

|-|-| |用途 |[流输出 （等） 阶段](stream-output-stage--so-.md)持续输出 （或流式处理） 将数据从以前的活动阶段顶点到内存中的一个或多个缓冲区。 可以为输入的数据或读取回 cpu 回管道 recirculated 流向内存的数据。 ||输入 |从以前的管道阶段的顶点数据。 ||输出 |持续的流输出 （等） 阶段输出 （或流式处理） 将数据从以前活动讲台中，如 geometry 着色 (GS) 讲台中，顶点到内存中的一个或多个缓冲区。 Geometry 着色 (GS) 阶段处于非活动状态，并且流输出 （等） 阶段处于活动状态，如果持续输出将数据从域着色 (DS) 阶段顶点到缓冲区内存 （或如果 DS 还处于非活动状态，从顶点着色 (VS) 阶段）。 |

## <a name="rasterizer-stage"></a>光栅器阶段

|-|-| |用途 |在视图中，不[光栅 (RS) 阶段](rasterizer-stage--rs-.md)剪辑基元基元准备像素着色 (PS) 讲台中，并确定如何调用像素着色器。 转换为光栅图像 （由像素组成） 以显示实时三维图形矢量 （由组成的形状或基元） 的信息。 ||输入 |顶点的 x、 y、 z （w） 进入光栅阶段假定已同类剪辑空间中。 此 X 轴向右，向上箭头的 Y 坐标空间中 Z points 离开照相机。 ||输出 |需要呈现实际像素。 包含用于某些顶点属性的像素着色器插值。 |

## <a name="pixel-shader-stage"></a>像素着色器阶段
 
|-|-| |用途 |[像素着色 (PS) 阶段](pixel-shader-stage--ps-.md)的基元接收插值的数据并生成每像素数据，如颜色。 ||输入 |当没有几何着色配置管道时，像素着色器仅限于 16，32 位，4 组件输入。 否则，像素着色器可能占用多达 32 个 32 位 4 分量输入。 像素着色器输入数据包含顶点属性（可以在使用或不使用透视校正的情况下内插），或者也可以被视为每基元常量。 像素着色器输入根据声明的内插模式从正在光栅化的基元的顶点属性内插。 如果基元在光栅化之前被剪裁，内插模式在剪裁过程中也受支持。 | |输出 |像素着色器可以输出最多为 8，32 位，4 组件颜色或无颜色如果部分像素将被丢弃。 必须声明像素着色器输出 register 组件后，就可以使用;每个注册允许不同的输出写掩码。 |

## <a name="output-merger-stage"></a>输出合并阶段
 
|-|-| |用途 |[输出合并 (OM) 阶段](output-merger-stage--om-.md)呈现目标和深度/模具缓冲区生成最终管道结果的内容与结合使用各种类型的输出数据 （像素着色器值、 深度和模具信息）。 ||输入 |输出合并输入是管道状态，生成像素着色器、 内容呈现目标和深度/模具缓冲区的内容的像素数据。 ||输出 |最终呈现像素的颜色。 |

## <a name="related-topics"></a>相关主题

- [Direct3D 图形学习指南](index.md)

- [计算管道](compute-pipeline.md)
