---
title: 三角形列表
description: 三角形列表是隔离的三角形的列表。 各个隔离的三角形可能相隔很近，也可能相隔不近。 三角形列表必须至少拥有 3 个顶点，并且顶点总数必须可以被 3 整除。
ms.assetid: BC50D532-9E9C-4AAE-B466-9E8C4AD1862A
keywords:
- 三角形列表
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: aae797db890c6bee141c3b4a79a6a85a55a6b512
ms.sourcegitcommit: c01c29cd97f1cbf050950526e18e15823b6a12a0
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/05/2018
ms.locfileid: "8704658"
---
# <a name="triangle-lists"></a>三角形列表


三角形列表是隔离的三角形的列表。 各个隔离的三角形可能相隔很近，也可能相隔不近。 三角形列表必须至少拥有 3 个顶点，并且顶点总数必须可以被 3 整除。

## <a name="span-idexamplespanspan-idexamplespanspan-idexamplespanexample"></a><span id="Example"></span><span id="example"></span><span id="EXAMPLE"></span>示例


使用三角形列表创建由非连续部分构成的对象。 例如，在 3D 游戏中创建力场墙的一种方式是指定大量小型的未连接三角形。 然后，应用似乎向三角形列表发射光的材料和纹理。 墙中的每个三角形似乎都发光。 墙后的场景通过三角形之间的空隙变得部分可见，因为玩家在查看力场时可能需要获得此效果。

三角形列表还可用于创建具有锐边并通过高氏着色来着色的基元。 请参阅[人脸和顶点的法线向量](face-and-vertex-normal-vectors.md)。

下图描绘了已呈现的三角形列表。

![已呈现三角形列表的图示](images/trilist.png)

以下代码说明如何为此三角形列表创建顶点。

```
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

下面的代码示例说明如何在 Direct3D 中呈现此三角形列表。

```
//
// It is assumed that d3dDevice is a valid
// pointer to a device interface.
//
d3dDevice->DrawPrimitive( D3DPT_TRIANGLELIST, 0, 2 );
```

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>相关主题


[基元](primitives.md)

 

 




