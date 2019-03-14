---
title: 什么是 Windows 文档中 2018 年 5 月中的新增功能-开发 UWP 应用
description: 新功能、 视频和开发人员指南具有已添加到 2018 年 5 月的 Windows 10 开发人员文档和 Microsoft Build 大会。
keywords: 新增功能、 更新功能，开发人员指南，Windows 10，月，生成
ms.date: 05/07/2018
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 69df2bbe8bc91fcf4a2631c0f257fc44851c24f2
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2019
ms.locfileid: "57598782"
---
# <a name="whats-new-in-the-windows-developer-docs-in-may-2018"></a>什么是 Windows 开发人员文档中在 2018 年 5 月中的新增功能

Windows 开发人员文档持续更新对整个 Windows 平台的开发人员提供的新功能的信息。 以下功能概述、 开发人员指南、 视频和示例可供在五月逢[Microsoft Build 2018](https://www.microsoft.com/build)开发者大会。

只需在 Windows 10 上[安装工具和 SDK](https://go.microsoft.com/fwlink/?LinkId=821431)，你便可以随时[创建新的通用 Windows 应用](../get-started/create-uwp-apps.md)，或了解如何使用 [Windows 上的现有应用代码](../porting/index.md)。

## <a name="features"></a>功能

### <a name="motion-in-fluent-design"></a>Fluent 设计中的动作

在发展，运动 Fluent 设计系统中的用户，基于计时、 简化、 方向性和重力的基础知识。 应用这些基础知识有助于指导用户完成您的应用程序，并将其与他们的数字体验连接通过专用于反映将自然的世界。 了解在此文章中的详细信息：

* [Motion 概述](../design/motion/index.md)已更新以反映这些基础知识。
* [Motion 的实践](../design/motion/motion-in-practice.md)示例展示了如何以应用在应用内的这些基础知识。
* [方向性和重力](../design/motion/directionality-and-gravity.md)固化您的应用程序的用户的思维模式。
* [计时并降低了](../design/motion/timing-and-easing.md)将真实性添加到您的应用程序中的动作。

![在操作中的动作](../design/motion/images/contextual.gif)

### <a name="fluent-design-updates"></a>Fluent 设计更新

已对以下 Fluent 设计页面视觉对象更新和细微的更改：

* [对齐方式，填充边距](../design/layout/alignment-margin-padding.md)
* [Color](../design/style/color.md)
* [命令基础知识](../design/basics/commanding-basics.md)
* [Windows 应用的 Fluent 设计](../design/fluent-design-system/index.md)
* [应用程序设计简介](../design/basics/design-and-ui-intro.md)
* [导航基础知识](../design/basics/navigation-basics.md)
* [响应式设计技术](../design/layout/responsive-design.md)
* [屏幕大小和断点](../design/layout/screen-sizes-and-breakpoints-for-responsive-design.md)
* [样式概述](../design/style/index.md)
* [编写样式](../design/style/writing-style.md)

此外，我们已重写与他们的内容区域的所有新信息的以下页面：

* [图标](../design/style/icons.md)现在提供实用的建议使用的图标和使其成为可单击。
* [版式](../design/style/typography.md)整合了相似的文章，将所有内容放在一个位置更新的指南和插图中的信息。

![颜色的调色板图像](../design/style/images/color/accent-color-palette.svg)

### <a name="app-installer-files-in-visual-studio"></a>Visual Studio 中的应用安装程序文件

现在可以使用 Visual Studio 2017 时，更新 15.7 创建应用安装程序文件。 [了解如何使用 Visual Studio 创建的应用安装程序文件](../packaging/create-appinstallerfile-vs.md)和启用自动更新为你的应用。 如果遇到问题的过程中，请参阅[排除安装问题与应用安装程序文件](../packaging/troubleshoot-appinstaller-issues.md)查看常见问题和解决方案。

### <a name="edge-webview-control-for-windows-forms-and-wpf-applications"></a>Edge web 视图控件可用于 Windows 窗体和 WPF 应用程序

通过使用 WebView 控件，以前只可用于 UWP 应用程序在桌面应用程序中显示 web 内容。 此控件使用 Microsoft Edge 呈现引擎将嵌入呈现格式丰富 HTML 的视图中的远程 web 服务器、 动态生成的代码或内容文件的内容。 查找中的最新版本的 WebView 控件[Windows 社区工具包。](https://docs.microsoft.com/windows/uwpcommunitytoolkit/)

Web 视图在将来的版本的 Windows 社区工具包正如其他控件的外观。 有关详细信息，请参阅[主机 UWP 控件在 WPF 和 Windows 窗体应用程序中。](https://docs.microsoft.com/windows/uwp/xaml-platform/xaml-host-controls)

### <a name="gaze-input-and-interactions"></a>注视输入和交互

[跟踪用户的视线移动、 关注和基于位置和移动用户眼存在。](../design/input/gaze-interactions.md) 此功能强大的新方法来使用和 UWP 应用中与之交互是作为辅助技术特别有用。 注视输入还提供了极具吸引力的游戏 （包括目标获取和跟踪） 和其他交互式场景，在传统的输入的设备 （键盘、 鼠标、 触摸） 不可用的机会。

### <a name="msix-packaging-format"></a>MSIX 打包格式

在 Microsoft Build 2018 大会上所宣布的 MSIX 是适用于所有 Windows 应用程序包括 Win32、 Windows 窗体、 WPF 和 UWP 的新容器化包格式。 这一新格式从 UWP 继承强大的功能：

* 可靠的安装和更新。 
* 管理与灵活功能系统的安全模型。
* 支持 Microsoft Store、 企业管理和自定义分布的多个模型。

工具创建这些包将在 Visual Studio 和 Windows SDK 的未来版本中可用。

MSIX 打包格式是一种开放源代码格式轻松我们的合作伙伴能够支持其工具和解决方案的 MSIX 生态系统。 若要了解有关 MSIX 打包格式的详细信息，请参阅[MSIX SDK](https://github.com/Microsoft/msix-packaging)。 

![MSIX 打包图像](images/msix.png)

### <a name="optional-packages-with-executable-code"></a>包含可执行代码的可选包

应用程序中的可选包现在可包含可执行文件C#代码。 [了解如何使用 Visual Studio 配置可选的附加包以支持你的主要应用程序包。](../packaging/optional-packages-with-executable-code.md)

### <a name="page-transitions"></a>页面过渡

[页面过渡](../design/motion/page-transitions.md)导航应用中的页面之间的用户。 它们可帮助用户了解他们的导航层次结构中的位置，并提供有关页面之间的关系的反馈。

### <a name="project-rome"></a>Project Rome

IOS 和 Android Sdk，添加新功能，例如用户活动和重构很多的代码来跨不同的 Sdk 提供一致的编程体验，已大幅改进项目罗马团队。 [所有新的 API 参考和操作方法文档](https://docs.microsoft.com/windows/project-rome/)Build 2018 开发者大会期间将会推出。

### <a name="sets"></a>集

设置功能现已推出 Windows Insider preview 内部版本。 使用集功能时，您应用程序绘制到可能与其他应用使用，具有自己的选项卡的标题栏中的每个应用共享的窗口。 

## <a name="developer-guidance"></a>开发人员指南

### <a name="get-started"></a>立即开始行动

我们已大大增加我们获取启动内容与新的学习路径。 这些新主题的目标是新的 Windows 10 开发人员提供他们可能需要完成一些常见的任务的信息。 它们无法教程并不提供手持演练中，但而是指出其中存在现有文档，以及如何使用它。 请查看改进[开始编码](../get-started/create-uwp-apps.md)页上，也可以浏览每个单独的学习轨迹：

* [构建窗体](../get-started/construct-form-learning-track.md)
* [显示在列表中的客户](../get-started/display-customers-in-list-learning-track.md)
* [保存和加载设置](../get-started/settings-learning-track.md)
* [使用的文件](../get-started/fileio-learning-track.md)

![获取已启动的映像](../get-started/images/build-your-app.png)

### <a name="advertising-performance-report"></a>广告性能报告

[广告性能报告](../publish/advertising-performance-report.md)在合作伙伴中心现在提供在工作场所内度量值。 我们还添加了[优化的广告单元可视性](../monetize/optimize-ad-unit-viewability.md)文章提供有关优化您的广告的可视性的建议。

### <a name="targeted-push-notifications"></a>定向推送通知

[通知](../publish/send-push-notifications-to-your-apps-customers.md)页在合作伙伴中心现在提供额外的分析数据的图形和世界地图视图中所有通知。

## <a name="videos"></a>视频

### <a name="cwinrt"></a>C++/WinRT

C + + / WinRT 是创作和使用 Windows 运行时 Api 的新方法。 它在标头文件中实现 sole 并能满足您的现代应用程序功能的优先访问权限。 [观看视频](https://www.youtube.com/watch?v=TLSul1XxppA&feature=youtu.be)若要了解其工作原理，然后[阅读开发人员文档](../cpp-and-winrt-apis/index.md)的详细信息。

### <a name="multi-instance-uwp-apps"></a>多实例 UWP 应用

Windows 现在可以使用其自己的单独进程中的每个运行的 UWP 应用，多个实例。 [观看视频](https://www.youtube.com/watch?v=clnnf4cigd0&feature=youtu.be)若要了解如何创建新的应用程序支持此功能，然后[阅读开发人员文档](../launch-resume/multi-instance-uwp.md)更多指导如何和为何要使用此功能。

## <a name="samples"></a>示例

### <a name="customer-database-tutorial"></a>客户数据库教程

本教程中创建用于管理的客户，列表的基本 UWP 应用，并介绍一些概念和实践在企业开发非常有用。 它将指导你完成实现 UI 元素并将添加对本地 SQLite 数据库中，执行的操作，并提供用于连接到远程的其余部分数据库，如果你想要进一步松散指导。 [请查阅此处教程](../enterprise/customer-database-tutorial.md)