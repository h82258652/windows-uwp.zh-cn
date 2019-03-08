---
title: 域着色器 (DS) 阶段
description: 域着色器 (DS) 阶段计算输出修补程序中细分点的顶点位置；它计算与每个域样本对应的顶点位置。
ms.assetid: 673CC04A-A74F-495F-AFB7-49157538749C
keywords:
- 域着色器 (DS) 阶段
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: bbde90d848d3bc8fb18a5ecf370c85121adc02f6
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2019
ms.locfileid: "57620492"
---
# <a name="domain-shader-ds-stage"></a>域着色器 (DS) 阶段


域着色器 (DS) 阶段计算输出修补程序中细分点的顶点位置；它计算与每个域样本对应的顶点位置。 对每个细化器阶段输出点运行一次域着色器，并且该着色器具有对外壳着色器输出修补程序、输出修补程序常量以及细化器阶段输出 UV 坐标的只读权限。

## <a name="span-idpurposeandusesspanspan-idpurposeandusesspanspan-idpurposeandusesspanpurpose-and-uses"></a><span id="Purpose_and_uses"></span><span id="purpose_and_uses"></span><span id="PURPOSE_AND_USES"></span>用途，并使用


域着色器 (DS) 阶段基于[外壳着色器 (HS) 阶段](hull-shader-stage--hs-.md)和[细化器 (TS) 阶段](tessellator-stage--ts-.md)中的输入，输出修补程序中细分点的顶点位置。

![域着色器阶段的图示](images/d3d11-domain-shader.png)

## <a name="span-idinputspanspan-idinputspanspan-idinputspaninput"></a><span id="Input"></span><span id="input"></span><span id="INPUT"></span>输入


-   域着色器使用[外壳着色器 (HS) 阶段](hull-shader-stage--hs-.md)的输出控制点。 外壳着色器输出包括：
    -   控制点。
    -   修补程序常量数据。
    -   细化因素。 例如，细化因素可以包括固定函数细化器使用的值以及原始值（例如在被整数细化舍入前），这有助于加快几何过渡。
-   对[细化器 (TS) 阶段](tessellator-stage--ts-.md)的每个输出坐标调用一次域着色器。

## <a name="span-idoutputspanspan-idoutputspanspan-idoutputspanoutput"></a><span id="Output"></span><span id="output"></span><span id="OUTPUT"></span>Output


-   域着色器 (DS) 阶段输出输出修补程序中细分点的顶点位置。

域着色器完成后，细分完成，且管道数据继续到下一个管道阶段，比如[几何着色器 (GS) 阶段](geometry-shader-stage--gs-.md)和[像素着色器 (PS) 阶段](pixel-shader-stage--ps-.md)。 细化活动（导致未定义行为，调试层将抱怨这种情况）时，几何着色器预计邻接基元（例如每个三角形 6 个顶点）无效。

## <a name="span-idexamplespanspan-idexamplespanspan-idexamplespanexample"></a><span id="Example"></span><span id="example"></span><span id="EXAMPLE"></span>示例


```
void main( out    MyDSOutput result, 
           float2 myInputUV : SV_DomainPoint, 
           MyDSInput DSInputs,
           OutputPatch<MyOutPoint, 12> ControlPts, 
           MyTessFactors tessFactors)
{
     ...

     result.Position = EvaluateSurfaceUV(ControlPoints, myInputUV);
}
```

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>相关主题


[图形管道](graphics-pipeline.md)

 

 




