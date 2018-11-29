---
ms.assetid: ''
description: 演示如何管理 UWP 应用的游戏广播。
title: 管理游戏广播
ms.date: 09/27/2017
ms.topic: article
keywords: windows 10, 游戏, 广播
ms.localizationpriority: medium
ms.openlocfilehash: c906551fd626dec726498ded9a7995007230504f
ms.sourcegitcommit: 89ff8ff88ef58f4fe6d3b1368fe94f62e59118ad
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/29/2018
ms.locfileid: "8200159"
---
# <a name="manage-game-broadcasting"></a>管理游戏广播
本文向你演示如何管理 UWP 应用的游戏广播。 用户必须使用内置在 Windows 中的系统 UI 启动广播，但从 Windows 10 版本 1709 开始，应用可以启动系统广播 UI，而且当广播开始和停止时可以收到通知。

## <a name="add-the-windows-desktop-extensions-for-the-uwp-to-your-app"></a>将适用于 UWP 的 Windows 桌面扩展添加到你的应用
位于 **[Windows.Media.AppBroadcasting](https://docs.microsoft.com/uwp/api/windows.media.appbroadcasting)** 命名空间的管理应用广播的 API 不包括在通用 API 协定中。 若要访问 API，你必须按照以下步骤将适用于 UWP 的 Windows 桌面扩展的引用添加到你的应用。

1. 在 Visual Studio 的**解决方案资源管理器**中，展开你的 UWP 项目并右键单击**引用**，然后选择**添加引用...**。 
2. 展开**通用 Windows** 节点并选择**扩展**。
3. 在扩展列表中，选中与你的项目的目标版本匹配的**适用于 UWP 的 Windows 桌面扩展**条目旁边的复选框。 对于应用广播功能，版本必须为 1709 或更高版本。
4. 单击**确定**。

## <a name="launch-the-system-ui-to-allow-the-user-to-initiate-broadcasting"></a>启动系统 UI 以允许用户启动广播
有几种原因导致你的应用当前可能无法广播，包括当前设备不满足广播所需的硬件要求，或另一个应用当前正在广播。 在启动系统 UI 前，你可以检查你的应用当前是否能够广播。 先检查广播 API 在当前设备上是否可用。 在操作系统版本早于 Windows 10 版本 1709 的设备上，这些 API 不可用。 也可以不检查具体的操作系统版本，改用 **[ApiInformation.IsApiContractPresent](https://docs.microsoft.com/uwp/api/windows.foundation.metadata.apiinformation.isapicontractpresent)** 方法查询 *Windows.Media.AppBroadcasting.AppBroadcastingContract* 版本 1.0。 如果此协定存在，则广播 API 在该设备上可用。

接下来通过在一次只有一位用户登录的电脑上调用工厂方法 **[GetDefault](https://docs.microsoft.com/uwp/api/windows.media.appbroadcasting.appbroadcastingui.GetDefault)** 获取 **[AppBroadcastingUI](https://docs.microsoft.com/uwp/api/windows.media.appbroadcasting.appbroadcastingui)** 类的实例。 在可以有多个用户登录的 XBox 上，改为调用 **[GetForUser](https://docs.microsoft.com/uwp/api/windows.media.appbroadcasting.appbroadcastingui.getforuser)**。 然后调用 **[GetStatus](https://docs.microsoft.com/uwp/api/windows.media.appbroadcasting.appbroadcastingui.GetStatus)** 获取应用的广播状态。

**AppBroadcastingStatus** 类的 **[CanStartBroadcast](https://docs.microsoft.com/uwp/api/windows.media.appbroadcasting.appbroadcastingstatus.CanStartBroadcast)** 属性向你指示应用当前是否可以开始广播。 如果不能，你可以检查**[详细信息](https://docs.microsoft.com/uwp/api/windows.media.appbroadcasting.appbroadcastingstatus.Details)** 属性以确定不可进行广播的原因。 根据具体原因，你可能要向用户显示状态或显示启用广播的说明。

[!code-cpp[CanStartBroadcast](./code/AppBroadcast/cpp/AppBroadcastExampleApp/App.cpp#SnippetCanStartBroadcast)]

通过调用 **[ShowBroadcastUI](https://docs.microsoft.com/uwp/api/windows.media.appbroadcasting.appbroadcastingui.ShowBroadcastUI)** 请求系统显示应用广播 UI。

> [!NOTE] 
> **ShowBroadcastUI** 方法表示可能失败的请求，具体取决于系统的当前状态。 你的应用在调用此方法后不应假设已经开始广播。 使用在开始或停止广播时要通知的 **[IsCurrentAppBroadcastingChanged](https://docs.microsoft.com/uwp/api/windows.media.appbroadcasting.appbroadcastingmonitor.IsCurrentAppBroadcastingChanged)** 事件。

[!code-cpp[LaunchBroadcastUI](./code/AppBroadcast/cpp/AppBroadcastExampleApp/App.cpp#SnippetLaunchBroadcastUI)]

## <a name="receive-notifications-when-broadcasting-starts-and-stops"></a>当广播开始和停止时收到通知
通过初始化 **[AppBroadcastingMonitor](https://docs.microsoft.com/uwp/api/windows.media.appbroadcasting.appbroadcastingmonitor)** 类的实例和注册 **[IsCurrentAppBroadcastingChanged](https://docs.microsoft.com/uwp/api/windows.media.appbroadcasting.appbroadcastingmonitor.IsCurrentAppBroadcastingChanged)** 处理程序的方法进行注册以在用户使用系统 UI 开始或停止广播你的应用时收到通知。 如上文所述，在尝试使用广播 API 前，请务必在某个时候使用 **[ApiInformation.IsApiContractPresent](https://docs.microsoft.com/uwp/api/windows.foundation.metadata.apiinformation.isapicontractpresent)** 验证这些 API 在设备上存在。 

[!code-cpp[AppBroadcastingRegisterChangedHandler](./code/AppBroadcast/cpp/AppBroadcastExampleApp/App.cpp#SnippetAppBroadcastingRegisterChangedHandler)]

在 **IsCurrentAppBroadcastingChanged** 事件的处理程序中，你可能想要更新应用的 UI 以反映当前广播状态。

[!code-cpp[AppBroadcastingChangedHandler](./code/AppBroadcast/cpp/AppBroadcastExampleApp/App.cpp#SnippetAppBroadcastingChangedHandler)]

## <a name="related-topics"></a>相关主题

* [游戏](index.md)

 

 




