---
title: 创建磁贴池
description: 应用程序可以为每个 Direct3D 设备创建一个或多个磁贴池。 每个磁贴池的总大小仅限于 Direct3D 11 资源大小限制，它大约等于 1/4 的图形处理单元 (GPU) RAM。
ms.assetid: BD51EDD3-4AD3-4733-B014-DD77B9D743BB
keywords:
- 创建磁贴池
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 5ce3824ab2d435b42df9586a6c229b68db10a0c9
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2019
ms.locfileid: "57612802"
---
# <a name="tile-pool-creation"></a>创建磁贴池


应用程序可以为每个 Direct3D 设备创建一个或多个磁贴池。 每个磁贴池的总大小仅限于 Direct3D 11 资源大小限制，它大约等于 1/4 的图形处理单元 (GPU) RAM。

磁贴池由 64KB 的磁贴组成，但操作系统（显示驱动程序）将整个池作为场景后台的一个或多个分配空间进行管理，细分内容对应用程序不可见。 流式资源通过指向磁贴池中的磁贴来定义内容。 将磁贴指向 **NULL** 可以取消磁贴与流式资源的映射。 此类未映射磁贴具有关于读取或写入行为的规则；请参阅[危险跟踪与磁贴池资源](hazard-tracking-versus-tile-pool-resources.md)。

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>相关主题


[到磁贴池中的映射](mappings-are-into-a-tile-pool.md)

 

 




