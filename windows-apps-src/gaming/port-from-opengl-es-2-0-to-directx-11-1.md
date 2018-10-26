---
author: mtoepke
title: 从 OpenGL ES 2.0 移植到 Direct3D 11
description: 包含有关将 OpenGL ES 2.0 图形管道移植到 Direct3D 11 和 Windows 运行时的文章、概述以及操作实例。
ms.assetid: 1e1cf668-a15f-0c7b-8daf-3260d27c6d9c
ms.author: mtoepke
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, 游戏, opengl, direct3d 11, 移植, 图形
ms.localizationpriority: medium
ms.openlocfilehash: 6421f5a5a71828d5234a11bab9e442a5accecda5
ms.sourcegitcommit: b7e3d222e229cdbf04e837fcb94fb7d84a93de09
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/26/2018
ms.locfileid: "5618034"
---
# <a name="port-from-opengl-es-20-to-direct3d-11"></a>从 OpenGL ES 2.0 移植到 Direct3D 11



包含有关将 OpenGL ES 2.0 图形管道移植到 Direct3D 11 和 Windows 运行时的文章、概述以及操作实例。

> **注意**移植 OpenGL ES 2.0 项目的中间步骤是使用 Microsoft 应用商店的 ANGLE。 ANGLE 通过将 OpenGL ES API 调用转换为 DirectX 11 API 调用，允许你在 Windows 上运行 OpenGL ES 内容。 有关 ANGLE 的详细信息，请转到[适用于 Microsoft Store 的 ANGLE Wiki](http://go.microsoft.com/fwlink/p/?linkid=618387)。

 

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
<td align="left"><p><a href="map-concepts-and-infrastructure.md">将 OpenGL ES 2.0 映射到 Direct3D 11.1</a></p></td>
<td align="left"><p>首次开始将图形体系结构从 OpenGL ES 2.0 移植到 Direct3D 的过程时，你需要自行熟悉各个 API 之间的主要差别。 本部分中的主题将帮助你规划你的移植策略以及将图形处理移动到 Direct3D 时必须进行的 API 更改。</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="port-a-simple-opengl-es-2-0-renderer-to-directx-11-1.md">如何：将简单的 OpenGL ES 2.0 呈现器移植到 Direct3D 11.1</a></p></td>
<td align="left"><p>对于本次移植练习，我们将从最基本的内容开始：将旋转且顶点着色的立方体的简单呈现器从 OpenGL ES 2.0 移植到 Direct3D 中，以便它与 Visual Studio 2015 中的 DirectX 11 应用（通用 Windows）模板相匹配。</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="opengl-es-2-0-to-directx-11-1-reference.md">OpenGL ES 2.0 到 Direct3D 11.1 参考</a></p></td>
<td align="left"><p>当从 OpenGL ES 2.0 移植到 Direct3D 11 时，使用这些参考主题查找 API 映射和简短的代码示例。</p></td>
</tr>
</tbody>
</table>

 

 

 




