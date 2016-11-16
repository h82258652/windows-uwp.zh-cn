---
author: awkoren
Description: "开始使用桌面到 UWP 桥将 Windows 桌面应用程序（如 Win32、WPF 和 Windows 窗体）转换为通用 Windows 平台 (UWP) 应用。"
Search.Product: eADQiWindows 10XVcnh
title: "使用桌面桥将桌面应用引入通用 Windows 平台 (UWP)"
translationtype: Human Translation
ms.sourcegitcommit: 933dcd48c03de1992bbfcf0d86951170264a309f
ms.openlocfilehash: 170ed75d1cd865bfc8e4feb776fce4332cc67850

---

# 使用桌面桥将桌面应用引入通用 Windows 平台 (UWP)

开始使用桌面到 UWP 桥将 Windows 桌面应用程序转换为通用 Windows 平台 (UWP) 应用。

桌面桥是一组可使你将 Windows 桌面应用程序（例如，Win32、Windows Forms 或 WPF）或游戏转换为 UWP 应用或游戏的技术。 转换后，将以面向 Windows10 桌面版的 UWP 应用包（.appx 或 .appxbundle）的形式打包、维护和部署 Windows 桌面应用程序。

支持桌面应用转换为 UWP 程序包的技术有两个部分。 第一个部分是转换过程，该过程将你的现有二进制文件重新打包为 UWP 程序包。 你的代码仍然相同，只是打包方式不同。 第二个部分由 Windows 周年更新中的运行时技术组成，这些技术使 UWP 程序包可以具有作为完全信任（而不是应用容器）运行的可执行文件。 此技术还为已转换的应用提供程序包标识，该标识需要使用某些 UWP API。

## 优势

以下是转换 Windows 桌面应用程序的一些好处： 

**简化了部署**。 使用桥的应用和游戏具有出色的部署体验，可确保用户妥当安装和更新。 如果用户选择卸载应用，则将完全删除它，不留下任何痕迹。 这能减少编写设置体验的时间，并使用户使用最新应用。

**自动更新和许可**。 你的应用能够参与 Windows 应用商店的内置许可和自动更新设施。 自动更新是高度可靠和高效的机制，因为仅下载文件的已更改部分。

**扩大了覆盖范围，并简化了盈利。** 选择通过 Windows 应用商店进行分配可将覆盖范围扩大至数以百万计的 Windows10 用户，这些用户可以通过当地支付选项获取应用、游戏和应用内购买。

**添加 UWP 功能**。  你可以按照自己的节奏向应用的程序包添加 UWP 功能，如 XAML 用户界面、动态磁贴更新、UWP 后台任务、应用服务以及许多其他功能。

**拓展了跨设备用例**。 通过使用桥，你可以将代码逐渐迁移到通用 Windows 平台，以覆盖所有 Windows10 设备，包括手机、Xbox One 和 HoloLens。

## 准备

桌面到 UWP 桥专为易于使用而设计，你可能不需要执行很多操作即可使应用为转换过程准备就绪。 但是，在转换前有一些附加说明和独特的情况需要注意。 参考文章[为桌面到 UWP 桥准备应用](desktop-to-uwp-prepare.md)，并在继续之前解决适用于应用的任何问题。

## 转换

转换应用有几个不同的选项。

**桌面应用转换器 (DAC)**。 DAC 是可自动为你转换应用并进行签名的工具。 使用 DAC 方便并且自动，如果你的应用进行了大量系统修改或者你不确定安装程序的用途，则它很有用。 若要开始使用，请参阅[桌面应用转换器](desktop-to-uwp-run-desktop-app-converter.md)上的文章。 

**手动转换**。 如果使用 xcopy 安装应用，或者你熟悉安装程序对系统所做的更改，手动转换可能更为直接。 这包括创建清单文件、运行 MakeAppx.exe 工具，然后对应用包签名。 有关如何手动转换的详细信息，请参阅[使用桌面桥将应用手动转换到 UWP](desktop-to-uwp-manual-conversion.md)。 

**第三方安装程序**。 以下三个常用第三方安装程序现在支持桌面桥，并且可以仅通过几下单击生成 MSI 安装程序和转换的应用包：[InstallShield by Flexera](http://www.flexerasoftware.com/producer/products/software-installation/installshield-software-installer)、[WiX by FireGiant](https://www.firegiant.com/r/appx) 和 [Advanced Installer by Caphyon](http://www.advancedinstaller.com/uwp-app-package)。 有关详细信息，请访问每个安装程序的各自网站。 

## 增强 

可使用各种 UWP API 美化已转换的桌面应用，以添加磁贴、推动通知等功能。 有关完整代码示例，请参阅 GitHub 上的[到 UWP 的桌面应用桥示例](https://github.com/Microsoft/DesktopBridgeToUWP-Samples)和[通用 Windows 平台 (UWP) 应用示例](https://github.com/Microsoft/Windows-universal-samples)存储库。 若要查看受支持 API 的完整列表，请查看[使用桌面桥转换的应用支持的 UWP API](desktop-to-uwp-supported-api.md)。 

除了调用 UWP API，还可以使用仅供已转换应用访问的功能扩展应用。 其中包括诸如在用户登录时启动进程和文件资源管理器集成之类的方案，旨在平滑处理原始桌面应用和完整 UWP 应用包之间的转换。 有关详细信息，请参阅[桌面桥应用扩展](desktop-to-uwp-extensions.md)。 

## 迁移

使用桥，可将较早的代码逐步迁移到 UWP，同时保留在 Windows 桌面上运行和发布应用的能力。 完全迁移到 UWP（并且应用不再包含任何 WPF/Win32 组件）后，你可以覆盖所有 Windows 设备，包括手机、Xbox One 和 HoloLens。

## 调试

可使用 Visual Studio 调试应用。 有关详细帮助，请咨询[调试使用桌面桥转换的应用](desktop-to-uwp-debug.md)。 

如果你对桌面桥的深层次工作原理感兴趣，请查看[在桌面桥幕后](desktop-to-uwp-behind-the-scenes.md)。 

## 分配

可使用 Windows 应用商店或通过旁加载分配应用。 有关完整详细信息，请参阅[分配使用桌面桥转换的应用](desktop-to-uwp-distribute.md)。 请注意，你需要先对应用签名，然后才可以将其部署到用户。 有关分布指导，请参阅[对使用桌面桥转换的应用签名](desktop-to-uwp-signing.md)。 

## 支持和反馈

如果在转换应用时遇到问题，可以访问[论坛](https://social.msdn.microsoft.com/Forums/windowsapps/en-US/home?forum=wpdevelop)获取帮助。 

若要提供反馈或提出功能建议，请在 [UserVoice](https://wpdev.uservoice.com/forums/110705-universal-windows-platform/category/161895-desktop-bridge-centennial) 上提交项目或投赞成票。 

## 本部分内容

| 主题 | 描述 |
|-------|-------------|
| [桌面应用转换器](desktop-to-uwp-run-desktop-app-converter.md) | 介绍如何运行桌面应用转换器。 |
| [使用桌面桥手动将你的应用转换到 UWP](desktop-to-uwp-manual-conversion.md) | 了解如何手动创建应用包和清单。 |
| [桌面桥应用扩展](desktop-to-uwp-extensions.md) | 通过扩展增强你的已转换应用，以支持启动任务和文件资源管理器集成等功能。 |
| [使用桌面桥转换的应用支持的 UWP API](desktop-to-uwp-supported-api.md) | 查看哪些 UWP API 可供已转换的桌面应用使用。 |
| [调试使用桌面桥转换的应用](desktop-to-uwp-debug.md) | 说明用于调试已转换应用的选项。 | 
| [对使用桌面桥转换的应用签名](desktop-to-uwp-signing.md) | 了解如何使用证书对已转换的应用包签名。 |
| [分配使用桌面桥转换的应用](desktop-to-uwp-distribute.md) | 查看可如何向用户分配已转换的应用。  |
| [在桌面桥幕后](desktop-to-uwp-behind-the-scenes.md) | 深入研究桌面到 UWP 桥深层次的工作原理。 | 
| [桌面桥的已知问题](desktop-to-uwp-known-issues.md) | 列出桌面到 UWP 桥的已知问题。 | 
| [UWP 代码示例的桌面应用桥](https://github.com/Microsoft/DesktopBridgeToUWP-Samples) | GitHub 上演示已转换应用的功能的代码示例。 |


<!--HONumber=Nov16_HO1-->


