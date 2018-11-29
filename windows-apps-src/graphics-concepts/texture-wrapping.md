---
title: 纹理环绕
description: 纹理环绕通过使用为每个顶点指定的纹理坐标，改变 Direct3D 光栅化纹理多边形的基本方式。
ms.assetid: C28FB369-9A91-4D57-A96D-4A5D36484B35
keywords:
- 纹理环绕
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 6618b7573be7cd39f703299b9418d1575297120e
ms.sourcegitcommit: b5c9c18e70625ab770946b8243f3465ee1013184
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/28/2018
ms.locfileid: "7973719"
---
# <a name="texture-wrapping"></a>纹理环绕


纹理环绕通过使用为每个顶点指定的纹理坐标，改变 Direct3D 光栅化纹理多边形的基本方式。 实行多边形光栅化时，系统会插入每个多边形顶点的纹理坐标之间，以确定应用于多边形的每个像素的纹素。 通常情况下，系统将纹理作为 2D 平面，并通过选择从纹理中 A 点到 B 点的最近路线插入新的纹素。如果 A 点表示 u、v 位置 (0.8, 0.1)，B 点位于 (0.1, 0.1)，则插入行如下图所示。

![图示为两点之间的插入行](images/interp1.png)

注意，本例中 A 点与 B 点之间的最短距离，大致通过纹理的中间位置。 启用 u 纹理或 v 纹理坐标环绕可改变 Direct3D 感知纹理坐标（u 方向及 v 方向）之间最近路线的方式。 根据定义，纹理环绕可使光栅器获取纹理坐标集（假定 0.0 和 1.0 的位置重合）之间的最近路线。 最后的位元是最棘手的部分：你可以想象往某个方向启用纹理环绕，以便系统处理纹理，就好像该纹理环绕在圆柱体周围。 如下图所示。

![图示为纹理以及环绕圆柱体的两个点](images/interp2.png)

上图显示在 u 方向的环绕如何影响系统插入纹理坐标的方式。 同样使用在正常或未环绕的纹理示例中的相同点，你可以看到 A 点和 B 点之间的最近路线不再经过纹理的中间位置，而是经过 0.0 和 1.0 都存在的边框位置。 在 v 方向的环绕与之类似，但其环绕的是一侧圆柱体周围的纹理。 在 u 方向以及 v 方向环绕更为复杂。 在此情况下，可以将纹理想象成圆环面或圆环图。

纹理环绕最常见的实际应用是进行环境绘图。 通常，采用环境映射的纹理对象会反光，并显示场景中物体周围事物的镜像。 为方便讨论，可以画出一个四面墙的房间，每面墙分别标上字母 R、G、B、Y，并涂上相应的颜色：红色、绿色、蓝色和黄色。 这个简单的房间的环境图如下图所示。

![图示为红色、绿色、蓝色和黄色的垂直条纹](images/envmap.png)

想象房间天花板是由一根反光性良好的四边形支柱支撑的。 将环境图纹理映射到支柱很简单，但要让支柱看起来好像反射字母和颜色可不是那么容易的事情。 下图所示为支柱线框，相应的纹理坐标列在靠近顶点的位置。 环绕与纹理边缘相交的接缝使用虚线表示。

![图示为使用平分虚线的矩形](images/seam.png)

通过在 u 方向启用环绕，纹理支柱可恰当地显示环境图中的颜色以及符号，且在纹理前面的接缝处，光栅器会正确选择纹理坐标之间的最近路线（假定 u 坐标 0.0 和 1.0 位置重合）。 纹理支柱如下图所示。

![图示为红色、绿色、蓝色和黄色各占 1/4 的支柱](images/tex-seam.png)

如果未启用纹理环绕，则光栅器不会在所需的方向插入以便于生成可信的反射图像。 相反，支柱前部区域包含横向压缩的位于 u 坐标值 0.175 和 0.875 之间的纹素，并且通过纹理的中间位置。 无法产生环绕效果。

不要将纹理环绕与名称类似的纹理寻址模式混淆。 纹理环绕先于纹理寻址发生。 确保纹理环绕数据不含任何超出 \[0.0, 1.0\] 范围的纹理坐标，以免产生不确定的结果。 有关纹理寻址的更多信息，请参见[纹理寻址模式](texture-addressing-modes.md)。

## <a name="span-iddisplacementmapwrappingspanspan-iddisplacementmapwrappingspanspan-iddisplacementmapwrappingspandisplacement-map-wrapping"></a><span id="Displacement_Map_Wrapping"></span><span id="displacement_map_wrapping"></span><span id="DISPLACEMENT_MAP_WRAPPING"></span>置换贴图环绕


置换贴图中可插入平面填充引擎。 由于无法指定平面填充引擎的环绕模式，因此无法使用置换贴图实施纹理环绕。 应用程序可以使用顶点集，以便强行内插以在任意方向环绕。 应用程序亦可将要进行的插入指定为简单的线性插入。

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>相关主题


[纹理](textures.md)

 

 




