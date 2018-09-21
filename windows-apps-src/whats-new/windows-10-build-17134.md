---
author: QuinnRadich
title: 面向开发人员的 Windows 10 中的新增工具和功能
description: Windows 10 版本 17134 和新开发人员工具提供由通用 Windows 平台支持的工具、功能和体验。
keywords: 新增功能, 新功能, 更新, 刷新, 功能, 新, Windows 10, 最新, 开发人员, 17134
ms.author: quradic
ms.date: 4/10/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
ms.localizationpriority: medium
ms.openlocfilehash: e2f12190c405ad611cf5b884b82c4a430aa5264f
ms.sourcegitcommit: a160b91a554f8352de963d9fa37f7df89f8a0e23
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/21/2018
ms.locfileid: "4122850"
---
# <a name="whats-new-in-windows-10-for-developers-build-17134"></a>面向开发人员的 Windows 10 版本 17134 中的最近更新

Windows 10 版本 17134（又称 4 月更新或版本 1803）与 Visual Studio 2017 和更新的 SDK 相结合，为打造出色的通用 Windows 平台应用提供了丰富的工具、功能和体验。 只需在 Windows10 上[安装工具和 SDK](http://go.microsoft.com/fwlink/?LinkId=821431)，你便可以随时[创建新的通用 Windows 应用](../get-started/create-uwp-apps.md)，或了解如何使用 [Windows 上的现有应用代码](../porting/index.md)。

该版本集合了 Windows 开发人员感兴趣的新增和改进功能及指南。 有关添加到 Windows SDK 的新命名空间的完整列表，请参阅 [Windows 10 版本 17134 API 更改](windows-10-build-17134-api-diff.md)。 有关 Windows 10 突出功能的详细信息，请参阅 [Windows 10 中的酷炫功能](http://go.microsoft.com/fwlink/?LinkId=823181)。 另请参阅 [Windows 开发人员平台功能](https://developer.microsoft.com/windows/platform/features)，了解有关 Windows 平台过去已添加的及将来要添加的功能的详尽概述。

## <a name="design--ui"></a>设计和 UI

功能 | 描述
 :------ | :------
自适应和交互式 Toast 通知 | 使用自适应和交互式通知增强你的应用。 使用我们的[更新的 Toast 通知指南](../design/shell/tiles-and-notifications/adaptive-interactive-toasts.md)开始，并了解有关图像大小限制、进度栏和添加输入选项的新信息。<br><br>计划的 Toast 通知现在支持 [ExpirationTime](https://docs.microsoft.com/uwp/api/windows.ui.notifications.scheduledtoastnotification.expirationtime#Windows_UI_Notifications_ScheduledToastNotification_ExpirationTime)。
内容链接 | 新[内容链接](../design/controls-and-patterns/content-links.md)控件提供了访问文本控件中的丰富的嵌入数据的方法，让用户无需离开应用上下文即可查找和使用有关人员或位置的更多信息。
设计示例 | BuildCast 示例已添加到[设计工具包和示例](../design/downloads/index.md)页面。 BuildCast 是一个端到端示例，用于展示 Fluent Design 系统，以及通用 Windows 平台的其他功能。
嵌入式手写 | 笔输入功能已添加到[文本控件](../design/controls-and-patterns/text-controls.md)，让用户可以使用 Windows Ink 直接在文本框内书写。 当用户书写时，文本会被转换为保持自然书写感觉的脚本。
Fluent Design 更新 | 我们已经更新了许多 Fluent Design 页面，增加了新的信息和指南： </br> * [Fluent Design 概述](../design/fluent-design-system/index.md)已针对最新的 Fluent 功能相应更新。 </br> * [突出显示](../design/style/reveal.md)提供新的主题和自定义控件指南。 </br> * [导航历史记录和向后导航](../design/basics/navigation-history-and-backwards-navigation.md)已经改进，提供详细示例、设备优化指南和自定义行为指南。
焦点导航 | 新的[焦点导航](../design/input/focus-navigation.md)主题介绍如何优化依赖非指向输入类型（例如键盘、游戏手柄或遥控器）的用户 UWP 应用程序。 此外，[编程焦点导航](../design/input/focus-navigation-programmatic.md)介绍可用于增强这些体验的 API。
键盘快捷方式 | 我们关于[键盘快捷键](../design/input/keyboard-accelerators.md)的指南已更新新的可用性信息。 将工具提示添加到键盘快捷键，并将标签添加到控件以提高可发现性，或使用新的 API 替代默认键盘快捷键行为。
页面布局 | 我们更新了 [XAML 页面布局](../design/layout/layouts-with-xaml.md)文档，增加了新的动态布局和视觉状态信息。 借助这些功能，可以更好地控制应用中元素的位置如果响应和适应可用视觉空间。
下拉以刷新 | [下拉刷新](../design/controls-and-patterns/pull-to-refresh.md)控件允许用户通过下拉数据列表来检索更多数据。 它被广泛用于带有触摸屏的设备。
导航视图 | [导航视图](../design/controls-and-patterns/navigationview.md)控件通过提供可折叠导航菜单让你的应用拥有顶级导航。 此控件实现导航窗格或汉堡包菜单、模式，并自动使其窗格的显示模式适应不同窗口大小。
显示焦点 | 新的[显示焦点](../design/style/reveal-focus.md)效果为 Xbox One 等体验和电视屏幕提供照明。 当用户将游戏板或键盘焦点移向可聚焦元素（如按钮）时，它会将这些元素的边框进行动画处理。
声音 | XAML 现在通过 **SpatialAudioMode** 属性支持 3D 音频。 请参阅[声音](../design/style/sound.md)了解有关如何配置的信息。
磁贴 | 基于 JavaScript 的 UWP 应用现支持[可追踪磁贴通知](../design/shell/tiles-and-notifications/chaseable-tile-notifications.md)。<br><br>辅助磁贴和锁屏提醒通知是[从桌面桥应用现在支持](../design/shell/tiles-and-notifications/secondary-tiles-desktop-pinning.md#send-tile-notifications)。
树状视图 | [树视图](../design/controls-and-patterns/tree-view.md)控件支持分层列表，其中具有包含嵌套项的展开节点和折叠节点。 它可用于说明你的用户界面中的文件夹结构或嵌套关系。
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
C++/WinRT | [C++/WinRT](https://docs.microsoft.com/windows/uwp/cpp-and-winrt-apis/)是适用于 Windows 运行时 (WinRT) API 的全新、完全标准的现代 C++17 语言投影。 它仅在标头文件中实现，旨在为你提供对现代 Windows API 的一流访问。 通过 C++/WinRT，你可以使用任何符合标准的 C++17 编译器编写和使用 WinRT API。 对于 C++ 应用程序（从 Win32 到 UWP），使用 C++/WinRT 来保持代码的标准、现代和干净，并且使应用程序轻量且快速。
控制台 UWP 应用 | 你现在可以编写在控制台窗口（如 DOS 或 PowerShell 控制台窗口）中运行的 C++ /WinRT 或 /CX UWP 控制台应用。 控制台应用使用控制台窗口进行输入和输出。 UWP 控制台应用可以发布到 Microsoft Store，在应用列表中有对应条目，并有可以固定到“开始”菜单的主要磁贴。 有关详细信息，请参阅[创建通用 Windows 平台控制台应用](../launch-resume/console-uwp.md)
扩展应用清单功能 | 应用程序包清单架构中添加了几项功能，包括: 广泛的文件系统访问、为服务点设备启用条形码扫描程序、定义 UWP 控制台应用等等。 请参阅[应用清单在 Windows 10 中的更改](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/what-s-changed-in-windows-10)了解详细信息。
辅助功能技术 (AT) 支持的标志和标题 | 标志和标题定义帮助辅助技术（如屏幕阅读器）用户高效导航的用户界面的各个部分。 有关详细信息，请参阅[标志和标题](../design/accessibility/landmarks-and-headings.md)。
机器学习 | Windows 机器学习允许你生成对 Windows 10 设备上本地保存的经过预先训练的机器学习模型进行评估的应用。 若要了解有关此平台的详细信息，请参阅 [Windows 机器学习](https://docs.microsoft.com/windows/ai/)。 </br> [MachineLearning](https://docs.microsoft.com/uwp/api/windows.ai.machinelearning.preview) 命名空间包含可让应用加载机器学习模型、作为输入绑定数据以及评估结果的类。
地图控件 | [MapControl](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapcontrol) 类有一个名为 **Region** 的新属性，可用于根据特定地区（例如，州或省/自治区/直辖市）的语言显示地图控件中的内容。
地图元素 | [MapElement](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapelement) 类有一个名为 **IsEnabled** 的新属性，可用于指定用户是否可以与 [MapElement](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapelement) 交互。
地图位置信息 | [PlaceInfo](https://docs.microsoft.com/uwp/api/windows.services.maps.placeinfo) 类包含新方法 **CreateFromAddress**，可用于使用地址和显示名称创建 [PlaceInfo](https://docs.microsoft.com/uwp/api/windows.services.maps.placeinfo)。
地图服务 | [MapRouteDrivingOptions](https://docs.microsoft.com/uwp/api/windows.services.maps.maproutedrivingoptions) 类包含一个名为 **DepartureTime** 的新属性，可用于使用对于指定日期和时间很常见的交通状况计算路线。
多实例 UWP 应用 | UWP 应用可以选择支持多个实例。 如果多实例 UWP 应用的一个实例正在运行，并发生后续的激活请求，平台将不会激活现有实例。 相反，它将创建一个新实例，而且在单独的进程中运行。 有关详细信息，请参阅[创建多实例通用 Windows 应用](../launch-resume/multi-instance-uwp.md)。
程序包资源索引 API 和自定义生成系统 | 通过[程序包资源索引 (PRI) API](https://docs.microsoft.com/windows/uwp/app-resources/pri-apis-custom-build-systems)，你可以开发 UWP 应用的资源的自定义生成系统。 生成系统可以创建、版本和转储 PRI 文件，以适应 UWP 应用所需的任何级别的复杂性。 如果具有当前使用 MakePri.exe 命令行工具的自定义生成系统，则我们建议更改为调用 PRI API，因为它们提供了更佳的性能和控制。
PlayReady | Microsoft PlayReady 是一组技术，用于防止数字内容在未经授权的情况下使用。 PlayReady 可以在各种类型的设备和应用上跨所有操作系统运行。 [了解如何将 PlayReady 合并到你的应用中。](https://docs.microsoft.com/playready/)
私人受众 | 如果你希望只有指定的所选用户可以看到应用的 Store 一览，请使用新的**私人受众**选项。 对于指定组中用户以外的任何用户，应用将不可发现或不可用。 此选项适用于 beta 版测试，因为它让你可以将应用分配给测试人员，而任何其他人则无法获取该应用，甚至无法查看该应用的 Store 一览。 有关详细信息，请参阅[选择可见性选项](../publish/choose-visibility-options.md)。
渐进式 Web 应用 | Microsoft Edge 和 UWP web 应用现支持[渐进式 Web 应用 (PWA)](https://docs.microsoft.com/microsoft-edge/progressive-web-apps)! </br> * 使用[基于标准的 Web 技术](https://developer.mozilla.org/Apps/Progressive)和[功能检测](https://docs.microsoft.com/microsoft-edge/progressive-web-apps/windows-features)可增强 Web 应用，以提供本机应用体验，包括推送通知、离线支持和操作系统集成，同时在尚不支持 PWA 技术的浏览器或平台上仍然维持良好的基线 Web 应用体验。 </br> * 向应用[添加清单文件](https://docs.microsoft.com/microsoft-edge/progressive-web-apps/get-started)，使其能够在整个 UWP 设备系列中安装（包括安全 [Windows 10 S 模式设备](https://www.microsoft.com/windows/windows-10-s)），并从 [Microsoft Store](https://docs.microsoft.com/microsoft-edge/progressive-web-apps/microsoft-store)分发。 </br> PWA 由*托管的 Web 应用*自然发展而来，但是由于*服务工作进程*、*缓存*和*推送 API*，因此它对离线方案提供基于标准的支持。
屏幕捕获 | [Windows.Graphics.Capture 命名空间](https://docs.microsoft.com/uwp/api/windows.graphics.capture)提供从屏幕或应用程序窗口获取帧的 API，以创建用于生成协作和交互式体验的视频流或快照。 请参阅[屏幕捕获](../audio-video-camera/screen-capture.md)了解详细信息。
系统触发器 | [CustomSystemEventTrigger](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.customsystemeventtrigger) 让你可以在操作系统未提供所需系统触发器时定义系统触发器。 例如，当硬件驱动程序和 UWP 应用均属于第三方，且硬件驱动程序需要引发其应用处理的自定义事件时。 例如，需要在插入音频插孔时通知用户的音频卡。
用户活动 | 新的 [UserActivity 文档](../launch-resume/useractivities.md)解释了如何帮助用户恢复他们在应用中，甚至是在多台设备中所执行的操作。</br>**UserActivitySessionHistoryItem** 类有检索近期用户活动的新方法。 请参阅 [GetRecentUserActivitiesAsync](https://docs.microsoft.com/uwp/api/windows.applicationmodel.useractivities.useractivitychannel.getrecentuseractivitiesasync) 及其重载了解详细信息。
Windows Mixed Reality API | 为支持不断扩大的 Windows Mixed Reality 平台，已向 [Windows.Graphic.Holographic](https://docs.microsoft.com/uwp/api/Windows.Graphics.Holographic) 和 [Windows.UI.Input.Spatial](https://docs.microsoft.com/uwp/api/Windows.UI.Input.Spatial) 命名空间添加了新 API。
Windows Mixed Reality 文档 | Windows Mixed Reality 开发人员指南[现托管在 docs.microsoft.com。](https://docs.microsoft.com/windows/mixed-reality/) 就像在这些 UWP 文档中，你现在可以包含 GitHub 问题的反馈意见或提交自己所做的贡献通过拉取请求。

## <a name="publish--monetize-windows-apps"></a>发布 Windows 应用并实现盈利

功能 | 描述
 :------ | :------
从 Microsoft Store 下载并安装程序包更新 | 我们已更新[下载和安装来自 Microsoft Store 的程序包更新](../packaging/self-install-package-updates.md)，并提供关于如何下载和安装程序包更新而不向用户显示通知 UI 的新指南和示例，卸载可选程序包，以及获取关于应用的下载和安装队列中的程序包的信息。
使用特定市场的当地货币输入自由格式价格 | 在覆盖特定市场的应用基价后，你将不再受限于选择其中一个标准价格段；现在你可以选择使用市场的当地货币输入自由格式价格。 有关详细信息，请参阅[设置和计划应用定价](../publish/set-and-schedule-app-pricing.md)。 **此功能适用于所有的 Windows 开发人员，并且不需要更新的 SDK。**
存储上下文 | [StoreContext](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext) 类已经更新，增强了一系列新方法。 这些方法管理应用的程序包更新和加载项的下载和安装。
订阅加载项现在可供所有开发人员使用 | 按照自动定期帐单期间创建和发布订阅加载项来在你的应用和游戏中销售数字产品（如应用功能或数字内容）。 有关详细信息，请参阅[启用应用加载项订阅](../monetize/enable-subscription-add-ons-for-your-app.md)。 **此功能适用于所有的 Windows 开发人员，并且不需要更新的 SDK。**

## <a name="videos"></a>视频

自 Fall Creator Update 以来已经发布了以下视频，其中突出展示了 Windows 10 中面向开发人员的新的和改进的功能。

### <a name="accessibility-tools-for-windows-developers"></a>面向 Windows 开发人员的辅助功能工具

Windows 10 SDK 拥有多种可帮助你测试并改进应用辅助功能的工具。 Inspect 和 AccEvent 工具帮助你确保应用可供所有人使用。 [观看视频](https://www.youtube.com/watch?v=ce0hKQfY9B8&list=PLWs4_NfqMtoycBFndriDmkQlMLwflyoFF&t=0s&index=1)了解这些工具，然后[阅读更多有关辅助功能测试的内容](../design/accessibility/accessibility-testing.md)以了解详细信息。

### <a name="creating-3d-app-launchers-for-windows-mixed-reality"></a>创建适用于 Windows Mixed Reality 的 3D 应用启动器

3D 启动器为用户提供在其混合现实家庭环境中放置应用的真正立体表示形式的唯一途径。 [观看视频](https://www.youtube.com/watch?v=TxIslHsEXno)了解如何准备 3D 模型并将其指定为应用的启动器，然后[阅读开发人员文档](https://developer.microsoft.com/windows/mixed-reality/implementing_3d_app_launchers)并[查看我们的设计指南](https://developer.microsoft.com/windows/mixed-reality/3d_app_launcher_design_guidance)了解详细信息。

### <a name="creating-a-uwp-console-app"></a>创建 UWP 控制台应用

现在可以创建在 PowerShell 或 DOS 控制台窗口中运行的 UWP 应用。 [观看视频](https://www.youtube.com/watch?v=bwvfrguY20s&t=0s&index=1&list=PLWs4_NfqMtoycBFndriDmkQlMLwflyoFF)了解如何操作，然后[查看文档](../launch-resume/console-uwp.md)以了解详细信息。 

### <a name="how-to-use-windows-ml-in-your-app"></a>如何在应用中使用 Windows ML

Windows 机器学习允许你生成对 Windows 10 设备上本地保存的经过预先训练的机器学习模型进行评估的应用。 [观看视频](https://www.youtube.com/watch?v=8MCDSlm326U&index=2&list=PLWs4_NfqMtoycBFndriDmkQlMLwflyoFF)了解快速演练，然后[阅读文档](https://docs.microsoft.com/windows/uwp/machine-learning/)了解详细内容。

### <a name="motion-controller-tracking"></a>运动控制器跟踪

运动控制器在 Windows Mixed Reality 中代表用户的手。 [观看视频](https://www.youtube.com/watch?v=rkDpRllbLII)了解运动控制器在混合现实头戴显示设备的视野内外如何工作，并[在此处阅读有关控制器跟踪的详细信息。](https://developer.microsoft.com/windows/mixed-reality/motion_controllers#controller_tracking_state%E2%80%9D)

### <a name="package-a-net-app-in-visual-studio"></a>在 Visual Studio 中创建 .NET 应用程序包

现在能够比以往更加轻松地将桌面应用添加到通用 Windows 平台。 [观看视频](https://www.youtube.com/watch?v=fJkbYPyd08w)了解如何打包要分发的 .NET 应用，然后[查看此页面](../porting/desktop-to-uwp-packaging-dot-net.md)了解更多详细信息。

### <a name="xbox-live-creators-program"></a>Xbox Live 创意者计划

Xbox Live 创意者计划支持开发人员快速将他们的 UWP 游戏发布到 Xbox One 和 Windows 10。 [观看视频](https://www.youtube.com/watch?v=zpFfHHBkVq4)以了解该程序，然后[查看此页面](https://www.xbox.com/developers/creators-program)开始操作。

### <a name="one-dev-question---why-was-docments-and-settings-renamed-users"></a>一个开发人员的问题 - 为什么文档和设置被重命名为用户？

想知道为什么要重命名文档和设置目录？ [Raymond Chen 解释了该名称的来源，以及更改它的原因](https://www.youtube.com/watch?v=4vDHQewVmM8&index=1&list=PLWs4_NfqMtoxjy3LrIdf2oamq1coolpZ7)。 有关 Windows 和其历史的更多发展的详细信息，请查看[Raymond 的博客。](https://blogs.msdn.microsoft.com/oldnewthing/)


## <a name="samples"></a>示例

### <a name="coloring-book"></a>彩色画册

[彩色画册示例](https://github.com/Microsoft/Windows-appsample-coloringbook)已进行重大更新，以使用自定义墨迹干燥 API 合并高级的墨迹方案，包括已改进的墨迹呈现性能。 它还支持在作品定义的区域线条内进行填充和着色。 

### <a name="photo-lab"></a>照片实验室

已更新[照片实验室示例](https://github.com/Microsoft/Windows-appsample-photo-lab)，以便从图片库中加载图像，并在图片库中存在大量文件时使用数据虚拟化提高性能。 此外，示例中的图像编辑页面现在使用 [XamlCompositionBrushBase](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.xamlcompositionbrushbase) 类来应用效果。