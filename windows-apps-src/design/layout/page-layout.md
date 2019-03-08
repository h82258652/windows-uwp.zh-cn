---
title: 适用于 UWP 应用的页面布局
description: 在设计您的应用程序，首先要考虑是布局结构。 本文介绍了基本的页面布局，包括哪些 UI 元素，你将需要并且它们应发送到何处的页上的通用结构。 在 UWP 应用中，每个页通常具有导航、 命令和内容元素。
ms.date: 03/19/2018
ms.topic: article
keywords: windows 10, uwp
localizationpriority: medium
ms.openlocfilehash: edda9948e70b1757ddb46fae45a579bb2fdb8de1
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2019
ms.locfileid: "57633712"
---
# <a name="page-layout"></a>页面布局

在 UWP 应用中，每个[**页面**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Page)通常具有导航、 命令和内容元素。 

您的应用程序可以有多个页面： 当用户启动的 UWP 应用时，应用程序代码创建[**帧**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Frame)放置在应用程序的内部[**窗口**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.window). 然后可以在帧[导航](../basics/navigate-between-two-pages.md)应用程序之间[**页**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Page)实例。 

大多数页面遵循常见布局结构，并且这篇文章介绍哪些 UI 元素都需要以及它们应发送到何处的页上。 

![页面结构](images/page-components.svg)

## <a name="navigation"></a>导航
使用导航模型选择，用于定义你的用户应用程序中的页面之间导航的方式开始您的应用布局。 在本文中，我们将讨论两个通用导航模式： 左导航栏和顶部的导航 选择其他导航选项的指导，请参阅[适用于 UWP 应用的导航设计基础知识](../basics/navigation-basics.md)。

![顶部和左侧的导航模式](images/top-left-nav.svg)

### <a name="left-nav"></a>左侧导航栏
左导航栏中或[导航窗格](../controls-and-patterns/navigationview.md)模式，是为应用程序级导航通常保留在应用中，这意味着，它应始终为可见和可用的最高级别存在。 有五个导航项或应用程序中的多个 5 个页面时，我们建议左侧导航栏。 导航窗格模式通常包含：
- 导航项
- 应用程序设置入口点
- 帐户设置入口点

[NavigationView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview)为 UWP 控件实现的左侧导航栏模式。

选择导航项后，框架应导航到选定的项的页。

![展开导航窗格](images/navview-expanded.svg)

菜单按钮，用户可以展开和折叠导航窗格。 当屏幕大小大于 640 像素，单击菜单按钮栏中折叠导航窗格。

![compact 的导航窗格](images/navview-compact.svg)

当屏幕大小小于 640 像素，导航窗格完全处于折叠状态。

![最小的导航窗格](images/navview-minimal.svg)

### <a name="top-nav"></a>顶部导航栏

顶部导航也可用作顶级导航。 可折叠左侧导航栏时，顶部导航始终是可见的。 [NavigationView](../controls-and-patterns/navigationview.md)为 UWP 控件实现的顶部的导航和选项卡模式。

![顶部导航栏](images/pivot-large.svg)

## <a name="command-bar"></a>命令栏

接下来，你可能想要向用户提供轻松访问你的应用的最常见的任务。 一个[命令栏](../controls-and-patterns/app-bars.md)可以提供访问应用程序级别或页级别的命令，并可与任何导航模式。

![在顶部的命令栏放置 ](images/app-bar-desktop.svg)

在命令栏可以放置在顶部或底部的页上，两者中较最适合您的应用程序。

![在底部命令栏放置](images/app-bar-mobile.svg)

## <a name="content"></a>内容

最后，内容而异广泛在应用，因此可以以多种不同方式显示内容。 在这里，我们将介绍一些常见的可能想要使用应用程序中的页模式。 许多应用使用部分或所有此类常见页面模式来显示不同类型的内容。 同样，随意混合和匹配这些模式以优化您的应用程序。

## <a name="landing"></a>登录

![登录页面](images/hero-screen.svg)

登录页面（也称为 hero 屏幕）通常显示在应用体验的最高层。 大型图面区域充当一个平台以供应用突出显示用户可能要浏览和使用的内容。

## <a name="collections"></a>集合

![库](images/gridview.svg)

集合可让用户浏览一组内容或数据。 [网格视图](../controls-and-patterns/item-templates-gridview.md)是适用于照片或媒体中心式内容的选项，[列表视图](../controls-and-patterns/item-templates-listview.md)是适用于以文字为主内容或数据的选项。

## <a name="masterdetail"></a>大纲/细节

![大纲细节](images/master-detail.svg)

[大纲/细节](../controls-and-patterns/master-details.md)模型包含一个列表视图（大纲）和一个内容视图（细节）。 这两个窗格都是固定的，并且具有垂直滚动。 选择列表视图中的项后，相应地更新内容的视图。 

## <a name="forms"></a>表单
![表单](images/form.svg)

[表单](../controls-and-patterns/forms.md)是一组控件，用于收集和提交来自用户的数据。 大多数（而非全部）应用将某种形式的表单用于设置页面、登录门户、反馈中心、账户创建或其他目的。 

## <a name="sample-apps"></a>示例应用
若要查看如何实现这些模式，请查看我们[UWP 示例应用](https://developer.microsoft.com/en-us/windows/samples):
- [BuildCast 视频播放器](https://github.com/Microsoft/BuildCast)
- [Lunch Scheduler](https://github.com/Microsoft/Windows-appsample-lunch-scheduler)
- [Coloring Book](https://github.com/Microsoft/Windows-appsample-coloringbook)
- [客户订单数据库](https://github.com/Microsoft/Windows-appsample-customers-orders-database)