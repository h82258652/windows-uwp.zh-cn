---
title: 2018 年 5 月 Windows 文档中的新增功能 - 开发 UWP 应用
description: 新的功能、视频和开发人员指南已添加到 2018 年 5 月版和 Microsoft Build 大会的 Windows 10 开发人员文档。
keywords: 新增功能, 更新, 功能, 开发人员指南, Windows 10, 5 月, 内部版本
ms.date: 05/07/2018
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 69df2bbe8bc91fcf4a2631c0f257fc44851c24f2
ms.sourcegitcommit: aaa4b898da5869c064097739cf3dc74c29474691
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/13/2019
ms.locfileid: "63805877"
---
# <a name="whats-new-in-the-windows-developer-docs-in-may-2018"></a>2018 年 5 月 Windows 开发人员文档中的新增功能

Windows 开发人员文档持续更新对整个 Windows 平台的开发人员提供的新功能的信息。 五月份提供了与 [Microsoft Build 2018](https://www.microsoft.com/build) 开发人员大会一致的以下功能概述、开发人员指南、视频和示例。

只需在 Windows 10 上[安装工具和 SDK](https://go.microsoft.com/fwlink/?LinkId=821431)，你便可以随时[创建新的通用 Windows 应用](../get-started/create-uwp-apps.md)，或了解如何使用 [Windows 上的现有应用代码](../porting/index.md)。

## <a name="features"></a>功能

### <a name="motion-in-fluent-design"></a>Fluent Design 中的动作

Fluent Design System 中，动作的使用基于计时、缓动、方向性和重力的基本原理在不断发展。 应用这些基本原理将有助于指导用户使用应用程序，并通过反映自然世界将用户与其数字体验联系起来。 在以下文章中了解更多信息：

* [动作概述](../design/motion/index.md)已更新，可反映这些基本原理。
* [实践中的动作](../design/motion/motion-in-practice.md)展示了应用中如何应用这些基本原理的示例。
* [方向性和重力](../design/motion/directionality-and-gravity.md)固化应用中用户的思维模式。
* [计时和缓动](../design/motion/timing-and-easing.md)增加了应用中动作的真实性。

![动作](../design/motion/images/contextual.gif)

### <a name="fluent-design-updates"></a>Fluent Design 更新

对以下 Fluent Design 页面进行了视觉更新和细微更改：

* [对齐、填充、边距](../design/layout/alignment-margin-padding.md)
* [颜色](../design/style/color.md)
* [命令基础知识](../design/basics/commanding-basics.md)
* [适用于 Windows 应用的 Fluent Design](../design/fluent-design-system/index.md)
* [应用设计简介](../design/basics/design-and-ui-intro.md)
* [导航基础知识](../design/basics/navigation-basics.md)
* [响应式设计技术](../design/layout/responsive-design.md)
* [屏幕大小和断点](../design/layout/screen-sizes-and-breakpoints-for-responsive-design.md)
* [样式概述](../design/style/index.md)
* [写入样式](../design/style/writing-style.md)

此外，以下页面的内容区域已全部重写：

* [图标](../design/style/icons.md)现在会提供实用的使用建议，也可点击使用。
* [版式](../design/style/typography.md)会整合相似文章中的信息，再依照更新后的指南和插图来将所有内容放在一个位置。

![调色板图像](../design/style/images/color/accent-color-palette.svg)

### <a name="app-installer-files-in-visual-studio"></a>Visual Studio 中的应用安装程序文件

现在可以使用 Visual Studio 2017 Update 15.7 创建应用安装程序文件。 [了解如何使用 Visual Studio 创建应用安装程序文件](../packaging/create-appinstallerfile-vs.md)并启用应用自动更新。 如果遇到问题，请参阅[排查应用安装程序文件的安装问题](../packaging/troubleshoot-appinstaller-issues.md)，以查看常见问题和解决方法。

### <a name="edge-webview-control-for-windows-forms-and-wpf-applications"></a>Windows 窗体和 WPF 应用程序的 Edge WebView 控件

使用 WebView 控件显示桌面应用程序中的 Web 内容，以前此功能仅适用于 UWP 应用程序。 此控件使用 Microsoft Edge 呈现引擎嵌入一个视图，用于呈现远程 Web 服务器、动态生成的代码或内容文件中丰富格式的 HTML 内容。 可以在最新版本的 [Windows 社区工具包](https://docs.microsoft.com/windows/uwpcommunitytoolkit/)中找到 WebView 控件。

可以在将来版本的 Windows 社区工具包中找到 WebView 等其他控件。 有关详细信息，请参阅[在 WPF 和 Windows 窗体应用程序中承载 UWP 控件](https://docs.microsoft.com/windows/uwp/xaml-platform/xaml-host-controls)。

### <a name="gaze-input-and-interactions"></a>视线输入和交互

[根据用户眼睛的位置及移动，跟踪用户的视线、注意和状态。](../design/input/gaze-interactions.md) 作为一种辅助技术，这种使用 UWP 应用并与之交互的全新强大方式特别有用。 视线输入还为游戏（包括目标采集和跟踪）及其他无法使用传统输入设备（键盘、鼠标、触摸）的交互式场景提供了颇具吸引力的机会。

### <a name="msix-packaging-format"></a>MSIX 打包格式

MSIX 已在 Microsoft Build 2018 大会上宣布推出，它是适用于所有 Windows 应用程序（包括 Win32、Windows 窗体、WPF 和 UWP）的新容器化包格式。 此新格式继承了 UWP 的卓越功能：

* 可靠的安装和更新。 
* 使用灵活功能系统的托管安全模型。
* 支持 Microsoft Store、企业管理和许多自定义分发模型。

将来版本的 Visual Studio 和 Windows SDK 中会提供用于创建这些包的工具。

MSIX 打包格式是一种开源格式，可让我们的合作伙伴更轻松地使用其工具和解决方案支持 MSIX 生态系统。 有关 MSIX 打包格式的详细信息，请参阅 [MSIX SDK](https://github.com/Microsoft/msix-packaging)。 

![MSIX 打包图像](images/msix.png)

### <a name="optional-packages-with-executable-code"></a>包含可执行代码的可选包

应用中的可选包现在可以包含可执行的 C# 代码。 [了解如何使用 Visual Studio 将可选的附加包配置为支持你的主要应用包。](../packaging/optional-packages-with-executable-code.md)

### <a name="page-transitions"></a>页面过渡

[页面过渡](../design/motion/page-transitions.md)导航应用中页面之间的用户。 它们帮助用户了解自己在导航层次结构中的位置，并提供有关页面之间关系的反馈。

### <a name="project-rome"></a>Project Rome

Project Rome 团队已大幅修改了其 iOS 和 Android SDK，添加了“用户活动”等新功能，并重构了大量的代码以跨不同的 SDK 提供一致的编程体验。 在 Build 2018 开发人员大会期间，[所有新 API 的参考和操作指南文档](https://docs.microsoft.com/windows/project-rome/)将会发布。

### <a name="sets"></a>集

Windows Insider Preview 内部版本中推出了“集”功能。 使用“集”功能时，你的应用将绘制到一个可与其他应用共享的窗口中，其中每个应用在标题栏中具有自身的选项卡。 

## <a name="developer-guidance"></a>开发人员指南

### <a name="get-started"></a>立即开始行动

我们已根据新的学习轨迹大范围改动了入门内容。 这些新主题旨在为新的 Windows 10 开发人员提供有关他们可能想要完成的一些常见任务的信息。 这些主题并非教程，也不提供手把手的演练，而是指出现有文档的位置及其用法。 请查看改编的[开始编写代码](../get-started/create-uwp-apps.md)页，或者浏览每篇个人学习轨迹文章：

* [构建表单](../get-started/construct-form-learning-track.md)
* [在列表中显示客户](../get-started/display-customers-in-list-learning-track.md)
* [保存和加载设置](../get-started/settings-learning-track.md)
* [处理文件](../get-started/fileio-learning-track.md)

![入门图像](../get-started/images/build-your-app.png)

### <a name="advertising-performance-report"></a>广告性能报告

合作伙伴中心内的[广告效果报告](../publish/advertising-performance-report.md)现在提供易查看性指标。 我们还添加了[优化广告单元的可见性](../monetize/optimize-ad-unit-viewability.md)文章，以提供有关优化广告易查看性的建议。

### <a name="targeted-push-notifications"></a>定向推送通知

合作伙伴中心内的[通知](../publish/send-push-notifications-to-your-apps-customers.md)页现在会在图形和世界地图视图中提供所有通知的其他分析数据。

## <a name="videos"></a>视频

### <a name="cwinrt"></a>C++/WinRT

C++/WinRT 是一种新的编写和使用 Windows 运行时 API 的方式。 该方式仅在头文件中实现，旨在提供对现代应用程序功能的一流访问。 [观看视频](https://www.youtube.com/watch?v=TLSul1XxppA&feature=youtu.be)了解其工作原理，然后[阅读开发人员文档](../cpp-and-winrt-apis/index.md)了解更多信息。

### <a name="multi-instance-uwp-apps"></a>多实例 UWP 应用

Windows 现在可以运行 UWP 应用中的多个实例，每个实例都在各自独立的进程中运行。 [观看视频](https://www.youtube.com/watch?v=clnnf4cigd0&feature=youtu.be)了解如何创建支持该功能的新应用，然后[阅读开发人员文档](../launch-resume/multi-instance-uwp.md)以获取更多使用该功能的方式和原因的有关指导。

## <a name="samples"></a>示例

### <a name="customer-database-tutorial"></a>客户数据库教程

此教程将创建用于管理客户列表的基本 UWP 应用，并介绍企业开发中有用的概念和实践。 它会引导你对本地 SQLite 数据库执行 UI 元素并添加操作，还为进一步操作时用以连接到远程的 REST 数据库提供了宽松指导。 [在此处查看教程](../enterprise/customer-database-tutorial.md)