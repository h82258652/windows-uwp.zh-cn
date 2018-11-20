---
title: 线条带
description: 线条带是由互连的线段组成的基元。 你的应用程序可使用线条带创建不封闭的多边形。 封闭多边形是通过线段将最后一个顶点连接到第一个顶点的多边形。
ms.assetid: 6E8C58E1-B463-44FD-A69F-81CCBF25D856
keywords:
- 线条带
author: michaelfromredmond
ms.author: mithom
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: a5fbf4d7fd4f82e6bc44795d64e6b98b6c732f49
ms.sourcegitcommit: ed0304b8a214c03b8aab74b8ef12c9f82b8e3c5f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/19/2018
ms.locfileid: "7296242"
---
# <a name="line-strips"></a>线条带


线条带是由互连的线段组成的基元。 你的应用程序可使用线条带创建不封闭的多边形。 封闭多边形是通过线段将最后一个顶点连接到第一个顶点的多边形。 如果你的应用程序基于线条带生成多边形，则不能保证顶点共面。

## <a name="span-idexamplespanspan-idexamplespanspan-idexamplespanexample"></a><span id="Example"></span><span id="example"></span><span id="EXAMPLE"></span>示例


下图显示了渲染的线条带。

![线条带示意图](images/linstrip.gif)

下面的代码显示如何为此线条带创建顶点。

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

下面的代码示例显示如何在 Direct3D 中渲染线条带。

```
//
// It is assumed that d3dDevice is a valid
// pointer to an IDirect3DDevice interface.
//
d3dDevice->DrawPrimitive( D3DPT_LINESTRIP, 0, 5 );
```

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>相关主题


[基元](primitives.md)

 

 




