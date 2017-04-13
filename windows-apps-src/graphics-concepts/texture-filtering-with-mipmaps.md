---
title: "使用 mipmap 进行纹理筛选"
description: "mipmap 是一种纹理序列，每个纹理都以逐步降低的分辨率表示同一图像。 mipmap 中各图像或各级的高度和宽度都比上一级小二次方。"
ms.assetid: 28E863A2-C776-40E4-8551-9851DF7EC93E
keywords: "使用 mipmap 进行纹理筛选"
author: PeterTurcan
ms.author: pettur
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
ms.openlocfilehash: 65c775a265f7c5a0b15f76d867a9403308fc7128
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="texture-filtering-with-mipmaps"></a>使用 mipmap 进行纹理筛选


*mipmap* 是一种纹理序列，每个纹理都以逐步降低的分辨率表示同一图像。 mipmap 中各图像或各级的高度和宽度都比上一级小二次方。 Mipmap 不需要为正方形。

高分辨率 mipmap 图像用来指示靠近用户的物体。 低分辨率图像用来指示看起来离得很远的物体。 Mip 映射可改善所呈现的纹理的质量，但会占用更多内存。

Direct3D 以附加表面链的形式呈现 mipmap。 分辨率最高的纹理位于链的最前面，并且将下一级的 mipmap 作为附件。 反过来，这一级又以下一级的 mipmap 作为附件，依此类推，直到分辨率最低的 mipmap 为止。

以下图示以这些层级为例。 位图纹理表示 3D 第一人称视角游戏中一个容器上的标志。 当创建为 mipmap 时，像素最高的纹理排在集合的第一位。 在 mipmap 集中，随后的每个纹理的高度和宽度都小二次方。 在此情况下，分辨率最高的 mipmap 的像素为 256x256。 下一个纹理的像素为 128x128。 链中最后一个纹理的像素为 64x64。

此标志与可见位置的距离最大。 如果用户开始离标志很远，则游戏将显示 mipmap 链中最小的纹理，在此情况下，纹理的像素为 64x64。

![图示为纹理像素为 64x64 的危险标志](images/mip1.jpg)

当用户的视点更接近标志时，游戏会使用 mipmap 链中分辨率逐渐提高的纹理。 下图中，像素分辨率为 128x128。

![图示为纹理像素为 128x128 的危险标志](images/mip2.jpg)

当用户视点在离标志的最小允许距离时，游戏使用分辨率最高的纹理，如下图所示。

![图示为纹理像素为 256x256 的危险标志](images/mip3.jpg)

使用这种方法模拟纹理的角度更为有效。 通过这种方法能更快地利用多个不同分辨率的纹理，而不是以多种分辨率呈现单个纹理。

Direct3D 可评估 mipmap 集中的哪个纹理的分辨率与所需的输出最为接近，而且可将像素映射到相应的纹素空间。 如果最终图像的分辨率介于 mipmap 集中的纹理的分辨率之间，则 Direct3D 可以检查两个 mipmap 中的纹素并混合它们的颜色值。

要使用 mipmap，应用程序必须构建一组 mipmap。 应用程序通过将 mipmap 集选作当前纹理集中的第一个纹理的方式应用 mipmap。 参见[纹理混合](texture-blending.md)。

接下来，应用程序必须设置 Direct3D 用于纹理采样的筛选方法。 最快速的 mipmap 筛选方法是让 Direct3D 选择最近的纹素。 在选择时，可以使用 D3DTEXF\_POINT 枚举值。 如果应用程序使用 D3DTEXF\_LINEAR 枚举值，则 Direct3D 的筛选效果更佳。 选择最接近的 mipmap，然后计算当前像素映射到的纹理周围的纹素的加权平均值。

Mipmap 纹理应用于 3D 场景中，可缩短呈现场景所需的时间。 Mipmap 纹理还可以增强场景的真实性， 但通常需要占用大量的内存。

**注意**   mipmap 链中每个表面的尺寸为链中前一个表面的尺寸的一半。 如果顶层 mipmap 的尺寸为 256x128，则第二层 mipmap 的尺寸为 128x64，第三层 mipmap 的尺寸为 64x32，依此类推，最低层 mipmap 的尺寸为 1x1。 链中任何 mipmap 的宽度或高度不得小于 1，因此你可以请求的 mipmap 层级在数量上存在限制。 以 4x2 顶层 mipmap 表面为例，允许的最大值为 3 层。 也就是，顶层 mipmap 的尺寸为 4x2，第二层 mipmap 的尺寸为 2x1，第三层 mipmap 的尺寸为 1x1。 如果值大于 3，会使第二层 mipmap 的高度变为小数值，因此这是不允许的。

 

Direct3D 可自动执行 mipmap 纹理筛选。 你可以通过应用程序，手动穿过 mipmap 链以便将位图数据加载到链中的每个表面。 这通常是移动纹理链的唯一理由。 由于 mipmap 驻留在视频存储器中，所以在纹理创建时自动生成 mipmap 利用了硬件筛选。

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>相关主题


[纹理筛选](texture-filtering.md)

 

 




