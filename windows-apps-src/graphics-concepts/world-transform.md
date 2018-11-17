---
title: 世界转换
description: 世界转换将坐标从相对模型的本地原点定义顶点的世界空间更改到世界空间。
ms.assetid: 767B032C-69D0-4583-8FEB-247F4C41E31D
keywords:
- 世界转换
author: michaelfromredmond
ms.author: mithom
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 9c6ada4bd964a9430d6ef47bd46f954ca76c404a
ms.sourcegitcommit: 9f8010fe67bb3372db1840de9f0be36097ed6258
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/16/2018
ms.locfileid: "7107193"
---
# <a name="world-transform"></a>世界转换


世界转换将坐标从模型空间（其中相对于模型的本地原点定义顶点）更改为世界空间。 在世界空间中，相对于场景中所有对象的共用原点定义顶点。 世界转换将模型放入世界空间中。

## <span id="What_Is_a_World_Transform"></span><span id="what_is_a_world_transform"></span><span id="WHAT_IS_A_WORLD_TRANSFORM"></span>


下图显示世界坐标系和模型的本地坐标系之间的关系。

![世界坐标和本地坐标的相关方式的图示](images/worldcrd.png)

世界转换可包含转换、旋转和缩放的任意组合。

## <a name="span-idsettingupaworldmatrixxmlspansetting-up-a-world-matrix"></a><span id="SETTING_UP_A_WORLD_MATRIX.XML"></span>设置世界矩阵


与任何其他转换一样，通过将一系列矩阵连接到一个包含其效果总计的矩阵来创建世界转换。 在最简单的情况下，当模型位于世界原点且其本地坐标轴的方向与世界空间的相同时，世界矩阵为标识矩阵。 更常见的情况是，世界矩阵是世界空间中的转换与根据需要对模型进行的一次或多次旋转的组合。

Direct3D 使用你设置的世界矩阵和视图矩阵来配置多个内部数据结构。 每当你设置新的世界矩阵或视图矩阵时，系统将重新计算关联的内部结构。 从计算方面来看，频繁设置这些矩阵（例如，每帧数千次）是非常耗时的。 你可以通过将世界矩阵和视图矩阵连接到设为世界矩阵的世界-视图矩阵，并将视图矩阵设置为标识来最大程度地减少所需的计算次数。 保留单个世界矩阵和视图矩阵的缓存副本，以便能根据需要修改、连接和重置世界矩阵。

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>相关主题


[转换](transforms.md)

 

 




