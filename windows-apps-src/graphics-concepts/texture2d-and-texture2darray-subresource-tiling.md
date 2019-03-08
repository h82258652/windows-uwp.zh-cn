---
title: Texture2D 和 Texture2DArray 子资源平铺
description: 这些表说明了 Texture2D 和 Texture2DArray 子资源的平铺方式。
ms.assetid: 2DC14DFC-5299-44D9-895F-5A223D3FD530
keywords:
- Texture2D 和 Texture2DArray 子资源平铺
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 2175ce19824068a850ff70340b467f09e5c76540
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2019
ms.locfileid: "57592742"
---
# <a name="texture2d-and-texture2darray-subresource-tiling"></a>Texture2D 和 Texture2DArray 子资源平铺


这些表说明了 [**Texture2D**](https://msdn.microsoft.com/library/windows/desktop/ff471525) 和 [**Texture2DArray**](https://msdn.microsoft.com/library/windows/desktop/ff471526) 子资源的平铺方式。 这些表中的值未计入尾部 mip 打包。

## <a name="span-idsubresources-with-multisample-counts-of-1spanspan-idsubresources-with-multisample-counts-of-1spanspan-idsubresources-with-multisample-counts-of-1spansubresources-with-multisample-counts-of-1"></a><span id="Subresources-with-multisample-counts-of-1"></span><span id="subresources-with-multisample-counts-of-1"></span><span id="SUBRESOURCES-WITH-MULTISAMPLE-COUNTS-OF-1"></span>多重采样计数 1 与子


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

 

不支持流式处理资源的格式位计数是 96 bpp 格式，视频格式，DXGI\_格式\_R1\_UNORM、 DXGI\_格式\_R8G8\_B8G8\_UNORM，和 DXGI\_格式\_R8R8\_G8B8\_UNORM。

## <a name="span-idsubresources-with-various-multisample-countsspanspan-idsubresources-with-various-multisample-countsspanspan-idsubresources-with-various-multisample-countsspansubresources-with-various-multisample-counts"></a><span id="Subresources-with-various-multisample-counts"></span><span id="subresources-with-various-multisample-counts"></span><span id="SUBRESOURCES-WITH-VARIOUS-MULTISAMPLE-COUNTS"></span>子与各种多重采样计数


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


[如何平铺流式处理资源的区域](how-a-streaming-resource-s-area-is-tiled.md)

 

 




