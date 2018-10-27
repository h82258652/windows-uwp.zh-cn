---
title: 顶点缓冲区视图 (VBV) 和索引缓冲区视图 (IBV)
description: 顶点缓冲区保留顶点列表的数据。
ms.assetid: 695115D2-9DA0-41F2-9416-33BFAB698129
keywords:
- 顶点缓冲区视图 (VBV)
author: michaelfromredmond
ms.author: mithom
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 7956c7a03256da04e98a5b8f9a33951d7e0216d3
ms.sourcegitcommit: 086001cffaf436e6e4324761d59bcc5e598c15ea
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/27/2018
ms.locfileid: "5705353"
---
# <a name="vertex-buffer-view-vbv-and-index-buffer-view-ibv"></a>顶点缓冲区视图 (VBV) 和索引缓冲区视图 (IBV)


顶点缓冲区保存顶点列表的数据。 每个顶点的数据都可能包含位置、颜色、法向矢量、纹理坐标等。 索引缓冲区将整数索引（偏移量）保存到顶点缓冲区中，并用于定义和呈现组成完整的顶点列表的一部分的对象。

单个顶点的定义通常是由应用程序负责，例如：

``` syntax
struct CUSTOMVERTEX { 
        FLOAT x, y, z;      // The position
        FLOAT nx, ny, nz;   // The normal
        DWORD color;        // RGBA color
        FLOAT tu, tv;       // The texture coordinates. 
}; 
```

CUSTOMVERTEX 的定义随后会在创建顶点缓冲区时传递到图形驱动程序。

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>相关主题


[视图](views.md)

 

 




