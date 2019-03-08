---
title: 流式资源纹理采样功能
description: 流式资源纹理采样功能包括获取有关映射区域的着色器状态反馈，检查正在访问的所有数据是否已在资源中映射，进行固定以帮助着色器避开 Mip 映射流式资源中已知的非映射区域，以及发现为整个纹理筛选器足迹完全映射的最小 LOD。
ms.assetid: C2B2DD69-8354-417A-894D-6235A8B48B53
keywords:
- 流式资源纹理采样功能
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 8b6290fba9d4194df78c39902b8d96e952134682
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2019
ms.locfileid: "57607412"
---
# <a name="streaming-resources-texture-sampling-features"></a>流式资源纹理采样功能


流式资源纹理采样功能包括获取有关映射区域的着色器状态反馈，检查正在访问的所有数据是否已在资源中映射，进行固定以帮助着色器避开 Mip 映射流式资源中已知的非映射区域，以及发现为整个纹理筛选器足迹完全映射的最小 LOD。

## <a name="span-idrequirementsofstreamingresourcestexturesamplingfeaturesspanspan-idrequirementsofstreamingresourcestexturesamplingfeaturesspanspan-idrequirementsofstreamingresourcestexturesamplingfeaturesspanrequirements-of-streaming-resources-texture-sampling-features"></a><span id="Requirements_of_streaming_resources_texture_sampling_features"></span><span id="requirements_of_streaming_resources_texture_sampling_features"></span><span id="REQUIREMENTS_OF_STREAMING_RESOURCES_TEXTURE_SAMPLING_FEATURES"></span>流式处理资源的要求纹理采样功能


本文所述的纹理采样功能需要[第 2 层](tier-2.md)级别的流式资源支持。

## <a name="span-idshaderstatusfeedbackaboutmappedareasspanspan-idshaderstatusfeedbackaboutmappedareasspanspan-idshaderstatusfeedbackaboutmappedareasspanshader-status-feedback-about-mapped-areas"></a><span id="Shader_status_feedback_about_mapped_areas"></span><span id="shader_status_feedback_about_mapped_areas"></span><span id="SHADER_STATUS_FEEDBACK_ABOUT_MAPPED_AREAS"></span>有关映射区域的着色器状态反馈


从流式资源读取和/或写入流式资源的任何着色器指令将导致系统记录状态信息。 此状态显示为进入 32 位临时寄存器的每条资源访问指令的可选额外返回值。 返回值的内容是透明的。 也就是说，不允许着色器程序直接读取。 但你可以使用 [**CheckAccessFullyMapped**](https://msdn.microsoft.com/library/windows/desktop/dn292083) 函数来提取状态信息。

## <a name="span-idfullymappedcheckspanspan-idfullymappedcheckspanspan-idfullymappedcheckspanfully-mapped-check"></a><span id="Fully_mapped_check"></span><span id="fully_mapped_check"></span><span id="FULLY_MAPPED_CHECK"></span>完全映射的检查


[  **CheckAccessFullyMapped**](https://msdn.microsoft.com/library/windows/desktop/dn292083) 函数解释从内存访问返回的状态并指示正在访问的所有数据是否已在资源中映射。 **CheckAccessFullyMapped** 在数据已映射的情况下返回 true (0xFFFFFFFF)，在数据未映射的情况下返回 false (0x00000000)。

在筛选操作期间，有时给定纹素的权重以 0.0 结束。 与直接在纹素 center 上的纹理坐标的线性示例是一个示例：3 （它们是哪些功能可能会因硬件而异） 其他纹素参与到筛选器，但与 0 的权重。 这些 0 权重纹素完全不影响筛选结果，因此如果它们恰好落在 **NULL** 磁贴上，则不算作未映射的访问。 请注意，相同保证适用于包含多个 mip 级别的纹理筛选器；如果其中一个 mipmap 上的纹素未映射但纹素权重为 0，则这些纹素不算作未映射的访问。

从具有少于 4 个组件的格式进行抽样时 (例如 DXGI\_格式\_R8\_UNORM)，都将位于任何纹素**NULL**磁贴中的结果**NULL**映射访问正在报告，而不考虑的着色器实际查看结果中的组件。 例如，读取 R8\_UNORM 和屏蔽.gba/.yzw 与着色器中的读取的结果似乎需要根本读取纹理。 但是，如果纹素地址为 **NULL** 映射磁贴，此操作仍算作 **NULL** 映射访问。

着色器可以检查状态，并针对失败情况采取任何需要的操作。 例如，操作可以是记录“未命中数”（例如通过 UAV 写入）和/或发出固定到已知已映射的粗粒度 LOD 的另一个读数。 应用程序可能还需跟踪成功的访问，以获知所访问的是映射磁贴组的哪一部分。

记录的复杂之处在于，目前尚无机制可用于报告已被访问的确切磁贴组。 应用程序可以根据已知用于访问的坐标并使用 LOD 指令进行保守猜测，例如，[**tex2Dlod**](https://msdn.microsoft.com/library/windows/desktop/bb509680) 将返回硬件 LOD 计算结果。

另一个难点是，大量访问将指向同一组磁贴，因此将产生大量冗余记录并可能引起内存争用。 如果能允许硬件在别处已报告磁贴访问的情况下不再重复报告，就会更加方便。 此类跟踪状态也许可以从 API（可能位于帧边界）重置。

## <a name="span-idper-sampleminlodclampspanspan-idper-sampleminlodclampspanspan-idper-sampleminlodclampspanper-sample-minlod-clamp"></a><span id="Per-sample_MinLOD_clamp"></span><span id="per-sample_minlod_clamp"></span><span id="PER-SAMPLE_MINLOD_CLAMP"></span>每个示例 MinLOD 次固定


为帮助着色器避开 Mip 映射流式资源中已知的非映射区域，使用采样器（筛选）的大多数着色器指令都有相应模式，允许着色器将额外的 float32 MinLOD 固定参数传递到纹理样本。 此值位于视图的 mipmap 数空间，而不是基础资源。

硬件在 LOD 计算中发生按资源 MinLOD 固定的相同位置执行 ` max(fShaderMinLODClamp,fComputedLOD) `，同时也是 [**max**](https://msdn.microsoft.com/library/windows/desktop/bb509624)()。

如果应用每个示例 LOD clamp 和采样器中定义的任何其他 LOD clamps 的结果为空集，结果是相同的超出边界访问结果作为每个资源 minLOD clamp:对组件中的图面上的格式和缺少的组件的默认值为 0。

先于此处所述的按样本 MinLOD 固定的 LOD 指令（例如 [**tex2Dlod**](https://msdn.microsoft.com/library/windows/desktop/bb509680)）将返回已固定和未固定的 LOD。 从此 LOD 指令返回的已固定 LOD 反映所有固定，包括按资源固定，但并非按样本固定。 按样本固定由着色器控制和知悉，因此着色器作者可在需要时为 LOD 指令的返回值手动应用固定。

## <a name="span-idminmaxreductionfilteringspanspan-idminmaxreductionfilteringspanspan-idminmaxreductionfilteringspanminmax-reduction-filtering"></a><span id="Min_Max_reduction_filtering"></span><span id="min_max_reduction_filtering"></span><span id="MIN_MAX_REDUCTION_FILTERING"></span>最小/最大减少筛选


应用程序可以选择管理自己的数据结构，通过此结构可了解流式资源的映射样式。 例如，包含一个纹素的图面含有某个流式资源中每个磁贴的信息。 可以存储在给定磁贴位置映射的第一个 LOD。 通过计划对流式资源采样的类似方式对此数据结构仔细采样，可以发现为整个纹理筛选器足迹完全映射的最小 LOD。 为帮助简化此过程，Direct3D 11.2 引入了新的通用采样器模式，即最小值/最大值筛选。

适用于 LOD 跟踪的最小值/最大值筛选也可用于其他目的，例如深度图面筛选。

最小值/最大值减少筛选是采样器的一种模式，可获取常规纹理筛选器获取的相同纹素组。 但是，它不会通过混合值来生成答案，而是会按各个组件返回所获取纹素的最小值 min() 或最大值 max()（例如，所有 R 值的最小值，单独表示的所有 G 值的最小值等）。

最小值/最大值运算遵循 Direct3D 算术精度规则。 比较顺序无关紧要。

在非最小值/最大值的筛选操作过程中，有时给定纹素的权重以 0.0 结束。 例如带有直接落入纹素中心的纹理坐标的线性样本；在这种情况下，3 个其他纹素（具体是哪些因硬件而变）将影响筛选器但权重为 0。 对于非最小值/最大值筛选中权重将为 0 的任何纹素，如果应用最小值/最大值筛选，这些纹素仍不会影响结果（且权重不会以其他方式影响最小值/最大值筛选操作）。

对此功能的支持取决于针对流式资源的[第 2 层](tier-2.md)支持。

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>相关主题


[管道到流式处理资源的访问权限](pipeline-access-to-streaming-resources.md)

 

 




