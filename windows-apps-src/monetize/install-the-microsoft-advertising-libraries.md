---
author: mcleanbyron
ms.assetid: 3aeddb83-5314-447b-b294-9fc28273cd39
description: "了解如何安装 Microsoft Advertising 库。"
title: "安装 Microsoft Advertising 库"
translationtype: Human Translation
ms.sourcegitcommit: cf695b5c20378f7bbadafb5b98cdd3327bcb0be6
ms.openlocfilehash: 0951818ceaf3d96543f9f97ec6993d08fdaab2b8


---

# 安装 Microsoft Advertising 库


\[ 已针对 Windows 10 上的 UWP 应用更新。 有关 Windows 8.x 文章，请参阅[存档](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

适用于 Windows 应用的 Microsoft Advertising 库包含在 [Microsoft 官方商城协定和盈利 SDK](http://aka.ms/store-em-sdk) 中。 此 SDK 是对 Visual Studio 2015 或 Visual Studio 2013 的扩展。

有关安装说明，请参阅[使用 Microsoft 官方商城协定和盈利 SDK 来获取应用收益并吸引客户](https://msdn.microsoft.com/windows/uwp/monetize/monetize-your-app-with-the-microsoft-store-engagement-and-monetization-sdk)。

> **注意** 已使用 Visual Studio 2015 安装了 Windows 10 Anniversary SDK Preview Build 14295 或更高版本后，如果你想要将广告添加到 JavaScript/HTML 应用，还必须安装 WinJS 库。 此库过去包含在以前版本的 Windows SDK（适用于 Windows 10）中，但从 Windows 10 Anniversary SDK Preview Build 14295 开始，此库必须单独安装。 若要安装 WinJS，请参阅[获取 WinJS](http://try.buildwinjs.com/download/GetWinJS/)。

## 广告和广告中介的库名称


Microsoft 官方商城协定和盈利 SDK 包括两组广告库：适用于 Microsoft Advertising 的库（提供适用于 XAML 和 JavaScript/HTML 应用的 [AdControl](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.aspx) 和 [InterstitialAd](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.interstitialad.aspx) 类）和适用于广告中介库的库（提供 **AdMediatorControl** 类）。

本文档介绍如何在 Microsoft Advertising 库中使用 **AdControl** 和 **InterstitialAd** 类，以显示来自 Microsoft 的横幅或视频间隙广告。 有关使用广告中介的信息，请参阅[使用广告中介使收益最大化](https://msdn.microsoft.com/windows/uwp/monetize/use-ad-mediation-to-maximize-revenue)。


必须在项目中引用相应的库，才能使用应用代码中的任意广告控件。 下表列出了 Microsoft 官方商城协定和盈利 SDK 中每个库的名称（显示在 Visual Studio 中的“引用管理器”****对话框中）。


<table>
    <thead>
        <tr><th>控件名称</th><th>项目类型</th><th>引用管理器中的库名</th><th>版本号</th></tr>
    </thead>
    <tbody>
    <tr>
            <td rowspan="3">**AdControl** 和 **InterstitialAd** (XAML)</td>
            <td>UWP</td>
            <td>适用于 XAML 的 Microsoft Advertising SDK</td>
            <td>10.0</td>
        </tr>
        <tr>
            <td>Windows 8.1</td>
            <td>适用于 Windows 8.1 XAML 的广告中介 SDK</td>
            <td>1.0</td>
        </tr>
        <tr>
            <td>Windows Phone 8.1</td>
            <td>适用于 Windows Phone 8.1 XAML 的广告中介 SDK</td>
            <td>1.0</td>
        </tr>
    <tr>
            <td rowspan="3">**AdControl** 和 **InterstitialAd** (JavaScript/HTML)</td>
            <td>UWP</td>
            <td>适用于 JavaScript 的 Microsoft Advertising SDK</td>
            <td>10.0</td>
        </tr>
        <tr>
            <td>Windows 8.1</td>
            <td>适用于 Windows 8.1 本机 (JS) 的 Microsoft Advertising SDK</td>
            <td>8.5</td>
        </tr>
        <tr>
            <td>Windows Phone 8.1</td>
            <td>适用于 Windows Phone 8.1 本机 (JS) 的 Microsoft Advertising SDK</td>
            <td>8.5</td>
        </tr>
    <tr>
            <td rowspan="3">**AdMediatorControl**（仅限 XAML）</td>
            <td>UWP</td>
            <td>Microsoft Advertising 通用 SDK</td>
            <td>1.0</td>
        </tr>
        <tr>
            <td>Windows 8.1</td>
            <td>适用于 Windows 8.1 XAML 的广告中介 SDK</td>
            <td>1.0</td>
        </tr>
        <tr>
            <td>Windows Phone 8.1</td>
            <td>适用于 Windows Phone 8.1 XAML 的广告中介 SDK</td>
            <td>1.0</td>
        </tr>
    </tbody>
</table>

 

 

 



<!--HONumber=Jun16_HO4-->


