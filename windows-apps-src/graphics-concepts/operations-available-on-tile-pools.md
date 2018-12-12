---
title: 可用于磁贴池的操作
description: 可对磁贴池执行的操作包括调整磁贴池的大小、提供资源（为整个磁贴池临时向系统给予内存）以及回收资源。
ms.assetid: 90347F7F-C991-4287-BD70-494533ECDC8A
keywords:
- 可用于磁贴池的操作
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 6b8b0c6f4fa578e4ec483492b320dc9bc346ab66
ms.sourcegitcommit: 49d58bc66c1c9f2a4f81473bcb25af79e2b1088d
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/11/2018
ms.locfileid: "8929733"
---
# <a name="operations-available-on-tile-pools"></a>可用于磁贴池的操作


可对磁贴池执行的操作包括调整磁贴池的大小、提供资源（为整个磁贴池临时向系统给予内存）以及回收资源。

-   磁贴池生命周期的工作方式跟任何其他的 Direct3D 资源一样，受引用计数支持，包括跟踪来自流式资源的映射。 当应用程序不再引用磁贴池、到存储器的任何磁贴映射消失并且图形处理单元 (GPU) 访问完成时，操作系统将解除分配磁贴池。
-   与表面共享和同步相关的 API 适用于磁贴池（但不直接作用于流式资源上）。 类似于所提供的磁贴池的行为，如果磁贴池已被共享并且当前由另一设备和进程获取，则丢弃访问指向磁贴池的流式资源的 Direct3D 命令。
-   调整磁贴池大小。
-   提供资源和回收资源 - 这些用于临时向系统给予内存的操作在整个磁贴池上进行（并且不能用于单个流式资源）。 如果流式资源指向所提供的磁贴池中的磁贴，则流式资源的行为方式与其提供方式相同（例如，运行时丢弃引用它的命令）。

数据不能直接复制到磁贴池内存，也不能直接从磁贴池内存复制数据。 访问内存始终通过流式资源进行。

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>相关主题


[创建流式资源](creating-streaming-resources.md)

 

 




