---
author: mcleanbyron
ms.assetid: ca92bed1-ad9e-4947-ad91-87d12de727c0
description: "查看有关 Microsoft Store Services SDK 中 Microsoft Advertising 库的发行说明。"
title: "Microsoft Advertising 库的发行说明"
translationtype: Human Translation
ms.sourcegitcommit: 2f0835638f330de0ac2d17dae28347686cc7ed97
ms.openlocfilehash: b82c4385b0e7089bdddbe094f47f0766f90aa21b

---

# Microsoft Advertising 库的发行说明




本部分提供了 Microsoft Store Services SDK（适用于 UWP 应用）以及适用于 Windows 和 Windows Phone 8.x 的 Microsoft Advertising SDK（适用于 Windows 8.1 和 Windows Phone 8.x 应用）中当前版本的 Microsoft Advertising 库的发行说明。 这些库支持适用于 Windows 10、Windows 8.1、Windows Phone 8.1 和 Windows Phone 8 的 XAML 和 JavaScript/HTML 应用。

## 安装


Microsoft Advertising 库作为 [Microsoft Store Services SDK](http://aka.ms/store-em-sdk)（适用于 UWP 应用）以及[适用于 Windows 和 Windows Phone 8.x 的 Microsoft Advertising SDK](http://aka.ms/store-8-sdk)（适用于 Windows 8.1 和 Windows Phone 8.x 应用的一部分提供。 有关安装该 SDK 以及其中包含的库的详细信息，请参阅[安装 Microsoft Advertising 库](install-the-microsoft-advertising-libraries.md)。

若要获取适用于 Windows Phone 8.x Silverlight 项目的 Microsoft Advertising 程序集，请安装[适用于 Windows 和 Windows Phone 8.x 的 Microsoft Advertising SDK](http://aka.ms/store-8-sdk)、在 Visual Studio 中打开你的项目，然后转到“项目”**** > “添加连接的服务”**** > “广告中介”****即可自动下载程序集。 在执行此操作之后，如果你不想要使用广告中介，可以将广告中介引用从项目中删除。 有关详细信息，请参阅 [Windows Phone Silverlight 中的 AdControl](adcontrol-in-windows-phone-silverlight.md)。


## 卸载以前版本

在安装 Microsoft Store Services SDK（适用于 UWP 应用）或适用于 Windows 和 Windows Phone 8.x 的 Microsoft Advertising SDK（适用于 Windows 8.1 和 Windows Phone 8.x 应用）之前，强烈建议你卸载 Microsoft Universal Ad Client SDK 或 Microsoft Advertising SDK 的所有之前的实例。

## 面向特定于体系结构的生成输出

在使用 Microsoft Advertising 库时，你无法在项目中面向**任何 CPU**。 如果你的项目面向**任何 CPU** 平台，在你添加对 Microsoft Advertising 库的引用后，可能会在项目中看到一条警告。 若要删除此警告，请更新你的项目以使用特定于体系结构的生成输出（例如，**x86**）。 有关详细信息，请参阅[已知问题](known-issues-for-the-advertising-libraries.md)。

## C++ 支持

Microsoft Advertising 库（其中包括 **AdControl** 和 **InterstitialAd** 类）支持在 C++ 和 DirectX 中使用 Windows 运行时互操作性（*互操作*）编写的应用。 有关使用 XAML 和 C++ 编程的详细信息和示例，请参阅[类型系统](https://msdn.microsoft.com/library/windows/apps/xaml/hh755822.aspx)。

## 没有工具箱控件

在 Microsoft Store Services SDK 或适用于 Windows 和 Windows Phone 8.x 的 Microsoft Advertising SDK 中的当前版本的 Microsoft Advertising 库中，没有任何工具箱控件可用于将 **AdControl** 或 **InterstitialAd** 拖动到应用中的设计图面。 有关在标记和代码中添加这些控件的说明，请参阅[开发人员演练](developer-walkthroughs.md)。

## 不再可用的纬度和经度属性

**AdControl** 类不会再有适用于 UWP 应用的“纬度”****和“经度”****属性。 内置于广告控件的代码将以应用的名义检测这些值，并将它们发送到广告服务器。

## 重要提示

请务必完整地阅读最终用户许可协议 (EULA)。 请参阅主题[重要提示 - EULA](important-notice-eula.md)。

 

 



<!--HONumber=Sep16_HO2-->


