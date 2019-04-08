---
title: 顶点缓冲区视图 (VBV) 和索引缓冲区视图 (IBV)
description: 顶点缓冲区保存顶点列表的数据。
ms.assetid: 695115D2-9DA0-41F2-9416-33BFAB698129
keywords:
- 顶点缓冲区视图 (VBV)
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: cfb92c4f876d85388ce325f151408fe7b9e8d8b4
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2019
ms.locfileid: "57636242"
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

 

 




