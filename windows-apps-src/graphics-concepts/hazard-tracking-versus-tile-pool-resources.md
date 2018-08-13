---
title: 危险跟踪与磁贴池资源
description: 对于非流式资源，Direct3D 可以在渲染期间避免某些危险情况；但对于流式资源，危险跟踪将在磁贴级别进行，因而在渲染流式资源期间跟踪危险情况的代价可能极其高昂。
ms.assetid: 8B0C73D3-3F77-41E8-B17D-C595DEE39E49
keywords:
- 危险跟踪与磁贴池资源
author: michaelfromredmond
ms.author: mithom
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
ms.localizationpriority: medium
ms.openlocfilehash: 982a590f602aa1692de91536d50cbadb19f8f1d1
ms.sourcegitcommit: 897a111e8fc5d38d483800288ad01c523e924ef4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/13/2018
ms.locfileid: "1043046"
---
# <a name="hazard-tracking-versus-tile-pool-resources"></a>危险跟踪与磁贴池资源


对于非流式资源，Direct3D 可以在渲染期间避免某些危险情况；但对于流式资源，危险跟踪将在磁贴级别进行，因而在渲染流式资源期间跟踪危险情况的代价可能极其高昂。

例如，对于非流式资源，运行时不允许同时将任何给定的子资源绑定为输入（例如着色器资源视图）和输出（例如渲染器目标视图）。如果遇到这种情况，运行时会解除绑定输入。 这种在运行时中跟踪开销是便宜的，并且在子资源级别完成。 这种跟踪开销的好处之一是将应用程序意外地取决于硬件着色器执行顺序的几率降至最低。 硬件着色器执行顺序可能会有所不同，如果不是在给定的图形处理单元 (GPU) 上，那么肯定是在不同的 GPU 中。

对于流式资源，跟踪资源绑定方式的代价可能太高昂，因为跟踪是在磁贴级别进行。 新问题会出现，例如可能需要使用同时映射到表面中的多个区域的磁贴验证渲染到渲染器目标视图的尝试。 即便事实证明这种按每个磁贴进行的危险跟踪对于运行时而言成本过高，至少这在理想情况下是调试层中的一个选择。

当应用程序针对引用磁贴池内存（在接下来的读取或写入操作中，该磁贴池也将被单独的流式资源引用）的流式资源发起写入或读取操作时，此应用程序必须告知显示驱动程序，在第一个操作完成之后，它才会开始接下来的操作。

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>相关主题


[映射到磁贴池](mappings-are-into-a-tile-pool.md)

 

 




