---
title: 资源
description: 资源是内存中可由 Direct3D 管道访问的区域。
ms.assetid: 2E68E5A8-83DA-4DC8-B7F3-B8988CF8090C
keywords:
- 资源
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: c31dcbcc3019538d769118b018c693174b17b4c7
ms.sourcegitcommit: 681c70f964210ab49ac5d06357ae96505bb78741
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2018
ms.locfileid: "7711958"
---
# <a name="resources"></a>资源


资源是内存中可由 Direct3D 管道访问的区域。 为了使管道能够高效地访问内存，提供给管道的数据（如输入几何图形、着色器资源和纹理）必须存储在资源中。 所有 Direct3D 资源都存储在两种类型的资源中：缓冲区或纹理。 每个管道阶段最多可以有 128 个活动资源。

每个应用程序通常将创建多个资源。 资源的示例包括：顶点缓冲区、索引缓冲区、常量缓冲区、纹理和着色器资源。 有多个确定资源的使用方式的选项。 你可以创建强类型的资源或无类型的资源；你可以控制资源是否具有读写访问权；你可以使资源仅可供 CPU 和/或 GPU 访问。 当然，将存在速度与功能之间的权衡 - 你允许资源拥有的资源越多，你将获得的性能越低。

由于应用程序通常使用多个纹理，因此 Direct3D 还具有纹理数组的概念以简化纹理管理。 纹理数组包含一个或多个纹理（所有这些纹理的类型和尺寸相同），可从应用程序中或通过着色器为这些纹理建立索引。 利用纹理数组，你可以将单个接口与多个索引结合使用来访问多个纹理。 你可以创建所需数目的纹理数组来管理不同的纹理类型。

在创建应用程序将使用的资源后，你将每个资源连接或绑定到将使用该资源的管道阶段。 这将通过调用绑定 API（将采用指向资源的指针）来完成。 由于多个管道阶段可能需要访问同一资源，因此 Direct3D 具有资源视图的概念。 视图标识可访问的资源部分。 你可以创建 *m* 个视图或一个资源，并将它们绑定到 *n* 个管道阶段，假定你遵循共享资源的绑定规则（如果你未遵循，运行时将在编译时生成错误）。

资源视图提供用于访问资源（例如，纹理或缓冲区）的常见模型。 由于你可以使用视图来告知运行时要访问的数据以及访问数据的方式，因此资源视图允许你创建无类型资源。 也就是说，你可以在编译时创建给定大小的资源，然后在资源绑定到管道时在资源中声明数据类型。 视图公开了使用资源时可用的多种功能，例如，能够在着色器中读回深度/模板图面、在单次传递中生成动态 cubemap 以及同时呈现到卷的多个切片。

## <a name="span-idin-this-sectionspanin-this-section"></a><span id="in-this-section"></span>本节内容


<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">主题</th>
<th align="left">描述</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p><a href="resource-types.md">资源类型</a></p></td>
<td align="left"><p>不同类型的资源具有不同的布局（或内存占用）。 Direct3D 使用的所有资源派生自两种基础资源类型：<a href="resource-types.md#buffer-resources">缓冲区</a>和<a href="resource-types.md#texture-resources">纹理</a>。 缓冲区是原始数据（元素）集合；纹理是纹理（纹理元素）集合。</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="choosing-a-resource.md">选择资源</a></p></td>
<td align="left"><p>资源是 3D 管道所使用的数据集合。 对应用程序进行编程的第一步是创建资源并定义其行为。 本指南介绍了选择应用程序所需资源的基本主题。</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="copying-and-accessing-resource-data.md">复制和访问资源数据</a></p></td>
<td align="left"><p>用法标志指示应用程序应如何使用资源数据以将资源放置到性能最佳的内存区域。 跨资源复制资源数据，以便 CPU 或 GPU 能够访问数据而不会影响性能。</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="texture-views.md">纹理视图</a></p></td>
<td align="left"><p>在 Direct3D 中，使用视图访问纹理资源，这是适用于内存中资源的硬件解释的机制。 视图允许特定管道阶段仅访问所需的<a href="resource-types.md">子资源</a>（采用应用程序所需的表示形式）。</p></td>
</tr>
</tbody>
</table>

 

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>相关主题


[坐标系统](coordinate-systems.md)

[Direct3D 图形学习指南](index.md)

[浮点规则](floating-point-rules.md)

[数据类型转换](data-type-conversion.md)
