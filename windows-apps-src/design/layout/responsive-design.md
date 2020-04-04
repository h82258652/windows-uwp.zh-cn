---
Description: 了解为特定设备定制应用的响应式设计技术
title: 响应式设计技术
template: detail.hbs
op-migration-status: ready
ms.date: 10/10/2017
ms.topic: article
keywords: windows 10, uwp
localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: f688522ec8970b1e3570610663f5a3e6cae65793
ms.sourcegitcommit: 789bfe3756c5c47f7324b96f482af636d12c0ed3
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/09/2019
ms.locfileid: "68867413"
---
# <a name="responsive-design-techniques"></a>响应式设计技术

UWP 应用使用有效像素，保证 UI 清晰可见并可在所有支持 Windows 的设备上使用。 那么，为什么你还想要针对特定设备系列自定义应用 UI 呢？

- **最有效地利用空间并减少导航需要**

    如果你设计了一个在小屏幕设备（如平板电脑）上看起来十分美观的应用，则该应用可在显示屏大得多的电脑上使用，但可能会浪费一些空间。 当屏幕大小超过某个值时，你可以自定义应用以显示更多内容。 例如，某个购物应用在平板电脑上可能一次显示一个商品类别，但在电脑或笔记本电脑上可以同时显示多个类别和产品。

    通过在屏幕上放置更多内容，你可以减少用户需要执行的导航次数。

- **利用设备的功能**

    某些设备可能具有特定的设备功能。 例如，平板电脑可能有位置传感器和相机，而电视可能两者皆无。 你的应用可以检测到哪些功能可用，并启用使用它们的功能。

- **输入优化**

    通用控件库适用于所有输入类型（触摸、触笔、键盘、鼠标），但你仍然可以通过重新排列 UI 元素来优化某些输入类型。 例如，如果将导航元素放置在屏幕底部，则手机用户将更容易访问它们，但大多数电脑用户希望在屏幕顶部看到导航元素。

在针对特定屏幕宽度优化应用 UI 时，假设你要创建一个响应式设计。 下面是可用于自定义应用 UI 的六个响应式设计技术。

>[!TIP]
> 许多 UWP 控件会自动实现这些响应行为。 若要创建响应式 UI，建议查看 [UWP 控件](../controls-and-patterns/index.md)一文。

## <a name="reposition"></a>重新定位

可以更改 UI 元素的位置和放置方式，充分利用窗口大小。 在以下示例中，较小的窗口以垂直方式堆叠元素。 当应用转换为较大的窗口时，元素可以利用的窗口宽度更宽。

![重新定位](images/rsp-design/rspd-reposition2.gif)

在此照片应用的示例设计中，照片应用在较大的屏幕上重新单位其内容。

## <a name="resize"></a>调整大小

可以调整 UI 元素的边距和大小，针对窗口大小进行优化。 例如，只需增大内容框架即可改善较大屏幕上的阅读体验。

![调整设计元素大小](images/rsp-design/rspd-resize2.gif)

## <a name="reflow"></a>重新排列

通过根据设备和方向更改 UI 元素的排列，你的应用可以以最佳方式呈现内容。 例如，在改用更大的屏幕时，可以添加列、使用更大的容器，或以不同的方式生成列表项。

以下示例显示如何在较大屏幕上重新排列较小屏幕上的单列垂直滚动内容，以显示两列文本。

![重新排列设计元素](images/rsp-design/rspd_reflow.gif)

## <a name="showhide"></a>显示/隐藏

可以基于屏幕空间显示或隐藏 UI 元素，或在设备支持附加功能、特定情况或首选屏幕方向时显示或隐藏 UI。

![隐藏设计元素](images/rsp-design/rspd-revealhide.gif)

例如，媒体播放器控件的按钮集在较小屏幕上折叠，在较大屏幕上展开。 例如，与较小窗口相比，较大窗口中的媒体播放器可以处理的屏幕上的功能要多得多。

显示或隐藏技术的一部分包括选择何时显示多个元数据。 使用较小窗口时，最好是显示最少量的元数据。 使用较大窗口时，可以显示大量元数据。 以下示例介绍了何时显示或隐藏元数据：

- 在电子邮件应用中，你可以显示用户的头像。
- 在音乐应用中，你可以显示有关专辑或歌手的详细信息。
- 在视频应用中，你可以显示有关电影或节目的详细信息，例如显示演职人员详细信息。
- 在任一应用中，都可以使各列分解并显示更多详细信息。
- 在任一应用中，都可以获取垂直堆叠的内容并使之以水平方式布局。 当从手机或平板手机转至较大型设备时，堆叠的列表项可以更改为显示多行列表项和多列元数据。

## <a name="replace"></a>替换

使用这种技术，可以针对特定断点来切换用户界面。 在此示例中，瞬态 UI（导航窗格及其精简版）适用于较小的屏幕，但在较大屏幕上，最好选择使用选项卡。

![替换设计元素](images/rsp-design/rspd-replace.gif)

[NavigationView](../controls-and-patterns/navigationview.md) 控件支持这种响应式技术，允许用户将窗格位置设置为顶部或左侧。

## <a name="re-architect"></a>重新构建

可以折叠或拆分应用的体系结构，以更好地适应特定设备。 在此示例中，展开窗口时会显示整个大纲/细节模式。

![重新构建用户界面的示例](images/rsp-design/rspd-rearchitect.gif)

## <a name="related-topics"></a>相关主题

- [屏幕大小和断点](screen-sizes-and-breakpoints-for-responsive-design.md)
- [采用 XAML 的响应式布局](layouts-with-xaml.md)
- [UWP 控件和模式](../controls-and-patterns/index.md)
