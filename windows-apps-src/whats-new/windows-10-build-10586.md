---
title: Windows 10 内部版本 10586 中的新增功能 - 2015 年 11 月
description: Windows 10 内部版本 10586 和新开发人员工具将提供新的通用 Windows 平台支持的工具、功能和体验。
keywords: 新增功能, 新功能, 更新, 新, 功能, 新增, Windows 10, 1511, 11 月, 10586
ms.date: 11/02/2017
ms.topic: article
ms.assetid: 0d6c65c5-2ad5-46c7-964e-a3a9833c94ce
ms.localizationpriority: medium
ms.openlocfilehash: 27dc7af20c856fd9eb1b5a01f4e0e5dd862c434c
ms.sourcegitcommit: aaa4b898da5869c064097739cf3dc74c29474691
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/13/2019
ms.locfileid: "66372896"
---
# <a name="whats-new-in-windows-10-for-developers-build-10586"></a>面向开发人员的 Windows 10 内部版本 10586 中的新增功能

Windows 10 内部版本 10586（又称 11 月更新或版本 1511）与 Visual Studio 2017 和更新的 SDK 相结合，为打造出色的通用 Windows 平台应用提供了丰富的工具、功能和体验。 只需在 Windows 10 上[安装工具和 SDK](https://go.microsoft.com/fwlink/?LinkId=821431)，你便可以随时[创建新的通用 Windows 应用](../get-started/create-uwp-apps.md)，或了解如何使用 [Windows 上的现有应用代码](../porting/index.md)。

## <a name="windows-10-build-10586---november-2015"></a>Windows 10 内部版本 10586 - 2015 年 11 月

功能 | 描述
 :---- | :----
 用户体验 | 新的 [Windows.UI.StartScreen.JumpList](https://docs.microsoft.com/uwp/api/windows.ui.startscreen) 和 [Windows.UI.StartScreen.JumpListItem](https://docs.microsoft.com/uwp/api/windows.ui.startscreen) 类为应用提供的功能有：以编程方式选择它们需要使用的系统管理的跳转列表类型、向其跳转列表添加自定义任务入口点以及向其跳转列表添加自定义组。
 Input | [键盘传递侦听器](https://docs.microsoft.com/uwp/api/windows.ui.input.keyboarddeliveryinterceptor)。 使应用能够替代系统对原始键盘输入的处理，包括快捷键、访问键（或热键）、加速键和应用程序键，但不包括安全注意序列 (SAS) 组合键。 安全注意序列 (SAS) 组合键（包括 Ctrl-Alt-Del 和 Windows-L）继续由系统处理。 <br /><br />同时适用于 [UWP 应用](https://docs.microsoft.com/uwp/api/windows.ui.core.corewindow)和[经典 Windows 应用](https://msdn.microsoft.com/library/windows/desktop/hh454903(v=vs.85).aspx)的指针输入跨进程链接。 支持输入跨进程链接的新指针事件。 <br /><br />[适用于经典桌面应用的墨迹演示器](https://msdn.microsoft.com/library/windows/desktop/mt622165(v=vs.85).aspx)。 墨迹表示器 API 使 Microsoft Win32 应用能够通过插入应用的 [DirectComposition](https://msdn.microsoft.com/library/windows/desktop/hh437371(v=vs.85).aspx) 可视化树中的 [InkPresenter](https://docs.microsoft.com/uwp/api/Windows.UI.Input.Inking.InkPresenter) 对象管理墨迹输入（标准和改良）的输入、处理和呈现。
网络 | 对于 WebSocket 用户：[MessageWebSocket.OutputStream.FlushAsync](https://docs.microsoft.com/uwp/api/windows.storage.streams.datawriter.flushasync) 和 [StreamWebSocket.OutputStream.FlushAsync](https://docs.microsoft.com/uwp/api/windows.storage.streams.datawriter.flushasync) 已完全实现，并等待之前发出的 WriteAsync 调用完成。 请注意，如果 WebSocket 在你调用 [FlushAsync](https://docs.microsoft.com/uwp/api/windows.storage.streams.datawriter.flushasync) 时处于无效状态，这可能导致现有代码引发异常。 <br /><br />新属性 [CookieUsageBehavior](https://docs.microsoft.com/uwp/api/windows.web.http.filters.httpbaseprotocolfilter) 已添加到现有的 [Windows.Web.Http.Filters.HttpBaseProtocolFilter 类](https://docs.microsoft.com/uwp/api/windows.web.http.filters.httpbaseprotocolfilter)。 这使得开发人员能够控制系统处理 Cookie 的方式。
ORTC | Microsoft Edge 现在实现了 [ORTC（对象实时通信）](https://msdn.microsoft.com/library/mt433097(v=vs.85).aspx)，支持通过本地 Javascript API 直接在浏览器、移动设备和服务器之间进行 Web 实时音频/视频通话。 开发人员现在可以使用 ORTC API 在 Microsoft Edge 浏览器顶部生成支持群组视频通话、同时联播、可伸缩视频编码 (SVC) 等的高级实时音频/视频通信应用程序。 有关在 Microsoft Edge 浏览器之间通过 ORTC API 进行 1:1 音频/视频通话的演示，请访问 [Test Drive 站点和演示](https://developer.microsoft.com/microsoft-edge/testdrive/demos/ortcdemo/)。
Microsoft Edge F12 开发人员工具 | Microsoft Edge 向 F12 开发人员工具引入了一些出色的新改进，包括 [UserVoice](https://wpdev.uservoice.com/forums/257854-microsoft-edge-developer) 中某些最受期待的功能。 了解关于 DOM 资源管理器、控制台、调试器、网络、性能、内存、仿真方面的新增功能，并了解新的实验工具，该工具允许你在强大的新功能完成开发之前先试用它们。 这些新工具内置于 TypeScript 中，并且会一直运行，因此无需重新加载。 此外，F12 开发人员工具文档现在是 [Microsoft Edge 开发人员站点](https://developer.microsoft.com/microsoft-edge/)的一部分，并且在 [GitHub](https://github.com/MicrosoftEdge/MicrosoftEdge-Documentation) 上完整提供。 从此时起，这些文档将不仅受你的反馈影响，我们还会邀请你帮助我们共同打造文档。 有关 F12 开发人员工具的视频简介，请访问[第 9 频道的 One Dev Minute](https://channel9.msdn.com/Blogs/One-Dev-Minute/Microsoft-Edge-F12-tools)。
Windows Hello | Windows Hello 支持你的应用通过面部或指纹识别来登录到 Windows 系统或设备。 提供程序 API 允许 IHV 和 OEM 在 UWP 中公开计算机影像的深度、红外和彩色相机（以及相关元数据），并在进行 Windows Hello 面部身份验证时指定一个相机。 [Windows.Devices.Perception](https://docs.microsoft.com/uwp/api/windows.devices.perception) 命名空间包含了一些客户端 API，它们允许 UWP 应用程序访问计算机视觉相机的色彩、深度或红外线数据。
新的游戏 API | 使用新的 Windows.Gaming.UI.GameBar 类以便在显示或隐藏游戏栏时接收通知。
蓝牙 API | 已添加并更新多个 API，以扩展对蓝牙 LE、设备枚举以及蓝牙中其他功能的支持。 请参阅 [Windows.Devices.Bluetooth](https://docs.microsoft.com/uwp/api/windows.devices.bluetooth) 命名空间。
智能卡 API | 已向 [Windows.Devices.SmartCards](https://docs.microsoft.com/uwp/api/windows.devices.smartcards) 命名空间添加了多个 SmartCardCryptogram API 以支持安全密文付款协议。 使用主机卡仿真来支持触碰以支付的付款应用可以使用这些 API 获得额外的安全性和性能。 应用可以使用 TPM 创建密钥并保护有限使用的交易密钥。 应用还可以利用 NGC（下一代凭据）框架通过用户的 PIN 来保护密钥。 这些 API 会将密文生成委派给系统以增强性能。 这也将阻止其他应用对这些密钥和密文进行任何访问。
更新的存储 API | 在 [Windows.Storage.DownloadsFolder 类](https://docs.microsoft.com/uwp/api/windows.storage.downloadsfolder)中，你的应用现在可以在“下载”文件夹内针对特定[用户](https://docs.microsoft.com/uwp/api/windows.system.user)[创建文件](https://docs.microsoft.com/uwp/api/windows.storage.downloadsfolder.createfileforuserasync)或[创建文件夹](https://docs.microsoft.com/uwp/api/windows.storage.downloadsfolder.createfolderforuserasync)。 在 [Windows.Storage.StorageLibrary 类](https://docs.microsoft.com/uwp/api/windows.storage.storagelibrary)中，你的应用现在可以针对特定[用户](https://docs.microsoft.com/uwp/api/windows.system.user)[获取指定的“库”](https://docs.microsoft.com/uwp/api/windows.storage.storagelibrary.getlibraryforuserasync)。
Windows 应用认证工具包 | Windows 应用认证工具包已通过改进的测试进行了更新。 有关更新的完整列表，请访问 [Windows 应用认证工具包](https://developer.microsoft.com/windows/develop/app-certification-kit)页面。
设计下载 | 查看适用于 Adobe Photoshop 的全新 UWP 应用设计模板。 我们还更新了 Microsoft PowerPoint 和 Adobe Illustrator 模板，并提供了 PDF 版本的指南。 [访问设计下载页面](https://developer.microsoft.com/windows/design/assets)。