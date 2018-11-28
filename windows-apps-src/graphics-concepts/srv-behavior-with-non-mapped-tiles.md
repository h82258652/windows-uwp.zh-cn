---
title: 使用非映射磁贴的 SRV 行为
description: 涉及非映射磁贴的着色器资源视图 (SRV) 读取的行为取决于硬件支持的程度。
ms.assetid: 0E1D64BE-EB09-4F9D-9800-BD23A3B374EE
keywords:
- 使用非映射磁贴的 SRV 行为
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: d62a1d3158e13187f89277a1ba009bd56fc2b39a
ms.sourcegitcommit: b11f305dbf7649c4b68550b666487c77ea30d98f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/28/2018
ms.locfileid: "7848701"
---
# <a name="span-iddirect3dconceptssrvbehaviorwithnon-mappedtilesspansrv-behavior-with-non-mapped-tiles"></a><span id="direct3dconcepts.srv_behavior_with_non-mapped_tiles"></span>使用非映射磁贴的 SRV 行为


涉及非映射磁贴的着色器资源视图 (SRV) 读取的行为取决于硬件支持的程度。 有关详细要求，请参阅[流式资源功能层](streaming-resources-features-tiers.md)的读取行为。 本部分总结了[第 2 层](tier-2.md)所要求的理想行为。

例如，从 SRV 中的一组纹素读取的纹理筛选操作。 落于非映射磁贴上的纹素沿着映射纹素的贡献值，将格式中所有非丢失组件的 0（以及丢失组件的默认值）贡献至总体筛选操作。 无论数据来自于映射还是非映射磁贴，纹素均会进行加权和合并。

某些第一代[第 2 层](tier-2.md)级别硬件不符合此规格要求，而如果任何纹素（非零权重）落于非映射磁贴，此类硬件则会返回 0 以及上述默认值作为总体筛选结果。 其他硬件必须满足这项要求，才能在筛选中添加所有（非零权重）纹素。

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>相关主题


[对流式资源的管道访问](pipeline-access-to-streaming-resources.md)

 

 




