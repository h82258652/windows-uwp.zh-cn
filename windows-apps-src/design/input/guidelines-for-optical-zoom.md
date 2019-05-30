---
Description: 本主题介绍 Windows 缩放和调整大小元素，并提供在你的应用中使用这些交互机制时的用户体验指南。
title: 视觉缩放和调整大小指南
ms.assetid: 51a0007c-8a5d-4c44-ac9f-bbbf092b8a00
label: Optical zoom and resizing
template: detail.hbs
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: b63c9191489ecae54b17cb75b8aa1af32f09fcb8
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/29/2019
ms.locfileid: "66363602"
---
# <a name="optical-zoom-and-resizing"></a>视觉缩放和调整大小



本文介绍 Windows 缩放和调整大小元素，并提供在你的应用中使用这些交互机制时的用户体验指南。

> **重要的 API**：[**Windows.UI.Input**](https://docs.microsoft.com/uwp/api/Windows.UI.Input)， [**输入 (XAML)** ](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Input)

使用视觉缩放，用户可以放大内容区域中内容的视图（在内容区域自身上执行），而使用调整大小，用户则可以更改一个或多个对象的相对大小，而不更改内容区域的视图（在内容区域中的对象上执行）。

视觉缩放和调整大小交互都是通过收缩和拉伸手势（手指分开即可放大，手指聚拢即可缩小）进行，或者通过在滚动鼠标滚轮的同时按下 Ctrl 键，或者按下 Ctrl 键（如果没有数字键盘，则使用 Shift 键）并按加号 (+) 或减号 (-) 键。

下图演示了调整大小和视觉缩放之间的区别。

**视觉缩放**:用户选择一个区域，然后将其缩放到整个区域。

![将手指并拢可放大内容区域，将手指分开可缩小](images/areazoom.png)

**重设大小**:用户选择一个区域内的对象，并调整该对象的大小。

![将手指并拢可缩小对象，将手指分开可放大](images/objectresize.png)

**请注意**  光学缩放不应与混淆[语义缩放](../controls-and-patterns/semantic-zoom.md)。 尽管两种交互使用相同的手势，但语义式缩放是指在单个视图中组织的内容的表示和导航（如计算机的文件夹结构、文档库或相册）。

 

## <a name="dos-and-donts"></a>应做事项和禁止事项


对支持调整大小或视觉缩放的应用使用以下指南：

-   如果定义了最大和最小大小限制或边界，则使用视觉反馈演示用户何时达到或超过这些边界。
-   可以使用吸附点通过提供内容中要停止操作的逻辑点来影响缩放和重新调整行为，并确保内容的特定子集显示在视口中。 为常用的缩放级别或逻辑视图提供吸附点，以便于用户选择这些级别。 例如，照片应用可能会提供 100% 级别的吸附点，而对于地图应用，吸附点对于市、州/省以及国家/地区视图可能非常有用。

    用户通过对齐点虽然不够精确，但仍能实现其目标。 如果你使用 XAML，请参阅 [**ScrollViewer**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ScrollViewer) 的吸附点属性。 对于 JavaScript 和 HTML，请使用 [ **-ms-content-zoom-snap-points**](https://msdn.microsoft.com/library/hh771895)。

    吸附点有两种类型：

    -   邻近 - 抬起接触之后，如果惯性停止在吸附点的距离阈值之内，则选择该吸附点。 邻近吸附点仍允许缩放或重新调整终止于吸附点之间。
    -   强制 - 所选择的吸附点紧跟抬起接触之前划过的最后一个吸附点之前或之后（取决于手势的方向和速度）。 操作必须终止于强制吸附点上。
-   使用惯性物理原则。 例如：
    -   减速度：当用户停止收缩或拉伸时发生。 这类似于滑动到一个斜面上的挡块。
    -   跳出：边界传递或大小限制时，会发生轻微反弹回效果。
-   根据[目标指南](guidelines-for-targeting.md)隔开控件。
-   提供用来进行受限大小调整的缩放手柄。 如果未指定句柄，则默认为等量或按比例调整大小。
-   请勿使用缩放导航 UI 或显示应用中的其他控件，而应使用平移区域。 有关平移的详细信息，请参阅[平移指南](guidelines-for-panning.md)。
-   不要将可调整大小的对象放置在可调整大小的内容区域中。 这种情况的例外情况包括：
    -   可调整大小的项可以显示在可调整大小的画布或画板上的绘图应用程序中。
    -   具有嵌入对象（如地图）的网页。

    **请注意**  在所有情况下，内容区域调整大小，除非所有触摸点都都可调整大小的对象中。

     

## <a name="related-articles"></a>相关文章


**示例**
* [基本的输入的示例](https://go.microsoft.com/fwlink/p/?LinkID=620302)
* [低延迟的输入的示例](https://go.microsoft.com/fwlink/p/?LinkID=620304)
* [用户交互模式示例](https://go.microsoft.com/fwlink/p/?LinkID=619894)
* [焦点视觉对象示例](https://go.microsoft.com/fwlink/p/?LinkID=619895)

**存档示例**
* [输入：XAML 用户输入的事件示例](https://go.microsoft.com/fwlink/p/?linkid=226855)
* [输入：设备功能示例](https://go.microsoft.com/fwlink/p/?linkid=231530)
* [输入：触控命中测试示例](https://go.microsoft.com/fwlink/p/?linkid=231590)
* [XAML 滚动、 平移和缩放示例](https://go.microsoft.com/fwlink/p/?linkid=251717)
* [输入：简化的墨迹示例](https://go.microsoft.com/fwlink/p/?linkid=246570)
* [输入：Windows 8 手势示例](https://go.microsoft.com/fwlink/p/?LinkId=264995)
* [输入：操作和手势 (C++) 示例](https://go.microsoft.com/fwlink/p/?linkid=231605)
* [DirectX 触摸输入的示例](https://go.microsoft.com/fwlink/p/?LinkID=231627)
 

 




