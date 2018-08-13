---
title: 基本纹理概念
description: 早期的计算机生成的 3D 图像，尽管在当时通常很先进，却往往带有闪亮的塑料状外观。
ms.assetid: 3CA3905D-E837-48EB-A81F-319AA1C6537E
keywords:
- 基本纹理概念
author: michaelfromredmond
ms.author: mithom
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
ms.localizationpriority: medium
ms.openlocfilehash: 7bbcdd17643f64ada02ff1045e57c873f40b2c36
ms.sourcegitcommit: 897a111e8fc5d38d483800288ad01c523e924ef4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/13/2018
ms.locfileid: "1044686"
---
# <a name="basic-texturing-concepts"></a>基本纹理概念


早期的计算机生成的 3D 图像，尽管在当时通常很先进，往往有一个闪亮的塑料外观。 这类图像缺少磨损、裂缝、指纹和污迹等标记类型，所以 3D 对象不具有真实的视觉复杂性。 为了增强计算机生成的 3D 图像的真实性，纹理变得非常流行。

在日常使用中，单词纹理指的是对象的平滑度或粗糙度。 但是，在电脑图形中，纹理是为对象提供纹理外观的像素颜色的位图。

因为 Direct3D 纹理是位图，所以可以将任何位图应用于 Direct3D 基元。 例如，应用程序可以创建并操作看起来具有木纹图案的对象。 草、泥土和岩石可以应用于形成山的一组 3D 基元。 这样可以产生逼真的山坡。 你还可以使用纹理创建多种效果，比如路旁的标志、峭壁中的岩层或大理石地板的外观。

此外，Direct3D 支持更多高级纹理技术，比如具有透明度或不具有透明度的纹理混合和光映射。 请参阅[纹理混合](texture-blending.md)和[使用纹理的光映射](light-mapping-with-textures.md)。

如果你的应用程序创建某个硬件抽象层 (HAL) 设备或某个软件设备，那么它可以使用 8、16、24、32、64 或 128 位纹理。

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>相关主题


[纹理](textures.md)

 

 




