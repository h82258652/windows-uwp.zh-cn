---
author: mcleanbyron
ms.assetid: adb2fa45-e18f-4254-bd8b-a749a386e3b4
description: "了解如何使用 AdControl 类在适用于 Windows 10 (UWP)、Windows 8.1 或 Windows Phone 8.1 的 JavaScript/HTML 应用中显示横幅广告。"
title: "HTML 5 和 JavaScript 中的 AdControl"
ms.author: mcleans
ms.date: 06/26/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "windows 10, uwp, 广告, AdControl, 广告控件, javascript, HTML"
ms.openlocfilehash: 44417516d773ea4faf103f6d4cdaf0bc8f290921
ms.sourcegitcommit: 8c4d50ef819ed1a2f8cac4eebefb5ccdaf3fa898
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/27/2017
---
# <a name="adcontrol-in-html-5-and-javascript"></a>HTML 5 和 JavaScript 中的 AdControl

本演练介绍如何使用 [AdControl](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.aspx) 类在适用于 Windows 10 (UWP)、Windows 8.1 或 Windows Phone 8.1 的 JavaScript/HTML 应用中显示横幅广告。

有关演示如何将横幅广告添加到 JavaScript/HTML 应用的完整示例项目，请参阅 [GitHub 上的广告示例](http://aka.ms/githubads)。

## <a name="prerequisites"></a>先决条件


* 对于 UWP 应用，请安装 [Microsoft 广告 SDK](http://aka.ms/ads-sdk-uwp) 与 Visual Studio 2015 或更高版本。
* 对于 Windows8.1 或 Windows Phone 8.1 应用：使用 Visual Studio 2015 或 Visual Studio 2013 安装[适用于 Windows 和 Windows Phone 8.x 的 Microsoft Advertising SDK](http://aka.ms/store-8-sdk)。

> [!NOTE]
> 如果你已向 UWP 应用中添加横幅广告并已安装 Windows 10 SDK 版本 10.0.14393（周年更新）或更高版本的 Windows SDK，那么还必须安装 WinJS 库。 此库过去包含在以前版本的 Windows SDK（适用于 Windows 10）中，但从 Windows 10 SDK 版本 10.0.14393（周年更新）开始，此库必须单独安装。 若要安装 WinJS，请参阅[获取 WinJS](http://try.buildwinjs.com/download/GetWinJS/)。

## <a name="code-development"></a>代码开发

1. 在 Visual Studio 中，打开项目或创建新项目。

2. 如果你的项目面向**任何 CPU**，请更新你的项目以使用特定于体系结构的生成输出（例如，**x86**）。 如果你的项目面向**任何 CPU**，你将无法在以下步骤中成功添加对 Microsoft Advertising 库的引用。 有关详细信息，请参阅[项目中由面向任何 CPU 引起的引用错误](known-issues-for-the-advertising-libraries.md#reference_errors)。

3.  在**解决方案资源管理器**窗口中，右键单击**引用**，然后选择**添加引用...**

4.  在**引用管理器**中，根据你的项目类型选择以下引用之一：

    -   对于通用 Windows 平台 (UWP) 项目：展开**通用 Windows**、单击**扩展**，然后选中**适用于 JavaScript 的 Microsoft Advertising SDK**（版本 10.0）旁边的复选框。

    -   对于 Windows 8.1 项目：展开 **Windows 8.1**、单击**扩展**，然后选中**适用于 Windows 8.1 Native (JS) 的 Microsoft Advertising SDK** 旁边的复选框。

    -   对于 Windows Phone 8.1 项目：展开**Windows Phone 8.1**、单击**扩展**，然后选中**适用于 Windows Phone 8.1 Native (JS) 的 Microsoft 广告 SDK** 旁边的复选框。

    ![javascriptaddreference](images/13-f7f6d6a6-161e-4f17-995d-1236d0b5d9f2.png)

5.  在**引用管理器**中，单击“确定”。

6.  打开 index.html 文件（或其他适用于你项目的 html 文件）。

7.  在**&lt;标题&gt;**部分中，在项目的 JavaScript 引用 default.css 和 main.js 之后，添加对 ad.js 的引用。

    在 UWP 项目中，添加以下代码。

    ``` HTML
    <!-- Advertising required references -->
    <script src="//Microsoft.Advertising.JavaScript/ad.js"></script>
    ```

    在 Windows 8.1 或 Windows Phone 8.1 项目中，添加以下代码。

    ``` HTML
    <!-- Advertising required references -->
    <script src="/MSAdvertisingJS/ads/ad.js"></script>
    ```

    > [!NOTE]
    > 在包含了 main.js; 之后，此行必须放置在 **&lt;head&gt;** 部分；否则，当你生成项目时将遇到错误。 如果你的项目面向 Windows 8.1 或 Windows Phone 8.1，则项目中的默认 HTML 文件名为 default.html 而不是 index.html，且项目中的默认 JavaScript 文件名为 default.js 而不是 main.j。

8.  修改 default.html 文件（或其他适用于你项目的 html 文件）的 **&lt;body&gt;** 部分，以便包含 **AdControl** 的 div。 将 **AdControl** 中的 **applicationId** 和 **adUnitId** 属性分配给[测试模式值](test-mode-values.md)中提供的测试值。 另外还要调整控件的高度和宽度，以使其适应[横幅广告支持的广告大小](supported-ad-sizes-for-banner-ads.md)。

    > [!NOTE]
    > 每个 **AdControl** 都有一个对应的*广告单元*，我们的服务使用该广告单元来为控件提供广告，每个广告单元都包含*单元 ID* 和*应用程序 ID*。 在这些步骤中，你将为控件分配测试广告单元 ID 和应用程序 ID 值。 这些测试值只能在应用的测试版本中使用。 在将应用发布到应用商店之前，你必须在 Windows 开发人员中心[将这些测试值替换为实时值](#release)。

    ``` HTML
    <div id="myAd" style="position: absolute; top: 50px; left: 0px; width: 300px; height: 250px; z-index: 1"
        data-win-control="MicrosoftNSJS.Advertising.AdControl"
        data-win-options="{applicationId: '3f83fe91-d6be-434d-a0ae-7351c5a997f1', adUnitId: 'test'}">
    </div>
    ```

9.  编译并运行应用以查看是否带有广告。

以下示例所示为简单 UWP 应用的完整 index.html。

``` HTML
<!DOCTYPE html>
<html>
<head>
    <meta charset="utf-8" />
    <title>AdControlExampleApp</title>
    <!-- WinJS references -->
    <link href="lib/winjs-4.0.1/css/ui-light.css" rel="stylesheet" />
    <script src="lib/winjs-4.0.1/js/base.js"></script>
    <script src="lib/winjs-4.0.1/js/ui.js"></script>
    <!-- AdControlExampleApp references -->
    <link href="css/default.css" rel="stylesheet" />
    <script src="js/main.js"></script>
    <!-- Required reference for AdControl -->
    <script src="//Microsoft.Advertising.JavaScript/ad.js"></script>
</head>
<body>
    <div id="myAd" style="position: absolute; top: 50px; left: 0px; width: 300px; height: 250px; z-index: 1"
      data-win-control="MicrosoftNSJS.Advertising.AdControl"
      data-win-options="{applicationId: '3f83fe91-d6be-434d-a0ae-7351c5a997f1', adUnitId: 'test'}">
    </div>
    <p>Content goes here</p>
</body>
</html>
```

<span id="release" />
## <a name="release-your-app-with-live-ads-using-windows-dev-center"></a>使用 Windows 开发人员中心发布带有实时广告的应用

1.  在开发人员中心仪表板中，转到应用的[通过广告盈利](../publish/monetize-with-ads.md)页面，然后[创建广告单元](../monetize/set-up-ad-units-in-your-app.md)。 对于广告单元类型，请指定**横幅**。 记下广告单元 ID 和应用程序 ID。

2. 如果你的应用是适用于 Windows 10 的 UWP 应用，则可以为 **AdControl** 选择性启用广告中介，方法是配置[通过广告盈利](../publish/monetize-with-ads.md)页面的[广告中介](../publish/monetize-with-ads.md#mediation)部分的设置。 广告中介显示来自多个广告网络（包括其他付费广告网络，如 Taboola 和 Smaato）的广告及 Microsoft 应用促销活动的广告，从而使你能够最大化你的广告收益和应用促销能力。

3.  在你的代码中，将测试广告单元值（**applicationId** 和 **adUnitId**）替换为你在开发人员中心生成的实时值。

4.  使用开发人员中心仪表板 [提交应用](../publish/app-submissions.md) 至应用商店。

5.  在开发人员中心仪表板中查看你的[广告性能报告](../publish/advertising-performance-report.md)。             

<span id="manage" />
## <a name="manage-ad-units-for-multiple-ad-controls-in-your-app"></a>管理你的应用中多个广告控件的广告单元

可在一个应用中使用多个 **AdControl** 对象（例如，应用中的每页可以托管不同的 **AdControl** 对象）。 在此情况下，我们建议你为每个控件分配不同的广告单元。 对每个控件使用不同的广告单元使你可以分别[配置中介设置](../publish/monetize-with-ads.md#mediation)并获取每个控件的离散[报告数据](../publish/advertising-performance-report.md)。 这还使我们的服务能够更好地优化我们为你的应用提供的广告。

> [!IMPORTANT]
> 每个广告单元都只能在一个应用中使用。 如果在多个应用中使用某个广告单元，将不为该广告单元提供广告。

## <a name="related-topics"></a>相关主题

* [GitHub 上的广告示例](http://aka.ms/githubads)
* [为应用设置广告单元](../monetize/set-up-ad-units-in-your-app.md)
