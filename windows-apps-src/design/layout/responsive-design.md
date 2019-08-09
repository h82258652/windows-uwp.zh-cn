---
Description: 了解针对特定设备定制应用程序的响应式设计技巧
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
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/09/2019
ms.locfileid: "68867413"
---
# <a name="responsive-design-techniques"></a>响应式设计技术

UWP 应用使用有效像素来保证你的 UI 在所有支持 Windows 的设备上都能识别和使用。 那么，为什么你还想要针对特定设备系列自定义应用 UI 呢？

- **充分利用空间, 并减少导航的需要**

    如果你将应用程序设计为在具有小屏幕的设备 (如平板电脑) 上看起来良好, 则该应用程序将在具有更大显示的 PC 上使用, 但可能会有一些浪费的空间。 当屏幕大小超过某个值时，你可以自定义应用以显示更多内容。 例如, 一个购物应用可能会一次在平板电脑上显示一个商品类别, 但会在电脑或笔记本电脑上同时显示多个类别和产品。

    通过在屏幕上放置更多内容，你可以减少用户需要执行的导航次数。

- **利用设备功能**

    某些设备可能具有特定的设备功能。 例如, 便携式计算机可能有位置传感器和照相机, 而电视可能没有这两者。 你的应用可以检测到哪些功能可用，并启用使用它们的功能。

- **优化输入**

    通用控件库适用于所有输入类型（触摸、触笔、键盘、鼠标），但你仍然可以通过重新排列 UI 元素来优化某些输入类型。 例如，如果将导航元素放置在屏幕底部，则手机用户将更容易访问它们，但大多数电脑用户希望在屏幕顶部看到导航元素。

在针对特定屏幕宽度优化应用 UI 时，假设你要创建一个响应式设计。 下面是可用于自定义应用 UI 的六个响应式设计技术。

>[!TIP]
> 许多 UWP 控件会自动实现这些响应行为。 若要创建响应式 UI, 建议签出[UWP 控件](../controls-and-patterns/index.md)。

## <a name="reposition"></a>重新定位

您可以更改 UI 元素的位置和位置, 以充分利用窗口大小。 在此示例中, 较小的窗口垂直堆栈元素。 当应用程序转换为更大的窗口时, 元素可以利用更宽的窗口宽度。

![重新定位](images/rsp-design/rspd-reposition2.gif)

在此照片应用的示例设计中，照片应用在较大的屏幕上重新单位其内容。

## <a name="resize"></a>调整大小

可以通过调整 UI 元素的边距和大小优化窗口大小。 例如, 这可以通过只增加内容框架, 在较大屏幕上增加阅读体验。

![调整大小设计要素](images/rsp-design/rspd-resize2.gif)

## <a name="reflow"></a>重新排列

通过根据设备和方向更改 UI 元素的排列，你的应用可以以最佳方式呈现内容。 例如, 在转到较大屏幕时, 添加列、使用较大容器或以不同方式生成列表项可能有意义。

此示例演示如何在较小屏幕上垂直滚动的单个列滚动内容, 以显示两列文本重排。

![重新排列设计元素](images/rsp-design/rspd_reflow.gif)

## <a name="showhide"></a>显示/隐藏

你可以基于屏幕空间显示或隐藏 UI 元素，或在设备支持附加功能、特定情况或首选屏幕方向时显示或隐藏 UI。

![隐藏设计要素](images/rsp-design/rspd-revealhide.gif)

例如, media player 控件减少了在较小屏幕上设置的按钮, 并在较大屏幕上展开。 较大窗口上的 media player 可以处理比小型窗口更多的屏幕功能。

显示或隐藏技术的一部分包括选择何时显示多个元数据。 对于较小的窗口, 最好显示最少量的元数据。 对于较大的窗口, 可以呈现大量的元数据。 显示或隐藏元数据的一些示例包括:

- 在电子邮件应用中，你可以显示用户的头像。
- 在音乐应用中，你可以显示有关专辑或歌手的详细信息。
- 在视频应用中，你可以显示有关电影或节目的详细信息，例如显示演职人员详细信息。
- 在任一应用中，都可以使各列分解并显示更多详细信息。
- 在任一应用中，都可以获取垂直堆叠的内容并使之以水平方式布局。 当从手机或平板手机转至较大型设备时，堆叠的列表项可以更改为显示多行列表项和多列元数据。

## <a name="replace"></a>替换

利用此方法, 您可以切换特定断点的用户界面。 在此示例中, 导航窗格及其简洁的暂时性 UI 适用于较小的屏幕, 但在较大屏幕上, 选项卡可能是更好的选择。

![替换设计元素](images/rsp-design/rspd-replace.gif)

通过让用户将窗格位置设置为顶部或左侧, [NavigationView](../controls-and-patterns/navigationview.md)控件支持这种响应式技术。

## <a name="re-architect"></a>重新构建

可以折叠或拆分应用的体系结构，以更好地适应特定设备。 在此示例中, 展开窗口将显示整个主/详细信息模式。

![重新构建用户界面的示例](images/rsp-design/rspd-rearchitect.gif)

## <a name="related-topics"></a>相关主题

- [屏幕大小和断点](screen-sizes-and-breakpoints-for-responsive-design.md)
- [采用 XAML 的响应式布局](layouts-with-xaml.md)
- [UWP 控件和模式](../controls-and-patterns/index.md)
