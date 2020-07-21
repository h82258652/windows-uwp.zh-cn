---
ms.assetid: E2B73380-D673-48C6-9026-96976D745017
description: 常见控件入门
title: 常见控件入门
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 979dabe193fc1ab6ecaa8c31a70ca2364448986e
ms.sourcegitcommit: c1226b6b9ec5ed008a75a3d92abb0e50471bb988
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/20/2020
ms.locfileid: "86493282"
---
# <a name="getting-started-common-controls"></a>入门：常见控件


## <a name="common-controls-list"></a>常见控件列表

在前面的部分中，你仅使用了两个控件：按钮和文本块。 当然，还有更多控件可供您使用。 下面是一些可在你的应用和与其等效的 iOS 中使用的常用控件。 iOS 控件按字母顺序列出，后跟最为相似的通用 Windows 平台 (UWP) 控件。

UWP 控件相当智能的方面是，它们可以感知到在其上运行的设备类型，并相应地更改外观和功能。 例如，如果项目使用 [**DatePicker**](https://docs.microsoft.com/previous-versions/windows/apps/br211681(v=win.10)) 控件，能够优化自身以在桌面计算机上呈现不同于手机上的外观和行为，这就足够智能。 你无需执行任何操作，因为控件会在运行时对自身进行调整。

| iOS 控件（类/协议） | 等效 UWP 控件 |
|------------------------------|--------------------------------------|
| 活动指示器 (**UIActivityIndicatorView**) | [**ProgressRing**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ProgressRing) <br/> 另请参阅[快速入门：添加进度控件](https://docs.microsoft.com/previous-versions/windows/apps/hh780651(v=win.10)) |
| 广告横幅视图 (**ADBannerView**) 和广告横幅视图委托 (**ADBannerViewDelegate**) | [AdControl](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.adcontrol) <br/> 另请参阅[在应用中显示广告](../monetize/display-ads-in-your-app.md) |
| 按钮 (UIButton) | [Button](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Button) <br/> 另请参阅[快速入门：添加按钮控件](https://docs.microsoft.com/previous-versions/windows/apps/jj153346(v=win.10)) |
| 日期选取器 (UIDatePicker) | [DatePicker](https://docs.microsoft.com/previous-versions/windows/apps/br211681(v=win.10)) |
| 图像视图 (UIDatePicker) | [图像](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Image) <br/> 另请参阅[图像和 ImageBrush](https://docs.microsoft.com/windows/uwp/controls-and-patterns/images-imagebrushes) |
| 标签 (UILabel) | [TextBlock](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TextBlock) <br/> 另请参阅[快速入门：显示文本](https://docs.microsoft.com/previous-versions/windows/apps/hh700392(v=win.10)) |
| 地图视图 (MKMapView) 和地图视图委派 (MKMapViewDelegate) | 请参阅[适用于 UWP 应用的 Bing 地图](https://msdn.microsoft.com/library/hh846481) |
| 导航控制器 (UINavigationController) 和导航控制器委托 (UINavigationControllerDelegate) | [Frame](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Frame) <br/> 另请参阅[导航](https://docs.microsoft.com/windows/uwp/layout/navigation-basics) |
| 页面控件 (UIPageControl) | [第](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Page) <br/> 另请参阅[导航](https://docs.microsoft.com/windows/uwp/layout/navigation-basics) |
| 选取器视图 (UIPickerView) 和选取器视图委托 (UIPickerViewDelegate) | [ComboBox](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ComboBox) <br/> 另请参阅[添加组合框和列表框](https://docs.microsoft.com/previous-versions/windows/apps/hh780616(v=win.10)) |
| 进度条 (UIProgressView) | [ProgressBar](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ProgressBar) <br/> 另请参阅[快速入门：添加进度控件](https://docs.microsoft.com/previous-versions/windows/apps/hh780651(v=win.10)) |
| 滚动视图 (UIScrollView) 和滚动视图委托 (UIScrollViewDelegate) | [ScrollViewer](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ScrollViewer) <br/>  另请参阅[Extensible Application Markup Language (XAML) 滚动、平移以及缩放示例](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Windows%208%20app%20samples/%5BC%23%5D-Windows%208%20app%20samples/C%23/Windows%208%20app%20samples/XAML%20scrolling%2C%20panning%2C%20and%20zooming%20sample%20(Windows%208)) |
| 搜索栏 (UISearchBar) 和搜索栏委派 (UISearchBarDelegate) | 请参阅[向应用添加搜索](https://docs.microsoft.com/previous-versions/windows/apps/jj130767(v=win.10)) <br/>  另请参阅[快速入门：向应用添加搜索](https://docs.microsoft.com/previous-versions/windows/apps/hh868180(v=win.10)) |
| 分段控件 (UISegmentedControl) | 无 |
| 滑块 (UISlider) | [滑块](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Slider) <br/>  另请参阅[如何添加滑块](https://docs.microsoft.com/previous-versions/windows/apps/hh868197(v=win.10)) |
| 拆分视图控制器 (UISplitViewController) 和拆分视图控制器委派 (UISplitViewControllerDelegate) | 无 |
| 开关 (UISwitch) | [ToggleSwitch](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ToggleSwitch) <br/>  另请参阅[如何添加切换开关](https://docs.microsoft.com/previous-versions/windows/apps/hh868198(v=win.10)) |
| 选项卡栏控制器 (UITabBarController) 和选项卡栏控制器委派 (UITabBarControllerDelegate) | 无 |
| 表视图控制器 (UITableViewController)、表视图 (UITableView)、表视图委托 (UITableViewDelegate) 和表单元格 (UITableViewCell) | [ListView](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ListView) <br/>  另请参阅[快速入门：添加 ListView 和 GridView 控件](https://docs.microsoft.com/previous-versions/windows/apps/hh780650(v=win.10)) |
| 文本字段 (UITextField) 和文本字段委托 (UITextFieldDelegate) | [TextBox](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TextBox) <br/>  另请参阅[显示和编辑文本](https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/text-controls) |
| 文本视图 (UITextView) 和文本视图委托 (UITextViewDelegate) | [TextBlock](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TextBlock) <br/>  另请参阅[快速入门：显示文本](https://docs.microsoft.com/previous-versions/windows/apps/hh700392(v=win.10)) |
| 视图 (UIView) 和视图控制器 (UIViewController) | [第](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Page) <br/>  另请参阅[导航](https://docs.microsoft.com/windows/uwp/layout/navigation-basics) |
| Web 视图 (UIWebView) 和 Web 视图委托 (UIWebViewDelegate) | [WebView](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.WebView) <br/>  另请参阅 [XAML WebView 控件示例](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Windows%208%20app%20samples/%5BC%23%5D-Windows%208%20app%20samples/C%23/Windows%208%20app%20samples/XAML%20WebView%20control%20sample%20(Windows%208)) |
| 窗口 (UIWindow) | [Frame](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Frame) <br/>  另请参阅[导航](https://docs.microsoft.com/windows/uwp/layout/navigation-basics) |

有关其他更多控件，请参阅[控件列表](https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/)。

**注意**   有关使用 JavaScript 和 HTML 的 UWP 应用的控件列表，请参阅[控件列表](https://docs.microsoft.com/previous-versions/windows/apps/hh465453(v=win.10))。

### <a name="next-step"></a>后续步骤

[入门：导航](getting-started-navigation.md)

## <a name="related-topics"></a>相关主题

* [版本 2014：XAML UI 和控件如何？](https://channel9.msdn.com/Events/Build/2014/2-516)
* [版本 2014：使用常见 XAML UI 框架开发应用](https://channel9.msdn.com/Events/Build/2014/2-507)
* [版本 2014：使用 Visual Studio 构建 XAML 融合应用](https://channel9.msdn.com/Events/Build/2014/3-591)
