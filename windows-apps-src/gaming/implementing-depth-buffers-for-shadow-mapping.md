---
title: 操作实例 -- Direct3D 11 阴影卷深度缓冲区
description: 本操作实例演示如何使用深度映射呈现阴影卷，并在所有 Direct3D 功能级别的设备上使用 Direct3D 11。
ms.assetid: d15e6501-1a1d-d99c-d1d8-ad79b849db90
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, 游戏, directx, 阴影卷, 深度缓冲区, directx 11
ms.localizationpriority: medium
ms.openlocfilehash: 2feecb3080efefb2f9625fd8b66c5b722ad02a45
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2019
ms.locfileid: "57622272"
---
# <a name="walkthrough-implement-shadow-volumes-using-depth-buffers-in-direct3d-11"></a>操作实例：实现在 Direct3D 11 中使用深度缓冲区的卷影卷



本操作实例演示如何使用深度映射呈现阴影卷，并在所有 Direct3D 功能级别的设备上使用 Direct3D 11。
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
<td align="left"><p><a href="create-depth-buffer-resource--view--and-sampler-state.md">创建深度缓冲区设备的资源</a></p></td>
<td align="left"><p>了解如何创建支持阴影卷的深度测试所需的 Direct3D 设备资源。</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="render-the-shadow-map-to-the-depth-buffer.md">呈现到深度缓冲区的卷影映射</a></p></td>
<td align="left"><p>从光线的角度呈现，以创建一个表示阴影卷的二维深度映射。</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="render-the-scene-with-depth-testing.md">呈现深度测试场景</a></p></td>
<td align="left"><p>通过向顶点（或几何图形）着色器和像素着色器中添加深度测试来创建阴影效果。</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="target-a-range-of-hardware.md">支持广泛的硬件上将卷影地图</a></p></td>
<td align="left"><p>在速度更快的设备上更高保真度地呈现阴影，在功能不够强大的设备上更快地呈现阴影。</p></td>
</tr>
</tbody>
</table>

 

## <a name="shadow-mapping-application-to-direct3d-9-desktop-porting"></a>阴影映射应用程序到 Direct3D 9 桌面的移植


Windows 8 添加真正 d 深度比较功能，到功能级别 9\_1 和 9\_3。 现在，你可以将具有阴影卷的呈现代码迁移到 DirectX 11，并且 Direct3D 11 呈现器将是与功能级别 9 设备兼容的下一级设备。 本操作实例介绍任何 Direct3D 11 应用或游戏如何使用深度测试实现传统的阴影卷。 代码包含以下过程：

1.  为阴影映射创建 Direct3D 设备资源。
2.  添加呈现传递以创建深度映射。
3.  将深度测试添加到主呈现传递。
4.  实现必需的着色器代码。
5.  用于在下一级硬件上快速呈现的选项。

在完成本演练，请您应熟悉如何在兼容功能级别为 9 的 Direct3D 11 中实现基本兼容的卷影卷技术\_1 及更高版本。

## <a name="prerequisites"></a>必备条件


应该[准备通用 Windows 平台 (UWP) DirectX 游戏开发的开发人员环境](prepare-your-dev-environment-for-windows-store-directx-game-development.md)。 您不需要模板，但是您将需要 Microsoft Visual Studio 2015 生成在本演练中的代码示例。

## <a name="related-topics"></a>相关主题


**Direct3D**

* [在 Direct3D 中编写 HLSL 着色器 9](https://msdn.microsoft.com/library/windows/desktop/bb944006)
* [为 UWP 创建新的 DirectX 11 项目](user-interface.md)

**卷影映射技术文章**

* [常见的技术以提高阴影深度映射](https://msdn.microsoft.com/library/windows/desktop/ee416324)
* [级联的卷影映射](https://msdn.microsoft.com/library/windows/desktop/ee416307)

 

 




