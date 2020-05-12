---
title: 使用视觉层现代化桌面应用
description: 使用视觉层增强 .NET 或 Win32 桌面应用的 UI。
ms.date: 03/18/2019
ms.topic: article
keywords: windows 10, uwp
ms.author: jimwalk
author: jwmsft
ms.localizationpriority: medium
ms.openlocfilehash: 33a5f0bc31a8fe1421f7ab0de5f229d2feb77915
ms.sourcegitcommit: ef723e3d6b1b67213c78da696838a920c66d5d30
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/02/2020
ms.locfileid: "82730130"
---
# <a name="using-the-visual-layer-in-desktop-apps"></a>在桌面应用中使用视觉层

现在，可以在非 UWP 桌面应用程序中使用 Windows 运行时 API 来增强 WPF、Windows 窗体和 C++ Win32 应用程序的感观和功能，并利用只能通过 UWP 获得的最新 Windows 10 UI 功能。

在许多情况下，可以使用 [XAML Islands](xaml-islands.md) 将现代化 XAML 控件添加到应用。 但是，如果需要创建超越内置控件功能范围的自定义体验，可以访问视觉层 API。

视觉层为图形、效果和动画提供高性能的保留模式 API。 它是各个 Windows 10 设备上的 UI 基础。 UWP XAML 控件是在视觉层上生成的，成就了 [Fluent Design 系统](/windows/uwp/design/fluent-design-system/index)的许多功能特色，例如“光源”、“深度”、“运动”、“材料”和“比例”。

![使用视觉层创建的用户界面](images/visual-layer-interop/pull-to-animate.gif)

> 使用视觉层创建的用户界面 

## <a name="create-a-visually-engaging-user-interface-in-any-windows-app"></a>在任何 Windows 应用中创建悦目的用户界面

视觉层可让你使用自定义绘制内容（视觉对象）的轻型合成控件，并对应用程序中的这些对象应用强大的动画、效果和操作，来创建悦目的体验。 视觉层不会替代任何现有的 UI 框架，而是为这些框架提供重要的补充。

可以使用视觉层来为应用程序提供独特的外观，并为它建立与众不同的印象。 视觉层还支持 Fluent Design 原则，这些原则旨在使应用程序更易用，吸引用户的更多关注。 例如，可以使用视觉层来创建视觉线索和动画屏幕过渡，以便在屏幕上显示各个项之间的关系。

## <a name="visual-layer-features"></a>视觉层的功能

### <a name="brushes"></a>画笔

使用[合成画笔](/windows/uwp/composition/composition-brushes)可以绘制具有纯色、渐变、图像、视频、复杂效果等内容的 UI 对象。

![使用 Material Creator 创建的鸡蛋](images/visual-layer-interop/egg.gif)

> 使用 [Material Creator 演示应用](https://github.com/Microsoft/WindowsCompositionSamples/tree/master/Demos/MaterialCreator)创建的鸡蛋。 

### <a name="effects"></a>效果

[合成效果](/windows/uwp/composition/composition-effects)包括光线、阴影和滤镜效果列表。 可以动画显示、自定义、链接这些内容，然后直接将其应用到视觉对象。 SceneLightingEffect 可与合成光线结合使用，以创建氛围、深度和材料。

![光线和材料](images/visual-layer-interop/light-interop.gif)

> [Windows UI 合成示例库](https://github.com/Microsoft/WindowsCompositionSamples/tree/master/SampleGallery)中演示的光线和材料。 

### <a name="animations"></a>动画

[合成动画](/windows/uwp/composition/composition-animation)直接在独立于 UI 线程的合成器进程中运行。 这可以确保平滑度和比例适当，因此可以运行大量并发的显式动画。 除了我们所熟悉的用于驱动属性不断变化的关键帧动画以外，还可以使用表达式来设置不同属性（包括用户输入）之间的数学关系。 使用输入驱动的动画可以创建动态流畅响应用户输入的 UI，从而可以提高用户的吸引力。

![使用视觉层创建的用户界面](images/visual-layer-interop/swipe-scroller.gif)

> [Windows UI 合成示例库](https://github.com/Microsoft/WindowsCompositionSamples/tree/master/SampleGallery)中演示的运动。 

## <a name="keep-your-existing-codebase-and-adopt-incrementally"></a>保留现有的基本代码并以增量方式采用

现有应用程序中的代码代表着你不希望失去的重大投资。 可以迁移内容岛以使用视觉层，并将 UI 的剩余部分保留在其现有框架中。  这意味着，可对应用程序 UI 做出重大更新和增强，而无需对现有基本代码进行大量更改。

## <a name="samples-and-tutorials"></a>示例和教程

参考我们的示例进行试验，了解如何在应用程序中使用视觉层。 这些示例和教程可帮助你开始使用视觉层，并演示了功能的工作原理。

### <a name="win32"></a>Win32

- [将视觉层与 Win32 结合使用](using-the-visual-layer-with-win32.md)教程
  - [Hello Composition 示例](https://github.com/Microsoft/Windows.UI.Composition-Win32-Samples/tree/master/cpp/HelloComposition)
- [Hello Vectors 示例](https://github.com/Microsoft/Windows.UI.Composition-Win32-Samples/tree/master/cpp/HelloVectors)
- [虚拟图面示例](https://github.com/Microsoft/Windows.UI.Composition-Win32-Samples/tree/master/cpp/VirtualSurfaces)
- [截屏示例](https://github.com/Microsoft/Windows.UI.Composition-Win32-Samples/tree/master/cpp/ScreenCaptureforHWND)

### <a name="windows-forms"></a>Windows 窗体

- [将视觉层与 Windows 窗体结合使用](using-the-visual-layer-with-windows-forms.md)教程
  - [Hello Composition 示例](https://github.com/Microsoft/Windows.UI.Composition-Win32-Samples/tree/master/dotnet/WinForms/HelloComposition)
- [视觉层集成示例](https://github.com/Microsoft/Windows.UI.Composition-Win32-Samples/tree/master/dotnet/WinForms/VisualLayerIntegration)

### <a name="wpf"></a>WPF

- [将视觉层与 WPF 结合使用](using-the-visual-layer-with-wpf.md)教程
  - [Hello Composition 示例](https://github.com/Microsoft/Windows.UI.Composition-Win32-Samples/tree/master/dotnet/WPF/HelloComposition)
- [视觉层集成示例](https://github.com/Microsoft/Windows.UI.Composition-Win32-Samples/tree/master/dotnet/WPF/VisualLayerIntegration)
- [截屏示例](https://github.com/Microsoft/Windows.UI.Composition-Win32-Samples/tree/master/dotnet/WPF/ScreenCapture)

## <a name="limitations"></a>限制

尽管许多视觉层功能在承载于桌面应用程序中的工作方式与它们在 UWP 应用中相同，但有些功能确实存在一些限制。 下面是需要注意的一些限制：

- 效果链依赖于使用 [Win2D](http://microsoft.github.io/Win2D/html/Introduction.htm) 进行效果描述。 桌面应用程序不支持 [Win2D NuGet 包](https://www.nuget.org/packages/Win2D.uwp)，因此你需要从[源代码](https://github.com/Microsoft/Win2D)重新编译该包。
- 若要进行命中测试，需要自行浏览整个视觉树来执行边界计算。 这与 UWP 中的视觉层相同，只不过在此情况下，无法轻松绑定到任何 XAML 元素进行命中测试。
- 视觉层不提供用于呈现文本的基元。
- 结合使用两种不同的 UI 技术（例如 WPF 和视觉层）时，它们各自负责在屏幕上绘制自身的像素，而不能共享像素。 因此，视觉层内容始终在其他 UI 内容的顶层呈现。 （这称为“空域”问题。）  可能需要执行额外的编码和测试，才能确保视觉层内容根据宿主 UI 调整大小，而不会遮挡其他内容。
- 桌面应用程序中承载的内容不会根据 DPI 自动调整大小或缩放。 可能需要执行额外的步骤才能确保内容能够应对 DPI 的变化。 （有关详细信息，请参阅平台特定的教程。）

## <a name="additional-resources"></a>其他资源

- [可视化层](/windows/uwp/composition/visual-layer)
- [合成视觉对象](/windows/uwp/composition/composition-visual-tree)
- [合成画笔](/windows/uwp/composition/composition-brushes)
- [合成效果](/windows/uwp/composition/composition-effects)
- [合成动画](/windows/uwp/composition/composition-animation)

API 参考

- [Windows.UI.Composition](/uwp/api/Windows.UI.Composition)