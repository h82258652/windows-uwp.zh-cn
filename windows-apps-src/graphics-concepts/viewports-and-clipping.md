---
title: 视区和剪切
description: 视区是将 3D 场景投影到其中的二维 (2D) 矩形。
ms.assetid: D0DD646E-13AE-452A-AD22-8C35000D0BA9
keywords:
- 视区和剪切
author: michaelfromredmond
ms.author: mithom
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 4dd319c686bebf2a30431017f399f48b08618cb6
ms.sourcegitcommit: 086001cffaf436e6e4324761d59bcc5e598c15ea
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/27/2018
ms.locfileid: "5695075"
---
# <a name="viewports-and-clipping"></a>视区和剪切


*视区*是将 3D 场景投影到其中的二维 (2D) 矩形。 在 Direct3D 中，矩形在系统作为渲染目标使用的 Direct3D 表面内作为坐标存在。 投影转换将顶点转换为用于视区的坐标系统。 视区还用于指定将在其中呈现场景的呈现目标图面的深度值范围（通常为 0.0 至 1.0）。

## <a name="span-idtheviewingfrustumspanspan-idtheviewingfrustumspanspan-idtheviewingfrustumspanthe-viewing-frustum"></a><span id="The_Viewing_Frustum"></span><span id="the_viewing_frustum"></span><span id="THE_VIEWING_FRUSTUM"></span>视锥


视锥是场景中相对于视区的相机放置的 3D 体。 体的形状影响模型从相机空间投影到屏幕上的方式。 最常见投影类型是透视投影，它负责让靠近相机的对象显得比远处的对象大。 对于透视视见，可以用金字塔直观地显示视锥，其中相机位于下图中显示的尖部。 此金字塔与前后剪裁平面相交。 金字塔中的前后剪裁平面之间的体就是视锥。 对象仅在位于此体中时才可见。

![带前后剪裁平面的视锥的图示](images/frustum.png)

假设你站在昏暗的房间内通过一个正方形窗户向外看，你就直观地显示了视锥。 在这个类比中，近的剪裁平面是窗户，后剪裁平面是最终挡住你的视线的任何物体 - 跨过街道的摩天跨街景、远处的山峰或什么都没有。 你可以看到从窗户开始并以挡住你的视线的任何物体结尾的被截断的金字塔内的一切，并且看不到任何其他东西。z

视锥由 fov（视场）和前后剪裁平面之间的距离（以 z 坐标指定）定义，如下图所示。

![视锥的图示](images/fovdiag.png)

在此图中，变量 D 是从相机到在几何图形管道的最后部分中定义的空间的原点的距离 - 视见变换。 这是你安排视锥的限制围绕的空间。 有关如何使用此 D 变量构建投影矩阵的信息，请参阅[投影转换](projection-transform.md)

## <a name="span-idviewportrectanglespanspan-idviewportrectanglespanspan-idviewportrectanglespanviewport-rectangle"></a><span id="Viewport_Rectangle"></span><span id="viewport_rectangle"></span><span id="VIEWPORT_RECTANGLE"></span>视区矩形


视区结构包含四个成员（X、Y、Width、Height），这些成员定义了将场景呈现到的呈现目标图面区域。 这些值对应于目标矩形（即视区矩形），如下图所示。

![视区矩形的图示](images/destrect.png)

为 X 成员、Y 成员、Width 成员和 Height 成员指定的值是相对于呈现目标图面的左上角的屏幕坐标。 该结构定义了两个其他成员（MinZ 和 MaxZ），指示将场景呈现到的深度范围。

Direct3D 假定视区剪裁体范围在 X 方向为 -1.0 - 1.0，在 Y 方向为 1.0 - -1.0。这些是应用程序在过去最常使用的设置。 你可以在剪裁之前使用[投影转换](projection-transform.md)调整视区纵横比。

**注意** MinZ 和 MaxZ 表示将场景呈现到深度范围不用于剪裁。 大多数应用程序将这些值设置为 0.0 和 1.0 而言，以便让系统呈现到深度缓冲区中的整个深度值范围。 在某些情况下，你可以通过使用其他深度范围来实现特殊效果。 例如，要在游戏中呈现抬头显示，你可以将这两个值都设置为 0.0 以强制系统在前台呈现场景中的对象，或者也可以将它们都设置为 1.0 以呈现应始终位于后台的对象。

 

在视区结构的 X 成员、Y 成员、Width 成员和 Height 成员中使用的尺寸定义了呈现目标图面上的视区的位置和尺寸。 这些值位于屏幕坐标中，相对于图面的左上角。

Direct3D 使用视区位置和尺寸来缩放顶点，以便将呈现的场景放在目标图面上的合适位置。 在内部，Direct3D 将这些值插入应用于每个顶点的以下矩阵。

![应用于每个顶点的矩阵的方程](images/vpscale.png)

此矩阵根据视区尺寸和所需的深度范围缩放顶点，并将它们转换到平移目标图面上的合适位置。 此矩阵还将翻转 y 坐标以在左上角反射屏幕原点，同时让 y 向下增大。 应用此矩阵后，顶点仍然是同类的（即，它们仍作为 \[x,y,z,w\] 顶点存在），并且它们在发送到光栅器之前必须平移到非同类坐标。

**注意**应用程序通常将 MinZ 和 MaxZ 为 0.0 和 1.0 分别以使系统呈现到整个深度范围。 但是，你可以使用其他值实现某些效果。 例如，你可以将这两个值都设置为 0.0 以强制所有对象位于前台，或将这两个值设置为 1.0 以将所有对象呈现到后台。

 

## <a name="span-idclearingaviewportspanspan-idclearingaviewportspanspan-idclearingaviewportspanclearing-a-viewport"></a><span id="Clearing_a_Viewport"></span><span id="clearing_a_viewport"></span><span id="CLEARING_A_VIEWPORT"></span>清除视区


清除视区将重置呈现目标图面上的视区矩形的内容。 它还可以清除深度和模具缓冲区图面中的矩形。

## <a name="span-idsetuptheviewportforclippingspanspan-idsetuptheviewportforclippingspanspan-idsetuptheviewportforclippingspanset-up-the-viewport-for-clipping"></a><span id="Set_Up_the_Viewport_for_Clipping"></span><span id="set_up_the_viewport_for_clipping"></span><span id="SET_UP_THE_VIEWPORT_FOR_CLIPPING"></span>设置要剪裁的视区


投影矩阵的结果将确定投影空间中的剪裁体，如下所示：

-w<sub>c</sub>&lt;= x<sub>c</sub>&lt;= w<sub>c</sub>

-w<sub>c</sub>&lt;= y<sub>c</sub>&lt;= w<sub>c</sub>

0 &lt;= z<sub>c</sub>&lt;= w<sub>c</sub>

其中：x、y、z 和 w 表示应用投影转换后顶点坐标。 如果启用了剪裁，那么位于这些范围之外的具有 x、y 或 z 分量的所有顶点都会被剪裁掉。

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>相关主题


[坐标系和几何图形](coordinate-systems-and-geometry.md)

 

 




