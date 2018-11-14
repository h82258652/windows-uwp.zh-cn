---
title: 第 2 层
description: 第 2 层流式资源支持在第 1 层基础上增加了一些功能，如在大小至少为一个标准磁贴形状时保证非环绕纹理的 mipmap；具有用于固定详细信息级别 (LOD) 及用于获取着色器操作相关状态的着色器指令；此外，可读取将采样值视为零的 NULL 映射磁贴。
ms.assetid: 111A28EA-661A-4D29-921A-F2E376A46DC5
keywords:
- 第 2 层
author: michaelfromredmond
ms.author: mithom
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: fac8780995231ce56d1264ea8a1a5cb52fd9a3d0
ms.sourcegitcommit: f2c9a050a9137a473f28b613968d5782866142c6
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/09/2018
ms.locfileid: "6258065"
---
# <a name="tier-2"></a>第 2 层


第 2 层流式资源支持在第 1 层基础上增加了一些功能，如在大小至少为一个标准磁贴形状时保证非环绕纹理的 mipmap；具有用于固定详细信息级别 (LOD) 及用于获取着色器操作相关状态的着色器指令；此外，可读取将采样值视为零的 NULL 映射磁贴。

## <a name="span-idtier2generalsupportspanspan-idtier2generalsupportspanspan-idtier2generalsupportspantier-2-general-support"></a><span id="Tier_2_general_support"></span><span id="tier_2_general_support"></span><span id="TIER_2_GENERAL_SUPPORT"></span>第 2 层常规支持


第 2 层支持包括以下各项。

-   最低功能级别 11.1 所用硬件。
-   上一层的所有功能（无[第 1 层](tier-1.md)特定限制）及以下新增功能：
-   提供针对固定 LOD 和映射状态反馈的着色器指令。 请参阅 [HLSL 流式资源暴露](hlsl-streaming-resources-exposure.md)。

除上述内容外，还有一些特定支持问题。

## <a name="span-idnon-mappedtilesspanspan-idnon-mappedtilesspanspan-idnon-mappedtilesspannon-mapped-tiles"></a><span id="Non-mapped_tiles"></span><span id="non-mapped_tiles"></span><span id="NON-MAPPED_TILES"></span>未映射磁贴


非映射磁贴的读数在格式的所有未缺失组件中返回 0，缺失组件返回默认值。

向非映射磁贴的写入被阻止进入内存，但可能会最终进入缓存，相同地址的后续读取可能选取或不选取此类缓存。

## <a name="span-idtexturefilteringspanspan-idtexturefilteringspanspan-idtexturefilteringspantexture-filtering"></a><span id="Texture_filtering"></span><span id="texture_filtering"></span><span id="TEXTURE_FILTERING"></span>纹理筛选


足迹跨 **NULL** 和非 **NULL** 磁贴的磁贴筛选在整体筛选操作中以 0 表示 **NULL** 磁贴上的纹素（默认值表示缺失的格式组件）。 部分早期硬件不满足此要求，如果任何纹素（权重不为零）落在 **NULL** 磁贴上，则为完整筛选结果返回 0（缺失格式组件返回默认值）。 不允许任何其他硬件违反在筛选操作中纳入所有（非零权重的）纹素的要求。

**NULL** 纹素访问将引起 [**CheckAccessFullyMapped**](https://msdn.microsoft.com/library/windows/desktop/dn292083) 针对纹理读取返回 false 的状态反馈的操作。 无论纹理访问结果如何屏蔽着色器中的写入，以及纹理格式包含多少组件（二者结合可能导致纹理看似不需要访问），都会产生上述结果。

## <a name="span-idalignmentconstraintsspanspan-idalignmentconstraintsspanspan-idalignmentconstraintsspanalignment-constraints"></a><span id="Alignment_constraints"></span><span id="alignment_constraints"></span><span id="ALIGNMENT_CONSTRAINTS"></span>对齐方式约束


标准磁贴形状的对齐约束：保证在所有维度至少填充一个标准磁贴的 Mipmap 使用标准平铺，其余视为整体填入 N 磁贴（N 已报告给应用程序）的一个**单位**。 应用程序可以将 N 磁贴映射到磁贴池中的任意非连续位置，但必须映射所有打包磁贴或不映射任何磁贴。 mip 打包是每个数组切片的一组唯一的打包磁贴。

## <a name="span-idminmaxreductionfilteringspanspan-idminmaxreductionfilteringspanspan-idminmaxreductionfilteringspanminmax-reduction-filtering"></a><span id="Min_Max_reduction_filtering"></span><span id="min_max_reduction_filtering"></span><span id="MIN_MAX_REDUCTION_FILTERING"></span>最小值/最大值减少筛选


支持最小/最大减少值筛选。 请参阅[流式资源纹理采样功能](streaming-resources-texture-sampling-features.md)。

## <a name="span-idlimitationsspanspan-idlimitationsspanspan-idlimitationsspanlimitations"></a><span id="Limitations"></span><span id="limitations"></span><span id="LIMITATIONS"></span>限制


对于拥有任何维度均小于标准磁贴大小的 mipmap 的流式资源，数组大小不得大于 1。

存在重复映射时磁贴的访问方式限制继续适用。 请参阅[存在重复映射时的磁贴访问限制](tile-access-limitations-with-duplicate-mappings.md)。

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>相关主题


[流式资源功能层](streaming-resources-features-tiers.md)

 

 




