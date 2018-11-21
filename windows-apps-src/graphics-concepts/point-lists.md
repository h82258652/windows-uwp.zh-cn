---
title: 点列表
description: 点列表是作为隔离的点呈现的顶点的集合。 你的应用程序可以在星图的 3D 场景中使用点列表，或在多边形的图面上使用虚线。
ms.assetid: 332954AE-019F-4A91-B773-E3A7C92F3297
keywords:
- 点列表
author: michaelfromredmond
ms.author: mithom
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: b4c2e0f6e99099df4bdda9452521acf3b0cd2b8f
ms.sourcegitcommit: cbe7cf620622a5e4df7414f9e38dfecec1cfca99
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/21/2018
ms.locfileid: "7443445"
---
# <a name="point-lists"></a>点列表


点列表是作为隔离的点呈现的顶点的集合。 你的应用程序可以在星图的 3D 场景中使用点列表，或在多边形的图面上使用虚线。

## <a name="span-idexamplespanspan-idexamplespanspan-idexamplespanexample"></a><span id="Example"></span><span id="example"></span><span id="EXAMPLE"></span>示例


下图描绘了呈现的点列表。

![点列表的图示](images/pointlst.png)

你的应用程序可能将材料和纹理应用于点列表。 材料或纹理的颜色仅在绘制的点上显示，不在两点之间的任何位置显示。

以下代码显示了如何为此点列表创建顶点。

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

下面的代码示例显示了如何在 Direct3D 中呈现此点列表。

```
//
// It is assumed that d3dDevice is a valid
// pointer to an IDirect3DDevice interface.
//
d3dDevice->DrawPrimitive( D3DPT_POINTLIST, 0, 6 );
```

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>相关主题


[基元](primitives.md)

 

 




