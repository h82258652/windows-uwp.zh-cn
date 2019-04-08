---
title: 面向开发人员的 Windows 10 的新增功能、工具和功能
description: Windows 10 生成 17763 和新开发人员工具提供工具、 功能和通用 Windows 平台为后盾的体验。
keywords: 是什么的新增功能新更新，更新功能，新的、 Windows 10 最新，开发人员，17763
ms.date: 10/03/2018
ms.topic: article
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 2b172844e75d9af3d0112e03f155708af3ca6bed
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2019
ms.locfileid: "57637742"
---
# <a name="whats-new-in-windows-10-for-developers-build-17763"></a>对于开发人员，生成 17763，什么是 Windows 10 中的新增功能

Windows 10 生成 17763 (也称为 2018 年 10 月更新或版本 1809年)，在与 Visual Studio 2017 和更新后的 SDK 结合使用，提供工具、 功能和体验，以使非同寻常的通用 Windows 平台应用。 只需在 Windows 10 上[安装工具和 SDK](https://go.microsoft.com/fwlink/?LinkId=821431)，你便可以随时[创建新的通用 Windows 应用](../get-started/create-uwp-apps.md)，或了解如何使用 [Windows 上的现有应用代码](../porting/index.md)。

该版本集合了 Windows 开发人员感兴趣的新增和改进功能及指南。 新的命名空间添加到 Windows SDK 的完整列表，请参阅[Windows 10 生成 17763 API 更改](windows-10-build-17763-api-diff.md)。 有关 Windows 10 突出功能的详细信息，请参阅 [Windows 10 中的酷炫功能](https://go.microsoft.com/fwlink/?LinkId=823181)。 另请参阅 [Windows 开发人员平台功能](https://developer.microsoft.com/windows/platform/features)，了解有关 Windows 平台过去已添加的及将来要添加的功能的详尽概述。

## <a name="design--ui"></a>设计和 UI

功能 | 描述
 :------ | :------
应用图标和徽标 | [应用图标和徽标页](../design/style/app-icons-and-logos.md)已重新编写，现在显示最新的 Visual Studio 图标工具并提供有关将映像添加到 Microsoft Store 中的应用程序的列表的信息。
设计登陆页面 | [更新登录页设计](https://developer.microsoft.com/windows/apps/design)具有 UWP 设计方面的问题和信息的最新新增的 Fluent 设计的一眼概述。
Fluent 设计控件 | 以下新的 UI 控件已添加，以增强 Fluent 设计系统和应用的 apparence: </br> * [CommandBarFlyout](../design/controls-and-patterns/command-bar-flyout.md)就可以在 UI 画布上的项的上下文中显示常见用户任务。 </br> * [DropDownButton](../design/controls-and-patterns/buttons.md#create-a-drop-down-button)， [SplitButton](../design/controls-and-patterns/buttons.md#create-a-split-button)，和[ToggleSplitButton](../design/controls-and-patterns/buttons.md#create-a-toggle-split-button)按钮控件提供专用功能，以增强您的应用程序的用户界面。 </br> * [菜单栏](../design/controls-and-patterns/menus.md)在水平行中显示一组多个顶级菜单。 </br> * [NavigationView](../design/controls-and-patterns/navigationview.md)现在支持顶部导航栏中，在其中您的应用程序具有较小数量的导航选项，且需要更多空间进行内容的用例。 </br> * [TreeView](../design/controls-and-patterns/tree-view.md)已经过增强，支持数据绑定、 项模板，并拖放。
Fluent Design 更新 | 已对以下 Fluent 设计页面视觉对象更新和细微的更改： </br> * [对齐方式，填充边距](../design/layout/alignment-margin-padding.md) </br> * [颜色](../design/style/color.md) </br> * [Windows 应用的 Fluent 设计](../design/fluent-design-system/index.md) </br> * [应用程序设计简介](../design/basics/design-and-ui-intro.md) </br> * [导航基础知识](../design/basics/navigation-basics.md) </br> * [响应式设计技术](../design/layout/responsive-design.md) </br> * [屏幕大小和断点](../design/layout/screen-sizes-and-breakpoints-for-responsive-design.md) </br> * [样式概述](../design/style/index.md) </br> * [编写样式](../design/style/writing-style.md) </br> 此外，我们已重写与他们的内容区域的所有新信息的以下页面： </br> * [图标](../design/style/icons.md)现在提供实用的建议使用的图标和使其成为可单击。 </br> * [版式](../design/style/typography.md)整合了相似的文章，将所有内容放在一个位置更新的指南和插图中的信息。
注视输入和交互 | [注视交互](../design/input/gaze-interactions.md)允许应用程序，以跟踪用户的视线移动、 关注和基于位置和移动用户眼是否存在。 此功能可用作辅助技术，并为游戏和传统的输入的设备不可用其他交互式方案提供了机会。
手写视图 | [HandwritingView](../design/controls-and-patterns/text-handwriting-view.md)文本框和 RichEditBox 是新的墨迹输入图面。 使用自己的笔书写表面到展开的控件，用户可以点击文本控件。 本指南介绍如何管理和自定义应用程序中的 HandwritingView。
Fluent 设计中的动作 | 使用 Fluent 设计系统中的动作在发展，基于计时、 简化、 方向性和重力的基础知识。 应用这些基础知识有助于指导用户完成您的应用程序，并将其与他们的数字体验连接通过专用于反映将自然的世界。 了解这些文章中的详细信息： </br> * [Motion 概述](../design/motion/index.md)已更新以反映这些基础知识。 </br> * [Motion 的实践](../design/motion/motion-in-practice.md)示例展示了如何以应用在应用内的这些基础知识。 它还包含允许轻松的内插，当 XAML 元素的属性更改时，旧的和新值之间的隐式动画的信息。 </br> * [方向性和重力](../design/motion/directionality-and-gravity.md)固化您的应用程序的用户的思维模式。 </br> * [计时并降低了](../design/motion/timing-and-easing.md)将真实性添加到您的应用程序中的动作。 </br> * [XAML 属性动画](../design/motion/xaml-property-animations.md)允许您直接属性进行动画处理的 XAML 元素中，而无需与基础组合视觉对象进行交互。
页面过渡 | [页面过渡](../design/motion/page-transitions.md)导航应用中的页面之间的用户。 它们可帮助用户了解他们的导航层次结构中的位置，并提供有关页面之间的关系的反馈。
文本缩放 | 新[文本缩放指南](../design/input/text-scaling.md)介绍了如何更新您的应用程序以适应缩放行为，提供用户跨操作系统和单个应用程序更改相对字体大小的功能的新文本。 而不是使用放大镜应用程序 （这通常只需将放大屏幕的区域内的所有内容，并引入了其自己的可用性问题）、 更改显示分辨率或依赖于 DPI 缩放 （用以调整大小基于显示和典型查看的所有内容距离），用户可以快速访问设置，以调整只是文本，范围为 100%（默认大小） 的大小高达 225%。
工具包 | [Adobe XD 和 Adobe Illustrator 工具包](../design/downloads/index.md)使用新功能进行了更新。 这些设计工具包为设计 UWP 应用提供控件和布局模板。
用户界面发出命令 | 更新到[UWP 命令基础结构](../design/basics/commanding-basics.md)包括更好的封装的命令对象 （行为、 标签、 图标、 键盘快捷键、 访问密钥和说明） 和一组标准的常见命令包括剪切、 复制将粘贴、 退出等，这消除了需要手动设置这些属性。 </br> 新[XamlUICommand](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.input.xamluicommand)类用于定义命令行为执行操作时调用的交互式 UI 元素提供基类。 这是为父类[StandardUICommand](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.input.standarduicommand)，公开的一组标准的平台具有预定义属性的命令。 
Windows UI 库 | [Windows 用户界面库](https://aka.ms/winui-docs)是一组控件和其他用户界面元素为 UWP 应用提供的 NuGet 包。 这些包也是与早期版本的 Windows 10 兼容的因此即使用户不具有最新的操作系统版本，适用于您的应用程序。 </br> 什么是 Windows 用户界面库中的详细信息，请参阅[API 命名空间包含在 NuGet 包中的此列表。](https://docs.microsoft.com/uwp/api/overview/winui/)

## <a name="develop-windows-apps"></a>开发 Windows 应用

功能 | 描述
 :------ | :------
条形码扫描仪 | [条码扫描器](https://docs.microsoft.com/windows/uwp/devices-sensors/pos-barcodescanner)已重新组织，和使用更多详细信息和代码片段改进文档。 我们还添加了新主题[获取和了解条形码数据](https://docs.microsoft.com/windows/uwp/devices-sensors/pos-barcodescanner-scan-data)，其中解释了如何获取和处理来自条码扫描器的数据。
C++/WinRT | [C + + WinRT](https://aka.ms/cppwinrt)包含许多新功能、 更改和此版本的修补程序。 有新的函数和基类，这些类以支持您实现您自己[集合的属性和集合类型](/windows/uwp/cpp-and-winrt-apis/collections); 而现在可以使用[{Binding}](/windows/uwp/xaml-platform/binding-markup-extension) XAML 标记扩展使用 C + + WinRT运行时类 (有关代码示例，请参阅[数据绑定概述](/windows/uwp/data-binding/data-binding-quickstart))。 有关新功能和更改此版本中的所有内容的完整说明，请参阅[新增 C + + WinRT](../cpp-and-winrt-apis/news.md)。</br></br>其他新的 C + + WinRT 内容包括：[XAML 的自定义控件](/windows/uwp/cpp-and-winrt-apis/xaml-cust-ctrl);[作者 COM 组件](/windows/uwp/cpp-and-winrt-apis/author-coclasses);[值类别](/windows/uwp/cpp-and-winrt-apis/cpp-value-categories); 并[强和弱引用](../cpp-and-winrt-apis/weak-references.md)。
C + + WinRT 代码示例 | 我们添加了 250 C + + WinRT 代码列表，这些主题中我们的文档，随附的现有 C + + /cli CX 代码示例。
相关指南 | 我们已更新[我们的相关指南](https://github.com/MicrosoftDocs/windows-uwp/blob/docs/CONTRIBUTING.md)UWP 文档。 工作流和发布到我们的文档的外部内容的期望，阐明了该新指南。
DirectX 图形基础结构 (DXGI) | 针对缺失的 DXGI Api 添加了新文档，并且 Windows 10 上进行演示时，我们已经提供有关最佳实践的文章。 </br> * [为了获得最佳性能，使用 DXGI 翻转模式](https://docs.microsoft.com/windows/desktop/direct3ddxgi/for-best-performance--use-dxgi-flip-model):指导如何充分利用性能和效率，最新版本的 Windows 上的演示文稿堆栈。 </br> * [IDXGIOutput6::CheckHardwareCompositionSupport 方法](https://docs.microsoft.com/windows/desktop/api/dxgi1_6/nf-dxgi1_6-idxgioutput6-checkhardwarecompositionsupport):通知应用程序，支持硬件拉伸。 </br> * [DXGI_HARDWARE_COMPOSITION_SUPPORT_FLAGS 枚举](https://docs.microsoft.com/windows/desktop/api/dxgi1_6/ne-dxgi1_6-dxgi_hardware_composition_support_flags):介绍支持哪些级别的硬件组成。
入门 | 我们[开始](../get-started/index.md)内容已大大增加了新的主题，熟悉 Windows 10 开发人员可能会如何完成以下常见任务提供信息和指导： </br> * [构建窗体](../get-started/construct-form-learning-track.md) </br> * [显示在列表中的客户](../get-started/display-customers-in-list-learning-track.md) </br> * [保存和加载设置](../get-started/settings-learning-track.md) </br> * [使用的文件](../get-started/fileio-learning-track.md)
地图样式表编辑器 | 使用新[地图样式表编辑器](https://www.microsoft.com/p/map-style-sheet-editor/9nbhtcjt72ft?rtc=1#activetab=pivot:overviewtab)应用程序能够以交互方式自定义地图添加到你的应用程序的外观。
Microsoft 了解 | 新[Microsoft 了解站点](https://www.microsoft.com/learning/default.aspx)为 Microsoft 开发人员提供新的动手学习和培训机会。 目前，Microsoft 了解提供的 Microsoft 365，Microsoft Azure、 Office 365 和 Windows Server 培训和认证。
记事本 | [已更新记事本](https://aka.ms/ant-man)、 添加缩放、 自动换行围绕查找/替换和对 Unix/Linux (LF) 和 Mac (CR) 行尾的支持。
Project Rome | [项目罗马](https://docs.microsoft.com/windows/project-rome/)现在跨受支持的平台和 Sdk 中提供一致的编程体验。 </br>  新[Microsoft Graph 通知](https://developer.microsoft.com/graph/docs/concepts/notifications-concept-overview)项目罗马用于提供您的应用程序以用户为中心、 跨平台通知平台。
屏幕截图 | 新[URI 方案](../launch-resume/launch-screen-snipping.md)让新代码段后，以编程方式打开应用或启动代码段和草图的应用程序与批注的特定映像。
桌面应用程序中的 UWP 控件 | 现在，Windows 10 支持你在 WPF、Windows 窗体和 C++ Win32 桌面应用程序中使用 UWP 控件。 这意味着你可以使用只能通过 UWP 控件（例如 Windows Ink）和支持 Fluent Design System 的控件提供的最新 Windows 10 UI 功能来增强现有桌面应用程序的外观和功能。 此功能被称为*XAML 群岛*。 </br> 我们提供多种方式来使用在应用程序，具体取决于正在使用的应用程序平台中的 XAML 岛。 WPF 和 Windows 窗体应用程序可以使用一组中的控件[Windows 社区工具包](https://docs.microsoft.com/windows/uwpcommunitytoolkit/)提供一种面向设计人员的开发体验。 C + + Win32 应用程序必须使用*托管 API 的 UWP XAML*中[Windows.UI.Xaml.Hosting](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting)命名空间。 有关详细信息，请参阅[桌面应用程序中的 UWP 控件](../xaml-platform/xaml-host-controls.md)。 </br> **注意：** Api 和控件，XAML 群岛是目前以开发者预览版形式提供。 尽管我们鼓励您试用这些原型代码中现在，但不建议你使用它们在生产代码中这一次。
Windows 机器学习 | [Windows 机器学习](https://docs.microsoft.com/windows/ai/)已经现在正式发布，为前沿的机器学习模型提供更快地分析和支持等功能。 为了支持想要将其集成到他们的应用程序开发人员，我们创建了新的文档站点具有多个新的和更新资源： </br> * [教程：创建 Windows Machine Learning 桌面应用程序 （c + +）](https://docs.microsoft.com/windows/ai/get-started-desktop):本教程演示如何构建用于桌面的简单 Windows 机器学习应用程序。 </br> * [教程：创建的 Windows 机器学习 UWP 应用程序 (C#)](https://docs.microsoft.com/windows/ai/get-started-uwp):使用 Windows 机器学习本分步教程中创建第一个 UWP 应用程序。 </br> * [Windows.AI.MachineLearning Namespace](https://docs.microsoft.com/uwp/api/windows.ai.machinelearning):已更新为最新版本的 Windows 10 SDK，API 参考和开发人员现在可以使用此 API 的 Win32 和 UWP 应用程序。
Windows Mixed Reality | 开发人员可以现在受硬件保护请求后台缓冲区纹理受显示硬件，则允许应用程序从源，如 PlayReady 使用受硬件保护的内容。 硬件保护支持的和设置是适用于主层使用的新属性[Windows.Graphics.Holographic.HolographicCamera](https://docs.microsoft.com/uwp/api/windows.graphics.holographic.holographiccamera)，并通过四层[Windows.Graphics.Holographic.HolographicQuadLayerUpdateParameters](https://docs.microsoft.com/uwp/api/windows.graphics.holographic.holographicquadlayerupdateparameters)。

## <a name="iot-core"></a>IoT 核心版

功能 | 描述
 :------ | :------
AssignedAccessSettings | [AssignedAccessSettings 类](https://docs.microsoft.com/uwp/api/windows.system.userprofile.assignedaccesssettings)启用调用不同的方法和属性访问用户的分配特定设备的访问权限设置。
默认应用程序概述 | [Windows 10 IoT Core 默认应用](https://docs.microsoft.com/windows/iot-core/develop-your-app/iotcoredefaultapp)已更新的新特性和功能，例如天气、 墨迹书写，和音频。
仪表板 | [Windows 10 Iot 核心版仪表板](https://docs.microsoft.com/windows/iot-core/tutorials/quickstarter/devicesetup)现在允许开发人员使用 Dragonboard 410 C 或 NXP 刷写到其设备上的自定义 FFUs。
屏幕键盘 | [为 IoT 设备屏幕键盘](https://docs.microsoft.com/windows/iot-core/develop-your-app/onscreenkeyboard)现在作为 Windows 桌面版使用相同的触摸键盘组件。 这样，例如听写模式、 IME 支持，以及一套完整的输入值范围的功能。
在登录对话框的标题栏 | Windows 10 IoT Core 现在提供了配置选项[对话框的标题栏系统](https://docs.microsoft.com/windows/iot-core/develop-your-app/signindialogtitlebars)。
唤醒触摸 | [唤醒触摸](https://docs.microsoft.com/windows/iot-core/learn-about-hardware/wakeontouch)使你的设备的屏幕，能够在不使用时快速开启时用户触摸屏幕时关闭。
Windows.System.Update | 新[Windows.System.Update 命名空间](https://docs.microsoft.com/uwp/api/windows.system.update)使系统更新的交互式控制。 此命名空间功能仅适用于 Windows 10 IoT Core。

## <a name="web-development"></a>Web 开发

功能 | 描述
 :------ | :------
EdgeHTML 18 | Windows 10 2018 年 10 月更新随附有[EdgeHTML 18](https://docs.microsoft.com/microsoft-edge/dev-guide)、 最新 Microsoft Edge 浏览器和用于 UWP 应用的 JavaScript 引擎的更新。 EdgeHTML 18 带来了经过改进和扩展支持 Web 身份验证 API、 新的 WebView 控件功能，和的详细信息 ！ 在工具方面，EdgeHTML 18 到 Edge DevTools 和 Edge DevTools 协议将 WebDriver 的新功能和自动更新和增强功能。 请查看[新增 EdgeHTML 18](https://docs.microsoft.com/microsoft-edge/dev-guide)并[DevTools 中最新的 Windows 10 更新 (EdgeHTML 18)](https://docs.microsoft.com/microsoft-edge/devtools-guide/whats-new)有关所有详细信息。
渐进式 Web 应用 | Windows 10 JavaScript 应用程序 (web 应用中运行*WWAHost.exe*进程) 现在支持可选[每个应用程序后台脚本](https://docs.microsoft.com/en-us/microsoft-edge/dev-guide#progressive-web-apps)的激活的任何视图之前启动和运行持续时间过程。 与此，可以监视和修改导航、 跨导航跟踪状态，监视导航错误和在激活视图之前运行代码。 如果指定为[ `StartPage` ](https://docs.microsoft.com/en-us/uwp/schemas/appxpackage/appxmanifestschema2010-v2/element-application)中你[应用程序清单](https://docs.microsoft.com/en-us/uwp/schemas/appxpackage/appx-package-manifest)，为的新实例的脚本公开的每个应用程序的视图 (windows) [ `WebUIView` ](https://docs.microsoft.com/en-us/uwp/api/windows.ui.webui.webuiview)类，为常规 (Win32) 提供相同的事件、 属性和方法[WebView](https://docs.microsoft.com/en-us/uwp/api/windows.web.ui.iwebviewcontrol)。
Web API 扩展 | 一系列[旧 Microsoft API 扩展](https://developer.mozilla.org/docs/Web/API/Microsoft_API_extensions)已添加到跨浏览器 web 开发的 Mozilla 开发人员网络文档。 这些 API 扩展 Internet Explorer 或 Microsoft Edge 是唯一的并补充了 MDN web 文档中的兼容性和浏览器支持的现有信息。旧版 Microsoft [CSS 扩展](https://developer.mozilla.org/docs/Web/CSS/Microsoft_Extensions)并[JavaScript 扩展](https://developer.mozilla.org/docs/Web/JavaScript/Microsoft_JavaScript_extensions)也可用，并且您可以找到丰富的 web API 信息从 MDN 中直接显示[Visual Studio Code。](https://code.visualstudio.com/updates/v1_25#_new-css-pseudo-selectors-and-pseudo-elements-from-mdn)
WebVR | 我们已对重大更新[WebVR 开发人员指南](https://docs.microsoft.com/microsoft-edge/webvr/)，包括主页上的经过完全重新设计和重组的目录。 我们已编写了若干新主题，包括： </br> * [什么是 WebVR？](https://docs.microsoft.com/microsoft-edge/webvr/what-is-webvr) 介绍 WebVR 是什么、 为什么你应该使用它，以及如何为其开发快速入门。 </br> * [渐进式 Web 应用中的 WebVR](https://docs.microsoft.com/microsoft-edge/webvr/webvr-in-pwas):了解如何将 WebVR 添加到渐进式 Web App (PWA)。 </br> * [WebView 中的 WebVR](https://docs.microsoft.com/microsoft-edge/webvr/webvr-in-webview):了解如何将 WebVR 添加到 Windows 10 应用程序中的 WebView 控件。 </br> * [WebVR 演示](https://docs.microsoft.com/microsoft-edge/webvr/demos):请查看使用 Microsoft Edge 和 Windows Mixed Reality 沉浸式头戴式一些 WebVR 演示。

## <a name="publish--monetize-windows-apps"></a>发布 Windows 应用并实现盈利

功能 | 描述
 :------ | :------
MSIX | [MSIX](https://docs.microsoft.com/windows/msix/overview)是提供对所有 Windows 应用程序的最新的打包体验的新 Windows 应用包格式。 这种开源的 MSIX 格式既保留了已有程序包的功能，又具有现代部署的特色。
MSIX 打包工具 | 新[MSIX 打包工具](https://docs.microsoft.com/windows/msix/mpt-overview)) 允许您重新打包 MSIX 格式对现有桌面应用程序，即使您没有访问其源代码。 在命令行中，或通过其交互式 UI，可以运行它。
MSIX Desktop App Converter 支持 | 可以使用[Desktop App Converter](https://docs.microsoft.com/windows/uwp/porting/desktop-to-uwp-root)若要使用输出 MSIX 包`-MakeMSIX`参数。
MakeAppx.exe MSIX 的工具支持 | MakeAppx.exe 工具可用于创建 UWP 应用或传统桌面应用程序的 MSIX 包。 此工具包含在 Windows 10 SDK 中，并且可以从命令提示符或脚本文件中使用。 </br> 对于 UWP 应用，请参阅[MakeAppx.exe 工具创建应用程序包](https://docs.microsoft.com/windows/uwp/packaging/create-app-package-with-makeappx-tool)。 </br> 对于桌面应用程序，请参阅[手动打包桌面应用程序](https://docs.microsoft.com/windows/uwp/porting/desktop-to-uwp-manual-conversion)。
包支持框架 | [包支持框架](https://docs.microsoft.com/windows/msix/package-support-framework-overview)是一个开放源代码工具包，可帮助你应用修补程序到您现有的桌面应用程序时不能访问源代码，以便它可以在 MSIX 容器中运行。
存储分析 API | [Microsoft Store 分析 API](../monetize/access-analytics-data-using-windows-store-services.md)现在包含以下新方法： </br> * [为 UWP 应用中获取见解数据](../monetize/get-insights-data-for-your-app.md) </br> * [获取桌面应用程序的 insights 数据](../monetize/get-insights-data-for-your-desktop-app.md) </br>* [获取升级块的桌面应用程序](../monetize/get-desktop-block-data.md) </br> * [获取桌面应用程序的升级块详细信息](../monetize/get-desktop-block-data-details.md)

## <a name="videos"></a>视频

自 Fall Creator Update 以来已经发布了以下视频，其中突出展示了 Windows 10 中面向开发人员的新的和改进的功能。

### <a name="cwinrt"></a>C++/WinRT

C + + / WinRT 是创作和使用 Windows 运行时 Api 的新方法。 它仅在头文件中实现并能满足您的现代应用程序功能的优先访问权限。 [观看视频](https://www.youtube.com/watch?v=TLSul1XxppA&feature=youtu.be)若要了解其工作原理，然后[阅读开发人员文档](../cpp-and-winrt-apis/index.md)的详细信息。

### <a name="get-started-for-devs-create-and-customize-a-form-on-windows-10"></a>开始进行开发人员：创建和自定义 Windows 10 上的窗体

我们[开始 docs](../get-started/index.md)为 Windows 开发人员现在向亲身体验提供基本的应用开发任务。 此视频将引导你完成这些主题之一，并介绍了在您的应用程序中创建窗体用户界面的基础知识。 [观看视频](https://www.youtube.com/watch?v=AgngKzq4hKI&feature=youtu.be)若要查看在操作中，然后代码[自己查看主题。](https://aka.ms/CreateForms)

### <a name="enhance-your-bot-with-project-personality-chat"></a>增强项目的个性聊天的机器人

项目的个性聊天，可以将可自定义角色添加到聊天机器人。 通过与 Microsoft Bot Framework SDK 集成，可以添加以更对话的方式与客户进行交互的小型对话功能。 [观看视频](https://www.youtube.com/watch?v=5C_uD8g2QKg&feature=youtu.be)若要了解如何实现它，然后[试用互动演示](https://aka.ms/PersonalityChat)动手体验。

### <a name="multi-instance-uwp-apps"></a>多实例 UWP 应用

Windows 现在可以使用其自己的单独进程中的每个运行的 UWP 应用，多个实例。 [观看视频](https://www.youtube.com/watch?v=clnnf4cigd0&feature=youtu.be)若要了解如何创建新的应用程序支持此功能，然后[阅读开发人员文档](../launch-resume/multi-instance-uwp.md)更多指导如何和为何要使用此功能。

### <a name="xbox-live-unity-plugin"></a>Xbox Live Unity 插件

适用于 Unity Xbox Live 插件包含支持将 Xbox Live 签名、 统计信息、 好友列表、 云存储和排行榜添加到你的标题。 [观看视频](https://youtu.be/fVQZ-YgwNpY)若要了解详细信息，然后[下载 GitHub 包](https://aka.ms/UnityPlugin)若要开始。

### <a name="one-dev-question"></a>一个适用于开发人员问题

在开发人员的一个问题视频系列中，资深 Microsoft 开发者介绍一系列的有关 Windows 开发、 团队区域性和历史记录的问题。

* [Raymond Chen 在 Windows 开发和历史记录](https://www.youtube.com/playlist?list=PLWs4_NfqMtoxjy3LrIdf2oamq1coolpZ7)

* [Larry Osterman 在 Windows 开发和历史记录](https://www.youtube.com/playlist?list=PLWs4_NfqMtoyPUkYGpJU0RzvY6PBSEA4K)

* [Aaron Gustafson 渐进式 Web 应用上](https://www.youtube.com/playlist?list=PLWs4_NfqMtoyPHoI-CIB71mEq-om6m35I)

* [Chris Heilmann webhint 工具](https://www.youtube.com/playlist?list=PLWs4_NfqMtow00LM-vgyECAlMDxx84Q2v)

## <a name="samples"></a>示例

### <a name="customer-orders-database"></a>客户订单数据库

[客户订单数据库示例](https://github.com/Microsoft/Windows-appsample-customers-orders-database)已更新为使用新控件，例如[DataGrid](https://docs.microsoft.com/windows/communitytoolkit/controls/datagrid)， [NavigationView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview)，以及[扩展器](https://docs.microsoft.com/windows/communitytoolkit/controls/expander)。

### <a name="customer-database-tutorial"></a>客户数据库教程

[客户数据库教程](../enterprise/customer-database-tutorial.md)创建基本的 UWP 应用以管理列表的客户，并介绍一些概念和实践在企业开发非常有用。 它将指导你完成实现 UI 元素并将添加对本地 SQLite 数据库中，执行的操作，并提供用于连接到远程的其余部分数据库，如果你想要进一步松散指导。

### <a name="photo-editor-cwinrt"></a>照片编辑器 C + + WinRT

[相片编辑器示例应用](https://github.com/Microsoft/Windows-appsample-photo-editor)演示了如何使用开发[C + + WinRT](../cpp-and-winrt-apis/intro-to-using-cpp-with-winrt.md)语言投影。 应用程序允许你检索从照片**图片**库，然后编辑所选图像具有关联的照片效果。

### <a name="windows-machine-learning"></a>Windows 机器学习

[Windows 机器学习](https://github.com/Microsoft/Windows-Machine-Learning)存储库已更新为使用最新的 Windows 10 SDK，并且包含编写的示例C#，c + + 和 JavaScript。

### <a name="xaml-hosting-api"></a>XAML 承载 API

[XAML 承载 API 示例](https://github.com/Microsoft/Windows-appsample-Xaml-Hosting)是 Win32 桌面应用程序，重点介绍了使用托管 API （也称为 XAML 群岛） UWP XAML 的各种的方案。 项目包含 Windows 墨迹、 Media Player 和导航视图控件库样式演示文稿中。 常规的外部控件使用情况，该示例还演示了处理 XAML 和本机 Windows 事件/消息，以及基本的 XAML 数据绑定。
