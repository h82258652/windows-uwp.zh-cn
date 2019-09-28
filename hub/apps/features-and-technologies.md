---
Description: 本部分可帮助你了解不同应用平台中的某些关键 Windows 功能的支持方式，以及如何开始在代码中使用这些功能。
title: 功能和技术
ms.topic: article
ms.date: 05/08/2019
ms.localizationpriority: medium
ms.author: mcleans
author: mcleanbyron
ms.openlocfilehash: 4fb49b5145350360be3b86c73110762cfa30e9db
ms.sourcegitcommit: a20457776064c95a74804f519993f36b87df911e
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71339345"
---
# <a name="features-and-technologies-for-windows-apps"></a>适用于 Windows 应用的功能和技术

无论正在构建的应用类型或目标设备是什么类型，Windows 都支持许多功能，这些功能是重要应用方案的关键构建基块。 其中一些功能以不同的方式公开到通用 Windows 平台（UWP）、Win32 （Windows API）和其他应用程序平台。 以下文章可帮助你了解不同应用程序平台中某些 Windows 功能的支持方式，以及如何开始在代码中使用这些功能。

本文提供了一系列定制的文章，以详细了解如何在 UWP、Win32 （Windows API）、WPF 和 Windows 窗体应用平台中访问重要的 Windows 功能和技术。 有关每个平台的开发功能的完整信息，请参阅以下资源：

* [UWP 文档](/windows/uwp/index)
* [Win32 （Windows API）文档](/windows/desktop/index)
* [WPF 文档](https://docs.microsoft.com/dotnet/framework/wpf/index)
* [Windows 窗体文档](https://docs.microsoft.com/dotnet/framework/winforms/index)

## <a name="key-windows-features-and-technologies"></a>关键的 Windows 功能和技术

以下各节重点介绍了几项重要的 Windows 功能和技术，这些功能使你能够交付新式并为客户提供引人注目的体验。

### <a name="windows-ink"></a>Windows Ink

![Surface 触控笔](images/hero-small.png)  

Windows Ink 平台与笔设备一起提供了一种创建数字手写便笺、绘图和批注的自然方法。 该平台支持将数字化器输入捕获为墨迹数据、生成墨迹数据、管理墨迹数据、在输出设备上以笔划墨迹形式呈现墨迹数据以及通过手写识别将墨迹转换为文本。

有关在 Windows 应用中使用 Windows Ink 的不同方法的详细信息，请参阅[Windows ink](/windows/uwp/design/input/pen-and-stylus-interactions)。

### <a name="speech-interactions"></a>语音交互

![基于 sgrs 语法文件的约束的初始识别屏幕](images/speech-listening-initial.png)

![基于 sgrs 语法文件的约束的最终识别屏幕](images/speech-listening-complete.png)

Windows 提供多种方式来将语音识别和文本到语音转换（也称为 TTS 或语音合成）直接集成到应用程序的用户体验。 语音可以是一种强大而愉快的方式，使用户能够与你的应用程序、进行补充，甚至替换、键盘、鼠标、触摸和手势交互。

有关在 Windows 应用中使用语音交互的不同方法的详细信息，请参阅[语音交互](/windows/uwp/design/input/speech-interactions)。

### <a name="windows-ai"></a>Windows AI

![Windows AI](images/windows-ai.png)

我们提供了几种不同的 AI 解决方案，可用于增强你的 Windows 应用程序。 通过 Windows 机器学习，你可以将定型的机器学习模型集成到你的应用，并在设备上本地运行。 Windows 视觉技能使你可以使用预建库来完成常见的图像处理任务，或创建你自己的自定义解决方案。 DirectML 提供低级别的 DirectX 样式 Api，可让你充分利用硬件。

有关在 Windows 应用中集成 AI 的不同方法的详细信息，请参阅[WINDOWS ai](https://docs.microsoft.com/windows/ai/)。

## <a name="features-and-technologies-by-platform"></a>按平台的功能和技术

以下部分提供了一些有用的链接，可详细了解如何与主要应用平台中的核心 Windows 功能和技术集成：UWP、Win32 （Windows API）、WPF 和 Windows 窗体。

### <a name="user-interface-and-accessibility"></a>用户界面和辅助功能

|  UWP  |  Win32 （Windows API） |  WPF  |  Windows 窗体  |
|-------|----------------------|-------|-----------------|
| [设计](/windows/uwp/design/basics/)<br/><br/>[布局](/windows/uwp/design/layout/)<br/><br/>[控件](/windows/uwp/design/controls-and-patterns/)<br/><br/>[送](/windows/uwp/design/input/)<br/><br/>[磁贴](/windows/uwp/design/shell/tiles-and-notifications/creating-tiles)<br/><br/>[可视化层](/windows/uwp/composition/visual-layer)<br/><br/>[XAML 平台](/windows/uwp/xaml-platform/)<br/><br/>[启动、恢复和后台任务](/windows/uwp/launch-resume/)<br/><br/>[Windows 辅助功能](/windows/uwp/design/accessibility/accessibility)<br/><br/>  |  [桌面用户界面](/windows/desktop/windows-application-ui-development)<br/><br/>[桌面环境和 shell](/windows/desktop/user-interface)<br/><br/>[Windows 控件](/windows/desktop/controls/window-controls)<br/><br/>[桌面应用中的 UWP 控件（XAML 岛）](/windows/apps/desktop/modernize/xaml-islands)<br/><br/>[桌面应用中的 UWP 视觉层](/windows/apps/desktop/modernize/visual-layer-in-desktop-apps)<br/><br/>[Windows 和消息](/windows/desktop/winmsg/windowing)<br/><br/>[菜单和其他资源](/windows/desktop/menurc/resources)<br/><br/>[高 DPI](/windows/desktop/hidpi/high-dpi-desktop-application-development-on-windows)<br/><br/>[辅助功能](/windows/desktop/accessibility)<br/><br/>  |  [WPF 中的窗口](https://docs.microsoft.com/dotnet/framework/wpf/app-development/windows-in-wpf-applications)<br/><br/>[导航概述](https://docs.microsoft.com/dotnet/framework/wpf/app-development/navigation-overview)<br/><br/>[WPF 中的 XAML](https://docs.microsoft.com/dotnet/framework/wpf/advanced/xaml-in-wpf)<br/><br/>[控件](https://docs.microsoft.com/dotnet/framework/wpf/controls/)<br/><br/>[可视层编程](https://docs.microsoft.com/dotnet/framework/wpf/graphics-multimedia/visual-layer-programming)<br/><br/>[送](https://docs.microsoft.com/dotnet/framework/wpf/advanced/input-wpf)<br/><br/>[辅助功能](https://docs.microsoft.com/dotnet/framework/ui-automation/)<br/><br/>  | [创建 Windows 窗体](https://docs.microsoft.com/dotnet/framework/winforms/creating-a-new-windows-form)<br/><br/>[控件](https://docs.microsoft.com/dotnet/framework/winforms/controls/)<br/><br/>[对话框](https://docs.microsoft.com/dotnet/framework/winforms/dialog-boxes-in-windows-forms)<br/><br/>[用户输入](https://docs.microsoft.com/dotnet/framework/winforms/user-input-in-windows-forms)<br/><br/>[Windows 窗体辅助功能](https://docs.microsoft.com/dotnet/framework/winforms/advanced/windows-forms-accessibility)<br/><br/> |

### <a name="audio-video-and-graphics"></a>音频、视频和图形

|  UWP  |  Win32 （Windows API） |  WPF  |  Windows 窗体  |
|-------|----------------------|-------|-----------------|
| [音频、视频和相机](/windows/uwp/audio-video-camera/)<br/><br/>[媒体播放](/windows/uwp/audio-video-camera/media-playback/)<br/><br/>[可视化层](/windows/uwp/composition/visual-layer)<br/><br/>[XAML 平台](/windows/uwp/xaml-platform/) |  [音频和视频](/windows/desktop/audio-and-video)<br/><br/>[图形和游戏](/windows/desktop/graphics-and-multimedia)<br/><br/>[DirectX](/windows/desktop/getting-started-with-directx-graphics)<br/><br/>[Direct2D](/windows/desktop/direct2d/direct2d-portal)<br/><br/>[Direct3D](/windows/desktop/direct3d)<br/><br/>[Windows GDI](/windows/desktop/gdi/windows-gdi)<br/><br/>[+](/windows/desktop/gdiplus/-gdiplus-gdi-start)  |  [显卡](https://docs.microsoft.com/dotnet/framework/wpf/graphics-multimedia/graphics)<br/><br/>[Mutimedia](https://docs.microsoft.com/dotnet/framework/wpf/graphics-multimedia/multimedia-overview)  |  [图形和绘图](https://docs.microsoft.com/dotnet/framework/winforms/advanced/graphics-and-drawing-in-windows-forms)<br/><br/>[SoundPlayer 类](https://docs.microsoft.com/dotnet/framework/winforms/controls/soundplayer-class-overview)  |

### <a name="data-access-and-app-resources"></a>数据访问和应用资源

|  UWP  |  Win32 （Windows API） |  WPF  |  Windows 窗体  |
|-------|----------------------|-------|-----------------|
| [数据访问](/windows/uwp/data-access/)<br/><br/>[数据绑定](/windows/uwp/data-binding/)<br/><br/>[文件、文件夹和库](/windows/uwp/files/)<br/><br/>[应用资源](/windows/uwp/app-resources/) |  [数据访问和存储](/windows/desktop/data-access-and-storage)<br/><br/>[本地文件系统](/windows/desktop/fileio/file-systems)<br/><br/>[资源概述](/windows/desktop/menurc/resources-overviews)</li>  |  [数据和建模](https://docs.microsoft.com/dotnet/framework/data/)<br/><br/>[数据绑定](https://docs.microsoft.com/dotnet/framework/wpf/data/data-binding-wpf)<br/><br/>[.NET 应用中的资源](https://docs.microsoft.com/dotnet/framework/resources/)<br/><br/>[应用程序资源、内容和数据文件](https://docs.microsoft.com/dotnet/framework/wpf/app-development/wpf-application-resource-content-and-data-files)  |  [数据和建模](https://docs.microsoft.com/dotnet/framework/data/)<br/><br/>[数据绑定](https://docs.microsoft.com/dotnet/framework/winforms/windows-forms-data-binding)<br/><br/>[.NET 应用中的资源](https://docs.microsoft.com/dotnet/framework/resources/)<br/><br/>[应用程序设置](https://docs.microsoft.com/dotnet/framework/winforms/advanced/application-settings-for-windows-forms)  |

### <a name="devices-documents-and-printing"></a>设备、文档和打印

|  UWP  |  Win32 （Windows API） |  WPF  |  Windows 窗体  |
|-------|----------------------|-------|-----------------|
| [启用设备功能](/windows/uwp/devices-sensors/enable-device-capabilities)<br/><br/>[枚举设备](/windows/uwp/devices-sensors/enumerate-devices)<br/><br/>[传感器](/windows/uwp/devices-sensors/sensors)<br/><br/>[蓝牙](/windows/uwp/devices-sensors/bluetooth)<br/><br/>[打印和扫描](/windows/uwp/devices-sensors/printing-and-scanning)<br/><br/>[NFC](/windows/uwp/devices-sensors/nfc) | [传感器 API](/windows/desktop/sensorsapi/portal)<br/><br/>[打印](/desktop/printdocs/printdocs-printing)<br/><br/>[UPnP Api](/desktop/upnp/universal-plug-and-play-start-page) |  [打印和打印系统管理](https://docs.microsoft.com/dotnet/framework/wpf/advanced/printing-and-print-system-management)  |  [打印支持](https://docs.microsoft.com/dotnet/framework/winforms/advanced/windows-forms-print-support)  |

### <a name="system-network-and-power"></a>系统、网络和电源

|  UWP  |  Win32 （Windows API） |  WPF  |  Windows 窗体  |
|-------|----------------------|-------|-----------------|
| [枚举设备](/windows/uwp/devices-sensors/enumerate-devices)<br/><br/>[获取电池信息](/windows/uwp/devices-sensors/get-battery-info)<br/><br/>[线程和异步编程](/windows/uwp/threading-async/)<br/><br/>[网络和 Web 服务](/windows/uwp/networking/) | [系统服务](/windows/desktop/system-services)<br/><br/>[内存管理](/windows/desktop/memory/memory-management)<br/><br/>[电源管理](/windows/desktop/power/power-management-portal)<br/><br/>[进程和线程](/windows/desktop/procthread/processes-and-threads)<br/><br/>[网络和 Internet](/windows/desktop/networking)<br/><br/>[Windows 系统信息](/windows/desktop/sysinfo/windows-system-information) |  [线程模型](https://docs.microsoft.com/dotnet/framework/wpf/advanced/threading-model)<br/><br/>[.NET Framework 中的网络编程](https://docs.microsoft.com/dotnet/framework/network-programming/)  |  [系统信息](https://docs.microsoft.com/dotnet/framework/winforms/advanced/system-information-and-windows-forms)<br/><br/>[电源管理](https://docs.microsoft.com/dotnet/framework/winforms/advanced/power-management-in-windows-forms)<br/><br/>[.NET Framework 中的网络编程](https://docs.microsoft.com/dotnet/framework/network-programming/)<br/><br/>[Windows 窗体中的网络](https://docs.microsoft.com/dotnet/framework/winforms/advanced/networking-in-windows-forms-applications)  |

### <a name="packaging-and-deployment"></a>打包和部署

|  UWP  |  Win32 （Windows API） |  WPF  |  Windows 窗体  |
|-------|----------------------|-------|-----------------|
| [打包应用](/windows/uwp/packaging/)<br/><br/>[MSIX](https://docs.microsoft.com/windows/msix/)<br/><br/>[应用包清单架构](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/schema-root) | [打包 Windows 桌面应用程序（.MSIX）](/windows/msix/desktop/desktop-to-uwp-root)<br/><br/>[应用程序安装和维护](/windows/desktop/application-installing-and-servicing)<br/><br/>[Windows 安装程序](/windows/desktop/msi/windows-installer-portal) |  [打包 Windows 桌面应用程序（.MSIX）](/windows/msix/desktop/desktop-to-uwp-root)<br/><br/>[部署 .NET Framework 和应用程序](https://docs.microsoft.com/dotnet/framework/deployment/)<br/><br/>[部署 WPF 应用程序](https://docs.microsoft.com/dotnet/framework/wpf/app-development/deploying-a-wpf-application-wpf)  |  [打包 Windows 桌面应用程序（.MSIX）](/windows/msix/desktop/desktop-to-uwp-root)<br/><br/>[部署 .NET Framework 和应用程序](https://docs.microsoft.com/dotnet/framework/deployment/)<br/><br/>[Windows 窗体的 ClickOnce 部署](https://docs.microsoft.com/dotnet/framework/winforms/clickonce-deployment-for-windows-forms)  |
