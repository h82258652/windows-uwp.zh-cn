---
author: v-angraf
title: "Xbox One 上的 UWP 的新增功能"
description: "突出显示 Xbox One 上的 UWP 应用的新功能。"
ms.author: v-angraf
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.assetid: fe63c527-8f06-43a5-868f-de909f5664b3
ms.openlocfilehash: 5546177401630e8938f0d25d77ea42afdbfb55d7
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="whats-new-for-developers-in-the-latest-update-of-uwp-on-xbox-one"></a>Xbox One 上的 UWP 的最新更新中面向开发人员的新增功能

Xbox One 上的通用 Windows 平台 (UWP) 的 2016 年 7 月版本包含以下新功能、现有功能的更新和 Bug 修复。

## <a name="networking-using-tcpudp-sockets-is-now-available"></a>使用 TCP/UDP 套接字的网络现已可用  
现在可以使用传统 TCP/UDP 套接字（WinSock、Windows.Networking.Sockets）执行主机的入站和出站网络访问。

## <a name="fiddler-support"></a>Fiddler 支持
对于已在 Xbox One 上启用 UWP 的主机，现在可以启用 Fiddler 作为代理。 Fiddler 允许你记录和检查与 Xbox 服务和依赖方 Web 服务之间的所有 HTTP/HTTPS 流量。 有关详细信息，请参阅[在针对 UWP 进行开发时如何将 Fiddler 用于 Xbox One](uwp-fiddler.md)。

## <a name="mouse-mode-is-now-enabled-by-default"></a>默认情况下，鼠标模式现在处于启用状态
对于 XAML 和托管的 Web 应用，默认情况下，鼠标模式现在处于启用状态。
我们强烈建议你关闭此模式，并针对方向控制器导航进行优化。
若要了解如何关闭鼠标模式，请参阅[如何禁用鼠标模式](how-to-disable-mouse-mode.md)。
有关如何为 Xbox 生成出色应用的详细信息，请参阅[针对 Xbox 和电视进行设计](../input-and-devices/designing-for-tv.md#mouse-mode)。

## <a name="extended-uwp-api-surface-area-is-now-functional-on-the-console"></a>扩展的 UWP API 图面区域现已在主机上正常工作
其他 UWP API 现在可以在 Xbox 主机上正常工作。 有关 UWP API 支持的详细信息，请参阅 [Xbox 上尚不支持的 UWP 功能](http://go.microsoft.com/fwlink/p/?LinkID=760755)。 

## <a name="background-music-and-audio-capabilities"></a>背景音乐和音频功能
你现在可以通过在后台运行的应用来播放音乐和音频。

## <a name="xaml-improvements"></a>XAML 改进
已对 XAML 平台作了以下改进：
-    焦点矩形的风格现已适配电视 10 英尺体验。
-    Xbox 声音现已嵌入 XAML 平台。
-    UI 元素之间的 XY 焦点导航已得到改进。 

## <a name="you-can-now-change-the-size-of-allocated-developer-storage-on-the-console"></a>现在，你可以更改主机上已分配的开发人员存储的大小。
借助 Dev Home 应用中的新设置，你可以增大或减小主机上已分配的开发人员存储的大小。 有关更改已分配的开发人员存储大小的详细信息，请参阅 [Xbox One 工具简介](introduction-to-xbox-tools.md)。

## <a name="wdp-tool-enhancements"></a>WDP 工具增强功能
已针对 Xbox 对 Windows Device Portal (WDP) 进行了以下改进：
 - 该工具包括其他主机设置。 有关主机设置的详细信息，请参阅 [/ext/settings](wdp-xboxsettings-api.md) 参考主题。 
 - 用户可以登录和注销主机。 有关用户的详细信息，请参阅 [/ext/user](wdp-user-management.md) 参考主题。
 - 现在，你可以捕获主机的屏幕截图。 有关获取屏幕截图的详细信息，请参阅 [/ext/screenshot](wdp-media-capture-api.md) 参考主题。
 - 该工具可以部署 Loose 文件版本的应用。 有关 Loose 文件版本的详细信息，请参阅 [/api/app/packagemanager/register](wdp-loose-folder-register-api.md) 参考主题。
 - 可以从开发电脑上的“文件资源管理器”访问主机上的开发人员文件。 有关通过“文件资源管理器”访问文件的详细信息，请参阅 [/ext/smb/developerfolder](wdp-smb-api.md) 参考主题。

## <a name="see-also"></a>另请参阅
- [已知问题](known-issues.md)
- [Xbox One 上的 UWP](index.md)
