---
description: 获取有关向 Windows 应用添加控件和模式的设计指南和编码说明。 为你找到了超过 45 种功能强大的控件，这些控件可与你的应用结合使用。
title: Windows 控件和模式 - Windows 应用开发
keywords: UWP 控件, 用户界面, 应用控件, Windows 控件
label: Controls & patterns
template: detail.hbs
ms.date: 03/23/2020
ms.topic: article
ms.assetid: ce2e611c-c419-4a14-9095-b88ac711d1b8
ms.localizationpriority: medium
ms.openlocfilehash: 63907b3bfe3fc6dece900f1a7c09ac535e859471
ms.sourcegitcommit: c660def841abc742600fbcf6ed98e1f4f7beb8cc
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/24/2020
ms.locfileid: "80218447"
---
# <a name="controls-for-windows-apps"></a>适用于 Windows 应用的控件

![控制](../images/controls-2x.png)

在 Windows 应用开发中，控件<i></i>是用于显示内容或支持交互的 UI 元素。 控件是用户界面的构建基块。 <i>模式</i>是合并多个控件来创造新内容的一种方式。

我们提供了超过 45 种控件供你使用，范围从简单按钮到网格视图之类的功能强大的数据控件。  这些控件是 Fluent Design System 的一部分，并且可以帮助你创建一个加粗、可缩放的 UI，此 UI 在所有设备和屏幕大小上都具有出色的外观。

本部分中的文章提供了有关向 Windows 应用添加控件和模式的设计指南和编码说明。

## <a name="intro"></a>简介

有关在 XAML 和 C# 中添加控件和设置其样式的常规说明和代码示例。

:::row:::
    :::column:::
      <p><b><a href="controls-and-events-intro.md">添加控件并处理事件</a></b> <br/>
向应用中添加控件有 3 个关键步骤：将控件添加到应用 UI、在控件上设置属性，然后将代码添加到控件的事件处理程序，以使其执行某个操作。</p>
    :::column-end:::
    :::column:::
      <p><b><a href="xaml-styles.md">设置控件样式</a></b> <br/>
可以使用 XAML 框架通过多种方式自定义应用的外观。 通过样式可以设置控件属性，并重复使用这些设置，以便保持多个控件具有一致的外观。</p>
    :::column-end:::
:::row-end:::

## <a name="get-the-windows-ui-library"></a>获取 Windows UI 库

|  |  |
| - | - |
| ![WinUI 徽标](images/winui-logo-64x64.png) | 某些控件仅在 Windows UI 库 (WinUI) 中提供，该库是一个包含新控件和 UI 功能的 NuGet 包。 若要获取它，请参阅 [Windows UI 库概述和安装说明](/uwp/toolkits/winui/)。<br/>从 WinUI 2.2 始，许多控件的默认样式已更新为使用圆角。 有关详细信息，请参阅[圆角半径](/windows/uwp/design/style/rounded-corner)。 |

## <a name="alphabetical-index"></a>按字母顺序排序的索引

有关特定控件和模式的详细信息。 （有关按功能排序的列表，请参阅[按功能排序的控件索引](controls-by-function.md)。）

:::row:::
    :::column:::

- 动画视觉播放器（请参阅 [Lottie](/windows/communitytoolkit/animations/lottie)）![WinUI 徽标](images/winui-logo-16x16.png)
- [自动建议框](auto-suggest-box.md)
- [Button](buttons.md)
- [日历日期选取器](calendar-date-picker.md)
- [日历视图](calendar-view.md)
- [复选框](checkbox.md)
- [颜色选取器](color-picker.md) ![WinUI 徽标](images/winui-logo-16x16.png)
- [组合框](combo-box.md)
- [命令栏](app-bars.md)
- [命令栏浮出控件](command-bar-flyout.md) ![WinUI 徽标](images/winui-logo-16x16.png)
- [联系人卡片](contact-card.md)
- [内容对话框](dialogs-and-flyouts/dialogs.md)
- [内容链接](content-links.md)
- [上下文菜单](menus.md)
- [日期选取器](date-picker.md)
- [对话框和浮出控件](dialogs-and-flyouts/index.md)
- [下拉按钮](buttons.md#create-a-drop-down-button) ![WinUI 徽标](images/winui-logo-16x16.png)
- [翻转视图](flipview.md)
- [浮出控件](dialogs-and-flyouts/flyouts.md)
- [窗体](forms.md)（模式）
- [网格视图](listview-and-gridview.md)
- [超链接](hyperlinks.md)
- [超链接按钮](hyperlinks.md#create-a-hyperlinkbutton)
- [图像和图像画笔](images-imagebrushes.md)
- [墨迹书写控件](inking-controls.md)
- [列表视图](listview-and-gridview.md)
- [地图控件](../../maps-and-location/controls-map.md)
- [大纲/细节](master-details.md)（模式）
- [媒体播放](media-playback.md)
- [菜单栏](menus.md#create-a-menu-bar) ![WinUI 徽标](images/winui-logo-16x16.png)
- [菜单浮出控件](menus.md)
- [导航视图](navigationview.md) ![WinUI 徽标](images/winui-logo-16x16.png)

    :::column-end:::
    :::column:::

- [数字框](number-box.md) ![WinUI 徽标](images/winui-logo-16x16.png)
- [视差视图](..\motion\parallax.md) ![WinUI 徽标](images/winui-logo-16x16.png)
- [密码框](password-box.md)
- [头像图片](person-picture.md) ![WinUI 徽标](images/winui-logo-16x16.png)
- [透视表](pivot.md)
- [进度栏](progress-controls.md) ![WinUI 徽标](images/winui-logo-16x16.png)
- [进度环](progress-controls.md)
- [单选按钮](radio-button.md)
- [评分控件](rating.md) ![WinUI 徽标](images/winui-logo-16x16.png)
- [重复按钮](buttons.md#create-a-repeat-button)
- [Rich Edit 框](rich-edit-box.md)
- [RTF 块](rich-text-block.md)
- [滚动查看器](scroll-controls.md)
- [搜索](search.md)（模式）
- [语义式缩放](semantic-zoom.md)
- [形状](shapes.md)
- [滑块](slider.md)
- [拆分按钮](buttons.md#create-a-split-button) ![WinUI 徽标](images/winui-logo-16x16.png)
- [拆分视图](split-view.md)
- [轻扫控件](swipe.md) ![WinUI 徽标](images/winui-logo-16x16.png)
- [选项卡视图](tab-view.md) ![WinUI 徽标](images/winui-logo-16x16.png)
- [教学提示](dialogs-and-flyouts/teaching-tip.md) ![WinUI 徽标](images/winui-logo-16x16.png)
- [文本块](text-block.md)
- [文本框](text-box.md)
- [时间选取器](time-picker.md)
- [切换开关](toggles.md)
- [切换按钮](buttons.md)
- [切换拆分按钮](buttons.md#create-a-toggle-split-button)
- [工具提示](tooltips.md)
- [树视图](tree-view.md) ![WinUI 徽标](images/winui-logo-16x16.png)
- [双窗格视图](two-pane-view.md) ![WinUI 徽标](images/winui-logo-16x16.png)
- [Web 视图](web-view.md)

    :::column-end:::
:::row-end:::




## <a name="xaml-controls-gallery"></a>XAML 控件库

从 Microsoft Store 获取 _XAML 控件库_ 应用，了解这些控件和 Fluent Design System 的实际应用。 该应用是该网站的交互式助手。 安装它后，你可以使用各控件页上的链接启动该应用并查看控件的实际应用。

<a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">获取 XAML 控件库应用 (Microsoft Store)</a>

<a href="https://github.com/Microsoft/Xaml-Controls-Gallery">获取源代码 (GitHub)</a>

<img src="images/xaml-controls-gallery.png" alt="XAML Controls Gallery screen" />

## <a name="additional-controls"></a>其他控件

可以从 <a href="https://www.telerik.com/">Telerik</a>、<a href="https://www.syncfusion.com/uwp-ui-controls">SyncFusion</a>、<a href="https://www.devexpress.com/Products/NET/Controls/Win10Apps/">DevExpress</a>、<a href="https://www.infragistics.com/products/universal-windows-platform">Infragistics</a>、<a href="https://www.componentone.com/Studio/Platform/UWP">ComponentOne</a>、<a href="https://www.actiprosoftware.com/products/controls/universal">ActiPro</a> 等公司获取用于 Windows 开发的其他控件。 这些控件通过使用自定义控件和服务扩展标准系统控件来为企业和 .NET 开发人员提供额外的支持。
