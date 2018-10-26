---
title: 单色光映射
description: 当较旧的 3D 加速器板不支持使用目标像素 Alpha 值的纹理混合时，单色光映射使较旧的适配器能够执行多通道纹理混合。
ms.assetid: 60F8F8F6-9DB7-452B-8DC0-407FFAA4BFE1
keywords:
- 单色光映射
author: michaelfromredmond
ms.author: mithom
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 72bea3badb19991883e2a6cc62463ab2dc58ec4a
ms.sourcegitcommit: 6cc275f2151f78db40c11ace381ee2d35f0155f9
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/26/2018
ms.locfileid: "5556319"
---
# <a name="monochrome-light-maps"></a>单色光映射


当较旧的 3D 加速器板不支持使用目标像素 Alpha 值的纹理混合时，单色光映射使较旧的适配器能够执行多通道纹理混合。

一些较旧的 3D 加速器板不支持使用目标像素的 Alpha 值的纹理混合。 这些适配器通常也不支持多纹理混合。 如果你的应用程序在此类适配器上运行，它可以使用多通道纹理混合来执行单色光映射。

要执行单色光映射，应用程序可将照明信息存储在其光照图纹理的 Alpha 数据中。 该应用程序使用 Direct3D 的纹理筛选功能来执行从基元图像中每个像素到光照图中相应纹素的映射。 它将源混合因子设为相应纹素的 Alpha 值。

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>相关主题


[使用纹理的光映射](light-mapping-with-textures.md)

 

 




