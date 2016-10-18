---
author: mtoepke
title: "从 OpenGL ES 2.0 移植到 Direct3D 11"
description: "包含有关将 OpenGL ES 2.0 图形管道移植到 Direct3D 11 和 Windows 运行时的文章、概述以及演练。"
ms.assetid: 1e1cf668-a15f-0c7b-8daf-3260d27c6d9c
translationtype: Human Translation
ms.sourcegitcommit: 814f056eaff5419b9c28ba63cf32012bd82cc554
ms.openlocfilehash: aab0c3e9f3816e0657dfb6fec4917d62f2be5280

---

# 从 OpenGL ES 2.0 移植到 Direct3D 11


\[ 已针对 Windows 10 上的 UWP 应用更新。 有关 Windows 8.x 文章，请参阅[存档](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

包含有关将 OpenGL ES 2.0 图形管道移植到 Direct3D 11 和 Windows 运行时的文章、概述以及操作实例。

> **注意**：移植 OpenGL ES 2.0 项目的中间步骤是使用适用于 Windows 应用商店的 ANGLE。 ANGLE 通过将 OpenGL ES API 调用转换为 DirectX 11 API 调用，允许你在 Windows 上运行 OpenGL ES 内容。 有关 ANGLE 的详细信息，请转到[适用于 Windows 应用商店的 ANGLE Wiki](http://go.microsoft.com/fwlink/p/?linkid=618387)。

 

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
<td align="left"><p>[将 OpenGL ES 2.0 映射到 Direct3D 11.1](map-concepts-and-infrastructure.md)</p></td>
<td align="left"><p>首次开始将图形体系结构从 OpenGL ES 2.0 移植到 Direct3D 的过程时，你需要自行熟悉各个 API 之间的主要差别。 本部分中的主题将帮助你规划你的移植策略以及将图形处理移动到 Direct3D 时必须进行的 API 更改。</p></td>
</tr>
<tr class="even">
<td align="left"><p>[如何：将简单的 OpenGL ES 2.0 呈现器移植到 Direct3D 11.1](port-a-simple-opengl-es-2-0-renderer-to-directx-11-1.md)</p></td>
<td align="left"><p>对于本次移植练习，我们将从最基本的内容开始：将旋转且顶点着色的立方体的简单呈现器从 OpenGL ES 2.0 移植到 Direct3D 中，以便它与 Visual Studio 2015 中的 DirectX 11 应用（通用 Windows）模板相匹配。</p></td>
</tr>
<tr class="odd">
<td align="left"><p>[OpenGL ES 2.0 到 Direct3D 11.1 参考](opengl-es-2-0-to-directx-11-1-reference.md)</p></td>
<td align="left"><p>当从 OpenGL ES 2.0 移植到 Direct3D 11 时，使用这些参考主题查找 API 映射和简短的代码示例。</p></td>
</tr>
</tbody>
</table>

 

> **注意**  
本文适用于编写通用 Windows 平台 (UWP) 应用的 Windows 10 开发人员。 如果你面向 Windows 8.x 或 Windows Phone 8.x 进行开发，请参阅[存档文档](http://go.microsoft.com/fwlink/p/?linkid=619132)。

 

 

 







<!--HONumber=Aug16_HO3-->


