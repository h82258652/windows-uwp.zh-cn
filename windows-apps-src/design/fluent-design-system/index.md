---
description: 了解 Fluent Design 以及如何将它合并到应用。
title: UWP 应用的 Fluent Design 系统
author: mijacobs
keywords: uwp 应用布局, 通用 windows 平台, 应用设计, 界面, Fluent Design 系统
ms.author: mijacobs
ms.date: 3/7/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
ms.localizationpriority: high
ms.openlocfilehash: 5b57dc2ddae4c6e260df663097db5649866b96aa
ms.sourcegitcommit: ef5a1e1807313a2caa9c9b35ea20b129ff7155d0
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/08/2018
ms.locfileid: "1638785"
---
# <a name="the-fluent-design-system-for-uwp-apps"></a>UWP 应用的 Fluent Design 系统

## <a name="introduction"></a>简介

<img src="images/fluentdesign-app-header.jpg" alt=" " />

用户界面在不断改进。 它增加了新的维度和交互渠道 - 从 2D 到 3D 和更高维度，从键盘和鼠标到注视、手写笔和触控。  

Fluent Design 系统是一套结合了最佳实践的创新型 UWP 功能，用于创建在所有类型的支持 Windows 的设备上都表现出色的应用。

它是用于创建自适应、共鸣和美观的用户界面的系统。 

**自适应：Fluent 体验在每台设备上都显得自然**

Fluent 体验可根据环境进行调整。 Fluent 体验在平板电脑、台式机和 Xbox 上显得很舒适 &mdash; 它甚至在混合现实头戴显示设备中表现得很好。 此外，当你添加更多硬件时（例如用于电脑的一台额外显示器），Fluent 体验将利用它。 

**共鸣：Fluent 体验直观且强大**

Fluent 体验可根据行为和意图进行调整 &mdash; 它们能了解和预测需求。 它们将人和思想结合起来，不管他们是站在地球的两头还是站在一起。 

体现共鸣就是在正确的时间做正确的事情。 

Fluent 体验始终使用控件和模式，因此它们的行为方式符合用户的期望。 Fluent 体验可供具有各种身体能力的人享用，并加入了全球化功能，使全球各地的人都可以使用它们。 

**美观：Fluent 体验吸引力十足且令人沉醉** 

通过融入物理世界的元素，Fluent 体验挖掘到了根本的东西。 它运用光线、阴影、运动、深度和纹理，以一种直观和本能的方式整理信息。 

Fluent Design 重视华丽的效果。 它融入了真正增强用户体验的物理效果，因为它们模拟了大脑被编程以高效处理的体验。 

## <a name="applying-fluent-design-to-your-app"></a>将 Fluent Design 应用于应用

Fluent Design 功能内置于 UWP 中。 其中的一些功能（如有效像素和通用输入系统）是自动的。 你无需编写额外的代码，可以直接使用这些功能。 其他功能（如亚克力）是可选的；你需要通写用于包含它们代码，从而将它们添加到应用。 

> 若要了解有关将在每个 UWP 应用中自动包含的基本功能的详细信息，请参阅 [UWP 应用设计简介文章](../basics/design-and-ui-intro.md)。 如果你完全不熟悉 UWP 开发，你可能想先查看我们 [“UWP 入门”页面](https://developer.microsoft.com/windows/apps/getstarted)。 

要详细了解可帮助将 Fluent Design 加入到应用中的新功能，请继续阅读。

## <a name="find-a-natural-fit"></a>寻找天然良配

你如何使应用在各种设备上都显得自然？ 通过让人觉得在设计它时考虑了每一台特定的设备。 适合不同的屏幕大小的 UI 布局（旨在充分利用空间，同时不显得拥挤）使体验显得自然，就像它是专为该设备设计的一样。 

*  **针对正确的断点进行设计**

    不要针对每个单独的屏幕大小进行设计，侧重于几个关键宽度（也称为“断点”）可以显著简化设计和代码，同时让应用无论在小屏幕还是大屏幕上看起来都很棒。

    [了解屏幕大小和断点](/windows/uwp/design/layout/screen-sizes-and-breakpoints-for-responsive-design)

*  **创建响应式布局**

    要让应用看起来自然，则需要让应用填充可用的显示空间但又不显得太拥挤。 UWP 提供了整理网格、堆栈和流中的内容的面板，你可以将这些面板互相嵌套。

    [了解 UWP 的布局面板](/windows/uwp/design/layout/layout-panels)

* **适用于各种设备的设计**

    UWP 应用可在各种支持 Windows 的设备上运行。 了解哪些设备可用、它们的用途以及用户如何与它们交互很有帮助。

    [了解 UWP 设备](/windows/uwp/design/devices/)

* **针对正确的输入进行优化**

    UWP 应用将自动支持常见的鼠标、键盘、手写笔和触控交互 &mdash; 不必执行任何额外的操作。 但是，你可以利用对特定输入（如手写笔和 Surface Dial）的优化支持增强应用。

    [了解输入和交互](/windows/uwp/design/input/input-primer)


## <a name="make-it-intuitive-and-powerful"></a>使其变得直观和强大

当某个体验的行为方式符合用户的期望时，该体验就显得很直观。 通过使用已建立的控件和模式并利用对可访问性和全球化的平台支持，你可以打造一种帮助用户提高效率的毫不费力的体验。 

* **对作业使用正确的控件**

    控制是用户界面的构建基块；使用正确的控件可帮助你创建行为方式符合用户的期望的用户界面。  UWP 提供了超过 45 个控件 - 从简单的按钮到强大的数据控件。 

    [了解 UWP 控件](/windows/uwp/design/controls-and-patterns/)

* **具有包容性** 

    设计精良的应用可供残障人士使用。 利用一些额外的编码，你可以将应用与世界各地的人分享。

    [了解可用性](/windows/uwp/design/usability/)


## <a name="be-engaging-and-immersive"></a>吸引力十足且令人沉醉 

通过融入物理元素（如光和运动）让应用变得更具吸引力。 

## <a name="use-light"></a>使用光线

光线能吸引人的注意力。 它能够营造氛围和场所感觉，是提供照明的实用工具。
        
将光线添加到 UWP 应用：
        
* [突出显示](../style/reveal.md)使用光线来突出显示交互性元素。光线会照亮用户可与之交互的交互性元素，同时显示隐藏的边框。 突出显示在某些控件（如列表视图和网格视图）上自动启用。 你可以通过应用预定义突出显示样式在其他控件上启用它。 

* [显示焦点](../style/reveal-focus.md)利用光线来引起对当前具有输入焦点的元素的注意。  

## <a name="create-a-sense-of-depth"></a>创建深度感

我们生活在三维世界。 通过专门将深度融入 UI，我们将平面的二维界面转变为能创建视觉层次、更丰富地有效呈现信息和概念的界面。 它彻底改变了对于在具有层次的物理环境中物体之间相互关系的呈现方式

将深度添加到 UWP 应用：

* [亚克力](../style/acrylic.md)是一种让用户能够看到内容层次的透明材料，它也为 UI 元素创建层次结构。

* [视差](../motion/parallax.md)通过让前景的物体看起来比背景中的物体移动速度更快，创造出立体感效应。

## <a name="incorporate-motion"></a>融入运动

想一想电影中的运动设计。 运动的无缝过渡让你能够专注于故事，为你带来真实体验。 我们可以将这些感觉融入设计，引导人们在观影过程中能够轻松从一个任务跳转到另一个任务。

将运动添加到 UWP 应用：

* [连接动画](../motion/connected-animation.md)通过在场景之间无缝过渡帮助用户保持画面的连贯。 

## <a name="build-it-with-the-right-material"></a>利用正确的材料生成它

在现实生活中，我们周围的材料是能感觉的，生动的。 它们会弯曲、拉伸、弹跳、碎裂、滑动。 材料的这些特质也在数字环境中体现出来，让人们需要伸出手去触摸设计。

将材料添加到 UWP 应用： 
        
* [亚克力](../style/acrylic.md)是一种让用户能够看到内容层次的透明材料，它也为 UI 元素创建层次结构。 

## <a name="design-toolkits-and-code-samples"></a>设计工具包和代理示例

希望开始使用 Fluent Design 创建你自己的应用？ 适用于 Adobe XD、Adobe Illustrator、Adobe Photoshop、Framer，和 Sketch 的工具包将帮助你开始设计，我们示例将帮助你更快地完成编码。

* 查看[设计工具包和示例页面](/windows/uwp/design/downloads/)

<img src="images/fluentdesign_header.png" alt=" " />








