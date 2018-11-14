---
author: stevewhims
ms.assetid: E2B73380-D673-48C6-9026-96976D745017
description: 常见控件入门
title: 常见控件入门
ms.author: stwhi
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: db2b4a74b5b40060779dd82764dcf2ed2799b285
ms.sourcegitcommit: 71e8eae5c077a7740e5606298951bb78fc42b22c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/13/2018
ms.locfileid: "6653021"
---
# <a name="getting-started-common-controls"></a>入门：常见控件


## <a name="common-controls-list"></a>常用控件列表

在前面的部分中，你仅使用了两个控件：按钮和文本块。 还有，当然，许多控件可供你。 下面是一些可在你的应用和与其等效的 iOS 中使用的常用控件。 iOS 控件按字母顺序列出，后跟最为相似的通用 Windows 平台 (UWP) 控件。

UWP 控件相当智能的方面是，它们可以感知到在其上运行的设备类型，并相应地更改外观和功能。 例如，如果项目使用 [**DatePicker**](https://msdn.microsoft.com/library/windows/apps/br211681) 控件，能够优化自身以在桌面计算机上呈现不同于手机上的外观和行为，这就足够智能。 你无需执行任何操作，因为控件会在运行时对自身进行调整。

| iOS 控件（类/协议） | 等效的 UWP 控件 |
|------------------------------|--------------------------------------|
| 活动指示器 (**UIActivityIndicatorView**) | [**ProgressRing**](https://msdn.microsoft.com/library/windows/apps/br227538) <br/> 另请参阅[快速入门：添加进度控件](https://msdn.microsoft.com/library/windows/apps/xaml/hh780651) |
| 广告横幅视图 (**ADBannerView**) 和广告横幅视图委托 (**ADBannerViewDelegate**) | [AdControl](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.aspx) <br/> 另请参阅[在应用中显示广告](../monetize/display-ads-in-your-app.md) |
| 按钮 (UIButton) | [按钮](https://msdn.microsoft.com/library/windows/apps/br209265) <br/> 另请参阅[快速入门：添加按钮控件](https://msdn.microsoft.com/library/windows/apps/xaml/jj153346) |
| 日期选取器 (UIDatePicker) | [DatePicker](https://msdn.microsoft.com/library/windows/apps/br211681) |
| 图像视图 (UIDatePicker) | [图像](https://msdn.microsoft.com/library/windows/apps/br242752) <br/> 另请参阅[图像和 ImageBrush](https://msdn.microsoft.com/library/windows/apps/mt280382) |
| 标签 (UILabel) | [TextBlock](https://msdn.microsoft.com/library/windows/apps/br209652) <br/> 另请参阅[快速入门：显示文本](https://msdn.microsoft.com/library/windows/apps/xaml/hh700392) |
| 地图视图 (MKMapView) 和地图视图委派 (MKMapViewDelegate) | 请参阅[适用于 UWP 应用的必应地图](http://go.microsoft.com/fwlink/p/?LinkId=263496) |
| 导航控制器 (UINavigationController) 和导航控制器委托 (UINavigationControllerDelegate) | [帧](https://msdn.microsoft.com/library/windows/apps/br242682) <br/> 另请参阅[导航](https://msdn.microsoft.com/library/windows/apps/mt187344) |
| 页面控件 (UIPageControl) | [页面](https://msdn.microsoft.com/library/windows/apps/br227503) <br/> 另请参阅[导航](https://msdn.microsoft.com/library/windows/apps/mt187344) |
| 选取器视图 (UIPickerView) 和选取器视图委托 (UIPickerViewDelegate) | [组合框](https://msdn.microsoft.com/library/windows/apps/br209348) <br/> 另请参阅[添加组合框和列表框](https://msdn.microsoft.com/library/windows/apps/xaml/hh780616) |
| 进度条 (UIProgressView) | [进度栏](https://msdn.microsoft.com/library/windows/apps/br227529) <br/> 另请参阅[快速入门：添加进度控件](https://msdn.microsoft.com/library/windows/apps/xaml/hh780651) |
| 滚动视图 (UIScrollView) 和滚动视图委托 (UIScrollViewDelegate) | [ScrollViewer](https://msdn.microsoft.com/library/windows/apps/br209527) <br/>  另请参阅[Extensible Application Markup Language (XAML) 滚动、平移以及缩放示例](http://go.microsoft.com/fwlink/p/?LinkId=238577) |
| 搜索栏 (UISearchBar) 和搜索栏委派 (UISearchBarDelegate) | 请参阅[向应用添加搜索](https://msdn.microsoft.com/library/windows/apps/xaml/jj130767) <br/>  另请参阅[快速入门：向应用添加搜索](https://msdn.microsoft.com/library/windows/apps/xaml/hh868180) |
| 分段控件 (UISegmentedControl) | 无 |
| 滑块 (UISlider) | [滑块](https://msdn.microsoft.com/library/windows/apps/br209614) <br/>  另请参阅[如何添加滑块](https://msdn.microsoft.com/library/windows/apps/xaml/hh868197) |
| 拆分视图控制器 (UISplitViewController) 和拆分视图控制器委派 (UISplitViewControllerDelegate) | 无 |
| 开关 (UISwitch) | [ToggleSwitch](https://msdn.microsoft.com/library/windows/apps/br209712) <br/>  另请参阅[如何添加切换开关](https://msdn.microsoft.com/library/windows/apps/xaml/hh868198) |
| 选项卡栏控制器 (UITabBarController) 和选项卡栏控制器委派 (UITabBarControllerDelegate) | 无 |
| 表视图控制器 (UITableViewController)、表视图 (UITableView)、表视图委托 (UITableViewDelegate) 和表单元格 (UITableViewCell) | [ListView](https://msdn.microsoft.com/library/windows/apps/br242878) <br/>  另请参阅[快速入门：添加 ListView 和 GridView 控件](https://msdn.microsoft.com/library/windows/apps/xaml/hh780650) |
| 文本字段 (UITextField) 和文本字段委托 (UITextFieldDelegate) | [TextBox](https://msdn.microsoft.com/library/windows/apps/br209683) <br/>  另请参阅[显示和编辑文本](https://msdn.microsoft.com/library/windows/apps/mt280218) |
| 文本视图 (UITextView) 和文本视图委托 (UITextViewDelegate) | [TextBlock](https://msdn.microsoft.com/library/windows/apps/br209652) <br/>  另请参阅[快速入门：显示文本](https://msdn.microsoft.com/library/windows/apps/xaml/hh700392) |
| 视图 (UIView) 和视图控制器 (UIViewController) | [页面](https://msdn.microsoft.com/library/windows/apps/br227503) <br/>  另请参阅[导航](https://msdn.microsoft.com/library/windows/apps/mt187344) |
| Web 视图 (UIWebView) 和 Web 视图委托 (UIWebViewDelegate) | [WebView](https://msdn.microsoft.com/library/windows/apps/br227702) <br/>  另请参阅 [XAML WebView 控件示例](http://go.microsoft.com/fwlink/p/?LinkId=238582) |
| 窗口 (UIWindow) | [帧](https://msdn.microsoft.com/library/windows/apps/br242682) <br/>  另请参阅[导航](https://msdn.microsoft.com/library/windows/apps/mt187344) |

有关其他更多控件，请参阅[控件列表](https://msdn.microsoft.com/library/windows/apps/mt185406)。

**请注意**控件适用于使用 JavaScript 和 HTML 的 UWP 应用的列表，请参阅[控件列表](https://msdn.microsoft.com/library/windows/apps/hh465453)。

### <a name="next-step"></a>下一步

[入门：导航](getting-started-navigation.md)

## <a name="related-topics"></a>相关主题

* [版本 2014：XAML UI 和控件如何？](http://go.microsoft.com/fwlink/p/?LinkID=397897)
* [版本 2014：使用常见 XAML UI 框架开发应用](http://go.microsoft.com/fwlink/p/?LinkID=397898)
* [版本 2014：使用 Visual Studio 构建 XAML 融合应用](http://go.microsoft.com/fwlink/p/?LinkID=397876)
