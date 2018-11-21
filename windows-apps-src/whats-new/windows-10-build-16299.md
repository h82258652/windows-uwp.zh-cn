---
author: QuinnRadich
title: 面向开发人员的 Windows 10 中的新增工具和功能
description: Windows 10 版本 16299 和新开发人员工具提供由通用 Windows 平台支持的工具、功能和体验。
keywords: 最近更新, 更新, 功能, 新增, Windows 10, 1709, 10 月, 最新, 开发人员, 16299, Fall Creators
ms.author: quradic
ms.date: 11/02/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 458a5999c1f56a3bc9f445f260d1d294c395b850
ms.sourcegitcommit: cbe7cf620622a5e4df7414f9e38dfecec1cfca99
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/21/2018
ms.locfileid: "7434795"
---
# <a name="whats-new-in-windows-10-for-developers-build-16299"></a>面向开发人员的 Windows 10 版本 16299 中的最近更新

Windows 10 版本 16299（又称 Fall Creators Update 或版本 1709）与 Visual Studio 2017 和更新的 SDK 相结合，为打造出色的通用 Windows 平台应用提供了丰富的工具、功能和体验。 只需在 Windows10 上[安装工具和 SDK](http://go.microsoft.com/fwlink/?LinkId=821431)，你便可以随时[创建新的通用 Windows 应用](../get-started/create-uwp-apps.md)，或了解如何使用 [Windows 上的现有应用代码](../porting/index.md)。

该版本集合了 Windows 开发人员感兴趣的新增和改进功能及指南。 有关添加到 Windows SDK 的新命名空间的完整列表，请参阅 [Windows 10 版本 16299 API 更改](windows-10-build-16299-api-diff.md)。 有关 Windows 10 重要功能的详细信息，请参阅 [Windows 10 中的酷炫功能](http://go.microsoft.com/fwlink/?LinkId=823181)。 另请参阅 [Windows 开发人员平台功能](https://developer.microsoft.com/windows/platform/features)，了解有关 Windows 平台过去已添加的及将来要添加的功能的详尽概述。

## <a name="design--ui"></a>设计和 UI

功能 | 说明
 :------ | :------
条件 XAML | 你现在可以使用[条件 XAML](../debug-test-perf/conditional-xaml.md) 以创建[版本自适应应用](../debug-test-perf/version-adaptive-apps.md)。 使用条件 XAML，你可以在 XAML 标记中使用 **ApiInformation.IsApiContractPresent** 方法，这样你就可以基于是否存在 API 在标记中设置属性并例示对象，而无需在之后使用代码。
设计工具包 | [适用于 UWP 应用的设计工具包和资源](../design/downloads/index.md)已通过添加草图和 Adobe XD 工具包进行扩展。 之前存在的工具包也已更新和改进，为你的 UWP 应用提供更强大的控件和布局模板。 此外还添加了新的工具和样本，以便提供示例和灵感。
Fluent 设计效果 | 这些新增效果是 Fluent 设计系统的一部分，并且使用深度、透视和移动来帮助用户关注重要的 UI 元素。 </br>* [亚克力材料](../design/style/acrylic.md)是一种可以创造透明纹理的画笔。 </br>* [视差效果](../design/motion/parallax.md)可将三维深度和透视添加到应用。 </br>* [显示](../design/style/reveal.md)突出应用的重要元素。 </br> 如需了解更多详细信息，请参阅 [Fluent 设计概述](https://fluent.microsoft.com/)。
键盘快捷方式 | 使用[键盘快捷方式](../design/input/keyboard-accelerators.md)增强应用的辅助功能和可用性。 键盘快捷方式使用户能够直观地调用常见的操作或命令，而无需浏览应用 UI，并且可以通过相关配置实现功能的适用性。
墨迹书写 | [CoreIncrementalInkStroke](https://docs.microsoft.com/uwp/api/windows.ui.input.inking.core.coreincrementalinkstroke) API 支持使用单独的 **InkPoint** 对象创建能够以增量方式呈现的单独墨迹笔划。 </br></br> [CoreInkPresenterHost](https://docs.microsoft.com/uwp/api/windows.ui.input.inking.core.coreinkpresenterhost) API 使你能够托管无相关 **InkCanvas** 控件的 **InkPresenter** 对象。
射线控制器 | [RadialControllerConfiguration](https://docs.microsoft.com/uwp/api/windows.ui.input.radialcontrollerconfiguration) API 已更新，能够将 **RadialController** 菜单的范围控制到应用的视图或进程。
动态磁贴 | [从桌面桥 Win32 应用固定辅助磁贴](../design/shell/tiles-and-notifications/secondary-tiles-desktop-pinning.md)。
Toast 通知 | 使用按钮上的[挂起的更新](../design/shell/tiles-and-notifications/toast-pending-update.md)在 Toast 内创建多步骤交互。
UI 控件 | 使用这些新控件能够更快速轻松地构建美观的 UI。 </br>* [颜色选取器控件](../design/controls-and-patterns/color-picker.md)使用户能够浏览和选择颜色。 </br>* 使用[导航视图控件](../design/controls-and-patterns/navigationview.md)可轻松向应用添加顶级导航。 </br>* [个人图片控件](../design/controls-and-patterns/person-picture.md)显示个人头像。 </br>* 使用[评分控件](../design/controls-and-patterns/rating.md)，用户能够轻松查看和设置反映内容和服务满意度的评分。
语音和声调 | 我们添加了新的 [UWP 应用中的语音和声调指南](../design/style/voice-and-tone.md)，为你提供了有关在应用中编写文本的建议。 无论你创建什么内容，用语应亲切、友好并提供有用信息。

## <a name="gaming"></a>游戏

功能 | 说明
 :------ | :------
游戏广播 | **[Windows.Media.AppBroadcasting](https://docs.microsoft.com/uwp/api/windows.media.appbroadcasting)** 命名空间中的新 API 允许应用启动系统提供的游戏广播 UI。 </br>你也可以登记通知应用广播何时开始或停止的事件。 使用 **[Windows.Media.AppRecording](https://docs.microsoft.com/uwp/api/windows.media.apprecording)** 命名空间中的新 API，你可以录制音频和视频并且捕获游戏截图。 </br>你也可以提供系统将嵌入广播和捕获流中的元数据，以便应用提供与游戏事件同步的查看体验。 有关这些功能的更多信息，请参阅[游戏广播和捕获](../gaming/game-broadcast-and-capture.md)。
游戏聊天覆盖 | [GameChatOverlay 类](https://docs.microsoft.com/uwp/api/windows.gaming.ui.gamechatoverlay)提供了用于获取默认游戏聊天覆盖实例、设置希望的覆盖位置以及添加消息的方法。
游戏设备信息 | 由于控制台的功能不同，通用 Windows 平台 (UWP) 的游戏开发人员需要通过某种方法确定运行游戏的控制台类型，以便在运行时选择充分利用硬件的方式。 **&lt;gamingdeviceinformation.h&gt;** 中的[游戏设备信息](https://aka.ms/gamingdeviceinfo) API 具有此项功能。
游戏模式 | 适用于通用 Windows 平台 (UWP) 的[游戏模式](https://msdn.microsoft.com/library/windows/desktop/mt808808) API 让你可以利用 Windows 10 中的游戏模式产生最优化的游戏体验。 这些 API 位于 **&lt;expandedresources.h&gt;** 标头中。
游戏监视器 | [GameMonitor 类](https://docs.microsoft.com/uwp/api/windows.gaming.ui.gamemonitor)允许应用获取设备的游戏监视权限状态，并且可能会提示用户启用游戏监视。
TruePlay | [TruePlay](https://aka.ms/trueplay) 为开发人员提供一组反电脑游戏作弊技术的新工具。 加入 TruePlay 的游戏将在受保护的程序中运行，减少了常见的攻击类型。 适用于通用 Windows 平台 (UWP) 的 TruePlay API 允许 Windows 10 电脑上的游戏以及游戏监视系统之间进行有限的交互。 这些 API 位于 **&lt;gamemonitor.h&gt;** 标头中。
Xbox Live | 我们添加了面向 Xbox Live 开发人员、适用于 UWP 和 Xbox 开发人员工具包 (XDK) 游戏的文档。 </br>* 请参阅 [Xbox Live 开发人员指南](../xbox-live/index.md)以了解如何使用 Xbox Live API 将你的游戏连接到 Xbox Live 社交游戏网络。 </br>* 借助 [Xbox Live 创意者计划](../xbox-live/get-started-with-creators/get-started-with-xbox-live-creators.md)，任何 UWP 游戏开发人员都可以在电脑和 Xbox One 上开发和发布支持 Xbox Live 的游戏。 </br>* 请参阅 [Xbox Live 开发人员计划概述](../xbox-live/developer-program-overview.md)，获取有关 Xbox Live 开发人员可用程序和功能的信息。

## <a name="develop-windows-apps"></a>开发 Windows 应用

功能 | 说明
 :------ | :------
激活 UWP 应用 | 以下新功能现在可用： </br>* 使用 [StartupTask 类](https://docs.microsoft.com/uwp/api/windows.applicationmodel.startuptask)指定 UWP 应用在用户登录时或者在系统启动时开始。 </br> * 确定 UWP 应用是否[从命令行启动](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Activation.ActivationKind)。 </br>* 以编程方式，使用 [RequestRestartAsync() 和 RequestRestartForUserAsync()](https://docs.microsoft.com/uwp/api/windows.applicationmodel.core.coreapplication) API 请求 UWP 应用重启。 </br>* [启动 Windows 设置应用](../launch-resume/launch-settings-app.md)已更新，以反映 `ms-settings:storagesense`、`ms-settings:cortana-notifications` 等新的 URI 方案。
应用打包 | 应用安装程序已扩展，支持从网页下载 UWP 应用包。 此外，现在可以使用应用安装程序下载相关的应用包。 查看新的[使用应用安装程序安装 UWP 应用](../packaging/appinstaller-root.md)部分以了解详细信息。
应用服务和扩展 | 我们添加了新指南，[创建和使用应用扩展](../launch-resume/how-to-create-an-extension.md)，帮助你编写并托管通用 Windows 平台 (UWP) 应用扩展，以便通过用户可从 Microsoft Store 中安装的程序包来扩展你的应用。 </br></br> 我们添加了新指南，[使用服务、扩展和程序包扩展你的应用](../launch-resume/extend-your-app-with-services-extensions-packages.md)，该指南对 Windows 10 中可用于实现应用扩展和组件化的不同技术进行了分类。
后台任务 | 我们添加了三项可帮助你充分利用后台任务的指南：</br></br> * [在后台无期限运行](../launch-resume/run-in-the-background-indefinetly.md)，以使用无任何后台或扩展执行限制的设备上的所有可用资源。 这适用于企业 UWP 应用以及不会提交到 Microsoft Store 的 UWP 应用。 </br></br> * [触发应用内的后台任务](../launch-resume/trigger-background-task-from-app.md)，以激活应用内的后台任务。 </br></br>* [在 UWP 应用更新时运行后台任务](../launch-resume/run-a-background-task-during-updatetask.md)，以创建在 UWP 应用更新时运行的后台任务。
Cortana | 使用 [Cortana 技能工具包](https://docs.microsoft.com/cortana/skills/overview)添加和测试技能，以便扩展 Cortana 的自然功能并使之与你的应用和服务交互。
桌面桥 | 我们添加了三个指南，帮助你将现代化体验带入 Windows 10 上的桌面应用程序： </br>* [增强 Windows 10 的桌面应用程序](../porting/desktop-to-uwp-enhance.md)指南方便查找和参考正确的文件，然后编写代码以便使 Windows 10 用户获得 UWP 体验。 </br></br>* [使用现代 UWP 组件扩展桌面应用程序](../porting/desktop-to-uwp-extend.md)，以并入必须在 UWP 应用容器中运行的现代 XAML UI 以及其他的 UWP 体验。 </br></br>* [将应用程序迁移到通用 Windows 平台](../porting/desktop-to-uwp-migrate.md)，以便在 WPF、Windows 窗体、UWP、Android 以及 iOS 应用程序之间分享代码。
桌面桥打包 | Visual Studio 引入了新的[打包项目](../porting/desktop-to-uwp-packaging-dot-net.md)，能够消除在打包完全受信任的桌面应用程序时必需的所有手动步骤。 只需添加打包项目，参考桌面项目，再按 F5 进行应用调试。 无需手动调整。 相比于使用以往版本的 Visual Studio 的体验，新的简洁体验是一个巨大的改进。
诊断和线程处理 | 新的诊断 API 提供运行应用的相关信息： </br></br>* [AppMemoryReport](https://docs.microsoft.com/uwp/api/Windows.System.AppMemoryReport) 类提供应用预计总提交限制、私有提交使用等信息。 </br>* [AppDiagnosticInfo](https://docs.microsoft.com/uwp/api/windows.system.appdiagnosticinfo) 类现在可以监视应用或任务的执行状态，并且在执行状态改变时发出通知。 </br>* [MemoryManager](https://docs.microsoft.com/uwp/api/windows.system.memorymanager) 类提供了若干新的方法，能够设置应用内存使用限制并报告预计的应用内存使用限制。 </br></br>你可以按优先顺序排列任务，并且使用 [DispatcherQueue](https://docs.microsoft.com/uwp/api/windows.system.dispatcherqueue) 类在不同的线程上运行任务。 通过 [CreateDispatcherQueueController](https://msdn.microsoft.com/library/windows/desktop/mt826210.aspx) 函数，在 Win32 中也可以使用此项功能。
EdgeHTML 16 | 支持 Microsoft Edge 以及基于 JS 的通用 Windows 平台的 Web 平台已更新到 EdgeHTML 16，并且现在加入对 F12 开发人员工具、CSS 网格布局支持以及其他重要功能的重大改进。 </br></br> * Microsoft Edge 中现在支持 [CSS 网格布局](https://docs.microsoft.com/microsoft-edge/dev-guide/css/grid-layout)。 网格布局是一种基于网格的二维布局系统，比使用浮点或脚本定位更能够实现良好的布局流度。</br></br> * 更新了 [Microsoft Edge F12 DevTools 文档](https://docs.microsoft.com/microsoft-edge/f12-devtools-guide)，提高了可靠性和性能。 添加了新功能，以优化你的开发体验。 </br></br>* 仅 Microsoft Edge 中的 [WebVR](https://docs.microsoft.com/microsoft-edge/webvr/) 添加了对[运动控制器](https://docs.microsoft.com/microsoft-edge/webvr/input#controller-buttons)以及各种 [Windows Mixed Reality 头戴显示设备](https://docs.microsoft.com/microsoft-edge/webvr/hardware)的支持。 WebVR 也已经过优化，每秒最多可支持 90 帧。 </br></br> 请参阅 [Microsoft Edge 开发人员指南](https://docs.microsoft.com/microsoft-edge/dev-guide)获取完整的更改以及受支持的最新 API 的列表。
3D 地图元素 | 你可以在地图中添加三维对象。 你可以使用新的 [MapModel3D](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapmodel3d) 类从 [3D 制造格式 (3MF)](http://3mf.io/specification/) 文件中导入 3D 对象。
地图元素样式 | 你可以使用以下两个新的 [MapElement](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapelement) 属性自定义地图元素的外观：[MapStyleSheetEntry](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapelement.MapStyleSheetEntry) 以及 [MapStyleSheetEntryState](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapelement.MapStyleSheetEntryState)。 </br></br>* 你可以使用 [MapStyleSheetEntry](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapelement.MapStyleSheetEntry) 属性使得地图元素看起来是底图的一部分（比如，通过将元素样式设置为地图样式表中的现有输入，比如 *Water*）。 </br></br>* 你可以使用 [MapStyleSheetEntryState](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapelement.MapStyleSheetEntryState) 属性，以便使用地图样式表中的默认状态（比如 *Hover* 和 *Selected*）修改地图元素的外观 ，或者将其覆盖以创建自定义样式。
图层 | 你可以将兴趣点元素添加到[图层](../maps-and-location/display-poi.md#layers)，然后直接将 XAML 绑定到该图层。 将元素分层。 然后，你可以分别操控每一层。 例如，每个层都有自己的事件组，你可以响应特定层的事件，并执行特定于该事件的操作。
地图位置信息 | 在[轻型弹出窗口](../maps-and-location/display-maps.md#placecard)中，你可以将地图展示在 UI 元素的上方、下方或侧面，或者展示在用户触摸的应用区域。  该窗口将在用户更改上下文时关闭。 这使得用户不需要切换到其他应用或浏览器窗口以获取位置信息。
地图服务 | 准备去观光？ 使用新的 [MapRouteOptimization.Scenic](https://docs.microsoft.com/uwp/api/windows.services.maps.maprouteoptimization) 值优化路线以加入最佳观光路线，以及使用 [MapRoute.IsScenic](https://docs.microsoft.com/uwp/api/windows.services.maps.maproute) 了解现有路线中是否已加入观光路线。
媒体捕获 | 文章[使用 MediaFrameReader 处理媒体帧](../audio-video-camera/process-media-frames-with-mediaframereader.md) 已更新，以显示新的 [MultiSourceMediaFrameReader](https://docs.microsoft.com/uwp/api/windows.media.capture.frames.multisourcemediaframereader) 类的使用，你可以通过此类获取来自多个媒体源的与时间关联的帧。 </br></br>[使用 MediaFrameReader 处理媒体帧](../audio-video-camera/process-media-frames-with-mediaframereader.md)已更新，加入了有关缓冲帧采集模式的描述，该模式允许应用请求按序列向应用提供采集帧，这样应用就不会在处理旧帧的时候丢失采集帧。 </br></br>此外，当 **MediaCapture** 对象使用包含一个或多个媒体帧源的媒体帧源组初始化时，你可以创建一个 **[MediaSource](https://docs.microsoft.com/uwp/api/windows.media.core.mediasource)** 对象，以便在 XAML 页面显示 **MediaPlayerElement** 控件中的媒体帧。</br></br>有关详细信息，请参阅[使用 MediaFrameReader 处理媒体帧](../audio-video-camera/process-media-frames-with-mediaframereader.md)。
媒体播放 | 新增部分已添加到基本媒体播放文章，[使用 MediaPlayer 播放音频和视频](../audio-video-camera/play-audio-and-video-with-mediaplayer.md)。 </br></br>* [使用 MediaPlayer 播放球面视频](../audio-video-camera/play-audio-and-video-with-mediaplayer.md)部分向你显示如何播放球面编码的视频，包括调整视野和受支持的格式的视图方向。 </br></br>* [在帧服务器模式中使用 MediaPlayer](../audio-video-camera/play-audio-and-video-with-mediaplayer.md#use-mediaplayer-in-frame-server-mode) 部分向你显示如何将使用 [MediaPlayer](https://docs.microsoft.com/uwp/api/Windows.Media.Playback.MediaPlayer) 播放的媒体的帧复制到 Direct3D 表面。 这将启用应用像素着色器的实时效果等方案。 示例代码显示如何使用 Win2D 为视频播放快速实现模糊效果。
我的人脉 | “我的人脉”允许用户将应用程序中的联系人直接固定到任务栏。 [了解如何将“我的人脉”支持添加到应用程序。](../contacts-and-calendar/my-people-support.md) </br></br>* [我的人脉共享](../contacts-and-calendar/my-people-sharing.md)使用户可以直接通过任务栏的应用程序共享文件。 </br>* [我的人脉通知](../contacts-and-calendar/my-people-support.md)是一种新的 toast 通知类型，用户可以将其发送到他们固定的联系人。
.NET Standard 2.0 | 通用 Windows 平台已经完全实施 [.NET Standard 2.0](https://docs.microsoft.com/dotnet/standard/net-standard#net-implementation-support)。 新版标准加入了显著增加的 .NET API，以及一个适用于你的收藏 NuGet 程序包以及第三方库的兼容性填充码。 </br></br> 如果你希望对准 iOS 和 Android 等其他平台，或者如果你有桌面应用程序并希望创建 UWP 应用，则可以将代码移至 .NET Standard 2.0 类库，然后在各应用版本中重新使用此代码。
固定到任务栏 | 使用新的 TaskbarManager 类，你可以让用户[将你的应用固定到任务栏](../design/shell/pin-to-taskbar.md)。
服务点 | 我们添加了帮助你[了解服务点设备](../devices-sensors/pos-get-started.md)的新指南。 它涵盖的主题包括设备枚举、检查设备功能、声明设备和设备共享等。
语音识别 | 你现在可以使用 [SpeechRecognitionListConstraint](https://docs.microsoft.com/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognitionListConstraint) 以及 Web 服务 [SpeechRecognitionTopicConstraint](https://docs.microsoft.com/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognitionTopicConstraint)，以通过提供一组你认为可能在听写过程中使用的领域特定的关键词来增加听写准确度。
用户活动 | 使用新的 [Windows.ApplicationModel.UserActivities](https://docs.microsoft.com/uwp/api/windows.applicationmodel.useractivities) API，你可以封装一项可以稍后继续以及可能在不同设备上继续的用户任务。

## <a name="publish--monetize-windows-apps"></a>发布 Windows 应用并实现盈利

在此部分中的功能自发布以往版本 Windows 1703 之后已经添加。 这些功能适用于所有的 Windows 开发人员，并且不需要更新的 SDK。

功能 | 说明
 :------ | :------
帐户管理 | 我们现在提供更大的灵活性时[将 Azure AD 租户与你的合作伙伴中心帐户相关联](../publish/associate-azure-ad-with-dev-center.md)添加多个帐户用户。 你可以将多个 Azure AD 租户与一个合作伙伴中心帐户，相关联，或将关联一个 Azure AD 租户与多个合作伙伴中心帐户。
广告 | Microsoft 广告 SDK 现在支持你在应用中显示[本机广告](../monetize/native-ads.md)。 本机广告是基于组件的广告格式，其中每一个广告创意元素（如标题、图像、说明和行动号召文字）都作为单独的元素提供给你。 本机广告目前仅适用于加入试验计划的开发人员，但我们计划不久后将此功能提供给所有的开发人员。
定价和可用性 |  新的定价和可用性选项可用于[计划价格更改](../publish/set-and-schedule-app-pricing.md)和[设置精确的发布日期](../publish/configure-precise-release-scheduling.md)。
Store 分析 API | [Microsoft Store 分析 API](../monetize/access-analytics-data-using-windows-store-services.md) 现在提供一种可用来[下载应用中的错误的 CAB 文件](../monetize/download-the-cab-file-for-an-error-in-your-app.md)的方法。
Store 一览 | Store 一览经过优化，可提供吸引潜在用户的新功能： </br>* 你的应用的 Store 一览现在可以包括[视频预告片](../publish/app-screenshots-and-images.md#trailers)。 </br></br>* 你可以[导入和导出 Store 一览](../publish/import-and-export-store-listings.md)以更快地进行更新，尤其是在你有多种语言版本的一览时。
提交 API | [Microsoft Store 提交 API](../monetize/create-and-manage-submissions-using-windows-store-services.md) 现在可以让你在应用提交中包括[视频预告片](../monetize/manage-app-submissions.md#trailer-object)和[游戏选项](../monetize/manage-app-submissions.md#gaming-options-object)。
定向优惠 | 使用[定向优惠](../publish/use-targeted-offers-to-maximize-engagement-and-conversions.md)可以通过极富吸引力的个性化内容锁定特定目标客户群体，以提升参与度、忠诚度和增加盈利。

## <a name="samples"></a>示例

### <a name="lunch-scheduler"></a>Lunch Scheduler

[Lunch Scheduler](https://github.com/Microsoft/Windows-appsample-lunch-scheduler) 示例能够安排你与朋友和同事的午餐。 创建午餐安排，邀请好友前往相关餐厅，然后应用会帮助实现各相关方的午餐管理。 此应用具有以下亮点：

- 展示与 Facebook、Microsoft Graph 等服务的集成，以进行身份验证、基于图形的操作以及发现好友。
- 使用 Yelp 和必应地图以提供餐馆建议。
- 将 Fluent 设计系统的元素并入 UWP 应用中，包括亚克力、显示和连贯动画。

### <a name="quiz-game"></a>问答游戏

[问答游戏应用（远程系统会话 API）](https://github.com/microsoft/Windows-appsample-remote-system-sessions)示例展示如何在问答游戏场景中使用远程系统会话 API。 主机向邻近的设备发送问题，参与者在自己的设备上回答问题。

远程系统会话 API 允许设备托管可被其他附近设备发现的会话。 然后，附近设备可以加入此会话，并将消息发送到主机和其他参与者。 
