---
author: QuinnRadich
title: Windows 10 版本 10586 中的新增功能 - 2015 年 11 月
description: Windows 10 版本 10586 和新开发人员工具将提供新的通用 Windows 平台支持的工具、功能和体验。
keywords: 新增功能, 更新, 功能, 新增, Windows 10, 1511, 11 月, 10586
ms.author: quradic
ms.date: 11/02/2017
ms.topic: article
ms.assetid: 0d6c65c5-2ad5-46c7-964e-a3a9833c94ce
ms.localizationpriority: medium
ms.openlocfilehash: 99abbc0e06f84fea87c4bbc96cb912424f9a2272
ms.sourcegitcommit: cbe7cf620622a5e4df7414f9e38dfecec1cfca99
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/20/2018
ms.locfileid: "7431583"
---
# <a name="whats-new-in-windows-10-for-developers-build-10586"></a>面向开发人员的 Windows 10 版本 10586 中的新增功能

Windows 10 版本 10586（又称 11 月更新或版本 1511）与 Visual Studio 2017 和更新的 SDK 相结合，为打造出色的通用 Windows 平台应用提供了丰富的工具、功能和体验。 只需在 Windows10 上[安装工具和 SDK](http://go.microsoft.com/fwlink/?LinkId=821431)，你便可以随时[创建新的通用 Windows 应用](../get-started/create-uwp-apps.md)，或了解如何使用 [Windows 上的现有应用代码](../porting/index.md)。

## <a name="windows-10-build-10586---november-2015"></a>Windows 10 版本 10586 - 2015 年 11 月

功能 | 描述
 :---- | :----
 用户体验 | 新的 [Windows.UI.StartScreen.JumpList](https://msdn.microsoft.com/library/windows/apps/windows.ui.startscreen.aspx) 和 [Windows.UI.StartScreen.JumpListItem](https://msdn.microsoft.com/library/windows/apps/windows.ui.startscreen.aspx) 类使应用能够以编程方式选择要使用的系统托管的跳转列表类型、将自定义任务入口点添加到其跳转列表，以及将自定义组添加到其跳转列表。
 输入 | [键盘传递侦听器](https://msdn.microsoft.com/library/windows/apps/windows.ui.input.keyboarddeliveryinterceptor.aspx)。 支持应用替代原始键盘输入的系统处理，包括快捷键、访问键（或热键）、加速键和应用程序键，但不包括安全注意序列 (SAS) 组合键。 安全注意序列 (SAS) 组合键（包括 Ctrl-Alt-Del 和 Windows-L）继续由系统处理。 <br /><br />适用于 [UWP 应用](https://msdn.microsoft.com/library/windows/apps/windows.ui.core.corewindow.aspx)和[经典 Windows 应用](https://msdn.microsoft.com/library/windows/desktop/hh454903(v=vs.85).aspx)的指针输入的跨进程链接。 支持输入的跨进程链接的新指针事件。 <br /><br />[适用于经典桌面应用的墨迹演示器](https://msdn.microsoft.com/library/windows/desktop/mt622165(v=vs.85).aspx)。 墨迹演示器 API 使 Microsoft Win32 应用能够通过已插入应用的 [DirectComposition](https://msdn.microsoft.com/library/windows/desktop/hh437371(v=vs.85).aspx) 可视化树中的 [InkPresenter](https://msdn.microsoft.com/library/windows/desktop/windows.ui.input.inking.inkpresenter.aspx) 对象来管理墨迹输入（标准和改良）的输入、处理和呈现。
网络 | 对于 Websocket 用户：[MessageWebSocket.OutputStream.FlushAsync](https://msdn.microsoft.com/library/windows/apps/windows.storage.streams.datawriter.flushasync.aspx) 和 [StreamWebSocket.OutputStream.FlushAsync](https://msdn.microsoft.com/library/windows/apps/windows.storage.streams.datawriter.flushasync.aspx) 已完全实现，并等待之前发出的 WriteAsync 调用完成。 请注意，如果 WebSocket 在你调用 [FlushAsync](https://msdn.microsoft.com/library/windows/apps/windows.storage.streams.datawriter.flushasync.aspx) 时处于无效状态，这可能会导致现有代码引发异常。 <br /><br />新属性 [CookieUsageBehavior](https://msdn.microsoft.com/library/windows/apps/windows.web.http.filters.httpbaseprotocolfilter.aspx) 已添加到现有的 [Windows.Web.Http.Filters.HttpBaseProtocolFilter 类](https://msdn.microsoft.com/library/windows/apps/windows.web.http.filters.httpbaseprotocolfilter.aspx)。 这使开发人员可以控制系统处理 Cookie 的方式。
ORTC | Microsoft Edge 现在实现了 [ORTC（对象实时通信）](https://msdn.microsoft.com/library/mt433097(v=vs.85).aspx)，支持通过本地 Javascript API 直接在浏览器、移动设备和服务器之间进行 Web 实时音频/视频通话。 由于支持组视频呼叫、同时联播、可伸缩视频编码 (SVC) 等，开发人员现在可以使用 ORTC API 生成基于 Microsoft Edge 浏览器的高级实时音频/视频通信应用程序。 有关通过 ORTC API 在 Microsoft Edge 浏览器之间进行 1:1 音频/视频通话的演示，请访问[测试驱动器站点和演示](https://developer.microsoft.com/microsoft-edge/testdrive/demos/ortcdemo/)。
Microsoft Edge F12 开发人员工具 | Microsoft Edge 新引入了对 F12 开发人员工具的出色改进，包括一些来自 [UserVoice](https://wpdev.uservoice.com/forums/257854-microsoft-edge-developer) 的最热门功能。 了解“DOM 资源管理器”、“控制台”、“调试程序”、“网络”、“性能”、“内存”、“仿真”中的新功能和新的“试验”工具，该工具可以让你在这些强大的新功能完成之前先进行体验。 这些新工具内置于 TypeScript，并且一直在运行，因此无需重新加载。 此外，F12 开发人员工具文档现在是 [Microsoft Edge 开发人员站点](https://developer.microsoft.com/microsoft-edge/)的一部分，并且完全可用于 [GitHub](https://github.com/MicrosoftEdge/MicrosoftEdge-Documentation)。 从此刻起，此文档将不仅因你的反馈而改进，我们还会邀请你帮助共同塑造我们的文档。 有关 F12 开发人员工具的视频简介，请访问[第 9 频道的一份开发备忘录](https://channel9.msdn.com/Blogs/One-Dev-Minute/Microsoft-Edge-F12-tools)。
Windows Hello | Windows Hello 使你的应用可支持面部或指纹识别来登录到 Windows 系统或设备。 提供程序 API 允许 IHV 和 OEM 在 UWP 中公开计算机视觉的深度、红外线和彩色摄像头（以及相关的元数据），并在进行 Windows Hello 面部身份验证时指定一个相机。 [Windows.Devices.Perception](https://msdn.microsoft.com/library/windows/apps/windows.devices.perception.aspx) 命名空间包含了一些客户端 API，它们允许 UWP 应用程序访问计算机视觉相机的色彩、深度或红外线数据。
新游戏 API | 使用新的 Windows.Gaming.UI.GameBar 类接收显示或关闭游戏栏时的通知。
蓝牙 API | 已添加多个 API 并进行了更新以扩展对蓝牙 LE、设备枚举以及蓝牙中其他功能的支持。 请参阅 [Windows.Devices.Bluetooth](https://msdn.microsoft.com/library/windows/apps/windows.devices.bluetooth.aspx) 命名空间。
智能卡 API | 已向 [Windows.Devices.SmartCards](https://msdn.microsoft.com/library/windows/apps/windows.devices.smartcards.aspx) 命名空间添加了多个 SmartCardCryptogram API 以支持安全密文付款协议。 使用主机卡仿真支持“触碰以支付”的付款应用可以使用这些 API 以确保额外的安全性和性能。 这些应用可使用 TPM 创建密钥并保护有限使用的交易密钥。 这些应用还可以通过用户的 PIN 利用 NGC（下一代凭据）框架来保护密钥。 这些 API 将密文生成委派给系统以增强性能。 这也将阻止其他应用对这些密钥和密文进行任何访问。
更新的存储 API | 在 [Windows.Storage.DownloadsFolder 类](https://msdn.microsoft.com/library/windows/apps/windows.storage.downloadsfolder.aspx)中，你的应用现在可以在“下载”文件夹内针对特定[用户](https://msdn.microsoft.com/library/windows/apps/windows.system.user.aspx) [创建文件](https://msdn.microsoft.com/library/windows/apps/windows.storage.downloadsfolder.createfileforuserasync.aspx)或[创建文件夹](https://msdn.microsoft.com/library/windows/apps/windows.storage.downloadsfolder.createfolderforuserasync.aspx)。 在 [Windows.Storage.StorageLibrary 类](https://msdn.microsoft.com/library/windows/apps/windows.storage.storagelibrary.aspx)中，你的应用现在可以针对特定[用户](https://msdn.microsoft.com/library/windows/apps/windows.system.user.aspx) [获取指定的“库”](https://msdn.microsoft.com/library/windows/apps/windows.storage.storagelibrary.getlibraryforuserasync.aspx)。
Windows 应用认证工具包 | Windows 应用认证工具包已更新为包含改进的测试。 有关更新的完整列表，请访问 [Windows 应用认证工具包](https://developer.microsoft.com/windows/develop/app-certification-kit)页面。
设计下载 | 查看我们适用于 Adobe Photoshop 的新 UWP 应用设计模板。 我们还更新了 Microsoft PowerPoint 和 Adobe Illustrator 模板，并提供了 PDF 版本的指南。 [访问设计下载页面](https://developer.microsoft.com/windows/design/assets)。