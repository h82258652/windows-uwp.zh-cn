---
title: 矩形
description: 在整个 Direct3D 和 Windows 编程中，将根据边界矩形引用屏幕上的对象。
ms.assetid: 3B78AE66-2C1A-4191-BDCA-D737E33460BA
keywords:
- 矩形
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: aa94eb00058ba3297e7ca7cc4f93581d9281fd1c
ms.sourcegitcommit: d7613c791107f74b6a3dc12a372d9de916c0454b
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/05/2018
ms.locfileid: "8749852"
---
# <a name="rectangles"></a>矩形


在整个 Direct3D 和 Windows 编程中，将根据边界矩形引用屏幕上的对象。 边界矩形的边始终与屏幕的边平行，始终可通过两个点（左上角和右下角）来描述矩形。

## <a name="span-idboundingrectanglesspanspan-idboundingrectanglesspanspan-idboundingrectanglesspanbounding-rectangles"></a><span id="Bounding_rectangles"></span><span id="bounding_rectangles"></span><span id="BOUNDING_RECTANGLES"></span>边界矩形


大多数应用程序使用 [**RECT**](https://msdn.microsoft.com/library/windows/desktop/dd162897) 结构（或其 typedef'd 别名）来传输有关在位块传输到屏幕或执行点击测试时要使用的边界矩形的信息。 在 C++ 中，**RECT** 结构具有以下定义。

```
typedef struct tagRECT { 
    LONG    left;    // This is the upper-left corner x-coordinate.
    LONG    top;     // The upper-left corner y-coordinate.
    LONG    right;   // The lower-right corner x-coordinate.
    LONG    bottom;  // The lower-right corner y-coordinate.
} RECT, *PRECT, NEAR *NPRECT, FAR *LPRECT; 
```

在上述示例中，左侧成员和顶部成员为边界矩形的左上角的 x 坐标和 y 坐标。 同样，右侧成员和底部成员构成了左下角的坐标。 以下示意图说明了你如何可视化这些值。

![由左侧值、顶部值、右侧值和底部值绑定的矩形的示意图](images/rect.png)

右边缘的像素列和下边缘的像素行未包含在 RECT 中。 例如，使用成员 {10, 10, 138, 138} 锁定 RECT 会生成宽度和高度为 128 像素的对象。

为了实现高效、一致性和易用性，所有 Direct3D 演示功能都使用矩形。

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>相关主题


[坐标系和几何图形](coordinate-systems-and-geometry.md)

 

 




