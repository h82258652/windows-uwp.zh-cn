---
title: 计算管道
description: Direct3D 计算管道旨在处理大部分可与图形管道并行完成的计算。
ms.assetid: 355B66C6-C0DF-47BA-A9C9-7AFA50B5B614
author: michaelfromredmond
ms.author: mithom
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 91c95019c327f39a58a7397a66f9d4bbc88f843d
ms.sourcegitcommit: 753e0a7160a88830d9908b446ef0907cc71c64e7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/30/2018
ms.locfileid: "5758075"
---
# <a name="compute-pipeline"></a>计算管道


\[在商业发行之前可能会实质性修改与预发布产品相关的一些信息。 Microsoft 不对此处提供的信息作任何明示或默示的担保。\]


Direct3D 计算管道旨在处理大部分可与图形管道并行完成的计算。 计算管道仅需几步即可完成，其中数据在可编程计算着色器阶段从输入流向输出。

| | |
|-|-|
|用途|与其他可编程着色器一样，[计算着色器 (CS) 阶段](compute-shader-stage--cs-.md)通过 HLSL 设计并实现。 计算着色器提供常规目的的高速计算并利用图形处理单元 (GPU) 上的大量并行处理器。 计算着色器提供内存共享和线程同步功能，允许采用更有效的并行编程方法。|
|输入|与其他可编程着色器不同，输入的定义是抽象的。 本质上，输入可以是一维、二维或三维，其决定了计算着色器要执行的调用数量。 它还可以定义一组调用要读取的共享数据。|
|输出|计算着色器的输出数据可能会高度变化，需要计算的数据时，其可与图形呈现管道一起同步。|
| | |




<!---
<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left">Purpose</td>
<td align="left">Like other programmable shaders, <a href="#compute-shader-stage--cs-.md">Compute Shader (CS) stage</a> is designed and implemented with HLSL. A compute shader provides high-speed general purpose computing and takes advantage of the large numbers of parallel processors on the graphics processing unit (GPU). The compute shader provides memory sharing and thread synchronization features to allow more effective parallel programming methods.</td>
</tr>
<tr class="even">
<td align="left">Input</td>
<td align="left">Unlike other programmable shaders, the definition of input is abstract. The input can be one, two or three-dimensional in nature, determining the number of invocations of the compute shader to execute. It is possible to define shared data for one set of invocations to read.</td>
</tr>
<tr class="odd">
<td align="left">Output</td>
<td align="left">Output data from the compute shader, which can be highly varied, can be synchronized with the graphics rendering pipeline when the computed data is required.</td>
</tr>
</tbody>
</table>
-->

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>相关主题


[Direct3D 图形学习指南](index.md)

 

 
