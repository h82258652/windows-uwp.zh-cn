---
title: 配置深度模具功能
description: 本节介绍了设置深度模具缓冲区的步骤以及输出合并器阶段的深度模具状态。
ms.assetid: B3F6CDAA-93ED-4DC1-8E69-972C557C7920
keywords:
- 配置深度模具功能
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 98cb6c62248fbf273a9d7ca1ef0d1d82293122eb
ms.sourcegitcommit: c01c29cd97f1cbf050950526e18e15823b6a12a0
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/05/2018
ms.locfileid: "8706910"
---
# <a name="span-iddirect3dconceptsconfiguringdepth-stencilfunctionalityspanconfiguring-depth-stencil-functionality"></a><span id="direct3dconcepts.configuring_depth-stencil_functionality"></span>配置深度模具功能


本节介绍了设置深度模具缓冲区的步骤以及输出合并器阶段的深度模具状态。

了解如何使用深度模具缓冲区和相应的深度模具状态，请参阅[高级模具技术](#advanced-stencil-techniques)。

## <a name="span-idcreatedepthstencilstatespanspan-idcreatedepthstencilstatespanspan-idcreatedepthstencilstatespancreate-depth-stencil-state"></a><span id="Create_Depth_Stencil_State"></span><span id="create_depth_stencil_state"></span><span id="CREATE_DEPTH_STENCIL_STATE"></span>创建深度模具状态


输出合并器阶段通过深度模具状态来判断如何执行[深度模具测试](https://msdn.microsoft.com/library/windows/desktop/bb205120)。 深度模具测试确定是否应该绘制给定像素。

## <a name="span-idbinddepthstenciltotheomstagespanspan-idbinddepthstenciltotheomstagespanspan-idbinddepthstenciltotheomstagespanbind-depth-stencil-data-to-the-om-stage"></a><span id="Bind_Depth_Stencil_to_the_OM_Stage"></span><span id="bind_depth_stencil_to_the_om_stage"></span><span id="BIND_DEPTH_STENCIL_TO_THE_OM_STAGE"></span>将深度模具数据绑定到 OM 阶段


绑定深度模具状态。

使用视图绑定深度模具资源。

呈现器目标必须全部是相同类型的资源。 如果使用多重采样反锯齿，则所有绑定的呈现器目标和深度缓冲区必须拥有相同的采样数量。

当使用缓冲区作为呈现器目标时，深度模具测试和多个呈现器目标不受支持。

-   可以同时绑定多达 8 个呈现器目标。
-   所有呈现器目标在所有维度上的尺寸必须相同（宽度和高度，3D 深度或 \*Array 类型的数组大小）。
-   每个呈现器目标都可能有不同的数据格式。
-   写掩码控制将哪些数据写入呈现器目标。 输出写掩码控制在每个呈现器目标、每个组件级别上将哪些数据写入呈现器目标。

## <a name="span-idadvancedstenciltechniquesspanspan-idadvancedstenciltechniquesspanspan-idadvancedstenciltechniquesspanspan-idadvanced-stencil-techniquesspanadvanced-stencil-techniques"></a><span id="Advanced_Stencil_Techniques"></span><span id="advanced_stencil_techniques"></span><span id="ADVANCED_STENCIL_TECHNIQUES"></span><span id="advanced-stencil-techniques"></span>高级模具技术


深度模具缓冲区的模具部分可用于创建合成、贴纸和轮廓等呈现效果。

-   [合成](#compositing)
-   [贴纸](#decaling)
-   [轮廓与剪影](#outlines-and-silhouettes)
-   [双面模具](#two-sided-stencil)
-   [将深度模具缓冲区读作纹理](#reading-the-depth-stencil-buffer-as-a-texture)

### <a name="span-idcompositingspanspan-idcompositingspanspan-idcompositingspancompositing"></a><span id="Compositing"></span><span id="compositing"></span><span id="COMPOSITING"></span>合成

你的应用程序可以使用模板缓冲区将 2D 或 3D 图像合成到 3D 场景中。 模具缓冲区中的掩码用于遮挡渲染目标图面的区域。 然后，可以将已存储 2D 信息（如文本或位图）写入遮挡区域。 或者，你的应用程序也可以将额外的 3D 基元渲染到渲染目标图面的已屏蔽模具区域。 它甚至可以渲染整个场景。

游戏通常会合成多个 3D 场景。 例如，赛车游戏通常会显示后视镜。 镜中呈现驾驶员身后的 3D 场景视图。 它本质上是与驾驶员前方视图合成的第二个 3D 场景。

### <a name="span-iddecalingspanspan-iddecalingspanspan-iddecalingspandecaling"></a><span id="Decaling"></span><span id="decaling"></span><span id="DECALING"></span>贴纸

Direct3D 应用程序使用贴纸来控制将特定基元图像中的哪些像素绘制到渲染目标图面。 应用程序将贴纸用于基元图像以使共面多边形正确渲染。

例如，在道路上应用轮胎压痕和黄线时，标记应显示直接在路面上显示。 但是，标记和道路的 z 值完全相同。 因此，深度缓冲区可能无法明确分离两者。 后部基元中的部分像素可能会渲染到前部基元上，反之亦然。 最后生成的图像看起来每一帧之间闪烁微光。 此效果称为 z-fighting 或 flimmering。

若要解决此问题，可使用模具屏蔽后部基元上显示贴纸的部分。 关闭 z 缓冲，并将前部基元的图像渲染至渲染目标曲面的已屏蔽区域。

可以使用多个纹理混合来解决此问题。

### <a name="span-idoutlinesandsilhouettesspanspan-idoutlinesandsilhouettesspanspan-idoutlinesandsilhouettesspanspan-idoutlines-and-silhouettesoutlines-and-silhouettes"></a><span id="Outlines_and_Silhouettes"></span><span id="outlines_and_silhouettes"></span><span id="OUTLINES_AND_SILHOUETTES"></span><span id="outlines-and-silhouettes">轮廓与剪影

你可以使用模具缓冲区生成更抽象的效果，例如轮廓与剪影。

如果你的应用程序具有两个呈现器通道，一个用于生成模具掩码，另一个用于将模具掩码应用于图像，但是第二个通道上的基元稍微小些，那么生成的图像将仅包含基元的轮廓。 然后，应用程序可以使用纯色填充图像的模具屏蔽区域，使基元呈现浮雕般的外观。

如果模具掩码与你正在渲染的基元的大小和形状相同，则在生成的图像中，基元应在的位置有一个洞。 然后，应用程序可以用黑色填充这个洞，以生成基元剪影。

### <a name="span-idtwosidedstencilspanspan-idtwosidedstencilspanspan-idtwosidedstencilspantwo-sided-stencil"></a><span id="Two_Sided_Stencil"></span><span id="two_sided_stencil"></span><span id="TWO_SIDED_STENCIL"></span>双面模具

阴影卷用于在模板缓冲区中绘制阴影。 应用程序通过遮挡几何图形计算阴影卷大小，方法是计算剪影边缘并将它们从光线中挤压到一组 3D 卷中。 然后，这些卷将两次渲染到模具缓冲区中。

第一次渲染绘制前向多边形，并增加模具缓冲区值。 第二次渲染绘制阴影卷的后向多边形，并减少模具缓冲区值。

通常情况下，所有增加和减少值相互抵消。但是，使用常规几何图形导致部分像素无法通过 z 缓冲测试，如呈现阴影卷已呈现场景。 留在模具缓冲区的值域阴影中像素相对应。 使用这些保留的模具缓冲区内容作为掩码，以在场景中 alpha 混合大面积、全环绕的黑色间隙。 将模具缓冲区作为掩码，结果是将位于阴影处的像素变暗。

这意味着每个光源的阴影几何图形将绘制两次，因此为 GPU 的顶点吞吐量带来了压力。 双面模具功能旨在缓解此问题。 这种方法使用两组模具状态（如下文所列），一组用于前向三角形，另一组用于后向三角形。 这样，每个阴影卷每个光源只需绘制一次。

### <a name="span-idreadingthedepth-stencilbufferasatexturespanspan-idreadingthedepth-stencilbufferasatexturespanspan-idreadingthedepth-stencilbufferasatexturespanspan-idreading-the-depth-stencil-buffer-as-a-texturespanreading-the-depth-stencil-buffer-as-a-texture"></a><span id="Reading_the_Depth-Stencil_Buffer_as_a_Texture"></span><span id="reading_the_depth-stencil_buffer_as_a_texture"></span><span id="READING_THE_DEPTH-STENCIL_BUFFER_AS_A_TEXTURE"></span><span id="reading-the-depth-stencil-buffer-as-a-texture"></span>将深度模具缓冲区读作纹理

着色器可以将非活动深度模具缓冲区读作纹理。 将深度模具缓冲区读作纹理的应用程序在两个通道中呈现，第一个通道写入深度模具缓冲区，第二个通道从缓冲区读取。 这样着色器可以将之前写入缓冲区的深度或模具值与当前正在呈现的像素值进行比较。 比较结果可用于创建效果，例如阴影贴图或粒子系统中的软粒子。

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>相关主题


[输出合并器 (OM) 阶段](output-merger-stage--om-.md)

[图形管道](graphics-pipeline.md)

[输出合并器阶段](https://msdn.microsoft.com/library/windows/desktop/bb205120)
