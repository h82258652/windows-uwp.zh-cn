---
title: 操作实例 -- 将 Direct3D 9 移植到 DirectX 11 和 UWP
description: 此移植练习显示了如何将一个简单的呈现框架从 Direct3D 9 移植到 Direct3D 11 和通用 Windows 平台 (UWP)。
ms.assetid: d4467e1f-929b-a4b8-b233-e142a8714c96
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, 游戏, directx, 端口, direct3d 9, direct3d 11
ms.localizationpriority: medium
ms.openlocfilehash: c7569c6b2f041f5535e0eabe934a91da86b60b9a
ms.sourcegitcommit: b4c502d69a13340f6e3c887aa3c26ef2aeee9cee
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/03/2018
ms.locfileid: "8466158"
---
# <a name="walkthrough-port-a-simple-direct3d-9-app-to-directx-11-and-universal-windows-platform-uwp"></a>演练：将简单的 Direct3D 9 应用移植到 DirectX 11 和通用 Windows 平台 (UWP)



此移植练习显示了如何将一个简单的呈现框架从 Direct3D 9 移植到 Direct3D 11 和通用 Windows 平台 (UWP)。
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
<td align="left"><p><a href="simple-port-from-direct3d-9-to-11-1-part-1--initializing-direct3d.md">初始化 Direct3D 11</a></p></td>
<td align="left"><p>介绍如何将 Direct3D 9 初始化代码转换为 Direct3D 11，包括如何获取 Direct3D 设备和设备上下文的句柄以及如何使用 DXGI 设置交换链。</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="simple-port-from-direct3d-9-to-11-1-part-2--rendering.md">转换呈现框架</a></p></td>
<td align="left"><p>介绍如何将简单的呈现框架从 Direct3D 9 转换到 Direct3D 11，包括如何移植几何图形缓冲区、如何编译和加载 HLSL 着色器程序以及如何在 Direct3D 11 中实现呈现链。</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="simple-port-from-direct3d-9-to-11-1-part-3--viewport-and-game-loop.md">移植游戏循环</a></p></td>
<td align="left"><p>介绍如何实现 UWP 游戏的窗口，以及如何显示游戏循环，包括如何构建 <a href="https://msdn.microsoft.com/library/windows/apps/hh700478"><strong>IFrameworkView</strong></a> 来控制全屏 <a href="https://msdn.microsoft.com/library/windows/apps/br208225"><strong>CoreWindow</strong></a>。</p></td>
</tr>
</tbody>
</table>

 

本主题介绍了执行以下同一基本图形任务的两种代码路径：显示旋转的顶点着色立方体。 在这两种情况下，该代码涉及以下流程：

1.  创建 Direct3D 设备和交换链。
2.  创建顶点缓冲区和索引缓冲区，用来表示彩色立方体网格。
3.  创建用来将顶点转换为屏幕空间的顶点着色器和用来混合颜色值的像素着色器、编译着色器和加载着色器作为 Direct3D 资源。
4.  实现渲染链并在屏幕上呈现绘制立方体。
5.  创建窗口、开始主循环并关注窗口消息处理。

完成本演练后，你应当熟悉 Direct3D 9 和 Direct3D 11 之间的以下基本差异：

-   分离设备、设备上下文和图形基础结构。
-   编译着色器并在运行时加载着色器字节码的过程。
-   如何为输入装配器 (IA) 阶段配置每顶点数据。
-   如何使用 [**IFrameworkView**](https://msdn.microsoft.com/library/windows/apps/hh700478) 创建 CoreWindow 视图。

请注意，此演练使用 [**CoreWindow**](https://msdn.microsoft.com/library/windows/apps/br208225) 进行简化，并不涉及 XAML 互操作。

## <a name="prerequisites"></a>先决条件


应该[为 UWP DirectX 游戏开发准备开发人员环境](prepare-your-dev-environment-for-windows-store-directx-game-development.md)。 你还不需要模板，但你将需要 Microsoft Visual Studio2015 来加载在本演练中的代码示例。

访问[移植概念和注意事项](porting-considerations.md)，以便更好地了解此演练中显示的 DirectX 11 和 UWP 编程概念。

## <a name="related-topics"></a>相关主题

**Direct3D**

* [在 Direct3D 9 中编写 HLSL 着色器](https://msdn.microsoft.com/library/windows/desktop/bb944006)
* [DirectX 游戏项目模板](user-interface.md)

**Microsoft Store**

* [**Microsoft::WRL::ComPtr**](https://msdn.microsoft.com/library/windows/apps/br244983.aspx)
* [**对象运算符的句柄 (^)**](https://msdn.microsoft.com/library/windows/apps/yk97tc08.aspx)

