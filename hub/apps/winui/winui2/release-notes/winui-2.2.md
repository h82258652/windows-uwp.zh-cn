---
title: WinUI 2.2 发行说明
description: WinUI 2.2 的发行说明，包括新功能和 Bug 修复。
ms.date: 07/15/2020
ms.topic: article
ms.openlocfilehash: 6706b0202057b9a3419ec45a43a5f6d4d574327b
ms.sourcegitcommit: c1226b6b9ec5ed008a75a3d92abb0e50471bb988
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/20/2020
ms.locfileid: "86493092"
---
# <a name="windows-ui-library-22"></a>Windows UI 库 2.2

WinUI 2.2 是 Windows UI 库的最新官方版本。

可以使用 NuGet 包管理器向应用添加 WinUI 包：有关详细信息，请参阅 [Windows UI 库入门](../getting-started.md)。

WinUI 是托管在 GitHub 上的开源项目。 我们欢迎你在 [Windows UI 库存储库](https://aka.ms/winui)中提供 Bug 报告、提交功能请求和贡献社区代码。

## <a name="microsoftuixaml-22-version-history"></a>Microsoft.UI.Xaml 2.2 版本历史记录

### <a name="windows-ui-library-22-official-release"></a>Windows UI 库 2.2 官方版本

2019 年 8 月

[GitHub 发布页](https://github.com/microsoft/microsoft-ui-xaml/releases)

[NuGet 包下载](https://www.nuget.org/packages/Microsoft.UI.Xaml)

### <a name="new-features"></a>新功能

#### <a name="tabview"></a>TabView

![示例](../images/tabview-gif.gif)

#### <a name="description"></a>说明

TabView 控件是选项卡的集合，每个选项卡表示应用中的新页面或文档。 当应用有多个内容页面且用户希望能够添加、关闭和重新排列选项卡时，TabView 非常有用。 新的 [Windows 终端](https://github.com/Microsoft/Terminal)使用 TabView 来显示多个命令行界面。

#### <a name="documentation"></a>文档

https://docs.microsoft.com/uwp/api/microsoft.ui.xaml.controls.tabview?view=winui-2.2

#### <a name="navigationview-updates"></a>NavigationView 更新

##### <a name="a-navigationviews-back-button-update"></a>a) NavigationView 的后退按钮更新

![示例](../images/navigationview-back-button.gif)

##### <a name="description"></a>说明

在 NavigationView 的最小模式下，后退按钮不再消失。 打开和关闭窗格时，用户不再需要移动其光标来单击汉堡包按钮。 此功能默认启用。 无需进行任何代码更改即可启用它。

##### <a name="b-navigationview---no-auto-padding"></a>b) NavigationView - 无自动填充

![示例](../images/navigationview-no-auto-padding.png)

##### <a name="description"></a>说明

应用开发人员现在可以在使用 NavigationView 控件并将其扩展到标题栏区域时回收其应用窗口中的所有像素。

##### <a name="documentation"></a>文档

https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/navigationview#top-whitespace

#### <a name="visual-style-updates"></a>视觉样式更新

##### <a name="a-corner-radius-update"></a>a) 角半径更新

![示例](../images/corner-radius.png)

##### <a name="description"></a>说明

添加了 CornerRadius 特性。 已将默认控件更新为使用稍圆的角。 开发人员可以轻松地自定义角半径，根据需要为应用提供独特的外观。

##### <a name="github-spec-link"></a>GitHub 规范链接

https://github.com/microsoft/microsoft-ui-xaml/issues/524

##### <a name="b-border-thickness-update"></a>b) 边框粗细更新

![示例](../images/border-thickness.png)

##### <a name="description"></a>说明

BorderThickness 属性的自定义变得更加容易。 已更新默认控件，减少轮廓厚度，使其更简洁，呈现用户熟悉的外观。

##### <a name="github-spec-link"></a>GitHub 规范链接

https://github.com/microsoft/microsoft-ui-xaml/issues/835

##### <a name="c-button-visual-update"></a>c) 按钮视觉对象更新

![示例](../images/button-hover-visual-update.png)

##### <a name="description"></a>说明： 
已更新默认按钮的视觉对象，删除悬停期间显示的大纲，使其更清晰。

##### <a name="github-spec-link"></a>GitHub 规范链接：  
https://github.com/microsoft/microsoft-ui-xaml/issues/953

##### <a name="d-splitbutton-visual-update"></a>d) SplitButton 视觉对象更新

![示例](../images/splitbutton-visual-update.png)

##### <a name="description"></a>说明： 
已更新默认 SplitButton 的视觉对象，使其更加不同于 DropDownButton。

##### <a name="github-spec-link"></a>GitHub 规范链接： 
https://github.com/microsoft/microsoft-ui-xaml/issues/986

##### <a name="e-toggleswitch-visual-update"></a>e) ToggleSwitch 视觉对象更新

![示例](../images/toggleswitch-update.png)

##### <a name="description"></a>说明： 
默认 ToggleSwitch 的宽度从 44px 减少到 40px，使之在视觉上保持平衡，同时保持可用性。

##### <a name="github-spec-link"></a>GitHub 规范链接： 
https://github.com/microsoft/microsoft-ui-xaml/issues/836

##### <a name="f-checkbox-and-radiobutton-visual-update"></a>f) CheckBox 和 RadioButton 视觉对象更新

![示例](../images/checkbox-radiobutton.png)

##### <a name="description"></a>说明： 
CheckBox 和 RadioButton 视觉对象进行了更新，使之与其余的视觉样式更改保持一致。

##### <a name="github-spec-link"></a>GitHub 规范链接： 
https://github.com/microsoft/microsoft-ui-xaml/issues/839

## <a name="examples"></a>示例

Xaml 控件库示例应用包括介绍如何使用 WinUI 控件的交互式演示和示例代码。

* 从 [Microsoft Store](
https://www.microsoft.com/p/xaml-controls-gallery/9msvh128x2zt) 安装 XAML 控件库应用

* Xaml 控件库也[在 GitHub 上开源](
https://github.com/Microsoft/Xaml-Controls-Gallery)

## <a name="documentation"></a>文档

Windows UI 库控件的操作方法文章包含在[通用 Windows 平台控件文档](/windows/uwp/design/controls-and-patterns/)中。

API 参考文档位于此处：[Windows UI 库 API](/uwp/api/overview/winui/)。

## <a name="microsoftuixaml-22-version-history"></a>Microsoft.UI.Xaml 2.2 版本历史记录

### <a name="microsoftuixaml-22190702001-prerelease"></a>Microsoft.UI.Xaml 2.2.190702001-prerelease

2019 年 7 月

[GitHub 发布页](https://github.com/microsoft/microsoft-ui-xaml/releases/tag/v2.2.190702001-prerelease)

[NuGet 包下载](https://www.nuget.org/packages/Microsoft.UI.Xaml/2.2.190702001-prerelease)

### <a name="experimental-feature"></a>实验性功能

* [TabView](https://docs.microsoft.com/uwp/api/microsoft.ui.xaml.controls.tabview?view=winui-2.2)

### <a name="microsoftuixaml-2220190416001-prerelease"></a>Microsoft.UI.Xaml 2.2.20190416001-prerelease

2019 年 4 月

[GitHub 发布页](https://github.com/Microsoft/microsoft-ui-xaml/releases/tag/v2.2.190416008-prerelease)

[NuGet 包下载](https://www.nuget.org/packages/Microsoft.UI.Xaml/2.2.190416008-prerelease)

#### <a name="experimental-features"></a>实验性功能

* [FlowLayout](https://docs.microsoft.com/uwp/api/microsoft.ui.xaml.controls.flowlayout)

* [LayoutPanel](https://docs.microsoft.com/uwp/api/microsoft.ui.xaml.controls.layoutpanel)

* [RadioButtons](https://docs.microsoft.com/uwp/api/microsoft.ui.xaml.controls.radiobuttons)

* [ScrollViewer](https://docs.microsoft.com/uwp/api/microsoft.ui.xaml.controls.scrollviewer)
