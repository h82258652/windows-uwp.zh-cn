---
title: WinUI 2.0 发行说明
description: WinUI 2.0 的发行说明。
ms.date: 04/15/2020
ms.topic: article
ms.openlocfilehash: 300a8eaa0077b3a59ef2ac01213cd82b0c76c343
ms.sourcegitcommit: d0f479f1955881afb62c2af249db5d0b053b63e5
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/19/2020
ms.locfileid: "83580654"
---
# <a name="windows-ui-library-20"></a>Windows UI 库 2.0

WinUI 2.0 是 Windows UI 库的第一个公共版本。

若要构建适用于 Windows 的出色 Fluent Design 体验，最简单的方法是使用 WinUI。

它包括两个 NuGet 包：

* [Microsoft.UI.Xaml](https://www.nuget.org/packages/Microsoft.UI.Xaml)：适用于 UWP 应用的控件和 Fluent Design。 这是主 WinUI 包。

* [Microsoft.UI.Xaml.Core.Direct](https://www.nuget.org/packages/Microsoft.UI.Xaml.Core.Direct)：用在中间件组件中的低级别 API。

可以使用 NuGet 包管理器在应用中下载并使用 WinUI 包：有关详细信息，请参阅 [Getting Started with the Windows UI Library](https://docs.microsoft.com/uwp/toolkits/winui/getting-started)（Windows UI 库入门）。

WinUI 是托管在 GitHub 上的开源项目。 我们欢迎你在 [Windows UI 库存储库](https://aka.ms/winui)中提供 Bug 报告、提交功能请求和贡献社区代码。

## <a name="microsoftuixaml-20181011001"></a>Microsoft.UI.Xaml 2.0.181011001

2018 年 10 月

这是 [Microsoft.UI.Xaml NuGet 包](https://www.nuget.org/packages/Microsoft.UI.Xaml)的第一版。 它包括适用于 Windows UWP 应用的官方本机 Fluent 控件和功能。

### <a name="new-features"></a>新功能

此版本中的控件和模式包括：

| 功能 | 说明 |
| --- | --- |
|[AcrylicBrush]( https://docs.microsoft.com/uwp/api/microsoft.ui.xaml.media.acrylicbrush)| 使用半透明材料绘制一个区域，该材料使用多种效果，其中包括模糊效果和杂色纹理效果。|
|[BitmapIconSource]( https://docs.microsoft.com/uwp/api/microsoft.ui.xaml.controls.bitmapiconsource)| 表示一个使用位图作为其内容的图标源。|
|[ColorPicker]( https://docs.microsoft.com/uwp/api/microsoft.ui.xaml.controls.colorpicker)| 表示一个控件，该控件允许用户通过色谱、滑块和文本输入来选取颜色。|
|[CommandBarFlyout](https://docs.microsoft.com/uwp/api/microsoft.ui.xaml.controls.commandbarflyout)|表示一个专用浮出控件，该控件为 AppBarButton 和相关的命令元素提供布局。|
|[DropDownButton](https://docs.microsoft.com/uwp/api/microsoft.ui.xaml.controls.dropdownbutton)|表示一个带 v 形图标的按钮，用于打开菜单。|
|[FontIconSource](https://docs.microsoft.com/uwp/api/microsoft.ui.xaml.controls.fonticonsource)|表示一个图标源，该源使用指定字体中的字形。|
|[MenuBar](https://docs.microsoft.com/uwp/api/microsoft.ui.xaml.controls.menubar)|表示一个专用容器，该容器在水平行中呈现一组菜单（通常位于应用窗口的顶部）。|
|[MenuBarItem](https://docs.microsoft.com/uwp/api/microsoft.ui.xaml.controls.menubaritem)|表示 MenuBar 控件中的顶级菜单。|
|[NavigationView](https://docs.microsoft.com/uwp/api/microsoft.ui.xaml.controls.navigationview)|表示一个容器，该容器允许对应用内容进行导航。 它有一个标头、一个针对主内容的视图，以及一个用于导航命令的菜单窗格。|
|[ParallaxView](https://docs.microsoft.com/uwp/api/microsoft.ui.xaml.controls.parallaxview)|表示一个容器，该容器将前景元素（如列表）的滚动位置与背景元素（如图像）关联到一起。 当你滚动浏览前景元素时，它会将背景元素动画化，形成视差效果。|
|[PersonPicture](https://docs.microsoft.com/uwp/api/microsoft.ui.xaml.controls.personpicture)|表示一个控件，该控件在用户头像可用的情况下显示用户头像，在用户头像不可用的情况下显示该用户的姓名缩写或通用字形。|
|[RatingControl](https://docs.microsoft.com/uwp/api/microsoft.ui.xaml.controls.ratingcontrol)|表示一个控件，该控件允许用户输入星级评分。|
|[RefreshContainer](https://docs.microsoft.com/uwp/api/microsoft.ui.xaml.controls.refreshcontainer)|表示一个容器控件，该控件提供 RefreshVisualizer 以及针对可滚动内容的下拉刷新功能。|
|[RefreshVisualizer](https://docs.microsoft.com/uwp/api/microsoft.ui.xaml.controls.refreshvisualizer)|表示一个控件，该控件为内容刷新提供动画化的状态指示器。|
|[RevealBackgroundBrush](https://docs.microsoft.com/uwp/api/microsoft.ui.xaml.media.revealbackgroundbrush)|使用合成画笔和光效绘制控件背景，并展示效果。|
|[RevealBorderBrush](https://docs.microsoft.com/uwp/api/microsoft.ui.xaml.media.revealborderbrush)|使用合成画笔和光效绘制控件边框，并展示效果。|
|[RevealBrush](https://docs.microsoft.com/uwp/api/microsoft.ui.xaml.media.revealbrush)|画笔的基类，此类画笔使用组合效果和照明来实现视觉展示效果的设计处理。|
|[SplitButton](https://docs.microsoft.com/uwp/api/microsoft.ui.xaml.controls.splitbutton)|表示一个按钮，该按钮的两个部分可单独调用。 一个部分的行为类似于标准按钮，另一个部分可调用浮出控件。|
|[SwipeControl](https://docs.microsoft.com/uwp/api/microsoft.ui.xaml.controls.swipecontrol)|表示一个容器，该容器允许通过触摸交互来访问上下文命令。|
|[SymbolIconSource](https://docs.microsoft.com/uwp/api/microsoft.ui.xaml.controls.symboliconsource)|表示一个图标源，该源使用 Segoe MDL2 Assets 字体中的字形作为其内容。|
|[TextCommandBarFlyout](https://docs.microsoft.com/uwp/api/microsoft.ui.xaml.controls.textcommandbarflyout)|表示专用的命令栏浮出控件，其中包含用于编辑文本的命令。|
|[ToggleSplitButton](https://docs.microsoft.com/uwp/api/microsoft.ui.xaml.controls.togglesplitbutton)|表示一个按钮，该按钮的两个部分可单独调用。 一个部分的行为类似于切换按钮，另一个部分可调用浮出控件。|
|[TreeView](https://docs.microsoft.com/uwp/api/microsoft.ui.xaml.controls.treeview)|表示一个分层列表，其中的展开节点和折叠节点包含嵌套项。|

## <a name="examples"></a>示例

Xaml 控件库示例应用包括介绍如何使用 WinUI 控件的交互式演示和示例代码。

* 从 [Microsoft Store](
https://www.microsoft.com/p/xaml-controls-gallery/9msvh128x2zt) 安装 XAML 控件库应用

* Xaml 控件库也[在 GitHub 上开源](
https://github.com/Microsoft/Xaml-Controls-Gallery)

## <a name="documentation"></a>文档

Windows UI 库控件的操作方法文章包含在[通用 Windows 平台控件文档](/windows/uwp/design/controls-and-patterns/)中。

API 参考文档位于此处：[Windows UI 库 API](/uwp/api/overview/winui/)。
