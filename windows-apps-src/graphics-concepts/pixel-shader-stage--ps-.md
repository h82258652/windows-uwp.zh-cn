---
title: 像素着色器 (PS) 阶段
description: 像素着色器 (PS) 阶段接收基元的插值数据并生成每像素数据，如颜色。
ms.assetid: 0AEBFDFB-0AD8-4633-AE4E-A44004B57745
keywords:
- 像素着色器 (PS) 阶段
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: e1f7e787f2ee80a3168d38a9afd9a249dc0e6de0
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2019
ms.locfileid: "57603062"
---
# <a name="pixel-shader-ps-stage"></a>像素着色器 (PS) 阶段


像素着色器 (PS) 阶段接收基元的插值数据并生成每像素数据，如颜色。

这是一个可编程着色阶段；它在[图形管道](graphics-pipeline.md)图中显示为一个圆角块。 此着色器阶段将公开自己的独特功能，该功能基于着色器模型 4.0 [常用着色器核心](https://msdn.microsoft.com/library/windows/desktop/bb509580)进行构建。

像素着色器 (PS) 阶段支持丰富的着色技术，如每像素照明和后处理。 像素着色器是一个程序，它将常变量、纹理数据、内插的每顶点值和其他数据组合起来以生成每像素输出。 [光栅器 (RS) 阶段](rasterizer-stage--rs-.md)将为基元覆盖的每个像素调用一个像素着色器，但是，可以指定 **NULL** 着色器以避免运行着色器。

当对某个纹理进行多重采样时，如果为每个覆盖的多重采样执行了一次深度/模具测试，则会为每个覆盖的像素调用一次像素着色器。 通过深度/模具测试的样本将使用像素着色器输出颜色进行更新。

像素着色器内部函数将生成或使用与屏幕空间 x 和 y 相关的数量的派生对象。 派生对象的最常见用途是计算纹理采样的详细程度，在各向异性过滤的情况下，其用途是选择各向异性的轴上的样本。 通常，硬件实现同时对多个像素（例如 2x2 网格）运行像素着色器，以便在像素着色器中计算得出的数量的派生对象可以合理地近似为相邻像素中的同一执行点上的值的增量。

## <a name="span-idinputsspanspan-idinputsspanspan-idinputsspaninputs"></a><span id="Inputs"></span><span id="inputs"></span><span id="INPUTS"></span>输入


如果管道在配置时未使用几何着色器，像素着色器限制为 16 个 32 位 4 分量输入。 否则，像素着色器可能占用多达 32 个 32 位 4 分量输入。

像素着色器输入数据包含顶点属性（可以在使用或不使用透视校正的情况下内插），或者也可以被视为每基元常量。 像素着色器输入根据声明的内插模式从正在光栅化的基元的顶点属性内插。 如果基元在光栅化之前被剪裁，内插模式在剪裁过程中也受支持。

顶点属性在像素着色器中心位置被内插（或评估）。 在[参数](https://msdn.microsoft.com/library/windows/desktop/bb509606)或[输入结构](https://msdn.microsoft.com/library/windows/desktop/bb509668)中，像素着色器属性内插模式将逐个元素地在输入寄存器声明中声明。 可以线性内插属性，也可以利用质心采样内插属性。 请参阅[光栅化规则](rasterization-rules.md)中的“多重采样抗锯齿时的质心采样”。 质心计算仅在多重采样期间相关，以包含像素由基元覆盖但像素中心可能并非如此的情况；质心计算尽可能靠近（非覆盖）像素中心进行。

输入也可以用[系统值语义](https://msdn.microsoft.com/library/windows/desktop/bb509647)声明，该语意将标记由其他管道阶段使用的参数。 例如，像素位置应标记为与 SV\_位置语义。 [输入组装器 (IA) 阶段](input-assembler-stage--ia-.md)可能导致像素着色器产生一个标量 (使用 SV\_PrimitiveID);[光栅器 (RS) 阶段](rasterizer-stage--rs-.md)还可以为像素着色器 （使用 SV生成一个标量\_IsFrontFace)。

## <a name="span-idoutputsspanspan-idoutputsspanspan-idoutputsspanoutputs"></a><span id="Outputs"></span><span id="outputs"></span><span id="OUTPUTS"></span>输出


像素着色器可输出多达 8 个 32 位 4 分量颜色，如果像素被弃用，则不会生成颜色。 像素着色器输出寄存器分量必须先声明才能使用；允许每个寄存器使用一个独特的输出写入掩码。

使用深度写入启用状态（在[输出合并 (OM) 阶段](output-merger-stage--om-.md)）可控制深度数据被写入深度缓冲区（还是使用弃用指令来弃用该像素的数据）。 像素着色器也可以输出的深度测试的可选的 32 位、 1 组件、 浮点、 深度值 (使用 SV\_语义深度)。 深度值是 oDepth 寄存器中的输出，它将替换用于深度测试的内插深度值（假定已启用深度测试）。 你无法在使用固定函数深度与使用着色器 oDepth 之间动态更改。

像素着色器无法输出模具值。

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>相关主题


[图形管道](graphics-pipeline.md)

 

 




