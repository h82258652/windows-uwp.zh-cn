---
title: 浮点规则
description: Direct3D 支持多种浮点表示。 所有浮点计算都依照 IEEE 754 32 位单精度浮点规则的既定子集运行。
ms.assetid: 3B0C95E2-1025-4F70-BF14-EBFF2BB53AFF
keywords:
- 浮点规则
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 4de5ba146c8241598527dd268d604fcc9bb97d6d
ms.sourcegitcommit: b11f305dbf7649c4b68550b666487c77ea30d98f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/28/2018
ms.locfileid: "7849391"
---
# <a name="span-iddirect3dconceptsfloating-pointrulesspanfloating-point-rules"></a><span id="direct3dconcepts.floating-point_rules"></span>浮点规则


Direct3D 支持多种浮点表示。 所有浮点计算都依照 IEEE 754 32 位单精度浮点规则的既定子集运行。

## <a name="span-idalpha32bitspanspan-idalpha32bitspan32-bit-floating-point-rules"></a><span id="alpha_32_bit"></span><span id="ALPHA_32_BIT"></span>32 位浮点规则


有两组规则：符合 IEEE-754 的规则和那些偏离标准的规则。

### <a name="span-idalpha754rulesspanspan-idalpha754rulesspanspan-idalpha754rulesspanhonored-ieee-754-rules"></a><span id="alpha_754_Rules"></span><span id="alpha_754_rules"></span><span id="ALPHA_754_RULES"></span>推崇的 IEEE-754 规则

这些规则中的一些是 IEEE-754 提供选择的单个选项。

-   除以 0 产生 +/- INF，除了会导致 NaN 的 0/0 以外。
-   (+/-) 0 的 log 产生 -INF。  

    负值（-0 除外）的 log 产生 NaN。
-   负数的倒数平方根 (rsq) 或平方根 (sqrt) 产生 NaN。  

    例外情况是 -0；sqrt(-0) 产生 -0，而 rsq(-0) 产生 -INF。
-   INF - INF = NaN
-   (+/-)INF / (+/-)INF = NaN
-   (+/-)INF \* 0 = NaN
-   NaN（任何 OP）任何值 = NaN
-   当任一或两个操作数都是 NaN 时，比较运算符 EQ、GT、GE、LT 和 LE 会返回 **FALSE**。
-   比较运算符忽略 0 的符号（因此 +0 等于 -0）。
-   当任一或两个操作数都是 NaN 时，比较运算符 NE 返回 **TRUE**。
-   任何非 NaN 值与 +/- INF 的比较运算都会返回正确的结果。

### <a name="span-idalpha754deviationsspanspan-idalpha754deviationsspanspan-idalpha754deviationsspandeviations-or-additional-requirements-from-ieee-754-rules"></a><span id="alpha_754_Deviations"></span><span id="alpha_754_deviations"></span><span id="ALPHA_754_DEVIATIONS"></span>IEEE-754 规则的偏差或其他要求

-   IEEE-754 要求浮点运算产生结果，该结果是最接近无限精确结果的可表示值，称为最近舍入。

    Direct3D 11 及以上定义了与 IEEE-754 相同的要求：32 位浮点运算产生的结果在无限精确结果的 0.5 最后位置单位 (ULP) 内。 这意味着，例如，硬件允许将结果截断到 32 位，而不是执行最近舍入，因为后者将导致至多 0.5 ULP 的误差。 此规则仅适用于加法、减法和乘法。

    Direct3D 的早期版本定义了比 IEEE-754 更宽松的要求：32 位浮点运算产生的结果在无限精确结果的一个最后位置单位 (1 ULP) 内。 这意味着，例如，硬件允许将结果截断到 32 位，而不是执行最近舍入，因为后者将导致至多 1 ULP 的误差。

-   不支持浮点异常、状态位或陷阱。
-   Denorm 在任何浮点数学运算的输入和输出上被刷新为符号保留的零。 不处理数据的任何 I/O 或数据移动运算属于例外情况。
-   包含浮点值（例如 Viewport MinDepth/MaxDepth 或 BorderColor 值）的状态可以作为 denorm 值提供，并且可能在或可能不会被硬件使用之前刷新。
-   最小值或最大值运算会刷新 denorm 以进行比较，但结果可能会或可能不会刷新 denorm。
-   在运算中输入 NaN 始终在输出上产生 NaN。 但是 NaN 的确切位模式不需要保持不变（除非运算是不会改变数据的原始移动指令）。
-   针对只有一个操作数为 NaN 的数据进行的最小值或最大值计算会返回另一个操作数作为结果（与我们前面看过的比较规则相反）。 这是 IEEE 754R 规则。

    用于浮点最小值和最大值运算的 IEEE-754R 规范规定，如果最小值或最大值的输入之一是静止的 QNaN 值，则运算的结果是另一个参数。 例如：

    ```ManagedCPlusPlus
    min(x,QNaN) == min(QNaN,x) == x (same for max)
    ```

    当一个输入是“信号”SNaN 值与 QNaN 值时，IEEE-754R 规范的修订版采用不同的最小值和最大值行为：

    ```ManagedCPlusPlus
    min(x,SNaN) == min(SNaN,x) == QNaN (same for max)
     
    ```

    一般情况下，Direct3D 会遵循算术标准：IEEE-754 和 IEEE-754R。 但这种情况下会存在偏差。

    Direct3D 10 和更高版本中的算术规则不对静止和信号 NaN 值（QNaN与SNaN）进行区分。 所有 NaN 值都以相同的方式处理。 在最小值和最大值的情况下，任何 NaN 值的 Direct3D 行为都类似于 QNaN 在 IEEE-754R 定义中的处理方式。 （为了保持完整性 - 如果两个输入都是 NaN，则返回任何 NaN 值。）

-   另一个 IEEE 754R 规则是 min(-0,+0) == min(+0,-0) == -0 和 max(-0,+0) == max(+0,-0) == +0，该规则推崇符号，与符号零的比较规则相反（如我们前面所看到的）。 Direct3D 推荐此处的 IEEE 754R 行为，但不强制使用；比较零的结果也可以通过使用忽略符号的比较运算符取决于参数的顺序。
-   x\*1.0f 的结果始终是 x（除了 denorm 刷新）。
-   x/1.0f 的结果始终是 x（除了 denorm 刷新）。
-   x +/- 0.0f 的结果始终是 x（除了 denorm 刷新）。 但是 -0 + 0 = +0。
-   融合运算（例如 mad、dp3）产生的结果与运算的未融合扩展的评估的最差的串行排序一样准确。 对于给定的融合运算，出于容差的目的，最差的排序的定义不是固定的定义；它取决于输入的特定值。 允许未融合扩展中的各个步骤出现 1 个 ULP 容差（或者，Direct3D 调用的命令具有比 1 个 ULP 更宽松的容差，则允许出现更宽松的容差）。
-   融合运算遵守与非融合运算相同的 NaN 规则。
-   sqrt 和 rcp 有 1 个 ULP 容差。 着色器倒数指令和倒数平方根指令 [**rcp**](https://msdn.microsoft.com/library/windows/desktop/hh447205) 和 [**rsq**](https://msdn.microsoft.com/library/windows/desktop/hh447221) 拥有各自的宽松精度要求。
-   乘法和除法运算为 32 位浮点精度水平（乘法的精度为 0.5 ULP，倒数的精度为 1.0 ULP）。 如果直接实现 x/y，结果必须比两步法具有更大或相等的精度。

## <a name="span-iddoubleprec64bitspanspan-iddoubleprec64bitspan64-bit-double-precision-floating-point-rules"></a><span id="double_prec_64_bit"></span><span id="DOUBLE_PREC_64_BIT"></span>64 位（双精度）浮点规则


硬件和显示驱动程序可选择支持双精度浮点。 要指示支持，当使用 [**D3D11\_FEATURE\_DOUBLES**](https://msdn.microsoft.com/library/windows/desktop/ff476124#d3d11-feature-doubles) 调用 [**ID3D11Device::CheckFeatureSupport**](https://msdn.microsoft.com/library/windows/desktop/ff476497) 时，驱动程序将 [**D3D11\_FEATURE\_DATA\_DOUBLES**](https://msdn.microsoft.com/library/windows/desktop/ff476127) 的 **DoublePrecisionFloatShaderOps** 设置为 TRUE。 驱动程序和硬件必须支持所有双精度浮点指令。

双精度指令遵循 IEEE 754R 行为要求。

双精度数据需要支持生成非标准化值（无刷新到零行为）。 同样地，指令不将非标准化数据读取为符号零，它们接受 denorm 值。

## <a name="span-idalpha16bitspanspan-idalpha16bitspan16-bit-floating-point-rules"></a><span id="alpha_16_bit"></span><span id="ALPHA_16_BIT"></span>16 位浮点规则


Direct3D 还支持浮点数的 16 位表示形式。

格式：

-   1 个符号位 (s) 在 MSB 位位置
-   5 位偏置指数 (e)
-   10 位的分数 (f)，具有附加的隐藏位

float16 值 (v) 遵循以下规则：

-   如果 e == 31 且 f != 0，那么 v 是 NaN，无论 s 是什么
-   如果 e == 31 且 f == 0，则 v =（-1）s\*无穷大（有符号无穷大）
-   如果 e 在 0 和 31 之间，那么 v = (-1)s\*2(e-15)\*(1.f)
-   如果 e == 0 且 f != 0，那么 v = (-1)s\*2(e-14)\*(0.f)（非标准化数字）
-   如果 e == 0 且 f == 0，那么 v = (-1)s\*0（符号零）

32 位浮点规则也适用于 16 位浮点数，根据前面描述的位布局进行调整。 这种情况的例外情况包括：

-   精度：16 位浮点数上的未融合运算产生的结果是最接近无限精确结果（根据 IEEE-754 执行最近舍入，应用于 16 位值）的可表示值。 32 位浮点规则遵守 1 个 ULP 容差，在16 位浮点规则中，未融合运算遵守 0.5 ULP，融合运算遵守 0.6 ULP。
-   16 位浮点数保留 denorm。

## <a name="span-idalpha11bitspanspan-idalpha11bitspan11-bit-and-10-bit-floating-point-rules"></a><span id="alpha_11_bit"></span><span id="ALPHA_11_BIT"></span>11 位和 10 位浮点规则


Direct3D 还支持 11 位和 10 位浮点格式。

格式：

-   没有符号位
-   5 位偏置指数 (e)
-   11 位格式的分数 (f) 为 6 位，10 位格式的分数 (f) 为 5 位，在任一情况下都具有附加的隐藏位。

float11/float10 值 (v) 遵循以下规则：

-   如果 e == 31 且 f != 0，那么 v 是 NaN
-   如果 e == 31 且 f == 0，那么 v = 正无穷
-   如果 e 在 0 和 31 之间，那么 v = 2(e-15)\*(1.f)
-   如果 e == 0 且 f != 0，那么 v = \*2(e-14)\*(0.f)（非标准化数字）
-   如果 e == 0 且 f == 0，那么 v = 0（零）

32 位浮点规则也适用于 11 位和 10 位浮点数，根据前面描述的位布局进行调整。 例外情况包括：

-   精度：32 位浮点规则遵守 0.5 ULP。
-   10/11 位浮点数保留 denorm。
-   所得结果小于零的任何运算都固定为零。

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>相关主题


[附录](appendix.md)

[资源](https://msdn.microsoft.com/library/windows/desktop/ff476894)

[纹理](https://msdn.microsoft.com/library/windows/desktop/ff476902)

 

 




