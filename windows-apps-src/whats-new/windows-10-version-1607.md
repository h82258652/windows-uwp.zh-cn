---
author: QuinnRadich
title: "面向开发人员的 Windows10 中的新增功能"
description: "Windows10 版本 1607 和新开发人员工具将提供受新通用 Windows 平台支持的工具、功能和体验。"
keywords: "新增功能, 新功能, 更新, 功能, 新, Windows10, 1607, 7 月, 最新"
ms.author: quradic
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
ms.assetid: f95cd87b-f959-4148-a9bd-ba0b90d16e71
ms.openlocfilehash: bac2705bd57c0b6874c58eaac66b4ecb19ebb5af
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="whats-new-in-windows-version-1607"></a>Windows 版本 1607 中的新增功能

Windows10 版本 1607 和 Windows 开发人员工具更新继续提供受通用 Windows 平台支持的工具、功能和体验。 只需在 Windows10 上[安装工具和 SDK](http://go.microsoft.com/fwlink/?LinkId=821431)，你便可以随时[创建新的通用 Windows 应用](https://msdn.microsoft.com/library/windows/apps/bg124288)，或了解如何使用 [Windows 上的现有应用代码](https://msdn.microsoft.com/library/windows/apps/mt238321)。

下面是开发人员感兴趣的新功能和改进的功能列表。 对于添加到 Windows SDK 的新命名空间的原始列表，请参阅 [Windows10 版本 1607 API 更改](windows-10-version-1607-api-diff.md)。 有关此更新中亮点功能的详细信息，请参阅 [Windows10 中的酷炫功能](http://go.microsoft.com/fwlink/?LinkId=823181)。

## <a name="windows-10-version-1607---july-2016"></a>Windows10 版本 1607 - 2016 年 7 月

功能 | 描述
 :---- | :----
XAML 元素的访问密钥 | 可以使用新的 [**AccessKey**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.uielement.accesskey.aspx) 属性和 [**AccessKeyManager**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.input.accesskeymanager.aspx) 改进应用的键盘辅助功能。
动态 GIF 支持 | XAML [**Image**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.image.aspx) 元素支持动态 GIF。 可以使用 [**BitmapImage**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.imaging.bitmapimage.aspx) 上的以下新成员控制播放：[**AutoPlay**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.imaging.bitmapimage.autoplay.aspx)、[**IsAnimatedBitmap**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.imaging.bitmapimage.isanimatedbitmap.aspx)、[**IsPlaying**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.imaging.bitmapimage.isplaying.aspx)、[**Play**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.imaging.bitmapimage.play.aspx)、[**Stop**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.imaging.bitmapimage.stop.aspx)。
应用可扩展性 | 编写 UWP 应用的[扩展](http://aka.ms/appextensibility)。 允许 Windows 应用商店应用托管其他 Windows 应用商店应用提供的内容。  从这些应用发现、枚举和访问只读内容。
评估测试 | [参加测验](https://technet.microsoft.com/edu/windows/take-tests-in-windows-10?f=255&MSPPError=-2147217396)是基于浏览器的应用，为高风险测试提供锁定的在线评估。 使用[参加测验 API](../apps-for-education/take-a-test-api.md) 防止学生在测试时使用其他计算机或 Internet 资源。
后台智能传输服务 (BITS) | 现在可以在 PowerShell 远程会话中使用 [BITS](https://msdn.microsoft.com/library/windows/desktop/bb968799.aspx) COM API 和 PowerShell cmdlet（如果可用）。 管理没有本地登录功能的 WindowsServer 2016 Technical Preview 版本时，此功能尤其有用。 BITS 作业通过在会话的用户帐户上下文中运行的 PowerShell 远程会话启动，并且仅在至少有一个与该用户帐户关联的活动本地登录会话或 PowerShell 远程会话时才会取得进展。 有关如何管理传输运行时间较长的会话的详细信息，请参阅[管理 PowerShell 远程会话](https://msdn.microsoft.com/library/windows/desktop/ee663885.aspx#manage_ps_remote_sessions)。<br/><br/>在支持 [BITS 帮助程序令牌](https://msdn.microsoft.com/library/windows/desktop/dd904467.aspx)的以前版本的 Windows 中，作业所有者实际上必须要有管理员权限才能设置帮助程序令牌。 在此版本中，BITS 作业所有者现在无需成为管理员即可设置帮助程序令牌，前提是帮助程序令牌没有管理员功能。 通过使这些令牌在低权限的 NetworkService 帐户而非在具有管理权限的帐户下运行，这可以减少后台下载或更新工具的漏洞占用。
改进了对颜色字体的支持 | Direct2D 现在支持呈现更多种颜色字体格式，这允许开发人员在支持 Direct2D 的应用中使用比以前更多的字体类型。 这包括对以下内容的支持： <br/>&bull; “sbix”OpenType 表，在字体中支持颜色位图内容。<br/>&bull; “SVG”OpenType 表，在字体中支持 SVG 内容。<br/>&bull; “CBDT”OpenType 表，在字体中支持颜色位图内容。 <br/><br/>启用 **D2D1_DRAW_TEXT_OPTIONS_ENABLE_COLOR_FONT** 标志时，Direct2D 自动支持这些颜色字体格式。  有关详细信息，请参阅以下主题： <br>&bull;[**ID2D1DeviceContext4**](http://go.microsoft.com/fwlink/?LinkId=822793)<br>&bull;[**D2D1_DRAW_TEXT_OPTIONS**](http://go.microsoft.com/fwlink/?LinkId=822794)<br>&bull;[**ID2D1SvgGlyphStyle**](http://go.microsoft.com/fwlink/?LinkId=822795)     
CommandBar 动态溢出 | 没有足够空间显示全部内容时，[**Commandbar**](https://msdn.microsoft.com/windows/uwp/controls-and-patterns/app-bars) 中的主要命令现在可自动移至溢出菜单。
组合交互 API| 新的 [**Windows.UI.Composition.Interactions**](https://msdn.microsoft.com/library/windows/apps/windows.ui.composition.interactions.aspx) 命名空间允许你访问促进动画和效果的输入。  可视化层中的新 API 系列有助于使应用即使在 UI 线程停顿和繁忙时也可以即时响应。  
Windows.UI.Composition | [**Windows.UI.Composition**](https://msdn.microsoft.com/library/windows/apps/windows.ui.composition.aspx) 命名空间添加了对许多功能的支持，包括： <br/><br/>&bull; 阴影 - 允许你向应用提供现实世界的深入体验 <br/>&bull; 场景照明 - 通过照射控件并为应用程序提供不同效果，可以多角度查看控件和 UI。<br/>&bull;  模糊效果 - 允许你聚焦正确信息，模糊其他信息。 可以动画方式模糊 UI，以使它们更生动。  <br/>&bull; 隐式动画 - 隐式动画有助于在视觉效果属性更改时对视觉进行动画处理。 可以使用隐式动画执行布局动画，即当应用布局更改时，可以在其新位置对它们进行动画处理。 <br/>&bull; CompositionBackdropBrush - CompositionBackdropBrush 是新的画笔类型，可用于选择当前 RenderTarget 作为效果输入内容。<br/>&bull; LayerVisual - 允许你将某个效果应用到某个视觉效果集合中。 例如 UI 的灰度部分指示 UI 的禁用部分<br/>&bull; CompositionMashBrush - 允许你指定不透明蒙板<br/>&bull; 剪裁转换 - 允许将转换应用到剪裁矩形<br/>&bull; 表面画笔转换 - 允许将转换应用到 CompositionSurfaceBrush<br/>&bull; CompositionNineGridBrush - 允许你在图像上指定九网格大小调整插入，或者创建矩形纯色边框。<br/>&bull; 表达式字符串添加 - 表达式字符串支持新函数、表达式运算符和关键字。
连接的动画 | [**ConnectedAnimationService**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.animation.connectedanimationservice.aspx) 允许你当用户在场景或页面之间移动时运行动画。 有关详细信息，请参阅此[连接的动画](https://channel9.msdn.com/Events/Build/2016/P485)视频。
连接的应用 | 发现连接了云的设备或附近设备，并生成在这些设备之间无缝过渡的体验。 有关详细信息，请参阅[连接的应用和设备](http://aka.ms/Bttm1d)。
桌面应用转换器 | 桌面应用转换器是一款工具，允许你将为 .NET 4.6.1 或 Win32 编写的现有桌面应用发布到通用 Windows 平台 (UWP)。
针对辅助功能开发应用 | 使用[应用辅助功能指南](https://developer.microsoft.com/windows/accessible-apps)设计非独占软件，提升可用性和客户满意度。 从辅助功能技术产品案例中获得启发。 在新的开发人员中心查找向所有人提供应用的信息。
Direct3D | 已向 Direct 3D 文档中添加了许多新主题。 有关这些更新的详细信息，请参阅 [Direct3D 12 新版本页面](https://msdn.microsoft.com/library/windows/desktop/mt748631(v=vs.85).aspx)中的 **Windows10 版本 1607**。
游戏 - 街机摇杆和赛车方向盘支持 | [**Windows.Gaming.Input**](https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.aspx) 支持两类新的输入设备：街机摇杆和赛车方向盘。 这允许标题支持将街机摇杆和赛车方向盘设备用作一类设备，无需编写特定于这些设备的个别版本的代码。 这支持这些类的所有 Xbox 360 和 Xbox One 设备以及选定的电脑 (HID) 设备。
游戏 - 力回馈支持 | [**Windows.Gaming.Input.ForceFeedback**](https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.forcefeedback.aspx) API 支持控制电脑 (HID) 赛车方向盘的力回馈。
游戏 - OEM 支持为新的输入设备自定义 WinRT 类库 | [**Windows.Gaming.Input.Custom**](https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.custom.aspx) API 支持第三方外部设备 OEM 为 Xbox 360 和 Xbox One 外部设备编写自定义 WinRT 类库。     
全球化 | 新的 [**Windows.Globalization.PhoneNumberFormatting**](https://msdn.microsoft.com/library/windows/apps/windows.globalization.phonenumberformatting.aspx) 命名空间中的类在全局支持范围内支持设置电话号码格式、验证和匹配电话号码。 新的类支持许多本地标准格式，甚至还支持在输入时提供部分数字的增量格式。
应用内购买和应用许可证 | [**System.Services.Store**](https://msdn.microsoft.com/library/windows/apps/windows.services.store.aspx) 命名空间提供新的 API，可执行应用内购买和访问应用的应用商店许可证信息。 有关详细信息，请参阅[启用应用内产品购买](https://msdn.microsoft.com/windows/uwp/monetize/enable-in-app-product-purchases)。
InkToolbar | [**InkToolbar**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Controls.InkToolbar.aspx) 是通用 Windows 应用控件，包含可激活在关联 **InkCanvas** 中与墨迹相关的功能的一组可自定义和可扩展按钮。<br/><br/>默认情况下，工具栏包括用于绘图、擦除、突出显示和显示标尺的按钮。 根据功能类别，其他设置和命令（例如墨迹颜色、笔划粗细、全部擦除等）均在浮出控件中提供。<br/><br/>**InkToolbar** 还可使用自己笔、工具和其他墨迹功能进行自定义。
MAX_PATH 限制已删除 | MAX_PATH 限制已从常见的 Win32 文件和目录 API 中删除。 该行为已选择。 [命名文件、路径和命名空间](https://msdn.microsoft.com/library/windows/desktop/aa365247.aspx)的**最大路径长度限制**部分的详细信息。
相机 - 媒体帧阅读器 |新的 [**Windows.Media.Capture.Frames**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.media.capture.frames.aspx) 命名空间提供阅读一个或多个可用源媒体帧的 API，这些可用源包括颜色、深度、红外线相机、音频设备，甚至是自定义的帧源（例如生成骨架跟踪帧的帧源）。 此功能旨在由实时处理媒体帧的应用使用，例如增强现实和感知深度的相机应用。
媒体播放 | 在应用中播放媒体的推荐方式是使用 [**MediaPlayer**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlayer) 类，即使用轻量 [**MediaPlayerElement**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Controls.MediaPlayerElement) 类以 XAML 形式呈现媒体（如果需要）。 **MediaPlayer** 类的改进包括能够向特定终结点播放音频、引入 [**MediaPlaybackSession**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlaybackSession) 类用于管理播放器状态、收缩和缩放视频、能够向 [**Windows.UI.Composition**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Composition) 表面呈现视频，并且提供 [**MediaTimelineController**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.MediaTimelineController) 类用于同步多台媒体播放器的播放进度。<br>使用 [**MediaSource**](https://msdn.microsoft.com/library/windows/apps/dn930905) 打开媒体项时，可以立即检测由于完全或部分编解码器不受支持而导致的媒体故障。<br><br>新的 [**MediaBreakManager**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaBreakManager) 类可以快速简单地向任何 [**MediaPlaybackItem**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlaybackItem) 类添加媒体中断，因此允许你轻松创建、计划和管理媒体中断，例如音频和视频广告。<br><br>**MediaPlayer** 类现在自动集成到系统媒体传输控件 (SMTC) 中。 新的 [**MediaPlaybackCommandManager**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlaybackCommandManager) 类允许你部分或完全覆盖 SMTC 命令。<br><br>现在可以使用全新的一个进程模型在后台播放媒体，这比之前的两个进程模型执行起来要更简单也更轻松。 新的清单功能允许你告知系统应用需要在后台播放媒体，并且新的应用生命周期事件 [**EnteredBackground**](https://msdn.microsoft.com/library/windows/apps/Windows.ApplicationModel.Core.CoreApplication.EnteredBackground) 和 [**LeavingBackground**](https://msdn.microsoft.com/library/windows/apps/Windows.ApplicationModel.Core.CoreApplication.LeavingBackground) 使你可以在后台运行时释放未使用的资源。
Microsoft Edge | Microsoft Edge 添加了对提取、流和信标 API 的支持。 现在提取替换了 **XMLHttpRequests**，从而添加用于请求和响应的低级别功能。 还添加了流式传输数据源的功能。 与在能够读取前缓冲整个源相反，流式传输支持读取源中的数据块。 信标 API 支持通过信标（单向请求）以有效方式向服务器发送信息，例如关键应用程序和测量信息。 信标 API 完全异步，无需处理请求，因此使其成为非阻止性的请求。<br/><br/>有关 Microsoft Edge 中新 API 的详细信息，请参阅 Microsoft Edge 开发人员指南中的[新增功能](https://developer.microsoft.com/microsoft-edge/platform/documentation/dev-guide/)。  
新的应用生命周期事件 | 已向[应用生命周期](https://msdn.microsoft.com/windows/uwp/launch-resume/app-lifecycle?f=255&MSPPError=-2147217396)添加两个新事件，使检测应用可见性变得简单。
单个进程后台活动 | 执行后台代码更加容易，并且不再需要创建后台任务。 可作为独立进程在后台运行代码，或直接在前台应用程序中运行。 有关详细信息，请参阅[单个进程的后台活动模型](https://blogs.windows.com/buildingapps/2016/06/07/background-activity-with-the-single-process-model/#z3OjoTRbQMVX1puj.99)。
“人脉”应用的社交可扩展性和联系人卡片 API | 将基于应用的消息、语音通话和视频通话操作直接集成到联系人卡片。 使用联系人关联向“人脉”应用的“新增功能”视图提供社交内容。
StreamSocket | 向 [**StreamSocket**](https://msdn.microsoft.com/library/windows/apps/windows.networking.sockets.streamsocket.aspx) 添加新的 **GetEndpointPairsAsync** 方法，支持获取 DNS 在特定主机名查找特定服务后返回的终结点列表。 这在该服务实际上托管在多台服务器上时很有用，因此，代码可以尝试连接多个潜在服务提供程序，并使用第一个建立的连接。
磁贴和通知 | 锁屏提醒通知现在显示在任务栏中。 <br /><br /> 应用程序现在可以使用新的 [**Windows.Ui.Notifications.Management**](https://msdn.microsoft.com/library/windows/apps/windows.ui.notifications.management.aspx) 命名空间向多太设备发送特定用户的通知。
文本排序 | 已向 [**Windows.Data.Text**](https://msdn.microsoft.com/library/windows/apps/windows.data.text.aspx) 添加新方法，从而支持使用音素排序顺序来对文本排序。 这主要是为了在排序数据中使用（例如日语的专有名词），在此情况中，务必要按语音顺序对名称排序，而非按字符代码点顺序排序。
XAML ComboBox 中的文本搜索 | 当用户在组合框中键入内容时，会显示与用户键入的字符串相匹配的候选项。
UI 自动化 | UI 自动化提供程序现在可以通过调用 [**UiaRaiseChangesEvent**](https://msdn.microsoft.com/library/windows/desktop/mt733044(v=vs.85).aspx) 函数通知系统文档的更改。
Xbox One 上的 UWP | 此更新以作为 Xbox One 上的通用 Windows 平台 (UWP) 的第一个完整版本为特色。 它包含新功能，对现有功能的更新和 Bug 修补程序。 有关详细信息，请参阅 [Xbox One 上的 UWP](https://msdn.microsoft.com/windows/uwp/xbox-apps/index) 主题。
Web 到应用链接 | 将应用与网站关联起来。 当用户打开指向网站的链接时，会打开应用。 有关详细信息，请参阅[使用 Uri 处理程序支持 Web 到应用链接](http://aka.ms/Hxfg4m)。
WebSockets | 添加了对 [**MessageWebSockets**](https://msdn.microsoft.com/library/windows/apps/windows.networking.sockets.messagewebsocket.aspx) 和 [**StreamWebSockets**](https://msdn.microsoft.com/library/windows/apps/windows.networking.sockets.streamwebsocket.aspx) 的支持，用于查看服务器证书、查看 SSL 协商期间服务器发送的中间证书、执行自定义服务器证书验证，以及指定要忽略的某些服务器证书错误。
Windows 信息保护 (WIP) API | [**WIP**](https://msdn.microsoft.com/windows/uwp/enterprise/wip-hub) 是台式计算机、笔记本电脑、平板电脑和手机上的一组用于移动设备管理 (MDM) 的功能。 WIP 可使企业更好地控制数据在企业管理的设备上的处理方式。 <br/><br/>可使用 WIP API 生成尊重数据策略的应用，同时将员工的个人数据分离，使其不受这些策略影响。 策略管理员将信任应用，允许它们使用组织数据。 员工也愿意他们的个人数据在设备上保持不变，即使取消注册组织的移动设备管理 (MDM) 或完全退出组织也是如此。
Windows IOT 核心版 | Windows IoT 核心版现在完全支持 Raspberry Pi 3 以及远程屏幕体验，使用户能够远程查看和控制在 IoT 核心版设备上运行的 UWP 应用程序。
具有配套 (IoT) 设备的 Windows 解锁 | 配套设备是可以与 Windows10 桌面版一起使用来增强用户身份验证体验的设备。 通过使用[配套设备框架](https://msdn.microsoft.com/windows/uwp/security/companion-device-unlock)，即使是在 Windows Hello 不可用时（例如 Windows10 桌面版缺少相机进行面部身份验证或缺少指纹读取器设备），配套设备也能提供丰富的 Microsoft Passport 体验。
Winsock | 通过设置 TCP_FASTOPEN 套接字选项，TCP 套接字现在可使用 [Winsock](https://tools.ietf.org/html/rfc7413) 进行配置，以使用 [RFC 7413](https://tools.ietf.org/html/rfc7413) TCP Fast Open。
