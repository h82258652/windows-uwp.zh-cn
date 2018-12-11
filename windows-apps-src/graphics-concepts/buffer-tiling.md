---
title: 缓冲区平铺
description: 一个缓冲区资源分为多个 64KB 的磁贴，如果最后一个磁贴的大小不是 64KB 的倍数，其中会有一些空白空间。
ms.assetid: 577DC6B0-F373-4748-AD80-2784C597C366
keywords:
- 缓冲区平铺
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: f3e5e117e05cef478ede508240a6b1d1022dea70
ms.sourcegitcommit: 8921a9cc0dd3e5665345ae8eca7ab7aeb83ccc6f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/11/2018
ms.locfileid: "8887116"
---
# <a name="buffer-tiling"></a>缓冲区平铺


一个[缓冲器](introduction-to-buffers.md)资源分为多个 64KB 的磁贴，如果最后一个磁贴的大小不是 64KB 的倍数，其中会有一些空白空间。

结构化的缓冲区对要平铺的步幅不能有约束。 但在第一时间将其平铺会牺牲使用 [**StructuredBuffers**](https://msdn.microsoft.com/library/windows/desktop/ff471514) 时可能的硬件性能优化。

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>相关主题


[流式资源区域的平铺方式](how-a-streaming-resource-s-area-is-tiled.md)

 

 




