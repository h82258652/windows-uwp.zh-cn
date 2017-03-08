---
title: "创建磁贴池"
description: "应用程序可以为每个 Direct3D 设备创建一个或多个磁贴池。 每个磁贴池的总大小受到 Direct3D 11 的资源大小限制，大约是图形处理单元 (GPU) RAM 的 1/4。"
ms.assetid: BD51EDD3-4AD3-4733-B014-DD77B9D743BB
keywords:
- "创建磁贴池"
author: PeterTurcan
ms.author: pettur
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
translationtype: Human Translation
ms.sourcegitcommit: c6b64cff1bbebc8ba69bc6e03d34b69f85e798fc
ms.openlocfilehash: 5289bcb57572ede45c6853b0077f5baa82af7ca2
ms.lasthandoff: 02/07/2017

---

# <a name="tile-pool-creation"></a>创建磁贴池


应用程序可以为每个 Direct3D 设备创建一个或多个磁贴池。 每个磁贴池的总大小受到 Direct3D 11 的资源大小限制，大约是图形处理单元 (GPU) RAM 的 1/4。

磁贴池由 64KB 的磁贴组成，但操作系统（显示驱动程序）将整个池作为场景后台的一个或多个分配空间进行管理，细分内容对应用程序不可见。 流式资源通过指向磁贴池中的磁贴来定义内容。 将磁贴指向 **NULL** 可以取消磁贴与流式资源的映射。 此类未映射磁贴具有关于读取或写入行为的规则；请参阅[危险跟踪与磁贴池资源](hazard-tracking-versus-tile-pool-resources.md)。

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>相关主题


[映射到磁贴池](mappings-are-into-a-tile-pool.md)

 

 





