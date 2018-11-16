---
title: 可用于流式资源的操作
description: 本节列出了可对流式资源执行的操作。
ms.assetid: 700D8C54-0E20-4B2B-BEA3-20F6F72B8E24
keywords:
- 可用于流式资源的操作
author: michaelfromredmond
ms.author: mithom
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 1289b01e7ffb780c7e3faa52585eb5f002cf519c
ms.sourcegitcommit: e38b334edb82bf2b1474ba686990f4299b8f59c7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/15/2018
ms.locfileid: "6848654"
---
# <a name="operations-available-on-streaming-resources"></a>可用于流式资源的操作


本节列出了可对流式资源执行的操作。

-   返回 void 的 Update Tile Mappings 操作和返回 void 的 Copy Tile Mappings 操作 - 这些操作将流式资源中的磁贴位置指向磁贴池中的位置，或指向 NULL，或同时指向两者。 这些操作可以更新磁贴指针的不相交子集。
-   复制和更新操作 - 所有可将数据复制到默认池表面及可从默认池表面复制数据的 API 都适用于流式资源。 从未映射的磁贴读取将产生 0，到未映射磁贴的写入会被丢弃。
-   Copy Tiles 和 Update Tiles 操作 - 这些操作用于以 64KB 的粒度将磁贴复制到任意流式资源和规范内存布局中的缓冲区资源，或进行反向操作。 显示驱动程序和硬件执行流式资源所需的任何内存“重排”。
-   将作用于非流式资源的 Direct3D 管道绑定和视图创建/绑定也作用于流式资源。

磁贴控件在即时或延迟上下文上可用（就像对典型资源的更新一样），并且在执行时影响对磁贴的后续访问（不是先前提交的操作）。

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>相关主题


[创建流式资源](creating-streaming-resources.md)

 

 




