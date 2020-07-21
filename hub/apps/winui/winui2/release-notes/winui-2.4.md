---
title: WinUI 2.4 发行说明
description: WinUI 2.4（包括新功能和 Bug 修复）的发行说明。
ms.date: 07/15/2020
ms.topic: reference
ms.openlocfilehash: 22fd028ba2059a092ee2f2be47a114fb2d618ce1
ms.sourcegitcommit: c1226b6b9ec5ed008a75a3d92abb0e50471bb988
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/20/2020
ms.locfileid: "86492822"
---
# <a name="windows-ui-library-24"></a>Windows UI 库 2.4

WinUI 2.4 是 Windows UI 库 (WinUI) 的最新官方版本。

WinUI 是托管在 GitHub 的 [Windows UI 库存储库](https://aka.ms/winui)中的开源项目。 请在此存储库中注册所有 Bug 报告、提交功能请求和贡献社区代码。

WinUI 版本：[GitHub 发布页](https://github.com/microsoft/microsoft-ui-xaml/releases)

可以通过 NuGet 包管理器将 WinUI 包添加到 Visual Studio 项目中。 有关详细信息，请参阅 [Windows UI 库入门](../getting-started.md)。

NuGet 包下载：[Microsoft.UI.Xaml](https://www.nuget.org/packages/Microsoft.UI.Xaml)

## <a name="new-features"></a>新功能

### <a name="radialgradientbrush"></a>RadialGradientBrush

RadialGradientBrush 在椭圆内绘制，该椭圆由 Center、RadiusX 和 RadiusY 属性定义。 渐变的颜色开始于椭圆的中心之处，结束于半径之处。

![径向渐变画笔](../images/radialgradientbrush.gif)<br>
*径向渐变画笔*

[使用准则](/windows/uwp/design/style/brushes#radial-gradient-brushes)

[API 参考](/uwp/api/microsoft.ui.xaml.media.radialgradientbrush)

### <a name="progressring"></a>ProgressRing

ProgressRing 控件用于模式交互，其中用户会受到阻止，直到 ProgressRing 消失。 如果操作要求在该操作完成之前挂起与应用的大多数交互，请使用此控件。

![ProgressRing 控件](../images/progressring.gif)<br>
*ProgressRing 控件*

[使用准则](/windows/uwp/design/controls-and-patterns/progress-controls)

[API 参考](/uwp/api/microsoft.ui.xaml.controls.progressring)

### <a name="tabview-updates"></a>TabView 更新

TabView 控件更新提供了对选项卡呈现方式的更多控制。

可以设置未选定选项卡的宽度，并仅显示一个图标来节省屏幕空间：

![TabView 控件 - 选项卡大小](..\images\tabview-sizing.gif)<br>
*TabView 控件 - 选项卡大小*

还可以隐藏未选定选项卡上的“关闭”按钮，直到用户将鼠标悬停在该选项卡上（在以前的版本中会始终显示该按钮）：

![TabView 控件 - 悬停以关闭](..\images\tabview-closebuttononhover.gif)<br>
*TabView 控件 - 悬停以关闭*

[使用准则](/windows/uwp/design/controls-and-patterns/tab-view)

[API 参考](/uwp/api/microsoft.ui.xaml.controls.tabview)

### <a name="dark-theme-updates-to-textbox-family-of-controls"></a>对 TextBox 系列控件的深色主题更新

如果启用了深色主题，则 TextBox 系列控件的背景色会在文本插入时默认保持深色（在以前的版本中，背景色在文本插入过程中会变为白色）。

| 之前 | 调整后的文本 |
| - | - |
| ![TextBox 深色主题更新（之前）](..\images\textbox-darkthemeupdates-before1.gif)<br>*TextBox 深色主题更新（之前）* | ![TextBox 深色主题更新（之后）](..\images\textbox-darkthemeupdates-after1.gif)<br>*TextBox 深色主题更新（之后）* |
| ![TextBox 深色主题更新（之前）](..\images\textbox-darkthemeupdates-before2.gif)<br>*TextBox 深色主题更新（之前）* | ![TextBox 深色主题更新（之后）](..\images\textbox-darkthemeupdates-after2.gif)<br>*TextBox 深色主题更新（之后）* |

下面是 TextBox 系列控件中包含的一些控件：

- [TextBox](/uwp/api/windows.ui.xaml.controls.textbox)
- [RichEditBox](/uwp/api/windows.ui.xaml.controls.richtextblock)
- [PasswordBox](/uwp/api/windows.ui.xaml.controls.passwordbox)
- [可编辑的 ComboBox](/uwp/api/windows.ui.xaml.controls.combobox)
- [AutoSuggestBox](/uwp/api/windows.ui.xaml.controls.autosuggestbox)

### <a name="hierarchical-navigation"></a>分层导航

[NavigationView](/uwp/api/microsoft.ui.xaml.controls.navigationview?view=winui-2.4) 控件现在支持分层导航，并包括 Left、Top 和 LeftCompact 显示模式。 对于显示页面类别、标识带有相关子页面的页面，或在具有链接到许多其他页面的中心式页面的应用内使用，分层的 NavigationView 非常有用。

![分层 NavigationView 控件](..\images\HierarchicalNavView.gif)<br>*分层 NavigationView 控件*

[使用准则](/windows/uwp/design/controls-and-patterns/navigationview#hierarchical-navigation)

[API 参考](/uwp/api/microsoft.ui.xaml.controls.navigationview)

## <a name="samples"></a>示例

**XAML 控件库**中提供了每个所述 WinUI 2.4 功能的示例。

如果尚未安装 XAML 控件库应用，请从 [Microsoft Store](https://www.microsoft.com/p/xaml-controls-gallery/9msvh128x2zt) 获取它。

还可以在 [GitHub](https://github.com/Microsoft/Xaml-Controls-Gallery)上查看 XAML 控件库源代码。
