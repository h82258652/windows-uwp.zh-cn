---
author: mcleanbyron
ms.assetid: ca92bed1-ad9e-4947-ad91-87d12de727c0
description: "查看有关 Microsoft 官方商城协定和盈利 SDK 中 Microsoft Advertising 库的发行说明。"
title: "Microsoft Advertising 库的发行说明"
translationtype: Human Translation
ms.sourcegitcommit: cf695b5c20378f7bbadafb5b98cdd3327bcb0be6
ms.openlocfilehash: 8e2114e969b27d579f62195f026cfcfd9672a94a

---

# Microsoft Advertising 库的发行说明


\[ 已针对 Windows 10 上的 UWP 应用更新。 有关 Windows 8.x 文章，请参阅[存档](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

本部分提供有关 Microsoft 官方商城协定和盈利 SDK 中当前版本的 Microsoft Advertising 库的发行说明。 这些库支持适用于 Windows 10、Windows 8.1、Windows Phone 8.1 和 Windows Phone 8 的 XAML 和 JavaScript/HTML 应用。

## 安装


Microsoft Advertising 库作为 [Microsoft 官方商城协定和盈利 SDK](http://aka.ms/store-em-sdk) 的一部分提供。 对于 Windows Phone 8.x Silverlight 以外的所有项目类型，用于在较早独立版本的 Microsoft 通用广告客户端 SDK 和 Microsoft Advertising SDK 中分发的 Microsoft Advertising 程序集现在随 Microsoft 官方商城协定和盈利 SDK 一起安装。 有关安装该 SDK 以及其中包含的库的详细信息，请参阅[安装 Microsoft Advertising 库](install-the-microsoft-advertising-libraries.md)。

若要获取 Windows Phone 8.x Silverlight 项目的 Microsoft Advertising 程序集，请安装 [Microsoft 官方商城协定和盈利 SDK](http://aka.ms/store-em-sdk)、在 Visual Studio 中打开你的项目，然后转到“项目”**** > “添加连接的服务”**** > “广告中介”****以自动下载程序集。 在执行此操作之后，如果你不想要使用广告中介，可以将广告中介引用从项目中删除。 有关详细信息，请参阅 [Windows Phone Silverlight 中的 AdControl](adcontrol-in-windows-phone-silverlight.md)。

## 了解 Microsoft Advertising 库和广告中介之间的区别

虽然 Microsoft Advertising 库和广告中介库均由 Microsoft 官方商城协定和盈利 SDK 提供，但这些库的用途不同。 如果要在 XAML 或 JavaScript 应用中显示来自 Microsoft 的横幅或间隙视频广告，请使用 Microsoft Advertising 库中的 [AdControl](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.aspx) 和 [InterstitialAd](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.interstitialad.aspx) 类。 如果你想要在 XAML 应用（JavaScript/HTML 应用不支持广告中介）中显示来自多个广告网络的横幅广告，请使用广告中介库中的 **AdMediatorControl** 类。 有关详细信息，请参阅[区别是什么：AdMediatorControl 或 AdControl](what-is-the-difference-admediatorcontrol-or-adcontrol.md)。

## 卸载以前的版本

在你安装 Microsoft 官方商城协定和盈利 SDK 之前，强烈建议你卸载 Microsoft 通用广告客户端 SDK 或 Microsoft Advertising SDK 的之前所有实例。

## 面向特定于体系结构的生成输出

在使用 Microsoft Advertising 库时，你无法在项目中面向**任何 CPU**。 如果你的项目面向**任何 CPU** 平台，在你添加对 Microsoft Advertising 库的引用后，可能会在项目中看到一条警告。 若要删除此警告，请更新你的项目以使用特定于体系结构的生成输出（例如，**x86**）。 有关详细信息，请参阅[已知问题](known-issues-for-the-advertising-libraries.md)。

## C++ 支持

Microsoft Advertising 库（其中包括 **AdControl** 和 **InterstitialAd** 类）支持在 C++ 和 DirectX 中使用 Windows 运行时互操作性（*互操作*）编写的应用。 有关使用 XAML 和 C++ 编程的详细信息和示例，请参阅[类型系统](https://msdn.microsoft.com/library/windows/apps/xaml/hh755822.aspx)。

## 没有工具箱控件

在 Microsoft 官方商城协定和盈利 SDK 中当前版本的 Microsoft Advertising 库中，没有任何工具箱控件可用于将 **AdControl** 或 **InterstitialAd** 拖动到应用中的设计图面。 有关在标记和代码中添加这些控件的说明，请参阅[开发人员演练](developer-walkthroughs.md)。

## 不再可用的纬度和经度属性

**AdControl** 类不会再有适用于 UWP 应用的“纬度”****和“经度”****属性。 内置于广告控件的代码将以应用的名义检测这些值，并将它们发送到广告服务器。

## 重要提示

请务必完整地阅读最终用户许可协议 (EULA)。 请参阅主题[重要提示 - EULA](important-notice-eula.md)。

 

 



<!--HONumber=Jun16_HO4-->


