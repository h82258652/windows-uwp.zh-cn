---
title: 需要流式资源
description: 流式资源可保证 GPU 内存不会浪费无法访问的表面存储区域，并告知硬件如何跨相邻的磁贴进行筛选。
ms.assetid: A88BE65B-104F-4176-9809-C12580A3684C
keywords:
- 需要流式资源
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 0e0354b0e727e84d562bf63779e74be72f87198f
ms.sourcegitcommit: 89ff8ff88ef58f4fe6d3b1368fe94f62e59118ad
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/29/2018
ms.locfileid: "8200565"
---
# <a name="the-need-for-streaming-resources"></a>需要流式资源


流式资源可保证 GPU 内存不会浪费无法访问的表面存储区域，并告知硬件如何跨相邻的磁贴进行筛选。

## <a name="span-idstreamingresourcesorsparsetexturesspanspan-idstreamingresourcesorsparsetexturesspanspan-idstreamingresourcesorsparsetexturesspanstreaming-resources-or-sparse-textures"></a><span id="Streaming_resources_or_sparse_textures"></span><span id="streaming_resources_or_sparse_textures"></span><span id="STREAMING_RESOURCES_OR_SPARSE_TEXTURES"></span>流式资源或稀疏纹理


*流式资源*（在 Direct3D 11 中称为*平铺资源*）是占用少量物理内存的庞大逻辑资源。

流式资源又称为*稀疏纹理*。 “稀疏”不仅传达资源的平铺性，而且传达平铺的主要原因，即并不需要一次映射所有的资源。 事实上，应用程序可以尝试制作流式资源，但在流式资源中并不特意针对资源的所有区域以及 mip 制作数据。 因此，内容本身可能很稀疏，并且图形处理单元 (GPU) 内存中特定时间的内容映射可能只是其中的一小部分（这部分可能更稀疏）。

## <a name="span-idwithouttilingmemoryallocationsaremanagedatsubresourcegranularityspanspan-idwithouttilingmemoryallocationsaremanagedatsubresourcegranularityspanspan-idwithouttilingmemoryallocationsaremanagedatsubresourcegranularityspanwithout-tiling-memory-allocations-are-managed-at-subresource-granularity"></a><span id="Without_tiling__memory_allocations_are_managed_at_subresource_granularity"></span><span id="without_tiling__memory_allocations_are_managed_at_subresource_granularity"></span><span id="WITHOUT_TILING__MEMORY_ALLOCATIONS_ARE_MANAGED_AT_SUBRESOURCE_GRANULARITY"></span>如果不平铺，则可以子资源粒度管理内存分配


在不支持流式资源的图形系统（即操作系统、显示驱动程序以及图形硬件）中，图形系统可以子资源粒度管理所有的 Direct3D 内存分配。

对于[缓冲区](introduction-to-buffers.md)，整个缓冲区都是子资源。

对于[纹理](textures.md)（例如 [**Texture2D**](https://msdn.microsoft.com/library/windows/desktop/ff471525)），每个 mip 层级都是子资源；对于纹理数组（例如  [**Texture2DArray**](https://msdn.microsoft.com/library/windows/desktop/ff471526)），特定数组片的每个 mip 层级都是子资源。 图形系统只能以该子资源粒度管理分配映射。 在流式资源的情况下，“映射”指使数据对 GPU 可见。

## <a name="span-idwithouttilingcantaccessonlyasmallportionofmipmapchainspanspan-idwithouttilingcantaccessonlyasmallportionofmipmapchainspanspan-idwithouttilingcantaccessonlyasmallportionofmipmapchainspanwithout-tiling-cant-access-only-a-small-portion-of-mipmap-chain"></a><span id="Without_tiling__can_t_access_only_a_small_portion_of_mipmap_chain"></span><span id="without_tiling__can_t_access_only_a_small_portion_of_mipmap_chain"></span><span id="WITHOUT_TILING__CAN_T_ACCESS_ONLY_A_SMALL_PORTION_OF_MIPMAP_CHAIN"></span>如果不平铺，就无法查看一小部分的 mipmap 链


假定应用程序了解特定的呈现操作只需要查看一小部分的 mipmap 链图像（可能只查看某个特定 mipmap 的部分区域）。 理想的情况下，应用程序最好要向图形系统告知此需求。 只有在为了确保在不占用太多内存的情况下在 GPU 上映射所需内存的情况下，图形系统才会干预。

事实上，在无流式资源支持的情况下，图形系统只能收到需要以子资源粒度在 GPU 上映射内存的通知（例如，只可以在全部 mipmap 层级的某个范围内查看）。 在图形系统方面也没有要求，因此可能需要额外使用大量的 GPU 内存，才能在执行呈现命令（参考任何部分的内存）之前映射完整的子资源。 有很多问题导致在没有流式资源支持的情况下难以在 Direct3D 中使用大量内存分配，而这只是其中一个问题。

## <a name="span-idsoftwarepagingtobreakthesurfaceintosmallertilesspanspan-idsoftwarepagingtobreakthesurfaceintosmallertilesspanspan-idsoftwarepagingtobreakthesurfaceintosmallertilesspansoftware-paging-to-break-the-surface-into-smaller-tiles"></a><span id="Software_paging_to_break_the_surface_into_smaller_tiles"></span><span id="software_paging_to_break_the_surface_into_smaller_tiles"></span><span id="SOFTWARE_PAGING_TO_BREAK_THE_SURFACE_INTO_SMALLER_TILES"></span>将表面分为若干更小的磁贴的软件编页


可通过软件编页，将表面分为若干足够小以便硬件处理的磁贴。

Direct3D 支持特定一侧的像素最高为 16384 的 [**Texture2D**](https://msdn.microsoft.com/library/windows/desktop/ff471525) 表面。 16384x16384（宽 x 高）的图像，每个像素为 4 字节，可占用 1GB 的视频内存（且添加 mipmap 可能使内存占用加倍）。 事实上，在单个呈现操作中很少需要用满 1GB。

一些游戏开发人员的地形表面建模尺寸相当大，为 128K x 128K。 为了使其适用于现有的 GPU，游戏开发人员会将表面分为若干足够小以便硬件处理的磁贴。 应用程序必须查明可能需要哪些磁贴，并将其加载到 GPU（软件编页系统）上的纹理缓存区。

这个方法存在一个很大的弊端，即硬件对当前进行的编页一无所知：当需要在跨磁贴的屏幕上显示部分图像时，硬件不知道如何执行固定功能，也就是不知道该如何高效地实施跨磁贴筛选。 这意味着，自主管理软件平铺的应用程序必须依靠使用着色器代码的手动纹理筛选（如果希望获得良好的各向异性筛选效果，代价将变得十分高昂），以及/或者耗用内存在磁贴周围创建栏距（包含相邻磁贴的数据），以便固定的硬件筛选功能继续有效。

## <a name="span-idmakingtiledrepresentationofsurfaceallocationsafirst-classfeaturespanspan-idmakingtiledrepresentationofsurfaceallocationsafirst-classfeaturespanspan-idmakingtiledrepresentationofsurfaceallocationsafirst-classfeaturespanmaking-tiled-representation-of-surface-allocations-a-first-class-feature"></a><span id="Making_tiled_representation_of_surface_allocations_a_first-class_feature"></span><span id="making_tiled_representation_of_surface_allocations_a_first-class_feature"></span><span id="MAKING_TILED_REPRESENTATION_OF_SURFACE_ALLOCATIONS_A_FIRST-CLASS_FEATURE"></span>使表面分配的平铺表示作为一级功能


如果表面分配的平铺表示是图形系统中的一级功能，则应用程序可告知硬件哪些磁贴可用。 通过这种方式只需用到很少的 GPU 内存（因为不会访问应用程序知道的表面存储区域），而且硬件了解如何在相邻磁贴间筛选，极大地方便了独立执行软件平铺的开发人员。

为了使解决方案更加完善，我们还必须有所作为，因为不考虑是否支持表面区域内平铺，表面尺寸目前最大为 16384（与应用程序希望的 128K+ 还相差甚远）。 一种方法是只需要硬件支持更大尺寸的纹理，但这种方法会产生高昂的成本并且/或者使效果大打折扣。

Direct3D 的纹理筛选路径以及呈现路径充分支持 16K 纹理并满足其他要求，比如支持在呈现过程中降离表面的视口范围，或者支持在筛选过程中脱离表面边缘环绕的纹理。 一种可能是确定一种折衷方案以便使纹理尺寸突破 16K，但这在一定程度上影响到功能性/精确性。 但即便作出此让步，也必须针对整个硬件系统的寻址能力投入额外的硬件成本，以便处理更大尺寸的纹理。

## <a name="span-idissuewithlargetexturesprecisionforlocationsonsurfacespanspan-idissuewithlargetexturesprecisionforlocationsonsurfacespanspan-idissuewithlargetexturesprecisionforlocationsonsurfacespanissue-with-large-textures-precision-for-locations-on-surface"></a><span id="Issue_with_large_textures__precision_for_locations_on_surface"></span><span id="issue_with_large_textures__precision_for_locations_on_surface"></span><span id="ISSUE_WITH_LARGE_TEXTURES__PRECISION_FOR_LOCATIONS_ON_SURFACE"></span>处理大纹理的问题：表面位置的精确性


当纹理变得非常大时，单精度浮点数纹理坐标（以及用以支持光栅化的相关内插）就不能准确指定表面位置。 接着，纹理筛选会出现抖晃。 解决方案是依靠双精度内插支持，但这样做成本高昂，但凡出现合理的替代方案，这个解决方案就会被取代。

## <a name="span-idenablingmultipleresourcesofdifferentdimensionstosharememoryspanspan-idenablingmultipleresourcesofdifferentdimensionstosharememoryspanspan-idenablingmultipleresourcesofdifferentdimensionstosharememoryspanenabling-multiple-resources-of-different-dimensions-to-share-memory"></a><span id="Enabling_multiple_resources_of_different_dimensions_to_share_memory"></span><span id="enabling_multiple_resources_of_different_dimensions_to_share_memory"></span><span id="ENABLING_MULTIPLE_RESOURCES_OF_DIFFERENT_DIMENSIONS_TO_SHARE_MEMORY"></span>启用多资源（不同大小）以共享内存


流式资源可能适用的另一个场景是启用多个大小/格式不同的资源以共享同一内存。 有时，应用程序包含已知不能同时使用的专用资源集，或者只为了短暂使用而创建且在创建其他资源之后即销毁的资源。 一种通用形式（可能不属于“流式资源”）是允许用户指向相同（重叠）内存中的多个不同的资源。 换言之，从应用程序角度看，“资源”（定义尺寸/格式等）的创建以及销毁可独立于内存管理（内存管理是资源的基础）。

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>相关主题


[流式资源](streaming-resources.md)

 

 




