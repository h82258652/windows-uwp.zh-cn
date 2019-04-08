---
title: 坐标系统
description: 通常，3D 图形应用程序使用两种类型的笛卡尔坐标系统之一：左手坐标系统或右手坐标系统。 在这两个坐标系统中，正 x 轴指向右侧，正 y 轴指向上方。
ms.assetid: 138D9B81-146F-4E9F-B742-1EDED8FBF2AE
keywords:
- 坐标系统
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: f85bf490bd1dd68e2d0ba31335f2fc0f89fe27b0
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2019
ms.locfileid: "57655792"
---
# <a name="coordinate-systems"></a>坐标系统


通常，3D 图形应用程序使用两种类型的笛卡尔坐标系统之一：左手坐标系统或右手坐标系统。 在这两个坐标系统中，正 x 轴指向右侧，正 y 轴指向上方。

## <a name="span-idleftandrighthandedcoordinatesspanspan-idleftandrighthandedcoordinatesspanspan-idleftandrighthandedcoordinatesspanleft-and-right-handed-coordinates"></a><span id="Left_and_right_handed_coordinates"></span><span id="left_and_right_handed_coordinates"></span><span id="LEFT_AND_RIGHT_HANDED_COORDINATES"></span>从左和右传递坐标


通过将你的左手或右手手指指向正 x 方向，然后弯曲手指，将其指向正 y 方向，你可以记住正 z 轴指向的方向。 你的拇指指向的方向（无论指向你还是相反方向）就是正 z 轴为坐标系统所指的方向。 下面的图例显示了这两个坐标系统。

![左手和右手笛卡尔坐标系统图例](images/leftrght.png)

Direct3D 使用左手坐标系统。 尽管左手和右手坐标是最常用的系统，但是 3D 软件中还会使用多种其他坐标系统。 例如，3D 建模应用程序使用坐标系统并不少见，其中，y 轴指向查看器或相反方向，且 z 轴指向上方。

## <a name="span-idverticesandvectorsspanspan-idverticesandvectorsspanspan-idverticesandvectorsspanvertices-and-vectors"></a><span id="Vertices_and_vectors"></span><span id="vertices_and_vectors"></span><span id="VERTICES_AND_VECTORS"></span>顶点和向量


鉴于坐标系统，x、y 和 z 坐标可以定义空间（“顶点”）或 3D 方向（“矢量”）中的点。

顶点集合可用于定义直线和图形。 顶点可定义的最简单的对象称为[基元](primitives.md)，由一组基元定义的较复杂的对象称为“网格”。

在 3D 坐标系统中定义的网格上执行的必要操作是转换、旋转和缩放。 你可以组合这些基本转换以创建转换矩阵。 有关详细信息，请参阅[转换](transforms.md)。

组合这些操作时，结果不可交换；你乘以矩阵的顺序很重要。

## <a name="span-idportingfromaright-handedcoordinatesystemspanspan-idportingfromaright-handedcoordinatesystemspanspan-idportingfromaright-handedcoordinatesystemspanporting-from-a-right-handed-coordinate-system"></a><span id="Porting_from_a_right-handed_coordinate_system"></span><span id="porting_from_a_right-handed_coordinate_system"></span><span id="PORTING_FROM_A_RIGHT-HANDED_COORDINATE_SYSTEM"></span>从惯用右手坐标系统的迁移


如果移植基于右手坐标系统的应用程序，你必须对传递到 Direct3D 的数据进行两项更改：

-   调换三角形顶点的顺序以便系统从前面按顺时针方向遍历它们。 换言之，如果顶点是 v0、v1、v2，则以 v0、v2、v1 将它们传递到 Direct3D。
-   使用视图矩阵在 z 方向将世界空间缩放 -1。 若要执行此操作，翻转数的符号\_31， \_32 \_33，和\_34 用于视图矩阵的矩阵结构的成员。

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>相关主题


[坐标系统和几何结构](coordinate-systems-and-geometry.md)

 

 




