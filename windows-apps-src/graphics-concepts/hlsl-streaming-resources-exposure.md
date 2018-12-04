---
title: HLSL 流式资源暴露
description: 支持 Shader 模型 5 中的流式资源需要特定 Microsoft 高级着色器语言 (HLSL) 语法。
ms.assetid: 00A40D82-0565-43DC-82AB-0675B7E772E3
keywords:
- HLSL 流式资源暴露
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 00d6c16ecaa64abf7d83154fdb864671dbff3eae
ms.sourcegitcommit: b4c502d69a13340f6e3c887aa3c26ef2aeee9cee
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/03/2018
ms.locfileid: "8474718"
---
# <a name="hlsl-streaming-resources-exposure"></a>HLSL 流式资源暴露


支持 [Shader 模型 5](https://msdn.microsoft.com/library/windows/desktop/ff471356) 中的流式资源需要特定 Microsoft 高级着色器语言 (HLSL) 语法。

仅能在支持流式资源的设备上使用 Shader 模型 5 的 HLSL 语法。 下表中用于流式资源的每个相关 HLSL 方法接受一个（反馈）或两个（以此顺序的固定和反馈）其他的可选参数。 例如，**Sample** 方法是：

**Sample(sampler, location \[, offset \[, clamp \[, feedback\] \] \])**

**Sample** 方法的一个示例是 [**Texture2D.Sample(S,float,int,float,uint)**](https://msdn.microsoft.com/library/windows/desktop/dn393787)。

offset、clamp 和 feedback 参数都是可选的。 你必须指定所需的所有可选参数，这与默认函数参数的 C ++ 规则一致。 例如，如果需要 feedback 状态，则需要向 **Sample** 显式提供 offset 和 clamp 参数，即使在逻辑上可能不需要它们。

clamp 参数是标量浮点值。 clamp=0.0f 的文本值表明 clamp 操作未执行。

feedback 参数是 **uint** 变量，可将其提供给内存访问查询内部 [**CheckAccessFullyMapped**](https://msdn.microsoft.com/library/windows/desktop/dn292083) 函数。 不得修改或解释 feedback 参数的值；但是编译器不提供任何高级分析和诊断来检测是否修改了值。

以下是 [**CheckAccessFullyMapped**](https://msdn.microsoft.com/library/windows/desktop/dn292083) 的语法：

**bool CheckAccessFullyMapped(in uint FeedbackVar);**

[**CheckAccessFullyMapped**](https://msdn.microsoft.com/library/windows/desktop/dn292083) 会解释 *FeedbackVar* 的值，如果正在访问的所有数据已映射到资源中，则返回 true；否则，**CheckAccessFullyMapped** 返回 false。

如果存在 clamp 或 feedback 参数，则编译器将发出基本指令的变体。 例如，流式资源的示例会生成 `sample_cl_s` 指令。

如果既不指定 clamp 也不指定 feedback，则编译器发出基本指令，使得当前行为不发生变化。

clamp 值 0.0f 表示不执行 clamp；因此，驱动程序编译器可以进一步根据目标硬件定制指令。 如果 feedback 是指令中的 NULL 寄存器，则不使用 feedback；因此，驱动程序编译器可以进一步根据目标体系结构定制指令。

如果 HLSL 编译器推断出 clamp 为 0.0f 并且 feedback 未使用，则编译器发出相应的基本指令（例如，`sample` 而不是 `sample_cl_s`）。

如果流式资源访问由若干构成字节代码指令组成，例如，对于结构化资源，编译器通过 OR 操作聚合单个 feedback 值以产生最终 feedback 值。 因此，你会看到这种复杂访问的单个 feedback 值。

以下是更改为支持 feedback 和/或 clamp 的 HLSL 方法的摘要表。 这些都适用于所有尺寸的平铺和非流式资源。 非流式资源总是显示为完全映射。

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left"><a href="https://msdn.microsoft.com/library/windows/desktop/ff471359">HLSL 对象</a> </th>
<th align="left">具有 feedback 选项 (*) 的内部方法 - 也具有 clamp 选项</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p>[RW]Texture2D</p>
<p>[RW]Texture2DArray</p>
<p>TextureCUBE</p>
<p>TextureCUBEArray</p></td>
<td align="left"><p>Gather</p>
<p>GatherRed</p>
<p>GatherGreen</p>
<p>GatherBlue</p>
<p>GatherAlpha</p>
<p>GatherCmp</p>
<p>GatherCmpRed</p>
<p>GatherCmpGreen</p>
<p>GatherCmpBlue</p>
<p>GatherCmpAlpha</p></td>
</tr>
<tr class="even">
<td align="left"><p>[RW]Texture1D</p>
<p>[RW]Texture1DArray</p>
<p>[RW]Texture2D</p>
<p>[RW]Texture2DArray</p>
<p>[RW]Texture3D</p>
<p>TextureCUBE</p>
<p>TextureCUBEArray</p></td>
<td align="left"><p>Sample*</p>
<p>SampleBias*</p>
<p>SampleCmp*</p>
<p>SampleCmpLevelZero</p>
<p>SampleGrad*</p>
<p>SampleLevel</p></td>
</tr>
<tr class="odd">
<td align="left"><p>[RW]Texture1D</p>
<p>[RW]Texture1DArray</p>
<p>[RW]Texture2D</p>
<p>Texture2DMS</p>
<p>[RW]Texture2DArray</p>
<p>Texture2DArrayMS</p>
<p>[RW]Texture3D</p>
<p>[RW]Buffer</p>
<p>[RW]ByteAddressBuffer</p>
<p>[RW]StructuredBuffer</p></td>
<td align="left">Load</td>
</tr>
</tbody>
</table>

 

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>相关主题


[对流式资源的管道访问](pipeline-access-to-streaming-resources.md)

 

 




