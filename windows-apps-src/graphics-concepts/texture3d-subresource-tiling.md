---
title: "Texture3D 子资源平铺"
description: "此表说明了 Texture3D 子资源的平铺方式。"
ms.assetid: 210D03E4-CF12-47E0-BA2F-C8D059B17D3E
keywords: "Texture3D 子资源平铺"
author: PeterTurcan
ms.author: pettur
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
ms.openlocfilehash: 3232ac03c2d0079faabff40b9325e819f8aca147
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="texture3d-subresource-tiling"></a>Texture3D 子资源平铺


此表说明了 [**Texture3D**](https://msdn.microsoft.com/library/windows/desktop/ff471562) 子资源的平铺方式。 此表中的值未计入尾部 mip 打包。

此表采用了 [**Texture2D**](https://msdn.microsoft.com/library/windows/desktop/ff471525) 平铺，将 x/y 尺寸分别除以 4，再加上 16 层的深度值。 第一平面的所有磁贴（二维平面的磁贴限定前 16 层的深度值）先于后面的平面出现。

**注意**  流式资源中的 [**Texture3D**](https://msdn.microsoft.com/library/windows/desktop/ff471562) 支持在流式资源的初步实现中并不显现，但所需的磁贴形状（以后的版本可能支持）会列在此处。

 

| 位数/像素（每像素选择 1 个示例） | 磁贴尺寸（像素，宽 x 高 x 深） |
|-----------------------------|---------------------------------|
| 8                           | 64x32x32                        |
| 16                          | 32x32x32                        |
| 32                          | 32x32x16                        |
| 64                          | 32x16x16                        |
| 128                         | 16x16x16                        |
| BC1,4                       | 128x64x16                       |
| BC2,3,5,6,7                 | 64x64x16                        |

 

流式资源不支持以下格式位数：96 位格式、视频格式、DXGI\_FORMAT\_R1\_UNORM、DXGI\_FORMAT\_R8G8\_B8G8\_UNORM 以及 DXGI\_FORMAT\_R8R8\_G8B8\_UNORM。

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>相关主题


[流式资源区域的平铺方式](how-a-streaming-resource-s-area-is-tiled.md)

 

 




