---
title: DirectX 游戏的基本 3D 图形
description: 我们将介绍如何使用 DirectX 编程来实现 3D 图形的基本概念。
ms.assetid: 2989c91f-7b45-7377-4e83-9daa0325e92e
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, uwp, 游戏, directx, 图形
ms.localizationpriority: medium
ms.openlocfilehash: 556c5c74e5c8284e56047b4b8a9b2c2bf9c0155c
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/29/2019
ms.locfileid: "66369077"
---
# <a name="basic-3d-graphics-for-directx-games"></a>DirectX 游戏的基本 3D 图形



我们将介绍如何使用 DirectX 编程来实现 3D 图形的基本概念。

**目标：** 了解如何编写 3D 图形应用程序。

## <a name="prerequisites"></a>先决条件


我们假定你熟悉 C++。 你还需要具有图形编程概念方面的基本经验。

**若要完成的总时间：** 30 分钟。

## <a name="where-to-go-from-here"></a>自此处转至


在这里，我们将讨论如何开发使用 DirectX 的 3D 图形和C++ \\Cx。 此教程包含五个部分，向你介绍 [Direct3D](https://docs.microsoft.com/windows/desktop/direct3d) API 以及在其他许多 DirectX 示例中也会用到的概念和代码。 这些部分从介绍如何为 UWP C++ 应用配置 DirectX，到如何设置基元的纹理和添加效果，循序渐进，逐层深入。

> **请注意**  列矢量与本教程使用右手坐标系。 很多 DirectX 示例和应用都使用具有行向量的左手坐标系。 为了获得更完整的图形数学解决方案以及支持具有行向量的左手坐标系的解决方案，请考虑使用 [DirectXMath](https://docs.microsoft.com/windows/desktop/dxmath/directxmath-portal)。 有关详细信息，请参阅[将 DirectXMath 与 Direct3D 结合使用](https://docs.microsoft.com/windows/desktop/dxmath/pg-xnamath-migration-d3dx)。

 

我们将向你展示如何：

-   使用 Windows 运行时初始化 [Direct3D](https://docs.microsoft.com/windows/desktop/direct3d) 接口。
-   应用每顶点着色器操作
-   设置几何图形
-   将场景栅格化（将 3D 场景扁平化为 2D 投影）
-   剔除隐藏的表面

> **注意**  

 

接下来，我们将创建 Direct3D 设备、交换链和呈现器目标视图，并向屏幕显示呈现的图像。

[快速入门： 设置了 DirectX 资源，并显示图像](setting-up-directx-resources.md)

## <a name="related-topics"></a>相关主题


* [Direct3D 11 图形](https://docs.microsoft.com/windows/desktop/direct3d11/atoc-dx-graphics-direct3d-11)
* [DXGI](https://docs.microsoft.com/windows/desktop/direct3ddxgi/dx-graphics-dxgi)
* [HLSL](https://docs.microsoft.com/windows/desktop/direct3dhlsl/dx-graphics-hlsl)

 

 




