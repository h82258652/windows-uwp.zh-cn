---
title: 适用于 UWP 应用的页面布局
description: 在设计您的应用程序，首先要考虑是布局结构。 本文介绍了基本的页面布局，包括哪些 UI 元素，你将需要并且它们应发送到何处的页上的通用结构。 在 UWP 应用中，每个页通常具有导航、 命令和内容元素。
ms.date: 03/19/2018
ms.topic: article
keywords: windows 10, uwp
localizationpriority: medium
ms.openlocfilehash: edda9948e70b1757ddb46fae45a579bb2fdb8de1
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2019
ms.locfileid: "57633712"
---
# <a name="page-layout"></a>页面布局

在 UWP 应用中，每个[**页面**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Page)通常包含导航、命令栏和内容元素。 

应用可以包含多个页面：当用户启动 UWP 应用时，应用程序代码会创建要放在应用[**窗口**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Frame)内部的[**框架**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.window). 框架随后即可以在应用程序的[页面](../basics/navigate-between-two-pages.md)实例之间进行[**导航**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Page)。 

大多数页面采用通用的布局结构，本文将介绍你将需要哪些 UI 元素，以及它们应该安置在页面的什么位置。 

![页面结构](images/page-components.svg)

## <a name="navigation"></a>导航
应用布局以你所选的导航模型开始，该模型定义了用户在应用中的页面之间的导航方式。 在本文中，我们将讨论两种通用导航模式： 左导航和顶部导航。 有关选择其他导航选项的说明，请参阅[适用于 UWP 应用的导航设计基础知识](../basics/navigation-basics.md)。

![顶部和左侧的导航模式](images/top-left-nav.svg)

### <a name="left-nav"></a>左侧导航
左侧导航，或[导航窗格](../controls-and-patterns/navigationview.md)模式，通常只用作应用级导航，并且在应用中处在最高层级，这意味着它应始终可见并且始终可用。 当应用中有超过五个导航项目或者页面时，我们建议使用左侧导航。 导航窗格模式通常包含：
- 导航项
- 应用程序设置入口点
- 帐户设置入口点

[NavigationView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview) 控件为 UWP 实现了左侧导航模式。

选择某个导航项后，框架应导航到选定的项所对应的页面。

![展开导航窗格](images/navview-expanded.svg)

菜单按钮允许用户展开和折叠导航窗格。 当屏幕大小大于 640 像素，单击菜单按钮栏中折叠导航窗格。

![压缩的导航窗格](images/navview-compact.svg)

当屏幕大小小于 640 像素时，导航窗格会完全折叠。

![最小的导航窗格](images/navview-minimal.svg)

### <a name="top-nav"></a>顶部导航

顶部导航也可以用作顶级导航。 与左侧导航可以折叠收起的风格不同，顶部导航始终是可见的。 [NavigationView](../controls-and-patterns/navigationview.md) 控件为 UWP 实现了顶部的导航和选项卡模式。顶部的导航和选项卡模式。

![顶部导航](images/pivot-large.svg)

## <a name="command-bar"></a>命令栏

你可能想为用户提供应用中常用任务的快捷访问方式。 [命令栏](../controls-and-patterns/app-bars.md)可以提供应用级或页级命令的访问途径，并且可在任何导航模式中使用。

![放置在顶部的命令栏 ](images/app-bar-desktop.svg)

命令栏可以放置在页面顶部或页面底部，以最适合应用的放置位置为准。

![放置在底部的命令栏](images/app-bar-mobile.svg)

## <a name="content"></a>内容

最后，内容会因应用不同而有很大差异，因此可以通过多种不同方式呈现内容。 在这里，我们介绍了一些你可能想在应用中使用的常见页面模式。 许多应用会使用此处的部分或者全部常见页面模式来显示不同类型的内容。 当然，也可以随意混合和搭配使用这些模式来优化应用。

## <a name="landing"></a>登陆

![登陆页面](images/hero-screen.svg)

登陆页面，也称作 hero 屏幕，通常显示在应用体验的最高层。 巨大的图面区域充当平台，供应用突出显示用户可能想要浏览和使用的内容。

## <a name="collections"></a>集合

![库](images/gridview.svg)

集合可以让用户浏览以组划分的内容或数据。 如果要展示以照片或者媒体为主的内容，那么使用[网格视图](../controls-and-patterns/item-templates-gridview.md)是个不错的选择。如果要展示以文字或数据为主的内容，那么使用[列表视图](../controls-and-patterns/item-templates-listview.md)则更为合适。

## <a name="masterdetail"></a>大纲/细节

![大纲细节](images/master-detail.svg)

[大纲/细节](../controls-and-patterns/master-details.md)模型包含一个列表视图（大纲）和一个内容视图（细节）。 这两个窗格都是固定的，并且具有垂直滚动功能。 选择列表视图中的项后，就会相应地更新内容视图。 

## <a name="forms"></a>表单
![表单](images/form.svg)

[表单](../controls-and-patterns/forms.md)由一组控件组成，用于收集和提交来自用户的数据。 大多数应用将某种形式的表单用于设置页面、登录门户、反馈中心、账户创建或其他目的。 

## <a name="sample-apps"></a>示例应用
若要查看如何实现这些模式，请查看我们[UWP 示例应用](https://developer.microsoft.com/en-us/windows/samples):
- [BuildCast 视频播放器](https://github.com/Microsoft/BuildCast)
- [Lunch Scheduler](https://github.com/Microsoft/Windows-appsample-lunch-scheduler)
- [Coloring Book](https://github.com/Microsoft/Windows-appsample-coloringbook)
- [客户订单数据库](https://github.com/Microsoft/Windows-appsample-customers-orders-database)