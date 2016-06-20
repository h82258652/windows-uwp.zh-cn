---
author: QuinnRadich
title: Windows 10 中的新增功能
description: Windows 10 周年 SDK 预览版及新增的开发人员工具将提供受新通用 Windows 平台支持的工具、功能和体验。
---

# Windows 中的新增功能

Windows 10 周年 SDK 预览版 14295 和 Windows 开发人员工具更新继续提供受通用 Windows 平台支持的工具、功能和体验。 只需在 Windows 10 上[安装工具和 SDK](https://developer.microsoft.com/en-us/windows/downloads#_blank)，你便可以随时[创建新的通用 Windows 应用](https://msdn.microsoft.com/library/windows/apps/bg124288)，或了解如何使用 [Windows 上的现有应用代码](https://msdn.microsoft.com/library/windows/apps/mt238321)。

## Windows 10 周年 SDK 预览版 12295

功能 | 描述
 :---- | :----
网络 | 你现在可以通过订阅 [HttpBaseProtocolFilter.ServerCustomValidationRequest](https://msdn.microsoft.com/library/windows/apps/windows.web.http.filters.httpbaseprotocolfilter.aspx#_blank) 事件来提供你自己的自定义服务器 SSL/TLS 证书验证。 你还可以通过在 HTTP 请求中指定 [HttpCacheReadBehavior.NoCache](https://msdn.microsoft.com/library/windows/apps/windows.web.http.filters.httpcachereadbehavior.aspx#_blank) 枚举值来完全禁用从缓存读取 HTTP 响应。 清除身份验证凭据以支持“注销”方案现在可以通过调用 [HttpBaseProtocolFilter.ClearAuthenticationCache](https://msdn.microsoft.com/library/windows/apps/windows.web.http.filters.httpbaseprotocolfilter.aspx#_blank) 方法来实现。
扩展 | Microsoft Edge 的一项新功能是可以使用扩展。 借助扩展，用户能够扩展 Microsoft Edge 的功能，从而提供对目标受众来说很重要的小众功能。 有关详细信息，请查看[扩展文档](https://developer.microsoft.com/en-us/microsoft-edge/platform/documentation/extensions/#_blank)。
蓝牙 API | 应用现在能够通过 [Windows.Devices.Bluetooth 和 Windows.Devices.Bluetooth.Rfcomm](https://msdn.microsoft.com/library/windows/apps/windows.devices.bluetooth.aspx#_blank) 在远程蓝牙外设上访问 RFCOMM 服务，而无需事先与外设配对。 新方法允许应用在非配对设备上搜索和访问 RFCOMM 服务。
聊天 API | 使用新的 [ChatSyncManager](https://msdn.microsoft.com/library/windows/apps/mt414181.aspx#_blank) 类，你可以与云同步短信。
[适用于 Android 和 iOS 开发人员的 Windows 应用概念映射](https://msdn.microsoft.com/windows/uwp/porting/android-ios-uwp-map#_blank) | 如果你是具有 Android 或 iOS 技能和/或代码的开发人员，并且希望移动到 Windows 10 和通用 Windows 平台 (UWP)，则此资源具有在三个平台之间映射平台功能（以及你的知识）所需的一切。
[企业数据保护 (EDP)](https://msdn.microsoft.com/windows/uwp/enterprise/edp-hub?branch=build2016#_blank) | EDP 是移动设备管理 (MDM) 的一组桌面、笔记本电脑、平板电脑和手机上的功能。 EDP 使企业可以更好地控制在企业所管理的设备上处理其数据（企业文件和数据 blob）的方式。
[Windows.ApplicationModel.AppExtensions](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.appextensions.aspx#_blank) | 新的 AppExtensions 命名空间允许 Windows 应用商店应用托管其他 Windows 应用商店应用提供的内容。 你可以从这些应用发现、枚举和访问只读内容。
Windows IoT | Windows 10 IoT 核心版使你能够在熟悉的 Windows 中创建 IoT 应用程序，目前在 Raspberry Pi 3（最新的 Raspberry Pi 板）上可用。
媒体 API | Windows.Media.Playback 命名空间中的新 MediaBreak API可使你在使用 MediaSource 和 MediaPlaybackItem 播放媒体时轻松计划和管理媒体中断。 Windows.Media.Audio 命名空间中的新 AudioGraph API 添加了空间音频处理，可使你将 3D 定位发射器和侦听器分配给音频图形节点。
地图 API | 改进了 [MapControl](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.maps.mapcontrol.aspx#_blank)，以允许开发人员获取相机附近的可见区域，在距离上远离和在高坡度视图中接近地平线的区域除外。 扩展了 [MapLocationFinder](https://msdn.microsoft.com/library/windows/apps/windows.services.maps.maplocationfinder.aspx#_blank) 类，从而允许开发人员通过指定所需的精确度在进行反向地理编码时优化网络通信。 开发人员现在可以通过使用 [LaunchUriAsync](https://msdn.microsoft.com/library/windows/apps/hh701480.aspx#_blank) 方法和指定纬度和经度来充分利用下载离线地图的好处。 有关详细信息，请参阅[启动 Windows 地图应用](https://msdn.microsoft.com/windows/uwp/launch-resume/launch-maps-app#_blank)。


<!--HONumber=Jun16_HO3-->


