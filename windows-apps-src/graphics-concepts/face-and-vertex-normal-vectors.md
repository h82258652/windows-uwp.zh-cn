---
title: 面和顶点的法向矢量
description: 网格的每个面都有垂直单位法向矢量。 矢量方向取决于顶点的定义顺序和坐标系统是左手坐标还是右手坐标。
ms.assetid: 02333579-9749-4612-B121-23F97898A3E0
keywords:
- 面和顶点的法向矢量
author: michaelfromredmond
ms.author: mithom
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 2081e8c09a6f6fd75f460af3f339902bcb80bac6
ms.sourcegitcommit: 144f5f127fc4fbd852f2f6780ef26054192d68fc
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/02/2018
ms.locfileid: "5978481"
---
# <a name="face-and-vertex-normal-vectors"></a>面和顶点的法向矢量


网格的每个面都有垂直单位法向矢量。 矢量方向取决于顶点的定义顺序和坐标系统是左手坐标还是右手坐标。

## <a name="span-idperpendicularunitnormalvectorforafrontfacespanspan-idperpendicularunitnormalvectorforafrontfacespanspan-idperpendicularunitnormalvectorforafrontfacespanperpendicular-unit-normal-vector-for-a-front-face"></a><span id="Perpendicular_unit_normal_vector_for_a_front_face"></span><span id="perpendicular_unit_normal_vector_for_a_front_face"></span><span id="PERPENDICULAR_UNIT_NORMAL_VECTOR_FOR_A_FRONT_FACE"></span>正面的垂直单位法向矢量


网格的每个面都有垂直单位法向矢量。 矢量方向取决于顶点的定义顺序和坐标系统是左手坐标还是右手坐标。 面法线指离面的前端。 在 Direct3D 中，仅正面可见。 正面是指在其中按顺时针顺序定义顶点的面。

下图显示了正面的法向矢量：

![正面的法向矢量](images/nrmlvect.png)

## <a name="span-idcullingbackfacesspanspan-idcullingbackfacesspanspan-idcullingbackfacesspanculling-back-faces"></a><span id="Culling_back_faces"></span><span id="culling_back_faces"></span><span id="CULLING_BACK_FACES"></span>剔除背面


任何非正面的面都属于背面。 Direct3D 不总是渲染背面；一般要将背面剔除。 背面剔除指消除背面渲染。 如果需要，你可以更改剔除模式以渲染背面。 有关详细信息，请参阅[剔除状态](https://msdn.microsoft.com/library/windows/desktop/bb204882)。

## <a name="span-idvertexunitnormalsspanspan-idvertexunitnormalsspanspan-idvertexunitnormalsspanvertex-unit-normals"></a><span id="Vertex_unit_normals"></span><span id="vertex_unit_normals"></span><span id="VERTEX_UNIT_NORMALS"></span>顶点单位法线


Direct3D 使用顶点单位法线产生高氏着色、照明和纹理效果。

下图显示了顶点法线：

![顶点法线](images/vertnrml.png)

对多边形应用高氏阴影着色时，Direct3D 使用顶点法线计算光源和表面之间的角度。 它计算顶点的颜色和强度值，并为所有基元表面的每个点插入顶点。 Direct3D 采用此角度计算光线强度值。 角度越大，表面上发亮的光越少。

## <a name="span-idflatsurfacesspanspan-idflatsurfacesspanspan-idflatsurfacesspanflat-surfaces"></a><span id="Flat_surfaces"></span><span id="flat_surfaces"></span><span id="FLAT_SURFACES"></span>平面


如果你要创建平面对象，请将顶点法线设置为垂直于此表面。

下图显示了由两个带有顶点法线的三角形组成的平面：

![平面由两个带有顶点法线的三角形组成](images/flatvert.png)

## <a name="span-idsmoothshadingonanon-flatobjectspanspan-idsmoothshadingonanon-flatobjectspanspan-idsmoothshadingonanon-flatobjectspansmooth-shading-on-a-non-flat-object"></a><span id="Smooth_shading_on_a_non-flat_object"></span><span id="smooth_shading_on_a_non-flat_object"></span><span id="SMOOTH_SHADING_ON_A_NON-FLAT_OBJECT"></span>在非平面对象上平滑着色


你的对象很可能不是由平面，而是由三角形带组成，并且三角形不是共面的。 在三角形带中为所有三角形实现平滑着色的一个简单方法是，首先计算关联了矢量的每个多边形面的表面法向矢量。 可将顶点法线设置为与每个表面法线等角。 但是，对于复杂的基元来说，此方法可能不是很高效。

下图对此方法进行了说明，该图显示了从侧边上方看到的两个表面-S1 和 S2。 S1 和 S2 的法向矢量显示为蓝色。 顶点法向矢量显示为红色。 顶点法向矢量与 S1 表面法线的角与顶点法线和 S2 表面法线的角相同。 当照亮这两个表面并使用高氏着色进行着色时，两者之间形成平滑着色的平滑圆边。

下图显示了两个表面（S1 和 S2）及其法向矢量和顶点法向矢量：

![两个表面（s1 和 s2）及其法向矢量和顶点法向矢量](images/gvert.png)

如果顶点法线和与之关联的一个表面斜交，则会导致此表面上的点的光线强度增加或降低，具体取决于其与光源形成的角。 下图显示了一个示例。 从侧边可以看到这些表面。 顶点法线与 S1 斜交时与光源之间的角要小于其与两个表面法线形成的角相同时与光源之间的角。

下图显示了带有一个顶点法向矢量的两个表面（S1 和 S2）与某个面斜交：

![带有一个顶点法向矢量的两个表面（S1 和 S2）与某个面斜交](images/gvert2.png)

## <a name="span-idsharpedgesspanspan-idsharpedgesspanspan-idsharpedgesspansharp-edges"></a><span id="Sharp_edges"></span><span id="sharp_edges"></span><span id="SHARP_EDGES"></span>锐边


你可以使用高氏着色在 3D 场景中显示带锐边的对象。 为此，在需要锐边的情况下，面相交时重复顶线法向矢量。

下图显示了锐边上两个重复的顶点法向矢量：

![锐边上两个重复的顶点法向矢量](images/shade1.png)

如果你使用 DrawPrimitive 方法渲染你的场景，则需将带锐边的对象定义为三角形列表，而非三角形带。 将对象定义为三角形带时，Direct3D 会将其视为一个由多个三角形面组成的多边形。 同时为多边形的各个面和邻近面之间应用高氏着色。

最终将产生一个从面到面平滑着色的对象。 因为三角形列表是一个由一系列不相交的三角形面组成的多边形，Direct3D 将跨多边形的各个面应用高氏着色。 但是，它不会从面到面进行应用。 如果一个三角形列表中的两个或多个三角形邻近，则它们之间会形成一个锐边。

另一种做法是在渲染带锐边的对象时更改为平面着色。 这是借助计算的最有效方法，但它可能会导致场景中的对象的渲染逼真度低于应用髙氏着色的对象。

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>相关主题


[坐标系和几何图形](coordinate-systems-and-geometry.md)

 

 




