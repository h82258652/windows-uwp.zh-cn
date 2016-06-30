---
author: scottmill
ms.assetid: a2751e22-6842-073a-daec-425fb981bafe
title: "可视化层"
description: "Windows.UI.Composition API 使你能够访问框架层 (XAML) 和图形层 (DirectX) 之间的合成层。"
translationtype: Human Translation
ms.sourcegitcommit: b3d198af0c46ec7a2041a7417bccd56c05af760e
ms.openlocfilehash: 164c01737d27451adcb685f9cda544cc00634af4

---
# 可视化层

\[ 已针对 Windows 10 上的 UWP 应用更新。 有关 Windows 8.x 的文章，请参阅[存档](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

在 Windows 10 中，已针对创建适用于所有 Windows 应用程序（桌面版或移动版）的全新统一的合成器和呈现引擎完成了大量工作。 该工作生成了称为 Windows.UI.Composition 的统一合成 WinRT API，用于提供对新的轻型合成对象以及 受动画和效果驱动的全新合成器的访问权限。

Windows.UI.Composition 是可以从任何通用 Windows 平台 (UWP) 应用程序调用的声明性[保留模式](https://msdn.microsoft.com/library/windows/desktop/ff684178.aspx) API ，从而可以直接在应用程序中创建合成对象、 动画和效果。 该 API 是对诸如 XAML 等现有框架的一个强大补充，从而为 UWP 应用程序开发人员 提供了一个熟悉的 C# 图面以供添加到其应用程序。 这些 API 可用于创建 DX 样式框架较少的应用程序。

XAML 开发人员可以使用 WinRT“下拉”到采用 C# 的合成层以便可以在该合成层上执行自定义工作，进而可以在 其 XAML 应用程序中创建对象的“合成岛”，而无需一直下拉到图形层并针对任何自定义 UI 工作使用 DirectX 和 C++。

![](images/layers-win-ui-composition.png)
## <span id="Composition_Objects_and_The_Compositor"></span><span id="composition_objects_and_the_compositor"></span><span id="COMPOSITION_OBJECTS_AND_THE_COMPOSITOR"></span>合成对象和合成器

合成对象通过充当组合对象工厂的 [**Compositor**](https://msdn.microsoft.com/library/windows/apps/Dn706789) 进行创建。 合成器可以创建 [**Visual**](https://msdn.microsoft.com/library/windows/apps/Dn706858) 对象， 以便可以创建已使用和生成 API 中的所有其他功能和合成对象的可视化数结构。

该 API 允许开发人员定义并创建一个或多个 [**Visual**](https://msdn.microsoft.com/library/windows/apps/Dn706858) 对象，其中每个对象表示可视化树中的单个节点。

视觉对象可以是其他视觉对象的容器，也可以托管内容视觉效果。 为了方便使用，该 API 针对层次结构中的特定任务提供了 一组清晰的 [**Visual**](https://msdn.microsoft.com/library/windows/apps/Dn706858) 对象：

-   [
            **Visual**](https://msdn.microsoft.com/library/windows/apps/Dn706858) – 基对象。 大部分属性均位于此处且继承自其他视觉对象。
-   [
            **ContainerVisual**](https://msdn.microsoft.com/library/windows/apps/Dn706810) – 派生自 [**Visual**](https://msdn.microsoft.com/library/windows/apps/Dn706858)，并添加了插入子视觉对象的功能。
-   [
            **SpriteVisual**](https://msdn.microsoft.com/library/windows/apps/Mt589433) – 派生自 [**ContainerVisual**](https://msdn.microsoft.com/library/windows/apps/Dn706810)，并包含图像、效果和交换链形式的内容。
-   [
            **Compositor**](https://msdn.microsoft.com/library/windows/apps/Dn706789) – 用于管理应用程序和系统合成器进程之间关系的对象工厂。

对于其他一些合成对象而言，合成器也是一个工厂，可用于剪裁或转换树中的视觉对象以及丰富的动画和效果集。

## <span id="Effects_System"></span><span id="effects_system"></span><span id="EFFECTS_SYSTEM"></span>效果系统

Windows.UI.Composition 支持实时效果，可以创建动画、自定义和链接。 效果包括 2D 仿射转换、算术复合、混合、颜色源、合成、 对比度、曝光、灰度、gamma 传输、色相旋转、反色、饱和度、棕褐色、温度以及色调。

有关详细信息，请参阅[合成效果](composition-effects.md)概述。

## <span id="Animation_System"></span><span id="animation_system"></span><span id="ANIMATION_SYSTEM"></span>动画系统

Windows.UI.Composition 包含一个极具表现力的框架不可知的动画系统，以便你可以设置以下两种类型的动画：关键帧动画和表达式动画。 这些动画用于移动视觉对象， 驱动转换或剪裁，或者创建动画效果。 通过直接运行在合成器进程中，以此来确保流畅和可扩展性，进而使你可以运行大量并发、独特的动画。

有关详细信息，请参阅[合成动画](composition-animation.md)概述。

## <span id="XAML_Interoperation"></span><span id="xaml_interoperation"></span><span id="XAML_INTEROPERATION"></span>XAML 互操作

除了从头开始创建可视化树外，合成 API 还可以使用 [**Windows.UI.Xaml.Hosting**](https://msdn.microsoft.com/library/windows/apps/Hh701908) 中的 [**ElementCompositionPreview**](https://msdn.microsoft.com/library/windows/apps/Mt608976) 类 与现有 XAML UI 交互操作。


**注意**  
本文适用于编写通用 Windows 平台 (UWP) 应用的 Windows 10 开发人员。 如果你要针对 Windows 8.x 或 Windows Phone 8.x 进行开发， 请参阅[存档文档](http://go.microsoft.com/fwlink/p/?linkid=619132)。

 

## <span id="Additional_Resources_"></span><span id="additional_resources_"></span><span id="ADDITIONAL_RESOURCES_"></span>其他资源：

-   阅读 Kenny Kerr 的关于此 API 的 MSDN 文章：[图形和动画 - Windows 合成支持 10 倍缩放](https://msdn.microsoft.com/magazine/mt590968)
-   [合成 GitHub](https://github.com/Microsoft/composition) 中的合成示例。
-   [
            **API 的完全参考文档**](https://msdn.microsoft.com/library/windows/apps/Dn706878)。
-   已知问题：[已知问题](https://social.msdn.microsoft.com/Forums/en-US/home?forum=Win10SDKToolsIssues)。

 

 







<!--HONumber=Jun16_HO4-->


