---
title: "使用系统生成的值"
description: "系统生成的值由输入装配器 (IA) 阶段（基于用户提供的输入语义）生成，用于在一定程度上提高着色器运算的效率。"
ms.assetid: C7CBA81D-68CA-4E9A-95E3-8185C280C843
keywords: "使用系统生成的值"
author: PeterTurcan
ms.author: pettur
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
ms.openlocfilehash: f2b9918161f99a6a841e57d9b2705093eb85809b
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="span-iddirect3dconceptsusingsystem-generatedvaluesspanusing-system-generated-values"></a><span id="direct3dconcepts.using_system-generated_values"></span>使用系统生成的值


系统生成的值由[输入装配器 (IA) 阶段](input-assembler-stage--ia-.md)（基于用户提供的输入[语义](https://msdn.microsoft.com/library/windows/desktop/bb509647)）生成，用于在一定程度上提高着色器运算的效率。 通过附加数据，如实例 ID（对[顶点着色器 (VS) 阶段](vertex-shader-stage--vs-.md)可见）、顶点 ID（对 VS 可见）或基元 ID（对[几何着色器 (GS) 阶段](geometry-shader-stage--gs-.md)/[像素着色器 (PS) 阶段](pixel-shader-stage--ps-.md)可见），后续的着色器阶段可以查找这些系统值，从而优化相应阶段内的处理。

例如，VS 阶段可能查找实例 ID 以便为着色器获取更多每顶点数据，或者执行其他操作；GS 和 PS 阶段可能使用基元 ID 以相同的方式获取每基元数据。

## <a name="span-idvertexidspanspan-idvertexidspanspan-idvertexidspanvertexid"></a><span id="VertexID"></span><span id="vertexid"></span><span id="VERTEXID"></span>VertexID


顶点 ID 由每个着色器阶段用于标识每个顶点。 它是默认值为 0 的 32 位无符号整数。 它在基元由[输入装配器 (IA) 阶段](input-assembler-stage--ia-.md)处理时分配给顶点。 将顶点 ID 语义附加到着色器输入声明以通知 IA 阶段生成每顶点 ID。

IA 会将顶点 ID 添加到每个顶点以供着色器阶段使用。 每调用一次绘制，顶点 ID 就增加 1。 对于所有已建立索引的绘制调用，计数将重置回起始值。 如果顶点 ID 溢出（超过 2³²– 1），它将回到 0。

对于所有基元类型，顶点都有与之关联的顶点 ID（无论邻近度如何）。

## <a name="span-idprimitiveidspanspan-idprimitiveidspanspan-idprimitiveidspanprimitiveid"></a><span id="PrimitiveID"></span><span id="primitiveid"></span><span id="PRIMITIVEID"></span>PrimitiveID


基元 ID 由每个着色器阶段用于标识每个基元。 它是默认值为 0 的 32 位无符号整数。 它在基元由[输入装配器 (IA) 阶段](input-assembler-stage--ia-.md)处理时分配给基元。 要通知 IA 阶段生成基元 ID，请将基元 ID 语义附加到着色器输入声明。

IA 阶段会将基元 ID 添加到每个基元以供[几何着色器 (GS) 阶段](geometry-shader-stage--gs-.md)或[顶点着色器 (VS) 阶段](vertex-shader-stage--vs-.md)使用（取在 IA 阶段后先进入活动状态的阶段）。 每调用一次已建立索引的绘制，基元 ID 就增加 1，但是，每当新实例开始时，基元 ID 就会重置为 0。 所有其他绘制调用都不会更改实例 ID 的值。 如果实例 ID 溢出（超过 2³²– 1），它将回到 0。

[像素着色器 (PS) 阶段](pixel-shader-stage--ps-.md)对于基元 ID 没有单独的输入；但是，指定基元 ID 的任何像素着色器输入都使用恒定的内插模式。

不支持为相邻的基元自动生成基元 ID。 对于带邻近度的基元类型（如带邻近度的三角形），基元 ID 仅为内部基元（非相邻基元）保留，就像不带邻近度的三角形带中的基元集一样。

## <a name="span-idinstanceidspanspan-idinstanceidspanspan-idinstanceidspaninstanceid"></a><span id="InstanceID"></span><span id="instanceid"></span><span id="INSTANCEID"></span>InstanceID


实例 ID 由每个着色器阶段用于标识当前正在处理的几何图形的实例。 它是默认值为 0 的 32 位无符号整数。

如果顶点着色器输入声明包含实例 ID 语义，[输入装配器 (IA) 阶段](input-assembler-stage--ia-.md)会将实例 ID 添加到每个顶点。 每调用一次已建立索引的绘制，实例 ID 就增加 1。 所有其他绘制调用都不会更改实例 ID 的值。 如果实例 ID 溢出（超过 2³²– 1），它将回到 0。

## <a name="span-idexamplespanspan-idexamplespanspan-idexamplespanexample"></a><span id="Example"></span><span id="example"></span><span id="EXAMPLE"></span>示例


下图显示了如何将系统值附加到[输入装配器 (IA) 阶段](input-assembler-stage--ia-.md)中的实例化三角形带。

![实例化三角形带的系统值的图示](images/d3d10-ia-example.png)

以下表显示了为同一三角形带的两个实例生成的系统值。 第一个实例（实例 U）显示为蓝色，第二个实例（实例 V）显示为绿色。 实线连接基元中的顶点，虚线连接相邻的顶点。

下表显示了实例 U 的系统生成的值。

| 顶点数据    | C,U | D,U | E,U | F,U | G,U | H,U | I,U | J,U | K,U | L,U |
|----------------|-----|-----|-----|-----|-----|-----|-----|-----|-----|-----|
| **VertexID**   | 0   | 1   | 2   | 3   | 4   | 5   | 6   | 7   | 8   | 9   |
| **InstanceID** | 0   | 0   | 0   | 0   | 0   | 0   | 0   | 0   | 0   | 0   |

 

三角形带实例 U 具有 3 个三角形基元，其系统生成值如下所示：

|                 |     |     |     |
|-----------------|-----|-----|-----|
| **PrimitiveID** | 0   | 1   | 2   |
| **InstanceID**  | 0   | 0   | 0   |

 

下表显示了实例 V 的系统生成的值。

| 顶点数据    | C,V | D,V | E,V | F,V | G,V | H,V | I,V | J,V | K,V | L,V |
|----------------|-----|-----|-----|-----|-----|-----|-----|-----|-----|-----|
| **VertexID**   | 0   | 1   | 2   | 3   | 4   | 5   | 6   | 7   | 8   | 9   |
| **InstanceID** | 1   | 1   | 1   | 1   | 1   | 1   | 1   | 1   | 1   | 1   |

 

三角形带实例 V 具有 3 个三角形基元，其系统生成值如下所示：

|                 |     |     |     |
|-----------------|-----|-----|-----|
| **PrimitiveID** | 0   | 1   | 2   |
| **InstanceID**  | 1   | 1   | 1   |

 

[输入装配器 (IA) 阶段](input-assembler-stage--ia-.md)将生成 ID（顶点、基元和实例）；另请注意，每个实例都将获得一个唯一的实例 ID。 该数据将以条带切割结尾，这将分隔三角形带的每个实例。

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>相关主题


[输入装配器 (IA) 阶段](input-assembler-stage--ia-.md)

 

 




