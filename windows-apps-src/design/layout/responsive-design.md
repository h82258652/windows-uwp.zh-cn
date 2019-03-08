---
Description: 了解响应式设计技术来定制特定设备的应用程序
label: Responsive design techniques
template: detail.hbs
op-migration-status: ready
ms.date: 10/10/2017
ms.topic: article
keywords: windows 10, uwp
localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 9e06131da5d7dd56354e1871867f9956ad13aa2c
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2019
ms.locfileid: "57624812"
---
# <a name="responsive-design-techniques"></a>响应式设计技术

UWP 应用使用有效像素来保证您的 UI 将清晰和所有基于 Windows 的设备上可用。 那么，为什么你还想要针对特定设备系列自定义应用 UI 呢？

- **若要最有效地使用的空间和减少需要导航**

    如果您设计用于显示的屏幕较小的设备上良好的应用，如平板电脑，应用将为大尺寸显示器的 PC 上可用，但可能会浪费一些空间。 当屏幕大小超过某个值时，你可以自定义应用以显示更多内容。 例如，购物应用可能是平板电脑、 上一次显示一个商品类别，但在 PC 或便携式计算机上同时显示多个类别和产品。

    通过在屏幕上放置更多内容，你可以减少用户需要执行的导航次数。

- **若要充分利用设备的功能**

    某些设备可能具有特定的设备功能。 例如，便携式计算机很可能有位置传感器和照相机，而电视可能不具有任何一个。 你的应用可以检测到哪些功能可用，并启用使用它们的功能。

- **若要优化输入**

    通用控件库适用于所有输入类型（触摸、触笔、键盘、鼠标），但你仍然可以通过重新排列 UI 元素来优化某些输入类型。 例如，如果将导航元素放置在屏幕底部，则手机用户将更容易访问它们，但大多数电脑用户希望在屏幕顶部看到导航元素。

在针对特定屏幕宽度优化应用 UI 时，假设你要创建一个响应式设计。 下面是可用于自定义应用 UI 的六个响应式设计技术。

>[!TIP]
> 许多 UWP 控件自动实现这些响应迅速的行为。 若要创建响应式 UI，我们建议签出[UWP 控件](../controls-and-patterns/index.md)。

## <a name="reposition"></a>重新定位

您可以更改的位置和 UI 元素，以充分利用的窗口大小的位置。 在此示例中，较小的窗口垂直排列元素。 当应用转换到更大的窗口中时，元素可以充分利用更广泛的窗口宽度。

![重新定位](images/rsp-design/rspd-reposition2.gif)

在此照片应用的示例设计中，照片应用在较大的屏幕上重新单位其内容。

## <a name="resize"></a>调整大小

可以通过调整的边距和 UI 元素的大小来优化窗口大小。 例如，这可以通过只需扩大内容框增大阅读体验较大屏幕上。

![调整大小设计要素](images/rsp-design/rspd-resize2.gif)

## <a name="reflow"></a>重新排列

通过根据设备和方向更改 UI 元素的排列，你的应用可以以最佳方式呈现内容。 例如，当转到更大的屏幕，可能会有用添加列、 使用较大的容器，或以其他方式生成列表项。

此示例演示如何包含一列的垂直滚动可以重排在更大的屏幕，显示的文本的两个列的小屏幕上的内容。

![重新排列设计元素](images/rsp-design/rspd_reflow.gif)

## <a name="showhide"></a>显示/隐藏

你可以基于屏幕空间显示或隐藏 UI 元素，或在设备支持附加功能、特定情况或首选屏幕方向时显示或隐藏 UI。

![隐藏设计要素](images/rsp-design/rspd-revealhide.gif)

例如，媒体播放器控件降低较小屏幕上的按钮集，然后展开在较大的屏幕上。 在更大的窗口上的媒体播放器可以处理得更多的屏幕比它的功能可以在一个较小的窗口上。

显示或隐藏技术的一部分包括选择何时显示多个元数据。 使用较小的 windows，它是最适合显示最少量的元数据。 使用较大的窗口，可以显示很多元数据。 若要显示或隐藏元数据的一些示例包括：

- 在电子邮件应用中，你可以显示用户的头像。
- 在音乐应用中，你可以显示有关专辑或歌手的详细信息。
- 在视频应用中，你可以显示有关电影或节目的详细信息，例如显示演职人员详细信息。
- 在任一应用中，都可以使各列分解并显示更多详细信息。
- 在任一应用中，都可以获取垂直堆叠的内容并使之以水平方式布局。 当从手机或平板手机转至较大型设备时，堆叠的列表项可以更改为显示多行列表项和多列元数据。

## <a name="replace"></a>替换

此方法允许您切换特定断点的用户界面。 在本示例中，导航窗格和其压缩，瞬时 UI 非常适合较小的屏幕，但在较大的屏幕上选项卡可能是更好的选择。

![替换设计元素](images/rsp-design/rspd-replace.gif)

[NavigationView](../controls-and-patterns/navigationview.md)控件允许用户设置到顶部或左侧的窗格的位置，支持此响应式技术。

## <a name="re-architect"></a>重新构建

可以折叠或拆分应用的体系结构，以更好地适应特定设备。 在此示例中，展开窗口中显示整个母版/详细信息模式。

![重新构建用户界面的示例](images/rsp-design/rspd-rearchitect.gif)

## <a name="related-topics"></a>相关主题

- [屏幕大小和断点](screen-sizes-and-breakpoints-for-responsive-design.md)
- [使用 XAML 的响应式布局](layouts-with-xaml.md)
- [UWP 控件和模式](../controls-and-patterns/index.md)
