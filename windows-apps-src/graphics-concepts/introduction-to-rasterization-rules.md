---
title: "光栅化规则简介"
description: "通常，为顶点指定的点不与屏幕上的像素精确匹配。 发生这种情况时，Direct3D 应用三角形光栅化规则来决定将哪些像素应用到给定的三角形。"
ms.assetid: 4232CDBA-F669-4417-9378-F9013E83462C
keywords:
- "光栅化规则简介"
author: PeterTurcan
ms.author: pettur
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
translationtype: Human Translation
ms.sourcegitcommit: c6b64cff1bbebc8ba69bc6e03d34b69f85e798fc
ms.openlocfilehash: b7814c01d18e7a5ef121d151438a25921f35aaf2
ms.lasthandoff: 02/07/2017

---

# <a name="introduction-to-rasterization-rules"></a>光栅化规则简介


通常，为顶点指定的点不与屏幕上的像素精确匹配。 发生这种情况时，Direct3D 应用三角形光栅化规则来决定将哪些像素应用到给定的三角形。

这是对光栅化规则的简略介绍。 有关详细信息，请参阅[光栅化规则](rasterization-rules.md)。 另请参阅[光栅器 (RS) 阶段](rasterizer-stage--rs-.md)。

## <a name="span-idtrianglerasterizationrulesspanspan-idtrianglerasterizationrulesspanspan-idtrianglerasterizationrulesspantriangle-rasterization-rules"></a><span id="Triangle_Rasterization_Rules"></span><span id="triangle_rasterization_rules"></span><span id="TRIANGLE_RASTERIZATION_RULES"></span>三角形光栅化规则


Direct3D 使用左上填充约定来填充几何图形。 这与 GDI 和 OpenGL 中用于矩形的约定相同。 在 Direct3D 中，像素的中心是决策点。 如果中心位于三角形内，则像素是三角形的一部分。 像素中心位于整数坐标处。

Direct3D 使用的三角形光栅化规则的描述不一定适用于所有可用的硬件。 你在测试时可能会发现这些规则的实现有细微差异。

下图显示了一个矩形，其左上角位于 (0, 0)，右下角位于 (5, 5)。 正如你预期的那样，此矩形填充 25 个像素。 矩形的宽度定义为右边减去左边。 高度定义为底部减去顶部。

![分成六行和六列的带有编号的正方形](images/pixmap.png)

在左上填充约定中，*顶部*是指水平跨度的垂直位置，*左边*是指跨度内像素的水平位置。 边缘不能是上边缘，除非它是水平的。 一般说来，大多数三角形只有左边缘和右边缘。 下图显示了上边缘和右边缘。

![包含两个三角形的带有编号的正方形](images/triedge.png)

左上填充约定确定当三角形通过像素中心时 Direct3D 所采取的操作。 下图显示了两个三角形，一个位于 (0, 0)、(5, 0)、(5, 5) 处，另一个位于 (0, 5)、(0, 0)、(5, 5) 处。 在这种情况下，第一个三角形获得 15 个像素（以黑色显示），第二个三角形只获得 10 个像素（以灰色显示），因为共享边缘是第一个三角形的左边缘。

![显示两个三角形的带有编号的正方形](images/twotris.png)

如果你定义了一个左上角为 (0.5, 0.5)、右下角为 (2.5, 4.5) 的矩形，则此矩形的中心点为 (1.5, 2.5)。 当 Direct3D 光栅器网格化这个矩形时，每个像素的中心明确地位于四个三角形中每一个的内部，不需要使用左上填充约定。 下图显示了这一点。 Direct3D 将矩形中的像素包含到三角形中，并使用这些三角形对像素进行标记。

![一个带有编号的正方形，其中包含划分为四个三角形的矩形](images/noambig.png)

如果你移动上图中的矩形，使其左上角为 (1.0, 1.0)、右下角为 (3.0, 5.0)、中心点为 (2.0, 3.0)，则 Direct3D 将应用左上填充约定。 此矩形中的大多数像素跨越两个或多个三角形之间的边界，如下图所示。

![一个带有编号的正方形，其中包含划分为四个三角形的矩形](images/fillrule.png)

对于两个矩形，相同的像素会受到影响，如下图所示。

![受上图中两个带有编号的正方形影响的像素](images/samepix.png)

## <a name="span-idpointandlinerulesspanspan-idpointandlinerulesspanspan-idpointandlinerulesspanpoint-and-line-rules"></a><span id="Point_and_Line_Rules"></span><span id="point_and_line_rules"></span><span id="POINT_AND_LINE_RULES"></span>点和线规则


点的渲染方式与点子画面的渲染方式相同，它们都被渲染为与屏幕对齐的四边形，因此遵守与多边形渲染相同的规则。

非抗锯齿线的渲染规则与 [GDI 线](https://msdn.microsoft.com/library/windows/desktop/dd145027)的渲染规则完全相同。

## <a name="span-idpointspriterulesspanspan-idpointspriterulesspanspan-idpointspriterulesspanpoint-sprite-rules"></a><span id="Point_Sprite_Rules"></span><span id="point_sprite_rules"></span><span id="POINT_SPRITE_RULES"></span>点子画面规则


点子画面和面片基元按如下方式光栅化：首先将基元网格化成三角形，然后将生成的三角形光栅化。

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>相关主题


[设备](devices.md)

[光栅器 (RS) 阶段](rasterizer-stage--rs-.md)

[光栅化规则](rasterization-rules.md)

 

 





