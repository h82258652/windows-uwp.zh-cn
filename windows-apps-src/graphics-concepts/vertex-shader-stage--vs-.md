---
title: 顶点着色器 (VS) 阶段
description: 顶点着色器 (VS) 阶段处理顶点，通常执行诸如转换、换肤以及照明之类的操作。 顶点着色器获取一个输入顶点并生成一个输出顶点。
ms.assetid: 5133C4BB-B4E6-4697-9276-F718AD44869C
keywords:
- 顶点着色器 (VS) 阶段
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: ca3b5e230270b46b7cb2709d4bfa06c4c51d0224
ms.sourcegitcommit: b4c502d69a13340f6e3c887aa3c26ef2aeee9cee
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/03/2018
ms.locfileid: "8478771"
---
# <a name="vertex-shader-vs-stage"></a>顶点着色器 (VS) 阶段


顶点着色器 (VS) 阶段处理顶点，通常执行诸如转换、换肤以及照明之类的操作。 顶点着色器获取一个输入顶点并生成一个输出顶点。

## <a name="span-idpurposeandusesspanspan-idpurposeandusesspanspan-idpurposeandusesspanpurpose-and-uses"></a><span id="Purpose_and_uses"></span><span id="purpose_and_uses"></span><span id="PURPOSE_AND_USES"></span>用途和用法


顶点着色器 (VS) 阶段用于单独的每顶点处理，例如：

-   转换
-   换肤
-   变形
-   每顶点照明

顶点着色器阶段是一个可编程着色器阶段；它在[图形管道](graphics-pipeline.md)图中显示为圆块。 此着色器阶段使用着色器模型 4.0 [常用着色器核心](https://msdn.microsoft.com/library/windows/desktop/bb509580)。

顶点着色器 (VS) 阶段处理输入装配器中的顶点。 顶点着色器始终在单个输入顶点上运行并生成单个输出顶点。 顶点着色器阶段必须始终处于活动状态，管道才能执行。 如果不需要顶点修改或转换，则必须创建直通顶点着色器并将其设置到管道中。

每个顶点着色器输入顶点可以由 16 个 32 位矢量（每个矢量最多有 4 个分量）组成。 每个输出顶点可以由多达 16 个 32 位 4 分量矢量组成。 所有顶点着色器都必须至少有一个输入和一个输出，该输入和输出可能仅仅是一个标量值。

顶点着色器阶段可能使用来自输入装配器的两个系统生成值：VertexID 和 InstanceID（请参阅“系统值和语义”）。 由于 VertexID 和 InstanceID 在顶点级别都有意义，由硬件生成的 ID 只能馈送到了解这些 ID 的第一个阶段中，这些 ID 值只能馈送到顶点着色器阶段中。

顶点着色器始终在所有顶点上运行，包括带邻近度的输入基元拓扑中的相邻顶点。 已执行顶点着色器的次数可通过 VSInvocations 管道统计信息从 CPU 中查询。

顶点着色器可执行不需要屏幕空间派生对象的加载和纹理采样操作（使用 HLSL 内部函数：[Sample (DirectX HLSL Texture Object)](https://msdn.microsoft.com/library/windows/desktop/bb509695)、[SampleCmpLevelZero (DirectX HLSL Texture Object)](https://msdn.microsoft.com/library/windows/desktop/bb509697) 和 [SampleGrad (DirectX HLSL Texture Object)](https://msdn.microsoft.com/library/windows/desktop/bb509698)）。

## <a name="span-idinputspanspan-idinputspanspan-idinputspaninput"></a><span id="Input"></span><span id="input"></span><span id="INPUT"></span>输入


具有 VertexID 和 InstanceID 系统生成值的单个顶点。 每个顶点着色器输入顶点可以由 16 个 32 位矢量（每个矢量最多有 4 个分量）组成。

## <a name="span-idoutputspanspan-idoutputspanspan-idoutputspanoutput"></a><span id="Output"></span><span id="output"></span><span id="OUTPUT"></span>输出


单个顶点。 每个输出顶点可以由多达 16 个 32 位 4 分量矢量组成。

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>相关主题


[图形管道](graphics-pipeline.md)

 

 




