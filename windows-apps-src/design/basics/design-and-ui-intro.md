---
author: mijacobs
Description: The universal design features included in every UWP app help you build apps that scale beautifully across a range of devices.
title: 通用 Windows 平台 (UWP) 应用设计（Windows 应用）简介
ms.assetid: 50A5605E-3A91-41DB-800A-9180717C1E86
ms.author: mijacobs
ms.date: 05/05/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 602a0af685e812f5c65f94d07297cac9fc411923
ms.sourcegitcommit: 9354909f9351b9635bee9bb2dc62db60d2d70107
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/16/2018
ms.locfileid: "4694094"
---
# <a name="introduction-to-uwp-app-design"></a>UWP 应用设计简介

![示例照明应用](images/introUWP-header.jpg)

通用 Windows 平台 (UWP) 设计指南可帮助你设计和构建美观、优化的应用。

它不是一份说明性规则的清单 - 它是一个动态文档，旨在适应我们不断演变的 [Fluent Design 系统](../fluent-design-system/index.md)以及应用构建社区的需求。

此引言概述了各项 UWP 应用中包含的通用设计功能，并帮助你构建可在多台设备中完美扩展的用户界面 (UI)。

## <a name="effective-pixels-and-scaling"></a>有效像素和缩放

UWP 应用在所有运行[Windows 10 设备](../devices/index.md)，从电视到平板电脑或电脑。 那么，如何设计在各种设备和屏幕大小外观良好的 UI？

![各种设备上的同一应用](images/universal-image-1.jpg)

UWP 有助于通过自动调整 UI 元素，以便它们清晰可见并易于交互在所有设备和屏幕大小上。

当你的应用在设备上运行时，系统将使用算法使 UI 元素在屏幕上的显示方式规范化。 此缩放算法考虑了观看距离和屏幕密度（每英寸像素），以针对感知大小（而不是物理大小）进行优化。 该缩放算法确保用户可从 10 英尺远处识别 Surface Hub 上高 24 像素的字体，正如从几英寸远处识别 5 英寸手机上高 24 像素的字体。

![不同设备的观看距离](images/scaling-chart.png)

基于缩放系统的工作原理，在设计 UWP 应用时，要以有效像素而不是实际物理像素为单位进行设计。 有效像素 (epx) 是一个虚拟度量单位，用于表示布局尺寸和间距（独立于屏幕密度）。 （在我们的指南中，epx、ep 和 px 可以互换使用。）

在设计时，你可以忽略像素密度和实际屏幕分辨率。 而是针对同一大小级别的有效分辨率（以有效像素为单位的分辨率）进行设计（有关详细信息，请参阅 [屏幕大小和断点文章](../layout/screen-sizes-and-breakpoints-for-responsive-design.md)）。

> [!TIP]
> 在使用图像编辑程序创建屏幕原型时，将 DPI 设置为 72，并将图像尺寸设置为目标大小级别的有效分辨率。 要获得大小类和有效分辨率的列表，请参阅[屏幕大小和断点文章](../layout/screen-sizes-and-breakpoints-for-responsive-design.md)。

### <a name="multiples-of-four"></a>四的倍数

:::row:::
    :::column span:::
        The sizes, margins, and positions of UI elements should always be in **multiples of 4 epx** in your UWP apps.

        UWP scales across a range of devices with scaling plateaus of 100%, 125%, 150%, 175%, 200%, 225%, 250%, 300%, 350%, and 400%. The base unit is 4 because it's the only integer that can be scaled by non-whole numbers (e.g. 4*1.5 = 6). Using multiples of four aligns all UI elements with whole pixels and ensures UI elements have crisp, sharp edges. (Note that text doesn't have this requirement; text can have any size and position.)
    :::column-end:::
    :::column:::
        ![grid](images/4epx.svg)
    :::column-end:::
:::row-end:::

## <a name="layout"></a>布局

由于 UWP 应用会自动扩展到所有设备，因此为任意设备设计 UWP 应用都遵循相同的结构。 我们从 UWP 应用的 UI 开始。

### <a name="windows-frames-and-pages"></a>窗口、框架和页面

:::row:::
    :::column:::
        When a UWP app is launched on any Windows 10 device, it launches in a [Window](/uwp/api/Windows.UI.Xaml.Controls.Window) with a [Frame](/uwp/api/Windows.UI.Xaml.Controls.Frame), which can navigate between [Page](/uwp/api/Windows.UI.Xaml.Controls.Page) instances.
    :::column-end:::
    :::column:::
        ![Frame](images/frame.svg)
    :::column-end:::
:::row-end:::

:::row:::
    :::column:::
        You can think of your app's UI as a collection of pages. It's up to you to decide what should go on each page, and the relationships between pages.

        To learn how you can organize your pages, see [Navigation basics](navigation-basics.md).
    :::column-end:::
    :::column:::
        ![Frame](images/collection-pages.svg)
    :::column-end:::
:::row-end:::

### <a name="page-layout"></a>页面布局

应如何显示页面？ 大多数页面遵循一种公用结构来提供一致性，以便用户能够轻松地在应用的页面之间和页面内导航。 页面通常包含三种类型的 UI 元素：

- [导航](navigation-basics.md)元素帮助用户选择他们希望显示的内容。
- [命令](commanding-basics.md)元素启动操作，例如处理、保存或共享内容。
- [内容](content-basics.md)元素显示应用内容。

![常见布局模式](../layout/images/page-components.svg)

要详细了解如何实现常见的 UWP 应用模式，请参阅[页面布局](../layout/page-layout.md)文章。

也可以在 Visual Studio 中使用 [Windows Template Studio](https://github.com/Microsoft/WindowsTemplateStudio/tree/master) 为应用设计布局。

## <a name="controls"></a>控件

UWP 的设计平台提供了一系列保证在所有支持 Windows 的设备上都能正常工作的常用控件，它们遵循 [Fluent Design 系统](../fluent-design-system/index.md)原则。 这些控件包括从简单控件（如按钮和文本元素）到复杂控件（可从一组数据和一个模板生成列表）的一切控件。

![UWP 控件](../style/images/color/windows-controls.svg)

有关 UWP 控件以及可从中创建的模式的完整列表，请参阅[控件和模式部分](../controls-and-patterns/index.md)部分。

## <a name="style"></a>样式

常用控件自动反映系统主题和主题色，使用所有输入类型并扩展到所有设备。 它们通过这种方式体现 Fluent Design 系统的特点 - 自适应、共鸣和美观。 常用控件的默认样式使用了灯光、运动和深度，因此一旦使用它们，就等于将 Fluent Design 系统集成到了你的应用之中。

常用控件是可高度自定义的，你可以更改控件的前景色或完全自定义其外观。 要覆盖控件中的默认样式，请使用[轻型样式设置](../controls-and-patterns/xaml-styles.md#lightweight-styling)或在 XAML 中创建[自定义控件](../controls-and-patterns/control-templates.md)。

![主题色 gif](images/intro-style.gif)

## <a name="shell"></a>Shell

:::row:::
    :::column:::
        Your UWP app will interact with the broader Windows experience with tiles and notifications in the Windows [Shell](../shell/tiles-and-notifications/creating-tiles.md).

        Tiles are displayed in the Start menu and when your app launches, and they provide a glimpse of what's going on in your app. Their power comes from the content behind them, and the intelligence and craft with which they're offered up.

        UWP apps have four tile sizes (small, medium, wide, and large) that can be customized with the app's icon and identity. For guidance on designing tiles for your UWP app, see [Guidelines for tile and icon assets](../shell/tiles-and-notifications/app-assets.md).
    :::column-end:::
    :::column:::
        ![tiles on start menu](images/shell.svg)
    :::column-end:::
:::row-end:::

## <a name="inputs"></a>输入

:::row:::
    :::column:::
        UWP apps rely on smart interactions. You can design around a click interaction without having to know or define whether the click comes from a mouse, a stylus, or a tap of a finger. However, you can also design your apps for [specific input modes](../input/input-primer.md).
    :::column-end:::
    :::column:::
        ![inputs](images/inputs.svg)
    :::column-end:::
:::row-end:::

## <a name="devices"></a>设备

![设备](../layout/images/size-classes.svg)

同样，虽然 UWP 会自动将应用扩展到不同的设备，但你也可以[针对特定设备优化 UWP 应用](../devices/index.md)。

## <a name="usability"></a>可用性

<img src="https://img-prod-cms-rt-microsoft-com.akamaized.net/cms/api/am/imageFileData/REYaAb?ver=727c">

最后但并非最不重要的是，可用性是对所有用户开放你的应用体验。 每个人都可从真正非独占的用户体验中受益 - 请参阅 [UWP 应用中的可用性](../usability/index.md)以了解如何使你的应用可供每个人轻松使用。

如果是面向全球用户设计，你可能需要了解[全球化和本地化](../globalizing/globalizing-portal.md)。

你还可能需要考虑面向在视觉、听觉和行动方面受限的用户的[辅助功能](../accessibility/accessibility-overview.md)。 如果辅助功能从一开始便内置于设计中，则[使你的应用可供访问](../accessibility/accessibility-in-the-store.md)应只需花费极少的额外时间和精力。

## <a name="tools-and-design-toolkits"></a>工具和设计工具包

现在你已了解基本的设计功能，那么如何开始设计 UWP 应用呢？

我们提供了各种工具来帮助你实施设计过程：

- 有关 XD、Illustrator、Photoshop、Framer 和 Sketch 工具包以及其他设计工具和字体下载的信息，请参阅我们的[设计工具包页面](../downloads/index.md)。

- 要设置计算机来为 UWP 应用编写代码，请参阅[入门 &gt; 准备工作](../../get-started/get-set-up.md)文章。

- 有关如何实现 UWP UI 的灵感，请参阅端到端[示例 UWP 应用](https://developer.microsoft.com/windows/samples)。

## <a name="video-summary"></a>视频摘要

> [!VIDEO https://channel9.msdn.com/Blogs/One-Dev-Minute/Designing-Universal-Windows-Platform-apps/player]

## <a name="next-fluent-design-system"></a>下一步：Fluent Design 系统

如果要了解 Fluent Design（Microsoft 提供的设计系统）背后的原则，并查看可融入 UWP 应用中的更多功能，请继续参阅 [Fluent Design 系统](../fluent-design-system/index.md)。

## <a name="related-articles"></a>相关文章

- [UWP 应用是什么？](../../get-started/universal-application-platform-guide.md)
- [Fluent Design 系统](../fluent-design-system/index.md)
- [XAML 平台概述](../../xaml-platform/index.md)
