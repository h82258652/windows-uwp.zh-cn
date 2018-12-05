---
title: Texture3D 子资源平铺
description: 此表说明了 Texture3D 子资源的平铺方式。
ms.assetid: 210D03E4-CF12-47E0-BA2F-C8D059B17D3E
keywords:
- Texture3D 子资源平铺
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: c9c232bc60bbbb3cccc16618d82ec23452c58ee8
ms.sourcegitcommit: c01c29cd97f1cbf050950526e18e15823b6a12a0
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/05/2018
ms.locfileid: "8709561"
---
# <a name="texture3d-subresource-tiling"></a>Texture3D 子资源平铺


此表说明了 [**Texture3D**](https://msdn.microsoft.com/library/windows/desktop/ff471562) 子资源的平铺方式。 此表中的值未计入尾部 mip 打包。

此表采用了 [**Texture2D**](https://msdn.microsoft.com/library/windows/desktop/ff471525) 平铺，将 x/y 尺寸分别除以 4，再加上 16 层的深度值。 第一平面的所有磁贴（二维平面的磁贴限定前 16 层的深度值）先于后面的平面出现。

**注意**在流式资源的[**Texture3D**](https://msdn.microsoft.com/library/windows/desktop/ff471562)支持流式资源的初步实现中并不显现，但可能在未来版本中支持的所需的磁贴形状此处列出。

 

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

 

 




