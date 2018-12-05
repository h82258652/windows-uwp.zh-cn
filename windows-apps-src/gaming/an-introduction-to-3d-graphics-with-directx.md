---
title: DirectX 游戏的基本 3D 图形
description: 我们将介绍如何使用 DirectX 编程来实现 3D 图形的基本概念。
ms.assetid: 2989c91f-7b45-7377-4e83-9daa0325e92e
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, uwp, 游戏, directx, 图形
ms.localizationpriority: medium
ms.openlocfilehash: 5dbdf6072f57d12d424f0787cfa2e8993a1624af
ms.sourcegitcommit: c01c29cd97f1cbf050950526e18e15823b6a12a0
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/05/2018
ms.locfileid: "8694523"
---
# <a name="basic-3d-graphics-for-directx-games"></a>DirectX 游戏的基本 3D 图形



我们将介绍如何使用 DirectX 编程来实现 3D 图形的基本概念。

**目标：** 了解如何编写 3D 图形应用。

## <a name="prerequisites"></a>先决条件


我们假定你熟悉 C++。 你还需要具有图形编程概念方面的基本经验。

**完成所需总时间：** 30 分钟。

## <a name="where-to-go-from-here"></a>自此处转至


下面我们介绍如何使用 DirectX 和 C++\\Cx 开发 3D 图形。 此教程包含五个部分，向你介绍 [Direct3D](https://msdn.microsoft.com/library/windows/desktop/hh309466) API 以及在其他许多 DirectX 示例中也会用到的概念和代码。 这些部分从介绍如何为 UWP C++ 应用配置 DirectX，到如何设置基元的纹理和添加效果，循序渐进，逐层深入。

> **注意**本教程中使用具有列向量的右手坐标系统。 很多 DirectX 示例和应用都使用具有行向量的左手坐标系。 为了获得更完整的图形数学解决方案以及支持具有行向量的左手坐标系的解决方案，请考虑使用 [DirectXMath](https://msdn.microsoft.com/library/windows/desktop/hh437833)。 有关详细信息，请参阅[将 DirectXMath 与 Direct3D 结合使用](https://msdn.microsoft.com/library/windows/desktop/ff729728#Use_DXMath_with_D3D)。

 

我们将向你展示如何：

-   使用 Windows 运行时初始化 [Direct3D](https://msdn.microsoft.com/library/windows/desktop/hh309466) 接口。
-   应用每顶点着色器操作
-   设置几何图形
-   将场景栅格化（将 3D 场景扁平化为 2D 投影）
-   剔除隐藏的表面

> **注意**  

 

接下来，我们将创建 Direct3D 设备、交换链和呈现器目标视图，并向屏幕显示呈现的图像。

[快速入门：设置 DirectX 资源和显示图像](setting-up-directx-resources.md)

## <a name="related-topics"></a>相关主题


* [Direct3D 11 图形](https://msdn.microsoft.com/library/windows/desktop/ff476080)
* [DXGI](https://msdn.microsoft.com/library/windows/desktop/hh404534)
* [HLSL](https://msdn.microsoft.com/library/windows/desktop/bb509561)

 

 




