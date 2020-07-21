---
title: Windows 应用的页面布局
description: 设计应用时，首先要考虑的是布局结构。 本文介绍了基本页面布局的常见结构，其中包括你需要的 UI 元素以及它们在页面上的位置。 在 Windows 应用中，每个页面通常都有导航、命令和内容元素。
ms.date: 03/19/2018
ms.topic: article
keywords: windows 10, uwp
localizationpriority: medium
ms.openlocfilehash: 3dbdf8d006472c288369fd528e89663aca5a9e10
ms.sourcegitcommit: 0dee502484df798a0595ac1fe7fb7d0f5a982821
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/08/2020
ms.locfileid: "82970882"
---
# <a name="page-layout"></a>页面布局

在 Windows 应用中，每个[页面  ](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Page)通常都有导航、命令和内容元素。 

应用可以有多个页面：当用户启动 Windows 应用时，应用程序代码会创建一个[帧  ](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Frame)放置在应用程序的[窗口  ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.window)内。 然后，帧可以在应用程序的[页面  ](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Page)实例之间[导航](../basics/navigate-between-two-pages.md)。 

大多数页面都遵循常见的布局结构，本文介绍了需要哪些 UI 元素以及它们在页面上的位置。 

![页面结构](images/page-components.svg)

## <a name="navigation"></a>导航
应用布局从所选的导航模型开始，后者定义用户在应用中的页面之间导航的方式。 本文介绍两种常见的导航模式：左侧导航和顶部导航。 有关选择其他导航选项的指南，请参阅 [Windows 应用的导航设计基础知识](../basics/navigation-basics.md)。

![顶部导航模式和左侧导航模式](images/top-left-nav.svg)

### <a name="left-nav"></a>左侧导航
对于应用级导航，左侧导航或[导航窗格](../controls-and-patterns/navigationview.md)模式通常会保留，并存在于应用的最高级别，这意味着它应始终可见且可用。 如果应用中有 5 个以上的导航项或超过 5 个页面，则建议使用左侧导航。 导航窗格模式通常包含：
- 导航项
- 应用设置中的入口点
- 帐户设置中的入口点

[NavigationView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview) 控件实现 UWP 的左侧导航模式。

选中导航项后，帧应导航到选定项页。

![导航窗格展开](images/navview-expanded.svg)

“菜单”按钮允许用户展开和折叠导航窗格。 当屏幕尺寸大于 640 像素时，单击“菜单”按钮会将导航窗格折叠为一个栏。

![导航窗格精简](images/navview-compact.svg)

当屏幕尺寸小于 640 像素时，导航窗格会完全折叠。

![最小导航窗格](images/navview-minimal.svg)

### <a name="top-nav"></a>顶部导航

顶部导航还可以充当顶级导航。 当左侧导航可折叠时，顶部导航始终可见。 [NavigationView](../controls-and-patterns/navigationview.md) 控件实现 UWP 的顶部导航和选项卡模式。

![顶部导航](images/pivot-large.svg)

## <a name="command-bar"></a>命令栏

接下来，你可能希望用户能够轻松访问应用的最常见任务。 [命令栏](../controls-and-patterns/app-bars.md)可以提供对应用级或页面级命令的访问，并且可以与任何导航模式一起使用。

![命令栏位于顶部 ](images/app-bar-desktop.svg)

命令栏可放置在页面顶部或底部，具体看哪种方式最适合你的应用。

![命令栏位于底部](images/app-bar-mobile.svg)

## <a name="content"></a>内容

最后，不同应用程序之间的内容差异很大，因此你可以以许多不同的方式呈现内容。 在这里，我们将介绍一些你可能想要在应用中使用的常见页面模式。 许多应用使用部分或所有此类常见页面模式来显示不同类型的内容。 同样地，可以随意混合和匹配这些模式以针对应用进行优化。

## <a name="landing"></a>登陆

![登陆页面](images/hero-screen.svg)

登陆页面（也称为 hero 屏幕）通常显示在应用体验的最高层。 大型图面区域充当一个平台以供应用突出显示用户可能要浏览和使用的内容。

## <a name="collections"></a>集合

![库](images/gridview.svg)

集合可让用户浏览一组内容或数据。 [网格视图](../controls-and-patterns/item-templates-gridview.md)是适用于照片或媒体中心式内容的选项，[列表视图](../controls-and-patterns/item-templates-listview.md)是适用于以文字为主内容或数据的选项。

## <a name="masterdetail"></a>大纲/细节

![大纲细节](images/master-detail.svg)

[大纲/细节](../controls-and-patterns/master-details.md)模型包含一个列表视图（大纲）和一个内容视图（细节）。 这两个窗格都是固定的，并且具有垂直滚动。 选择列表视图中的项时，将相应地更新内容视图。 

## <a name="forms"></a>窗体
![表单](images/form.svg)

[表单](../controls-and-patterns/forms.md)是一组控件，用于收集和提交来自用户的数据。 大多数（而非全部）应用将某种形式的表单用于设置页面、登录门户、反馈中心、帐户创建或其他目的。 

## <a name="sample-apps"></a>示例应用
若要了解如何实现这些模式，请查看 [Windows 示例应用](https://developer.microsoft.com/windows/samples)：
- [BuildCast 视频播放器](https://github.com/Microsoft/BuildCast)
- [Lunch Scheduler](https://github.com/Microsoft/Windows-appsample-lunch-scheduler)
- [Coloring Book](https://github.com/Microsoft/Windows-appsample-coloringbook)
- [客户订单数据库](https://github.com/Microsoft/Windows-appsample-customers-orders-database)
