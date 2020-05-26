---
title: Windows 10 内部版本 17763 中的新增功能
description: Windows 10 内部版本 17763 和新的开发人员工具提供由通用 Windows 平台支持的工具、功能和体验。
keywords: Windows 10, 17763, 1809
ms.date: 10/03/2018
ms.topic: article
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 77b47866bc064baec7c0ecf556eb615f67af0554
ms.sourcegitcommit: 87fd0ec1e706a460832b67f936a3014f0877a88c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/12/2020
ms.locfileid: "83234379"
---
# <a name="whats-new-in-windows-10-for-developers-build-17763"></a>面向开发人员的 Windows 10 内部版本 17763 中的新增功能

Windows 10 内部版本 17763（又称 2018 年 10 月更新或版本 1809）与 Visual Studio 2019 和更新的 SDK 相结合，为打造出色的通用 Windows 平台应用提供了相关工具、功能和体验。 只需在 Windows 10 上[安装工具和 SDK](https://developer.microsoft.com/windows/downloads#_blank)，你便可以随时[创建新的通用 Windows 应用](../get-started/create-uwp-apps.md)，或了解如何使用 [Windows 上的现有应用代码](../porting/index.md)。

该版本集合了 Windows 开发人员感兴趣的新增和改进功能及指南。 有关已添加到 Windows SDK 的新命名空间的完整列表，请参阅 [Windows 10 内部版本 17763 API 更改](windows-10-build-17763-api-diff.md)。 有关 Windows 10 中亮点功能的详细信息，请参阅 [Windows 10 中的酷炫功能](https://developer.microsoft.com/windows/windows-10-for-developers)。 另请参阅 [Windows 开发人员平台功能](https://developer.microsoft.com/windows/platform/features)，了解有关 Windows 平台过去已添加的及将来要添加的功能的详尽概述。

## <a name="design--ui"></a>设计和 UI

功能 | 说明
 :------ | :------
应用图标和徽标 | [应用图标和徽标页面](../design/style/app-icons-and-logos.md)已经过重新编写，现在显示最新的 Visual Studio 图标工具，并提供有关向 Microsoft Store 中的应用列表添加图像的信息。
设计登陆页面 | [已更新的设计登陆页面](https://developer.microsoft.com/windows/apps/design)提供对 UWP 设计区域的简要概述以及有关 Fluent Design 最新新增功能的信息。
Fluent Design 控件 | 添加了以下新 UI 控件，增强了 Fluent Design System 并改进了应用外观： </br> * [CommandBarFlyout](../design/controls-and-patterns/command-bar-flyout.md) 用于在 UI 画布项的上下文中显示常见用户任务。 </br> * [DropDownButton](../design/controls-and-patterns/buttons.md#create-a-drop-down-button)、[SplitButton](../design/controls-and-patterns/buttons.md#create-a-split-button) 和 [ToggleSplitButton](../design/controls-and-patterns/buttons.md#create-a-toggle-split-button) 提供具有专门功能的按钮控件，可增强应用的用户界面。 </br> * [MenuBar](../design/controls-and-patterns/menus.md) 可以在水平行中显示一组多个顶级菜单。 </br> * [NavigationView](../design/controls-and-patterns/navigationview.md) 现在支持顶部导航栏，适用于应用的导航选项较少且需要更多内容空间的情况。 </br> * [TreeView](../design/controls-and-patterns/tree-view.md) 已经过增强，可支持数据绑定、项模板和拖放。
Fluent Design 更新 | 对以下 Fluent Design 页面进行了视觉更新和细微更改： </br> * [对齐、填充、边距](../design/layout/alignment-margin-padding.md) </br> * [颜色](../design/style/color.md) </br> * [适用于 Windows 应用的 Fluent Design](../design/fluent-design-system/index.md) </br> * [应用设计简介](../design/basics/design-and-ui-intro.md) </br> * [导航基础知识](../design/basics/navigation-basics.md) </br> * [响应式设计技术](../design/layout/responsive-design.md) </br> * [屏幕大小和断点](../design/layout/screen-sizes-and-breakpoints-for-responsive-design.md) </br> * [样式概述](../design/style/index.md) </br> * [写作风格](../design/style/writing-style.md) </br> 此外，我们重新编写了以下页面，在其内容区域中提供全新信息： </br> * [图标](../design/style/icons.md)现在提供有关使用图标并使其可点击的实用建议。 </br> * [版式](../design/style/typography.md)可合并相似文章中的信息，将所有内容放在一个位置，并提供更新后的指南和说明。
凝视输入和交互 | [凝视交互](../design/input/gaze-interactions.md)可让应用根据用户眼睛的位置和移动来跟踪用户的凝视、注意和状态。 此功能可以用作辅助技术，在传统输入设备不可用的情况下用于游戏和其他交互场景。
手写视图 | [HandwritingView](../design/controls-and-patterns/text-handwriting-view.md) 是 TextBox 和 RichEditBox 的新墨迹输入界面。 用户可以用触笔点击文本控件，将该控件扩展为书写界面。 本指南解释了如何管理和自定义应用程序中的 HandwritingView。
Fluent Design 中的动作 | 在 Fluent Design System 中，动作的使用方式不断改进，其基础原理为计时、缓动、方向性和引力。 应用这些基本原理有助于指导用户使用应用，并通过反映自然世界将用户与其数字体验联系起来。 通过以下文章了解详细信息： </br> * [动作概述](../design/motion/index.md)已更新，反映了这些基本原理。 </br> * [动作练习](../design/motion/motion-in-practice.md)提供了在应用中应用这些基本原理的示例。 它还包含有关隐式动画的信息，当 XAML 元素的属性更改时，隐式动画可用于在旧值与新值之间实现轻松内插。 </br> * [方向性和引力](../design/motion/directionality-and-gravity.md)可巩固用户为应用建立的心理模型。 </br> * [计时和缓动](../design/motion/timing-and-easing.md)增加了应用中动作的真实性。 </br> * [XAML 属性动画](../design/motion/xaml-property-animations.md)可用于直接设置 XAML 元素属性的动画，而无需与基础组合 Visual 进行交互。
页面过渡 | [页面过渡](../design/motion/page-transitions.md)可将用户导航到应用中的各个页面。 它们帮助用户了解自己在导航层次结构中的位置，并提供有关页面之间关系的反馈。
文本缩放 | 新的[文本缩放指南](../design/input/text-scaling.md)介绍了如何更新应用程序以适应新的文本缩放行为，通过该缩放行为，用户可跨操作系统和单个应用程序来更改相对字号。 用户不必使用放大镜应用（通常只放大屏幕某个区域内的全部内容，并带来其自身的可用性问题）、更改显示分辨率或依赖 DPI 缩放（根据显示器和典型观看距离调整所有内容的大小），而是可以快速访问设置，只调整文本大小，调整范围为 100%（默认大小）至最高 225%。
工具包 | [Adobe XD 和 Adobe Illustrator 工具包](../design/downloads/index.md)已更新，添加了新功能。 这些设计工具包提供用于设计 UWP 应用的控件和布局模板。
UI 命令 | [UWP 命令基础结构](../design/basics/commanding-basics.md)更新包括更好地封装命令对象（行为、标签、图标、键盘快捷方式、访问密钥和描述）和一组标准的常用命令（包括剪切、复制、粘贴、退出等），因而无需手动设置这些属性。 </br> 新的 [XamlUICommand](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.xamluicommand) 类提供了一个基类，用于定义在调用时执行操作的交互式 UI 元素的命令行为。 这是 [StandardUICommand](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.standarduicommand) 的父类，StandardUICommand 公开了一组具有预定义属性的标准平台命令。 
Windows UI 库 | [Windows UI 库](https://docs.microsoft.com/uwp/toolkits/winui/)是一组 NuGet 程序包，提供用于 UWP 应用的控件和其他用户界面元素。 这些程序包还与 Windows 10 的早期版本兼容，因此即使用户没有最新的操作系统版本，应用也可以正常工作。 </br> 有关 Windows UI 库中内容的详细信息，请参阅 [NuGet 程序包中包含的 API 命名空间列表](https://docs.microsoft.com/uwp/api/overview/winui/)。

## <a name="develop-windows-apps"></a>开发 Windows 应用

功能 | 说明
 :------ | :------
条形码扫描仪 | [条码扫描仪](https://docs.microsoft.com/windows/uwp/devices-sensors/pos-barcodescanner)文档已经过重新组织，改进了更多细节和代码片段。 我们还添加了一个新主题，即[获取并了解条形码数据](https://docs.microsoft.com/windows/uwp/devices-sensors/pos-barcodescanner-scan-data)，其中解释了如何获取和使用条形码扫描仪中的数据。
C++/WinRT | [C++/ WinRT](https://docs.microsoft.com/windows/uwp/cpp-and-winrt-apis/index) 包含许多新功能、更改和针对此版本的修补程序。 还包含新的函数和基类，用于支持你实现自己的[集合属性和集合类型](/windows/uwp/cpp-and-winrt-apis/collections)；并且现在可以结合使用 [{Binding}](/windows/uwp/xaml-platform/binding-markup-extension) XAML 标记扩展和 C++/ WinRT 运行时类 （有关代码示例，请参阅[数据绑定概述](/windows/uwp/data-binding/data-binding-quickstart)）。 有关此版本中新增内容和更改内容的完整描述，请参阅 [C++/WinRT 中的新增功能](../cpp-and-winrt-apis/news.md)。</br></br>其他新 C++/WinRT 内容包括：[XAML 自定义控件](/windows/uwp/cpp-and-winrt-apis/xaml-cust-ctrl)；[创作 COM 组件](/windows/uwp/cpp-and-winrt-apis/author-coclasses)；[值类别](/windows/uwp/cpp-and-winrt-apis/cpp-value-categories)；以及[强引用和弱引用](../cpp-and-winrt-apis/weak-references.md)。
C++/WinRT 代码示例 | 我们的文档中增加了 250 C++/WinRT 代码列表以及伴随出现的现有 C++/CX 代码示例。
参与指南 | 我们更新了适用于 UWP 文档的[参与指南](https://github.com/MicrosoftDocs/windows-uwp/blob/docs/CONTRIBUTING.md)。 这一新的指南阐明了针对我们的文档进行外部参与的工作流和预期。
DirectX 图形基础结构 (DXGI) | 已添加针对缺失的 DXGI API 的新文档，并提供了在 Windows 10 上进行演示的最佳做法的相关文章。 </br> * [为了获得最佳性能，请使用 DXGI 翻转模式](https://docs.microsoft.com/windows/desktop/direct3ddxgi/for-best-performance--use-dxgi-flip-model)：提供有关如何最大限度提高新版 Windows 上演示堆栈的性能和效率的指南。 </br> * [IDXGIOutput6::CheckHardwareCompositionSupport 方法](https://docs.microsoft.com/windows/desktop/api/dxgi1_6/nf-dxgi1_6-idxgioutput6-checkhardwarecompositionsupport)：通知应用程序支持硬件拉伸。 </br> * [DXGI_HARDWARE_COMPOSITION_SUPPORT_FLAGS 枚举](https://docs.microsoft.com/windows/desktop/api/dxgi1_6/ne-dxgi1_6-dxgi_hardware_composition_support_flags)：描述受支持的硬件组合级别。
入门 | [入门](../get-started/index.md)内容包含全新主题，提供有关 Windows 10 新手开发人员如何完成以下常见任务的信息和指导： </br> * [构建表单](../get-started/construct-form-learning-track.md) </br> * [在列表中显示客户](../get-started/display-customers-in-list-learning-track.md) </br> * [保存并加载设置](../get-started/settings-learning-track.md) </br> * [处理文件](../get-started/fileio-learning-track.md)
地图样式表参考 | 使用新的 [地图样式表编辑器](https://www.microsoft.com/p/map-style-sheet-editor/9nbhtcjt72ft?rtc=1#activetab=pivot:overviewtab)应用程序，以交互方式自定义添加到应用程序的地图的外观。
Microsoft Learn | 新的 [Microsoft Learn 站点](https://www.microsoft.com/learning/default.aspx)为 Microsoft 开发人员提供新的实践学习和培训机会。 目前，Microsoft Learn 为 Microsoft 365、Microsoft Azure、Office 365 和 Windows Server 提供培训和认证。
记事本 | [已更新记事本](https://blogs.windows.com/windowsexperience/2018/07/11/announcing-windows-10-insider-preview-build-17713/)，添加了缩放、环绕内容查找/替换，以及对 Unix/Linux (LF) 和 Mac (CR) 行尾的支持。
Project Rome | [Project Rome](https://docs.microsoft.com/windows/project-rome/) 现在提供跨支持的平台和 SDK 的一致编程体验。 </br>  新的 [Microsoft Graph 通知](https://developer.microsoft.com/graph/docs/concepts/notifications-concept-overview)使用 Project Rome 为应用提供以人为中心的跨平台通知平台。
屏幕截取 | 新的 [URI 方案](../launch-resume/launch-screen-snipping.md)可让应用以编程方式打开一个新的截图，或者使用特定的注释图像来启动“截图和草图”应用。
桌面应用程序中的 UWP 控件 | 现在，Windows 10 支持你在 WPF、Windows 窗体和 C++ Win32 桌面应用程序中使用 UWP 控件。 这意味着你可以使用只能通过 UWP 控件（例如 Windows Ink）和支持 Fluent Design System 的控件提供的最新 Windows 10 UI 功能来增强现有桌面应用程序的外观和功能。 此功能称为“XAML 岛”。 </br> 我们提供了几种在应用程序中使用 XAML 岛的方法，具体取决于正在使用的应用程序平台。 WPF 和 Windows 窗体应用程序可以使用 [Windows 社区工具包](https://docs.microsoft.com/windows/uwpcommunitytoolkit/)中的一组控件，这些控件提供面向设计人员的开发体验。 C++ Win32 应用程序必须使用 [Windows.UI.Xaml.Hosting](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting) 命名空间中的 UWP XAML 托管 API。 有关详细信息，请参阅[桌面应用程序中的 UWP 控件](../xaml-platform/xaml-host-controls.md)。 </br> **注意：** 启用 XAML 岛的 API 和控件当前以开发者预览版的形式提供。 现在，尽管我们鼓励在自己的原型代码中试用，但不建议在生产代码中使用它们。
Windows 机器学习 | [Windows 机器学习](https://docs.microsoft.com/windows/ai/)现已正式推出，提供针对最前沿的机器学习模型进行更快的评估和支持等功能。 我们创建了一个新的文档站点，以便为想要将机器学习集成到应用程序中的开发人员提供支持，该站点中包含一些新资源和更新资源： </br> * [教程：创建 Windows 机器学习桌面应用程序 (C++)](https://docs.microsoft.com/windows/ai/get-started-desktop)：本教程演示如何构建简单的桌面版 Windows 机器学习应用程序。 </br> * [教程：创建 Windows 机器学习 UWP 应用程序 (C#)](https://docs.microsoft.com/windows/ai/get-started-uwp)：按照此分步教程，使用 Windows 机器学习创建第一个 UWP 应用程序。 </br> * [Windows.AI.MachineLearning 命名空间](https://docs.microsoft.com/uwp/api/windows.ai.machinelearning)：已更新最新版本 Windows 10 SDK 的 API 参考，开发人员现可将此 API 用于 Win32 和 UWP 应用程序。
Windows Mixed Reality | 如果显示硬件支持，开发人员现在可以请求受硬件保护的后缓冲纹理，这可让应用程序使用 PlayReady 等源中受硬件保护的内容。 通过使用 [Windows.Graphics.Holographic.HolographicCamera](https://docs.microsoft.com/uwp/api/windows.graphics.holographic.holographiccamera) 的新属性，可将硬件保护支持与设置用于主层，而通过 [Windows.Graphics.Holographic.HolographicQuadLayerUpdateParameters](https://docs.microsoft.com/uwp/api/windows.graphics.holographic.holographicquadlayerupdateparameters)，可将其用于四层。

## <a name="iot-core"></a>IoT 核心版

功能 | 说明
 :------ | :------
AssignedAccessSettings | 使用 [AssignedAccessSettings 类](https://docs.microsoft.com/uwp/api/windows.system.userprofile.assignedaccesssettings)，可调用不同的方法和属性来访问用户针对特定设备已分配访问设置。
默认应用概述 | [Windows 10 IoT 核心版默认应用](https://docs.microsoft.com/windows/iot-core/develop-your-app/iotcoredefaultapp)已更新，添加了新的特性和功能（例如天气、墨迹书写和音频）。
仪表板 | [Windows 10 Iot 核心版仪表板](https://docs.microsoft.com/windows/iot-core/tutorials/quickstarter/devicesetup)现在支持开发人员使用 Dragonboard 410C 或 NXP 将自定义 FFU 刷写到其设备上。
屏幕键盘 | [IoT 设备的屏幕键盘](https://docs.microsoft.com/windows/iot-core/develop-your-app/onscreenkeyboard)现在使用与 Windows 桌面版相同的触摸键盘组件。 这可实现诸如听写模式、IME 支持和完整输入范围等特性。
登录对话框标题栏 | Windows 10 IoT 核心版现在提供配置[系统对话框标题栏](https://docs.microsoft.com/windows/iot-core/develop-your-app/signindialogtitlebars)的选项。
触控唤醒 | [触控唤醒](https://docs.microsoft.com/windows/iot-core/learn-about-hardware/wakeontouch)可让设备在未被使用时关闭，在用户触控其屏幕时快速打开。
Windows.System.Update | 新的 [Windows.System.Update 命名空间](https://docs.microsoft.com/uwp/api/windows.system.update)可实现系统更新的交互式控制。 此命名空间仅适用于 Windows 10 IoT 核心版。

## <a name="web-development"></a>Web 开发

功能 | 说明
 :------ | :------
EdgeHTML 18 | Windows 10 2018 年 10 月更新附带 [EdgeHTML 18](https://docs.microsoft.com/microsoft-edge/dev-guide) 更新，这是对 Microsoft Edge 浏览器以及用于 UWP 应用的 JavaScript 引擎的最新更新。 EdgeHTML 18 对 Web 身份验证 API、新 WebView 控件功能等提供新式扩展支持！ 在工具方面，EdgeHTML 18 提供新的 WebDriver 功能和自动更新，并增强了 Edge DevTools 和 Edge DevTools 协议。 有关所有详细信息，请查看 [What’s new in EdgeHTML 18](https://docs.microsoft.com/microsoft-edge/dev-guide)（EdgeHTML 18 的新增功能）和 [DevTools in the latest Windows 10 update (EdgeHTML 18)](https://docs.microsoft.com/microsoft-edge/devtools-guide/whats-new)（最新 Windows 10 更新中的 DevTools (EdgeHTML 18)）。
渐进式 Web 应用 | Windows 10 JavaScript 应用（在 WWAHost.exe 进程中运行的 Wed 应用）现在支持可选的[每应用程序后台脚本](https://docs.microsoft.com/microsoft-edge/dev-guide#progressive-web-apps)，该脚本在激活任何视图之前启动，并在进程持续期间运行。 使用此应用，可以监视和修改导航、跨导航跟踪状态、监视导航错误，以及在激活视图之前运行代码。 如果指定为[应用清单](https://docs.microsoft.com/uwp/schemas/appxpackage/appx-package-manifest)中的 [`StartPage`](https://docs.microsoft.com/uwp/schemas/appxpackage/appxmanifestschema2010-v2/element-application)，每个应用视图（窗口）都作为新 [`WebUIView`](https://docs.microsoft.com/uwp/api/windows.ui.webui.webuiview)类的实例向脚本公开，提供与常规 (Win32) [WebView](https://docs.microsoft.com/uwp/api/windows.web.ui.iwebviewcontrol) 相同的事件、属性和方法。
Web API 扩展 | [旧版 Microsoft API 扩展](https://developer.mozilla.org/docs/Web/API/Microsoft_API_extensions)列表已添加到 Mozilla 开发人员网络文档中，用于进行跨浏览器 Web 开发。 这些 API 扩展是 Internet Explorer 或 Microsoft Edge 所独有的，补充了 MDN Web 文档中有关兼容性和浏览器支持的现有信息。也可使用旧版 Microsoft [CSS 扩展](https://developer.mozilla.org/docs/Web/CSS/Microsoft_Extensions)和 [JavaScript 扩展](https://developer.mozilla.org/docs/Web/JavaScript/Microsoft_JavaScript_extensions)，并且可在 [Visual Studio Code](https://code.visualstudio.com/updates/v1_25#_new-css-pseudo-selectors-and-pseudo-elements-from-mdn) 中直接显示的 MDN 中找到大量 Web API 信息。
WebVR | 我们已对 [WebVR 开发人员指南](https://docs.microsoft.com/microsoft-edge/webvr/)进行了重大更新，其中包括完全重新设计了主页并重新组织了目录。 我们编写了多个新主题，包括： </br> * [什么是 WebVR？](https://docs.microsoft.com/microsoft-edge/webvr/what-is-webvr) 介绍 WebVR 是什么、使用原因，以及如何开始开发它。 </br> * [渐进式 Web 应用中的 WebVR](https://docs.microsoft.com/microsoft-edge/webvr/webvr-in-pwas)：了解如何将 WebVR 添加到渐进式 Web 应用 (PWA)。 </br> * [WebView 中的 WebVR](https://docs.microsoft.com/microsoft-edge/webvr/webvr-in-webview)：了解如何将 WebVR 添加到 Windows 10 应用程序中的 WebView 控件。 </br> * [WebVR 演示](https://docs.microsoft.com/microsoft-edge/webvr/demos)：请使用 Microsoft Edge 和 Windows Mixed Reality 沉浸式头戴显示设备来查看 WebVR 演示。

## <a name="publish--monetize-windows-apps"></a>发布 Windows 应用并实现盈利

功能 | 说明
 :------ | :------
MSIX | [MSIX](https://docs.microsoft.com/windows/msix/overview) 是一种新的 Windows 应用包格式，能为所有 Windows 应用提供现代打包体验。 这种开源的 MSIX 格式既保留了已有程序包的功能，又具有新式部署的功能。
MSIX 打包工具 | 即使你无权访问其源代码，也可使用新的 [MSIX 打包工具](https://docs.microsoft.com/windows/msix/mpt-overview) 以 MSIX 格式重新打包现有桌面应用程序。 它可以在命令行中运行，也可以通过其交互式 UI 运行。
对 MSIX 的 Desktop App Converter 支持 | 通过使用 `-MakeMSIX` 参数，可使用 [Desktop App Converter](https://docs.microsoft.com/windows/uwp/porting/desktop-to-uwp-root) 来输出 MSIX 包。
对 MSIX 的 MakeAppx.exe 工具支持 | 可以使用 MakeAppx.exe 工具为 UWP 应用或传统桌面应用程序创建 MSIX 包。 此工具包含在 Windows 10 SDK 中，并且可以从命令提示符或脚本文件中使用。 </br> 对于 UWP 应用，请参阅[使用 MakeAppx.exe 工具创建应用包](/windows/msix/package/create-app-package-with-makeappx-tool)。 </br> 对于桌面应用程序，请参阅[手动打包桌面应用程序](https://docs.microsoft.com/windows/uwp/porting/desktop-to-uwp-manual-conversion)。
包支持框架 | [包支持框架](https://docs.microsoft.com/windows/msix/package-support-framework-overview)是一个开放源代码工具包，有助于在无权访问源代码时将修补程序应用于现有桌面应用程序，以便其在 MSIX 容器中运行。
Microsoft Store 分析 API | [Microsoft Store 分析 API](../monetize/access-analytics-data-using-windows-store-services.md)现在包含以下新方法： </br> * [获取 UWP 应用的见解数据](../monetize/get-insights-data-for-your-app.md) </br> * [获取桌面应用程序的见解数据](../monetize/get-insights-data-for-your-desktop-app.md) </br>* [获取桌面应用程序的升级块](../monetize/get-desktop-block-data.md) </br> * [获取桌面应用程序的升级块详细信息](../monetize/get-desktop-block-data-details.md)

## <a name="videos"></a>视频

自 Fall Creator 更新以来已经发布了以下视频，其中突出展示了 Windows 10 中面向开发人员的新的和改进的功能。

### <a name="cwinrt"></a>C++/WinRT

C++/WinRT 是一种编写和使用 Windows 运行时 API 的新方式。 它仅在头文件中实现，旨在提供对现代应用功能的一流访问。 [观看视频](https://www.youtube.com/watch?v=TLSul1XxppA&feature=youtu.be)了解其工作原理，然后[阅读开发人员文档](../cpp-and-winrt-apis/index.md)了解详细信息。

### <a name="get-started-for-devs-create-and-customize-a-form-on-windows-10"></a>开发人员入门：在 Windows 10 上创建和自定义窗体

面向 Windows 开发人员的[入门文档](../get-started/index.md)现在提供基本应用开发任务的实践经验。 此视频将引导你了解其中一个主题，并介绍在应用中创建窗体 UI 的基本知识。 [观看视频](https://www.youtube.com/watch?v=AgngKzq4hKI&feature=youtu.be)，查看实际操作中使用的代码，然后[自行查看主题。](https://docs.microsoft.com/windows/uwp/get-started/construct-form-learning-track)

### <a name="enhance-your-bot-with-project-personality-chat"></a>使用“项目个性化聊天”增强机器人

使用“项目个性化聊天”可将可自定义的角色添加到聊天机器人。 通过与 Microsoft Bot Framework SDK 集成，可以添加闲聊功能，从而以更随意的交谈方式与客户进行交互。 [观看视频](https://www.youtube.com/watch?v=5C_uD8g2QKg&feature=youtu.be)了解相关实现方法，然后[尝试互动演示](https://www.microsoft.com/research/project/personality-chat/)进行实践体验。

### <a name="multi-instance-uwp-apps"></a>多实例 UWP 应用

Windows 现在支持运行多个 UWP 应用实例，每个实例都在其自己独立的进程中运行。 [观看视频](https://www.youtube.com/watch?v=clnnf4cigd0&feature=youtu.be)，了解如何创建支持该功能的新应用，然后[阅读开发人员文档](../launch-resume/multi-instance-uwp.md)，获取有关使用该功能的方式和原因的更多指导。

### <a name="xbox-live-unity-plugin"></a>Xbox Live Unity 插件

适用于 Unity 的 Xbox Live 插件支持将 Xbox Live 签名、统计信息、好友列表、云存储和排行榜添加到标题。 [观看视频](https://youtu.be/fVQZ-YgwNpY)了解详细信息，然后[下载 GitHub 包](https://aka.ms/UnityPlugin)以开始使用。

### <a name="one-dev-question"></a>一个开发问题

在“一个开发问题”视频系列中，资深的 Microsoft 开发人员介绍了关于 Windows 开发、团队文化和发展历程的一系列问题。

* [Raymond Chen：Windows 开发和历史](https://www.youtube.com/playlist?list=PLWs4_NfqMtoxjy3LrIdf2oamq1coolpZ7)

* [Larry Osterman：Windows 开发和历史](https://www.youtube.com/playlist?list=PLWs4_NfqMtoyPUkYGpJU0RzvY6PBSEA4K)

* [Aaron Gustafson：渐进式 Web 应用](https://www.youtube.com/playlist?list=PLWs4_NfqMtoyPHoI-CIB71mEq-om6m35I)

* [Chris Heilmann：Webhint 工具](https://www.youtube.com/playlist?list=PLWs4_NfqMtow00LM-vgyECAlMDxx84Q2v)

## <a name="samples"></a>示例

### <a name="customer-orders-database"></a>客户订单数据库

已更新[客户订单数据库示例](https://github.com/Microsoft/Windows-appsample-customers-orders-database)，以使用新控件（例如，[DataGrid](https://docs.microsoft.com/windows/communitytoolkit/controls/datagrid)、[NavigationView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview) 和 [Expander](https://docs.microsoft.com/windows/communitytoolkit/controls/expander)）。

### <a name="customer-database-tutorial"></a>客户数据库教程

[客户数据库教程](../enterprise/customer-database-tutorial.md)创建用于管理客户列表的基本 UWP 应用，并介绍企业开发中有用的概念和实践。 该教程引导你对本地 SQLite 数据库实现 UI 元素并添加操作，还提供了有关连接到远程 REST 数据库以进行进一步操作的粗略指南。

### <a name="photo-editor-cwinrt"></a>照片编辑器 C++/WinRT

[照片编辑器示例应用](https://github.com/Microsoft/Windows-appsample-photo-editor)通过 [C++/WinRT](../cpp-and-winrt-apis/intro-to-using-cpp-with-winrt.md) 语言投影展示开发。 使用此应用，可以从“图片”库中检索照片，然后使用关联的照片效果来编辑所选图像。

### <a name="windows-machine-learning"></a>Windows 机器学习

[Windows-Machine-Learning](https://github.com/Microsoft/Windows-Machine-Learning) 存储库已更新，现在适用于最新的 Windows 10 SDK，并且包含用 C#、C++ 和 JavaScript 编写的示例。

### <a name="xaml-hosting-api"></a>XAML 托管 API

[XAML 托管 API 示例](https://github.com/Microsoft/Windows-appsample-Xaml-Hosting)是一款 Win32 桌面应用，使用 UWP XAML 托管 API（也称为 XAML 岛）突出展示了分类的方案。 该项目将 Windows Ink、Media Player 和导航视图控件融入库样式的演示文稿中。 除了常规控件用法外，该示例还演示了如何处理 XAML 和本机 Windows 事件/消息，以及基本的 XAML 数据绑定。
