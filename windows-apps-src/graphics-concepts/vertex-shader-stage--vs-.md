---
title: 顶点着色器 (VS) 阶段
description: 顶点着色器 (VS) 阶段处理顶点，通常执行诸如转换、换肤以及照明之类的操作。 顶点着色器获取一个输入顶点并生成一个输出顶点。
ms.assetid: 5133C4BB-B4E6-4697-9276-F718AD44869C
keywords:
- 顶点着色器 (VS) 阶段
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 5893719e43314eb15c684948a31de5a025a926fc
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/29/2019
ms.locfileid: "66370831"
---
# <a name="vertex-shader-vs-stage"></a>顶点着色器 (VS) 阶段


顶点着色器 (VS) 阶段处理顶点，通常执行诸如转换、换肤以及照明之类的操作。 顶点着色器获取一个输入顶点并生成一个输出顶点。

## <a name="span-idpurposeandusesspanspan-idpurposeandusesspanspan-idpurposeandusesspanpurpose-and-uses"></a><span id="Purpose_and_uses"></span><span id="purpose_and_uses"></span><span id="PURPOSE_AND_USES"></span>用途，并使用


顶点着色器 (VS) 阶段用于单独的每顶点处理，例如：

-   转换
-   换肤
-   变形
-   每顶点照明

顶点着色器阶段是一个可编程着色器阶段；它在[图形管道](graphics-pipeline.md)图中显示为圆块。 此着色器阶段使用着色器模型 4.0 [常用着色器核心](https://docs.microsoft.com/windows/desktop/direct3dhlsl/dx-graphics-hlsl-common-core)。

顶点着色器 (VS) 阶段处理输入装配器中的顶点。 顶点着色器始终在单个输入顶点上运行并生成单个输出顶点。 顶点着色器阶段必须始终处于活动状态，管道才能执行。 如果不需要顶点修改或转换，则必须创建直通顶点着色器并将其设置到管道中。

每个顶点着色器输入顶点可以由 16 个 32 位矢量（每个矢量最多有 4 个分量）组成。 每个输出顶点可以由多达 16 个 32 位 4 分量矢量组成。 所有顶点着色器都必须至少有一个输入和一个输出，该输入和输出可能仅仅是一个标量值。

顶点着色器阶段可以使用两个系统生成的值从输入装配器：VertexID 和实例 Id （请参阅系统值和语义）。 由于 VertexID 和 InstanceID 在顶点级别都有意义，由硬件生成的 ID 只能馈送到了解这些 ID 的第一个阶段中，这些 ID 值只能馈送到顶点着色器阶段中。

顶点着色器始终在所有顶点上运行，包括带邻近度的输入基元拓扑中的相邻顶点。 已执行顶点着色器的次数可通过 VSInvocations 管道统计信息从 CPU 中查询。

顶点着色器可以执行负载和纹理采样操作不需要的屏幕空间派生类 (使用 HLSL 内部函数：[示例 （DirectX HLSL 纹理对象）](https://docs.microsoft.com/windows/desktop/direct3dhlsl/dx-graphics-hlsl-to-sample)， [SampleCmpLevelZero （DirectX HLSL 纹理对象）](https://docs.microsoft.com/windows/desktop/direct3dhlsl/dx-graphics-hlsl-to-samplecmplevelzero)，和[SampleGrad （DirectX HLSL 纹理对象）](https://docs.microsoft.com/windows/desktop/direct3dhlsl/dx-graphics-hlsl-to-samplegrad))。

## <a name="span-idinputspanspan-idinputspanspan-idinputspaninput"></a><span id="Input"></span><span id="input"></span><span id="INPUT"></span>输入


具有 VertexID 和 InstanceID 系统生成值的单个顶点。 每个顶点着色器输入顶点可以由 16 个 32 位矢量（每个矢量最多有 4 个分量）组成。

## <a name="span-idoutputspanspan-idoutputspanspan-idoutputspanoutput"></a><span id="Output"></span><span id="output"></span><span id="OUTPUT"></span>Output


单个顶点。 每个输出顶点可以由多达 16 个 32 位 4 分量矢量组成。

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>相关主题


[图形管道](graphics-pipeline.md)

 

 




