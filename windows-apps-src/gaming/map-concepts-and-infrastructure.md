---
title: 将 OpenGL ES 2.0 映射到 Direct3D 11
description: 首次开始将图形体系结构从 OpenGL ES 2.0 移植到 Direct3D 的过程时，你需要自行熟悉一下 API 之间的主要差别。
ms.assetid: 7f9b136c-aa22-04b3-d385-6e9e1f38b948
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, 游戏, opengl, direct3d, 移植
ms.localizationpriority: medium
ms.openlocfilehash: e09dcb1830e62d17983f564771b4808d132179a0
ms.sourcegitcommit: 49d58bc66c1c9f2a4f81473bcb25af79e2b1088d
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/11/2018
ms.locfileid: "8926509"
---
# <a name="map-opengl-es-20-to-direct3d-11"></a>将 OpenGL ES 2.0 映射到 Direct3D 11



首次开始将图形体系结构从 OpenGL ES 2.0 移植到 Direct3D 的过程时，你需要自行熟悉一下 API 之间的主要差别。 本部分中的主题将帮助你规划你的移植策略以及将图形处理移动到 Direct3D 时必须进行的 API 更改。
## 
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
<td align="left"><p><a href="compare-opengl-es-2-0-api-design-to-directx.md">规划从 OpenGL ES 2.0 到 Direct3D 的移植</a></p></td>
<td align="left"><p>如果你移植 iOS 或 Android 平台中的游戏，那么你可能需要在 OpenGL ES 2.0 方面进行大量投资。 如果你准备将你的图形管道代码库移动到 Direct3D 11 和 Windows 运行时，那么在开始之前你应该考虑以下事项。</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="moving-from-egl-to-dxgi.md">将 EGL 代码与 DXGI 和 Direct3D 进行比较</a></p></td>
<td align="left"><p>DirectX Graphics Interface (DXGI) 以及若干个 Direct3D API 所起的作用与 EGL 相同。 本主题帮助你从 EGL 的角度了解 DXGI 和 Direct3D 11。</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="porting-uniforms-and-attributes.md">将 OpenGL ES 2.0 缓冲区、uniform 和顶点属性与 Direct3D 进行比较</a></p></td>
<td align="left"><p>在从 OpenGL ES 2.0 移植到 Direct3D 11 的过程中，必须更改用于在应用和着色器程序之间传递数据的语法和 API 行为。</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="change-your-shader-loading-code.md">将 OpenGL ES 2.0 着色器管道与 Direct3D 进行比较</a></p></td>
<td align="left"><p>从概念上来说，Direct3D 11 着色器管道与 OpenGL ES 2.0 中的着色器管道非常相似。 但是，就 API 设计而言，用于创建和管理着色器阶段的主要组件是两个主要接口 <a href="https://msdn.microsoft.com/library/windows/desktop/hh404575"><strong>ID3D11Device1</strong></a> 和 <a href="https://msdn.microsoft.com/library/windows/desktop/hh404598"><strong>ID3D11DeviceContext1</strong></a> 的一部分。 本主题尝试在这些接口中将常用的 OpenGL ES 2.0 着色器管道 API 模式映射到 Direct3D 11 同等模式。</p></td>
</tr>
</tbody>
</table>

 

## <a name="notes-on-specific-opengl-es-20-providers"></a>有关特定 OpenGL ES 2.0 提供商的说明


这些主题都使用 Khronos OpenGL ES 2.0 规范，该规范使用与平台无关的 C 程序。iOS 和 Android 使用相同的规范，为这些平台开发的 OpenGL ES 2.0 代码与我们将介绍的代码段非常相似，尽管它们通常显示为面向对象的 API。 此外，由于每个平台的复杂性和语言差异性，可能存在细微的差别，特别是在方法参数类型或常规的语言语法方面。 例如，iOS 使用 Objective-C。 Android 能够使用 C++；但是，某些开发人员可能依赖纯 Java 实现。 鉴于这一点，这些主题应该依然有用，因为 OpenGL ES API 的总体概念、结构以及用法并没有什么不同。

 

 




