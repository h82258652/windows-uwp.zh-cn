---
title: 实现使用可视化层桌面应用程序的现代化
description: 使用可视化层以增强.NET 或 Win32 桌面应用的 UI。
ms.date: 03/18/2019
ms.topic: article
keywords: windows 10, uwp
ms.author: jimwalk
author: jwmsft
ms.localizationpriority: medium
ms.openlocfilehash: 249291c59a31036fa967ac338209404557b57503
ms.sourcegitcommit: d1c3e13de3da3f7dce878b3735ee53765d0df240
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/24/2019
ms.locfileid: "66215177"
---
# <a name="using-the-visual-layer-in-desktop-apps"></a>在桌面应用程序中使用可视化层

你现在可以使用非 UWP 桌面应用程序中的 UWP Api 来增强外观、 感觉和你的 WPF、 Windows 窗体的功能和C++Win32 应用程序，并充分利用最新，才可通过 UWP 的 Windows 10 用户界面功能。

对于许多方案中，你可以使用[XAML 群岛](xaml-islands.md)现代的 XAML 控件添加到您的应用程序。 但是，当您需要创建超出内置控件的自定义体验，可以访问可视化层的 Api。

可视化层提供高性能、 图形、 效果和动画的保留模式 API。 它是 UI 跨 Windows 10 设备的基础。 可视化层上生成 UWP XAML 控件并使的许多方面[Fluent 设计系统](/windows/uwp/design/fluent-design-system/index)，例如光、 深度、 运动、 材料和规模。

![使用可视化层创建的用户界面](images/visual-layer-interop/pull-to-animate.gif)

> _使用可视化层创建的用户界面_

## <a name="create-a-visually-engaging-user-interface-in-any-windows-app"></a>在任何 Windows 应用中创建一个直观且具有吸引力的用户界面

可视化层，可以通过使用轻量组合的自定义绘制的内容 （视觉对象） 的情况下，并在应用程序中这些对象上应用强大的动画、 效果和操作创建具有吸引力的体验。 可视化层不会替换任何现有的 UI 框架;相反，它是到这些框架的重要补充。

可以使用可视化层为你的应用程序提供唯一的外观和感觉，并建立一个标识，使其不同于其他应用程序标识。 它还使 Fluent 设计原则，旨在使您的应用程序更易于使用，从用户绘制参与度。 例如，您可以使用它来创建视觉提示和经过动画处理的屏幕过渡效果，以在屏幕上显示项目之间的关系。

## <a name="visual-layer-features"></a>可视化层功能

### <a name="brushes"></a>画笔

[组合画笔](/windows/uwp/composition/composition-brushes)let 绘制 UI 对象使用纯色、 渐变、 图像、 视频、 复杂的影响和的详细信息。

![使用材料创建者创建鸡蛋](images/visual-layer-interop/egg.gif)

> _使用创建鸡蛋[材料创建者演示应用](https://github.com/Microsoft/WindowsCompositionSamples/tree/master/Demos/MaterialCreator)。_

### <a name="effects"></a>Effects

[组合效果](/windows/uwp/composition/composition-effects)包括 light、 阴影和筛选器效果的列表。 它们可以是经过动画处理的、 自定义，并链接在一起，然后直接应用于视觉对象。 可以组合照明创建 atmosphere、 深度和材料与组合 SceneLightingEffect。

![灯和材料](images/visual-layer-interop/light-interop.gif)

> _灯和材料中所示[Windows UI 复合示例库](https://github.com/Microsoft/WindowsCompositionSamples/tree/master/SampleGallery)。_

### <a name="animations"></a>Animations

[组合动画](/windows/uwp/composition/composition-animation)直接在复合器进程，独立于 UI 线程中运行。 这可确保流畅程度和可扩展性，因此你可以运行大量的并发、 显式的动画。 除了熟悉随着时间的推移驱动器属性更改的关键帧动画，可以使用表达式来设置其他属性，包括用户输入之间的数学关系。 输入驱动的动画，可以创建动态和流畅地响应用户输入，这可能会导致更高版本的用户参与度的 UI。

![使用可视化层创建的用户界面](images/visual-layer-interop/swipe-scroller.gif)

> _运动中所示[Windows UI 复合示例库](https://github.com/Microsoft/WindowsCompositionSamples/tree/master/SampleGallery)。_

## <a name="keep-your-existing-codebase-and-adopt-incrementally"></a>保留现有基本代码和以增量方式采用

将现有应用程序中的代码表示您不希望丢失的投入。 你可以迁移_群岛_要使用可视化层，使其余的 UI 保持其现有框架的内容。 这意味着您可以进行重大更新和增强功能到应用程序 UI 而无需对现有代码进行大量更改基。

## <a name="samples-and-tutorials"></a>示例和教程

了解如何通过使用我们的示例应用程序中使用可视化层。 这些示例和教程帮助您开始使用可视化层和显示功能的工作原理。

### <a name="win32"></a>Win32

- [可视化层中使用 Win32](using-the-visual-layer-with-win32.md)教程
  - [Hello 组合示例](https://github.com/Microsoft/Windows.UI.Composition-Win32-Samples/tree/master/cpp/HelloComposition)
- [Hello 向量示例](https://github.com/Microsoft/Windows.UI.Composition-Win32-Samples/tree/master/cpp/HelloVectors)
- [虚拟图面示例](https://github.com/Microsoft/Windows.UI.Composition-Win32-Samples/tree/master/cpp/VirtualSurfaces)
- [屏幕捕获示例](https://github.com/Microsoft/Windows.UI.Composition-Win32-Samples/tree/master/cpp/ScreenCaptureforHWND)

### <a name="windows-forms"></a>Windows 窗体

- [可视化层中使用 Windows 窗体](using-the-visual-layer-with-windows-forms.md)教程
  - [Hello 组合示例](https://github.com/Microsoft/Windows.UI.Composition-Win32-Samples/tree/master/dotnet/WinForms/HelloComposition)
- [可视化层集成示例](https://github.com/Microsoft/Windows.UI.Composition-Win32-Samples/tree/master/dotnet/WinForms/VisualLayerIntegration)

### <a name="wpf"></a>WPF

- [可视化层中使用 WPF](using-the-visual-layer-with-wpf.md)教程
  - [Hello 组合示例](https://github.com/Microsoft/Windows.UI.Composition-Win32-Samples/tree/master/dotnet/WPF/HelloComposition)
- [可视化层集成示例](https://github.com/Microsoft/Windows.UI.Composition-Win32-Samples/tree/master/dotnet/WPF/VisualLayerIntegration)
- [屏幕捕获示例](https://github.com/Microsoft/Windows.UI.Composition-Win32-Samples/tree/master/dotnet/WPF/ScreenCapture)

## <a name="limitations"></a>限制

虽然许多可视化层功能的工作方式与在 UWP 应用中托管的桌面应用程序时相同，某些功能有限制。 下面是一些需要注意的限制：

- 依赖于影响链[Win2D](http://microsoft.github.io/Win2D/html/Introduction.htm)效果说明。 [Win2D NuGet 包](https://www.nuget.org/packages/Win2D.uwp)不支持在桌面应用程序，因此需要重新编译它从[源代码](https://github.com/Microsoft/Win2D)。
- 若要进行命中测试，需要通过遍历可视化树自己执行边界计算。 这等同于可视化层中 UWP，但在这种情况下没有任何 XAML 元素，您可以轻松地绑定到进行命中测试。
- 可视化层没有呈现文本的基元。
- 时同时使用了两个不同的 UI 技术，如 WPF 和可视化层中，它们分别负责绘制其自己的像素为单位的屏幕上，并且它们不能共享像素为单位。 因此，可视化层的内容始终呈现在其他 UI 内容中。 (我们称之为_空域_问题。)可能需要进行额外编码和测试以确保你可视化层的内容与主机用户界面调整大小，不会遮蔽其他内容。
- 托管桌面应用程序中的内容不会自动调整大小或对 DPI 缩放。 确保你的内容处理 DPI 更改可能需要额外的步骤。 （请参阅详细信息的平台特定教程。）

## <a name="additional-resources"></a>其他资源

- [可视化层](/windows/uwp/composition/visual-layer)
- [组合 visual](/windows/uwp/composition/composition-visual-tree)
- [组合画笔](/windows/uwp/composition/composition-brushes)
- [组合效果](/windows/uwp/composition/composition-effects)
- [组合动画](/windows/uwp/composition/composition-animation)

API 参考

- [Windows.UI.Composition](/uwp/api/Windows.UI.Composition)