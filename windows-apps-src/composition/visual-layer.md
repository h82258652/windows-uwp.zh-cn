---
ms.assetid: a2751e22-6842-073a-daec-425fb981bafe
title: 可视化层
description: Windows.UI.Composition API 使你能够访问框架层 (XAML) 和图形层 (DirectX) 之间的合成层。
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 4607280fd031fa556bfc5d1c719f4b4e1aeb928e
ms.sourcegitcommit: f0f933d5cf0be734373a7b03e338e65000cc3d80
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/21/2019
ms.locfileid: "65984103"
---
# <a name="visual-layer"></a>可视化层

可视化层为图形、效果和动画提供高性能的保留模式 API，是 Windows 设备上所有 UI 的基础。 可采用声明方式定义 UI，可视化层依靠图形硬件加速来确保内容、效果和动画以独立于应用 UI 线程的流畅、无故障的方式进行呈现。

值得注意的要点：

* 熟悉的 WinRT API
* 为更动态的 UI 和交互而设计
* 符合设计工具的概念
* 无突发性能下降的线性可扩展性

Windows UWP 应用已在通过 UI 框架之一使用可视化层。 还可以非常轻松地直接将可视化层用于自定义呈现、效果和动画。

![UI 框架分层：框架层 (Windows.UI.XAML) 基于可视化层 (Windows.UI.Composition) 生成，而可视化层基于图形层 (DirectX) 生成](images/layers-win-ui-composition.png)

## <a name="whats-in-the-visual-layer"></a>可视化层中是什么？

可视化层的主要功能有：

1. **内容**:轻量级组合的自定义绘制的内容的情况下
1. **效果**:实时 UI 影响的系统，其效果可以经过动画处理的、 链接和自定义
1. **动画**:运行独立于 UI 线程的富有表现力的不区分框架的动画

### <a name="content"></a>内容

内容由使用视觉对象的动画和效果系统进行托管、转换并提供进行使用。 类层次结构的底层是 [**Visual**](https://msdn.microsoft.com/library/windows/apps/Dn706858) 类，这是应用进程中用于合成器中的视觉状态的轻型线程敏捷代理。 视觉对象的类包括  [**ContainerVisual** ](https://msdn.microsoft.com/library/windows/apps/Dn706810)若要允许的子级创建的视觉对象的树并[ **SpriteVisual** ](https://msdn.microsoft.com/library/windows/apps/Mt589433) ，包含内容，并且可以使用任一纯色，自定义绘制的内容或可视化效果绘制。 这些 Visual 类型一起组成 2D UI 的可视化树结构并支持大多数可见 XAML FrameworkElement。

有关详细信息，请参阅[合成视觉对象](composition-visual-tree.md)概述。

### <a name="effects"></a>Effects

可视化层中的效果系统使你可以将一系列筛选器和透明效果应用于视觉对象或视觉对象树。 这是 UI 效果系统，不要与图像和媒体效果混淆。 效果与动画系统配合使用，从而使用户可以实现效果属性的流畅动态动画（独立于 UI 线程呈现）。 可视化层中的效果提供有创意的构建基块，它们可以进行组合和动画处理，以构造定制的交互体验。

除了可动画处理的效果链之外，可视化层还支持一个照明模型，它使视觉对象可以通过响应可动画处理的光来模拟材料属性。 视觉对象还可能会投影。 照明和阴影可以组合起来以创造深度和现实感。

有关详细信息，请参阅[合成效果](composition-effects.md)概述。

### <a name="animations"></a>Animations

可视化层中的动画系统使你可以移动视觉对象、对效果进行动画处理以及驱动转换、剪辑和其他属性。  它是与框架无关的系统，是在考虑到性能的情况下从头开始设计的。  它独立于 UI 线程运行，以确保流畅性和可扩展性。  虽然它使你可以使用熟悉的 KeyFrame 动画随时间推移驱动属性更改，不过也使你可以在不同属性（包括用户输入）之间设置数学关系，从而使你可以直接创造无缝的精心设计的体验。

有关详细信息，请参阅[合成动画](composition-animation.md)概述。

### <a name="working-with-your-xaml-uwp-app"></a>使用 XAML UWP 应用

可以访问 XAML 框架创建的视觉对象，并使用 [**Windows.UI.Xaml.Hosting**](https://msdn.microsoft.com/library/windows/apps/Hh701908) 中的 [**ElementCompositionPreview**](https://msdn.microsoft.com/library/windows/apps/Mt608976) 类支持可见 FrameworkElement。 请注意，框架创建的视觉对象对自定义具有一些限制。 这是因为框架在管理偏移、转换和生存期。 但是可以创建自己的视觉对象，并通过 ElementCompositionPreview 将它们附加到现有 XAML 元素（或通过将其添加到可视化树结构中某个位置的现有 ContainerVisual）。

有关详细信息，请参阅[将可视化层与 XAML 结合使用](using-the-visual-layer-with-xaml.md)概述。

### <a name="working-with-your-desktop-app"></a>使用桌面应用程序

可以使用可视化层以增强的外观、 感受和功能在 WPF 中，Windows 窗体和C++Win32 桌面应用程序。 你可以迁移岛的内容，使用可视化层，并将你的 UI 的其余部分保留在其现有的框架。 这意味着您可以进行重大更新和增强功能到应用程序 UI 而无需对现有代码进行大量更改基。

有关详细信息，请参阅[实现使用可视化层桌面应用程序的现代化](/windows/apps/desktop/modernize/visual-layer-in-desktop-apps)。

## <a name="additional-resources"></a>其他资源

* [**API 的完整的参考文档**](https://msdn.microsoft.com/library/windows/apps/Dn706878)
* [WindowsUIDevLabs GitHub](https://github.com/microsoft/windowsuidevlabs) 中的高级 UI 和复合示例
* [Windows.UI.Composition 示例库](https://aka.ms/winuiapp)
* [@windowsui Twitter 订阅源 ](https://twitter.com/windowsui)
* 阅读有关此 API Kenny Kerr 的 MSDN 文章：[图形和动画-Windows 组合支持 10 倍缩放](https://msdn.microsoft.com/magazine/mt590968)
