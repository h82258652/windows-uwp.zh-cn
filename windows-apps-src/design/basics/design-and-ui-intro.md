---
author: serenaz
Description: An overview of the universal design features that are included in every UWP app to help you build apps that scale beautifully across a range of devices.
title: 通用 Windows 平台 (UWP) 应用设计（Windows 应用）简介
ms.assetid: 50A5605E-3A91-41DB-800A-9180717C1E86
ms.author: sezhen
ms.date: 12/13/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: d0527866777c7b5dbbc10697bb313d664f4555fa
ms.sourcegitcommit: 91511d2d1dc8ab74b566aaeab3ef2139e7ed4945
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/30/2018
ms.locfileid: "1817275"
---
#  <a name="introduction-to-uwp-app-design"></a>UWP 应用设计简介

通用 Windows 平台 (UWP) 设计指南可帮助你设计和构建美观、优化的应用。

它不是一份说明性规则的清单 - 它是一个动态文档，旨在适应我们不断演变的 [Fluent Design 系统](../fluent-design-system/index.md)以及应用构建社区的需求。

此引言概述了各项 UWP 应用中包含的通用设计功能，并帮助你构建可在多台设备中完美扩展的用户界面 (UI)。

## <a name="video-summary"></a>视频摘要

> [!VIDEO https://channel9.msdn.com/Blogs/One-Dev-Minute/Designing-Universal-Windows-Platform-apps/player]

## <a name="effective-pixels-and-scaling"></a>有效像素和缩放

UWP 应用会自动调整控件、字体和其他 UI 元素的大小，以使它们在[所有支持 UWP 应用的设备](../devices/index.md)上清晰可见并易于交互。

当你的应用在设备上运行时，系统将使用算法来使 UI 元素在屏幕上的显示方式规范化。 此缩放算法考虑了观看距离和屏幕密度（每英寸像素），以针对感知大小（而不是物理大小）进行优化。 该缩放算法确保用户可从 10 英尺远处识别 Surface Hub 上高 24 像素的字体，正如从几英寸远处识别 5 英寸手机上高 24 像素的字体。

![不同设备的观看距离](images/1910808-hig-uap-toolkit-03.png)

基于缩放系统的工作原理，在设计 UWP 应用时，要以有效像素而不是实际物理像素为单位进行设计。 有效像素 (epx) 是一个虚拟度量单位，用于表示布局尺寸和间距（独立于屏幕密度）。 （在我们的指南中，epx、ep 和 px 可以互换使用。）

在设计时，你可以忽略像素密度和实际屏幕分辨率。 而是针对同一大小级别的有效分辨率（以有效像素为单位的分辨率）进行设计（有关详细信息，请参阅 [屏幕大小和断点文章](../layout/screen-sizes-and-breakpoints-for-responsive-design.md)）。

> [!TIP]
> 在使用图像编辑程序创建屏幕原型时，将 DPI 设置为 72，并将图像尺寸设置为目标大小级别的有效分辨率。 要获得大小类和有效分辨率的列表，请参阅[屏幕大小和断点文章](../layout/screen-sizes-and-breakpoints-for-responsive-design.md)。


## <a name="layout"></a>布局

### <a name="margins-spacing-and-positioning"></a>边距、间距和位置 
![网格](images/4epx.png)

当系统缩放应用的 UI 时，会按 4 的倍数进行缩放。

因此，**UI 元素的大小、边距和位置应始终为 4 epx 的倍数**。 这将通过与整个像素对齐来生成最佳呈现。 它还确保 UI 元素具有清晰的锐边。 

请注意，文本不会有此要求；文本可以是任何大小和位置。 有关将文本与其他 UI 元素对齐的指导信息，请参阅 [UWP 版式指南](../style/typography.md)。

![在网格上缩放](images/epx-4pixelgood.png)

### <a name="layout-patterns"></a>布局模式

![常见布局模式](images/page-components.png)

用户界面由三类元素构成： 
1. **导航元素**帮助用户选择他们希望显示的内容。 请参阅[导航基础知识](navigation-basics.md)。
2. **命令元素**启动操作，例如处理、保存或共享内容。 请参阅[命令基础知识](commanding-basics.md)。
3. **内容元素**显示应用内容。 请参阅[内容基础知识](content-basics.md)。

### <a name="adaptive-behavior"></a>自适应行为
![手机和桌面设备的自适应行为](../controls-and-patterns/images/patterns_masterdetail.png)

虽然应用 UI 在所有 Windows 驱动的设备上都清晰可见且可用，但你可能想针对特定的设备和屏幕大小来自定义应用 UI。 有关更多的具体指导，请参阅[屏幕大小和断点](../layout/screen-sizes-and-breakpoints-for-responsive-design.md)和[响应式设计技术](../layout/responsive-design.md)。

## <a name="type"></a>类型

默认情况下，UWP 应用使用 **Segoe UI** 字体，并且 UWP 类型渐变包含 7 类版式，以便在所有显示大小中实现最有效的方法。 

![字体渐变](images/type-ramp.png)

有关类型渐变的详细信息，请参阅 [UWP 版式指南](../style/typography.md)。 要了解如何在应用中使用不同级别的 UWP 类型渐变，请参阅[主题资源](../controls-and-patterns/xaml-theme-resources.md#the-xaml-type-ramp)。

## <a name="color"></a>颜色

颜色提供了向用户传达信息的直观方式。 它可用来表示交互、向用户操作提供反馈、传达状态信息并使界面具有视觉连续性。 

Windows 10 提供了一个包含 48 种颜色的共享通用调色板，可在 Shell 和 UWP 应用中使用。 

![通用 Windows 调色板](images/colors.png)

系统自动将颜色应用于带系统主题色的 UWP 应用。 当用户在“设置”中选择调色板中的主题色时，颜色将作为其系统主题的一部分显示。 根据用户首选项，还可以在“开始”菜单、“开始”菜单磁贴、任务栏和标题栏上显示系统主题色。 

![在设置中选择主题色](images/selectcolor.png)

在 UWP 应用中，常见控件中的超链接和所选状态将反映系统主题色。

![控件上的系统主题色](images/accentcolor.png)

UWP 应用可通过使用[轻型样式设置](../controls-and-patterns/xaml-styles.md#lightweight-styling)或创建[自定义控件](../controls-and-patterns/control-templates.md)来覆盖控件中显示的系统主题色。

有关在 UWP 应用中使用颜色的其他指南，请参阅[颜色](../style/color.md)一文。

### <a name="themes"></a>主题

用户可选择浅色主题、深色主题或高对比度主题。 你可以根据用户的主题首选项选择应用的外观，也可以选择退出。

浅色主题最适合生产力任务和阅读。

深色主题更能够看到媒体中心式应用和包含丰富的视频或图像的方案的对比。 在这些方案中，主要任务更有可能是看电影而不是阅读，并且会在光线较暗的环境条件下进行。

![浅色和深色主题中的计算器显示](images/light-dark.png)

高对比度主题使用一块小型的对比色调色板，使界面看起来更加适宜。
![在浅色主题和高对比度黑色主题下显示的计算器。](../accessibility/images/high-contrast-calculators.png)


有关在应用中使用主题和 UWP 颜色渐变的更多信息，请参阅[主题资源](../controls-and-patterns/xaml-theme-resources.md)。

## <a name="icons"></a>图标
![图标](images/icons.png)

图标是一种已彻底融入我们的日常生活的视觉语言。 它们可让我们以一种在视觉上引人注目的方式表达概念和行动，节省屏幕空间，并作为一种导航数字生活的方式。 

所有 UWP 应用都能访问 [Segoe MDL2 字体](../style/segoe-ui-symbol-font.md)的图标。 虽然这些图标依赖于每个人都熟悉并能轻松识别的已建立的形式，但它们也已进行现代化，让人感觉它们是用一只手画出来的。

如果你想创建你自己的图标，请参阅 [UWP 应用的图标](../style/icons.md)。

## <a name="tiles"></a>磁贴
![“开始”菜单上的磁贴](images/tiles.png)

磁贴显示在“开始”菜单上，它们简要描述了应用中发生的情况。 其功能来自背后的内容及其提供的智能和工艺。 

UWP 应用具有四种磁贴大小（小、中、宽和大），可使用应用的图标和标识来自定义这几种大小。 有关为 UWP 应用设计磁贴的指南，请参阅[磁贴和图标资产指南](../shell/tiles-and-notifications/app-assets.md)。

## <a name="controls"></a>控件
![按钮控件](images/1910808-hig-uap-toolkit-01.png)

UWP 提供一组常用控件，可保证在所有支持 Windows 的设备上良好工作。 这些控件包括从简单控件（如按钮和文本元素）到复杂控件（可从一组数据和一个模板生成列表）的一切控件。

UWP 控件自动反映系统主题和主题色，使用所有输入类型并扩展到所有设备。 它们也是可高度自定义的，您可以更改控件的前景色或完全自定义其外观。 

有关 UWP 控件以及可从中创建的模式的完整列表，请参阅[控件和模式部分](../controls-and-patterns/index.md)部分。

## <a name="input"></a>输入
![输入](../input/images/input-interactions/icons-inputdevices03.png)

UWP 应用依赖于智能交互。 你可以围绕单击交互进行设计，而无需知道或定义该单击是来自鼠标、触笔还是手指点击。 不过，你也可以针对[特定输入模式和设备](../input/input-primer.md)设计应用。

## <a name="accessibility"></a>辅助功能
![非独占设计人物](images/inclusive.png)

最后但并非最不重要的是，辅助功能是对所有用户开放你的应用体验。 它与每个人都有关，而不只是与残障人士有关。 每个人都可从真正非独占的用户体验中受益 - 请参阅 [UWP 应用中的可用性](../usability/index.md)以了解如何使你的应用可供每个人轻松使用。 你还可能需要考虑面向在视觉、听觉和行动方面受限的用户的[辅助功能](../accessibility/accessibility-overview.md)。 

如果辅助功能从一开始便内置于设计中，则[使你的应用可供访问](../accessibility/accessibility-in-the-store.md)应只需花费极少的额外时间和精力。

## <a name="tools-and-design-toolkits"></a>工具和设计工具包
现在你已了解基本的设计功能，那么如何开始设计 UWP 应用呢？

我们提供了各种工具来帮助你实施设计过程：

* 有关 XD、Illustrator、Photoshop、Framer 和 Sketch 工具包以及其他设计工具和字体下载的信息，请参阅我们的[设计工具包页面](../downloads/index.md)。 

* 要设置计算机来为 UWP 应用编写代码，请参阅[入门 &gt; 准备工作](../../get-started/get-set-up.md)文章。 

* 有关如何实现 UWP UI 的灵感，请参阅端到端[示例 UWP 应用](https://developer.microsoft.com/windows/samples)。

## <a name="next-fluent-design-system"></a>下一步：Fluent Design 系统
如果你想了解 Fluent Design（Microsoft 提供的设计系统）背后的原则，并查看可融入 UWP 应用中的更多功能，请继续参阅 [Fluent Design 系统](../fluent-design-system/index.md)。