---
title: 视图转换
description: 视图转换在世界空间中定位查看器，并将顶点转换为相机空间。
ms.assetid: DA4C2051-4C28-4ABF-8C06-241C8CB87F2F
keywords:
- 视图转换
author: michaelfromredmond
ms.author: mithom
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 40f9312b4b85e9f6e54a8bf02c6d07df35b8b626
ms.sourcegitcommit: 38f06f1714334273d865935d9afb80efffe97a17
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/09/2018
ms.locfileid: "6190821"
---
# <a name="view-transform"></a>视图转换


*视图转换* 在世界空间中定位查看器，并将顶点转换为相机空间。 在相机空间中，相机或查看器位于原点，并面向正 z 方向。 视图矩阵会在围绕相机的位置（相机空间的原点）和方向的世界空间中重新定位各个对象。 Direct3D 使用左手坐标系，因此 z 在场景中是正向的。

可通过多种方式创建视图矩阵。 在所有情况下，相机在世界空间中都有某个逻辑位置和方向，此位置和方向将用作起点来创建将应用于场景中的模型的视图矩阵。 视图矩阵会转换和旋转对象以将其放置在相机空间中，其中相机位于原点。 一种用于创建视图矩阵的方法是，将转换矩阵与每个轴的旋转矩阵组合在一起。 在此方法中，以下一般矩阵方程将适用。

![视图转换的方程](images/viewtran.png)

在此公式中，V 是将创建的视图矩阵，T 是在世界空间中重新定位对象的转换矩阵，Rₓ 到 R<sub>z</sub> 是沿 x 轴、y 轴和 z 轴旋转对象的旋转矩阵。 转换矩阵和旋转矩阵基于相机在世界空间中的逻辑位置和方向。 因此，如果相机在世界空间中的逻辑位置为 &lt;10,20,100&gt;，则旋转矩阵的目标是沿 x 轴将对象移动 -10 个单位，沿 y 轴将对象移动 -20 个单位以及沿 z 轴将对象移动 -100 个单位。 公式中的旋转矩阵基于相机的方向，这与旋转的相机空间轴偏离世界空间的程度有关。 例如，如果前面提及的相机笔直向下，则其 z 轴将与世界空间的 z 轴偏离 90 度（pi/2 弧度），如下图所示。

![相对于世界空间的相机视图空间的图示](images/camtop.png)

旋转矩阵向场景中的模型应用大小相同但方向相反的旋转。 此相机的视图矩阵包括围绕 x 轴旋转 -90 度。 旋转矩阵与转换矩阵组合使用可创建一个视图矩阵，该矩阵调整对象在场景中的位置和方向以使其顶部面向相机，并提供相机位于模型上方的外观。

## <a name="span-idsettingupaviewmatrixspanspan-idsettingupaviewmatrixspanspan-idsettingupaviewmatrixspansetting-up-a-view-matrix"></a><span id="Setting_Up_a_View_Matrix"></span><span id="setting_up_a_view_matrix"></span><span id="SETTING_UP_A_VIEW_MATRIX"></span>设置视图矩阵


Direct3D 使用世界矩阵和视图矩阵来配置多个内部数据结构。 每次设置新的世界矩阵或视图矩阵时，系统都将重新计算关联的内部结构。 从计算方面来看，频繁设置这些矩阵需要耗费大量时间。 你可以通过将世界矩阵和视图矩阵连接到设为世界矩阵的世界-视图矩阵，并将视图矩阵设置为标识来最大程度地减少所需的计算次数。 保留可修改和连接的单个世界矩阵和视图矩阵的缓存副本，并根据需要重置世界矩阵。

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>相关主题


[转换](transforms.md)

 

 




