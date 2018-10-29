---
description: 了解 Fluent Design 以及如何将它合并到应用。
title: 适用于 Windows 的 fluent Design 系统
author: mijacobs
keywords: uwp 应用布局, 通用 windows 平台, 应用设计, 界面, Fluent Design 系统
ms.author: mijacobs
ms.date: 3/7/2018
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 2ab8e8c18a0b1db0991bf470f194f8774f2357b4
ms.sourcegitcommit: 753e0a7160a88830d9908b446ef0907cc71c64e7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/29/2018
ms.locfileid: "5747613"
---
# <a name="the-fluent-design-system-for-windows-app-creators"></a>Fluent 设计系统的 Windows 应用创意者

![Fluent 设计标头](images/fluentdesign-app-header.jpg)

## <a name="introduction"></a>介绍

Fluent Design 系统是用于创建自适应、 共鸣和美观用户界面的系统。

## <a name="principles"></a>原则

**自适应：Fluent 体验在每台设备上都显得自然**

Fluent 体验可根据环境进行调整。 Fluent 体验感觉舒适平板电脑、 台式机、 和 Xbox 上，它甚至可以很好运行混合现实头戴显示设备上。 此外，当你添加更多硬件时（例如用于电脑的一台额外显示器），Fluent 体验将利用它。

**共鸣：Fluent 体验直观且强大**

Fluent 体验可根据行为和意图进行调整 &mdash; 它们能了解和预测需求。 它们将人和思想结合起来，不管他们是站在地球的两头还是站在一起。

**美观：Fluent 体验吸引力十足且令人沉醉**

通过融入物理世界的元素，Fluent 体验挖掘到了根本的东西。 它运用光线、阴影、运动、深度和纹理，以一种直观和本能的方式整理信息。


## <a name="applying-fluent-design-to-your-app-with-uwp"></a>应用到你的应用的 UWP 的 Fluent 设计

![Fluent 设计徽标](images/fluentdesign_header.png)

我们的设计指南中介绍如何适用于应用的 Fluent 设计原则。 哪种类型的应用？ 尽管我们的指南中的许多可应用于任何平台，我们创建了 UWP （通用 Windows 平台） 以支持 Fluent Design。

Fluent Design 功能内置于 UWP 中。 其中的一些功能（如有效像素和通用输入系统）是自动的。 你无需编写额外的代码，可以直接使用这些功能。 其他功能（如亚克力）是可选的；你需要通写用于包含它们代码，从而将它们添加到应用。

> 我们正在将 UWP 控件引入桌面，以帮助您通过 Fluent Design 功能增强现有 WPF 或 Windows 应用程序的外观、使用体验和功能。 若要了解详细信息，请参阅[WPF 和 Windows 窗体应用程序中的主机 UWP 控件](/windows/uwp/xaml-platform/xaml-host-controls)。

<!-- To apply Fluent Design to your app, follow our guidelines and use UWP (Universal Windows Platform) you can use UWP UI features combined with best practices for creating apps that perform beautifully on all types of Windows-powered devices. -->

除了设计指南，我们 Fluent Design 文章还向你介绍如何编写代码，使你的设计发生这种情况。 UWP 使用 XAML 中，可轻松创建用户界面的基于标记语言。 下面是一个示例：

```xaml
<Grid BorderBrush="Blue" BorderThickness="4">
    <TextBox Text="Design with XAML" Margin="20" Padding="24,16"/>
</Grid>
```

![](images/xaml-example.png)


> 如果你不熟悉 UWP 开发，请查看我们[开始使用 UWP 页面](https://developer.microsoft.com/windows/apps/getstarted)。

## <a name="find-a-natural-fit"></a>寻找天然良配

你如何使应用在各种设备上都显得自然？ 通过让人觉得在设计它时考虑了每一台特定的设备。 适合不同的屏幕大小的 UI 布局（旨在充分利用空间，同时不显得拥挤）使体验显得自然，就像它是专为该设备设计的一样。

:::row:::
    :::column:::
        ![fpo image](images/thumbnail-size-classes.jpg)
    :::column-end:::
    :::column span="2":::
        **Design for the right breakpoints**

        Instead of designing for every individual screen size, focusing on a few key widths (also called "breakpoints") can greatly simplify your designs and code while still making your app look great on small to large screens.

        [Learn about screen sizes and breakpoints](/windows/uwp/design/layout/screen-sizes-and-breakpoints-for-responsive-design)
    :::column-end:::
:::row-end:::

:::row:::
    :::column:::
        ![fpo image](images/rspd-resize.gif)
    :::column-end:::
    :::column span="2":::
        **Create a responsive layout**

        For an app to feel natural, it should adapt its layout to different screen sizes and devices. You can use automatic sizing, layout panels, visual states, and even separate UI definitions in XAML to create a responsive UI.

        [Learn about responsive design](/windows/uwp/design/layout/responsive-design)
    :::column-end:::
:::row-end:::

:::row:::
    :::column:::
        ![fpo image](images/devices.jpg)
    :::column-end:::
    :::column span="2":::
        **Design for a spectrum of devices**

        UWP apps can run on a wide variety of Windows-powered devices. It's helpful to understand which devices are available, what they're made for, and how users interact with them.

        [Learn about UWP devices](/windows/uwp/design/devices/)
:::row-end:::

:::row:::
    :::column:::
        ![fpo image](images/keyboard-shortcuts.jpg)
    :::column-end:::
    :::column span="2":::
        **Optimize for the right input**

        UWP apps automatically support common mouse, keyboard, pen, and touch interactions&mdash;there's nothing extra you have to do. But you can enhance your app with optimized support for specific inputs, like pen and the Surface Dial.

        [Learn about inputs and interactions](/windows/uwp/design/input/input-primer)
:::row-end:::

## <a name="make-it-intuitive"></a>使直观

当它的行为，用户期望到它的方法，该体验就显得直观。 通过使用已建立的控件和模式并利用对可访问性和全球化的平台支持，你可以打造一种帮助用户提高效率的毫不费力的体验。

体现共鸣就是在正确的时间做正确的事情。

Fluent 体验始终使用控件和模式，因此它们的行为方式符合用户的期望。 Fluent 体验可供具有各种身体能力的人享用，并加入了全球化功能，使全球各地的人都可以使用它们。

:::row:::
    :::column:::
        ![fpo image](images/thumbnail-navview.png)
    :::column-end:::
    :::column span="2":::
        **Provide the right navigation**

        Create an effortless experience by using the right app structure and navigation components.

        [Learn about navigation](/windows/uwp/design/basics/navigation-basics/)
:::row-end:::

:::row:::
    :::column:::
        ![fpo image](images/thumbnail-commanding.png)
    :::column-end:::
    :::column span="2":::
        **Be interactive**

        Buttons, command bars, keyboard shortcuts, and context menus enable users to interact with your app; they're the tools that change a static experience into something dynamic.

        [Learn about commanding](/windows/uwp/design/basics/commanding-basics/)
:::row-end:::

:::row:::
    :::column:::
        ![fpo image](images/thumbnail-controls-2.jpg)
    :::column-end:::
    :::column span="2":::
        **Use the right control for the job**

        Controls are the building blocks of the user interface; using the right control helps you create a user interface that behaves the way users expect it to.  UWP provides more than 45 controls,ranging from simple buttons to powerful data controls.

        [Learn about UWP controls](/windows/uwp/design/controls-and-patterns/)
:::row-end:::

:::row:::
    :::column:::
        ![inclusive image](images/thumbnail-inclusive.png)
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
        ![fpo image](../style/images/Nav_Reveal_Animation.gif)
    :::column-end:::
    :::column span="2":::
        **Reveal highlight**

        [Reveal highlight](../style/reveal.md) uses light to make interactive elements stand out. Light illuminates the elements the user can interact with, revealing hidden borders. Reveal is automatically enabled on some controls, such as list view and grid view. You can enable it on other controls by applying our predefined Reveal highlight styles.
:::row-end:::

:::row:::
    :::column:::
        ![fpo image](../style/images/traveling-focus-fullscreen-light-rf.gif)
    :::column-end:::
    :::column span="2":::
        **Reveal focus**

        [Reveal focus](../style/reveal-focus.md) uses light to call attention to the element that currently has input focus.
:::row-end:::

## <a name="create-a-sense-of-depth"></a>创建深度感

我们生活在三维世界。 通过专门将深度融入 UI，我们将平面的二维界面转变为能创建视觉层次、更丰富地有效呈现信息和概念的界面。 它彻底改变了对于在具有层次的物理环境中物体之间相互关系的呈现方式

将深度添加到 UWP 应用：

:::row:::
    :::column:::
        ![fpo image](../motion/images/_parallax_v2.gif)
    :::column-end:::
    :::column span="2":::
        **Parallax**

        [Parallax](../motion/parallax.md) creates the illusion of depth by making items in the foreground appear to move more quickly than items in the background.
:::row-end:::

## <a name="incorporate-motion"></a>融入运动

想一想电影中的运动设计。 运动的无缝过渡让你能够专注于故事，为你带来真实体验。 我们可以将这些感觉融入设计，引导人们在观影过程中能够轻松从一个任务跳转到另一个任务。

将运动添加到 UWP 应用：

:::row:::
    :::column:::
        ![continuity gif](images/continuityXbox.gif)
    :::column-end:::
    :::column span="2":::
        **Connected animations**

        [Connected animations](../motion/connected-animation.md) help the user maintain context by creating a seamless transition between scenes.
:::row-end:::

## <a name="build-it-with-the-right-material"></a>利用正确的材料生成它

在现实生活中，我们周围的材料是能感觉的，生动的。 它们会弯曲、拉伸、弹跳、碎裂、滑动。 材料的这些特质也在数字环境中体现出来，让人们需要伸出手去触摸设计。

将材料添加到 UWP 应用：

:::row:::
    :::column:::
        ![fpo image](../style/images/acrylic_lighttheme_base.png)
    :::column-end:::
    :::column span="2":::
        **Acrylic**

        [Acrylic](../style/acrylic.md) is a translucent material that lets the user see layers of content, establishing a hierarchy of UI elements.
:::row-end:::

## <a name="design-toolkits-and-code-samples"></a>设计工具包和代理示例

希望开始使用 Fluent Design 创建你自己的应用？ 适用于 Adobe XD、Adobe Illustrator、Adobe Photoshop、Framer，和 Sketch 的工具包将帮助你开始设计，我们示例将帮助你更快地完成编码。

:::row:::
    :::column:::
        ![fpo image](images/thumbnail-toolkits.jpg)
    :::column-end:::
    :::column span="2":::
        **Design toolkits and samples page**

        Check out our [Design toolkits and samples page](/windows/uwp/design/downloads/)
:::row-end:::









