---
title: 2017 年 7 月 Windows 文档中的新增功能 - 开发 UWP 应用
description: 新的功能、示例和开发人员指南已添加到 2017 年 7 月 Windows 10 开发人员文档
keywords: 新增功能, 更新, 功能, 开发人员指南, Windows 10
ms.date: 07/05/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: dd00d1aba0e6a58cd2128c90b2c7f714d3f8f802
ms.sourcegitcommit: b4c502d69a13340f6e3c887aa3c26ef2aeee9cee
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/03/2018
ms.locfileid: "8460170"
---
# <a name="whats-new-in-the-windows-developer-docs-in-july-2017"></a>2017 年 7 月 Windows 开发人员文档中的新增功能

Windows 开发人员文档持续更新对整个 Windows 平台的开发人员提供的新功能的信息。 最近提供了以下功能概述、开发人员指南和代码示例，包含面向 Windows 开发人员的新的和更新的信息。

只需在 Windows10 上[安装工具和 SDK](http://go.microsoft.com/fwlink/?LinkId=821431)，你便可以随时[创建新的通用 Windows 应用](../get-started/your-first-app.md)，或了解如何使用 [Windows 上的现有应用代码](../porting/index.md)。

## <a name="features"></a>功能

### <a name="fluent-design"></a>Fluent 设计

这些新增效果使用深度、透视和移动来帮助用户专注于重要的 UI 元素，对使用 SDK 预览版的 [Windows 预览体验成员](https://insider.windows.com/) 可用。

[亚克力材料](../design/style/acrylic.md) 是一种可以创造透明纹理的画笔。 

![浅色主题中的亚克力](../design/style/images/Acrylic_DarkTheme_Base.png)

[视差效果](../design/motion/parallax.md)可将三维深度和透视添加到应用。

![一个列表和背景图像的视差示例](../design/style/images/_Parallax_v2.gif)

[展示](../design/style/reveal.md) 突出应用的重要元素。 

![展示视觉](../design/style/images/Nav_Reveal_Animation.gif)

### <a name="ui-controls"></a>UI 控件

这些新控件对使用 SDK 预览版的 [Windows 预览体验成员](https://insider.windows.com/) 可用，便于更加轻松地生成出色外观的 UI。

[颜色选取器控件](../design/controls-and-patterns/color-picker.md) 使用户能够浏览和选择颜色。  

![默认颜色选取器](../design/controls-and-patterns/images/color-picker-default.png)

使用[导航视图控件](../design/controls-and-patterns/navigationview.md)可轻松向应用添加顶级导航。

![导航视图部分](../design/controls-and-patterns/images/navview_sections.png)

[个人图片控件](../design/controls-and-patterns/person-picture.md)显示某个人的头像。

![个人图片控件](../design/controls-and-patterns/images/person-picture/person-picture_hero.png)

使用[评分控件](../design/controls-and-patterns/rating.md)，用户能够轻松查看和设置反映内容和服务满意度的评分级别。

![评分控件示例](../design/controls-and-patterns/images/rating_rs2_doc_ratings_intro.png)

### <a name="design-toolkits"></a>设计工具包

[适用于 UWP 应用的设计工具包和资源](../design/downloads/index.md)已通过添加草图和 Adobe XD 工具包进行扩展。 之前存在的工具包也已进行更新和改进，为你的 UWP 应用提供更强大的控件和布局模板。

### <a name="dashboard-monetization-and-store-services"></a>仪表板、盈利和应用商店服务

以下新功能现在可用：

* Microsoft 广告 SDK 现在支持你在应用中显示[本机广告](../monetize/native-ads.md)。 本机广告是基于组件的广告格式，其中每一个广告创意元素（如标题、图像、说明和行动号召文字）都作为单独的元素提供给你。 本机广告目前仅适用于加入试验计划的开发人员，但我们计划不久后将此功能提供给所有的开发人员。

* [Microsoft Store 分析 API](../monetize/access-analytics-data-using-windows-store-services.md) 现提供一种可用来[下载应用中的错误的 CAB 文件](../monetize/download-the-cab-file-for-an-error-in-your-app.md)的方法。

* 使用[定向优惠](../publish/use-targeted-offers-to-maximize-engagement-and-conversions.md) 可以通过极富吸引力的个性化内容锁定特定目标客户群体，以提升参与度、忠诚度和增加盈利。 

* 你的应用的应用商店一览现在可以包括[视频预告片](../publish/app-screenshots-and-images.md#trailers)。

* 新的定价和可用性选项可用于[计划价格更改](../publish/set-and-schedule-app-pricing.md) 和[设置精确的发布日期](..//publish/configure-precise-release-scheduling.md)。

* 你可以[导入和导出应用商店一览](../publish/import-and-export-store-listings.md) 以更快地进行更新，尤其是在你有多种语言版本的一览时。

### <a name="my-people"></a>我的人脉

即将推出的“我的人脉”功能对使用 SDK 预览版的 [Windows 预览体验成员](https://insider.windows.com/)可用，用户可以将来自应用程序的人脉直接固定到任务栏。 [了解如何将“我的人脉”支持添加到应用程序。](../contacts-and-calendar/my-people-support.md)

![“我的人脉”联系面板](images/my-people.png)

[我的人脉共享](../contacts-and-calendar/my-people-sharing.md)使用户可以直接通过任务栏的应用程序共享文件。

![“我的人脉”共享](images/my-people-sharing.png)

[“我的人脉”通知](../contacts-and-calendar/my-people-support.md) 是一种新的 toast 通知类型，用户可以将其发送到他们固定的联系人。

![“我的人脉”通知](images/my-people-notification.png)

### <a name="pin-to-taskbar"></a>固定到任务栏

新的 TaskbarManager 类对使用 SDK 预览版的 [Windows 预览体验成员](https://insider.windows.com/) 可用，你可以通过它要求用户[将你的应用固定到任务栏](../design/shell/pin-to-taskbar.md)。

## <a name="developer-guidance"></a>开发人员指南

### <a name="media-playback"></a>媒体播放

新增部分已添加到基本媒体播放文章，[使用 MediaPlayer 播放音频和视频](../audio-video-camera/play-audio-and-video-with-mediaplayer.md)。 [使用 MediaPlayer 播放球面视频](../audio-video-camera/play-audio-and-video-with-mediaplayer.md) 部分向你显示如何播放球面编码的视频，包括调整视野和受支持的格式的视图方向。 [在帧服务器模式中使用 MediaPlayer](../audio-video-camera/play-audio-and-video-with-mediaplayer.md#use-mediaplayer-in-frame-server-mode) 部分向你显示如何将使用 [MediaPlayer](https://docs.microsoft.com/uwp/api/Windows.Media.Playback.MediaPlayer) 播放的媒体的帧复制到 Direct3D 表面。 这将启用应用像素着色器的实时效果等方案。 示例代码显示如何使用 Win2D 为视频播放快速实现模糊效果。

### <a name="media-capture"></a>媒体捕获

文章[使用 MediaFrameReader 处理媒体帧](../audio-video-camera/process-media-frames-with-mediaframereader.md) 已更新，以显示新的 [MultiSourceMediaFrameReader](https://docs.microsoft.com/uwp/api/windows.media.capture.frames.multisourcemediaframereader) 类的使用，你可以通过此类获取来自多个媒体源的与时间关联的帧。 如果您需要处理来自不同源（例如深度相机和彩色相机）的帧，并且需要确保从每个源捕获帧的时间彼此接近，那么这非常有用。 有关详细信息，请参阅[使用 MultiSourceMediaFrameReader 从多个源获取与时间关联的帧](../audio-video-camera/process-media-frames-with-mediaframereader.md#use-multisourcemediaframereader-to-get-time-corellated-frames-from-multiple-sources)。

### <a name="scoped-search"></a>范围搜索

“UWP”范围已添加到 docs.microsoft.com 上的 [UWP 概念](../get-started/universal-application-platform-guide.md)和 [API 参考](https://docs.microsoft.com/en-us/uwp/api/)文档。 除非已停用此范围，否则从这些区域中执行的搜索将仅返回 UWP 文档。

![范围搜索](images/scoped-search.png)

### <a name="test-your-windows-app-for-windows-10-s"></a>测试适用于 Windows 10 S 的 Windows 应用

测试你的 Windows 应用，以确保其在运行 Windows S 的设备上正常运行。使用[此新指南](../porting/desktop-to-uwp-test-windows-s.md) 了解测试方法。 

## <a name="samples"></a>示例

### <a name="annotated-audio-app-sample"></a>Annotated Audio 应用示例

[一款迷你应用示例，演示了音频、墨迹和 OneDrive 数据漫游方案](https://github.com/Microsoft/Windows-appsample-annotated-audio)。 此示例录制音频，同时允许同步捕获墨迹注释，以便你可以稍后回顾在记笔记时讨论的内容。

![Annotated Audio 示例屏幕截图](images/Playback.png)  

### <a name="shopping-app-sample"></a>购物应用示例

[一款迷你应用，提供用户可以购买表情符号的基本购物体验](https://github.com/Microsoft/Windows-appsample-shopping)。 此应用显示如何使用[付款请求 API](https://docs.microsoft.com/uwp/api/windows.applicationmodel.payments) 实现结帐体验。

![购买应用示例的屏幕截图](images/shoppingcart.png)  

## <a name="videos"></a>视频

### <a name="accessibility"></a>辅助功能

将辅助功能内置到应用中会让应用获得更广泛的受众。 [观看视频](https://channel9.msdn.com/Blogs/One-Dev-Minute/Developing-Apps-for-Accessibility)，然后详细了解[开发应用的辅助功能](https://developer.microsoft.com/en-us/windows/accessible-apps)。

### <a name="payments-request-api"></a>付款请求 API

付款请求 API 帮助客户和卖家无缝地完成在线结帐过程。 [观看视频](https://channel9.msdn.com/Blogs/One-Dev-Minute/Using-the-Payments-Request-API)，然后探索[付款请求文档](https://channel9.msdn.com/Blogs/One-Dev-Minute/Using-the-Payments-Request-API)。

### <a name="windows-10-iot-core"></a>Windows 10 IoT 核心版

通过 Windows 10 IoT 核心版和通用 Windows 平台，可以快速使用影像和组件连接建立项目原型和生成项目，如 Pet Recognition Door。 [观看视频](https://channel9.msdn.com/Blogs/One-Dev-Minute/Building-a-Pet-Recognition-Door-Using-Windows-10-IoT-Core)，然后详细了解如何[开始使用 Windows 10 IoT 核心版](https://developer.microsoft.com/en-us/windows/iot)。