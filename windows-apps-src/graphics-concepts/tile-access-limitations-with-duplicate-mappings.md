---
title: 使用重复映射的磁贴访问限制
description: 使用重复映射的磁贴访问存在着限制，例如在复制源区域和目标区域重叠的流式资源时，或者在渲染至在渲染区内共享的磁贴时。
ms.assetid: 6E40B1DC-BCF1-4B09-82A8-7B2D9B209A61
keywords:
- 使用重复映射的磁贴访问限制
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: d5563a9909ba3d6cb3deaae43bcf9e55b4b2c880
ms.sourcegitcommit: b5c9c18e70625ab770946b8243f3465ee1013184
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/28/2018
ms.locfileid: "7966755"
---
# <a name="tile-access-limitations-with-duplicate-mappings"></a>使用重复映射的磁贴访问限制


使用重复映射的磁贴访问存在着限制，例如在复制源区域和目标区域重叠的流式资源时，或者在渲染至在渲染区内共享的磁贴时。

## <a name="span-idcopyingstreamingresourceswithoverlappingsourceanddestinationspanspan-idcopyingstreamingresourceswithoverlappingsourceanddestinationspanspan-idcopyingstreamingresourceswithoverlappingsourceanddestinationspancopying-streaming-resources-with-overlapping-source-and-destination"></a><span id="Copying_streaming_resources_with_overlapping_source_and_destination"></span><span id="copying_streaming_resources_with_overlapping_source_and_destination"></span><span id="COPYING_STREAMING_RESOURCES_WITH_OVERLAPPING_SOURCE_AND_DESTINATION"></span>复制源区域和目标区域重叠的流式资源


如果复制\*操作的源区域和目标区域中的磁贴在会重叠（即使资源不属于流式资源）的复制区域有重复映射，并且复制\*操作支持重叠复制，那么复制\*操作则不会受到影响（类似于源区域先复制到临时位置，再复制到目标区域）。 但是，如果重叠不明显（例如，源区域和目标区域虽不同，但共享映射，或者映射复制在特定表面），那么共享磁贴上的复制操作的结果则不明确。

## <a name="span-idcopyingtostreamingresourcewithduplicatedtilesindestinationareaspanspan-idcopyingtostreamingresourcewithduplicatedtilesindestinationareaspanspan-idcopyingtostreamingresourcewithduplicatedtilesindestinationareaspancopying-to-streaming-resource-with-duplicated-tiles-in-destination-area"></a><span id="Copying_to_streaming_resource_with_duplicated_tiles_in_destination_area"></span><span id="copying_to_streaming_resource_with_duplicated_tiles_in_destination_area"></span><span id="COPYING_TO_STREAMING_RESOURCE_WITH_DUPLICATED_TILES_IN_DESTINATION_AREA"></span>复制到目标区域中使用重复磁贴的流式资源


复制到目标区域中使用重复磁贴的流式资源会在这些磁贴中产生不明确的结果，除非数据本身完全相同；不同磁贴可能会以不同的顺序写入磁贴。

## <a name="span-iduavaccessestoduplicatetilesmappingsspanspan-iduavaccessestoduplicatetilesmappingsspanspan-iduavaccessestoduplicatetilesmappingsspanuav-accesses-to-duplicate-tiles-mappings"></a><span id="UAV_accesses_to_duplicate_tiles_mappings"></span><span id="uav_accesses_to_duplicate_tiles_mappings"></span><span id="UAV_ACCESSES_TO_DUPLICATE_TILES_MAPPINGS"></span>对重复磁贴映射的 UAV 访问


假设流式资源上的无序访问视图 (UAV) 在区域内有重复磁贴映射，或者有其他资源绑定至管道。 如果由不同线程执行，那么这些重复磁贴的访问排序则不明确，正如内存访问 UAV 的任何排序通常都是无序。

## <a name="span-idrenderingaftertilemappingchangesorcontentupdatesfromoutsidemappingsspanspan-idrenderingaftertilemappingchangesorcontentupdatesfromoutsidemappingsspanspan-idrenderingaftertilemappingchangesorcontentupdatesfromoutsidemappingsspanrendering-after-tile-mapping-changes-or-content-updates-from-outside-mappings"></a><span id="Rendering_after_tile_mapping_changes_or_content_updates_from_outside_mappings"></span><span id="rendering_after_tile_mapping_changes_or_content_updates_from_outside_mappings"></span><span id="RENDERING_AFTER_TILE_MAPPING_CHANGES_OR_CONTENT_UPDATES_FROM_OUTSIDE_MAPPINGS"></span>磁贴映射变更或从映射外部更新内容后的渲染


如果流式资源的磁贴映射已更改或者映射平铺池磁贴的内容已通过其他流式资源的映射发生更改，并且流式资源即将通过渲染目标视图或深度模具视图进行渲染，那么应用程序必须清除（使用固定功能 Clear API）或使用 Copy\*/Update\* API 全面复制到在渲染区域（映射或非映射）内更改的磁贴。

在这些情况下，如果应用程序未清除或复制，则会导致特定渲染目标视图或深度模具视图的硬件优化结构失效，并将导致某些硬件上出现糟糕的渲染结果以及不同硬件之间的不一致。 硬件使用的这些隐藏优化数据结构可能存在于各个映射的本地，并且对相同内存的其他映射不可见。

在清除资源视图（将资源中的所有元素设为一个值）时，你可清除含有矩形的渲染目标视图。 对于支持流式资源的硬件，清除资源视图必须还支持在仅深度表面（无模具）上清除含有矩形的深度模具视图。 这项操作可让应用程序仅清除表面的必要区域。

如果应用程序需要保留映射变更的流式资源中区域的内存内容，则必须绕过清除要求。 应用程序可首先保存磁贴映射变更的区域的内容（将其复制到临时表面），发布必要的清除命令，然后将内容复制回来，即可绕过清除要求。 尽管这能够完成保留表面内容的任务，进而实现增量渲染，但缺点是表面上后续的渲染性能会受到影响，因为渲染优化可能会丢失。

如果磁贴同时映射至多个流式资源，且通过其中一个流式资源以任何方式（渲染、复制等）操作磁贴内容，那么若要通过任何其他流式资源渲染相同磁贴，则首先必须按照上述步骤清除磁贴。

## <a name="span-idrenderingtotilessharedoutsiderenderareaspanspan-idrenderingtotilessharedoutsiderenderareaspanspan-idrenderingtotilessharedoutsiderenderareaspanrendering-to-tiles-shared-outside-render-area"></a><span id="Rendering_to_tiles_shared_outside_render_area"></span><span id="rendering_to_tiles_shared_outside_render_area"></span><span id="RENDERING_TO_TILES_SHARED_OUTSIDE_RENDER_AREA"></span>渲染至在渲染区外共享的磁贴


假设流式资源中的某个区域正在从渲染区向外部渲染，并且该渲染区引用的磁贴池磁贴也从渲染区向外部映射（包括同时或非同时通过其他流式资源）。 即使底层内存布局兼容，在通过其他映射查看时，渲染至这些磁贴的数据也不一定会正确呈现。 这种现象是因为某些硬件所使用的优化数据结构存在于可渲染表面的各个映射的本地，对相同内存位置的其他映射不可见。

你可从渲染映射复制到可访问的相同内存的所有其他映射（如果不再需要旧内容，则可以清理该内存或将其他数据复制到其中），进而绕开这种限制。 尽管看似多余，但是这种绕开限制的方法可让相同内存的所有其他映射正确了解如何访问其内容，至少保证只需一个物理内存备份所节约的内存的完整性。

此外，当你在使用共享映射（除非仅读取）的不同流式资源之间切换时，你必须在切换之间指定多个平铺资源之间的数据访问排序限制（障碍）。

## <a name="span-idrenderingtotilessharedwithinrenderareaspanspan-idrenderingtotilessharedwithinrenderareaspanspan-idrenderingtotilessharedwithinrenderareaspanrendering-to-tiles-shared-within-render-area"></a><span id="Rendering_to_tiles_shared_within_render_area"></span><span id="rendering_to_tiles_shared_within_render_area"></span><span id="RENDERING_TO_TILES_SHARED_WITHIN_RENDER_AREA"></span>渲染至在渲染区内共享的磁贴


如果流式资源中的某个区域的渲染位置与渲染区内多个磁贴的映射位置是同一个磁贴池位置，那么这些磁贴上的渲染结果则不明确。

## <a name="span-iddatacompatibilityacrossstreamingresourcessharingtilesspanspan-iddatacompatibilityacrossstreamingresourcessharingtilesspanspan-iddatacompatibilityacrossstreamingresourcessharingtilesspandata-compatibility-across-streaming-resources-sharing-tiles"></a><span id="Data_compatibility_across_streaming_resources_sharing_tiles"></span><span id="data_compatibility_across_streaming_resources_sharing_tiles"></span><span id="DATA_COMPATIBILITY_ACROSS_STREAMING_RESOURCES_SHARING_TILES"></span>流式资源共享磁贴之间的数据兼容性


假设多个流式资源映射至相同的磁贴池位置，并且每个资源均用于访问相同的数据。 如果关于避免硬件优化结构问题的其他规定得以避免、相应障碍得以确定（指定多个平铺资源之间的数据访问排序限制）并且流式资源互相兼容，这种情况才会发生。

对于后者，我们会在本文中讨论流式资源共享磁贴不兼容的影响。 访问重复磁贴映射上相同数据的不兼容条件是使用不同的表面尺寸或格式，或者资源上渲染目标或深度模具绑定标记的差异。 如果你之后通过来自不兼容资源的映射进行读取或渲染，那么以一种映射写入内存会产生不明确的结果。

如果其他资源共享映射首先使用新数据进行初始化（循环内存用于不同目的），那么由于数据并未在不兼容的解释中流出，因此后续的读取或渲染操作不受影响。 但是，当你在访问此类不兼容映射之间切换时，你必须指定障碍（指定多个平铺资源之间的数据访问排序限制）。

如果渲染目标或深度模具绑定标记在任何相互的资源共享映射上不固定，则限制会少得多。 只要格式和表面类型（例如 Texture2D）相同，就可以共享磁贴。 不同格式兼容的情况有 BC\* 表面和同等大小的未压缩 32 位或 16 位逐组件格式，例如 BC6H 和 R32G32B32A32。 很多 32 位逐元素格式也可使用 R32\_\* 作为别名（R10G10B10A2\_\*、R8G8B8A8\_\*、B8G8R8A8\_\*、B8G8R8X8\_\*、R16G16\_\*）；非流式资源均可执行这项操作。

如果格式相兼容，并且磁贴填充纯色，则不影响填充和非填充磁贴之间的共享。

最后，如果共享磁贴映射的资源之间没有什么共同点（任何资源都没有渲染目标或深度模具绑定标记的情况除外），那么只能安全共享填充为 0 的内存；对于指定资源格式的定义（通常为 0），映射将呈现为 0 所解码的任何内容。

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>相关主题


[对流式资源的管道访问](pipeline-access-to-streaming-resources.md)

 

 




