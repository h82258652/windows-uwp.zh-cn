---
title: 模板缓冲区
description: 模板缓冲区用于遮罩图像中的像素，以产生特殊效果。
ms.assetid: 544B3B9E-31E3-41DA-8081-CC3477447E94
keywords:
- 模板缓冲区
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 285e4a70062c57c957530aa1e548c22c4cf7711e
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2019
ms.locfileid: "57629462"
---
# <a name="stencil-buffers"></a>模板缓冲区


*模具缓冲区*用于遮罩图像中的像素，以产生特殊效果。 掩码控制是否绘制像素。 特殊效果包括合成、贴纸、溶解、淡化、滑动、轮廓描绘和剪影，以及双面模板。 一些较常见的效果如下所示。

模板缓冲区逐个像素地启用或禁用渲染目标图面绘制。 究其本质，它使应用程序遮罩部分渲染图像，因此这些部分不会显示。 应用程序常常使用模板缓冲区实现特殊效果，例如溶解、贴纸和轮廓描绘。

模板缓冲区信息嵌入在 z 缓冲区数据中。

## <a name="span-idhowthestencilbufferworksspanspan-idhowthestencilbufferworksspanspan-idhowthestencilbufferworksspanhow-the-stencil-buffer-works"></a><span id="How_the_Stencil_Buffer_Works"></span><span id="how_the_stencil_buffer_works"></span><span id="HOW_THE_STENCIL_BUFFER_WORKS"></span>模具缓冲区的工作原理


Direct3D 针对模板缓冲区的内容逐个像素地执行测试。 对于目标图面中的每个像素，它使用模板缓冲区中相应的值（模具参考值、模具掩码值）对其执行测试。 如果测试通过，Direct3D 将执行操作。 测试执行步骤如下。

1.  用模具掩码执行模具参考值的按位 AND 运算。
2.  用模具掩码执行当前像素的模板缓冲区值的按位 AND 运算。
3.  使用比较函数，对比第 1 步和第 2 步的结果。

上述步骤如以下代码行所示：

```
(StencilRef & StencilMask) CompFunc (StencilBufferValue & StencilMask)
```

-   StencilRef 表示模具参考值。
-   StencilMask 表示模具掩码值。
-   CompFunc 是比较函数。
-   StencilBufferValue 是当前像素的模板缓冲区内容。
-   与号 (&) 表示按位 AND 运算。

如果模具测试通过，当前像素将写入目标图面，否则将忽略。 默认比较行为是写入像素，无论每次按位运算的结果如何。若要更改此行为，你可以更改枚举类型值以确定所需的比较函数。

你的应用程序可以自定义设置模板缓冲区的运算。 它可以设置比较函数、模具掩码和模具参考值。 它还可以控制模具测试通过或失败时 Direct3D 采取的操作。

## <a name="span-idcompositingspanspan-idcompositingspanspan-idcompositingspancompositing"></a><span id="Compositing"></span><span id="compositing"></span><span id="COMPOSITING"></span>组合的情况下


你的应用程序可以使用模具缓冲区将 2D 或 3D 图像合成到 3D 场景中。 模具缓冲区中的掩码用于遮挡渲染目标图面的区域。 然后，可以将已存储 2D 信息（如文本或位图）写入遮挡区域。 或者，你的应用程序也可以将额外的 3D 基元渲染到渲染目标图面的已屏蔽模具区域。 它甚至可以渲染整个场景。

游戏通常会合成多个 3D 场景。 例如，赛车游戏通常会显示后视镜。 镜中呈现驾驶员身后的 3D 场景视图。 它本质上是与驾驶员前方视图合成的第二个 3D 场景。

## <a name="span-iddecalingspanspan-iddecalingspanspan-iddecalingspandecaling"></a><span id="Decaling"></span><span id="decaling"></span><span id="DECALING"></span>Decaling


Direct3D 应用程序使用贴纸来控制将特定基元图像中的哪些像素绘制到渲染目标图面。 应用程序将贴纸用于基元图像以使共面多边形正确渲染。

例如，在道路上应用轮胎压痕和黄线时，标记应显示直接在路面上显示。 但是，标记和道路的 z 值完全相同。 因此，深度缓冲区可能无法明确分离两者。 后部基元中的部分像素可能会渲染到前部基元上，反之亦然。 最后生成的图像看起来每一帧之间闪烁微光。 此效果称为 *z-fighting* 或 *flimmering*。

若要解决此问题，可使用模具屏蔽后部基元上显示贴纸的部分。 关闭 z 缓冲，并将前部基元的图像渲染至渲染目标曲面的已屏蔽区域。

尽管可使用多纹理混合来解决此问题，但这样做将限制应用程序可生成的其他特殊效果的数量。 使用模板缓冲区应用贴纸可释放纹理混合阶段以用于其他效果。

## <a name="span-iddissolvesfadesandswipesspanspan-iddissolvesfadesandswipesspanspan-iddissolvesfadesandswipesspandissolves-fades-and-swipes"></a><span id="Dissolves__fades__and_swipes"></span><span id="dissolves__fades__and_swipes"></span><span id="DISSOLVES__FADES__AND_SWIPES"></span>溶解、 淡出和扫


应用程序越来越多地使用电影和视频中常用的特殊效果，例如溶解、滑动和淡化。

在溶解效果中，一个图像经过平滑流畅的帧序列逐渐替换成另一个图像。 尽管 Direct3D 提供的多纹理混合方法可实现同样的效果，但使用模板缓冲区生成溶解效果的应用程序可以在溶解过程中将纹理混合功能用于其他效果。

当应用程序执行溶解效果时，它必须渲染两个不同的图像。 它使用模板缓冲区来控制将各图像中的哪些像素绘制到渲染目标图面。 你可以定义一系列模具掩码，并将它们复制到连续帧上的模板缓冲区中。 或者，你也可以为第一个帧定义一个基本模具掩码，然后递增更改。

溶解开始时，你的应用程序将设置模具函数和模具掩码，以使起始图像中的大部分像素通过模具测试。 结束图像中的大部分像素应在模具测试中失败。 在连续的帧上，模具掩码将会更新，因此起始图像中通过测试的像素将越来越少。 随着帧不断跳转，结束图像中未能通过测试的像素将越来越少。 通过这种方式，你的应用程序可使用任意溶解模式执行溶解。

淡入或淡出是一种特殊的溶解形式。 淡入时，模板缓冲区用于将黑色或白色图像溶解至渲染的 3D 场景。 淡出与之相反，应用程序将从渲染的 3D 场景开始，溶解至黑色或白色。 你可以采用任意模式来完成淡化。

Direct3D 应用程序使用类似的技术实现滑动效果。 例如，当应用程序执行从左到右滑动时，结束图像从左至右逐渐覆盖起始图像，显示于幻灯片上。 与溶解相同，你必须定义一系列模具掩码并将它们加载到连续帧上的模板缓冲区中，或逐帧修改起始模具掩码。 模具掩码用于禁用从起始图像写入像素以及启用从结束图像写入像素。

某种程度上，滑动比溶解更复杂，因为应用程序必须按照与滑动相反的方向从结束图像中读取像素。 也就是说，如果滑动是从左到右的，应用程序必须从右到左从结束图像中读取像素。

## <a name="span-idoutlinesandsilhouettesspanspan-idoutlinesandsilhouettesspanspan-idoutlinesandsilhouettesspanoutlines-and-silhouettes"></a><span id="Outlines_and_silhouettes"></span><span id="outlines_and_silhouettes"></span><span id="OUTLINES_AND_SILHOUETTES"></span>概述了和轮廓


你可以使用模具缓冲区生成更抽象的效果，例如轮廓与剪影。

如果应用程序将模具掩码应用于形状相同但略小的基元图像，则生成的图像仅包含基元的轮廓。 然后，应用程序可以使用纯色填充图像的模具屏蔽区域，使基元呈现浮雕般的外观。

如果模具掩码与你正在渲染的基元的大小和形状相同，则在生成的图像中，基元应在的位置有一个洞。 然后，应用程序可以用黑色填充这个洞，以生成基元剪影。

## <a name="span-idtwo-sidedstencilspanspan-idtwo-sidedstencilspanspan-idtwo-sidedstencilspantwo-sided-stencil"></a><span id="Two-sided_stencil"></span><span id="two-sided_stencil"></span><span id="TWO-SIDED_STENCIL"></span>双侧模具


阴影卷用于在模具缓冲区中绘制阴影。 应用程序通过遮挡几何图形计算阴影卷大小，方法是计算剪影边缘并将它们从光线中挤压到一组 3D 卷中。 然后，这些卷将两次渲染到模具缓冲区中。

第一次渲染绘制前向多边形，并增加模具缓冲区值。 第二次渲染绘制阴影卷的后向多边形，并减少模具缓冲区值。 通常，所有增加和减少值会相互抵消。但是，由于阴影卷已被渲染，使用常规几何图形渲染的场景会导致部分像素无法通过 z 缓冲测试。 留在模具缓冲区的值域阴影中像素相对应。 使用这些保留的模具缓冲区内容作为掩码，以在场景中 alpha 混合大面积、全环绕的黑色间隙。 将模具缓冲区作为掩码，结果是将位于阴影处的像素变暗。

这意味着每个光源的阴影几何图形将绘制两次，因此为 GPU 的顶点吞吐量带来了压力。 双面模具功能旨在缓解此问题。 这种方法使用两组模具状态（如下文所列），一组用于前向三角形，另一组用于后向三角形。 这样，每个阴影卷每个光源只需绘制一次。

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>相关主题


[深度和模具缓冲区](depth-and-stencil-buffers.md)

 

 




