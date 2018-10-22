---
author: QuinnRadich
title: 面向开发人员的 Windows 10 中的新增工具和功能
description: Windows 10 版本 17763 和新开发人员工具提供工具、 功能和通用 Windows 平台支持的体验。
keywords: 新增功能，新功能，更新，更新，功能，新，Windows 10，最新，开发人员，17763
ms.author: quradic
ms.date: 10/03/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
ms.localizationpriority: medium
ms.openlocfilehash: 19f2d29d94759a4b8fd273c8fdc0cdf5c93311de
ms.sourcegitcommit: c4d3115348c8b54fcc92aae8e18fdabc3deb301d
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/22/2018
ms.locfileid: "5396332"
---
# <a name="whats-new-in-windows-10-for-developers-build-17763"></a>对于开发人员，生成 17763，什么是 Windows 10 中的新增功能

Windows 10 版本 17763 (也称为年 10 月 2018年更新或版本 1809年)，在与 Visual Studio 2017 和更新的 SDK 结合使用，提供工具、 功能和体验用于打造出色的通用 Windows 平台应用。 只需在 Windows10 上[安装工具和 SDK](http://go.microsoft.com/fwlink/?LinkId=821431)，你便可以随时[创建新的通用 Windows 应用](../get-started/create-uwp-apps.md)，或了解如何使用 [Windows 上的现有应用代码](../porting/index.md)。

该版本集合了 Windows 开发人员感兴趣的新增和改进功能及指南。 有关添加到 Windows SDK 的新命名空间的完整列表，请参阅[Windows 10 版本 17763 API 更改](windows-10-build-17763-api-diff.md)。 有关 Windows 10 突出功能的详细信息，请参阅 [Windows 10 中的酷炫功能](http://go.microsoft.com/fwlink/?LinkId=823181)。 另请参阅 [Windows 开发人员平台功能](https://developer.microsoft.com/windows/platform/features)，了解有关 Windows 平台过去已添加的及将来要添加的功能的详尽概述。

## <a name="design--ui"></a>设计和 UI

功能 | 说明
 :------ | :------
应用图标和徽标 | [应用图标和徽标页面](../design/style/app-icons-and-logos.md)已重新编写，现在显示的最新的 Visual Studio 图标工具，并将图像添加到你的应用一览的 Microsoft 应用商店中提供的信息。
设计登录页面 | [更新登录页的设计](https://developer.microsoft.com/windows/apps/design)具有一览概述 UWP 设计区域和 Fluent 设计添加的最新功能的信息。
Fluent 设计控件 | 以下新 UI 控件已添加，以增强 Fluent 设计系统和你的应用的 apparence: </br> * [CommandBarFlyout](../design/controls-and-patterns/command-bar-flyout.md)可使你的 UI 画布上的某个项目上下文中显示用户的常见任务。 </br> * [DropDownButton](../design/controls-and-patterns/buttons.md#create-a-drop-down-button)、[拆分按钮](../design/controls-and-patterns/buttons.md#create-a-split-button)和[ToggleSplitButton](../design/controls-and-patterns/buttons.md#create-a-toggle-split-button)提供专用的功能来增强你的应用的用户界面与按钮控件。 </br> * [菜单栏](../design/controls-and-patterns/menus.md)显示水平行中的多个顶级菜单的一组。 </br> * [NavigationView](../design/controls-and-patterns/navigationview.md)现在支持有关你的应用具有大量较小的导航选项，并且需要更多空间内容的情况下的顶部导航。 </br> * [树视图](../design/controls-and-patterns/tree-view.md)已增强，以支持数据绑定，项模板中，并将拖放。
Fluent Design 更新 | 已对以下 Fluent Design 页面视觉更新和次要更改： </br> * [对齐方式，填充、 边距](../design/layout/alignment-margin-padding.md) </br> * [颜色](../design/style/color.md) </br> * [适用于 Windows 应用的 fluent 设计](../design/fluent-design-system/index.md) </br> * [应用设计简介](../design/basics/design-and-ui-intro.md) </br> * [导航基础知识](../design/basics/navigation-basics.md) </br> * [响应式设计技术](../design/layout/responsive-design.md) </br> * [屏幕大小和断点](../design/layout/screen-sizes-and-breakpoints-for-responsive-design.md) </br> * [样式概述](../design/style/index.md) </br> * [写作风格](../design/style/writing-style.md) </br> 此外，我们已经重写与他们的内容区域上的所有新信息的以下页面： </br> * [图标](../design/style/icons.md)现在提供实用建议使用图标并使用户成为变成可点击状态。 </br> * [版式](../design/style/typography.md)合并从类似文章的信息将所有内容放在一个位置的更新的指南和插图。
凝视输入和交互 | [凝视交互](../design/input/gaze-interactions.md)允许你的应用要跟踪用户的凝视、 注意和状态，具体取决于的位置及移动的眼睛。 此功能可作为辅助技术，并为游戏和传统输入的设备不可用其他交互式场景提供的机会。
手写视图 | [HandwritingView](../design/controls-and-patterns/text-handwriting-view.md)是 TextBox 和 RichEditBox 的新墨迹输入的表面。 用户可以使用自己笔以展开控件到书写图面点击文本控件。 本指南介绍了如何管理和自定义你的应用程序中 HandwritingView。
在 Fluent 设计的运动 | 正在发展的运动 Fluent 设计系统使用，基于计时、 缓动、 方向性和引力的基础知识。 应用这些基础功能将帮助指导你的应用，用户和其数字体验用连接由反映自然世界。 有关详细信息，这些文章： </br> * [运动概述](../design/motion/index.md)已更新，以反映这些基础知识。 </br> * [运动的实践](../design/motion/motion-in-practice.md)提供如何应用这些基础应用中的示例。 它还包含有关隐式动画，以便轻松旧的和新值的 XAML 元素的属性发生更改时之间的内插。 </br> * [方向性和引力](../design/motion/directionality-and-gravity.md)固化你的应用的用户的心理模型。 </br> * [计时和缓动](../design/motion/timing-and-easing.md)向你的应用中运动逼真效果。 </br> * [XAML 属性动画](../design/motion/xaml-property-animations.md)允许你直接属性进行动画处理的 XAML 元素，而无需与基础合成视觉对象进行交互。
页面过渡 | [页面过渡](../design/motion/page-transitions.md)将用户在应用中的页面之间导航。 这些设置有助于用户了解他们处于导航层次结构，并提供有关页面之间的关系的反馈。
文本缩放 | 新的[文本缩放指南](../design/input/text-scaling.md)介绍了如何更新你的应用程序，以适应新的文本缩放行为，从而提供的能力，用户在操作系统和个别应用程序更改相对字体大小。 而不是使用放大镜应用 （这通常只需放大屏幕区域内的所有内容，并引入了自己的可用性问题）、 更改显示分辨率或依赖于 DPI 缩放 （用以调整大小具体取决于显示和典型观看的所有内容距离），用户可以快速地访问用于调整大小只是文本，范围从 100%（默认大小） 的设置 225%。
工具包 | [Adobe XD 和 Adobe Illustrator 工具包](../design/downloads/index.md)已更新新功能。 这些设计工具包提供用于设计 UWP 应用的控件和布局模板。
UI 命令 | 更新到[UWP 命令基础结构](../design/basics/commanding-basics.md)包括更好的封装命令对象 （行为、 标签、 图标、 键盘加速键，访问键和说明） 和一组标准的常用命令包括剪切、 复制、 粘贴、 退出等。，无需手动设置这些属性。 </br> 新的[XamlUICommand](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.input.xamluicommand)类为 deveining 执行操作时调用的交互式 UI 元素的命令行为提供基类。 这是[StandardUICommand](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.input.standarduicommand)，它公开了一组预定义的属性与标准平台命令的父类。 
Windows UI 库 | [Windows UI 库](https://aka.ms/winui-docs)是一组提供适用于 UWP 应用的控件和其他用户界面元素的 NuGet 程序包。 这些程序包也是与早期版本的 Windows 10 兼容，因此即使你的用户没有设置的最新的操作系统版本的工作原理你的应用。 </br> 什么是 Windows UI 库中的详细信息，请参阅[此列表的 NuGet 程序包中包含的 API 命名空间。](https://docs.microsoft.com/uwp/api/overview/winui/)

## <a name="develop-windows-apps"></a>开发 Windows 应用

功能 | 说明
 :------ | :------
条形码扫描仪 | 已重新整理，并改进了更多详细信息和代码段的[条形码扫描仪](https://docs.microsoft.com/windows/uwp/devices-sensors/pos-barcodescanner)文档。 我们还添加了新的主题中，[获取并了解条形码数据](https://docs.microsoft.com/windows/uwp/devices-sensors/pos-barcodescanner-scan-data)，这就解释了如何获取和使用条形码扫描仪中的数据。
C++/WinRT | [C + + WinRT](https://aka.ms/cppwinrt)包含许多新功能、 更改和修复了此版本。 有新的函数和基本类，你在实现你自己的[集合属性和集合类型](/windows/uwp/cpp-and-winrt-apis/collections); 支持你现在可以使用 C + 使用[{Binding}](/windows/uwp/xaml-platform/binding-markup-extension) XAML 标记扩展 + WinRT 运行时类 （有关代码示例，请参阅[数据绑定概述](/windows/uwp/data-binding/data-binding-quickstart)）。 有关新的和更改在此版本中的所有内容的完整说明，请参阅[新增功能在 C + + WinRT](../cpp-and-winrt-apis/news.md)。</br></br>其他新 C + + WinRT 内容包括：[自定义 XAML 控件](/windows/uwp/cpp-and-winrt-apis/xaml-cust-ctrl);[创作 COM 组件](/windows/uwp/cpp-and-winrt-apis/author-coclasses);[值类别](/windows/uwp/cpp-and-winrt-apis/cpp-value-categories);和[强和弱引用](../cpp-and-winrt-apis/weak-references.md)。
C + + WinRT 代码示例 | 我们添加了 250 C + + WinRT 代码一览主题中我们的文档，附带的现有 C + + CX 代码示例。
投稿指南 | 我们为我们的 UWP 文档更新了[我们提供的指南](https://github.com/MicrosoftDocs/windows-uwp/blob/docs/CONTRIBUTING.md)。 此新指南阐明的工作流和我们的文档的外部贡献的期望。
DirectX 图形 Infastructure (DXGI) | 已添加了新文档对缺少 DXGI Api，并且我们提供了有关最佳做法的文章 Windows 10 上的显示。 </br> * [为了获得最佳性能，使用 DXGI 翻转模型](https://docs.microsoft.com/windows/desktop/direct3ddxgi/for-best-performance--use-dxgi-flip-model)： 提供如何最大限度提高性能和现代版本的 Windows 上演示文稿堆栈中的效率的指南。 </br> * [IDXGIOutput6::CheckHardwareCompositionSupport 方法](https://docs.microsoft.com/windows/desktop/api/dxgi1_6/nf-dxgi1_6-idxgioutput6-checkhardwarecompositionsupport)： 通知应用程序，受支持硬件拉伸。 </br> * [DXGI_HARDWARE_COMPOSITION_SUPPORT_FLAGS 枚举](https://docs.microsoft.com/windows/desktop/api/dxgi1_6/ne-dxgi1_6-dxgi_hardware_composition_support_flags)： 描述支持硬件合成的哪些级别。
入门 | 大大我们[要开始使用](../get-started/index.md)的内容的增加了已使用新的主题，提供有关如何开发人员新到 Windows 10 可能完成下列常见任务的信息和指南： </br> * [构建表单](../get-started/construct-form-learning-track.md) </br> * [以列表形式显示客户](../get-started/display-customers-in-list-learning-track.md) </br> * [保存和加载设置](../get-started/settings-learning-track.md) </br> * [处理文件](../get-started/fileio-learning-track.md)
地图样式表编辑器 | 使用新[地图样式表编辑器](https://www.microsoft.com/p/map-style-sheet-editor/9nbhtcjt72ft?rtc=1#activetab=pivot:overviewtab)应用程序以交互方式自定义你将添加到你的应用程序的地图的外观。
了解 Microsoft | 新的[Microsoft 了解站点](https://www.microsoft.com/learning/default.aspx)向 Microsoft 开发人员提供新动手学习和培训机会。 目前，Microsoft 了解提供培训和认证的 Microsoft 365、 Microsoft Azure、 Office 365 和 Windows Server。
记事本 | [已更新，记事本](http://aka.ms/ant-man)，添加缩放、 环绕查找/替换，以及对 Unix/Linux （换行符） 和 Mac （回车） 行尾支持。
Project Rome | 现在，[项目 rome](https://docs.microsoft.com/windows/project-rome/)跨受支持的平台和 Sdk 提供一致的编程体验。 </br>  新[Microsoft Graph 通知](https://developer.microsoft.com/graph/docs/concepts/notifications-concept-overview)使用项目 rome 产品/服务以用户为中心的、 跨平台通知平台为你的应用。
屏幕截图 | 新的[URI 方案](../launch-resume/launch-screen-snipping.md)允许你的应用以编程方式打开新的代码段，或启动具有特定注释的图像的代码段和草图应用。
在桌面应用程序的 UWP 控件 | Windows 10 现在可以在 WPF、 Windows 窗体和 c + + Win32 桌面应用程序中使用 UWP 控件。 这意味着你可以增强的外观、 体验和功能的现有桌面应用程序将仅可通过 UWP 控件，如 Windows Ink 和支持 Fluent 设计系统的控件的最新 Windows 10 UI 功能。 此功能称为*XAML 群岛*。 </br> 我们提供几种方法使用你的应用程序，具体取决于你所使用的应用程序平台中的 XAML 群岛。 WPF 和 Windows 窗体应用程序可以使用一组[Windows 社区工具包](https://docs.microsoft.com/windows/uwpcommunitytoolkit/)提供面向设计器的开发体验的控件。 C + + Win32 应用程序必须[Windows.UI.Xaml.Hosting](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting)命名空间中使用*UWP XAML 托管 API* 。 有关详细信息，请参阅[在桌面应用程序的 UWP 控件](../xaml-platform/xaml-host-controls.md)。 </br> **注意：** 作为开发人员预览版当前可用的 Api 和控件应能使 XAML 群岛。 尽管我们鼓励你试用它们在原型代码中现在，我们不建议你使用它们在生产代码中这一次。
Windows 机器学习 | [Windows 机器学习](https://docs.microsoft.com/windows/ai/)具有现在正式启动后，为尖端机器学习模型提供更快地评估和支持等功能。 若要支持想要将其集成到其应用程序开发人员，我们创建了一个新文档站点具有多个新的和更新的资源： </br> * [教程： 创建 Windows 机器学习桌面应用程序 （c + +）](https://docs.microsoft.com/windows/ai/get-started-desktop)： 本教程介绍如何建立适用于桌面简单的 Windows ML 应用程序。 </br> * [教程： 创建 Windows 机器学习 UWP 应用程序 (C#)](https://docs.microsoft.com/windows/ai/get-started-uwp)： 使用 Windows ML 在此分步教程创建你的第一个 UWP 应用程序。 </br> * [Windows.AI.MachineLearning Namespace](https://docs.microsoft.com/uwp/api/windows.ai.machinelearning)： 已更新为最新版本的 Windows 10 SDK API 参考和开发人员现在可以使用此 API 的 Win32 和 UWP 应用程序。
Windows Mixed Reality | 开发人员现在可以将受硬件保护的请求后缓冲纹理如果受显示硬件，从而允许应用程序从等 PlayReady 源使用受硬件保护的内容。 硬件保护支持和设置是适用于通过使用新属性[Windows.Graphics.Holographic.HolographicCamera](https://docs.microsoft.com/uwp/api/windows.graphics.holographic.holographiccamera)，主图层，用于通过[四层Windows.Graphics.Holographic.HolographicQuadLayerUpdateParameters](https://docs.microsoft.com/uwp/api/windows.graphics.holographic.holographicquadlayerupdateparameters)。

## <a name="iot-core"></a>IoT Core

功能 |描述:--|:---AssignedAccessSettings |[AssignedAccessSettings 类](https://docs.microsoft.com/uwp/api/windows.system.userprofile.assignedaccesssettings)支持不同方法的调用和属性来访问用户的分配的访问为特定设备的设置。
默认应用概述 |[Windows 10 IoT 核心版默认应用](https://docs.microsoft.com/windows/iot-core/develop-your-app/iotcoredefaultapp)已使用的新特性和功能，如墨迹书写的天气更新和音频。
仪表板 |[Windows 10 Iot 核心版仪表板](https://docs.microsoft.com/windows/iot-core/tutorials/quickstarter/devicesetup)现在允许开发人员使用 Dragonboard 410 C 或 NXP flash 到其设备上的自定义 FFUs。
屏幕键盘 |[屏幕键盘的 IoT 设备](https://docs.microsoft.com/windows/iot-core/develop-your-app/onscreenkeyboard)现在使用作为 Windows 桌面版相同的触摸键盘组件。 这使功能，例如听写模式、 输入法支持和一组完整的输入范围。
标题栏中登录对话框 |Windows 10 IoT 核心版现在提供用于配置[为系统对话框标题栏](https://docs.microsoft.com/windows/iot-core/develop-your-app/signindialogtitlebars)的选项。
唤醒触摸 |[唤醒触摸](https://docs.microsoft.com/windows/iot-core/learn-about-hardware/wakeontouch)使你的设备屏幕关闭时不在使用，同时快速打开，当用户触摸屏幕时。 Windows.System.Update |新[Windows.System.Update 命名空间](https://docs.microsoft.com/uwp/api/windows.system.update)支持系统更新的交互式的控件。 此命名空间仅适用于 Windows 10 IoT 核心版。

## <a name="web-development"></a>Web 开发

功能 |描述:--|:---EdgeHTML 18 |Windows 10 年 10 月使用[EdgeHTML 18](https://docs.microsoft.com/microsoft-edge/dev-guide)，最新的更新到 Microsoft Edge 浏览器和适用于 UWP 应用的 JavaScript 引擎更新海上。 EdgeHTML 18 带来现代化和扩展 Web 身份验证 API、 新 WebView 控件功能和详细信息的支持 ！ 一侧工具，EdgeHTML 18 为带来了新 WebDriver 功能和自动更新以及增强的 Edge DevTools 和 Edge DevTools 协议。 有关所有详细信息，请查看[EdgeHTML 18 中的新增](https://docs.microsoft.com/microsoft-edge/dev-guide)和[DevTools 中最新的 Windows 10 更新 (EdgeHTML 18)](https://docs.microsoft.com/microsoft-edge/devtools-guide/whats-new) 。
渐进式 Web 应用 |Windows 10 JavaScript 应用 （ *WWAHost.exe*进程中运行的 web 应用） 现在支持启动才能激活任何视图是一个可选[每个应用程序背景脚本](https://docs.microsoft.com/en-us/microsoft-edge/dev-guide#progressive-web-apps)和过程的持续时间内运行。 与此，你可以监视和修改导航、 跨导航跟踪状态、 监视导航错误，和视图激活前运行代码。 如果指定为[`StartPage`](https://docs.microsoft.com/en-us/uwp/schemas/appxpackage/appxmanifestschema2010-v2/element-application)在[应用清单](https://docs.microsoft.com/en-us/uwp/schemas/appxpackage/appx-package-manifest)你，每个应用的视图 (windows) 公开给该脚本作为的新实例[`WebUIView`](https://docs.microsoft.com/en-us/uwp/api/windows.ui.webui.webuiview)类，作为一般 (Win32) [WebView](https://docs.microsoft.com/en-us/uwp/api/windows.web.ui.iwebviewcontrol)中提供相同的事件、 属性和方法。
Web API 扩展 |已添加到在 Mozilla Developer Network 文档中针对跨浏览器 web 开发的[旧 Microsoft API 扩展](https://developer.mozilla.org/docs/Web/API/Microsoft_API_extensions)列表。 这些 API 扩展是唯一的 Internet Explorer 或 Microsoft Edge，并补充有关 MDN web 文档中的兼容性和浏览器支持的现有信息。传统的 Microsoft[扩展 CSS](https://developer.mozilla.org/docs/Web/CSS/Microsoft_Extensions)和[JavaScript 扩展](https://developer.mozilla.org/docs/Web/JavaScript/Microsoft_JavaScript_extensions)也是可用，并且你可以找到丰富的 web API 信息从 MDN 直接在呈现[Visual Studio Code。](https://code.visualstudio.com/updates/v1_25#_new-css-pseudo-selectors-and-pseudo-elements-from-mdn)
WebVR |我们已主要更新中[WebVR 开发人员指南](https://docs.microsoft.com/microsoft-edge/webvr/)，包括主页页面的完全重新设计和目录的重组。 我们还编写多个新的主题，其中包括： </br> * [什么是 WebVR？](https://docs.microsoft.com/microsoft-edge/webvr/what-is-webvr) 说明了什么 WebVR，为什么应使用它，以及如何为其开发入门。 </br> * [渐进式 Web 应用中的 WebVR](https://docs.microsoft.com/microsoft-edge/webvr/webvr-in-pwas)： 了解如何将 WebVR 添加到渐进式 Web 应用 (PWA)。 </br> * [WebVR 在 web 视图中](https://docs.microsoft.com/microsoft-edge/webvr/webvr-in-webview)： 了解如何将 WebVR 添加到 Windows 10 应用中的 web 视图控件。 </br> * [WebVR 演示](https://docs.microsoft.com/microsoft-edge/webvr/demos)： 查看一些 WebVR 演示使用 Microsoft Edge 和 Windows Mixed Reality 沉浸式头戴显示设备。

## <a name="publish--monetize-windows-apps"></a>发布 Windows 应用并实现盈利

功能 | 说明
 :------ | :------
MSIX | [MSIX](https://docs.microsoft.com/windows/msix/overview)是提供对所有 Windows 应用的现代打包体验的新 Windows 应用包格式。 开源 MSIX 格式保留现有包的功能并启用现代的部署功能。
MSIX 打包工具 | 新[MSIX 打包工具](https://docs.microsoft.com/windows/msix/mpt-overview)） 允许你重新打包你现有的桌面应用程序以 MSIX 格式，即使你不有权访问他们的源代码。 它可以运行命令行中，或通过其交互式 UI。
MSIX 的桌面应用转换器支持 | 你可以使用[Desktop App Converter](https://docs.microsoft.com/windows/uwp/porting/desktop-to-uwp-root)输出 MSIX 一个包中，通过使用`-MakeMSIX`参数。
MSIX MakeAppx.exe 工具支持 | 你可以使用 MakeAppx.exe 工具创建的 UWP 应用或传统桌面应用程序 MSIX 包。 此工具包含在 Windows 10 SDK 中，并且可以从命令提示符或脚本文件中使用。 </br> 对于 UWP 应用，请参阅[创建应用包使用 MakeAppx.exe 工具](https://docs.microsoft.com/windows/uwp/packaging/create-app-package-with-makeappx-tool)。 </br> 对于桌面应用程序，请参阅[手动打包的桌面应用程序](https://docs.microsoft.com/windows/uwp/porting/desktop-to-uwp-manual-conversion)。
包支持框架 | [包支持框架](https://docs.microsoft.com/windows/msix/package-support-framework-overview)是可帮助你修复时应用到你现有的桌面应用程序不能访问的源代码，以便它可以 MSIX 容器中运行的开源工具包。
应用商店分析 API | [Microsoft Store 分析 API](../monetize/access-analytics-data-using-windows-store-services.md)现在包含以下新的方法： </br> * [获取你的 UWP 应用的见解数据](../monetize/get-insights-data-for-your-app.md) </br> * [获取桌面应用程序的见解数据](../monetize/get-insights-data-for-your-desktop-app.md) </br>* [获取桌面应用程序的升级块](../monetize/get-desktop-block-data.md) </br> * [获取桌面应用程序的升级块详情](../monetize/get-desktop-block-data-details.md)

## <a name="videos"></a>视频

自 Fall Creator Update 以来已经发布了以下视频，其中突出展示了 Windows 10 中面向开发人员的新的和改进的功能。

### <a name="cwinrt"></a>C++/WinRT

C + + /winrt 是一种新创作和使用 Windows 运行时 Api。 它有在标头文件中，实现唯一，旨在为你提供对现代应用功能的一流访问。 [观看视频](https://www.youtube.com/watch?v=TLSul1XxppA&feature=youtu.be)以了解它的工作原理，然后[阅读开发人员文档](../cpp-and-winrt-apis/index.md)的详细信息。

### <a name="get-started-for-devs-create-and-customize-a-form-on-windows-10"></a>要开始使用适用于开发人员： 创建和自定义 Windows 10 上的表单

我们的[入门文档](../get-started/index.md)面向 Windows 开发人员现在提供基本应用开发任务动手的体验。 本视频将指导你通过这些主题中，之一和介绍在应用中创建窗体 UI 的基础知识。 [观看视频](https://www.youtube.com/watch?v=AgngKzq4hKI&feature=youtu.be)以查看的代码中操作，然后[自行查看本主题。](http://aka.ms/CreateForms)

### <a name="enhance-your-bot-with-project-personality-chat"></a>增强你的项目个性聊天机器人

项目个性聊天允许你将自定义角色添加到你聊天机器人。 通过与 Microsoft 机器人框架 SDK 集成，你可以添加更谈话地与客户交互的小访谈功能。 [观看视频](https://www.youtube.com/watch?v=5C_uD8g2QKg&feature=youtu.be)以了解如何实现它，然后[尝试操作交互式演示](http://aka.ms/PersonalityChat)动手体验。

### <a name="multi-instance-uwp-apps"></a>多实例 UWP 应用

Windows 现在允许你与每个单独进程中运行的 UWP 应用，多个实例。 [观看视频](https://www.youtube.com/watch?v=clnnf4cigd0&feature=youtu.be)以了解如何创建新的应用支持此功能，请[阅读开发人员文档](../launch-resume/multi-instance-uwp.md)为如何更多指南以及为什么要使用此功能。

### <a name="xbox-live-unity-plugin"></a>Xbox Live Unity 插件

Xbox Live 的 Unity 插件包含向你的游戏添加 Xbox Live 签名、 统计数据、 好友列表、 云存储和排行榜的支持。 [观看视频](https://youtu.be/fVQZ-YgwNpY)以了解更多信息，然后[下载 GitHub 程序包](https://aka.ms/UnityPlugin)若要开始使用。

### <a name="one-dev-question"></a>一个开发人员的问题

在开发人员的一个问题视频系列中，longtime Microsoft 开发人员介绍一系列有关 Windows 开发、 团队区域性和历史记录的问题。

* [Raymond Chen 在 Windows 开发和历史记录](https://www.youtube.com/playlist?list=PLWs4_NfqMtoxjy3LrIdf2oamq1coolpZ7)

* [在 Windows 开发和历史记录 Larry Osterman](https://www.youtube.com/playlist?list=PLWs4_NfqMtoyPUkYGpJU0RzvY6PBSEA4K)

* [兆辉 Gustafson 上渐进式 Web 应用](https://www.youtube.com/playlist?list=PLWs4_NfqMtoyPHoI-CIB71mEq-om6m35I)

* [Chris Heilmann 取决于 webhint 工具](https://www.youtube.com/playlist?list=PLWs4_NfqMtow00LM-vgyECAlMDxx84Q2v)

## <a name="samples"></a>示例

### <a name="customer-orders-database"></a>客户订单数据库

[客户订单数据库示例](https://github.com/Microsoft/Windows-appsample-customers-orders-database)已更新使用新的控件，如[网格](https://docs.microsoft.com/windows/communitytoolkit/controls/datagrid)、 [NavigationView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview)和[扩展](https://docs.microsoft.com/windows/communitytoolkit/controls/expander)。

### <a name="customer-database-tutorial"></a>客户数据库教程

[客户数据库教程](../enterprise/customer-database-tutorial.md)创建基本的 UWP 应用用于管理客户列表，并介绍了概念和企业开发中非常有用的做法。 它将指导你通过实现 UI 元素并添加针对本地 SQLite 数据库中，操作，并提供用于连接到远程 REST 数据库，如果你想要继续进行操作的松散指南。

### <a name="photo-editor-cwinrt"></a>照片编辑器 C + + WinRT

[照片编辑器示例应用](https://github.com/Microsoft/Windows-appsample-photo-editor)展示了使用开发[C + + WinRT](../cpp-and-winrt-apis/intro-to-using-cpp-with-winrt.md)语言投影。 该应用使你从**图片**库检索照片，然后编辑使用关联的照片效果的选择的图像。

### <a name="windows-machine-learning"></a>Windows 机器学习

[Windows 机器学习](https://github.com/Microsoft/Windows-Machine-Learning)存储库已更新为使用最新的 Windows 10 SDK，并包含在 C#、 c + + 和 JavaScript 编写的示例。

### <a name="xaml-hosting-api"></a>XAML 托管 API

[XAML 托管 API 示例](https://github.com/Microsoft/Windows-appsample-Xaml-Hosting)是使用 UWP XAML 托管 API （也称为 XAML 群岛） 的分类的方案将突出显示的 Win32 桌面应用。 项目都包含库样式演示文稿中的 Windows Ink、 媒体播放器和导航视图控件。 常规外控制用法，该示例还演示了处理 XAML 和本机 Windows 事件/消息，以及基本 XAML 数据绑定。