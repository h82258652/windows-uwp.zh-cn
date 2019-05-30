---
title: 外壳着色器 (HS) 阶段
description: 外壳着色器 (HS) 阶段是分割阶段之一，其有效地将模型的单个表面分解为许多三角形。
ms.assetid: C62F6F15-CAD7-4C72-9735-00762E346C4C
keywords:
- 外壳着色器 (HS) 阶段
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: dd45408c484534009bf632eb81be11e6ef4d58b7
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/29/2019
ms.locfileid: "66370579"
---
# <a name="hull-shader-hs-stage"></a>外壳着色器 (HS) 阶段

外壳着色器 (HS) 阶段是分割阶段之一，其有效地将模型的单个表面分解为许多三角形。 外壳着色器 (HS) 阶段产生对应于每个输入修补程序（四边形、三角形或线条）的几何图形修补程序（和修补程序常量）。 每个修补程序调用一次外壳着色器，它将定义低阶表面的输入控制点转换为构成修补程序的控制点。 它还执行一些按每个修补程序进行的计算，以便为[细化器 (TS) 阶段](tessellator-stage--ts-.md)和[域着色器 (DS) 阶段](domain-shader-stage--ds-.md)提供数据。

## <a name="span-idpurposeandusesspanspan-idpurposeandusesspanspan-idpurposeandusesspanpurpose-and-uses"></a><span id="Purpose_and_uses"></span><span id="purpose_and_uses"></span><span id="PURPOSE_AND_USES"></span>用途，并使用


![外壳着色器阶段图示](images/d3d11-hull-shader.png)

三个分割阶段一起将高阶图面（保留了模型简易度和效率）转换为多个三角形，从而在图形管道中进行精确渲染。 分割阶段包括外壳着色器 (HS) 阶段、[细化器 (TS) 阶段](tessellator-stage--ts-.md)和[域着色器 (DS) 阶段](domain-shader-stage--ds-.md)。

外壳着色器 (HS) 阶段是可编程着色器阶段。 使用 HLSL 函数实现外壳着色器。

外壳着色器在两个阶段中运行：控制点阶段和修补程序常量阶段，它们由硬件并行地运行。 HLSL 编译器提取外壳着色器中的并行度，并将其编码为驱动硬件的字节码。

-   每个控制点运行一次控制点阶段，该阶段会读取修补程序的控制点，并生成一个输出控制点（通过 **ControlPointID** 标识）。
-   每个修补程序运行一次修补程序常量阶段，以生成边缘细化因素和其他每个修补程序的常量。 在内部，许多修补程序常量阶段可以同时运行。 修补程序常量阶段对所有输入和输出控制点具有只读访问权限。

## <a name="span-idinputspanspan-idinputspanspan-idinputspaninput"></a><span id="Input"></span><span id="input"></span><span id="INPUT"></span>输入


在 1 到 32 个输入控制点之间，它们一起定义了低阶表面。

-   外壳着色器声明[细化器 (TS) 阶段](tessellator-stage--ts-.md)所需的状态。 这包括控制点的数量、修补程序面的类型和在分割时使用的分区类型等信息。 此信息作为声明显示，通常位于着色器代码的前面。
-   细化因素决定细分每个修补程序的量。

## <a name="span-idoutputspanspan-idoutputspanspan-idoutputspanoutput"></a><span id="Output"></span><span id="output"></span><span id="OUTPUT"></span>Output


在 1 到 32 个输出控制点之间，它们共同构成修补程序。

-   无论细化因素的数量是多少，着色器输出都在 1 到 32 个控制点之间。 来自外壳着色器的控制点输出可由域着色器阶段使用。 修补程序常量数据可由域着色器使用。 细化因素可由[细化器 (TS) 阶段](tessellator-stage--ts-.md)和[域着色器 (DS) 阶段](domain-shader-stage--ds-.md)使用。
-   如果外壳着色器将任何边缘细化因素设置为 ≤0 或 NaN，则修补程序将被剔除（省略）。 因此，细化器阶段可能运行也可能不运行，域着色器将不运行，并且不会为该修补程序生成可见输出。

## <a name="span-idexamplespanspan-idexamplespanspan-idexamplespanexample"></a><span id="Example"></span><span id="example"></span><span id="EXAMPLE"></span>示例


```hlsl
[patchsize(12)]
[patchconstantfunc(MyPatchConstantFunc)]
MyOutPoint main(uint Id : SV_ControlPointID,
     InputPatch<MyInPoint, 12> InPts)
{
     MyOutPoint result;
     
     ...
     
     result = TransformControlPoint( InPts[Id] );

     return result;
}
```

请参阅[如何：创建外壳着色器](https://docs.microsoft.com/windows/desktop/direct3d11/direct3d-11-advanced-stages-hull-shader-create)。

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>相关主题


[图形管道](graphics-pipeline.md)

 

 




