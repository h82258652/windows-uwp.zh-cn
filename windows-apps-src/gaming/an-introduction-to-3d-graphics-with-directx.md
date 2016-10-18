---
author: mtoepke
title: "DirectX 游戏的基本 3D 图形"
description: "我们将介绍如何使用 DirectX 编程来实现 3D 图形的基本概念。"
ms.assetid: 2989c91f-7b45-7377-4e83-9daa0325e92e
translationtype: Human Translation
ms.sourcegitcommit: 6530fa257ea3735453a97eb5d916524e750e62fc
ms.openlocfilehash: d4b059bed3cb21403b742048d6f6df47780b8aab

---

# DirectX 游戏的基本 3D 图形


\[ 已针对 Windows 10 上的 UWP 应用更新 有关 Windows 8.x 文章，请参阅[存档](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

我们将介绍如何使用 DirectX 编程来实现 3D 图形的基本概念。

**目标：**了解如何编写 3D 图形应用。

## 先决条件


我们假定你熟悉 C++。 你还需要具有图形编程概念方面的基本经验。

**完成所需总时间：**30 分钟。

## 自此处转至


下面我们介绍如何使用 DirectX 和 C++\\Cx 开发 3D 图形。 此教程包含五个部分，向你介绍 [Direct3D](https://msdn.microsoft.com/library/windows/desktop/hh309466) API 以及在其他许多 DirectX 示例中也会用到的概念和代码。 这些部分从介绍如何为 UWP C++ 应用配置 DirectX，到如何设置基元的纹理和添加效果，循序渐进，逐层深入。

> **注意** 此教程使用一个具有列向量的右手坐标系。 很多 DirectX 示例和应用都使用具有行向量的左手坐标系。 为了获得更完整的图形数学解决方案以及支持具有行向量的左手坐标系的解决方案，请考虑使用 [DirectXMath](https://msdn.microsoft.com/library/windows/desktop/hh437833)。 有关详细信息，请参阅[将 DirectXMath 与 Direct3D 结合使用](https://msdn.microsoft.com/library/windows/desktop/ff729728#Use_DXMath_with_D3D)。

 

我们将向你展示如何：

-   使用 Windows 运行时初始化 [Direct3D](https://msdn.microsoft.com/library/windows/desktop/hh309466) 接口。
-   应用每顶点着色器操作
-   设置几何图形
-   将场景栅格化（将 3D 场景扁平化为 2D 投影）
-   剔除隐藏的表面

> **注意**  
本文适用于编写通用 Windows 平台 (UWP) 应用的 Windows 10 开发人员。 如果你要针对 Windows 8.x 或 Windows Phone 8.x 进行开发，请参阅[存档文档](http://go.microsoft.com/fwlink/p/?linkid=619132)。

 

接下来，我们将创建 Direct3D 设备、交换链和呈现器目标视图，并向屏幕显示呈现的图像。

[快速入门：设置 DirectX 资源和显示图像](setting-up-directx-resources.md)

## 相关主题


* [Direct3D 11 图形](https://msdn.microsoft.com/library/windows/desktop/ff476080)
* [DXGI](https://msdn.microsoft.com/library/windows/desktop/hh404534)
* [HLSL](https://msdn.microsoft.com/library/windows/desktop/bb509561)

 

 







<!--HONumber=Aug16_HO3-->


