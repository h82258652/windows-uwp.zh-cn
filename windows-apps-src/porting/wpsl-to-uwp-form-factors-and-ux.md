---
author: stevewhims
description: Windows 应用跨电脑、移动设备以及许多其他类型的设备共享常见的外观。 用户界面、输入和交互模式都非常相似，并且用户在设备之间移动的操作也将是熟悉的体验。
title: 针对外形规格和 UX 进行移植 WindowsPhone silverlight 移植到 UWP
ms.assetid: 96244516-dd2c-494d-ab5a-14b7dcd2edbd
ms.author: stwhi
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 809cf2691a2bc7b7c72d4ba031fa4c6b45335dde
ms.sourcegitcommit: e2fca6c79f31e521ba76f7ecf343cf8f278e6a15
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/15/2018
ms.locfileid: "6970763"
---
#  <a name="porting-windowsphone-silverlight-to-uwp-for-form-factor-and-ux"></a>针对外形规格和 UX 进行移植 WindowsPhone silverlight 移植到 UWP


上一主题是[移植业务和数据层](wpsl-to-uwp-business-and-data.md)。

Windows 应用跨电脑、移动设备以及许多其他类型的设备共享常见的外观。 用户界面、输入和交互模式都非常相似，并且用户在设备之间移动的操作也将是熟悉的体验。 设备，例如物理大小、 默认方向和有效像素分辨率到通用 Windows 平台 (UWP) 应用通过 windows 10 的呈现的方式之间的差异。 好消息是，系统使用智能概念（例如有效像素）为你处理了大量繁琐的工作。

## <a name="different-form-factors-and-user-experience"></a>不同的外形规格和用户体验

不同的设备具有多种纵向和横向分辨率以及不同的纵横比。 UWP 应用缩放时其界面、文本和资源的可视方面将如何显示？ 如何支持触摸以及鼠标和键盘输入？ 在不同大小的设备上支持触摸的应用（使用不同的查看距离）中，如何使控件既可以在不同的像素密度上作为大小适当的触摸目标，*又*可以使其内容在不同的距离上均可读？ 以下部分介绍了你需要了解的内容。

## <a name="what-is-the-size-of-a-screen-really"></a>屏幕的实际大小是多少？

简单地说这是主观的，因为该大小不仅取决于显示器的客观大小，还取决于你与显示器的距离。 实际上，这意味着我们必须设身处地为用户考虑，如果开发人员想要构建出色的应用，则在任何情况下都应该这样做。

客观来讲，屏幕应以英寸为测量单位，并且应采用物理（原始）像素。 知道这两个指标后，你便可以知道一英寸可以容纳的像素数。 这是像素密度，既可称为 DPI（每英寸点数），又可称为 PPI（每英寸像素）。 DPI 的倒数近似于像素的物理大小。 像素密度也称为*分辨率*，尽管该术语通常用于表示像素计数。

由于观看距离增加，所有这些目标指标*看起来*都会更小，而且它们将解析为屏幕的*有效大小*及其*有效分辨率*。 以下设备与眼睛的距离按从近到远的顺序依次为：手机、平板电脑和 PC 显示器、[Surface Hub](http://www.microsoft.com/microsoft-surface-hub) 设备和电视。 若要进行补偿，在客观上设备距离通常应大于观看距离。 当设置 UI 元素的大小时，应采用称为有效像素 (epx) 的单位来设置这些大小。 和 windows 10 将考虑使用 dpi 和设备中的典型观看距离来计算 UI 元素以物理像素为单位，以提供最佳的观看体验的最佳大小。 请参阅[视图/有效像素、观看距离和比例系数](wpsl-to-uwp-porting-xaml-and-ui.md)。

即便如此，我们仍建议你用多种不同的设备来测试你的应用，以便你可以自行确认每种体验。

## <a name="touch-resolution-and-viewing-resolution"></a>触摸分辨率和查看分辨率

提供内容（UI 小组件）的大小需要适合触摸。 因此在可能具有不同像素密度的不同设备上，触摸目标需要或多或少地保留其物理大小。 有效像素同样也能助你“脱困”：它们可在不同设备上缩放（会考虑到像素密度），从而能相应调整常量物理大小以使其非常适合触摸目标。

文本需要采用适当大小进行读取（最好的观看距离是 20 英寸对应 12 点），图像也需要采用适当大小和有效分辨率以适应观看距离。 在不同的设备上，相同的有效像素缩放可使 UI 元素保持适当大小，且保持可读。 文本和其他基于矢量的图形将自动进行缩放，且效果很好。 如果开发人员提供的资源大小单一且较大，则基于光栅（位图）的图形也会自动缩放。 但开发人员最好为每种资源都提供对应的大小范围，以便可以自动加载一种适用于目标设备的比例系数的大小。 有关该内容的详细信息，请参阅[视图/有效像素、观看距离和比例系数](wpsl-to-uwp-porting-xaml-and-ui.md)。

## <a name="layout-and-adaptive-visual-state-manager"></a>布局和自适应视觉状态管理器

为了加深对屏幕大小的理解，我们介绍了相关比例系数。 现在我们来考虑一下应用的布局，以及当额外屏幕空间可用时如何使用它。 请从非常简单的应用（设计用于在屏幕窄小的移动设备上运行）考虑此页面。 在较大的屏幕上，此页面的外观应该如何？

![移植的 Windows Phone 应用商店应用](images/wpsl-to-uwp-case-studies/c01-04-uni-phone-app-ported.png)

将移动版本限制为仅纵向方向，因为这是适用于书籍列表的最佳纵横比；而且我们应对同一页面中的文本执行相同的操作，这最好在移动设备上保留为一列。 但页面在 PC 和平板电脑屏幕的任一方向上都显得很大，因此移动设备的约束在较大的设备上似乎是不必要的限制。

以光学方式缩放此应用来使其看起来像移动版本，只会使其变得更大，而不能充分利用设备及其额外空间，从而不能很好地为用户提供服务。 我们应该考虑显示更多内容，而不是使相同内容显示得更大。 即使在平板手机上，我们也可以多显示几行内容。 我们可以使用额外空间来显示不同的内容（如广告），也可以将该列表框更改为列表视图，并让其将项目包装到多个列中，如果可以这样做，则采用此方式利用空间。 请参阅[列表和网格视图控件指南](https://msdn.microsoft.com/library/windows/apps/mt186889)。

除了诸如列表视图和网格视图等新控件的大多数已建立的布局类型从 WindowsPhone Silverlight 在通用 Windows 平台 (UWP) 中都有等效项。 例如，[**Canvas**](https://msdn.microsoft.com/library/windows/apps/br209267)、[**Grid**](https://msdn.microsoft.com/library/windows/apps/br242704) 和 [**StackPanel**](https://msdn.microsoft.com/library/windows/apps/br209635)。 移植使用这些类型的大部分 UI 应该比较简单，但应始终寻找利用这些布局面板的动态布局功能以自动调整大小并在不同大小的设备上重新布局的方法。

除了内置于系统控件和布局面板的动态布局，我们可以使用一个称为[自适应视觉状态管理器](wpsl-to-uwp-porting-xaml-and-ui.md)的新 windows 10 功能。

## <a name="input-modalities"></a>输入形式

WindowsPhone Silverlight 界面是触摸专用界面。 当然，已移植应用的界面还应该支持触摸，但是你可以选择支持除此之外的其他输入形式，例如鼠标和键盘。 在 UWP 中，将鼠标、笔和触控输入统一称为*指针输入*。 有关详细信息，请参阅[处理指针输入](https://msdn.microsoft.com/library/windows/apps/mt404610)和[键盘交互](https://msdn.microsoft.com/library/windows/apps/mt185607)。

## <a name="maximizing-markup-and-code-re-use"></a>最大程度地重复使用标记和代码

有关将你的 UI 共享到多种外形规格的目标设备的技术，请重新参考[最大程度地重复使用标记和代码](wpsl-to-uwp-porting-to-a-uwp-project.md)列表。

## <a name="more-info-and-design-guidelines"></a>详细信息和设计指南

-   [设计 UWP 应用](http://dev.windows.com/design)
-   [字体指南](https://msdn.microsoft.com/library/windows/apps/hh700394)
-   [不同外观规格规划](https://msdn.microsoft.com/library/windows/apps/dn958435)

## <a name="related-topics"></a>相关主题

* [命名空间和类映射](wpsl-to-uwp-namespace-and-class-mappings.md)

