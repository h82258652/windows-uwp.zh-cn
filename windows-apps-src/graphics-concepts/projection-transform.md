---
title: 投影转换
description: 投影转换控制相机的内部结构，如选择相机的镜头。 这是三种转换类型中最复杂的。
ms.assetid: 378F205D-3800-4477-9820-5EBE6528B14A
keywords:
- 投影转换
author: michaelfromredmond
ms.author: mithom
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 01a410e0e2759dcdfd6adff9c25238447fe4138b
ms.sourcegitcommit: e814a13978f33654d8e995584f4b047cb53e0aef
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/05/2018
ms.locfileid: "6042079"
---
# <a name="projection-transform"></a>投影转换


*投影转换* 控制相机的内部结构，如选择相机的镜头。 这是三种转换类型中最复杂的。

投影矩阵通常是缩放和透视投影。 投影转换可将视锥转换为长方体形状。 由于视锥的近端小于远端，这将产生拉伸靠近相机的对象的效果；这是透视应用于场景的方式。

在[视锥](viewports-and-clipping.md)中，相机与视见转换空间原点之间的距离被任意定义为 D，因此投影矩阵如下图所示。

![投影矩阵的图示](images/projmat1.png)

视见矩阵通过在 z 方向平移 - D 来将相机平移到原点。平移矩阵如下图所示。

![平移矩阵的图示](images/projmat2.png)

平移矩阵乘以投影矩阵 (T\*P) 将生成复合投影矩阵，如下图所示。

![复合投影矩阵的图示](images/projmat3.png)

透视转换可将视锥转换到新的坐标空间中。 请注意，视锥将变成长方体，原点将从场景的右上角移动到中间，如下图所示。

![透视转换将视锥更改到新坐标空间中的图示](images/cuboid.png)

在透视转换中，x 方向和 y 方向的极限分别为 -1 和 1。 z 方向的极限对于前平面为 0，对于后平面为 1。

此矩阵将根据从相机到近剪裁平面的指定距离平移和缩放对象，但它不会考虑视场 (fov)，并且它为该距离内的对象生成的 z 值可能几乎相同，从而加大了深度比较的难度。 以下矩阵将解决这些问题，该矩阵将调整顶点以实现视区的纵横比，从而使其成为透视投影的明智之选。

![透视投影的矩阵的图示](images/prjmatx1.png)

在此矩阵中，Zₙ 是仅剪裁平面的 z 值。 变量 w、h 和 Q 具有以下含义。 请注意，fov<sub>w</sub> 和 fovₖ 表示视区的水平和垂直视场（以弧度为单位）。

![变量含义的方程](images/prjmatx2.png)

对于你的应用程序，使用视场角度来定义 x 和 y 缩放系数可能没有使用视区的水平和垂直维度（在相机空间中）方便。 随着数学结果被解出，涉及 w 和 h 的以下两个方程将使用视区的维度并等效于上述方程。

![w 和 h 变量含义的方程](images/prjmatx3.png)

在这些公式中，Zₙ 表示近剪裁平面的位置，V<sub>w</sub> 和 Vₕ 变量表示视区在相机空间中的宽度和高度。

无论你决定使用哪个公式，请务必将 Zₙ 的值设置得尽可能大，因为非常靠近相机 z 值变化不会很大。 这在某种程度上增加了使用 16 位 z 缓冲区的深度比较得复杂度。

## <a name="span-idawfriendlyprojectionmatrixspanspan-idawfriendlyprojectionmatrixspanspan-idawfriendlyprojectionmatrixspana-w-friendly-projection-matrix"></a><span id="A_W_Friendly_Projection_Matrix"></span><span id="a_w_friendly_projection_matrix"></span><span id="A_W_FRIENDLY_PROJECTION_MATRIX"></span>w 友好投影矩阵


Direct3D 可使用已由世界矩阵、视图矩阵和投影矩阵转换的顶点的 w 分量在深度缓冲区或雾效果中执行基于深度的计算。 这样的计算要求你的投影矩阵将 w 规范化为与世界空间 z 等效。 简而言之，如果投影矩阵包含不为 1 的 (3,4) 系数，你必须以 (3,4) 系数的倒数为缩放度来缩放所有系数以生成正确的矩阵。 如果你不提供符合要求的矩阵，则雾效果和深度缓冲无法正确应用。

下图显示了不合要求的投影矩阵，相同的矩阵已缩放，以便启用相对于眼睛的雾化。

![不合要求的投影矩阵和带有相对于眼睛的雾化矩阵图示](images/eyerlmx.png)

在上述矩阵中，所有变量都假定为非零。 有关基于 w 的深度缓冲的信息，请参阅[深度缓冲区](depth-buffers.md)。

Direct3D 在其基于 w 的深度计算中使用当前设置的投影矩阵。 因此，应用程序必须设置符合要求的投影矩阵才能获得所需的基于 w 的功能，即使它们不使用 Direct3D 进行转换也是如此。

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>相关主题


[转换](transforms.md)

 

 




