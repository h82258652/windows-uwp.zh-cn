---
title: WinUI 2.1 发行说明
description: WinUI 2.1 的发行说明，包括新功能和 Bug 修复。
ms.date: 07/15/2020
ms.topic: article
ms.openlocfilehash: f362c4ae7654d6ef3b888b908c4779fce62af5fe
ms.sourcegitcommit: c1226b6b9ec5ed008a75a3d92abb0e50471bb988
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/20/2020
ms.locfileid: "86493082"
---
# <a name="windows-ui-library-21"></a>Windows UI 库 2.1

最新官方版 Windows UI 库 – WinUI 2.1 – 刚于 2019 年 4 月 8 日发布。 

WinUI 提供许多最新的 Windows UX 平台功能，其中包括最新的 Fluent 控件和样式，以可供用户立即使用的方式发布，并且后向兼容 Windows 10 周年更新 (14393)。 [XAML 控件库](https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/#xaml-controls-gallery)提供了一些示例，供你探索添加到库中的所有酷新功能。

下载 [WinUI 2.1 NuGet 包](https://www.nuget.org/packages/Microsoft.UI.Xaml/2.1.190405004)

可以通过 NuGet 包管理器选择使用应用中的 WinUI 包：有关详细信息，请参阅 [Windows UI 库入门](https://docs.microsoft.com/uwp/toolkits/winui/getting-started)。

WinUI 是托管在 GitHub 上的开源项目。 我们欢迎你在 [Windows UI 库存储库](https://aka.ms/winui)中提供 Bug 报告、提交功能请求和贡献社区代码。

## <a name="whats-new-in-this-release"></a>此版本的新增功能

### <a name="itemsrepeater"></a>ItemsRepeater

通过 ItemsRepeater 创建使用灵活布局系统、自定义视图和虚拟化的自定义集合体验。
与 ListView 不同，ItemsRepeater 不提供综合性的最终用户体验 - 它没有默认 UI，不提供任何围绕焦点、选择或用户交互的策略， 而是一个构建基块，你可以使用它来创建自己的基于集合的独特体验和自定义控件。 它支持生成更丰富且性能更高的体验。

![示例](../images/ItemsRepeater%20-%20MSN%20News.gif)

[文档](https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/items-repeater)

### <a name="animatedvisualplayer"></a>AnimatedVisualPlayer

AnimatedVisualPlayer 用于承载和控制动画视觉对象的播放，使你可以向应用添加高性能自定义运动图形。 例如，AnimatedVisualPlayer 用于显示和控制 Lottie 动画。

![示例](../images/AnimatedVisualPlayerUpdated.gif)

[文档](https://docs.microsoft.com/windows/communitytoolkit/animations/lottie)

### <a name="teachingtip"></a>TeachingTip

TeachingTip 为应用程序提供了一种吸引人的 Fluent 方法，通过非侵入性的内容丰富的提示为用户提供指导和知识。 TeachingTip 根据上下文提供与你手头的任务相关的信息，可以让你将重点放在新功能或重要功能上，可以告知用户如何执行任务，同时还能增强工作流。

![示例](../images/TeachingTipUpdated.gif)

[文档](https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/dialogs-and-flyouts/teaching-tip)

### <a name="radiomenuflyoutitem"></a>RadioMenuFlyoutItem

包括在 MenuBar 中提供“单选按钮”样式选项的功能。 这样就可以将包含项目符号的选项分组。这些选项绑定到一起，就像一个单选按钮组一样。 将为开发人员处理逻辑。

![示例](../images/RadioMenuFlyoutItem1.png)

[文档](https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/menus#create-a-menu-flyout-or-a-context-menu)

### <a name="compactdensity"></a>CompactDensity

紧密模式使开发人员能够为任意数量的方案创建舒适的体验。 只需添加一个资源字典，应用程序就可以适应 UI 平均增加 ~ 33% 的情况。

![压实密度示例](../images/CompactDensityUpdated.png)

[文档](https://docs.microsoft.com/windows/uwp/design/style/spacing )

### <a name="shadows"></a>阴影

![示例](../images/shadow.gif)

在 UI 中创建元素的视觉层次结构可以使 UI 易于扫描并传达需要重点关注的信息。 提升（将 UI 的所选元素推向用户的操作）通常用于在软件中实现此类层次结构。 

使用 Windows 10 2019 年 5 月更新时，许多常见控件都在默认情况下通过景深和阴影来增加提升效果。 WinUI 2.1 中的 NavigationView 和 TeachingTip 控件在使用 Windows 10 2019 年 5 月更新的 OS 上运行时，也会有默认阴影。 在 Windows 10 2019 年 5 月更新发布后，我们会提供具有默认阴影的控件的完整列表并介绍如何使用其他 API。链接会在此处发布。

## <a name="examples"></a>示例

Xaml 控件库示例应用包括介绍如何使用 WinUI 控件的交互式演示和示例代码。

* 从 [Microsoft Store](
https://www.microsoft.com/p/xaml-controls-gallery/9msvh128x2zt) 安装 XAML 控件库应用

* Xaml 控件库也[在 GitHub 上开源](
https://github.com/Microsoft/Xaml-Controls-Gallery)

## <a name="documentation"></a>文档

Windows UI 库控件的操作方法文章包含在[通用 Windows 平台控件文档](/windows/uwp/design/controls-and-patterns/)中。

API 参考文档位于此处：[Windows UI 库 API](/uwp/api/overview/winui/)。

## <a name="microsoftuixaml-21-version-history"></a>Microsoft.UI.Xaml 2.1 版本历史记录

### <a name="microsoftuixaml-21-official-release"></a>Microsoft.UI.Xaml 2.1 官方版本

2019 年 4 月

[GitHub 发布页](https://github.com/Microsoft/microsoft-ui-xaml/releases)

[NuGet 包下载](https://www.nuget.org/packages/Microsoft.UI.Xaml/2.1.190405004)

#### <a name="new-feature-not-included-in-earlier-pre-releases"></a>新功能（不包含在早期的预发行版本中）

* [CompactDensity](https://docs.microsoft.com/windows/uwp/design/style/spacing)：紧密模式使开发人员能够为任意数量的方案创建舒适的体验。 只需添加一个资源字典，应用程序就可以适应 UI 平均增加 ~ 33% 的情况。

* 阴影：在 UI 中创建元素的视觉层次结构可以使 UI 易于扫描并传达需要重点关注的信息。 提升（将 UI 的所选元素推向用户的操作）通常用于在软件中实现此类层次结构。 许多常见控件都在默认情况下通过景深和阴影来增加提升效果。  

### <a name="microsoftuixaml-21190218001-prerelease"></a>Microsoft.UI.Xaml 2.1.190218001-prerelease

2019 年 2 月

[GitHub 发布页](https://github.com/Microsoft/microsoft-ui-xaml/releases/tag/v2.1.190219001-prerelease)

[NuGet 包下载](https://www.nuget.org/packages/Microsoft.UI.Xaml/2.1.190218001-prerelease)

新的实验性功能：

* [TeachingTip 控件](https://github.com/Microsoft/microsoft-ui-xaml/issues/21)  
  有了这个新控件，应用就可以通过非侵入性的内容丰富的通知为应用程序中的用户提供指导和知识。 TeachingTip 根据上下文提供与用户手头的任务相关的信息，可以用来让用户将重点放在新功能或重要功能上，可以告知用户如何执行任务，或者增强用户工作流。

### <a name="microsoftuixaml-21190131001-prerelease"></a>Microsoft.UI.Xaml 2.1.190131001-prerelease

2019 年 2 月

[GitHub 发布页](https://github.com/Microsoft/microsoft-ui-xaml/releases/tag/v2.1.190131001-prerelease)

[NuGet 包下载](https://www.nuget.org/packages/Microsoft.UI.Xaml/2.1.190131001-prerelease)

新的实验性功能：

* [AnimatedVisualPlayer](https://docs.microsoft.com/uwp/api/microsoft.ui.xaml.controls.animatedvisualplayer)  
  此新控件可用于播放复杂的高性能矢量动画，包括使用 [Lottie-Windows](https://docs.microsoft.com/windows/communitytoolkit/animations/lottie) 创建的 [Lottie](https://github.com/airbnb/lottie) 动画。

### <a name="microsoftuixaml-21181217001-prerelease"></a>Microsoft.UI.Xaml 2.1.181217001-prerelease

2018 年 12 月

[GitHub 发布页](https://github.com/Microsoft/microsoft-ui-xaml/releases/tag/v2.1.181217001-prerelease)

[NuGet 包下载](https://www.nuget.org/packages/Microsoft.UI.Xaml/2.1.181217001-prerelease)

新的实验性功能：

* [ItemsRepeater](https://docs.microsoft.com/uwp/api/microsoft.ui.xaml.controls.itemsrepeater)

* [RadioButtons](https://docs.microsoft.com/uwp/api/microsoft.ui.xaml.controls.radiobuttons)

* [RadioMenuFlyoutItem](https://docs.microsoft.com/uwp/api/microsoft.ui.xaml.controls.radiomenuflyoutitem)
