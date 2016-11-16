---
author: mcleanbyron
ms.assetid: 3aeddb83-5314-447b-b294-9fc28273cd39
description: "了解如何安装 Microsoft Advertising 库。"
title: "安装 Microsoft Advertising 库"
translationtype: Human Translation
ms.sourcegitcommit: 126fee708d82f64fd2a49b844306c53bb3d4cc86
ms.openlocfilehash: c717fa693c6edf8757c3eef79d60193434104bd8


---

# 安装 Microsoft Advertising 库




对于用于 Windows10 的通用 Windows 平台 (UWP) 应用，Microsoft Advertising 库包含在 [Microsoft Store Services SDK](http://aka.ms/store-em-sdk) 中。 此 SDK 是 Visual Studio 2015 和更高版本的扩展。 有关安装此 SDK 的详细信息，请参阅[此文章](microsoft-store-services-sdk.md)。

> 
            **注意**
            &nbsp;&nbsp;如果已安装了 Windows10 SDK (14393) 或更高版本，如果想要将广告添加到 JavaScript/HTML UWP 应用，还必须安装 WinJS 库。 此库过去包含在以前版本的 Windows10 SDK 中，但从 Windows10 SDK (14393) 开始，此库必须单独安装。 若要安装 WinJS，请参阅[获取 WinJS](http://try.buildwinjs.com/download/GetWinJS/)。

对于用于 Windows8.1 和 Windows Phone 8.x 的 XAML 和 JavaScript/HTML 应用，Microsoft Advertising 库包含在[用于 Windows 和 Windows Phone 8.x 的 Microsoft Advertising SDK](http://aka.ms/store-8-sdk) 中。 此 SDK 是 Visual Studio 2015 或 Visual Studio 2013 的扩展。

对于 Windows Phone Silverlight 8.x 应用，Microsoft Advertising 库适用于可下载并安装到项目的 NuGet 程序包中。 有关详细信息，请参阅 [Windows Phone Silverlight 中的 AdControl](adcontrol-in-windows-phone-silverlight.md)。

## 广告的库名称


Windows 和 Windows Phone 8.x 的 Microsoft Store Services SDK 和 Microsoft Advertising SDK 中有几种不同的广告库：

* Microsoft Store Services SDK 包含 Microsoft Advertising 库（可为 XAML 和 JavaScript/HTML 应用提供 [AdControl](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.aspx) 和 [InterstitialAd](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.interstitialad.aspx) 类）。

* 用于 Windows 和 Windows Phone 8.x 的 Microsoft Advertising SDK 包括两组广告库：适用于 Microsoft Advertising 的库（提供适用于 XAML 和 JavaScript/HTML 应用的 [AdControl](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.aspx) 和 [InterstitialAd](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.interstitialad.aspx) 类）和适用于广告中介的库（提供 **AdMediatorControl** 类）。

本文档介绍如何在 Microsoft Advertising 库中使用 **AdControl** 和 **InterstitialAd** 类，以显示横幅或视频间隙广告。 有关将广告中介用于 Windows8.1 和 Windows Phone 8.x 应用的信息，请参阅[使用广告中介使收益最大化](https://msdn.microsoft.com/library/windows/apps/xaml/dn864359.aspx)。

>
            **注意**
            &nbsp;&nbsp;Windows10 的 UWP 应用当前不支持使用 **AdMediatorControl** 类的广告中介。 即将使用与用于横幅广告 (**AdControl**) 和视频间隙广告 (**InterstitialAd**) 相同的 API 推出用于 UWP 应用的服务器端中介。

必须在项目中引用相应的库，才能使用应用代码中的任意广告控件。 下表列出了所有显示在 Visual Studio“引用管理器”对话框中的库的名称。


<table>
    <thead>
        <tr><th>控件名称</th><th>项目类型</th><th>引用管理器中的库名</th><th>版本号</th></tr>
    </thead>
    <tbody>
    <tr>
            <td rowspan="3">
            **AdControl** 和 **InterstitialAd** (XAML)</td>
            <td>UWP</td>
            <td>适用于 XAML 的 Microsoft Advertising SDK</td>
            <td>10.0</td>
        </tr>
        <tr>
            <td>Windows8.1</td>
            <td>适用于 Windows8.1 XAML 的广告中介 SDK</td>
            <td>1.0</td>
        </tr>
        <tr>
            <td>Windows Phone 8.1</td>
            <td>适用于 Windows Phone 8.1 XAML 的广告中介 SDK</td>
            <td>1.0</td>
        </tr>
    <tr>
            <td rowspan="3">
            **AdControl** 和 **InterstitialAd** (JavaScript/HTML)</td>
            <td>UWP</td>
            <td>适用于 JavaScript 的 Microsoft Advertising SDK</td>
            <td>10.0</td>
        </tr>
        <tr>
            <td>Windows8.1</td>
            <td>适用于 Windows8.1 本机 (JS) 的 Microsoft Advertising SDK</td>
            <td>8.5</td>
        </tr>
        <tr>
            <td>Windows Phone 8.1</td>
            <td>适用于 Windows Phone 8.1 本机 (JS) 的 Microsoft Advertising SDK</td>
            <td>8.5</td>
        </tr>
    <tr>
            <td rowspan="3">
            **AdMediatorControl**（仅限 XAML）</td>
            <td>UWP</td>
            <td>Microsoft Advertising 通用 SDK</td>
            <td>1.0</td>
        </tr>
        <tr>
            <td>Windows8.1</td>
            <td>适用于 Windows8.1 XAML 的广告中介 SDK</td>
            <td>1.0</td>
        </tr>
        <tr>
            <td>Windows Phone 8.1</td>
            <td>适用于 Windows Phone 8.1 XAML 的广告中介 SDK</td>
            <td>1.0</td>
        </tr>
    </tbody>
</table>

 

 

 



<!--HONumber=Nov16_HO1-->


