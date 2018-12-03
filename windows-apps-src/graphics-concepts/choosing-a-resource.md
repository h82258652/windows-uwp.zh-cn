---
title: 选择资源
description: 资源是 3D 管道所使用的数据集合。
ms.assetid: 6BAD6287-2930-42F8-BF51-69A379D1D2C3
keywords:
- 选择资源
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: ccc99395dba2f2d1894db81fb48abb59f9a8ba4f
ms.sourcegitcommit: b4c502d69a13340f6e3c887aa3c26ef2aeee9cee
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/03/2018
ms.locfileid: "8464253"
---
# <a name="choosing-a-resource"></a>选择资源


资源是 3D 管道所使用的数据集合。 对应用程序进行编程的第一步是创建资源并定义其行为。 本指南介绍了选择应用程序所需资源的基本主题。

## <a name="span-ididentifybindingspanspan-ididentifybindingspanspan-ididentifybindingspanidentify-pipeline-stages-that-need-resources"></a><span id="Identify_Binding"></span><span id="identify_binding"></span><span id="IDENTIFY_BINDING"></span>确认需要资源的管道阶段


第一步是选择使用资源的[图形管道](graphics-pipeline.md)阶段。 即，确认从资源中读取数据的每个阶段和将数据写入资源的每个阶段。 了解使用资源的管道阶段将确定被调用以将资源绑定到此阶段的 API。

此表列出了可绑定到每个管道阶段的资源类型。 包括资源是作为输入还是作为输出进行绑定。

| 管道阶段  | 输入/输出 | 资源               | 资源类型                           |
|-----------------|--------|------------------------|-----------------------------------------|
| 输入组装器 | 输入     | 顶点缓冲区          | 缓冲区                                  |
| 输入组装器 | 输入     | 索引缓冲区           | 缓冲区                                  |
| 着色器阶段   | 输入     | 着色器资源视图    | 缓冲区，Texture1D，Texture2D，Texture3D |
| 着色器阶段   | 输入     | 着色器常量缓冲区 | 缓冲区                                  |
| 流输出   | 输出    | 缓冲区                 | 缓冲区                                  |
| 输出合并器   | 输出    | 呈现器目标视图     | 缓冲区，Texture1D，Texture2D，Texture3D |
| 输出合并器   | 输出    | 深度/模具视图     | Texture1D，Texture2D                    |

 

## <a name="span-ididentifyusagespanspan-ididentifyusagespanspan-ididentifyusagespanidentify-how-each-resource-will-be-used"></a><span id="Identify_Usage"></span><span id="identify_usage"></span><span id="IDENTIFY_USAGE"></span>确定将如何使用每个资源


选择你的应用程序要使用的管道阶段（和每个阶段需要的资源）后，下一步是确定如何使用每个资源，即资源能被 CPU 还是 GPU 访问。

运行你的应用程序的硬件至少需要一个 CPU 和一个 GPU。 若要获取使用情况值，请在以下选项中考虑哪种类型的处理器需要读取或写入资源。

| 资源用法 | 可如何更新                    | 更新频率 |
|----------------|--------------------------------------|---------------------|
| 默认        | GPU                                  | 不频繁        |
| 动态        | CPU                                  | 频繁          |
| 临时        | GPU                                  | 不适用                 |
| 不可变      | CPU（仅在创建资源时） | 不适用                 |

 

默认用法应用于 CPU 预计不经常更新的资源（每帧少于一次）。 理想情况下，CPU 从不将其直接写入采用默认用法的资源，以避免引起可能的性能损失。

动态用法应用于 CPU 相对经常更新的资源（每帧一次或多次）。 动态资源的典型方案是创建动态顶点和索引缓冲区，这些顶点和缓冲区将在运行时用从用户视角可见的每帧几何图形相关数据进行填充。 这些缓冲区会用于仅呈现该帧对用户可见的几何图形。

临时用法应用于将数据复制到其他资源和从其他资源复制数据。 典型方案为将采用默认用法的资源（CPU 不可访问）中的数据复制到采用临时用法的资源（CPU 可访问）。

资源中的数据从不改变时，应使用不可变资源。

评估这一理念的另一种方法是了解应用程序是如何处理资源的。

| 应用程序如何使用资源     | 资源用法       |
|---------------------------------------|----------------------|
| 一旦加载从不更新            | 不可变或默认 |
| 应用程序重复填充资源 | 动态              |
| 渲染到纹理                     | 默认              |
| CPU 访问 GPU 数据                | 临时              |

 

如果你不确定要选择哪个用法，请先使用默认用法，因为它应为最常见的情况。 着色器常量缓冲区是一个应始终采用默认用法的资源类型。

## <a name="span-idresourcetypesandpipelinestagesspanspan-idresourcetypesandpipelinestagesspanspan-idresourcetypesandpipelinestagesspanbinding-resources-to-pipeline-stages"></a><span id="Resource_Types_and_Pipeline_stages"></span><span id="resource_types_and_pipeline_stages"></span><span id="RESOURCE_TYPES_AND_PIPELINE_STAGES"></span>将资源绑定到管道阶段


只要满足创建资源时指定的限制条件，就可以同时将一个资源绑定到多个管道阶段。 指定的这些限制为用法标志、绑定标志或 cpu 访问标志。 更具体地说，只要不可同时读取和写入资源的一部分，即可将资源作为输入和输出同时绑定。

绑定资源时，请考虑 GPU 和 CPU 如何访问资源。 用于单一目的的资源（不使用多个用法、绑定和 cpu 访问标志）的性能可能更佳。

例如，考虑多次将呈现器目标用作纹理的情况。 使用两个资源可能更快：一个呈现器目标和一个用作着色器资源的纹理。 每个资源只使用一个绑定标志，用于指示“呈现器目标”或“着色器资源”。 数据将从呈现器目标纹理复制到着色器纹理。

此示例中的此技术可能会通过将呈现器目标写入与着色器纹理读取隔离来提高性能。 确保实现此目的的唯一方法是实现这两种方法并度量特定应用程序的性能差异。

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>相关主题


[资源](resources.md)

 

 




