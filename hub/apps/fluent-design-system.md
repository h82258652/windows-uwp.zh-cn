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
ms.openlocfilehash: 63e0cf18c2df28db22e79a057761996f9e8d679b
ms.sourcegitcommit: d1c3e13de3da3f7dce878b3735ee53765d0df240
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/24/2019
ms.locfileid: "66215182"
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

除了设计指南之外，我们的 Fluent Design 文章还展示了如何编写代码来实现你的设计。 UWP 使用 XAML，这是一种基于标记的语言，可用于更加轻松地创建用户界面。 以下是一个示例：

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
        ![fpo image](images/fluent/thumbnail-size-classes.jpg)
    :::column-end:::
    :::column span="2":::
        **Design for the right breakpoints**

        Instead of designing for every individual screen size, focusing on a few key widths (also called "breakpoints") can greatly simplify your designs and code while still making your app look great on small to large screens.

        [Learn about screen sizes and breakpoints](/windows/uwp/design/layout/screen-sizes-and-breakpoints-for-responsive-design)
    :::column-end:::
:::row-end:::

:::row:::
    :::column:::
        ![fpo image](images/fluent/rspd-resize.gif)
    :::column-end:::
    :::column span="2":::
        **Create a responsive layout**

        For an app to feel natural, it should adapt its layout to different screen sizes and devices. You can use automatic sizing, layout panels, visual states, and even separate UI definitions in XAML to create a responsive UI.

        [Learn about responsive design](/windows/uwp/design/layout/responsive-design)
    :::column-end:::
:::row-end:::

:::row:::
    :::column:::
        ![fpo image](images/fluent/devices.jpg)
    :::column-end:::
    :::column span="2":::
        **Design for a spectrum of devices**

        UWP apps can run on a wide variety of Windows-powered devices. It's helpful to understand which devices are available, what they're made for, and how users interact with them.

        [Learn about UWP devices](/windows/uwp/design/devices/)
:::row-end:::

:::row:::
    :::column:::
        ![fpo image](images/fluent/keyboard-shortcuts.jpg)
    :::column-end:::
    :::column span="2":::
        **Optimize for the right input**

        UWP apps automatically support common mouse, keyboard, pen, and touch interactions&mdash;there's nothing extra you have to do. But you can enhance your app with optimized support for specific inputs, like pen and the Surface Dial.

        [Learn about inputs and interactions](/windows/uwp/design/input/input-primer)
:::row-end:::

## <a name="make-it-intuitive"></a>使其直观

当某个体验的行为方式符合用户的期望时，该体验就显得很直观。 通过使用已建立的控件和模式并利用对可访问性和全球化的平台支持，你可以打造一种帮助用户提高效率的毫不费力的体验。

体现共鸣就是在正确的时间做正确的事情。

Fluent 体验始终使用控件和模式，因此它们的行为方式符合用户的期望。 Fluent 体验可供具有各种身体能力的人享用，并加入了全球化功能，使全球各地的人都可以使用它们。

:::row:::
    :::column:::
        ![fpo image](images/fluent/thumbnail-navview.png)
    :::column-end:::
    :::column span="2":::
        **Provide the right navigation**

        Create an effortless experience by using the right app structure and navigation components.

        [Learn about navigation](/windows/uwp/design/basics/navigation-basics/)
:::row-end:::

:::row:::
    :::column:::
        ![fpo image](images/fluent/thumbnail-commanding.png)
    :::column-end:::
    :::column span="2":::
        **Be interactive**

        Buttons, command bars, keyboard shortcuts, and context menus enable users to interact with your app; they're the tools that change a static experience into something dynamic.

        [Learn about commanding](/windows/uwp/design/basics/commanding-basics/)
:::row-end:::

:::row:::
    :::column:::
        ![fpo image](images/fluent/thumbnail-controls-2.jpg)
    :::column-end:::
    :::column span="2":::
        **Use the right control for the job**

        Controls are the building blocks of the user interface; using the right control helps you create a user interface that behaves the way users expect it to.  UWP provides more than 45 controls,ranging from simple buttons to powerful data controls.

        [Learn about UWP controls](/windows/uwp/design/controls-and-patterns/)
:::row-end:::

:::row:::
    :::column:::
        ![inclusive image](images/fluent/thumbnail-inclusive.png)
    :::column-end:::
    :::column span="2":::
        **Be inclusive**
        A well-design app is accessible to people with disabilities. With some extra coding, you can share your app with people around the world.

        [Learn about Usability](/windows/uwp/design/usability/)
:::row-end:::

## <a name="be-engaging-and-immersive"></a>吸引力十足且令人沉醉

Fluent Design 重视华丽的效果。 它融入了真正增强用户体验的物理效果，因为它们模拟了大脑被编程以高效处理的体验。

## <a name="use-light"></a>使用光线

光线能吸引人的注意力。 它能够营造氛围和场所感觉，是提供照明的实用工具。

将光线添加到 UWP 应用：

:::row:::
    :::column:::
        ![fpo image](images/fluent/Nav_Reveal_Animation.gif)
    :::column-end:::
    :::column span="2":::
        **Reveal highlight**

        [Reveal highlight](/windows/uwp/design/style/reveal) uses light to make interactive elements stand out. Light illuminates the elements the user can interact with, revealing hidden borders. Reveal is automatically enabled on some controls, such as list view and grid view. You can enable it on other controls by applying our predefined Reveal highlight styles.
:::row-end:::

:::row:::
    :::column:::
        ![fpo image](images/fluent/traveling-focus-fullscreen-light-rf.gif)
    :::column-end:::
    :::column span="2":::
        **Reveal focus**

        [Reveal focus](/windows/uwp/design/style/reveal-focus) uses light to call attention to the element that currently has input focus.
:::row-end:::

## <a name="create-a-sense-of-depth"></a>创建深度感

我们生活在三维世界。 通过专门将深度融入 UI，我们将平面的二维界面转变为能创建视觉层次、更丰富地有效呈现信息和概念的界面。 它彻底改变了对于在具有层次的物理环境中物体之间相互关系的呈现方式

将深度添加到 UWP 应用：

:::row:::
    :::column:::
        ![fpo image](images/fluent/_parallax_v2.gif)
    :::column-end:::
    :::column span="2":::
        **Parallax**

        [Parallax](/windows/uwp/design/motion/parallax) creates the illusion of depth by making items in the foreground appear to move more quickly than items in the background.
:::row-end:::

## <a name="incorporate-motion"></a>包括动作

想一想电影中的动作设计。 动作的无缝过渡让你能够专注于故事，为你带来真实体验。 我们可以将这些感觉融入设计，引导人们在观影过程中能够轻松从一个任务跳转到另一个任务。

将动作添加到 UWP 应用：

:::row:::
    :::column:::
        ![continuity gif](images/fluent/continuityXbox.gif)
    :::column-end:::
    :::column span="2":::
        **Connected animations**

        [Connected animations](/windows/uwp/design/motion/connected-animation) help the user maintain context by creating a seamless transition between scenes.
:::row-end:::

## <a name="build-it-with-the-right-material"></a>利用正确的材料生成它

在现实生活中，我们周围的材料是能感觉的，生动的。 它们会弯曲、拉伸、弹跳、碎裂、滑动。 材料的这些特质也在数字环境中体现出来，让人们需要伸出手去触摸设计。

将材料添加到 UWP 应用：

:::row:::
    :::column:::
        ![fpo image](images/fluent/acrylic_lighttheme_base.png)
    :::column-end:::
    :::column span="2":::
        **Acrylic**

        [Acrylic](/windows/uwp/design/style/acrylic) is a translucent material that lets the user see layers of content, establishing a hierarchy of UI elements.
:::row-end:::

## <a name="design-toolkits-and-code-samples"></a>设计工具包和代理示例

希望开始使用 Fluent Design 创建你自己的应用？ 适用于 Adobe XD、Adobe Illustrator、Adobe Photoshop、Framer，和 Sketch 的工具包将帮助你开始设计，我们示例将帮助你更快地完成编码。

:::row:::
    :::column:::
        ![fpo image](images/fluent/thumbnail-toolkits.jpg)
    :::column-end:::
    :::column span="2":::
        **Design toolkits and samples page**

        Check out our [Design toolkits and samples page](/windows/uwp/design/downloads/)
:::row-end:::









