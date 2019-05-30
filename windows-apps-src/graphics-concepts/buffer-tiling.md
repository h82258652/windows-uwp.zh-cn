---
title: 缓冲区平铺
description: 一个缓冲区资源分为多个 64KB 的磁贴，如果最后一个磁贴的大小不是 64KB 的倍数，其中会有一些空白空间。
ms.assetid: 577DC6B0-F373-4748-AD80-2784C597C366
keywords:
- 缓冲区平铺
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 34201c889ed984b27d50126bd2a04e9b320a17a3
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/29/2019
ms.locfileid: "66370656"
---
# <a name="buffer-tiling"></a>缓冲区平铺


一个[缓冲区](introduction-to-buffers.md)资源分为多个 64KB 的磁贴，如果最后一个磁贴的大小不是 64KB 的倍数，其中会有一些空白空间。

结构化的缓冲区对要平铺的步幅不能有约束。 但在第一时间将其平铺会牺牲使用 [**StructuredBuffers**](https://docs.microsoft.com/windows/desktop/direct3dhlsl/sm5-object-structuredbuffer) 时可能的硬件性能优化。

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>相关主题


[如何平铺流式处理资源的区域](how-a-streaming-resource-s-area-is-tiled.md)

 

 




