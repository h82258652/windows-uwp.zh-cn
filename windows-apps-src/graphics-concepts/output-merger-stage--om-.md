---
title: 输出合并 (OM) 阶段
description: 输出合并 (OM) 阶段将各种类型的输出数据（像素着色器值、深度和模板信息）与着色器目标的内容以及深度/模板缓冲区组合在一起，以生成最终的管道结果。
ms.assetid: ED2DC4A0-2B92-47AF-884A-BFC2183C78B8
keywords:
- 输出合并 (OM) 阶段
author: michaelfromredmond
ms.author: mithom
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
ms.localizationpriority: medium
ms.openlocfilehash: 7106b36b367716fdb53af3da506e60d019ac4e8e
ms.sourcegitcommit: 0ab8f6fac53a6811f977ddc24de039c46c9db0ad
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/15/2018
ms.locfileid: "1653846"
---
# <a name="output-merger-om-stage"></a>输出合并 (OM) 阶段


输出合并 (OM) 阶段将各种类型的输出数据（像素着色器值、深度和模板信息）与着色器目标的内容以及深度/模板缓冲区组合在一起，以生成最终的管道结果。

## <a name="span-idpurpose-and-usesspanspan-idpurpose-and-usesspanspan-idpurpose-and-usesspanpurpose-and-uses"></a><span id="Purpose-and-uses"></span><span id="purpose-and-uses"></span><span id="PURPOSE-AND-USES"></span>用途和用法


输出合并 (OM) 阶段是确定可见的像素（利用深度模板测试）和混合最终像素颜色的最后一步。

OM 阶段将使用以下项的组合生成最终呈现的像素颜色：

-   管道状态
-   由像素着色器生成的像素数据
-   呈现目标的内容
-   深度/模板缓冲区的内容。

### <a name="span-idblending-overviewspanspan-idblending-overviewspanspan-idblending-overviewspanblending-overview"></a><span id="Blending-overview"></span><span id="blending-overview"></span><span id="BLENDING-OVERVIEW"></span>混合概述

混合是指将一个或多个像素值组合起来以创建最终的像素颜色。 下图显示了混合像素数据涉及的过程。

![混合数据的工作原理的图示](images/d3d10-blend-state.png)

从概念上来说，你可以将在输出合并阶段实施了两次的此流程图可视化：第一次混合 RGB 数据，第二次混合 alpha 数据，两者并行执行。 若要了解如何使用 API 创建和设置混合状态，请参阅[配置混合功能](https://msdn.microsoft.com/library/windows/desktop/bb205072)。

可为每个呈现目标单独启用固定函数混合。 但是，只存在一组混合控件，这是为了在启用混合后让同一混合应用于所有 RenderTarget。 在混合之前，混合值（包括 BlendFactor）始终固定到呈现目标格式的范围。 根据呈现目标类型，固定对每个呈现目标执行一次。 唯一的例外情况是 float16、float11 或 float10 格式，为了让针对这些格式的混合运算可在至少具有与输出格式相等的精度/范围的情况下完成，没有对这些格式进行固定。 在所有情况下都会传播 NaN 和有符号零（包括 0.0 混合权重）。

当你使用 sRGB 呈现目标时，运行时在执行混合之前会将呈现目标颜色转换到线性空间中。 运行时在将最终混合的值保存回呈现目标之前会将该值转换回 sRGB 空间中。

### <a name="span-iddual-source-color-blendingspanspan-iddual-source-color-blendingspanspan-iddual-source-color-blendingspandual-source-color-blending"></a><span id="Dual-source-color-blending"></span><span id="dual-source-color-blending"></span><span id="DUAL-SOURCE-COLOR-BLENDING"></span>双源颜色混合

此功能使输出合并阶段能够同时使用两个像素着色器输出（o0 和 o1）作为单个呈现目标位于槽 0 中的混合运算的输入。 有效的混合运算包括：add、subtract 和 revsubtract。 混合方程和输出写入掩码指定了像素着色器将输出的组件。 额外组件将被忽略。

向其他像素着色器输出（o2、o3 等）的写入未定义；如果呈现目标未绑定到槽 0，你就无法写入到呈现目标。 在双源颜色混合期间写入 oDepth 很有效。

### <a name="span-iddepth-stencil-testspanspan-iddepth-stencil-testspanspan-iddepth-stencil-testspandepth-stencil-testing-overview"></a><span id="Depth-Stencil-Test"></span><span id="depth-stencil-test"></span><span id="DEPTH-STENCIL-TEST"></span>深度模板测试概述

作为纹理资源创建的深度模板缓冲区可同时包含深度数据和模板数据。 深度数据用于确定最靠近相机的像素，模板数据用于掩盖可以更新的像素。 最终，深度值和模板值数据将被输出合并阶段用来确定是否应绘制某个像素。 下图显示了在概念上如何完成深度模板测试。

![深度模板测试的工作原理的图示](images/d3d10-depth-stencil-test.png)

若要配置深度模板测试，请参阅[配置深度模板功能](configuring-depth-stencil-functionality.md)。 深度模板对象将封装深度模板状态。 应用程序可指定深度模板状态，否则 OM 阶段将使用默认值。 如果已禁用多重采样，则为每个像素执行一次混合运算。 如果已启用多重采样，则在每次多重采样时执行一次混合。

使用深度缓冲区确定应绘制的像素的过程称为深度缓冲，有时也称为 z 缓冲。

当深度值到达输出合并阶段（无论来自内插还是来自像素着色器），将使用浮点规则并根据深度缓冲区的格式/精度始终固定它们：z = min(Viewport.MaxDepth,max(Viewport.MinDepth,z))。 在固定后，深度值将与现有深度缓冲区值进行比较（使用 DepthFunc）。 如果没有绑定深度缓冲区，深度测试将总是通过。

如果没有采用深度缓冲区格式的模板组件，或没有绑定深度缓冲区，模板测试将总是通过。

一次只能有一个深度/模板缓冲区可处于活动状态；任何绑定的资源视图必须与（相同大小和尺寸的）深度/模板视图匹配。 这并不意味着资源大小必须匹配，而只是意味着视图大小必须匹配。

### <a name="span-idsample-maskspanspan-idsample-maskspanspan-idsample-maskspansample-mask-overview"></a><span id="Sample-Mask"></span><span id="sample-mask"></span><span id="SAMPLE-MASK"></span>采样掩码概述

采样掩码是 32 位多重采样覆盖掩码，用于确定哪些样本在活动呈现目标中获得更新。 一次只允许有一个采样掩码。 采样掩码中的位到资源中的样本的映射由用户定义。 对于 n 采样呈现，将使用采样掩码的前 n 位（从 LSB 起，32 位是最大位数）。

## <a name="span-idinputspanspan-idinputspanspan-idinputspaninput"></a><span id="Input"></span><span id="input"></span><span id="INPUT"></span>输入


输出合并 (OM) 阶段将使用以下项的组合生成最终呈现的像素颜色：

-   管道状态
-   由像素着色器生成的像素数据
-   呈现目标的内容
-   深度/模板缓冲区的内容。

## <a name="span-idoutputspanspan-idoutputspanspan-idoutputspanoutput"></a><span id="Output"></span><span id="output"></span><span id="OUTPUT"></span>输出


### <a name="span-idoutput-write-mask-overviewspanspan-idoutput-write-mask-overviewspanspan-idoutput-write-mask-overviewspanoutput-write-mask-overview"></a><span id="Output-write-mask-overview"></span><span id="output-write-mask-overview"></span><span id="OUTPUT-WRITE-MASK-OVERVIEW"></span>输出写入掩码概述

使用输出写入掩码可控制（对每个组件）可写入到呈现目标的数据。

### <a name="span-idmultiple-render-targets-overviewspanspan-idmultiple-render-targets-overviewspanspan-idmultiple-render-targets-overviewspanmultiple-render-targets-overview"></a><span id="Multiple-render-targets-overview"></span><span id="multiple-render-targets-overview"></span><span id="MULTIPLE-RENDER-TARGETS-OVERVIEW"></span>多呈现目标概述

像素着色器可用于呈现至少 8 个单独的呈现目标，所有这些目标都必须属于同一类型（缓冲区、Texture1D、Texture1DArray 等）。 此外，所有呈现目标在所有尺寸上都必须都具有相同的大小（宽度、高度、深度、数组大小、样本计数）。 每个呈现目标都可能有不同的数据格式。

你可以使用呈现目标槽（最多 8 个）的任意组合。 但是，资源视图不能同时绑定到多个呈现目标槽。 只要资源未同时使用，视图就可以重复使用。

## <a name="span-idin-this-sectionspanin-this-section"></a><span id="in-this-section"></span>本节内容


<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">主题</th>
<th align="left">描述</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p><a href="configuring-depth-stencil-functionality.md">配置深度模板功能</a></p></td>
<td align="left"><p>本节介绍了设置深度模板缓冲区的步骤以及输出合并阶段的深度模板状态。</p></td>
</tr>
</tbody>
</table>

 

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>相关主题


[图形管道](graphics-pipeline.md)

 

 




