---
title: WinUI 2.3 发行说明
description: WinUI 2.3 的发行说明，包括新功能和 Bug 修复。
ms.date: 04/15/2020
ms.topic: article
ms.openlocfilehash: a61932a6f0060a4be79424e02aad3dd312128aef
ms.sourcegitcommit: d0f479f1955881afb62c2af249db5d0b053b63e5
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/19/2020
ms.locfileid: "83580454"
---
# <a name="windows-ui-library-23"></a>Windows UI 库 2.3

WinUI 2.3 是 Windows UI 库 (WinUI) 的最新官方版本。

WinUI 是托管在 GitHub 的 [Windows UI 库存储库](https://aka.ms/winui)中的开源项目。 请在此存储库中注册所有 Bug 报告、提交功能请求和贡献社区代码。

WinUI 版本：[GitHub 发布页](https://github.com/microsoft/microsoft-ui-xaml/releases)

可以通过 NuGet 包管理器将 WinUI 包添加到 Visual Studio 项目中。 有关详细信息，请参阅 [Windows UI 库入门](../getting-started.md)。

NuGet 包下载：[Microsoft.UI.Xaml](https://www.nuget.org/packages/Microsoft.UI.Xaml)

## <a name="new-features"></a>新功能

### <a name="progress-bar-visual-refresh"></a>进度栏视觉刷新

**ProgressBar** 有两个不同的视觉对象表示形式。

#### <a name="indeterminate-progress-bar"></a>不确定的进度栏

显示任务正在进行，但不会阻止用户交互。

![不确定的进度栏](../images/IndeterminateProgressBar.gif)

#### <a name="determinate-progress-bar"></a>确定的进度栏

显示工作量已知情况下的进度。 

![确定的进度栏](../images/DeterminateProgressBar.gif)

[文档链接](https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/progress-controls)

[示例链接](https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/progress-controls#examples)

### <a name="numberbox"></a>NumberBox

**NumberBox** 表示一个可以用来显示和编辑数字的控件。 它支持验证、递增步进以及对基本等式进行内联计算，例如加减乘除。

![NumberBox](../images/NumberBoxGif.gif)

[文档和示例链接](https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/number-box)

### <a name="radiobuttons"></a>RadioButtons

**RadioButtons** 是一个新的容器控件，可以让你轻松地创建相关的 RadioButton 元素组，同时还能正确地支持键盘操作和讲述人/屏幕阅读器功能

![RadioButtons](../images/RadioButtons.png)

[文档和示例链接](https://github.com/microsoft/microsoft-ui-xaml-specs/blob/c8d3d3668af546091656dfc37436b13cd062f52d/active/radiobuttons/RadioButtons_Spec.md)

## <a name="examples"></a>示例

Xaml 控件库示例应用包括介绍如何使用 WinUI 控件的交互式演示和示例代码。

* 从 [Microsoft Store](
https://www.microsoft.com/p/xaml-controls-gallery/9msvh128x2zt) 安装 XAML 控件库应用

* Xaml 控件库也[在 GitHub 上开源](
https://github.com/Microsoft/Xaml-Controls-Gallery)

## <a name="documentation"></a>文档

Windows UI 库控件的操作方法文章包含在[通用 Windows 平台控件文档](/windows/uwp/design/controls-and-patterns/)中。

API 参考文档位于此处：[Windows UI 库 API](/uwp/api/overview/winui/)。
