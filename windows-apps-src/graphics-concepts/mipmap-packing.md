---
title: Mipmap 打包
description: 可将一定数量的 mip（每个数组切片）打包到一定数量的磁贴中，这取决于流式资源的尺寸、格式、mipmap 的数量和数组切片。
ms.assetid: 906C3CAC-4E84-4947-B508-06788551BE85
keywords:
- Mipmap 打包
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 8b2733da1f843062a1fa7f2b4a7969326523d54e
ms.sourcegitcommit: d7613c791107f74b6a3dc12a372d9de916c0454b
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/05/2018
ms.locfileid: "8737940"
---
# <a name="mipmap-packing"></a>Mipmap 打包


可将一定数量的 mip（每个数组切片）打包到一定数量的磁贴中，这取决于流式资源的尺寸、格式、mipmap 的数量和数组切片。

根据流式资源支持的[层](streaming-resources-features-tiers.md)，具有特定维度的 mipmap 不遵循标准磁贴形状，并且被认为全部以对应用程序不透明的方式彼此打包在一起。 较高的支持层对于哪些类型的表面尺寸适合标准磁贴形状具有更广泛的保证（因此可以被应用程序单独映射）。

不同的实现可以存在以下差异：给定流式资源的维度、格式、mipmap 数量和数组切片，可将一定数量 M 的 mip（每个数组切片）打包到一定数量 N 的磁贴中。 当你获得设备的资源平铺信息时，驱动程序会向应用程序报告 M 和 N 是什么（以及有关所有硬件供应商都遵循的标准表面的其他详情）。 打包 mip 的磁贴组仍为 64KB，并且可以被单独映射到磁贴池中的不同位置。

但磁贴的像素形状以及 mipmap 映射到磁贴组的方式是特定于硬件供应商的（由于太过复杂，此处不予讲解）。 因此，应用程序需要映射指定为打包磁贴的所有磁贴，或者一个也不映射。 否则，访问流式资源的行为是未定义的。

对于数组化表面，打包 mip 组和存储这些 mip 的打包磁贴的数量（上文中所说的 M 和 N）为每个数组切片单独应用。

用于复制磁贴的专用 API 无法访问打包 mip。 要将数据复制到打包 mip 或从中复制数据的应用程序可以使用所有特定于非流式资源的 API 来执行此操作，以将它们复制和渲染到表面。

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>相关主题


[流式资源区域的平铺方式](how-a-streaming-resource-s-area-is-tiled.md)

 

 




