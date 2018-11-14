---
title: 数据类型转换
description: 以下部分介绍 Direct3D 如何处理数据类型之间的转换。
ms.assetid: B50AB8DE-CAED-465B-B18C-81F3A984B8AC
keywords:
- 数据类型转换
author: michaelfromredmond
ms.author: mithom
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: a6ceb1e779f8622d3e358bc131b21f6ec66ac2f8
ms.sourcegitcommit: f2c9a050a9137a473f28b613968d5782866142c6
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/09/2018
ms.locfileid: "6283279"
---
# <a name="data-type-conversion"></a>数据类型转换


以下部分介绍 Direct3D 如何处理数据类型之间的转换。

## <a name="span-iddatatypeterminologyspanspan-iddatatypeterminologyspanspan-iddatatypeterminologyspandata-type-terminology"></a><span id="Data_Type_Terminology"></span><span id="data_type_terminology"></span><span id="DATA_TYPE_TERMINOLOGY"></span>数据类型术语


下面一组术语在随后用于描述各种格式转换。

| 术语  | 定义                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |
|-------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| SNORM | 有符号的规范化整数，即，对于 n 位 2 的补数而言，最大值为 1.0f（例如，5 位值 01111 映射到 1.0f），最小值为 -1.0f（例如，5 位值 10000 映射到 -1.0f）。 此外，第二小的数字映射到 -1.0f（例如，5 位值 10001 映射到 -1.0f）。 因此，-1.0f 有两个整数表示形式。 0.0f 有单个表示形式，1.0f 有单个表示形式。 这会导致范围 (-1.0f...0.0f) 中间距均匀的浮点值有一组整数表示形式，且范围 (0.0f...1.0f) 中的数字还有一组补充表示形式。 |
| UNORM | 无符号的规范化整数，即对于 n 位数字而言，所有 0 都为 0.0f，且所有 1 都为 1.0f。 对从 0.0f 至 1.0f 的均匀分布的浮点值序列进行了表示。 例如，2 位 UNORM 表示 0.0f、1/3、2/3 和 1.0f。                                                                                                                                                                                                                                                                                                                                                                                                                                |
| SINT  | 有符号整数。 2 的补充整数。 例如，3 位 SINT 表示整数值 -4、-3、-2、-1、0、1、2、3。                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  |
| UINT  | 无符号整数。 例如，3 位 UINT 表示整数值 0、1、2、3、4、5、6、7。                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |
| FLOAT | 任意表示形式中由 Direct3D 定义的浮点值。                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              |
| SRGB  | 与 UNORM 类似，对于其中的 n 位数字而言，所有 0 都意味着 0.0f，且所有 1 都意味着 1.0f。 但是，和 UNORM 不同，在 SRGB 中，所有 0 至所有 1 之间的有符号整数编码序列表示 0.0f 至 1.0f 之间数字的浮点解释中的非线性进度。 大致上说，如果此非线性进度 (SRGB) 显示为颜色序列，在“average”查看条件下，它将在“average”显示器上对“average”观察者显示为亮度级别的线性渐变。 有关完整的详细信息，请参考 IEC（国际电工委员会）的 IEC 61996-2-1，SRGB 颜色标准。                |

 

上述条款经常用作“格式名称修饰符”，它们会描述数据在内存中的布局方式，以及要在进入或离开管道单元的内存（例如着色器）中的传输路径中执行何种转换（可能包含筛选）。

## <a name="span-idfloatingpointconversionspanspan-idfloatingpointconversionspanspan-idfloatingpointconversionspanfloating-point-conversion"></a><span id="Floating_Point_Conversion"></span><span id="floating_point_conversion"></span><span id="FLOATING_POINT_CONVERSION"></span>浮点转换


无论不同表示形式之间的浮点转换何时发生（包括进入或离开非浮点表示形式），以下规则均适用。

### <a name="span-idconververtingfromahigherrangerepresentationtoalowerrangerepresentationspanspan-idconververtingfromahigherrangerepresentationtoalowerrangerepresentationspanspan-idconververtingfromahigherrangerepresentationtoalowerrangerepresentationspanconververting-from-a-higher-range-representation-to-a-lower-range-representation"></a><span id="Conververting_from_a_higher_range_representation_to_a_lower_range_representation"></span><span id="conververting_from_a_higher_range_representation_to_a_lower_range_representation"></span><span id="CONVERVERTING_FROM_A_HIGHER_RANGE_REPRESENTATION_TO_A_LOWER_RANGE_REPRESENTATION"></span>从上限范围表示形式转换为下限范围表示形式

-   向零舍入在转换为其他浮点格式时使用。 如果目标是整数固定点格式，则使用最近舍入，除非此转换被明确记录为使用其他舍入行为，例如，对 FLOAT 至 SNORM、FLOAT 至 UNORM 或 FLOAT 至 SRGB 进行最近舍入。 其他例外还有 ftoi 和 ftou 着色器指令，它们使用向零舍入。 最后，纹理采样器和光栅器使用的浮动到固定的转换具有以最小单位从无限精确的理想值开始测量的指定容错。
-   对于大于下限范围目标格式的动态范围的源值（例如， 大型 32 位浮点值会写入 16 位浮动 RenderTarget），会生成最大可表示（已适当添加符号）值，不包含有符号的无穷大值（由于上文所述的向零舍入）。
-   如果下限范围格式中存在 NaN 表示形式，上限范围格式中的 NaN 将转换为下限范围格式中的 NaN 表示形式。 如果下限格式没有 NaN 表示形式，结果为 0。
-   如果可用，上限范围格式中的 INF 将转换为下限范围格式中的 INF。 如果下限格式没有 INF 表示形式，它将转换为可表示的最大值。 如果在目标格式中可用，将保留符号。
-   如果在下限范围中可用并可以转换，上限范围格式中的 Denorm 将转换为下限范围格式中的 Denorm 表示形式，否则结果为 0。 如果在目标格式中可用，将保留符号位。

### <a name="span-idconvertingfromalowerrangerepresentationtoahigherrangerepresentationspanspan-idconvertingfromalowerrangerepresentationtoahigherrangerepresentationspanspan-idconvertingfromalowerrangerepresentationtoahigherrangerepresentationspanconverting-from-a-lower-range-representation-to-a-higher-range-representation"></a><span id="Converting_from_a_lower_range_representation_to_a_higher_range_representation"></span><span id="converting_from_a_lower_range_representation_to_a_higher_range_representation"></span><span id="CONVERTING_FROM_A_LOWER_RANGE_REPRESENTATION_TO_A_HIGHER_RANGE_REPRESENTATION"></span>从下限范围表示形式转换为上限范围表示形式

-   如果在上限范围格式中可用，下限范围格式中的 NaN 将转换为上限范围格式中的 NaN 表示形式。 如果上限范围格式没有 NaN 表示形式，将转换为 0。
-   如果在上限范围格式中可用，下限范围格式中的 INF 将转换为上限范围格式中的 INF 表示形式。 如果上限格式没有 INF 表示形式，它将转换为可表示的最大值（此种格式的 MAX\_FLOAT）。 如果在目标格式中可用，将保留符号。
-   如果可能，下限范围格式中的 Denrom 将转换为上限范围格式中的标准化表示形式，否则，如果存在 Denorm 表示形式，则将转换为上限范围格式中的 Denorm 表示形式。 如果上限范围格式没有 Denorm 表示形式，它将转换为 0。 如果在目标格式中可用，将保留符号。 注意，32 位浮点数记为没有 Denorm 表示形式的格式（因为 32 位浮点中操作的 Denorm 刷新为保留 0 的符号）。

## <a name="span-idintegerconversionspanspan-idintegerconversionspanspan-idintegerconversionspaninteger-conversion"></a><span id="Integer_Conversion"></span><span id="integer_conversion"></span><span id="INTEGER_CONVERSION"></span>整数转换


下表介绍了从上文描述的各种表示形式转换为其他表示形式。 只会显示在 Direct3D 中实际发生的转换。

对于整数，除非另行指定，将准确执行下文描述的所有整数表示形式与浮点表示形式之间的转换。

<table>
<colgroup>
<col width="33%" />
<col width="33%" />
<col width="33%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">源数据类型</th>
<th align="left">目标数据类型</th>
<th align="left">转换规则</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left">SNORM</td>
<td align="left">FLOAT</td>
<td align="left"><p>假设一个表示符号范围为 [-1.0f 至 1.0f] 的 n 位整数值，则转换为浮点，如下所示。</p>
<ul>
<li>最负值映射到 -1.0f。 例如，5 位值 10000 映射到 -1.0f。</li>
<li>每个其他值转换为浮点（将其命名为 c），然后结果 = c * (1.0f / (2⁽ⁿ⁻¹⁾-1))。 例如，5 位值 10001 转换为 -15.0f，然后除以 15.0f，生成 -1.0f。</li>
</ul></td>
</tr>
<tr class="even">
<td align="left">FLOAT</td>
<td align="left">SNORM</td>
<td align="left"><p>假设一个浮点数，转换为表示符号范围为 [-1.0f 至 1.0f] 的 n 位整数值，如下所示。</p>
<ul>
<li>用 c 表示起始值。</li>
<li>如果 c 为 NaN，则结果为 0。</li>
<li>如果 c &gt; 1.0f（包括 INF），则将它固定为 1.0f。</li>
<li>如果 c &lt; -1.0f（包括 -INF），则将它固定为 -1.0f。</li>
<li>从浮动缩放转换为整数缩放： c = c * (2ⁿ⁻¹ 1)。</li>
<li>转换为整数，如下所示。
<ul>
<li>如果 c &gt;= 0，则 c = c + 0.5f，否则，c = c - 0.5f。</li>
<li>除去小数部分，剩余浮点（整型）值直接转换为整数。</li>
</ul></li>
</ul>
<p>允许此转换有 D3D<em>xx</em>_FLOAT32_TO_INTEGER_TOLERANCE_IN_Unit-Last-Place 最小单位的容错（整数边）。 这意味着，从浮动缩放转换为整数缩放后，允许可表示的目标格式值的 D3D<em>xx</em>_FLOAT32_TO_INTEGER_TOLERANCE_IN_ULP 最小单位中的任意值映射到该值。 其他数据可逆性要求确保转换在范围中非递减，且所有输出数据均可实现。 （在此处显示的常量中，<em>xx</em> 应替换为 Direct3D 版本，例如 10、11 或 12。)</p></td>
</tr>
<tr class="odd">
<td align="left">UNORM</td>
<td align="left">FLOAT</td>
<td align="left"><p>起始 n 位值转换为浮点（0.0f、1.0f、2.0f 等），然后除以 (2ⁿ-1)。</p></td>
</tr>
<tr class="even">
<td align="left">FLOAT</td>
<td align="left">UNORM</td>
<td align="left"><p>用 c 表示起始值。</p>
<ul>
<li>如果 c 为 NaN，则结果为 0。</li>
<li>如果 c &gt; 1.0f（包括 INF），则将它固定为 1.0f。</li>
<li>如果 c &lt; 0.0f（包括 -INF），则将它固定为 0.0f。</li>
<li>从浮动缩放转换为整数缩放： c = c * (2ⁿ-1)。</li>
<li>转换为整数。
<ul>
<li>c = c + 0.5f。</li>
<li>除去小数部分，剩余浮点（整型）值直接转换为整数。</li>
</ul></li>
</ul>
<p>允许此转换有 D3D<em>xx</em>_FLOAT32_TO_INTEGER_TOLERANCE_IN_ULP 最小单位的容错（整数边）。 这意味着，从浮动缩放转换为整数缩放后，允许可表示的目标格式值的 D3D<em>xx</em>_FLOAT32_TO_INTEGER_TOLERANCE_IN_ULP 最小单位中的任意值映射到该值。 其他数据可逆性要求确保转换在范围中非递减，且所有输出数据均可实现。</p></td>
</tr>
<tr class="odd">
<td align="left">SRGB</td>
<td align="left">FLOAT</td>
<td align="left"><p>以下是 SRGB 到 FLOAT 的理想转换。</p>
<ul>
<li>取起始 n 位值，转换为浮点（0.0f、1.0f、2.0f 等），将其命名为 c。</li>
<li>c = c * (1.0f / (2ⁿ-1))</li>
<li>如果 (c &lt; = D3D<em>xx</em>_SRGB_TO_FLOAT_THRESHOLD)，则：结果 = c / D3D<em>xx</em>_SRGB_TO_FLOAT_DENOMINATOR_1，否则，结果 = ((c + D3D<em>xx</em>_SRGB_TO_FLOAT_OFFSET)/D3D<em>xx</em>_SRGB_TO_FLOAT_DENOMINATOR_2)D3D<em>xx</em>_SRGB_TO_FLOAT_EXPONENT</li>
</ul>
<p>允许此转换有 D3D<em>xx</em>_SRGB_TO_FLOAT_TOLERANCE_IN_ULP 最小单位的容错（SRGB 边）。</p></td>
</tr>
<tr class="even">
<td align="left">FLOAT</td>
<td align="left">SRGB</td>
<td align="left"><p>以下是 FLOAT 到&gt; SRGB 的理想转换。</p>
<p>假设目标 SRGB 颜色分量有 n 位：</p>
<ul>
<li>假设起始值为 c。</li>
<li>如果 c 为 NaN，则结果为 0。</li>
<li>如果 c &gt; 1.0f（包括 INF），则将它固定为 1.0f。</li>
<li>如果 c &lt; 0.0f（包括 -INF），则将它固定为 0.0f。</li>
<li>如果 (c &lt;= D3D<em>xx</em>_FLOAT_TO_SRGB_THRESHOLD)，则：c = D3D<em>xx</em>_FLOAT_TO_SRGB_SCALE_1 * c，否则：c = D3D<em>xx</em>_FLOAT_TO_SRGB_SCALE_2 * c(D3D<em>xx</em>_FLOAT_TO_SRGB_EXPONENT_NUMERATOR/D3D<em>xx</em>_FLOAT_TO_SRGB_EXPONENT_DENOMINATOR) - D3D<em>xx</em>_FLOAT_TO_SRGB_OFFSET</li>
<li>从浮动缩放转换为整数缩放： c = c * (2ⁿ-1)。</li>
<li>转换为整数：
<ul>
<li>c = c + 0.5f。</li>
<li>除去小数部分，剩余浮点（整型）值直接转换为整数。</li>
</ul></li>
</ul>
<p>允许此转换有 D3D<em>xx</em>_FLOAT32_TO_INTEGER_TOLERANCE_IN_ULP 最小单位的容错（整数边）。 这意味着，从浮动缩放转换为整数缩放后，允许可表示的目标格式值的 D3D<em>xx</em>_FLOAT32_TO_INTEGER_TOLERANCE_IN_ULP 最小单位中的任意值映射到该值。 其他数据可逆性要求确保转换在范围中非递减，且所有输出数据均可实现。</p></td>
</tr>
<tr class="odd">
<td align="left">SINT</td>
<td align="left">位数更多的 SINT</td>
<td align="left"><p>为从 SINT 转换为位数更多的 SINT，起始数字的最高有效位 (MSB) 已&quot;符号扩展&quot;至目标格式中的其他可用位。</p></td>
</tr>
<tr class="even">
<td align="left">UINT</td>
<td align="left">位数更多的 SINT</td>
<td align="left"><p>要从 UINT 转换为位数更多的 SINT，请将数字复制到目标格式的最低有效位 (LSB)，且其他 MSB 用 0 填充。</p></td>
</tr>
<tr class="odd">
<td align="left">SINT</td>
<td align="left">位数更多的 UINT</td>
<td align="left"><p>要从 SINT 转换为位数更多的 UINT：如果为负，则将值固定为 0。 否则，将数字复制到目标格式的 LSB，且其他 MSB 用 0 填充。</p></td>
</tr>
<tr class="even">
<td align="left">UINT</td>
<td align="left">位数更多的 UINT</td>
<td align="left"><p>要从 UINT 复制到位数更多的 UINT，请将数字复制到目标格式的 LSB，且其他 MSB 用 0 填充。</p></td>
</tr>
<tr class="odd">
<td align="left">SINT 或 UINT</td>
<td align="left">SINT 或位数更少或位数相等的 UNIT</td>
<td align="left"><p>要从 SINT 或 UINT 转换为位数更少或位数相等的 SINT 或 UINT（和/或更改符号），起始值将固定为目标格式的范围。</p></td>
</tr>
</tbody>
</table>

 

## <a name="span-idfixedpointintegerconversionspanspan-idfixedpointintegerconversionspanspan-idfixedpointintegerconversionspanfixed-point-integer-conversion"></a><span id="Fixed_Point_Integer_Conversion"></span><span id="fixed_point_integer_conversion"></span><span id="FIXED_POINT_INTEGER_CONVERSION"></span>固定点整数转换


固定点整数仅仅是某些在固定位置具有隐式小数点的位大小的整数。

普遍的“整数”数据类型是数字末尾带有小数点的固定点整数的特殊情况。

固定点数字表示形式以 i.f 为特征，其中，i 是整数位数字，f 是小数位数字。 例如，16.8 意味着 16 位整数，后跟 8 位小数。 整数部分存储在 2 的补数中，至少如此处所定义（尽管也可以为无符号整数同等定义）。 小数部分按无符号形式存储。 小数部分始终表示两个最近整数值之间的正小数，从最负值开始。

只需使用标准整数算术对固定点数字进行加减运算，无需考虑隐式小数点的位置。 将 16.8 固定点数加上 1 仅意味着加上 256，因为小数点位于数字最末端之前第 8 位。 乘法等其他运算也可以使用整数算术进行，前提是讲清固定十进制的影响。 例如，使用整数相乘将两个 16.8 整数相乘产生的结果是 32.16。

固定点整数表示方式在 Direct3D 中有两种使用方式。

-   光栅器中的裁剪后顶点位置将贴靠到固定点，以在 RenderTarget 区域中统一分发精度。 许多光栅器操作（以正面剔除为例）都发生在固定点贴靠位置，而其他操作（例如，属性内插器设置）则使用已从固定点贴靠位置转换回浮点的位置。
-   采样操作的纹理坐标已贴靠到固定点（按纹理大小缩放后），以在选择筛选点击位置/权重时在纹理空间中统一分发精度。 在实际执行筛选算术之前，将权重值转换回浮点值。

<table>
<colgroup>
<col width="33%" />
<col width="33%" />
<col width="33%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">源数据类型</th>
<th align="left">目标数据类型</th>
<th align="left">转换规则</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left">FLOAT</td>
<td align="left">固定点整数</td>
<td align="left"><p>下面是将浮点数 n 转换为固定点整数 i.f 的一般过程，其中，i 是（有符号）整数位数字，f 是小数位数字。</p>
<ul>
<li>计算 FixedMin = -2⁽ⁱ⁻¹⁾</li>
<li>计算 FixedMax = 2⁽ⁱ⁻¹⁾ - 2<sup>(-f)</sup></li>
<li>如果 n 为 NaN，结果 = 0；如果 n 为 +Inf，结果 = FixedMax*2<sup>f</sup>；如果 n 为 -Inf，结果 = FixedMin*2<sup>f</sup></li>
<li>如果 n &gt;= FixedMax，结果 = Fixedmax*2<sup>f</sup>；如果 n &lt;= FixedMin，结果 = FixedMin*2<sup>f</sup></li>
<li>否则，计算 n*2<sup>f</sup> 并转换为整数。</li>
</ul>
<p>整数结果中，允许实现有 D3D<em>xx</em>_FLOAT32_TO_INTEGER_TOLERANCE_IN_ULP 最小位数的容错，而不是上文中最后一步之后的无限精确值 n*2<sup>f</sup>。</p></td>
</tr>
<tr class="even">
<td align="left">固定点整数</td>
<td align="left">FLOAT</td>
<td align="left"><p>假设转换为浮动的特定固定点表示形式不包含超过总共 24 位的信息，则小数部分的信息不超过 23 位。 假设给定的固定点数 fxp 为 i.f 形式（i 为整数位，f 为小数位）。 转换为浮点的操作与以下伪代码类似。</p>
<p>浮点结果 = (float) (fxp &gt; &gt; f) + / / 提取整数</p>
((float) (fxp &amp; (2<sup>f</sup> - 1)) / (2<sup>f</sup>));提取小数</td>
</tr>
</tbody>
</table>

 

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>相关主题


[附录](appendix.md)

 

 




