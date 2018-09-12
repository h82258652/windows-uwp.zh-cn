---
author: QuinnRadich
title: 什么是 Windows 文档中 2018 年 5 月的新增功能-开发 UWP 应用
description: 新功能、 视频和开发人员指南已添加到 Windows 10 开发人员文档中的，2018 年 5 月针对和 Microsoft Build 会议。
keywords: 新增功能，更新，功能，开发人员指南，Windows 10 月，生成
ms.author: quradic
ms.date: 5/7/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
ms.localizationpriority: medium
ms.openlocfilehash: 322bc056411095019dfc027078cbfef7de0883fb
ms.sourcegitcommit: 2a63ee6770413bc35ace09b14f56b60007be7433
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/12/2018
ms.locfileid: "3932722"
---
# <a name="whats-new-in-the-windows-developer-docs-in-may-2018"></a>什么是 Windows 开发人员文档中 2018 年 5 月的新增功能

Windows 开发人员文档持续更新对整个 Windows 平台的开发人员提供的新功能的信息。 以下功能概述、 开发人员指南、 视频和示例已在五月以符合[Microsoft Build 2018](https://www.microsoft.com/build)开发者会议中可用。

只需在 Windows10 上[安装工具和 SDK](http://go.microsoft.com/fwlink/?LinkId=821431)，你便可以随时[创建新的通用 Windows 应用](../get-started/create-uwp-apps.md)，或了解如何使用 [Windows 上的现有应用代码](../porting/index.md)。

## <a name="features"></a>功能

### <a name="motion-in-fluent-design"></a>在 Fluent 设计的运动

在 Fluent 设计系统的运动用户正在发展，基于计时、 缓动、 方向性和引力的基础知识。 将应用这些基础功能将帮助指导用户在你的应用，以及用其数字体验连接通过反映自然世界。 了解此文章中的详细信息：

* [运动概述](../design/motion/index.md)已更新，以反映这些基础知识。
* [运动的实践](../design/motion/motion-in-practice.md)提供如何应用这些基础应用中的示例。
* [方向性和引力](../design/motion/directionality-and-gravity.md)固化你的应用的用户的心理模型。
* [计时和缓动](../design/motion/timing-and-easing.md)向你的应用中运动逼真效果。

![在操作中的运动](../design/motion/images/contextual.gif)

### <a name="fluent-design-updates"></a>Fluent Design 更新

已对以下 Fluent Design 页面视觉更新和次要更改：

* [对齐方式，填充、 边距](../design/layout/alignment-margin-padding.md)
* [Color](../design/style/color.md)
* [命令基础知识](../design/basics/commanding-basics.md)
* [适用于 Windows 应用的 fluent 设计](../design/fluent-design-system/index.md)
* [应用设计简介](../design/basics/design-and-ui-intro.md)
* [导航基础知识](../design/basics/navigation-basics.md)
* [响应式设计技术](../design/layout/responsive-design.md)
* [屏幕大小和断点](../design/layout/screen-sizes-and-breakpoints-for-responsive-design.md)
* [样式概述](../design/style/index.md)
* [写入样式](../design/style/writing-style.md)

此外，我们已重新编写使用他们的内容区域上的所有新信息的以下页面：

* [图标](../design/style/icons.md)现在提供实际建议使用图标，并使变成可点击状态。
* [版式](../design/style/typography.md)合并类似文章中的信息将所有内容放在一个位置的更新的指南和插图。

![颜色调色板图像](../design/style/images/color/accent-color-palette.svg)

### <a name="app-installer-files-in-visual-studio"></a>在 Visual Studio 中的应用安装程序文件

现在可以使用 Visual Studio 2017 更新 15.7 开始创建应用安装程序文件。 [了解如何使用 Visual Studio 创建应用安装程序文件](../packaging/create-appinstallerfile-vs.md)和启用自动更新向你的应用。 如果你遇到问题，请参阅[疑难解答的安装问题的应用安装程序文件](../packaging/troubleshoot-appinstaller-issues.md)以查看常见问题和解决方案。

### <a name="edge-webview-control-for-windows-forms-and-wpf-applications"></a>边缘 WebView 控件，用于 Windows 窗体和 WPF 应用程序

通过使用 WebView 控件，之前仅适用于 UWP 应用程序在桌面应用程序中显示 web 内容。 此控件使用 Microsoft Edge 的呈现引擎嵌入视图呈现丰富格式化 HTML 内容从远程 web 服务器、 动态生成的代码或内容文件。 查找 WebView 控件中的最新版本[Windows 社区工具包。](https://docs.microsoft.com/windows/uwpcommunitytoolkit/)

对于其他控件，如 WebView 在将来版本的 Windows 社区工具包的外观。 有关详细信息，请参阅[主机 UWP 控件在 WPF 和 Windows 窗体应用程序中。](https://docs.microsoft.com/windows/uwp/xaml-platform/xaml-host-controls)

### <a name="gaze-input-and-interactions"></a>凝视输入和交互

[根据用户眼睛的位置及移动，跟踪用户的凝视、注意和状态。](../design/input/gaze-interactions.md) 此功能强大的新方法，能够使用和与你的 UWP 应用交互是特别有用的辅助技术。 凝视输入还提供了游戏 （包括目标获取和跟踪） 和传统输入的设备 （键盘、 鼠标和触控） 不可用其他交互式场景具有吸引力的机会。

### <a name="msix-packaging-format"></a>MSIX 打包格式

宣布 Microsoft Build 2018 大会，MSIX 是适用于所有 Windows 应用程序包括 Win32、 Windows 窗体、 WPF 和 UWP 的新此包格式。 这一新格式继承 UWP 出色的功能：

* 可靠的安装和更新。 
* 管理与灵活功能系统安全模型。
* Microsoft 应用商店、 企业管理和支持许多自定义分发模型。

工具，用于创建这些程序包将在将来版本的 Visual Studio 和 Windows SDK 中提供。

MSIX 打包格式是一种开放源格式，以便我们的合作伙伴支持 MSIX 生态系统使用其工具和解决方案。 若要了解有关 MSIX 打包格式，请参阅[MSIX SDK](https://github.com/Microsoft/msix-packaging)。 

![MSIX 打包图像](images/msix.png)

### <a name="optional-packages-with-executable-code"></a>包含可执行代码的可选包

现在，你的应用中的可选包可以包含可执行 C# 代码。 [了解如何使用 Visual Studio 配置要支持主应用包的可选的加载项程序包。](../packaging/optional-packages-with-executable-code.md)

### <a name="page-transitions"></a>页面过渡

[页面过渡](../design/motion/page-transitions.md)将用户在应用中的页面之间导航。 这些设置有助于用户了解他们处于导航层次结构，并提供有关页面之间的关系的反馈。

### <a name="project-rome"></a>Project Rome

项目 rome 团队已检查其 iOS 和 Android 的 Sdk，添加新功能，如用户活动和重构很多代码以不同的 Sdk 提供一致的编程体验。 [所有新的 API 参考和操作方法文档](https://docs.microsoft.com/windows/project-rome/)将介绍实时 Build 2018 开发者会议期间。

### <a name="sets"></a>设置

集功能可用于 Windows Insider preview 版本。 使用集功能时，你的应用绘制到可能与其他应用，与具有自己的选项卡标题栏中的每个应用共享的窗口。 [针对集进行设计](../design/shell/design-for-sets.md)如何优化应用以提供最佳体验设置 UI 中的提供的指南。

## <a name="developer-guidance"></a>开发人员指南

### <a name="get-started"></a>入门

我们已大大增加我们 Get 启动较新的学习轨迹的内容。 这些新主题旨在向新的 Windows 10 开发人员提供他们可能想要完成一些常见任务的信息。 它们不是教程并不提供手持演练中，但改为指出现有文档所在以及如何使用它。 请查看改良[开始编写代码](../get-started/create-uwp-apps.md)页上，或了解每个单独的学习轨迹：

* [构建表单](../get-started/construct-form-learning-track.md)
* [以列表形式显示客户](../get-started/display-customers-in-list-learning-track.md)
* [保存并加载设置](../get-started/settings-learning-track.md)
* [处理文件](../get-started/fileio-learning-track.md)

![获取启动的映像](../get-started/images/build-your-app.png)

### <a name="advertising-performance-report"></a>广告性能报告

在开发人员中心仪表板中的[广告性能报告](../publish/advertising-performance-report.md)现在提供可见性指标。 我们还添加了[优化广告单元的可见性](../monetize/optimize-ad-unit-viewability.md)文章提供有关优化你的广告的可见性的建议。

### <a name="targeted-push-notifications"></a>定向的推送通知

在开发人员中心仪表板中的[通知](../publish/send-push-notifications-to-your-apps-customers.md)页面现在提供的图形和世界地图视图中所有通知的额外分析数据。

## <a name="videos"></a>视频

### <a name="cwinrt"></a>C++/WinRT

C + + /winrt 是创作和使用 Windows 运行时 Api 的新方法。 它已在标头文件中实现唯一，设计，为你提供对现代应用功能的一流访问。 [观看视频](https://www.youtube.com/watch?v=TLSul1XxppA&feature=youtu.be)以了解它的工作原理，然后[阅读开发人员文档](../cpp-and-winrt-apis/index.md)的详细信息。

### <a name="multi-instance-uwp-apps"></a>多实例 UWP 应用

Windows 现在可以使用每个单独进程中运行的 UWP 应用，多个实例。 [观看视频](https://www.youtube.com/watch?v=clnnf4cigd0&feature=youtu.be)以了解如何创建新的应用支持此功能，请[阅读开发人员文档](../launch-resume/multi-instance-uwp.md)为如何更多指南以及为什么要使用此功能。

## <a name="samples"></a>示例

### <a name="customer-database-tutorial"></a>客户数据库教程

本教程创建基本的 UWP 应用用于管理客户列表，并介绍了概念和企业开发中非常有用的做法。 它将指导你通过实现 UI 元素并添加针对本地 SQLite 数据库中，操作，并提供用于连接到远程的其余部分数据库，如果你想要继续进行操作的松散指南。 [请查看下面的教程](../enterprise/customer-database-tutorial.md)