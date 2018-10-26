---
title: 多通道纹理混合
description: 通过在多通道呈现的过程中将不同纹理应用到基元，Direct3D 应用可以实现多个特殊效果。
ms.assetid: FB4D6E3F-4EF5-4D20-BF7E-1008E790E30C
keywords:
- 多通道纹理混合
author: michaelfromredmond
ms.author: mithom
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: c55f371e97daba5f81945812f8179eb708bbadd6
ms.sourcegitcommit: 6cc275f2151f78db40c11ace381ee2d35f0155f9
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/26/2018
ms.locfileid: "5567179"
---
# <a name="multipass-texture-blending"></a>多通道纹理混合


通过在多通道呈现的过程中将不同纹理应用到基元，Direct3D 应用程序可以实现多个特殊效果。 我们通用将此称为*多通道纹理混合*。 多通道纹理混合通常用于：通过应用几种不同纹理的多种颜色来模拟复杂照明及着色模型的效果。 比如*光照映射*。 请参阅[使用纹理的光映射](light-mapping-with-textures.md)。

**注意**某些设备都能向在单程基元应用多个纹理。 参见[纹理混合](texture-blending.md)。

 

如果用户的硬件不支持多纹理混合，你的应用程序可以使用多通道纹理混合来实现相同的视觉效果。 但是，当使用多纹理混合时，应用程序无法维持相同的帧速率。

在 C/C++ 应用程序中执行多通道纹理混合：

1.  在纹理阶段 0 中设置纹理。
2.  选择所需的颜色和 Alpha 混合参数及操作。 默认设置非常适合多通道纹理混合。
3.  渲染场景中的适当对象。
4.  在纹理阶段 0 中设置下一个纹理。
5.  设置渲染状态以根据需要调整源和目标混合因子。 系统根据这些参数将新纹理与渲染目标表面中的现有像素混合。
6.  重复步骤 3、4 和 5，根据需要使用尽可能多的纹理。

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>相关主题


[纹理混合](texture-blending.md)

 

 




