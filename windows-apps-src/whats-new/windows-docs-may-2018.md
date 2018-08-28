---
author: QuinnRadich
title: What's New in Windows 文档中可能 2018年-开发 UWP 应用程序
description: 新功能、 视频和开发人员指南均已添加到年 5 月 2018年 Windows 10 开发人员文档和 Microsoft 内部会议。
keywords: what's new、 更新功能，开发人员指南，Windows 10 月，生成
ms.author: quradic
ms.date: 5/7/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
ms.localizationpriority: medium
ms.openlocfilehash: 322bc056411095019dfc027078cbfef7de0883fb
ms.sourcegitcommit: 9a17266f208ec415fc718e5254d5b4c08835150c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/28/2018
ms.locfileid: "2887866"
---
# <a name="whats-new-in-the-windows-developer-docs-in-may-2018"></a>What's New in Windows 开发人员文档中可能 2018

Windows 开发人员文档持续更新对整个 Windows 平台的开发人员提供的新功能的信息。 以下功能概述、 开发人员指南、 视频和示例进行了五月一致与[Microsoft 生成 2018年](https://www.microsoft.com/build)开发人员会议中可用。

只需在 Windows10 上[安装工具和 SDK](http://go.microsoft.com/fwlink/?LinkId=821431)，你便可以随时[创建新的通用 Windows 应用](../get-started/create-uwp-apps.md)，或了解如何使用 [Windows 上的现有应用代码](../porting/index.md)。

## <a name="features"></a>功能

### <a name="motion-in-fluent-design"></a>Fluent 设计中的动态

运动 Fluent 设计系统中的用户发展，基于计时、 减轻、 方向性和 gravity 的基础知识。 应用这些基础知识有助于指导您的应用程序，用户，并将它们连接提供其数字体验，通过这就反映了自然世界。 了解详细信息此文章中：

* [运动概述](../design/motion/index.md)已更新以反映这些基础知识。
* [动态的实践](../design/motion/motion-in-practice.md)提供如何应用这些应用程序中的基础知识的示例。
* [方向性和 gravity](../design/motion/directionality-and-gravity.md)固化您的应用程序的用户的精神模型。
* [计时和简化](../design/motion/timing-and-easing.md)将真实性向您的应用程序中的动作。

![在操作的动态](../design/motion/images/contextual.gif)

### <a name="fluent-design-updates"></a>Fluent 设计更新

已对以下 Fluent 设计页面 visual 更新和细微更改：

* [对齐方式、 边距、 边距](../design/layout/alignment-margin-padding.md)
* [Color](../design/style/color.md)
* [命令基础知识](../design/basics/commanding-basics.md)
* [Fluent 设计 Windows 应用程序 （英文）](../design/fluent-design-system/index.md)
* [应用程序设计简介](../design/basics/design-and-ui-intro.md)
* [导航基础知识](../design/basics/navigation-basics.md)
* [响应式设计技术](../design/layout/responsive-design.md)
* [屏幕大小和断点](../design/layout/screen-sizes-and-breakpoints-for-responsive-design.md)
* [样式概述](../design/style/index.md)
* [写入样式](../design/style/writing-style.md)

此外，我们已重新编写以下页面时带他们的内容区域上的所有新信息：

* [图标](../design/style/icons.md)现在提供了有关使用图标并使其成为可单击的实践建议。
* [版式](../design/style/typography.md)合并将全部内容放在一个位置更新了的指南和示意图，介绍与从类似文章的信息。

![彩色调色板图像](../design/style/images/color/accent-color-palette.svg)

### <a name="app-installer-files-in-visual-studio"></a>在 Visual Studio 中的应用程序安装程序文件

现在可以使用 Visual Studio 2017，更新 15.7 创建应用程序安装程序文件。 [了解如何使用 Visual Studio 创建的应用程序安装程序文件](../packaging/create-appinstallerfile-vs.md)和启用自动更新到您的应用程序。 如果您遇到问题，请参阅[解决安装问题的应用程序安装程序文件](../packaging/troubleshoot-appinstaller-issues.md)以查看一般问题和解决方案。

### <a name="edge-webview-control-for-windows-forms-and-wpf-applications"></a>边缘 Windows 窗体和 WPF 应用程序的 web 视图控件

使用以前仅供 UWP 应用程序的 web 视图控件显示桌面应用程序中的 web 内容。 此控件使用 Microsoft 边缘呈现引擎嵌入视图呈现丰富格式化 HTML 内容从远程 web 服务器、 动态生成的代码或内容文件。 查找最新版本中的 web 视图控件[Windows 社区工具包。](https://docs.microsoft.com/windows/uwpcommunitytoolkit/)

对于其他控件，如 web 视图的将来版本中 Windows 社区工具包的外观。 有关详细信息，请参阅[主机 UWP 控制 WPF 和 Windows 窗体应用程序中。](https://docs.microsoft.com/windows/uwp/xaml-platform/xaml-host-controls)

### <a name="gaze-input-and-interactions"></a>对此输入和交互

[根据用户眼睛的位置及移动，跟踪用户的凝视、注意和状态。](../design/input/gaze-interactions.md) 此功能强大的新方法，以使用并与其交互 UWP 应用程序是作为辅助技术特别有用。 对此输入还提供令人信服机会游戏 （包括目标获取和跟踪） 和其他交互式方案 （键盘、 鼠标或触摸） 的传统输入的设备不可用。

### <a name="msix-packaging-format"></a>MSIX 打包格式

MSIX 宣布 Microsoft 生成 2018年大会，为新 containerization 包格式应用于所有 Windows 应用程序包括 Win32、 Windows 窗体、 WPF 和 UWP。 这一新格式继承 UWP 强大功能：

* 可靠的安装和更新。 
* 管理与灵活功能系统的安全模型。
* 支持 Microsoft 存储、 企业管理和多个自定义通讯模型。

创建这些程序包的工具将 Visual Studio 和 Windows SDK 的将来版本中提供。

MSIX 打包格式是开放源代码格式，这便于我们的合作伙伴以支持其工具和解决方案的 MSIX 生态系统。 若要了解详细信息的 MSIX 打包格式，请参阅[MSIX SDK](https://github.com/Microsoft/msix-packaging)。 

![MSIX 打包图像](images/msix.png)

### <a name="optional-packages-with-executable-code"></a>包含可执行代码的可选包

在您的应用程序的可选包现在可以包含可执行的 C# 代码。 [了解如何使用 Visual Studio 配置可选的加载项包来支持您的主应用程序包。](../packaging/optional-packages-with-executable-code.md)

### <a name="page-transitions"></a>页面过渡

[网页过渡](../design/motion/page-transitions.md)导航应用程序中的页面之间的用户。 这些解决方案有助于用户理解何处中导航层次结构，并提供有关页面之间的关系的反馈。

### <a name="project-rome"></a>Project Rome

项目 Rome 团队已检查其 iOS 和 Android Sdk，添加新功能，如用户活动和重构何种自己的代码以跨不同 Sdk 中提供一致的编程体验。 [所有新的 API 参考和操作方法文档](https://docs.microsoft.com/windows/project-rome/)将转 live 生成 2018年开发人员会议期间。

### <a name="sets"></a>设置

在预览生成的 Windows 内幕中可用集功能。 使用集功能时，您应用程序绘制到可能与其他应用程序，与具有自己的选项卡的标题栏中每个应用程序共享窗口。 [设计集](../design/shell/design-for-sets.md)对如何优化您的应用程序，以提供最佳体验设置 UI 中的指南。

## <a name="developer-guidance"></a>开发人员指南

### <a name="get-started"></a>入门

我们已大大增加我们 Get 启动与新学习跟踪内容。 这些新主题旨在向新 Windows 10 开发人员提供他们可能需要完成一些常见任务的信息。 他们不能教程和不提供手持演练中，但改为指出所在现有文档以及如何使用它。 签出修改了[开始编码](../get-started/create-uwp-apps.md)页上，或浏览每个单独的学习跟踪：

* [构建表单](../get-started/construct-form-learning-track.md)
* [以列表形式显示客户](../get-started/display-customers-in-list-learning-track.md)
* [保存并加载设置](../get-started/settings-learning-track.md)
* [处理文件](../get-started/fileio-learning-track.md)

![获取开始的图像](../get-started/images/build-your-app.png)

### <a name="advertising-performance-report"></a>广告性能报告

开发人员中心仪表板中的[广告性能报告](../publish/advertising-performance-report.md)现在提供了可视性指标。 我们还添加[优化的 ad 单位可视性](../monetize/optimize-ad-unit-viewability.md)文章提供有关优化您的广告的可视性的建议。

### <a name="targeted-push-notifications"></a>目标的推送通知

开发人员中心仪表板中的[通知](../publish/send-push-notifications-to-your-apps-customers.md)页现在提供图表和 world 地图视图中所有通知其他分析数据。

## <a name="videos"></a>视频

### <a name="cwinrt"></a>C++/WinRT

C + + / WinRT 是创作和使用 Windows 运行时 Api 的新方法。 它具有头文件中实现唯一，旨在为您提供一类访问现代的应用程序功能。 [观看视频](https://www.youtube.com/watch?v=TLSul1XxppA&feature=youtu.be)来了解工作原理，然后[阅读开发人员文档](../cpp-and-winrt-apis/index.md)的详细信息。

### <a name="multi-instance-uwp-apps"></a>多实例 UWP 应用

Windows 现在可以使用它自己的单独进程中的每个运行您 UWP 的应用程序的多个实例。 [观看视频](https://www.youtube.com/watch?v=clnnf4cigd0&feature=youtu.be)以了解如何创建新的应用程序的更多指南如何支持此功能，则[读取开发人员文档](../launch-resume/multi-instance-uwp.md)和为什么要使用此功能。

## <a name="samples"></a>示例

### <a name="customer-database-tutorial"></a>客户数据库教程

本教程中创建用于管理的客户，列表的基本 UWP 应用程序，并介绍的概念和做法有用企业开发中。 它指导您在实现 UI 元素并添加对本地 SQLite 数据库，操作，并提供用于连接到远程 REST 数据库，如果您想要进一步松散指导。 [签出的教程此处](../enterprise/customer-database-tutorial.md)