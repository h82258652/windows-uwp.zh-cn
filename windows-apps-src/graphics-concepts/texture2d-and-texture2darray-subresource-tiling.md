---
title: Texture2D 和 Texture2DArray 子资源平铺
description: 这些表说明了 Texture2D 和 Texture2DArray 子资源的平铺方式。
ms.assetid: 2DC14DFC-5299-44D9-895F-5A223D3FD530
keywords:
- Texture2D 和 Texture2DArray 子资源平铺
author: michaelfromredmond
ms.author: mithom
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 245581e4eb2a8526b242feadb5877590283e24f9
ms.sourcegitcommit: 753e0a7160a88830d9908b446ef0907cc71c64e7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/30/2018
ms.locfileid: "5761079"
---
# <a name="texture2d-and-texture2darray-subresource-tiling"></a>Texture2D 和 Texture2DArray 子资源平铺


这些表说明了 [**Texture2D**](https://msdn.microsoft.com/library/windows/desktop/ff471525) 和 [**Texture2DArray**](https://msdn.microsoft.com/library/windows/desktop/ff471526) 子资源的平铺方式。 这些表中的值未计入尾部 mip 打包。

## <a name="span-idsubresources-with-multisample-counts-of-1spanspan-idsubresources-with-multisample-counts-of-1spanspan-idsubresources-with-multisample-counts-of-1spansubresources-with-multisample-counts-of-1"></a><span id="Subresources-with-multisample-counts-of-1"></span><span id="subresources-with-multisample-counts-of-1"></span><span id="SUBRESOURCES-WITH-MULTISAMPLE-COUNTS-OF-1"></span>多重采样数为 1 的子资源


此表说明了多重采样计数为 1 的 [**Texture2D**](https://msdn.microsoft.com/library/windows/desktop/ff471525) 和 [**Texture2DArray**](https://msdn.microsoft.com/library/windows/desktop/ff471526) 子资源的平铺方式。

| 位数/像素（每像素选择 1 个示例） | 磁贴尺寸（像素，宽 x 高） |
|-----------------------------|-------------------------------|
| 8                           | 256x256                       |
| 16                          | 256x128                       |
| 32                          | 128x128                       |
| 64                          | 128x64                        |
| 128                         | 64x64                         |
| BC1,4                       | 512x256                       |
| BC2,3,5,6,7                 | 256x256                       |

 

流式资源不支持以下格式位数：96 位格式、视频格式、DXGI\_FORMAT\_R1\_UNORM、DXGI\_FORMAT\_R8G8\_B8G8\_UNORM 以及 DXGI\_FORMAT\_R8R8\_G8B8\_UNORM。

## <a name="span-idsubresources-with-various-multisample-countsspanspan-idsubresources-with-various-multisample-countsspanspan-idsubresources-with-various-multisample-countsspansubresources-with-various-multisample-counts"></a><span id="Subresources-with-various-multisample-counts"></span><span id="subresources-with-various-multisample-counts"></span><span id="SUBRESOURCES-WITH-VARIOUS-MULTISAMPLE-COUNTS"></span>多重采样数各不相同的子资源


此表说明了多重采样数各不相同的 [**Texture2D**](https://msdn.microsoft.com/library/windows/desktop/ff471525) 和 [**Texture2DArray**](https://msdn.microsoft.com/library/windows/desktop/ff471526) 子资源的平铺方式。

| 位数/像素（每像素选择 1 个示例） | 磁贴尺寸（像素，宽 x 高） |
|-----------------------------|-------------------------------|
| 1                           | 1x1                           |
| 2                           | 2x1                           |
| 4                           | 2x2                           |
| 8                           | 4x2                           |
| 16                          | 4x4                           |

 

流式资源只需要（及允许）采样数为 1 和 4。 流式资源当前不支持 2、8 和 16，即使它们显示。

即使流式资源不支持 2、8 和/或 16，在实现时，也可以选择支持针对非流式资源的 2、8 和/或 16 样本多重样本反混叠 (MSAA) 模式。

采样数大于 1 的流式资源不能使用 128 bpp 格式。

在支持的采样数及格式方面的限制归因于硬件不合规范。

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>相关主题


[流式资源区域的平铺方式](how-a-streaming-resource-s-area-is-tiled.md)

 

 




