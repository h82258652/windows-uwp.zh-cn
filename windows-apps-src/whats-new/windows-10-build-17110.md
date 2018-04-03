---
author: QuinnRadich
title: 面向开发人员的 Windows 10 中的新增工具和功能
description: Windows 10 版本 17110 和新开发人员工具提供由通用 Windows 平台支持的工具、功能和体验。
keywords: 新增功能, 新功能, 更新, 刷新, 功能, 新, Windows 10, 最新, 开发人员, 17110 预览版
ms.author: quradic
ms.date: 3/07/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
ms.localizationpriority: high
ms.openlocfilehash: 5d416ad13c2e689c5265164c0269244a387a6c7f
ms.sourcegitcommit: ef5a1e1807313a2caa9c9b35ea20b129ff7155d0
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/08/2018
---
# <a name="whats-new-in-windows-10-for-developers-sdk-preview-build-17110"></a>面向开发人员的 Windows 10 SDK 预览版 17110 中的最近更新

Windows 10 SDK 预览版 17110 与 Visual Studio 2017 和更新的 SDK 相结合，为打造出色的通用 Windows 平台应用提供了丰富的工具、功能和体验。 只需在 Windows10 上[安装工具和 SDK](http://go.microsoft.com/fwlink/?LinkId=821431)，你便可以随时[创建新的通用 Windows 应用](../get-started/create-uwp-apps.md)，或了解如何使用 [Windows 上的现有应用代码](../porting/index.md)。

该 SDK 预览版集合了 Windows 开发人员感兴趣的新增和改进功能及指南。 现在，这些功能向 [Windows 预览体验计划](https://insider.windows.com/en-us/)的成员开放，并且将在 Windows 10 的下一个主要更新中公开发布。 有关添加到 Windows SDK 的新命名空间的完整列表，请参阅 [Windows 10 版本 17110 API 更改](windows-10-build-17110-api-diff.md)。有关 Windows 10 重要功能的详细信息，请参阅 [Windows 10 中的酷炫功能](http://go.microsoft.com/fwlink/?LinkId=823181)。 另请参阅 [Windows 开发人员平台功能](https://developer.microsoft.com/windows/platform/features)，了解有关 Windows 平台过去已添加的及将来要添加的功能的详尽概述。

## <a name="design--ui"></a>设计和 UI

功能 | 描述
 :------ | :------
自适应和交互式 Toast 通知 | 使用自适应和交互式通知增强你的应用。 使用我们的[更新的 Toast 通知指南](../design/shell/tiles-and-notifications/adaptive-interactive-toasts.md)开始，并了解有关图像大小限制、进度栏和添加输入选项的新信息。
内容链接 | 新[内容链接](../design/controls-and-patterns/content-links.md)控件提供了访问文本控件中的丰富的嵌入数据的方法，让用户无需离开应用上下文即可查找和使用有关人员或位置的更多信息。
设计示例 | BuildCast 示例已添加到[设计工具包和示例](../design/downloads/index.md)页面。 BuildCast 是一个端到端示例，用于展示 Fluent Design 系统，以及通用 Windows 平台的其他功能。
嵌入式手写 | 笔输入功能已添加到[文本控件](../design/controls-and-patterns/text-controls.md)，让用户可以使用 Windows Ink 直接在文本框内书写。 当用户书写时，文本会被转换为保持自然书写感觉的脚本。
Fluent Design 更新 | 我们已经更新了许多 Fluent Design 页面，增加了新的信息和指南： </br> * [Fluent Design 概述](../design/fluent-design-system/index.md)已针对最新的 Fluent 功能相应更新。 </br> * [突出显示](../design/style/reveal.md)提供新的主题和自定义控件指南。 </br> * [导航历史记录和向后导航](../design/basics/navigation-history-and-backwards-navigation.md)已经改进，提供详细示例、设备优化指南和自定义行为指南。
页面布局 | 我们更新了 [XAML 页面布局](../design/layout/layouts-with-xaml.md)文档，增加了新的动态布局和视觉状态信息。 借助这些功能，可以更好地控制应用中元素的位置如果响应和适应可用视觉空间。
下拉以刷新 | [下拉刷新](../design/controls-and-patterns/pull-to-refresh.md)控件允许用户通过下拉数据列表来检索更多数据。 它被广泛用于带有触摸屏的设备。
导航视图 | [导航视图](../design/controls-and-patterns/navigationview.md)控件通过提供可折叠导航菜单让你的应用拥有顶级导航。 此控件实现导航窗格或汉堡包菜单、模式，并自动使其窗格的显示模式适应不同窗口大小。
显示焦点 | 新的[显示焦点](../design/style/reveal-focus.md)效果为 Xbox One 等体验和电视屏幕提供照明。 当用户将游戏板或键盘焦点移向可聚焦元素（如按钮）时，它会将这些元素的边框进行动画处理。
声音 | XAML 现在通过 **SpatialAudioMode** 属性支持 3D 音频。 请参阅[声音](../design/style/sound.md)了解有关如何配置的信息。
树视图 | [树视图](../design/controls-and-patterns/tree-view.md)控件支持分层列表，其中具有包含嵌套项的展开节点和折叠节点。 它可用于说明你的用户界面中的文件夹结构或嵌套关系。
写入样式 | 我们已升级并扩充了有关语音和声调的文章内容，现在变为[写入样式指南](../design/style/writing-style.md)。 这些新信息提供在应用中创建有效文本的准则，并为控件编写（如错误消息或对话）推荐最佳做法。

## <a name="gaming"></a>游戏
功能 | 描述
 :------ | :------
游戏开发入门 | 对开发适用于 Windows10 的游戏感兴趣？ 新的[游戏开发入门](../gaming/getting-started.md)页面为你提供了自行完成设置、注册并准备好提交应用和游戏所需完成事项的完整概述。
图形适配器 | 已添加以下 DXGI API，它们是关于图形适配器的首选项和删除： </br> * [IDXGIFactory6](https://msdn.microsoft.com/library/windows/desktop/mt814823) 界面实现了根据给定的 GPU 首选项枚举图形适配器的单一方法。 </br> * [DXGIDeclareAdapterRemovalSupport](https://msdn.microsoft.com/library/windows/desktop/mt814821) 函数允许执行指示恢复任何正在删除的图形设备的进程。 </br> * [DXGI_GPU_PREFERENCE](https://msdn.microsoft.com/library/windows/desktop/mt814822) 枚举描述应用在其上运行的 GPU 的首选项。


## <a name="develop-windows-apps"></a>开发 Windows 应用

功能 | 描述
 :------ | :------
自适应卡 | [自适应卡](https://docs.microsoft.com/adaptive-cards/) 是一种开放的卡交换格式，让开发人员可以通过常见且一致的方式交换 UI 内容。 它们将内容描述为可以自动呈现以适应主机应用程序的外观和感觉的 JSON 对象。
应用资源组 | [AppResourceGroupInfo](https://docs.microsoft.com/uwp/api/windows.system.appresourcegroupinfo) 类有新的方法，你可以用来启动向已暂停、活动（已恢复）和已终止状态应用的转换。
广泛的文件系统访问 | **broadFileSystemAccess** 功能授予应用与当前运行该应用的用户相同的文件系统访问权限，而无需文件选取器样式提示符。 有关详细信息，请参阅[文件访问权限](../files/file-access-permissions.md)和[应用功能声明](../packaging/app-capability-declarations.md)中的 **broadFileSystemAccess** 一项。
控制台 UWP 应用 | 你现在可以编写在控制台窗口（如 DOS 或 PowerShell 控制台窗口）中运行的 C++ /WinRT 或 /CX UWP 控制台应用。 控制台应用使用控制台窗口进行输入和输出。 UWP 控制台应用可以发布到 Microsoft Store，在应用列表中有对应条目，并有可以固定到“开始”菜单的主要磁贴。 有关详细信息，请参阅[创建通用 Windows 平台控制台应用](../launch-resume/console-uwp.md)
辅助功能技术 (AT) 支持的标志和标题 | 标志和标题定义帮助辅助技术（如屏幕阅读器）用户高效导航的用户界面的各个部分。 有关详细信息，请参阅[标志和标题](../design/accessibility/landmarks-and-headings.md)。
机器学习 | Windows 机器学习允许你生成对 Windows 10 设备上本地保存的经过预先训练的机器学习模型进行评估的应用。 若要了解有关此平台的详细信息，请参阅 [Windows 机器学习](../machine-learning/index.md)。 </br> [MachineLearning](https://docs.microsoft.com/uwp/api/windows.ai.machinelearning.preview) 命名空间包含可让应用加载机器学习模型、作为输入绑定数据以及评估结果的类。
地图控件 | [MapControl](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapcontrol) 类有一个名为 **Region** 的新属性，可用于根据特定地区（例如，州或省/自治区/直辖市）的语言显示地图控件中的内容。
地图元素 | [MapElement](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapelement) 类有一个名为 **IsEnabled** 的新属性，可用于指定用户是否可以与 [MapElement](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapelement) 交互。
地图位置信息 | [PlaceInfo](https://docs.microsoft.com/uwp/api/windows.services.maps.placeinfo) 类包含新方法 **CreateFromAddress**，可用于使用地址和显示名称创建 [PlaceInfo](https://docs.microsoft.com/uwp/api/windows.services.maps.placeinfo)。
地图服务 | [MapRouteDrivingOptions](https://docs.microsoft.com/uwp/api/windows.services.maps.maproutedrivingoptions) 类包含一个名为 **DepartureTime** 的新属性，可用于使用对于指定日期和时间很常见的交通状况计算路线。
多实例 UWP 应用 | UWP 应用可以选择支持多个实例。 如果多实例 UWP 应用的一个实例正在运行，并发生后续的激活请求，平台将不会激活现有实例。 相反，它将创建一个新实例，而且在单独的进程中运行。 有关详细信息，请参阅[创建多实例通用 Windows 应用](../launch-resume/multi-instance-uwp.md)。
PlayReady | Microsoft PlayReady 是一组技术，用于防止数字内容在未经授权的情况下使用。 PlayReady 可以在各种类型的设备和应用上跨所有操作系统运行。 [了解如何将 PlayReady 合并到你的应用中。](https://docs.microsoft.com/playready/)
屏幕捕获 | [Windows.Graphics.Capture 命名空间](https://docs.microsoft.com/uwp/api/windows.graphics.capture)提供从屏幕或应用程序窗口获取帧的 API，以创建用于生成协作和交互式体验的视频流或快照。 请参阅[屏幕捕获](../audio-video-camera/screen-capture.md)了解详细信息。
系统触发器 | [CustomSystemEventTrigger](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.customsystemeventtrigger) 让你可以在操作系统未提供所需系统触发器时定义系统触发器。 例如，当硬件驱动程序和 UWP 应用均属于第三方，且硬件驱动程序需要引发其应用处理的自定义事件时。 例如，需要在插入音频插孔时通知用户的音频卡。
用户活动 | **UserActivitySessionHistoryItem** 类有检索近期用户活动的新方法。 请参阅 [GetRecentUserActivitiesAsync](https://docs.microsoft.com/uwp/api/windows.applicationmodel.useractivities.useractivitychannel#Windows_ApplicationModel_UserActivities_UserActivityChannel_GetRecentUserActivitiesAsync_System_Int32_) 及其重载了解详细信息。
Windows Mixed Reality | 为支持不断扩大的 Windows Mixed Reality 平台，已向 [Windows.Graphic.Holographic](https://docs.microsoft.com/uwp/api/Windows.Graphics.Holographic) 和 [Windows.UI.Input.Spatial](https://docs.microsoft.com/uwp/api/Windows.UI.Input.Spatial) 命名空间添加了新 API。

## <a name="publish--monetize-windows-apps"></a>发布 Windows 应用并实现盈利

功能 | 描述
 :------ | :------
使用特定市场的当地货币输入自由格式价格 | 在覆盖特定市场的应用基价后，你将不再受限于选择其中一个标准价格段；现在你可以选择使用市场的当地货币输入自由格式价格。 有关详细信息，请参阅[设置和计划应用定价](../publish/set-and-schedule-app-pricing.md)。 **此功能适用于所有的 Windows 开发人员，并且不需要更新的 SDK。**
存储上下文 | [StoreContext](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext) 类已经更新，增强了一系列新方法。 这些方法管理应用的程序包更新和加载项的下载和安装。
订阅加载项现在可供所有开发人员使用 | 按照自动定期帐单期间创建和发布订阅加载项来在你的应用和游戏中销售数字产品（如应用功能或数字内容）。 有关详细信息，请参阅[启用应用加载项订阅](../monetize/enable-subscription-add-ons-for-your-app.md)。 **此功能适用于所有的 Windows 开发人员，并且不需要更新的 SDK。**

## <a name="videos"></a>视频

自 Fall Creator Update 以来已经发布了以下视频，其中突出展示了 Windows 10 中面向开发人员的新的和改进的功能。

### <a name="package-a-net-app-in-visual-studio"></a>在 Visual Studio 中创建 .NET 应用程序包

现在能够比以往更加轻松地将桌面应用添加到通用 Windows 平台。 [观看视频](https://www.youtube.com/watch?v=fJkbYPyd08w)了解如何打包要分发的 .NET 应用，然后[查看此页面](../porting/desktop-to-uwp-packaging-dot-net.md)了解更多详细信息。

### <a name="xbox-live-creators-program"></a>Xbox Live 创意者计划

Xbox Live 创意者计划支持开发人员快速将 UWP 游戏发布到 Xbox One 和 Windows 10。[观看视频](https://www.youtube.com/watch?v=zpFfHHBkVq4)了解此计划，然后[查看此页面](https://www.xbox.com/developers/creators-program)开始使用。

### <a name="creating-3d-app-launchers-for-windows-mixed-reality"></a>创建适用于 Windows Mixed Reality 的 3D 应用启动器

3D 启动器为用户提供在其混合现实家庭环境中放置应用的真正立体表示形式的唯一途径。 [观看视频](https://www.youtube.com/watch?v=TxIslHsEXno)了解如何准备 3D 模型并将其指定为应用的启动器，然后[阅读开发人员文档](https://developer.microsoft.com/windows/mixed-reality/implementing_3d_app_launchers)并[查看我们的设计指南](https://developer.microsoft.com/windows/mixed-reality/3d_app_launcher_design_guidance)了解详细信息。

### <a name="motion-controller-tracking"></a>运动控制器跟踪

运动控制器在 Windows Mixed Reality 中代表用户的手。 [观看视频](https://www.youtube.com/watch?v=rkDpRllbLII)了解运动控制器在混合现实头戴显示设备的视野内外如何工作，并[在此处阅读有关控制器跟踪的详细信息。](https://developer.microsoft.com/windows/mixed-reality/motion_controllers#controller_tracking_state%E2%80%9D)

### <a name="accessibility-tools-for-windows-developers"></a>面向 Windows 开发人员的辅助功能工具

Windows 10 SDK 拥有多种可帮助你测试并改进应用辅助功能的工具。 Inspect 和 AccEvent 工具帮助你确保应用可供所有人使用。 [观看视频](https://www.youtube.com/watch?v=ce0hKQfY9B8&list=PLWs4_NfqMtoycBFndriDmkQlMLwflyoFF&t=0s&index=1)了解这些工具，然后[阅读更多有关辅助功能测试的内容](../design/accessibility/accessibility-testing.md)以了解详细信息。