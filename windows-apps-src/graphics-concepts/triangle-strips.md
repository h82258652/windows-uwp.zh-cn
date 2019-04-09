---
title: 三角形带
description: 三角形带是一系列连接的三角形。 由于这些三角形是连接的，因此应用程序不需要为每个三角形重复指定所有三个顶点。
ms.assetid: BACC74C5-14E5-4ECC-9139-C2FD1808DB82
keywords:
- 三角形带
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 62ad93fa480f0515c4ed6df2d73a745454197ac6
ms.sourcegitcommit: 82edc63a5b3623abce1d5e70d8e200a58dec673c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/20/2019
ms.locfileid: "58291635"
---
# <a name="triangle-strips"></a>三角形带

三角形带是一系列连接的三角形。 由于这些三角形是连接的，因此应用程序不需要为每个三角形重复指定所有三个顶点。 例如，只需 7 个顶点即可定义以下三角形带。

## <a name="span-idexamplespanspan-idexamplespanspan-idexamplespanexample"></a><span id="Example"></span><span id="example"></span><span id="EXAMPLE"></span>示例

![具有 7 个顶点的三角形带的图示](images/tristrip.png)

系统使用顶点 v1、v2 和 v3 绘制第一个三角形；使用 v2、v4 和 v3 绘制第二个三角形；使用 v3、v4 和 v5 绘制第三个三角形；使用 v4、v6 和 v5 绘制第四个三角形；依此类推。 请注意，第二个三角形和第四个三角形的顶点的顺序是颠倒的；这是确保按顺时针方向绘制所有三角形所必需的。

3D 场景中的大多数对象都由三角形带构成。 这是因为三角形带可用于以高效利用内存和处理时间的方式指定复杂对象。

下图描绘了已呈现的三角形带。

![已呈现的三角形带的图示](images/tstrip2.png)

以下代码说明如何为此三角形带创建顶点。

```cpp
struct CUSTOMVERTEX
{
float x,y,z;
};

CUSTOMVERTEX Vertices[] = 
{
    {-5.0, -5.0, 0.0},
    { 0.0,  5.0, 0.0},
    { 5.0, -5.0, 0.0},
    {10.0,  5.0, 0.0},
    {15.0, -5.0, 0.0},
    {20.0,  5.0, 0.0}
};
```

下面的代码示例说明如何在 Direct3D 中呈现此三角形带。

```cpp
//
// It is assumed that d3dDevice is a valid
// pointer to a device interface.
//
d3dDevice->DrawPrimitive( D3DPT_TRIANGLESTRIP, 0, 4);
```

## <a name="span-idpolygonsspanspan-idpolygonsspanspan-idpolygonsspanpolygons"></a><span id="Polygons"></span><span id="polygons"></span><span id="POLYGONS"></span>多边形


通常，三角形带用于构建多边形。 多边形是由至少三个顶点绘制的封闭 3D 图。 最简单的多边形是三角形。 Microsoft Direct3D 使用三角形构建其大多数多边形，因为可以保证三角形中的所有三个顶点共面。 呈现非平面顶点的效率很低。 你可组合三角形来构建较大的复杂多边形和网格。

下图显示了立方体。 两个三角形构成立方体的一个面。 整组三角形构成一个立方基元。 你可对基元的表面应用纹理以使其看上去像单个立体形状。 有关详细信息，请参阅[纹理](textures.md)。

![每个面上有两个三角形的立方体的图示](images/cube3d.png)

你还可使用三角形创建其表面似乎是平滑曲线的基元。 下图说明如何使用三角形模拟球体。 在应用材料后，可在呈现球体时使球体呈弧形。

![使用三角形模拟的球体的图示](images/sphere3d.png)

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>相关主题


[基元](primitives.md)

 

 




