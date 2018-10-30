---
author: Mtoepke
title: Xbox One 上的 UWP
description: 如何在 Xbox One 上生成适用于通用 Windows 平台 (UWP) 的应用。
ms.author: mstahl
ms.date: 10/25/2017
ms.topic: article
keywords: windows 10, uwp
ms.assetid: 2d935f53-84db-4108-86dc-cb6a0749782f
ms.localizationpriority: medium
ms.openlocfilehash: 9b35733978ffbaca00caae2128b799d6cc923542
ms.sourcegitcommit: 753e0a7160a88830d9908b446ef0907cc71c64e7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/30/2018
ms.locfileid: "5765043"
---
# <a name="uwp-on-xbox-one"></a>Xbox One 上的 UWP

在 Xbox One 上生成适用于通用 Windows 平台 (UWP) 的应用入门。

Xbox One 上的 UWP 支持开发应用和游戏。 你不必参与开发人员计划，便可在 Xbox 上试用、创建和测试游戏。 你只需要在 Windows 开发人员中心上拥有[开发人员帐户](https://developer.microsoft.com/en-us/store/register)。 当你准备在 Xbox One 上发布和出售游戏或利用 Windows 10 上的 Xbox Live 时，你需要加入 [Xbox Live 创意者计划](https://developer.microsoft.com/games/xbox/xboxlive/creator) 或者需要是 [ID@Xbox](http://www.xbox.com/Developers/id) 开发人员。 如果计划成为 ID@Xbox 开发人员，建议在注册开发人员帐户前先申请加入该计划。 有关详细信息，请参阅[开发人员计划概述](../xbox-live/developer-program-overview.md)。

本部分包括设置步骤、身份验证过程指南、有关安装所需的 Visual Studio 版本和 Windows 10 工具的信息，以及生成、运行和调试你的第一个简单应用程序的步骤。 

| 主题      | 说明 |
|------------|-------------|
|[入门](getting-started.md)| Xbox One 上的 UWP 开发入门指南。 |
|[新增功能](whats-new.md)| 突出显示 Xbox One 上的 UWP 中的新功能。 |
|[Xbox One 开发人员模式激活](devkit-activation.md)| 说明如何在 Xbox One 上启用开发人员模式。 |
|[在 Xbox One 上禁用开发人员模式](devkit-deactivation.md)| 解释如何在 Xbox One 上禁用开发人员模式。 |
|[在 Xbox 开发环境上设置 UWP](development-environment-setup.md)| 介绍设置和测试 Xbox One 开发环境的步骤。 |
|[示例](samples.md)| TVHelpers 是面向 GitHub 位置的指针，可以在这里找到有用的 XAML 和 JavaScript 示例以开始针对 Xbox 进行开发。 示例包括完整的 XAML 媒体应用模板以及自动控制器导航、丰富的媒体播放和基于 Web 技术的搜索。 |
|[已知问题](known-issues.md)| Xbox One 上的 UWP 的已知问题。 |
|[常见问题](frequently-asked-questions.md)| 与 Xbox One 上的 UWP 相关的常见问题。 |
|[工具](introduction-to-xbox-tools.md)| 介绍特定于 Xbox One 的工具 _Dev Home_、如何使用 Windows Device Portal 以及如何设置 Visual Studio 以进行开发。 本部分还指导新开发人员完成他们的第一个 Xbox UWP 应用程序，并说明如何使用 Fiddler 工具来查看网络流量。 |
| [Xbox 应用开发活动](https://developer.microsoft.com/windows/projects/campaigns/app-dev-on-xbox-event) | 对于刚开始在 Xbox 上生成应用的开发人员来说，Xbox 应用开发活动是一个很好的起点。 观看会议录像和浏览活动期间发布的博客文章。 |
|[针对 Xbox 和电视进行设计](../design/devices/designing-for-tv.md)| 介绍了有关设计可在电视上看到并将控制器用于输入的应用的最佳做法。 |
|[Xbox 最佳做法](tailoring-for-xbox.md)| 如何关闭鼠标模式、绘制到屏幕的边缘和禁用缩放。 |
|[使用语音调用 UI 元素](ves-on-xbox.md)| 介绍在 Xbox 上的 UWP 应用中支持 Voice Enabled Shell 的最佳实践。 |
|[Xbox One 上 UWP 应用和游戏的系统资源](system-resource-allocation.md)| 介绍了应用程序在 Xbox One 上运行时可用的资源。 |
|[多用户应用程序简介](multi-user-applications.md)| 介绍了 Xbox One 上的多用户应用程序 (MUA)。 |
| [自动化 Xbox One 开发任务](https://github.com/Microsoft/WindowsDevicePortalWrapper/tree/v0.9.4) | GitHub 上的 WindowsDevicePortalWrapper 项目提供了一个库，允许自动执行常见的开发任务，例如部署或启动应用。 该项目包括一个示例 XboxWdpDriver.exe，演示如何使用这些 API 执行常见任务。 |
|[将现有游戏移植到 Xbox](development-lanes-landing.md)|基于生成你的游戏所熟练掌握的技术，我们可以直接向你提供分步说明，这可以加快使用 UWP 将你的游戏移植到 Xbox 的进度。|
|[Xbox One 上尚不支持的 UWP 功能](http://go.microsoft.com/fwlink/p/?LinkId=760755)|  介绍了在 Xbox One 上尚不能正常运行的 UWP 功能区域。|

## <a name="videos"></a>视频

有关在 Xbox 上生成出色应用的信息，请参阅第 9 频道上的以下讨论：

* [生成适用于 Xbox 的出色通用 Windows 平台 (UWP) 应用](https://channel9.msdn.com/Events/Build/2016/B883)
* [调整你的适用于 Xbox One 和电视的应用](https://channel9.msdn.com/Events/Build/2016/T651-R1)
* [UWP 开发 1：生成自适应 UI](https://channel9.msdn.com/Events/Build/2016/L724-R1)
* [浏览器外的 Web 应用：跨平台与跨设备](https://channel9.msdn.com/Events/Build/2016/B888)

## <a name="see-also"></a>另请参阅

- [自动启动 Windows 10 UWP 应用](automate-launching-uwp-apps.md)
- [适用于游戏开发的 CPUSets](cpusets-games.md)