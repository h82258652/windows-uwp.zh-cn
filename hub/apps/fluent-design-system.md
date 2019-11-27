---
description: 了解 Fluent Design 以及如何将它包括到应用中。
title: 适用于 Windows 的 Fluent Design System
keywords: uwp 应用布局, 通用 windows 平台, 应用设计, 界面, Fluent Design System
ms.date: 03/07/2018
ms.topic: article
ms.localizationpriority: medium
ms.custom: RS5
ms.author: mcleans
author: mcleanbyron
ms.openlocfilehash: f3a2575b17cc4228d7c4db273845478aecf65f29
ms.sourcegitcommit: f0936ce8e88d78b1af99998794a8765094f6a487
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/25/2019
ms.locfileid: "72915104"
---
# <a name="the-fluent-design-system-for-windows-app-creators"></a>适用于 Windows 应用创建者的 Fluent Design System

![Fluent Design 标题](images/fluent/fluentdesign-app-header.jpg)

## <a name="introduction"></a>简介

Fluent Design System 是用于创建自适应、引人共鸣且美观的用户界面的系统。

## <a name="principles"></a>原则

**自适应：Fluent 体验在每台设备上都显得自然**

Fluent 体验可根据环境进行调整。 Fluent 体验在平板电脑、台式机和 Xbox 上显得很舒适 - 它甚至在混合现实头戴显示设备中表现得很好。 此外，当你添加更多硬件时（例如用于电脑的一台额外显示器），Fluent 体验将利用它。

**引人共鸣：Fluent 体验直观且强大**

Fluent 体验可根据行为和意图进行调整&mdash;它们能了解和预测需求。 它们将人和思想结合起来，不管他们是站在地球的两头还是站在一起。

**美观：Fluent 体验吸引力十足且令人沉醉**

通过融入物理世界的元素，Fluent 体验挖掘到了根本的东西。 它运用光线、阴影、动作、深度和纹理，以一种直观和本能的方式整理信息。


## <a name="applying-fluent-design-to-your-app-with-uwp"></a>通过 UWP 将 Fluent Design 应用于你的应用

![Fluent Design 徽标](images/fluent/fluentdesign_header.png)

我们的设计指南解释了如何将 Fluent Design 原则应用于应用。 应用是什么类型的？ 虽然我们的许多指南可以应用于任何平台，但我们创建了 UWP（通用 Windows 平台）来支持 Fluent Design。

Fluent Design 功能内置于 UWP 中。 其中的一些功能（例如，有效像素和通用输入系统）是自动的。 你无需编写额外的代码，可以直接使用这些功能。 其他功能（例如亚克力）是可选的；你需要编写代码来包括它们，从而将它们添加到应用。

> 我们正在将 UWP 控件引入桌面，以帮助您通过 Fluent Design 功能增强现有 WPF 或 Windows 应用程序的外观、使用体验和功能。 若要了解详细信息，请参阅[在 WPF 和 Windows 窗体应用程序中托管 UWP 控件](/windows/uwp/xaml-platform/xaml-host-controls)。

<!-- To apply Fluent Design to your app, follow our guidelines and use UWP (Universal Windows Platform) you can use UWP UI features combined with best practices for creating apps that perform beautifully on all types of Windows-powered devices. -->

除了设计指南之外，我们的 Fluent Design 文章还展示了如何编写代码来实现你的设计。 UWP 使用 XAML，这是一种基于标记的语言，可用于更加轻松地创建用户界面。 下面是一个示例：

```xaml
<Grid BorderBrush="Blue" BorderThickness="4">
    <TextBox Text="Design with XAML" Margin="20" Padding="16,24"/>
</Grid>
```

![](images/fluent/xaml-example.png)


> 如果你是初次接触 UWP 开发，请查看我们的 [UWP 页面入门](/windows/uwp/get-started)。

## <a name="find-a-natural-fit"></a>寻找天然良配

你如何使应用在各种设备上都显得自然？ 通过让人觉得在设计它时考虑了每一台特定的设备。 适合不同的屏幕大小的 UI 布局（旨在充分利用空间，同时不显得拥挤）使体验显得自然，就像它是专为该设备设计的一样。

:::row:::
    :::column:::
        ![fpo 图像](images/fluent/thumbnail-size-classes.jpg)
    :::column-end:::
    :::column span="2":::
**针对正确的断点进行设计**

不要针对每个单独的屏幕大小进行设计，侧重于几个关键宽度（也称为“断点”）可以显著简化设计和代码，同时让应用无论在小屏幕还是大屏幕上看起来都很棒。

[了解屏幕大小和断点](/windows/uwp/design/layout/screen-sizes-and-breakpoints-for-responsive-design)
    :::column-end:::
:::row-end:::

:::row:::
    :::column:::
        ![fpo 图像](images/fluent/rspd-resize.gif)
    :::column-end:::
    :::column span="2":::
**创建响应式布局**

要使应用显得自然，应根据不同的屏幕大小和设备调整布局。 可以使用自动大小调整、布局面板、视觉状态，甚至是以 XAML 编写的单独的 UI 定义来创建响应式 UI。

[了解响应式设计](/windows/uwp/design/layout/responsive-design)
    :::column-end:::
:::row-end:::

:::row:::
    :::column:::
        ![fpo 图像](images/fluent/devices.jpg)
    :::column-end:::
    :::column span="2":::
**适用于各种设备的设计**

UWP 应用可在各种支持 Windows 的设备上运行。 了解哪些设备可用、它们的用途以及用户如何与它们交互很有帮助。

[了解 UWP 设备](/windows/uwp/design/devices/)
:::row-end:::

:::row:::
    :::column:::
        ![fpo 图像](images/fluent/keyboard-shortcuts.jpg)
    :::column-end:::
    :::column span="2":::
**针对正确的输入进行优化**

UWP 应用将自动支持常见的鼠标、键盘、手写笔和触控交互 &mdash; 不必执行任何额外的操作。 但是，你可以利用对特定输入（如手写笔和 Surface Dial）的优化支持增强应用。

[了解输入和交互](/windows/uwp/design/input/input-primer)
:::row-end:::

## <a name="make-it-intuitive"></a>使其直观

当某个体验的行为方式符合用户的期望时，该体验就显得很直观。 通过使用已建立的控件和模式并利用对可访问性和全球化的平台支持，你可以打造一种帮助用户提高效率的毫不费力的体验。

体现共鸣就是在正确的时间做正确的事情。

Fluent 体验始终使用控件和模式，因此它们的行为方式符合用户的期望。 Fluent 体验可供具有各种身体能力的人享用，并加入了全球化功能，使全球各地的人都可以使用它们。

:::row:::
    :::column:::
        ![fpo 图像](images/fluent/thumbnail-navview.png)
    :::column-end:::
    :::column span="2":::
**提供正确的导航**

通过使用正确的应用结构和导航组件来打造轻松的体验。

[了解导航](/windows/uwp/design/basics/navigation-basics/)
:::row-end:::

:::row:::
    :::column:::
        ![fpo 图像](images/fluent/thumbnail-commanding.png)
    :::column-end:::
    :::column span="2":::
**进行交互**

借助按钮、命令栏、键盘快捷方式和上下文菜单，用户可以与应用进行交互；这些工具可以将静态体验转变为动态体验。

[了解命令](/windows/uwp/design/basics/commanding-basics/)
:::row-end:::

:::row:::
    :::column:::
        ![fpo 图像](images/fluent/thumbnail-controls-2.jpg)
    :::column-end:::
    :::column span="2":::
**对作业使用正确的控件**

控制是用户界面的构建基块；使用正确的控件可帮助你创建行为方式符合用户的期望的用户界面。 UWP 提供了超过 45 个控件 - 从简单的按钮到强大的数据控件。

[了解 UWP 控件](/windows/uwp/design/controls-and-patterns/)
:::row-end:::

:::row:::
    :::column:::
        ![具有包容性的图像](images/fluent/thumbnail-inclusive.png)
    :::column-end:::
    :::column span="2":::
**具有包容性** 设计精良的应用可供残疾人士使用。 利用一些额外的编码，可以将应用与世界各地的人分享。

[了解可用性](/windows/uwp/design/usability/)
:::row-end:::

## <a name="be-engaging-and-immersive"></a>吸引力十足且令人沉醉

Fluent Design 重视华丽的效果。 它融入了真正增强用户体验的物理效果，因为它们模拟了大脑被编程以高效处理的体验。

## <a name="use-light"></a>使用光线

光线能吸引人的注意力。 它能够营造氛围和场所感觉，是提供照明的实用工具。

将光线添加到 UWP 应用：

:::row:::
    :::column:::
        ![fpo 图像](images/fluent/Nav_Reveal_Animation.gif)
    :::column-end:::
    :::column span="2":::
**突出显示**

[突出显示](/windows/uwp/design/style/reveal)利用光线来突出显示交互式元素。光线会照亮用户可与之交互的元素，从而显示隐藏的边框。 突出显示在某些控件（如列表视图和网格视图）上自动启用。 可以通过应用预定义突出显示样式在其他控件上启用它。
:::row-end:::

:::row:::
    :::column:::
        ![fpo 图像](images/fluent/traveling-focus-fullscreen-light-rf.gif)
    :::column-end:::
    :::column span="2":::
**显示焦点**

[显示焦点](/windows/uwp/design/style/reveal-focus)利用光线来引起对当前具有输入焦点的元素的注意。
:::row-end:::

## <a name="create-a-sense-of-depth"></a>创建深度感

我们生活在三维世界。 通过专门将深度融入 UI，我们将平面的二维界面转变为能创建视觉层次、更丰富地有效呈现信息和概念的界面。 它彻底改变了对于在具有层次的物理环境中物体之间相互关系的呈现方式

将深度添加到 UWP 应用：

:::row:::
    :::column:::
        ![fpo 图像](images/fluent/_parallax_v2.gif)
    :::column-end:::
    :::column span="2":::
**视差**

[视差](/windows/uwp/design/motion/parallax)通过让前景中的物体看起来比背景中的物体移动速度更快，创造出立体感效应。
:::row-end:::

## <a name="incorporate-motion"></a>包括动作

想一想电影中的动作设计。 动作的无缝过渡让你能够专注于故事，为你带来真实体验。 我们可以将这些感觉融入设计，引导人们在观影过程中能够轻松从一个任务跳转到另一个任务。

将动作添加到 UWP 应用：

:::row:::
    :::column:::
        ![连续性 Gif](images/fluent/continuityXbox.gif)
    :::column-end:::
    :::column span="2":::
**连接的动画**

[连接动画](/windows/uwp/design/motion/connected-animation)通过在场景之间无缝切换帮助用户保持画面的连贯。
:::row-end:::

## <a name="build-it-with-the-right-material"></a>利用正确的材料生成它

在现实生活中，我们周围的材料是能感觉的，生动的。 它们会弯曲、拉伸、弹跳、碎裂、滑动。 材料的这些特质也在数字环境中体现出来，让人们需要伸出手去触摸设计。

将材料添加到 UWP 应用：

:::row:::
    :::column:::
        ![fpo 图像](images/fluent/acrylic_lighttheme_base.png)
    :::column-end:::
    :::column span="2":::
**Acrylic**

[亚克力](/windows/uwp/design/style/acrylic)是一种让用户能够看到内容层次的透明材料，它也为 UI 元素创建层次结构。
:::row-end:::

## <a name="design-toolkits-and-code-samples"></a>设计工具包和代理示例

希望开始使用 Fluent Design 创建你自己的应用？ 适用于 Adobe XD、Adobe Illustrator、Adobe Photoshop、Framer，和 Sketch 的工具包将帮助你开始设计，我们示例将帮助你更快地完成编码。

:::row:::
    :::column:::
        ![fpo 图像](images/fluent/thumbnail-toolkits.jpg)
    :::column-end:::
    :::column span="2":::
**“设计工具包和示例”页面**

查看[“设计工具包和示例”页面](/windows/uwp/design/downloads/)
:::row-end:::









